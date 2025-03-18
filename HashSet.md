# HashSet Operations Guide

### Initialization

## 1. Initialization and Basic Operations

```java
import java.util.*;

public class HashSetDemo {
    public static void main(String[] args) {
        // Different ways to initialize HashSet

        // Empty HashSet
        HashSet<String> set1 = new HashSet<>();

        // HashSet with initial capacity
        HashSet<Integer> set2 = new HashSet<>(20);

        // HashSet with initial capacity and load factor
        HashSet<String> set3 = new HashSet<>(20, 0.75f);

        // Initialize from another collection
        ArrayList<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
        HashSet<String> set4 = new HashSet<>(list);
    }
}
```

## 2. Adding Elements

```java
HashSet<String> fruits = new HashSet<>();

// Adding single elements
fruits.add("Apple");                  // Returns true
boolean isAdded = fruits.add("Apple"); // Returns false (duplicate)

// Adding multiple elements
fruits.addAll(Arrays.asList("Banana", "Cherry", "Date"));

System.out.println(fruits); // Prints: [Apple, Banana, Cherry, Date]
```

## 3. Removing Elements

```java
HashSet<Integer> numbers = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5));

// Remove single element
numbers.remove(1);                    // Returns true
boolean isRemoved = numbers.remove(10); // Returns false (not present)

// Remove multiple elements
numbers.removeAll(Arrays.asList(2, 3));

// Remove elements based on condition
numbers.removeIf(n -> n % 2 == 0);    // Removes even numbers

// Clear all elements
numbers.clear();
```

## 4. Checking Set Contents

```java
HashSet<String> colors = new HashSet<>(Arrays.asList("Red", "Green", "Blue"));

// Check if element exists
boolean hasRed = colors.contains("Red");          // Returns true
boolean hasYellow = colors.contains("Yellow");    // Returns false

// Check if set is empty
boolean isEmpty = colors.isEmpty();               // Returns false

// Get set size
int size = colors.size();                        // Returns 3

// Check if set contains all elements from collection
List<String> checkList = Arrays.asList("Red", "Green");
boolean hasAll = colors.containsAll(checkList);   // Returns true
```

## 5. Set Operations (Union, Intersection, Difference)

```java
HashSet<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5));
HashSet<Integer> set2 = new HashSet<>(Arrays.asList(4, 5, 6, 7, 8));

// Union (combine sets)
HashSet<Integer> union = new HashSet<>(set1);
union.addAll(set2);
System.out.println("Union: " + union); // [1, 2, 3, 4, 5, 6, 7, 8]

// Intersection (common elements)
HashSet<Integer> intersection = new HashSet<>(set1);
intersection.retainAll(set2);
System.out.println("Intersection: " + intersection); // [4, 5]

// Difference (elements in set1 but not in set2)
HashSet<Integer> difference = new HashSet<>(set1);
difference.removeAll(set2);
System.out.println("Difference: " + difference); // [1, 2, 3]
```

## 6. Converting HashSet to Other Collections

```java
HashSet<String> set = new HashSet<>(Arrays.asList("A", "B", "C"));

// Convert to Array
String[] array = set.toArray(new String[0]);

// Convert to ArrayList
ArrayList<String> list = new ArrayList<>(set);

// Convert to TreeSet (sorted)
TreeSet<String> treeSet = new TreeSet<>(set);
```

## 7. Iterating Over HashSet

```java
HashSet<String> languages = new HashSet<>(Arrays.asList("Java", "Python", "JavaScript"));

// Using enhanced for loop
for (String lang : languages) {
    System.out.println(lang);
}

// Using iterator
Iterator<String> iterator = languages.iterator();

//The key thing to understand is that hasNext() checks if there's another element available before you call next(). 
//It doesn't mean "has next after the current one" - it means "has a next element to retrieve."

while (iterator.hasNext()) {
    String lang = iterator.next();
    System.out.println(lang);
}

// Using forEach method
languages.forEach(lang -> System.out.println(lang));
```

