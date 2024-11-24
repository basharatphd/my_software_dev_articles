# Relationship Between Pipes-and-Filters and Decorator Design Patterns

This repository demonstrates the relationship between the **Pipes-and-Filters** architectural pattern and the **Decorator** design pattern through implementations in C#. The goal is to showcase their similarities, differences, and how each can be applied to solve real-world problems.
 
## Introduction

This project explores two design approaches to solve the same problem:
1. **Pipes-and-Filters**, focusing on sequential or pipeline-based data processing.
2. **Decorator**, emphasizing object-oriented dynamic behavior extension.

## Pipes-and-Filters Pattern

The **Pipes-and-Filters** architectural pattern structures a system as a series of independent filters connected by pipes, where:
- **Filters**: Process, transform, or refine data.
- **Pipes**: Transmit data streams between filters.

### Key Components
- **Filters**: Active or passive components that transform data.
- **Pipes**: Connections between filters for data transmission.
- **Data Source**: Provides input to the first filter.
- **Data Sink**: Consumes output from the last filter.

### Advantages
- Enforces sequential data processing.
- Allows filter reuse and reordering.
- Enables parallel processing for higher throughput.
- Facilitates rapid prototyping with flexible configurations.

### Limitations
- Inefficient for non-sequential logic (e.g., branching, looping).
- High overhead for state sharing and data transformation.
- Limited error handling capabilities.

## Decorator Pattern

The **Decorator** design pattern dynamically adds responsibilities to objects without altering their structure. Decorators wrap objects and implement the same interface, making the behavior extension transparent to clients.

### Core Concept
- **Decorators** act as wrappers, forwarding method calls to the wrapped object.
- Additional logic can be added before or after delegating the call.

### Benefits
- Flexible exchange or reordering of decorators.
- Encourages reusability and prototyping of behaviors.
- Simplifies extensions without subclassing.

## Comparison

| Aspect                | Pipes-and-Filters                  | Decorator                    |
|-----------------------|-------------------------------------|------------------------------|
| **Focus**            | Stream processing                  | Object behavior extension    |
| **Data Handling**    | Stream-based                       | Object-based                 |
| **Reusability**      | Filters can be reused in pipelines  | Decorators can be reused     |
| **Flexibility**      | Rearrange filters dynamically       | Chain decorators dynamically |
| **Parallelism**      | Supports parallel processing        | Not inherently supported     |

## Implementations

This repository includes:
1. **Pipes-and-Filters Implementation**: A pipeline with sequential filters processing a data stream.
2. **Decorator Implementation**: A chain of decorators adding responsibilities to objects.

### Example Filters (Pipes-and-Filters)
- **LoadFileFilter**: Reads data from a file.
- **StartWithSearchFilter**: Filters data starting with a keyword.
- **WordLengthSearchFilter**: Filters words below a certain length.
- **PalindromeSearchFilter**: Filters palindromes.
- **Output**: Displays the results.

### Example Decorators
- **StartWithSearch**: Searches for words starting with a keyword.
- **WordLengthSearch**: Filters words below a certain length.
- **PalindromeSearch**: Filters palindromes.

## Usage

### Pipes-and-Filters
# Relationship Between Pipes-and-Filters and Decorator Design Patterns

This repository demonstrates the relationship between the **Pipes-and-Filters** architectural pattern and the **Decorator** design pattern through implementations in C#. The goal is to showcase their similarities, differences, and how each can be applied to solve real-world problems.

