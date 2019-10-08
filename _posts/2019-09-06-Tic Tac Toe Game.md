---
layout: post
title: 6. Tic Tac Toe game
category: Python
tag: Python
---



```python
board = [0, 1, 2,
         3, 4, 5,
         6, 7, 8]

```
```python
type(board)
# list
board
#[0, 1, 2, 3, 4, 5, 6, 7, 8]
```

```python
def TicTacToe_draw():
    print(board[0], board[1], board[2])
    print(board[3], board[4], board[5])
    print(board[6], board[7], board[8])
    print("good luck")
TicTacToe_draw()
```

```
0 1 2
3 4 5
6 7 8
good luck
```

```python
def Player_1():
    while True:
        print("Player#1, Enter your desired board location: ")
        location = int(input())   
        if board[location] != 'X' and board[location] != 'O':
            board[location] = 'X'
            break;
        else:
            print('This spot is taken, please choose somewhere else')
```

```python

Player_1()
TicTacToe_draw()

```

```
Player#1, Enter your desired board location:
1
0 X 2
3 4 5
6 7 8
good luck
```

```python

def Player_2():
    while True:
        print("Player#2, Enter your desired board location: ")
        location = int(input())
        if board[location] != 'X' and board[location] != 'O':
            board[location] = 'O'
            break;
        else:
            print('This spot is taken, please choose somewhere else')

```

```python

Player_2()
TicTacToe_draw()
```

```
Player#2, Enter your desired board location:
1
This spot is taken, please choose somewhere else
Player#2, Enter your desired board location:
1
This spot is taken, please choose somewhere else
Player#2, Enter your desired board location:
1
This spot is taken, please choose somewhere else
Player#2, Enter your desired board location:
2
0 X O
3 4 5
6 7 8
good luck

```

```python
def Check_Winner(X_or_O):
    if board[0] == X_or_O and board[1] == X_or_O and board[2] == X_or_O:
        return True
    if board[3] == X_or_O and board[4] == X_or_O and board[5] == X_or_O:
        return True
    if board[6] == X_or_O and board[7] == X_or_O and board[8] == X_or_O:
        return True
    if board[0] == X_or_O and board[3] == X_or_O and board[6] == X_or_O:
        return True
    if board[1] == X_or_O and board[4] == X_or_O and board[7] == X_or_O:
        return True
    if board[2] == X_or_O and board[5] == X_or_O and board[8] == X_or_O:
        return True

    # Diagonal terms
    if board[0] == X_or_O and board[4] == X_or_O and board[8] == X_or_O:
        return True
    if board[2] == X_or_O and board[4] == X_or_O and board[6] == X_or_O:
        return True
```

```python
while True:
    Player_1()
    TicTacToe_draw()
    if Check_Winner('X')==True:
        print("Player #1 Wins")
        break;

    Player_2()
    TicTacToe_draw()
    if Check_Winner('O')==True:
        print("Player #2 Wins")
        break;

```
