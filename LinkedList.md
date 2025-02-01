# Java Collections Framework Guide

## LinkedList Operations

### Initialization

```java
LinkedList<String> linkedList = new LinkedList<>();
LinkedList<String> linkedList2 = new LinkedList<>(Arrays.asList("A", "B", "C"));
```

### Basic Operations

1. Adding Elements

```java
linkedList.add("Element");                    // Adds at end
linkedList.add(2, "Element");                 // Adds at specified index
linkedList.addFirst("First");                 // Adds at beginning
linkedList.addLast("Last");                   // Adds at end
linkedList.push("Element");                   // Adds at front (same as addFirst)
linkedList.offer("Element");                  // Adds at end (returns boolean)
linkedList.offerFirst("Element");             // Adds at front (returns boolean)
linkedList.offerLast("Element");              // Adds at end (returns boolean)
linkedList.addAll(collection);                // Adds all elements at end
linkedList.addAll(2, collection);             // Adds all elements from index
```

2. Accessing Elements

```java
String element = linkedList.get(2);           // Get element at index
String first = linkedList.getFirst();         // Get first element
String last = linkedList.getLast();           // Get last element
String peek = linkedList.peek();              // Retrieve but don't remove first element
String peekFirst = linkedList.peekFirst();    // Same as peek()
String peekLast = linkedList.peekLast();      // Retrieve but don't remove last element
int index = linkedList.indexOf("Element");    // First occurrence index
int lastIndex = linkedList.lastIndexOf("Element"); // Last occurrence index
```

3. Removing Elements

```java
linkedList.remove();                          // Removes first element
linkedList.remove(2);                         // Removes element at index
linkedList.remove("Element");                 // Removes first occurrence
linkedList.removeFirst();                     // Removes first element
linkedList.removeLast();                      // Removes last element
linkedList.removeFirstOccurrence("Element");  // Removes first occurrence
linkedList.removeLastOccurrence("Element");   // Removes last occurrence
String poll = linkedList.poll();              // Retrieves and removes first element
String pollFirst = linkedList.pollFirst();    // Same as poll()
String pollLast = linkedList.pollLast();      // Retrieves and removes last element
linkedList.clear();                           // Removes all elements
```

4. Modifying Elements

```java
linkedList.set(1, "New Element");             // Replace element at index
```

5. Information Methods

```java
int size = linkedList.size();                 // Get size
boolean isEmpty = linkedList.isEmpty();        // Check if empty
boolean contains = linkedList.contains("Element"); // Check if element exists
```

6. Converting to Other Data Structures

```java
// To Array
Object[] array = linkedList.toArray();
String[] strArray = linkedList.toArray(new String[0]);

// To List
List<String> list = new ArrayList<>(linkedList);
```

7. Iteration

```java
// Using Iterator
Iterator<String> iterator = linkedList.iterator();
while (iterator.hasNext()) {
    String element = iterator.next();
}

// Using Descending Iterator
Iterator<String> descendingIterator = linkedList.descendingIterator();
while (descendingIterator.hasNext()) {
    String element = descendingIterator.next();
}

// Using Enhanced For Loop
for (String element : linkedList) {
    // Process element
}
```

### Example Usage

```java
LinkedList<String> linkedList = new LinkedList<>();

// Adding elements
linkedList.add("First");
linkedList.addLast("Last");
linkedList.add(1, "Middle");

// Processing elements
System.out.println("First element: " + linkedList.getFirst());
System.out.println("Last element: " + linkedList.getLast());

// Removing elements
linkedList.removeFirst();
linkedList.removeLast();

// Check size
System.out.println("Size: " + linkedList.size());
```
