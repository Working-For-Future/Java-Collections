# Java Maps Guide: HashMap, TreeMap, LinkedHashMap

This guide covers the common operations and methods for the three primary Map implementations in Java: HashMap, TreeMap, and LinkedHashMap with practical examples.

## Table of Contents
1. [Basics of Maps](#basics-of-maps)
2. [HashMap](#hashmap)
3. [TreeMap](#treemap)
4. [LinkedHashMap](#linkedhashmap)
5. [Common Operations](#common-operations)
6. [Iterating Maps](#iterating-maps)
7. [Advanced Operations](#advanced-operations)

## 1. Basics of Maps

### 1.1 Map Declaration

```java
// Basic declaration
Map<KeyType, ValueType> map = new HashMap<>();
Map<KeyType, ValueType> map = new TreeMap<>();
Map<KeyType, ValueType> map = new LinkedHashMap<>();

// With initial capacity
Map<String, Integer> hashMap = new HashMap<>(16);

// With initial capacity and load factor
Map<String, Integer> hashMap = new HashMap<>(16, 0.75f);

// From another map
Map<String, Integer> existingMap = new HashMap<>();
Map<String, Integer> newMap = new HashMap<>(existingMap);
```

**Example: Creating a student grade map**
```java
// Simple student grades map
Map<String, Integer> studentGrades = new HashMap<>();
studentGrades.put("John", 85);
studentGrades.put("Lisa", 92);
studentGrades.put("Mike", 78);

// Creating a map with initial capacity for 100 students
Map<String, Integer> largeClassGrades = new HashMap<>(100);

// Creating a map from another map (copying grades)
Map<String, Integer> backupGrades = new HashMap<>(studentGrades);
```

### 1.2 Differences Between Map Implementations

| Map Implementation | Ordering | Performance | Null Keys/Values | Thread Safety |
|-------------------|----------|-------------|------------------|---------------|
| **HashMap** | No guaranteed order | O(1) for basic operations | Allows one null key and multiple null values | Not thread-safe |
| **TreeMap** | Sorted by natural ordering or comparator | O(log n) for basic operations | No null keys (with natural ordering), null values allowed | Not thread-safe |
| **LinkedHashMap** | Insertion order (by default) | O(1) for basic operations | Allows one null key and multiple null values | Not thread-safe |

**Example: Demonstrating different map behaviors**
```java
public class MapTypesDemo {
    public static void main(String[] args) {
        // Create three different maps
        Map<String, String> hashMap = new HashMap<>();
        Map<String, String> treeMap = new TreeMap<>();
        Map<String, String> linkedHashMap = new LinkedHashMap<>();
        
        // Add the same elements to each map
        for (Map<String, String> map : Arrays.asList(hashMap, treeMap, linkedHashMap)) {
            map.put("C", "Charlie");
            map.put("A", "Alpha");
            map.put("B", "Bravo");
            map.put("D", "Delta");
        }
        
        // Display the order of elements in each map
        System.out.println("HashMap (random order):");
        hashMap.forEach((key, value) -> System.out.println(key + " -> " + value));
        
        # HashMap (random order):
        A -> Alpha
        B -> Bravo
        C -> Charlie
        D -> Delta
        (Note: HashMap output order may vary between runs as it doesn't guarantee any specific order)

        System.out.println("\nTreeMap (sorted by key):");
        treeMap.forEach((key, value) -> System.out.println(key + " -> " + value));
        
        # TreeMap (sorted by key):
        A -> Alpha
        B -> Bravo
        C -> Charlie
        D -> Delta

        System.out.println("\nLinkedHashMap (insertion order):");
        linkedHashMap.forEach((key, value) -> System.out.println(key + " -> " + value));

        # LinkedHashMap (insertion order):
        C -> Charlie
        A -> Alpha
        B -> Bravo
        D -> Delta
    }
}




```

## 2. Common Operations

### 2.1 Adding Elements

```java
// Basic put
map.put("key", value);

// Put if absent (only adds if key doesn't exist)
map.putIfAbsent("key", value);

// Put all entries from another map
Map<String, Integer> source = new HashMap<>();
map.putAll(source);
```

**Example: Building a product inventory**
```java
public class InventoryExample {
    public static void main(String[] args) {
        // Create a product inventory with prices
        Map<String, Double> inventory = new HashMap<>();

        // Add products with prices
        inventory.put("Laptop", 999.99);
        inventory.put("Smartphone", 699.99);
        inventory.put("Headphones", 149.99);

        // Don't update price if product already exists
        inventory.putIfAbsent("Laptop", 899.99); // Won't change existing price
        System.out.println("Laptop price: $" + inventory.get("Laptop")); 
        // Output: Laptop price: $999.99

        // Add seasonal products
        Map<String, Double> holidayItems = new HashMap<>();
        holidayItems.put("Smart Speaker", 79.99);
        holidayItems.put("Fitness Tracker", 89.99);

        // Add all seasonal products to main inventory
        inventory.putAll(holidayItems);

        System.out.println("Complete inventory:");
        // Output: Complete inventory:
        
        inventory.forEach((product, price) ->
            System.out.println(product + ": $" + price));
        /*
         Output:
         Laptop: $999.99
         Smartphone: $699.99
         Headphones: $149.99
         Smart Speaker: $79.99
         Fitness Tracker: $89.99
        */
    }
}
```

### 2.2 Retrieving Elements

```java
// Get a value
ValueType value = map.get("key");

// Get with default value if key doesn't exist
ValueType value = map.getOrDefault("key", defaultValue);

// Check if contains key
boolean hasKey = map.containsKey("key");

// Check if contains value
boolean hasValue = map.containsValue(value);

// Get the size of map
int size = map.size();

// Check if map is empty
boolean isEmpty = map.isEmpty();
```

**Example: User preferences system**
```java
public class UserPreferences {
    public static void main(String[] args) {
        // User settings with default values where needed
        Map<String, String> userSettings = new HashMap<>();
        userSettings.put("theme", "dark");
        userSettings.put("fontSize", "medium");
        
        // Retrieve known setting
        String theme = userSettings.get("theme");
        System.out.println("Current theme: " + theme); 
        // Output: Current theme: dark
        
        // Retrieve with default for potentially missing setting
        String language = userSettings.getOrDefault("language", "english");
        System.out.println("Language: " + language); 
        // Output: Language: english
        
        // Check if specific settings exist
        if (userSettings.containsKey("notifications")) {
            System.out.println("Notification setting exists");
        } else {
            System.out.println("Notification setting doesn't exist");
        }
        // Output: Notification setting doesn't exist
        
        // Check if specific value exists
        if (userSettings.containsValue("dark")) {
            System.out.println("Someone is using dark theme");
        }
        // Output: Someone is using dark theme
        
        // Get number of settings
        System.out.println("Number of settings: " + userSettings.size());
        // Output: Number of settings: 2
        
        // Check if any settings exist
        System.out.println("Has settings: " + !userSettings.isEmpty());
        // Output: Has settings: true
    }
}
```

### 2.3 Removing Elements

```java
// Remove by key
ValueType removedValue = map.remove("key");

// Remove by key-value pair (only removes if key is mapped to specified value)
boolean removed = map.remove("key", value);

// Clear all entries
map.clear();
```

**Example: Shopping cart management**
```java
public class ShoppingCartExample {
    public static void main(String[] args) {
        // Shopping cart with item names and quantities
        Map<String, Integer> cart = new HashMap<>();
        
        // Add items to cart
        cart.put("Book", 2);
        cart.put("Pen", 5);
        cart.put("Notebook", 1);
        
        System.out.println("Initial cart: " + cart);
        // Output (order may vary by runtime):
        // Initial cart: {Book=2, Pen=5, Notebook=1}
        
        // Remove an item completely
        Integer removedQuantity = cart.remove("Pen");
        System.out.println("Removed " + removedQuantity + " pens from cart");
        // Output: Removed 5 pens from cart
        
        // Try to remove item only if it has a specific quantity
        boolean removed = cart.remove("Book", 3);
        System.out.println("Removed books? " + removed);
        // Output: Removed books? false
        
        // Try again with correct quantity
        removed = cart.remove("Book", 2);
        System.out.println("Removed books? " + removed);
        // Output: Removed books? true
        
        System.out.println("Updated cart: " + cart);
        // Output (order may vary): Updated cart: {Notebook=1}
        
        // Clear the cart completely (checkout)
        cart.clear();
        System.out.println("After checkout, cart is empty: " + cart.isEmpty());
        // Output: After checkout, cart is empty: true
    }
}
```

## 3. HashMap

HashMap is the most commonly used Map implementation. It offers constant-time performance for basic operations (get and put).

**Example: Word frequency counter**
```java
public class WordFrequencyCounter {
    public static void main(String[] args) {
        String text = "to be or not to be that is the question whether tis nobler in the mind to suffer";
        
        // Convert to array of words and count frequency
        String[] words = text.toLowerCase().split(" ");
        Map<String, Integer> wordFrequency = new HashMap<>();
        
        for (String word : words) {
            // If word exists, increment count, otherwise set to 1
            wordFrequency.put(word, wordFrequency.getOrDefault(word, 0) + 1);
        }
        
        System.out.println("Word frequency analysis:");
        // Output (order may vary):
        // Word frequency analysis:
        
        for (Map.Entry<String, Integer> entry : wordFrequency.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue() + " occurrences");
        }
        /* 
         Possible output lines (order not guaranteed):
         to: 3 occurrences
         be: 2 occurrences
         or: 1 occurrences
         not: 1 occurrences
         that: 1 occurrences
         is: 1 occurrences
         the: 2 occurrences
         question: 1 occurrences
         whether: 1 occurrences
         tis: 1 occurrences
         nobler: 1 occurrences
         in: 1 occurrences
         mind: 1 occurrences
         suffer: 1 occurrences
        */
        
        // Find the most frequent word
        String mostFrequentWord = "";
        int maxFrequency = 0;
        
        for (Map.Entry<String, Integer> entry : wordFrequency.entrySet()) {
            if (entry.getValue() > maxFrequency) {
                maxFrequency = entry.getValue();
                mostFrequentWord = entry.getKey();
            }
        }
        
        System.out.println("\nMost frequent word: '" + mostFrequentWord + 
                "' with " + maxFrequency + " occurrences");
        // Output: 
        // Most frequent word: 'to' with 3 occurrences
    }
}
```

## 4. TreeMap

TreeMap stores its entries in a sorted order based on keys.

**Example: Employee salary ranking**
```java
public class SalaryRanking {
    public static void main(String[] args) {
        // TreeMap to automatically sort employees by salary (key)
        TreeMap<Double, String> salaryRanking = new TreeMap<>();
        
        // Add employee salaries (mapping salary -> employee name)
        salaryRanking.put(75000.00, "John");
        salaryRanking.put(85000.00, "Alice");
        salaryRanking.put(65000.00, "Bob");
        salaryRanking.put(95000.00, "Sarah");
        salaryRanking.put(55000.00, "Mike");
        
        System.out.println("Employees sorted by salary (ascending):");
        // Output (ascending by key):
        // Employees sorted by salary (ascending):
        // Mike: $55000.0
        // Bob: $65000.0
        // John: $75000.0
        // Alice: $85000.0
        // Sarah: $95000.0
        
        salaryRanking.forEach((salary, name) ->
            System.out.println(name + ": $" + salary));
        
        // Get highest and lowest paid employees
        Map.Entry<Double, String> highestPaid = salaryRanking.lastEntry();
        Map.Entry<Double, String> lowestPaid = salaryRanking.firstEntry();
        
        System.out.println("\nHighest paid: " + highestPaid.getValue() +
                " with $" + highestPaid.getKey());
        // Output:
        // Highest paid: Sarah with $95000.0
        
        System.out.println("Lowest paid: " + lowestPaid.getValue() +
                " with $" + lowestPaid.getKey());
        // Output:
        // Lowest paid: Mike with $55000.0
        
        // Get salary ranges
        SortedMap<Double, String> midTierSalaries = salaryRanking.subMap(70000.00, 90000.00);
        
        System.out.println("\nMid-tier salary employees ($70k-$90k):");
        // Output:
        // Mid-tier salary employees ($70k-$90k):
        
        midTierSalaries.forEach((salary, name) ->
            System.out.println(name + ": $" + salary));
        // Output:
        // John: $75000.0
        // Alice: $85000.0
    }
}
```

### 4.1 TreeMap-Specific Operations

```java
TreeMap<String, Integer> treeMap = new TreeMap<>();
treeMap.put("A", 1);
treeMap.put("C", 3);
treeMap.put("B", 2);
treeMap.put("D", 4);

// Get first/last entries
Map.Entry<String, Integer> firstEntry = treeMap.firstEntry(); // A=1
Map.Entry<String, Integer> lastEntry = treeMap.lastEntry();   // D=4

// Get first/last keys
String firstKey = treeMap.firstKey();  // A
String lastKey = treeMap.lastKey();    // D

// Get entry with least key greater than or equal to given key
Map.Entry<String, Integer> ceilingEntry = treeMap.ceilingEntry("B");  // B=2

// Get entry with greatest key less than or equal to given key
Map.Entry<String, Integer> floorEntry = treeMap.floorEntry("B");      // B=2

// Get entry with least key strictly greater than given key
Map.Entry<String, Integer> higherEntry = treeMap.higherEntry("B");    // C=3

// Get entry with greatest key strictly less than given key
Map.Entry<String, Integer> lowerEntry = treeMap.lowerEntry("B");      // A=1
```

**Example: Temperature readings throughout the day**
```java
public class TemperatureTracker {
    public static void main(String[] args) {
        // Track hourly temperatures using TreeMap for time-based sorting
        TreeMap<Integer, Double> hourlyTemps = new TreeMap<>();
        
        // Record some temperature readings (hour, temperature in Celsius)
        hourlyTemps.put(8, 18.5);   // 8 AM
        hourlyTemps.put(12, 24.3);  // 12 PM
        hourlyTemps.put(16, 26.0);  // 4 PM
        hourlyTemps.put(20, 22.5);  // 8 PM
        hourlyTemps.put(0, 16.8);   // 12 AM
        hourlyTemps.put(4, 15.2);   // 4 AM
        
        System.out.println("Hourly temperature readings:");
        hourlyTemps.forEach((hour, temp) ->
            System.out.println(hour + ":00 - " + temp + "°C"));
        /*
         Possible output (ordered by hour):
           Hourly temperature readings:
           0:00 - 16.8°C
           4:00 - 15.2°C
           8:00 - 18.5°C
           12:00 - 24.3°C
           16:00 - 26.0°C
           20:00 - 22.5°C
        */
        
        // Current time is 14 (2 PM) - get nearest readings
        int currentHour = 14;
        
        // Find the temperatures right before and after current time
        Map.Entry<Integer, Double> previousReading = hourlyTemps.floorEntry(currentHour);
        Map.Entry<Integer, Double> nextReading = hourlyTemps.ceilingEntry(currentHour);
        
        System.out.println("\nCurrent time: " + currentHour + ":00");
        // Output: Current time: 14:00
        
        System.out.println("Previous reading: " + previousReading.getKey() +
                ":00 - " + previousReading.getValue() + "°C");
        // Output: Previous reading: 12:00 - 24.3°C
        
        System.out.println("Next reading: " + nextReading.getKey() +
                ":00 - " + nextReading.getValue() + "°C");
        // Output: Next reading: 16:00 - 26.0°C
        
        // Get morning temperatures (from 6 AM to 12 PM)
        SortedMap<Integer, Double> morningTemps = hourlyTemps.subMap(6, 13);
        
        System.out.println("\nMorning temperatures (6 AM - 12 PM):");
        morningTemps.forEach((hour, temp) ->
            System.out.println(hour + ":00 - " + temp + "°C"));
        /*
         Possible output:
           Morning temperatures (6 AM - 12 PM):
           8:00 - 18.5°C
           12:00 - 24.3°C
        */
        
        // Get daily high and low
        Map.Entry<Integer, Double> firstTemp = hourlyTemps.firstEntry();
        Map.Entry<Integer, Double> lastTemp = hourlyTemps.lastEntry();
        
        System.out.println("\nFirst reading: " + firstTemp.getKey() +
                ":00 - " + firstTemp.getValue() + "°C");
        // Output: First reading: 0:00 - 16.8°C
        
        System.out.println("Last reading: " + lastTemp.getKey() +
                ":00 - " + lastTemp.getValue() + "°C");
        // Output: Last reading: 20:00 - 22.5°C
        
        // Get highest and lowest temperature regardless of time
        double max = Collections.max(hourlyTemps.values());
        double min = Collections.min(hourlyTemps.values());
        
        System.out.println("\nHighest temperature: " + max + "°C");
        // Output: Highest temperature: 26.0°C
        
        System.out.println("Lowest temperature: " + min + "°C");
        // Output: Lowest temperature: 15.2°C
    }
}
```

## 5. LinkedHashMap

LinkedHashMap maintains insertion order (by default).

**Example: Browser history tracking**
```java
public class BrowserHistory {
    public static void main(String[] args) {
        // Track visited URLs with timestamps using LinkedHashMap to maintain visit order
        LinkedHashMap<String, String> browserHistory = new LinkedHashMap<>();
        
        // Simulate user browsing session (URL, timestamp)
        browserHistory.put("https://www.google.com", "10:30:00");
        browserHistory.put("https://www.wikipedia.org", "10:32:15");
        browserHistory.put("https://www.stackoverflow.com", "10:35:42");
        browserHistory.put("https://www.github.com", "10:37:50");
        browserHistory.put("https://www.example.com", "10:40:11");
        
        System.out.println("Browser history (in order of visits):");
        browserHistory.forEach((url, time) ->
            System.out.println(time + " - " + url));
        /*
             Possible output (order will match insertion):
               Browser history (in order of visits):
               10:30:00 - https://www.google.com
               10:32:15 - https://www.wikipedia.org
               10:35:42 - https://www.stackoverflow.com
               10:37:50 - https://www.github.com
               10:40:11 - https://www.example.com
        */
        
        // User visits a previously visited site (moves to end of history in LinkedHashMap)
        browserHistory.put("https://www.wikipedia.org", "10:45:33");
        
        System.out.println("\nUpdated browser history after revisiting Wikipedia:");
        browserHistory.forEach((url, time) ->
            System.out.println(time + " - " + url));
        /*
             Possible output:
               Updated browser history after revisiting Wikipedia:
               10:30:00 - https://www.google.com
               10:35:42 - https://www.stackoverflow.com
               10:37:50 - https://www.github.com
               10:40:11 - https://www.example.com
               10:45:33 - https://www.wikipedia.org
        */
    }
}
```

### 5.1 Access Order LinkedHashMap (LRU Cache)

```java
// LinkedHashMap with access order (true) instead of insertion order (false)
// Last parameter specifies to remove oldest entries when a size limit is exceeded
Map<String, Integer> lruCache = new LinkedHashMap<>(16, 0.75f, true) {
    @Override
    protected boolean removeEldestEntry(Map.Entry<String, Integer> eldest) {
        return size() > 3; // Limit cache to 3 entries
    }
};
```

**Example: Simple LRU cache implementation**
```java
public class LRUCacheExample {
    public static void main(String[] args) {
        // Create a simple LRU cache with capacity of 3
        // LinkedHashMap with accessOrder=true for LRU behavior
        final int CACHE_SIZE = 3;
        LinkedHashMap<String, String> lruCache = new LinkedHashMap<String, String>(
            CACHE_SIZE, 0.75f, true) {
            @Override
            protected boolean removeEldestEntry(Map.Entry<String, String> eldest) {
                return size() > CACHE_SIZE;
            }
        };
        
        System.out.println("Adding data to cache...");
        
        // Add data to cache
        lruCache.put("user:1001", "John Smith");
        printCache(lruCache);
        /*
            Possible output:
              Adding data to cache...
              
              Current cache state (in LRU order):
              user:1001 -> John Smith
        */

        lruCache.put("user:1002", "Jane Doe");
        printCache(lruCache);
        /*
            Current cache state (in LRU order):
            user:1001 -> John Smith
            user:1002 -> Jane Doe
        */

        lruCache.put("user:1003", "Bob Johnson");
        printCache(lruCache);
        /*
            Current cache state (in LRU order):
            user:1001 -> John Smith
            user:1002 -> Jane Doe
            user:1003 -> Bob Johnson
        */

        // Access an entry (will move user:1001 to most recently used)
        System.out.println("\nAccessing user:1001");
        String user = lruCache.get("user:1001");
        System.out.println("Retrieved: " + user);
        printCache(lruCache);
        /*
            Output:
              Accessing user:1001
              Retrieved: John Smith

              Current cache state (in LRU order):
              user:1002 -> Jane Doe
              user:1003 -> Bob Johnson
              user:1001 -> John Smith
        */

        // Add a new entry - should evict the least recently used (user:1002)
        System.out.println("\nAdding user:1004");
        lruCache.put("user:1004", "Alice Brown");
        printCache(lruCache);
        /*
            Current cache state (in LRU order):
            user:1003 -> Bob Johnson
            user:1001 -> John Smith
            user:1004 -> Alice Brown
          (user:1002 was evicted)
        */

        // Check if evicted entry exists
        System.out.println("\nTrying to access user:1002");
        String evictedUser = lruCache.get("user:1002");
        System.out.println("Result: " + (evictedUser != null ? evictedUser : "Not in cache"));
        /*
            Output:
              Trying to access user:1002
              Result: Not in cache
        */
    }
    
    private static void printCache(LinkedHashMap<String, String> cache) {
        System.out.println("\nCurrent cache state (in LRU order):");
        cache.forEach((key, value) -> System.out.println(key + " -> " + value));
    }
}
```

## 6. Iterating Maps

### 6.1 Using entrySet

```java
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer value = entry.getValue();
    System.out.println(key + " -> " + value);
}
```

**Example: Product inventory report**
```java
public class InventoryReport {
    public static void main(String[] args) {
        Map<String, Integer> inventory = new HashMap<>();
        inventory.put("Laptop", 45);
        inventory.put("Smartphone", 125);
        inventory.put("Tablet", 35);
        inventory.put("Headphones", 80);
        
        int totalItems = 0;
        System.out.println("INVENTORY REPORT");
        System.out.println("----------------");
        
        // Iterate using entrySet for efficient access to both keys and values
        for (Map.Entry<String, Integer> entry : inventory.entrySet()) {
            String product = entry.getKey();
            Integer quantity = entry.getValue();
            
            System.out.println(product + ": " + quantity + " units");
            totalItems += quantity;
        }
        
        System.out.println("----------------");
        System.out.println("Total items: " + totalItems);
    }
}
```

### 6.2 Using keySet

```java
for (String key : map.keySet()) {
    Integer value = map.get(key);
    System.out.println(key + " -> " + value);
}
```

**Example: Student score lookup**
```java
public class StudentScores {
    public static void main(String[] args) {
        Map<String, Double> studentScores = new HashMap<>();
        studentScores.put("Alice", 92.5);
        studentScores.put("Bob", 84.0);
        studentScores.put("Charlie", 76.8);
        studentScores.put("Diana", 95.2);
        
        // Set passing threshold
        double passingScore = 80.0;
        
        // Process student results using keySet iteration
        System.out.println("STUDENT RESULTS");
        System.out.println("---------------");
        
        for (String student : studentScores.keySet()) {
            Double score = studentScores.get(student);
            String result = score >= passingScore ? "PASS" : "FAIL";
            
            System.out.println(student + ": " + score + " - " + result);
        }
    }
}
```

### 6.3 Using values

```java
for (Integer value : map.values()) {
    System.out.println(value);
}
```

**Example: Calculating statistics from values**
```java
public class SalesStatistics {
    public static void main(String[] args) {
        Map<String, Double> monthlySales = new HashMap<>();
        monthlySales.put("January", 45678.90);
        monthlySales.put("February", 38752.75);
        monthlySales.put("March", 52190.30);
        monthlySales.put("April", 41209.45);
        monthlySales.put("May", 56789.10);
        
        // Calculate statistics using just the values
        double total = 0;
        double max = Double.MIN_VALUE;
        double min = Double.MAX_VALUE;
        
        for (Double sales : monthlySales.values()) {
            // Update total
            total += sales;
            
            // Update max and min
            if (sales > max) max = sales;
            if (sales < min) min = sales;
        }
        
        double average = total / monthlySales.size();
        
        // Print results
        System.out.println("SALES STATISTICS");
        System.out.println("----------------");
        System.out.printf("Total Sales: $%.2f\n", total);
        System.out.printf("Average Monthly Sales: $%.2f\n", average);
        System.out.printf("Highest Month: $%.2f\n", max);
        System.out.printf("Lowest Month: $%.2f\n", min);
    }
}
```

### 6.4 Using forEach (Java 8+)

```java
map.forEach((key, value) -> {
    System.out.println(key + " -> " + value);
});
```

**Example: Task management system**
```java
public class TaskManager {
    public static void main(String[] args) {
        Map<String, String> tasks = new HashMap<>();
        tasks.put("TASK-1", "Complete project proposal");
        tasks.put("TASK-2", "Review code changes");
        tasks.put("TASK-3", "Fix login bug");
        tasks.put("TASK-4", "Update documentation");
        tasks.put("TASK-5", "Prepare demo for client");
        
        System.out.println("TASK LIST");
        System.out.println("---------");
        
        // Process tasks using forEach with a lambda
        tasks.forEach((id, description) -> {
            System.out.println("ID: " + id);
            System.out.println("Description: " + description);
            System.out.println("---------");
        });
        
        // More complex example with filter
        System.out.println("\nBUG FIX TASKS ONLY");
        System.out.println("---------");
        
        tasks.forEach((id, description) -> {
            if (description.toLowerCase().contains("bug") || 
                description.toLowerCase().contains("fix")) {
                System.out.println(id + ": " + description);
            }
        });
    }
}
```

### 6.5 Using Iterator

```java
Iterator<Map.Entry<String, Integer>> iterator = map.entrySet().iterator();
while (iterator.hasNext()) {
    Map.Entry<String, Integer> entry = iterator.next();
    System.out.println(entry.getKey() + " -> " + entry.getValue());
    
    // Safe removal during iteration
    if (entry.getValue() < 60) {
        iterator.remove();
    }
}
```

**Example: Filtering expired products**
```java
public class ExpiredProductRemoval {
    public static void main(String[] args) {
        // Map of products with days until expiration
        Map<String, Integer> products = new HashMap<>();
        products.put("Milk", 3);
        products.put("Bread", 5);
        products.put("Cheese", 2);
        products.put("Yogurt", 7);
        products.put("Apples", 1);
        products.put("Chicken", 2);
        
        System.out.println("Initial inventory: " + products);
        
        // Define expiration threshold
        int expirationThreshold = 3; // days
        
        // Remove products that will expire soon using iterator
        Iterator<Map.Entry<String, Integer>> iterator = products.entrySet().iterator();
        System.out.println("\nRemoving products expiring in less than " + expirationThreshold + " days...");
        
        while (iterator.hasNext()) {
            Map.Entry<String, Integer> entry = iterator.next();
            String product = entry.getKey();
            Integer daysLeft = entry.getValue();
            
            if (daysLeft < expirationThreshold) {
                System.out.println("Removing " + product + " (expires in " + daysLeft + " days)");
                iterator.remove();
            }
        }
        
        System.out.println("\nUpdated inventory: " + products);
    }
}
```

## 7. Advanced Operations

### 7.1 Compute Operations (Java 8+)

```java
// Compute a new value if key exists
map.compute("key", (k, v) -> v == null ? 1 : v + 1);

// Compute a new value only if key exists
map.computeIfPresent("key", (k, v) -> v + 1);

// Compute a new value only if key doesn't exist
map.computeIfAbsent("key", k -> 1);
```

**Example: Page view counter**
```java
public class PageViewCounter {
    public static void main(String[] args) {
        // Map to track page views
        Map<String, Integer> pageViews = new HashMap<>();
        
        // Simulate page visits
        String[] pageVisits = {
            "/home", "/products", "/about", "/home", "/products", 
            "/products", "/contact", "/home", "/about"
        };
        
        // Count views using compute
        for (String page : pageVisits) {
            pageViews.compute(page, (key, count) -> count == null ? 1 : count + 1);
        }
        
        System.out.println("Page view counts:");
        pageViews.forEach((page, count) -> 
            System.out.println(page + ": " + count + " views"));
        
        // Add default values for pages with no views
        String[] allPages = {"/home", "/products", "/about", "/contact", "/login", "/register"};
        
        for (String page : allPages) {
            // Only sets value if page doesn't exist in map
            pageViews.computeIfAbsent(page, k -> 0);
        }
        
        // Increment counter only if page already exists and has views
        pageViews.computeIfPresent("/login", (k, v) -> v + 1);
        
        System.out.println("\nComplete page view report:");
        pageViews.forEach((page, count) -> 
            System.out.println(page + ": " + count + " views"));
    }
}
```

### 7.2 Merge Operation (Java 8+)

```java
// Provide a new value if key doesn't exist, otherwise combine with existing value
map.merge("key", 1, (oldValue, newValue) -> oldValue + newValue);
```

**Example: Combining shopping carts**
```java
public class ShoppingCartMerge {
    public static void main(String[] args) {
        // User's saved cart
        Map<String, Integer> savedCart = new HashMap<>();
        savedCart.put("Book", 1);
        savedCart.put("Pen", 3);
        
        // User's current session cart
        Map<String, Integer> sessionCart = new HashMap<>();
        sessionCart.put("Book", 2);
        sessionCart.put("Notebook", 1);
        
        System.out.println("Saved cart: " + savedCart);
        System.out.println("Session cart: " + sessionCart);
        
        // Merge carts when user logs in
        sessionCart.forEach((product, quantity) -> 
            savedCart.merge(product, quantity, (oldQty, newQty) -> oldQty + newQty));
        
        System.out.println("\nMerged cart: " + savedCart);
        
        // Add another product with merge
        savedCart.merge("Pencil", 5, (oldQty, newQty) -> oldQty + newQty);
        
        System.out.println("After adding pencils: " + savedCart);
        
        // Remove product by merging with null remapping function
        savedCart.merge("Pen", 0, (oldQty, newQty) -> null);
        
        System.out.println("After removing pens: " + savedCart);
    }
}
```

### 7.3 Replace Operations

```java
// Replace value for key
map.replace("key", newValue);

// Replace only if key is mapped to
