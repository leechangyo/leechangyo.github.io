---
layout: post
title: 1. Guessing Game
category: Python
tag: Python
---

```Python
import random
```

```Python
true_number = random.randint(1, 49) # 1~49까지 랜덤 넘버
```

```Python
true_number
```

```Python
guess_number = int(input("Enter your guess between 1 and 49: "))

```

```Python
guess_number
```

```Python
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
