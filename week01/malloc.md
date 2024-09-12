# Malloc Revision

1.  What does malloc do?

    Allocates memory on the heap.

2.  Explain how these two pieces of code differ:

    ```c
    int main(void) {
        stackInt();
    }
    
    void stackInt(void) {
        int a = 5;
    }
    ```

    `stackInt` creates an `int` (allocated on the stack) which is automatically de-allocated once the function returns.

    ```c
    int main(void) {
        heapInt();
    }
    
    void heapInt(void) {
        int *a = malloc(sizeof(int));
        *a = 5;
    }
    ```

    `heapInt` mallocs an `int` (allocated on the heap) which persists even after `heapInt` returns, creating a memory leak in this case.

3.  Modify the code below so that it allocates the struct on the heap, instead of the stack.

    ```c
    struct node {
        int value;
        struct node *next;
    };
    
    int main(void) {
        struct node n;
        n.value = 42;
        n.next = NULL;
    }
    ```

    #### Modified code

    ```c
    struct node {
        int value;
        struct node *next;
    };
    
    int main(void) {
        struct node *n = malloc(sizeof(struct node));
        n->value = 42;
        n->next = NULL;
    }
    ```

4.  The following code creates an array of 5 integers on the stack and uses it to store some values. How can you allocate the array on the heap instead?

    ```c
    int main(void) {
        int a[5];
        for (int i = 0; i < 5; i++) {
            a[i] = 42;
        }
    }
    ```

    #### Modified code

    ```c
    int main(void) {
        int *a = malloc(sizeof(int) * 5);
        for (int i = 0; i < 5; i++) {
            a[i] = 42;
        }
    }
    ```

    
