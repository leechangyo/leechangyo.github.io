---
layout: post
title: 5. Zodiac Sign
category: Python
tag: Python
---

# Zodiac Sign
<a href="https://postimg.cc/34zVFBfL"><img src="https://i.postimg.cc/vTmyM2rd/324124.png" width="700px" title="source: imgur.com" /><a>
```python
from datetime import date

Year = input("What's your year of birth? [ex: 1992]" )
Month = input("What's your month of birth? [ex: 10]" )
Day = input("What's your day of birth? [ex: 25]" )
```
```python
print('Your Date of Birth is ', (Day + "/" + Month + "/" + Year))
```

```python
today_day = date.today()
today_day
```

```python
age = today_day.year - int(Year)
print('You are', age, 'years old')

```

```python

if ((int(Month)==12 and int(Day) >= 22) or (int(Month)==1 and int(Day)<= 19)):
    sign = ("\n Capricorn")
elif ((int(Month)==1 and int(Day) >= 20)or(int(Month)==2 and int(Day)<= 18)):
    sign = ("\n aquarius")
elif ((int(Month)==2 and int(Day) >= 19)or(int(Month)==3 and int(Day)<= 20)):
    sign = ("\n Pisces")
elif ((int(Month)==3 and int(Day) >= 21)or(int(Month)==4 and int(Day)<= 19)):
    sign = ("\n Aries")
elif ((int(Month)==4 and int(Day) >= 20)or(int(Month)==5 and int(Day)<= 20)):
    sign = ("\n Taurus")
elif ((int(Month)==5 and int(Day) >= 21)or(int(Month)==6 and int(Day)<= 20)):
    sign = ("\n Gemini")
elif ((int(Month)==6 and int(Day) >= 21)or(int(Month)==7 and int(Day)<= 22)):
    sign = ("\n Cancer")
elif ((int(Month)==7 and int(Day) >= 23)or(int(Month)==8 and int(Day)<= 22)):
    sign = ("\n Leo")
elif ((int(Month)==8 and int(Day) >= 23)or(int(Month)==9 and int(Day)<= 22)):
    sign = ("\n Virgo")
elif ((int(Month)==9 and int(Day) >= 23)or(int(Month)==10 and int(Day)<= 22)):
    sign = ("\n Libra")
elif ((int(Month)==10 and int(Day) >= 23)or(int(Month)==11 and int(Day)<= 21)):
    sign = ("\n Scorpio")
elif ((int(Month)==11 and int(Day) >= 22)or(int(Month)==12 and int(Day)<= 21)):
    sign = ("\n Sagittarius")

print(sign)

```
