# Cell Phone - An Implementation of Behavioral Pattern to Describe States

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

### Sample Implementation in C#
Here’s an example of the `IStat`:
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

