---
layout: post
title: Double Pointers Example
category: C++
tag: C++
---

[why use pointer](https://stackoverflow.com/questions/5580761/why-use-double-indirection-or-why-use-pointers-to-pointers)

"Use ** when you want to preserve (OR retain change in) the Memory-Allocation or Assignment even outside of a function call"


알고리즘에서는 보통 class 및 struct안에 포인터가 자기 자신을 가르키고 있거다 할경우 사용이 된다[K-D 행렬 및 doublely linked list ]

## Example

```c++
#include <iostream>
using namespace std;
//node declaration for doubly linked list
struct Node {
   int data;
   struct Node * prev, *next;
};

Node* newNode(int val)
{
   Node* temp = new Node;
   temp->data = val;
   temp->prev = temp->next = nullptr;
   return temp;
}
void displayList(Node* head)
{
   while (head->next != nullptr) {
      cout << head->data << " <==> ";
      head = head->next;
      }
   cout << head->data << endl;
}

// Insert a new node at the head of the list
void insert(Node** head, int node_data)
{
   Node* temp = newNode(node_data);
   temp->next = *head;
   (*head)->prev = temp;
   (*head) = temp;
}

// reverse the doubly linked list
void reverseList(Node** head)
{
   Node* left = *head, * right = *head;

   // traverse entire list and set right next to right
   while (right->next != nullptr)
   right = right->next;

   //swap left and right data by moving them towards each other till they meet or cross
   while (left != right && left->prev != right) {

      // Swap left and right pointer data
      swap(left->data, right->data);

      // Advance left pointer
      left = left->next;

      // Advance right pointer
      right = right->prev;
   }
}
int main()
{
   Node* headNode = newNode(5); // assigned pointer Node
   /*
   data = 5;
   struct Node * prev = null, *next = null;
   */
   insert(&headNode, 4); // of course when parsing pointer need to be in reference for another not initialized pointer pointing already existing variable of pointer.
   // A reference is an alias for an already existing variable.
   insert(&headNode, 3);
   insert(&headNode, 2);
   insert(&headNode, 1);
   cout << "Original doubly linked list: " << endl;
   displayList(headNode);
   cout << "Reverse doubly linked list: " << endl;
   reverseList(&headNode);
   displayList(headNode);

   return 0;
}
```

### Note
**A pointer in C++ is a variable that holds the memory address of another variable.**
**Reference is C++ is an address(box of memory address and value)**

A reference is an alias for an already existing variable. Once a reference is initialized to a variable, it cannot be changed to refer to another variable. Hence, a reference is similar to a const pointer.

reference is often used in LValue which is not copy variable. just alias it in directly in the allocating memory of it and change value directly already in

on Rvalue of reference is address(box) of value or some value.

and pointer need to have create allocate memory or pointing to already existent memory in heap or stack somewhere.

thats why it need to pointing reference(Rvalue way, address) memory of variable and initialise pointer.

easy understand address is box and value is inside box
