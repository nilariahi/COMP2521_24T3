# ADTs

1.  A classic problem in computer science is the problem of implementing a queue using two stacks. Consider the following stack ADT interface:

    ```c
    // Creates a new empty stack O(1)
    Stack StackNew(void);
    
    // Pushes an item onto the stack O(1)
    void StackPush(Stack s, int item);
    
    // Pops an item from the stack and returns it O(1)
    // Assumes that the stack is not empty
    int StackPop(Stack s);
    
    // Returns the number of items on the stack O(1)
    int StackSize(Stack s);
    ```

    Use the stack ADT interface to complete the implementation of the queue ADT below:

    ```c
    #include "Stack.h"
    
    struct queue {
        Stack s1;
        Stack s2;
    };
    
    Queue QueueNew(void) {
        Queue q = malloc(sizeof(struct queue));
        q->s1 = StackNew();
        q->s2 = StackNew();
        return q;
    }
    
    // O(1)
    void QueueEnqueue(Queue q, int item) {
        StackPush(q->s1, item);
    }
    
    // O(n)
    int QueueDequeue(Queue q) {
        if (StackSize(q->s2) == 0) {
            while (StackSize(q->s1) > 0) {
                int it = StackPop(q->s1);
                StackPush(q->s2, it);
            } 
        }
    
        return StackPop(q->s2);
    }
    ```