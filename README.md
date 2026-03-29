# Data Structure: Naura's Mini Project 
A mini project from Data Structure. Implementation of Stack, Queue and Deque.

# Case Explanation 
Untuk Mini Project, saya memiliki ide untuk membuat program yang dapat menyimpan Book Wishlist. Yang bisa membantu orang-orang yang suka membaca buku untuk menyimpan wishlist yang mau mereka beli atau baca ke dalam sini. 
**[Queue]** -> Bisa menginput buku yang ingin mereka baca dan akan disimpan dalam stack.
**[Deque]** -> Bisa memprioritaskan buku yang mereka mau baca, bisa pula mencoret atau menghilangkan buku dari list jika mereka sudah membacanya.
**[Stack]** -> Lalu juga bisa undo untuk melihat apa yang terakhir kali user lakukan. 

*For the Mini Project, I have an idea to create a program that can store a book wishlist. This would help people who love reading to save a list of books they want to buy or read. 
**[Queue]** -> Users can add books they want to read, and these will be stored in a queue.
**[Deque]** -> Users can prioritize the books they want to read, and they can also cross out or remove books from the list once they’ve read them.
**[Stack]** -> Users can also use the undo feature to see what they did last.*

# Source Code (C)
~~~c
#include <stdio.h>
#include <string.h>

#define MAX 100

typedef struct {
    char title[100];
} Book;

/*Stack itu buat undo push n pop, queue buat input blkg, deque buat input dari depan*/
typedef struct {
    Book data[MAX];
    int top;
} Stack;

typedef struct {
    Book data[MAX];
    int front, rear;
} Queue;

typedef struct {
    Book data[MAX];
    int front, rear;
} Deque;


Stack undoStack;
Queue readingQueue;
Deque wishlist;


void Stacknya(Stack *s) {
    s->top = -1;
}

void Queuenya(Queue *q) {
    q->front = q->rear = -1;
}

void Dequenya(Deque *d) {
    d->front = MAX / 2;
    d->rear = MAX / 2 - 1;
}


//stack
void push(Stack *s, Book b) {
    if (s->top < MAX - 1)
        s->data[++s->top] = b;
}

Book pop(Stack *s) {
    Book empty = {"EMPTY"};
    if (s->top == -1) return empty;
    return s->data[s->top--];
}


//queue
void enqueue(Queue *q, Book b) {
    if (q->rear == MAX - 1) return;

    if (q->front == -1) q->front = 0;
    q->data[++q->rear] = b;
}


//deque
void addRear(Deque *d, Book b) {
    if (d->rear == MAX - 1) return;
    d->data[++d->rear] = b;
}

void addFront(Deque *d, Book b) {
    if (d->front == 0) return;
    d->data[--d->front] = b;
}

Book removeFrontDeque(Deque *d) {
    Book empty = {"EMPTY"};

    if (d->front > d->rear)
        return empty;

    return d->data[d->front++];
}


void showWishlist() {
    if (wishlist.front > wishlist.rear) {
        printf("Wishlist empty!\n");
        return;
    }

    printf("\n=== YOUR WISHLIST ===\n");
    for (int i = wishlist.front; i <= wishlist.rear; i++) {
        printf("%d. %s", i - wishlist.front + 1, wishlist.data[i].title);
    }
}

void menu() {
    int choice;
    Book b;

    do {
        printf("\n===== BOOK WISHLIST =====\n");
        printf("1. Add Book\n");
        printf("2. Add Priority Book\n");
        printf("3. Start Reading Book\n");
        printf("4. Undo\n");
        printf("5. Show Wishlist\n");
        printf("6. Exit\n");
        printf("Choice: "); scanf("%d", &choice);
        getchar();

        switch(choice) {
            case 1:
                printf("Book title: ");
                fgets(b.title, 100, stdin);

                addRear(&wishlist, b);
                push(&undoStack, b);
                printf("Added to wishlist!\n");
                break;

            case 2:
                printf("Priority book: ");
                fgets(b.title, 100, stdin);

                addFront(&wishlist, b);
                push(&undoStack, b);
                printf("Priority book added!\n");
                break;

            case 3:
                b = removeFrontDeque(&wishlist);

                if (strcmp(b.title, "EMPTY") != 0) {
                    enqueue(&readingQueue, b);
                    printf("Now reading: %s", b.title);
                } else {
                    printf("Wishlist empty!\n");
                }
                break;

            case 4:
                b = pop(&undoStack);

                if (strcmp(b.title, "EMPTY") != 0)
                    printf("Undo: %s", b.title);
                else
                    printf("Nothing to undo!\n");
                break;

            case 5:
                showWishlist();
                break;

        }

    } while(choice != 6);
}

int main() {
    Stacknya(&undoStack);
    Queuenya(&readingQueue);
    Dequenya(&wishlist);

    menu();
    return 0;
}
~~~

# How To Run 
**[1] Add Book Function**
-> Dapat menginput judul buku yang ingin disimpan. 
<img width="1428" height="261" alt="image" src="https://github.com/user-attachments/assets/bf9470bc-f42a-4c08-bfa3-e88bb9802fd8" />


**[2] Show Wishlist Function**
-> Output daftar buku yang sudah di input oleh user.
<img width="1262" height="289" alt="image" src="https://github.com/user-attachments/assets/e1f82296-acf8-4cd4-bf8f-8ab768ca435d" />


**[3] Add Priority Book Function**
-> Memprioritaskan buku yang diinput dengan menyimpannya di top queue. 
<img width="1172" height="545" alt="image" src="https://github.com/user-attachments/assets/b571f139-6b40-40df-83ec-94b2ab1abe30" />


**[4] Start Reading Book Function**
-> Menghilangkan daftar buku yang paling _top_ atau mem pop agar sudah tercoret dari queue.
<img width="1021" height="486" alt="image" src="https://github.com/user-attachments/assets/1d287ff2-fdf9-42dc-a1d2-f3a80197cbed" />


**[5] Undo Function**
-> Melihat fungsi yang terakhir dilakukan oleh user.
<img width="916" height="212" alt="image" src="https://github.com/user-attachments/assets/130d25b8-8e77-420e-9ebb-1821d9bc1b2a" />


**[6] Quit Function**
-> Keluar dari program.
<img width="959" height="199" alt="image" src="https://github.com/user-attachments/assets/8bcc43b1-29c0-4519-8b13-7a8922fd7114" />

