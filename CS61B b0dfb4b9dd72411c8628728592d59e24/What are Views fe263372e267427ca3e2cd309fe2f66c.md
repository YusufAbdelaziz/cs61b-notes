# What are Views ?

- Views are just an **alternative representation** **of an actual object**, they usually limit the access that the user has to the underlying object.
- Any changes that happen to a view will affect the actual object.
    
    ```java
    List<String> l = new ArrayList<>();
    
    /** Add some items. */
    L.add(“at”); L.add(“ax”); …
    ```
    
- Now say that you only want a list from index 1 to 4. You can use $sublist$ method to do that. The sublist object is a view of the actual list object.
    
    ```java
    /** subList me up fam. */
    List<String> SL = l.subList(1, 4);
    /** Mutate that thing. */
    SL.set(0, “jug”);
    ```
    
- This is useful when you want to reverse a part of a list. So rather than giving indices to a reverse method, you can just pass a list (sublist in our case) that can be reversed instantly.
- How is that possible? sublist method returns a List object. The method then returns Sublist object that extends AbstractList which implements List interface, so the Sublist object is of type List.
    
    ```java
    // ArrayList class ...
    
    List<Item> sublist(int start, int end){
        return new this.Sublist(start,end);
    }
    ```
    
    ```java
    private class Sublist extends AbstractList<Item>{
        private int start, end;
        Sublist(inst start, int end){...}
    }
    ```
    
- The mutation operations or reading operation is done via the parent or outer class, not the sublist object itself.
- So when you’re mutating an existing item or adding a new item, you’re using the original object with an offset value (start index).
    
    ```java
    public Item get(int k){return AbstractList.this.get(start+k);}
    public void add(int l, Item x){AbstractList.this.add(start+k, x); end+=1}
    ```