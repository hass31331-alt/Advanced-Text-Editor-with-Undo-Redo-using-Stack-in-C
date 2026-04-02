# Advanced-Text-Editor-with-Undo-Redo-using-Stack-in-C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 100

// Stack structure
typedef struct Stack {
    char data[MAX][100];
    int top;
} Stack;

// Initialize stack
void init(Stack *s) {
    s->top = -1;
}

// Push to stack
void push(Stack *s, char *text) {
    if (s->top == MAX - 1) {
        printf("Stack Overflow\n");
        return;
    }
    strcpy(s->data[++(s->top)], text);
}

// Pop from stack
char* pop(Stack *s) {
    if (s->top == -1) {
        return NULL;
    }
    return s->data[(s->top)--];
}

// Display current text
void display(char *text) {
    printf("\nCurrent Text: %s\n", text);
}

int main() {
    Stack undoStack, redoStack;
    char currentText[100] = "";
    char input[100];
    int choice;

    init(&undoStack);
    init(&redoStack);

    while (1) {
        printf("\n--- Text Editor ---\n");
        printf("1. Write Text\n");
        printf("2. Undo\n");
        printf("3. Redo\n");
        printf("4. Display\n");
        printf("5. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);
        getchar(); // clear buffer

        switch (choice) {
            case 1:
                push(&undoStack, currentText); // save state
                printf("Enter new text: ");
                fgets(input, 100, stdin);
                input[strcspn(input, "\n")] = 0;
                strcpy(currentText, input);
                init(&redoStack); // clear redo
                break;

            case 2: {
                char *prev = pop(&undoStack);
                if (prev != NULL) {
                    push(&redoStack, currentText);
                    strcpy(currentText, prev);
                } else {
                    printf("Nothing to undo\n");
                }
                break;
            }

            case 3: {
                char *next = pop(&redoStack);
                if (next != NULL) {
                    push(&undoStack, currentText);
                    strcpy(currentText, next);
                } else {
                    printf("Nothing to redo\n");
                }
                break;
            }

            case 4:
                display(currentText);
                break;

            case 5:
                printf("Exiting...\n");
                exit(0);

            default:
                printf("Invalid choice\n");
        }
    }

    return 0;
}
