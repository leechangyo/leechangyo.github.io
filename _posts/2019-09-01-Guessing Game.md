---
layout: post
title: 1. Guessing Game
category: Python
tag: Python
---

```python
import random
```

```python
true_number = random.randint(1, 49) # 1~49까지 랜덤 넘버
```

```python
true_number
```

```python
guess_number = int(input("Enter your guess between 1 and 49: "))

```

```python
guess_number
```

```python
while True:
    if guess_number == true_number:
        print('YOU ARE RIGHT! GOOD JOB!')
        break

    elif guess_number < true_number:
        print('YOUR GUESS IS LOW, TRY AGAIN')
        guess_number = int(input("Enter your guess between 1 and 49: "))

    elif guess_number > true_number:
        print('YOUR GUESS IS HIGH, TRY AGAIN')
        guess_number = int(input("Enter an integer from 1 and 49: "))

```
