# Linked List Revision

1.  Consider the following two linked list representations:

    ```c
    // Representation 1
    struct node {
        int value;
        struct node *next;
    };
    
    int listLength(struct node *list);
    ```

    ```c
    // Representation 2
    struct node {
        int value;
        struct node *next;
    };
    
    struct list {
        struct node *head;
    };
    
    int listLength(struct list *list);
    ```

    1.  Compare the two representations diagramatically.

        Rep1:

        ```
        [x] -> [y] -> [z] -> NULL
        ```

        Rep2:

        ```
        [head]
           v
          [x] -> [y] -> [z] -> NULL
        ```

    2.  How is an empty list represented in each case?

        Rep1: `NULL`

        Rep2: `list->head == NULL`

    3.  What are the advantages of having a separate list struct as in Representation 2?

        *   Functions that modify the list don't need to return a pointer to it.
        *   Additional fields can be included in the wrapper struct and maintained to speed up certain computations (e.g. `size`, `tail`).

2.  Consider the following simple linked list representation:

    ```c
    struct node {
        int value;
        struct node *next;
    };
    ```

    Write a function to sum the values in the list. Implement it first using `while` and then using `for`.

    ```c
    int sumWhile(struct node *head) {
        int sum = 0;
        struct node *n = head;
        while (n != NULL) {
            sum += n->value;
            n = n->next;
        }
        return sum;
    }
    
    int sumFor(struct node *head) {
        int sum = 0;
        for (struct node *n = head; n != NULL; n = n->next) {
            sum += n->value;
        }
        return sum;
    }
    ```

3.  Implement a function to delete the first instance of a value from a list, if it exists. Use the following list representation and prototype:

    ```c
    struct node {
        int value;
        struct node *next;
    };
    
    struct node *listDelete(struct node *list, int value) {
        if (list == NULL) return list;

        struct node *prev = NULL;
        struct node *curr;
        for (curr = list; curr != NULL; curr = curr->next) {
            if (curr->value == value) break;
            prev = curr;
        }

        if (curr == NULL) return list;

        // delete head
        if (prev == NULL) {
            struct node *newHead = curr->next;
            free(curr);
            return newHead;
        }

        // delete middle or end
        prev->next = curr->next;
        free(curr);

        return list;
    }
    ```
    
    How would the implementation and prototype be different if the following list representation was used instead?
    
    ```c
    struct node {
        int value;
        struct node *next;
    };
    
    struct list {
    	struct node *head;
    };
    
    void listDelete(struct list *list, int value) {
        if (list->head == NULL) return;

        struct node *prev = NULL;
        struct node *curr;
        for (curr = list->head; curr != NULL; curr = curr->next) {
            if (curr->value == value) break;
            prev = curr;
        }

        if (curr == NULL) return;

        // delete head
        if (prev == NULL) {
            list->head = curr->next;
            free(curr);
        }

        // delete middle or end
        prev->next = curr->next;
        free(curr);
    }
    ```
