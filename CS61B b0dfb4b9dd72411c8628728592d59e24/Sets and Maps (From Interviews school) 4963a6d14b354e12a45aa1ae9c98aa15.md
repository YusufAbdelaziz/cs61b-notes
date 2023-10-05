# Sets and Maps (From Interviews.school).

- Sets
    - Set is a data structure that contains a collection of unique elements, and supports insertion of new elements and deletion of the existing ones. Its concept is similar to the mathematical concept of a set.
    - There are two variations of a set implementation :
        - Hash table based sets.
            - Hash table based set performs all basic operations in constant O(1) time. In **most interview questions**, you **should use this implementation**. Here is how it looks in Java :
                
                ```java
                Set<Integer> set = new HashSet<>();
                        
                set.add(5);
                set.add(7);
                set.add(7);
                System.out.println(set);              // prints [5, 7]
                
                System.out.println(set.size());       // prints 2
                
                System.out.println(set.contains(7));  // true
                System.out.println(set.contains(8));  // false
                
                set.remove(7);
                System.out.println(set);
                ```
                
        - Tree based sets
            - Tree based sets are usually implemented with **balanced binary trees**, and perform most operations in O(logN) time.
            - In addition to the basic set operations, they also keep set elements in a sorted order, and support cool operations like these:
                
                ```java
                TreeSet<Integer> set = new TreeSet<>();
                        
                set.add(5);
                set.add(9);
                set.add(3);
                set.add(7);                        // set is now [3, 5, 7, 9], and guaranteed to be in this order
                
                System.out.println(set.first());   // prints 3, the smallest element in the set
                System.out.println(set.last());    // prints 9, the biggest element in the set
                
                // higher(X) returns the smallest element in the set greater than X
                System.out.println(set.higher(6)); // 7
                System.out.println(set.higher(7)); // 9
                System.out.println(set.higher(9)); // null
                
                // ceiling(X) returns the smallest element in the set greater or equal to X
                System.out.println(set.ceiling(6)); // 7
                System.out.println(set.ceiling(7)); // 7
                System.out.println(set.ceiling(9)); // 9
                ```
                
            
- Maps
    - Maps are data structures that contain keys and the values that the keys correspond to.
    - As with sets, there are two map variations:
        - Hash table based Map
            - Hash table based map performs all basic operations in constant O(1) time. In most interview questions, you should use this implementation. Here is how it looks in Java :
                
                ```java
                	Map<Integer, String> map = new HashMap<>();
                
                map.put(5, "Five");
                map.put(7, "Seven");
                map.put(7, "VII");
                System.out.println(map);                          // prints {5=Five, 7=VII}
                
                System.out.println(map.size());                   // prints 2
                
                System.out.println(map.containsKey(7));           // true
                System.out.println(map.containsValue("Seven"));   // false
                
                map.remove(7);
                System.out.println(map);                          // prints {5=Five}
                ```
                
        - Tree based map
            - Tree based maps are usually implemented with balanced binary trees, and perform most operations in O(logN) time.
            - As with tree based sets, they also keep elements in a sorted order (sorted by key), and support cool operations like these:
                
                ```java
                TreeMap<Integer, String> map = new TreeMap<>();
                        
                map.put(5, "Five");
                map.put(9, "Nine");
                map.put(3, "Three");
                map.put(7, "Seven");                    
                
                // prints {3=Three, 5=Five, 7=Seven, 9=Nine}, this order is guaranteed    
                System.out.println(map);                
                
                System.out.println(map.firstEntry());   // prints 3=Three, entry with the smallest key in the map
                System.out.println(map.lastEntry());    // prints 9=Nine, entry with the biggest key in the map
                
                // higherEntry(X) returns the entry in the map with the smallest key greater than X
                System.out.println(map.higherEntry(6)); // 7=Seven
                System.out.println(map.higherEntry(7)); // 9=Nine
                System.out.println(map.higherEntry(9)); // null
                
                // ceilingEntry(X) returns the entry in the map with the smallest key greater or equal to X
                System.out.println(map.ceilingEntry(6)); // 7=Seven
                System.out.println(map.ceilingEntry(7)); // 7=Seven
                System.out.println(map.ceilingEntry(9)); // 9=Nine
                ```