## 8. Practical Example: Removing Duplicates

```java
public class DuplicateRemover {
    public static void main(String[] args) {
        // Original list with duplicates
        ArrayList<Integer> numbersWithDuplicates = new ArrayList<>(
            Arrays.asList(1, 2, 3, 2, 4, 1, 5, 3, 4, 7)
        );

        // Remove duplicates using HashSet
        HashSet<Integer> uniqueNumbers = new HashSet<>(numbersWithDuplicates);

        // Convert back to ArrayList if needed
        ArrayList<Integer> numbersWithoutDuplicates = new ArrayList<>(uniqueNumbers);

        System.out.println("Original: " + numbersWithDuplicates);
        System.out.println("Without duplicates: " + numbersWithoutDuplicates);
    }
}
```

## 9. Custom Objects in HashSet

```java
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}

public class CustomObjectHashSet {
    public static void main(String[] args) {
        HashSet<Person> people = new HashSet<>();

        people.add(new Person("John", 25));
        people.add(new Person("Jane", 30));
        // This won't be added as it's considered equal to existing person
        people.add(new Person("John", 25));

        System.out.println("People in set: " + people.size()); // Prints 2
        System.out.println(people);
    }
}
```

## 10. Performance Considerations

- Initial Capacity: Choose appropriate initial capacity to avoid resizing
- Load Factor: Default is 0.75, lower values decrease collisions but increase space usage
- equals() and hashCode(): Must be properly implemented for custom objects
- Time Complexity: add(), remove(), contains() operations are O(1) on average

## 11. Navigable Set Operations (TreeSet)

**Note:** HashSet doesn't maintain order and doesn't provide these operations directly. To use ordered set operations, convert your HashSet to a TreeSet:

```java
HashSet<Integer> hashSet = new HashSet<>(Arrays.asList(5, 2, 8, 1, 9, 3));
TreeSet<Integer> treeSet = new TreeSet<>(hashSet);

// Get first element
Integer first = treeSet.first();  // Returns 1

// Get last element
Integer last = treeSet.last();    // Returns 9

// Get higher element (strictly greater)
Integer higher = treeSet.higher(5);  // Returns 8

// Get lower element (strictly less)
Integer lower = treeSet.lower(5);    // Returns 3

// Get ceiling (greater than or equal to)
Integer ceiling = treeSet.ceiling(4);  // Returns 5

// Get floor (less than or equal to)
Integer floor = treeSet.floor(4);      // Returns 3

// Poll first element (retrieve and remove)
Integer firstElement = treeSet.pollFirst();  // Returns 1 and removes it

// Poll last element (retrieve and remove)
Integer lastElement = treeSet.pollLast();    // Returns 9 and removes it

// Get subset
NavigableSet<Integer> subset = treeSet.subSet(3, true, 8, true);  // [3, 5, 8]

// Get head set (less than)
NavigableSet<Integer> headSet = treeSet.headSet(5, false);  // [1, 2, 3]

// Get tail set (greater than or equal)
NavigableSet<Integer> tailSet = treeSet.tailSet(5, true);   // [5, 8, 9]

// Get descending set
NavigableSet<Integer> descSet = treeSet.descendingSet();    // [9, 8, 5, 3, 2, 1]
```

## 12. Using LinkedHashSet for Predictable Iteration Order

```java
// LinkedHashSet maintains insertion order
LinkedHashSet<String> linkedSet = new LinkedHashSet<>();
linkedSet.add("C");
linkedSet.add("A");
linkedSet.add("B");

System.out.println(linkedSet);  // Prints: [C, A, B]

// Convert HashSet to LinkedHashSet to preserve original encounter order
HashSet<String> hashSet = new HashSet<>(Arrays.asList("X", "Y", "Z"));
LinkedHashSet<String> orderedSet = new LinkedHashSet<>(hashSet);
```
