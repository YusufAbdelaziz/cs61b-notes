# Extension vs Delegation.

- Extension is done by inheriting. In Extension, you need to know how the methods are implemented inside the parent class. So you’re saying that your class acts similarly to the class that it extends form.
    
    ```java
    public class ExtensionStack<Item> extends LinkedList<Item> {
        public void push(Item x) {
            add(x);
        }
    }
    ```
    
- Delegation is done by instantiating a class inside another one or passing an object of a class to another class. Delegation is used when you don’t want your class to be a version of the class you’re pulling the methods from.
    
    ```java
    public class DelegationStack<Item> {
        private LinkedList<Item> L = new LinkedList<Item>();
        public void push(Item x) {
            L.add(x);
        }
    }
    
    // Or
    
    public class StackAdapter<Item> {
        private List L;
        public StackAdapter(List<Item> worker) {
            L = worker;
        }
    
        public void push(Item x) {
            L.add(x);
        }
    }
    ```
    
- Second approach is similar to the first, except that it adds more flexibility of the implementation we want, we may pass any class that implements the List interface like ArrayList, LinkedList, etc.