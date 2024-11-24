# Cell Phone - An Implementation of Behavioral Pattern to Describe States

### Note: 

This implementation is inspired by my article on CodeProject. For more information, visit the [original article](https://www.codeproject.com/articles/10128/Cell-Phone-An-implementation-of-behavioral-pattern).

This project demonstrates the implementation of the **State Pattern** using a cell phone's state-based behavior as an example. The application models various states of a cell phone (e.g., Idle, Menu, Ringing, Dialing, Talking) and their transitions, based on events.

---

## Overview

A **Finite State Machine (FSM)** is a key design model for dynamic behavior. This implementation leverages the Object-Oriented approach for FSM, inspired by the **State Pattern** from the "Gang of Four" design patterns. 

Key characteristics:
1. A finite number of distinct states.
2. Defined methods for transitions between states.

This project encapsulates each state in a separate class, ensuring modular and state-specific behavior.

---

## States and Transitions

The cell phone can exist in one of the following states:
1. **Idle**: Default state. Cancelling other states transitions back to Idle.
2. **Menu**: Accessed via the menu command, displaying options or the last call list.
3. **Dialing**: Initiated by the call command, transitions to "Talking" after connecting.
4. **Ringing**: Triggered when an incoming call is received.
5. **Talking**: Activated after answering a call.

**Special Cases**:
- Repeated commands in the current state show specific messages (e.g., "Already in Idle state").
- Certain operations may be ignored in specific states (e.g., "Operation Ignored: Currently in Talking State").
## Special Cases and Responses

The system provides specific responses to invalid or redundant commands in various states:

1. **Idle State**:
   - **Command**: `Cancel`
   - **Response**: `Already in Idle state`

2. **Menu State**:
   - **Command**: `Menu`
   - **Response**: `Already in Menu state`

3. **Dialing/Talking State**:
   - **Command**: `Call`
   - **Response**: `Already in Dialing/Talking state`

4. **Talking State**:
   - **Command**: `Menu`
   - **Response**: `Operation Ignored: Currently in Talking State`

5. **Ringing State**:
   - **Command**: `Menu`
   - **Response**: `Operation Ignored: Currently in Ringing State`

These predefined responses ensure clarity in system behavior and prevent invalid operations within a given state.

---

## Features

- Encapsulation of state-specific behavior.
- Centralized transition logic in the `CellPhone` class.
- Efficient reuse of state instances via a state object pool.

---

## Design

### Class Diagram
The design includes:
- **`IState` Interface**: Defines methods (`menuButton`, `callButton`, `exitButton`) for state transitions.
- **`CellPhone` Class**: Manages current and previous states, performs state transitions, and provides shared behaviors.
- **Concrete State Classes**: Implements state-specific behaviors for Idle, Menu, Ringing, Dialing, and Talking states.

## Sample Implementation in C#
### `IState` interface
The IState interface is how you implement the actual "intelligence". The idea is that an entity can only be in one state at any given time, so you can use this interface to dictate what happens when that state is entered, when to leave that state, and where to go.
Here’s an example of the `IState`:
```csharp
class Idle : IState {
    public void menuButton(CellPhone cellPhone) {
        cellPhone.setCurrentState(eState.MENU);
        cellPhone.showMenu();
    }

    public void callButton(CellPhone cellPhone) {
        cellPhone.setCurrentState(eState.MENU);
        cellPhone.showLastCall();
    }

    public void exitButton(CellPhone cellPhone) { 
        cellPhone.repeat();
    }
}
```
### `CellPhone` Class

The `CellPhone` class is responsible for managing the current and previous states of the system. It facilitates state transitions and provides wrappers for actions specific to each state. This class leverages the `IState` interface to ensure modular and state-specific behavior.

Key responsibilities of the `CellPhone` class:
- Maintain references to the current and previous states.
- Handle state transitions through a defined state map.
- Delegate state-specific actions to the appropriate state objects.

```csharp
public class CellPhone
{
    private Form1 m_form; 
    private IState m_previousState;
    private IState m_currentState;
    private Hashtable m_states = new Hashtable();

    public CellPhone(Form1 f)
    {
        m_states.Add(eState.IDLE, new Idle());
        m_states.Add(eState.MENU, new Menu());
        m_states.Add(eState.RINGING, new Ringing());
        m_states.Add(eState.DIALING, new Dialing());
        m_states.Add(eState.TALKING, new Talking());
    
        // Default state is Idle
        m_currentState = (IState)m_states[eState.IDLE]; 
        m_previousState = null; 
        m_form = f;
        disconnect();
    }

    public eState getCurrentState()
    {
        foreach(object key in m_states.Keys)
        {
            if((IState)m_states[key] == m_currentState)
                return (eState)key;
        }    
        return eState.UNKNOWN;
    }

    public void setCurrentState(eState s)
    {
        m_previousState = m_currentState;
        m_currentState = (IState)m_states[s];    
    }

    // Wrappers for state-specific actions
    public void menuButton() => m_currentState.menuButton(this);
    public void callButton() => m_currentState.callButton(this);
    public void exitButton() => m_currentState.exitButton(this);
    public void ringButton()
    {
        setCurrentState(eState.RINGING);
        ringing();
    }

    // Additional actions
    public void call() => Show("Calling...");
    public void showLastCall() => Show("LastCall List...");
    public void showMenu() => Show("Menu List...");
    public void talking() => Show("Call Connected...");
    public void ringing() => Show("Call is coming...");
    public void answer() => Show("Answering...");
    public void disconnect() => Show("Disconnected/Idle...");

    public void Show(string s)
    {
        ListViewItem item = new ListViewItem();
        item.Text = s;
        item.SubItems.Add(m_previousState?.ToString().Replace("StatePattern.", "") ?? "Unknown");
        item.SubItems.Add(m_currentState.ToString().Replace("StatePattern.", ""));
        m_form.MainListView.Items.Add(item);

        m_form.LState.Text = m_currentState.ToString().Replace("StatePattern.", "");
        m_form.LOperation.Text = s;
    }

    public void ignore()
    {
        string str = $"Ignored: Currently in {m_currentState}".Replace("StatePattern.", "");
        m_form.MainListView.Items.Add(str);
    }

    public void repeat()
    {
        string str = $"Already in {m_currentState}".Replace("StatePattern.", "");
        m_form.MainListView.Items.Add(str);
    }
}
```
### Concrete Classes

The Concrete classes implement the `IState` interface to define the behavior for each specific state of the cell phone. These classes encapsulate the rules for transitioning between states and managing actions within their respective states.

Each state class overrides the methods in the `IState` interface (`menuButton`, `callButton`, and `exitButton`) to dictate behavior and transitions.

---

#### `Idle` Class

The `Idle` class represents the idle state of the cell phone. Actions such as invoking the menu or call commands result in transitions to the appropriate states.

```csharp
class Idle : IState
{
    public void menuButton(CellPhone cellPhone)
    {
        cellPhone.setCurrentState(eState.MENU);
        cellPhone.showMenu();
    }

    public void callButton(CellPhone cellPhone)
    {
        cellPhone.setCurrentState(eState.MENU);
        cellPhone.showLastCall();
    }

    public void exitButton(CellPhone cellPhone)
    {
        cellPhone.repeat();
    }
}
```
#### `Menu` Class
The Menu class handles actions in the menu state. It transitions to the dialing state when a call is initiated and back to the idle state when exited.

```csharp
class Menu : IState
{
    public void menuButton(CellPhone cellPhone)
    {
        cellPhone.repeat();
    }

    public void callButton(CellPhone cellPhone)
    {
        Alpha a = new Alpha(cellPhone);
        Thread t = new Thread(new ThreadStart(a.threadProc));
        t.Start();
    }

    public void exitButton(CellPhone cellPhone)
    {
        cellPhone.setCurrentState(eState.IDLE);
        cellPhone.disconnect();
    }
}
```
#### `Ringing` Class
The Ringing class represents the state when a call is incoming. It transitions to the talking state upon answering or back to idle when the call is declined.

```csharp
class Ringing : IState
{
    public void menuButton(CellPhone cellPhone)
    {
        cellPhone.ignore();
    }

    public void callButton(CellPhone cellPhone)
    {
        cellPhone.setCurrentState(eState.TALKING);
        cellPhone.answer();
    }

    public void exitButton(CellPhone cellPhone)
    {
        cellPhone.setCurrentState(eState.IDLE);
        cellPhone.disconnect();
    }
}
```
#### `Dialing` Class
The Dialing class represents the state of an outgoing call. It ensures invalid actions are ignored or appropriately handled, and transitions to idle when canceled.

```csharp
class Dialing : IState
{
    public void menuButton(CellPhone cellPhone)
    {
        cellPhone.ignore();
    }

    public void callButton(CellPhone cellPhone)
    {
        cellPhone.repeat();
    }

    public void exitButton(CellPhone cellPhone)
    {
        cellPhone.setCurrentState(eState.IDLE);
        cellPhone.disconnect();
    }
}
```
#### `Talking` Class
The Talking class handles the state of an active call. It ignores unrelated actions and transitions back to idle when the call ends.

```csharp
class Talking : IState
{
    public void menuButton(CellPhone cellPhone)
    {
        cellPhone.ignore();
    }

    public void callButton(CellPhone cellPhone)
    {
        cellPhone.repeat();
    }

    public void exitButton(CellPhone cellPhone)
    {
        cellPhone.setCurrentState(eState.IDLE);
        cellPhone.disconnect();
    }
}
```
These concrete classes provide a modular and extensible implementation of the `IState` interface, ensuring clear state-specific behavior and robust state management.


