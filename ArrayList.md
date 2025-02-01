## ArrayList Operations Guide

#### Initialization

```java
// Standard initialization
List<String> list = new ArrayList<>();
List<Integer> numbers = new ArrayList<Integer>();

// Initialize with elements
List<String> colors = Arrays.asList("Red", "Green", "Blue");
List<String> fruits = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange"));

// Initialize list of lists
List<List<Integer>> matrix = new ArrayList<>();
matrix.add(new ArrayList<>(Arrays.asList(1, 2, 3)));
matrix.add(new ArrayList<>(Arrays.asList(4, 5, 6)));
```

#### Basic Operations

1. Adding Elements

```java
list.add("Element");              // Adds at end
list.add(0, "First Element");     // Adds at specific index
list.addAll(otherList);           // Adds all elements from another collection
```

2. Accessing Elements

```java
String element = list.get(0);     // Get element at index
int size = list.size();           // Get size of list
boolean isEmpty = list.isEmpty(); // Check if list is empty
```

3. Modifying Elements

```java
list.set(1, "New Element");       // Replace element at index
list.clear();                     // Remove all elements
list.remove(0);                   // Remove element at index
list.remove("Element");           // Remove specific element
```

4. Searching and Subsets

```java
boolean exists = list.contains("Element");    // Check if element exists
int index = list.indexOf("Element");          // Find first occurrence
List<String> subList = list.subList(0, 3);   // Get portion of list
```

5. Sorting and Manipulation

```java
Collections.sort(list);                       // Natural order sort
Collections.sort(list, Collections.reverseOrder()); // Reverse sort
Collections.reverse(list);                    // Reverse list order
```

6. Copying and shuffle

```java
List<String> list2 = new ArrayList<>(list);     // Create copy of equal size as list1
Collections.copy(list2,list1)                  // Copy elements from list1 to list2
Collections.shuffle(list);                    // Shuffle elements
```

7. TrimtoSize and ensureCapacity

```java
// Note: These methods are specifically for ArrayList and not available in the general List interface.

// Reduces internal array capacity to match current size
// Useful for memory optimization
// Should be used when you know no more elements will be added

ArrayList<String> list = new ArrayList<>();

// Add elements
list.add("A");
list.add("B");
list.add("C");
// At this point, the ArrayList has capacity 10 but size 3
System.out.println("Size before trimToSize: " + list.size()); // Prints: 3

// trimToSize() reduces the capacity to match current size
list.trimToSize();
// Now capacity is reduced to 3 to match the size


// Increases capacity to at least the specified value
// Useful when you know you'll add many elements
// Helps avoid multiple internal resizing operations
// Can improve performance when adding large number of elements

 // Initialize a new ArrayList
   ArrayList<Integer> numbers = new ArrayList<>();

// ensureCapacity guarantees that the ArrayList can hold
// at least the specified number of elements without resizing
numbers.ensureCapacity(20);

// Now you can add up to 20 elements without any internal resizing
 for(int i = 0; i < 15; i++) {
  numbers.add(i);}

// The ArrayList won't need to resize during these additions
// because we ensured a capacity of 2
```

8. Converting to Array

```java
String[] array = list.toArray(new String[0]);
Object[] objArray = list.toArray();
```

## Stack Operations

### Initialization

```java
Stack<String> stack = new Stack<>();
```

### Stack Methods

```java
stack.push("Element");           // Add element to top
String top = stack.pop();        // Remove and return top element
String peek = stack.peek();      // View top element without removing
boolean empty = stack.isEmpty(); // Check if stack is empty
int size = stack.size();         // Get number of elements
int position = stack.search("Element"); // Find position from top (1-based)
```

### Common Stack Usage Example

```java
Stack<String> stack = new Stack<>();
stack.push("First");
stack.push("Second");
stack.push("Third");

while (!stack.isEmpty()) {
    System.out.println(stack.pop()); // Prints: Third, Second, First
}
```

## Iterator Usage with Collections

```java
List<String> list = new ArrayList<>();
// ... add elements ...

// Using Iterator
Iterator<String> iterator = list.iterator();
while (iterator.hasNext()) {
    String element = iterator.next();
    // Process element
}

// Using Enhanced For Loop
for (String element : list) {
    // Process element
}
```

## Common Collection Utilities

```java
Collections.shuffle(list);        // Randomly shuffle elements
Collections.max(list);           // Find maximum element
Collections.min(list);           // Find minimum element
Collections.frequency(list, "Element"); // Count occurrences
Collections.swap(list, 0, 1);    // Swap elements at positions
Collections.fill(list, "Value"); // Fill list with value
Collections.copy(dest, src);     // Copy one list to another
```
