# Java Collections Framework Guide

## List Interface
### ArrayList Operations

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

6. Converting to Array
```java
String[] array = list.toArray(new String[0]);
Object[] objArray = list.toArray();
```

## LinkedList Operations
### Initialization
```java
LinkedList<String> linkedList = new LinkedList<>();
LinkedList<String> linkedList2 = new LinkedList<>(Arrays.asList("A", "B", "C"));
```

### Specific LinkedList Operations
```java
linkedList.addFirst("First");     // Add at beginning
linkedList.addLast("Last");       // Add at end
linkedList.getFirst();            // Get first element
linkedList.getLast();             // Get last element
linkedList.removeFirst();         // Remove first element
linkedList.removeLast();          // Remove last element
linkedList.peek();                // Retrieve but don't remove first element
linkedList.peekFirst();           // Same as peek()
linkedList.peekLast();           // Retrieve but don't remove last element
linkedList.poll();               // Retrieve and remove first element
linkedList.pollFirst();          // Same as poll()
linkedList.pollLast();           // Retrieve and remove last element
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
