---
layout: post
title: 8. Day Planner
category: Python
tag: Python
---

A. TAKES A USERNAME AND PASSWORD FROM A USER, GRANT OR DENY ACCESS
B. ONCE ACCESS IS GRANTED, THE SYSTEM REQUESTS TO GET THE TIME OF THE DAY FROM A USER
C. THE SYSTEM THEN PROVIDES THE USER WITH DETAILS ON THE LECTURE TITLE AND LOCATION
D. THE PROGRAM THEN PROVIDES THE STUDENT WITH PREVIOUS LECTURE NOTES, THE NOTES ARE CONTAINED IN THE PROVIDED FILE NAMED "LECTURE NOTES.TXT"

```python
import getpass # this is the function hide the password
username = "Ryan"
password = "ysk%123"
#create a id and passward.
```
```python
# this is the process of to select the day_planner schedule.
def day_planner(time_h_now):
    daily_task    = {10: 'Programming Lecture, Room A',
                     12: 'Computer Vision Lecture, Room C',
                     13: 'Lunch @Cafeteria!',
                     15: 'Group Project Meeting, Room E'}

    for task_time in daily_task.keys(): # let's go to the daily_task.key()
        if time_h_now <= task_time:
            print('You should attend:', daily_task[task_time])
            break
```

```python
input_username = input("Enter Username: ").lower()
# that convert whatever word is lower key
input_password = getpass.getpass('Password:') # this is the fuction hide the password(getpass.getpass)

if username == input_username and input_password == password:
    print("Access granted, Welcome to the system")
    time_h_now = float(input("What's the time now [Use 24h format]? Insert Time between 8AM-3PM [8-15]?"))

    day_planner(time_h_now) #class.

    myfile = open("Lecture_Notes.txt") # open the file
    print(myfile.read()) # to read opend the file ('myfile')

else:
    print("Username or password is not correct, access denied")
```
