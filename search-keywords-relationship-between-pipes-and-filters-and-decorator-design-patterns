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
```csharp
LinearPipeline<StreamingControlImpl<Item>> pipeline = new LinearPipeline<StreamingControlImpl<Item>>();
pipeline.AddLink(Factory<Item>.CreateLoadFile("data.log"));
pipeline.AddLink(Factory<Item>.CreateStartWithSearch("keyword"));
pipeline.AddLink(Factory<Item>.CreateWordLengthSearch(9));
pipeline.AddLink(Factory<Item>.CreatePalindromeSearch());
pipeline.AddLink(Factory<Item>.CreateOutput());

StreamingControlImpl<Item> controlData = new StreamingControlImpl<Item>();
pipeline.Process(controlData);