## Table of Contents
- [Introduction](#introduction)
- [Pipes-and-Filters Pattern](#pipes-and-filters-pattern)
  - [Key Components](#key-components)
  - [Advantages](#advantages)
  - [Limitations](#limitations)
- [Decorator Pattern](#decorator-pattern)
  - [Core Concept](#core-concept)
  - [Benefits](#benefits)
- [Comparison](#comparison)
- [Implementations](#implementations)
- [Usage](#usage)
- [References](#references)

## Introduction

This project explores two design approaches to solve the same problem:
1. **Pipes-and-Filters**, focusing on sequential or pipeline-based data processing.
2. **Decorator**, emphasizing object-oriented dynamic behavior extension.

## Pipes-and-Filters Pattern

The **Pipes-and-Filters** architectural pattern structures a system as a series of independent filters connected by pipes, where:
- **Filters**: Process, transform, or refine data.
- **Pipes**: Transmit data streams between filters.

### Key Components
- **Filters**: Active or passive components that transform data.
- **Pipes**: Connections between filters for data transmission.
- **Data Source**: Provides input to the first filter.
- **Data Sink**: Consumes output from the last filter.

### Advantages
- Enforces sequential data processing.
- Allows filter reuse and reordering.
- Enables parallel processing for higher throughput.
- Facilitates rapid prototyping with flexible configurations.

### Limitations
- Inefficient for non-sequential logic (e.g., branching, looping).
- High overhead for state sharing and data transformation.
- Limited error handling capabilities.

## Decorator Pattern

The **Decorator** design pattern dynamically adds responsibilities to objects without altering their structure. Decorators wrap objects and implement the same interface, making the behavior extension transparent to clients.

### Core Concept
- **Decorators** act as wrappers, forwarding method calls to the wrapped object.
- Additional logic can be added before or after delegating the call.

### Benefits
- Flexible exchange or reordering of decorators.
- Encourages reusability and prototyping of behaviors.
- Simplifies extensions without subclassing.

## Comparison

| Aspect                | Pipes-and-Filters                  | Decorator                    |
|-----------------------|-------------------------------------|------------------------------|
| **Focus**            | Stream processing                  | Object behavior extension    |
| **Data Handling**    | Stream-based                       | Object-based                 |
| **Reusability**      | Filters can be reused in pipelines  | Decorators can be reused     |
| **Flexibility**      | Rearrange filters dynamically       | Chain decorators dynamically |
| **Parallelism**      | Supports parallel processing        | Not inherently supported     |

## Implementations

This repository includes:
1. **Pipes-and-Filters Implementation**: A pipeline with sequential filters processing a data stream.
2. **Decorator Implementation**: A chain of decorators adding responsibilities to objects.

### Example Filters (Pipes-and-Filters)
- **LoadFileFilter**: Reads data from a file.
- **StartWithSearchFilter**: Filters data starting with a keyword.
- **WordLengthSearchFilter**: Filters words below a certain length.
- **PalindromeSearchFilter**: Filters palindromes.
- **Output**: Displays the results.

### Example Decorators
- **StartWithSearch**: Searches for words starting with a keyword.
- **WordLengthSearch**: Filters words below a certain length.
- **PalindromeSearch**: Filters palindromes.

## Usage

### Pipes-and-Filters
We may consider the generic filter as a component that processes data. The IComponent basic interface could be defined as follows:

C#
public interface IComponent<T>
{
    bool Process(T t);
}
Throughout the sample, I used �T� as a generic type parameter. InputStream and OutputStream are interfaces used for the input and the output streams.

```csharp   
public interface InputStream<T> : IEnumerable<T>
{
    bool Available();
    T Read();
    ulong Read(out T[] Data, ulong offset, ulong length);
    void Reset();
    void Skip(ulong offset);
}

public interface OutputStream<T>
{
    void Flush();
    void Write(T data);
    void Write(T[] Data);
    void Write(T[] Data, ulong offset, ulong length);
}

public interface IStreamingControl<T>
{
    InputStream<T> Input
    {
        get; set;
    }
    OutputStream<T> Output
    {
        get; set;
    }

    InputStream<T> FactoryInputStream();
    OutputStream<T> FactoryOutputStream();
}
```
Now, define the concrete implementation class StreamingControlImpl which implements the IStreamingControl as:

```csharp
public class StreamingControlImpl<T> : 
       IStreamingControl<T> where T : new()

public interface IComponentStreaming<T> : 
       IComponent<StreamingControlImpl<T>> where T : new()
{
    bool Process(InputStream<T> input, OutputStream<T> output);
}

public abstract class InputSource<T> : 
       StreamingComponentBase<T> where T : new()
{
    public const bool isOutputSink = false;

    public abstract bool Process(OutputStream<T> output);

    public override bool Process(InputStream<T> input, 
                                 OutputStream<T> output)
    {
        foreach (T t in input)
            output.Write(t);

        return Process(output);
    }
}

public abstract class OutputSink<T> : 
       StreamingComponentBase<T> where T : new()
{
    public const bool isOutputSink = true;

    public abstract bool Process(InputStream<T> input);

    public override bool Process(InputStream<T> input, 
                                 OutputStream<T> output)
    {
        return Process(input);
    }
}
```
We can have two processing types: sequential processing and pipeline processing. Connecting components with asynchronous pipes allows each unit in the chain to operate in its own thread or its own process. When a unit has completed processing one filter, it can send the data to the output stream and immediately start processing another data. It does not have to wait for the subsequent components to read and process the data. This allows multiple data messages to be processed concurrently as they pass through the individual stages. For example, after the first message has been decrypted, it can be passed on to the authentication component. At the same time, the next message can already be decrypted. We call such a configuration a processing pipeline because messages flow through the filters like liquid flows through a pipe. When compared to strictly sequential processing, a processing pipeline can significantly increase system throughput. Here, I only discuss sequential processing.

The pipeline interface is defined as:

```csharp
public interface IPipeline<T> 
{
    void AddLink(IComponent<T> link);
    bool Process(T t);
}
```
LinearPipeline is the IPipeline implementation that can process filters sequentially. There are two ways to process the filters in the order defined by the following enumerator:

```csharp
public enum OPTYPE { AND, OR };

If processing of any filter in the pipeline fails and/or returns false, the processing stops without executing the preceding filters, and the pipeline returns false to the caller program. So the pipeline processing with the AND operator is as follows:

```csharp
if (_optype == OPTYPE.AND)
{
    result = true;
    foreach (IComponent<T> link in _links)
    {
        if (! link.Process(t))
        {
            result = false;
            break;
        }
    }
}
```
If the processing of any filter in the pipeline returns true, the pipeline returns true to the caller program. The pipeline continues to process each filter regardless of the result obtained in processing the filters. The pipeline processing with the OR operator is as follows.

```csharp
if (_optype == OPTYPE.OR)
{
    result = false;
    foreach (IComponent<T> link in _links)
    {
        if (link.Process(t))
        {
            result = true;
            //break;

        }
    }
}
```
Different filters do different jobs, hence are implemented as different classes. Here is the list of five filters in the pipeline:

LoadFileFilter reads the program text (i.e., source code) from a file (or perhaps a sequence of files) as a stream of characters, and recognizes a sequence of tokens. It is not very efficient to read a character at a time, since file IO operations are slow. Therefore, we can read a file at a time using a StreamReader. The class Tokens is an iterator tokenizer that uses either commas or semicolons and spaces to divide into tokens.
StartWithSearchFilter gets data stream from LoadFileFilter and sinks the words which start with the keyword provided as the parameter.
WordLengthSearchFilter gets data stream from StartWithSearchFilter and sinks the words having length less than or equal to the maximum value provided as the parameter.
PlaindromeSearchFilter gets a data stream from WordLengthSearchFilter and sinks the words that are palindrome in nature.
Output is used to display the results to the user.
The following class describes the implementation of the LoadFileFilter task:

```csharp
internal class LoadFileFliter : InputSource<Item>
{
    private string _path;

    public LoadFileFliter(string path)
    {
        _path = path;
    }
    public override bool Process(OutputStream<Item> output)
    {
        try
        {
            if (File.Exists(_path))
            {
                string buffer;
                // Create an instance of StreamReader to read from a file.

                // The using statement also closes the StreamReader.

                using (StreamReader sr = new StreamReader(_path))
                {
                    buffer = sr.ReadToEnd();
                }
                
               buffer = buffer.Replace("\r\n", " ");
               Tokens Tokenizer = new Tokens(buffer, new char[] { ' ', ';', ',' });
                foreach (string Token in Tokenizer)
                {
                    if (Token == string.Empty) continue;

                    System.Diagnostics.Debug.Write(Token);
                    output.Write(new Item(Token));
                }
                return true;
            }
            else
                throw new Exception("File could not be read");
        }
        catch (Exception ex)
        {
            System.Diagnostics.Debug.Write(ex.Message);
        }
   
        return false;
        
    }
}
```
As every filter exposes a very simple interface, it receives messages on the inbound pipe, processes the message, and publishes the results to the outbound pipe. The pipe connects one filter to the next, sending output messages from one filter to the next. Because, all components use the same external interface.

They can be composed into different solutions by connecting the components to different pipes. We can add new filters, omit existing ones, or rearrange them into a new sequence�all without having to change the filters themselves. Here is the major Invoke class that prepares an order of filters into a pipeline and processes it:

```csharp
public static void Invoke()
{
    LinearPipeline<StreamingControlImpl<Item>> pipeline = 
      new LinearPipeline<StreamingControlImpl<Item>>(/*OPTYPE.OR*/);

    pipeline.AddLink(Factory<Item>.CreateLoadFile(@"c:\a.log"));
    pipeline.AddLink(Factory<Item>.CreateStartWithSearch("wa"));
    pipeline.AddLink(Factory<Item>.CreateWordLengthSearch(9));
    pipeline.AddLink(Factory<Item>.CreatePalindromeSearch());
    pipeline.AddLink(Factory<Item>.CreateOutput());

    StreamingControlImpl<Item> controldata = 
              new StreamingControlImpl<Item>();
    bool done = pipeline.Process(controldata);
    if (done)
        System.Diagnostics.Debug.Write("All filters " + 
                             "processed successfully");

}
```
While reading a file and tokenizing it to prepare stream data, I want to use the �string� native data type to be passed as the type parameter but using StreamingControlImpl<string> generates a compiler error as the constraint where T : new() on every level of upward hierarchy requires a parameter-less constructor for type �string�. Since string does not have a parameter-less constructor and it is a sealed class, we cannot inherit a class from string. To solve this problem instead of having a is-a relationship, we can implement a new class Item having a has-a relationship and can get the value using the Item.Text property! No big deal:

```csharp
internal class Item
{
    private string _text = null;
    public string Text
    {
        get { return _text; }
        set { _text = value; }
    }
    public Item()
    {
        _text = string.Empty;
    }
    public Item(string t)
    {
        _text = t;
    }
}
```
 ## Liabilities of Pipes-and-Filters

While the Pipes-and-Filters architectural pattern provides flexibility and modularity, it also has several limitations, as highlighted by Buschmann:

1. **State Sharing**: 
   - Sharing state information across filters is expensive and inflexible. Data must be encoded, transmitted, and decoded between components.
   
2. **Parallel Processing Overhead**: 
   - While parallel processing can improve throughput, the efficiency gain may often be an illusion due to the high costs of data transfer, synchronization, and context switching.
   - Non-incremental filters (e.g., Unix `sort`) can become bottlenecks.

3. **Data Transformation Overhead**: 
   - The use of a single data channel between filters often necessitates significant data transformation, such as translating numbers between binary and character formats.

4. **Error Handling Challenges**: 
   - Detecting and recovering from errors in Pipes-and-Filters systems is often difficult and can lead to fragile designs.

---

## Decorator Pattern - An Alternative or Related Pattern

### Intent

The **Decorator** design pattern dynamically attaches additional responsibilities to objects without using subclassing. Decorators wrap the object they decorate and look identical to the client, as they implement the same interface.

- A **Decorator** intercepts messages sent to the object, applies some processing, and optionally forwards the message to the wrapped object.
- The wrapped object may return a value, which the decorator can further modify before passing it back to the client.
- This approach is **transparent** to the client, making the decorator a seamless extension mechanism.

### Implementation Details

To address the liabilities of the Pipes-and-Filters pattern, the **Decorator** pattern can be employed as an alternative. For instance, by re-arranging the Pipes-and-Filters code, we can replace filters with decorators:

- **Abstract SearchDecorator**: 
  - Implements the `IComponent` interface and takes an existing component (e.g., `ISearch`) as a parameter in its constructor.
  - Allows wrapping additional behavior dynamically.

### Example: Using Decorators

Here’s an example of how a decorator can dynamically layer behaviors:

```csharp
public abstract class SearchDecorator<T> : IComponent<T> where T : new()
{
    protected IComponent<T> _ancestor;

    public SearchDecorator(IComponent<T> ancestor)
    {
        _ancestor = ancestor;
    }

    public virtual bool Process()
    {
        return _ancestor.Process();
    }

    public virtual IEnumerable<T> Words
    {
        get { return _ancestor.Words; }
    }
}
```
The calling program uses the decoration of search in the following ways:

```csharp
IComponent<Item> S = new FileLoader(".log");
S = new StartWithSearch(S, "ma");
S = new WordLengthSearch(S, 5);
S = new PalindromeSearch(S);
S = new Output(S);
S.Process();
```
### Comparisons
### Benefits of Pipes-and-Filters
The pipes-and-filters architectural pattern has the following benefits [Buschmann]:

Flexibility by filter exchange. It is easy to exchange one filter element for another with the same interfaces and functionality.
Flexibility by recombination. It is not difficult to reconfigure a pipeline to include new filters or perhaps to use the same filters in a different sequence. As we can see here:
```csharp
pipeline.AddLink(Factory<Item>.CreateLoadFile(@"c:\a.log"));
pipeline.AddLink(Factory<Item>.CreateStartWithSearch("wa"));
pipeline.AddLink(Factory<Item>.CreateWordLengthSearch(9));
pipeline.AddLink(Factory<Item>.CreatePalindromeSearch());
pipeline.AddLink(Factory<Item>.CreateOutput());
pipeline.Process();
```
Reuse of filter elements. The ease of filter recombination encourages filter reuse. Small, active filter elements are normally easy to reuse if the environment makes them easy to connect.
Rapid prototyping of pipelines. Flexibility of exchange and recombination, and ease of reuse enables the rapid creation of prototype systems.
Efficiency by parallel processing. Since active filters run in separate processes or threads, pipes-and-filters systems can take advantage of a multiprocessor. By implementing a pipeline with AND and OR operators, it may also process SPLIT and JOIN operators.
A filter in a pipeline may be a sub-pipeline. The pipeline structure may consist of two pipelines: the main pipeline and the sub-pipeline. For example, the main pipeline contains step 1 to step 5, and the sub-pipeline contains step 3-1, step 3-2, and step 3-3. Particularly, step 3 in the main pipeline has two roles: it is a normal step in the main pipeline, and is also a sub-pipeline which contains sub-steps as:


### Benefits of Decorator
The Decorator pattern has the following benefits:

Flexibility by concrete decorator exchange. It is easy to exchange a concrete decorator for another.
Flexibility by recombination. It is not big deal to reconfigure a calling client to assign a new set of responsibilities or perhaps to use the same responsibilities in a different sequence. As we can see here:
```csharp
IComponent<Item> S = new FileLoader(".log");
S = new StartWithSearch(S, "wa");
S = new WordLengthSearch(S, 5);
S = new PalindromeSearch(S);
S = new Output(S);
S.Process();
```
Reuse of concrete decorator. The ease of responsibility recombination encourages concrete decorator reuse.
Rapid prototyping of pipelines. Flexibility of exchange and recombination and ease of reuse enables the rapid creation of prototype systems.
Efficiency by parallel processing. I am not clear here whether different responsibilities can be practically processed into separate processes by using multithreading. I think parallel processing is an extra advantage of Pipes-and-Filters.
Responsibilities Implementation is linear. Although the responsibilities implementation is linear and simple, taking into account the nested or sub-pipeline like functionality will cause complications and difficulties in keeping references, and extensibility will also become a headache.

### Conclusions
I implemented the same problem using both Pipes-and-Filters architectural pattern and Decorator pattern. The basic idea that encourages me is the dynamic arrangement of filters on one side and dynamic pluggablility of responsibilities (wrappers) on the other side. If I don't talk about parallel pipeline processing in pipes-and-filters, you can see how pipes-and-filters with sequential processing maps to the decorator pattern exactly. So they may be related patterns in a sense that in Pipes-and-Filters, pipes are stateless and serve as conduits for moving streams of data across multiple filters, while filters are stream modifiers which process the input streams and send the resulting output stream to the pipe of another filter, and in the Decorator pattern we can see decorators as filters.

