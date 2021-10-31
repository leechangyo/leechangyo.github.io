---
layout: post
title: Jekyll markdown
category: Programming
tag: Programming
---

블로그가 Jekyll로 만들어져있어 게시물을 게시할때 깔끔하게 정리할 수 있는 markdown을 정리해보았다.

# 1. Header & Sub-Header

## - Header

**Syntax**

```
이것은 헤더입니다.
===
```

**Example**

이것은 헤더입니다.
===

## - sub-Header

**Syntax**

```
이것은 부제목입니다.
---
```


**Example**

이것은 부제목입니다.
---

## - H1 ~ H6 Tags

**Syntax**

```
 # H1
 ## H2
 ### H3
 #### H4
 ##### H5
 ###### H6
```

**Example**

# H1
## H2
### H3
#### H4
##### H5
###### H6

# 2. Links

링크를 넣고 싶은 경우는 2가지 방법이 있다.

**Syntax**

```
Link: [참조 키워드][링크변수]
[링크변수]: WebAddress "Descriptions"
```

**Example**

Link : [Google][https://google.com]

**Syntax**

```
Link: [Google](https://google.com)
```

**Example**

Link: [Google](https://google.com)

# 3. BlockQuote

**Syntax**

```
 >이것은 Block Quote
 >이것은 Block Quote
  >이것은 Block Quote
  >이것은 Block Quote
```

**Example**
>이것은 Block Quote
>이것은 Block Quote
  >이것은 Block Quote
  >이것은 Block Quote


# 4. Ordered List

**Syntax**

```
 1. 순서가 있는 목록
 2. 순서가 있는 목록
 3. 순서가 있는 목록
```

**Example**
1. 순서가 있는 목록
2. 순서가 있는 목록
3. 순서가 있는 목록

# 5. Unordered list

**Syntax**

```
 - 순서가 없는 목록
 - 순서가 있는 목록
 - 순서가 있는 목록
```

**Example**
- 순서가 없는 목록
- 순서가 있는 목록
- 순서가 있는 목록

# 6. Code Block

## - General

**Syntax**

```
 <pre> Code block open
 <code> write a code here</code>
 code block close</pre>
```

**Example**
<pre> Code block open
<code> write a code here</code>
code block close</pre>

## - python

**Syntax**
```
  ```python
     # This program adds up integers in the command line
  import sys
  try:
      total = sum(int(arg) for arg in sys.argv[1:])
      print 'sum =', total
  except ValueError:
      print 'Please supply integer arguments'
  ```
```
```

**Example**
```python
   # This program adds up integers in the command line
import sys
try:
    total = sum(int(arg) for arg in sys.argv[1:])
    print 'sum =', total
except ValueError:
    print 'Please supply integer arguments'
```

## Ruby

**Syntax**
```
  ```ruby
  a = [ 45, 3, 19, 8 ]
  b = [ 'sam', 'max', 56, 98.9, 3, 10, 'jill' ]
  print (a + b).join(' '), "\n"
  print a[2], " ", b[4], " ", b[-2], "\n"
  print a.sort.join(' '), "\n"
  a << 57 << 9 << 'phil'
  print "A: ", a.join(' '), "\n"
  ```
```
```

**Example**

```ruby
a = [ 45, 3, 19, 8 ]
b = [ 'sam', 'max', 56, 98.9, 3, 10, 'jill' ]
print (a + b).join(' '), "\n"
print a[2], " ", b[4], " ", b[-2], "\n"
print a.sort.join(' '), "\n"
a << 57 << 9 << 'phil'
print "A: ", a.join(' '), "\n"
```

## C++

**Syntax**
```
  ```c++
  int str_equals(char *equal1, char *eqaul2)
  {
     while(*equal1==*eqaul2)
     {
        if ( *equal1 == '\0' || *eqaul2 == '\0' ){break;}
        equal1++;
        eqaul2++;
     }
     if(*eqaul1 == '\0' && *eqaul2 == '\0' ){return 0;}
     else {return -1};
  }
  ```
```
```

**Example**

```c++
int str_equals(char *equal1, char *eqaul2)
{
   while(*equal1==*eqaul2)
   {
      if ( *equal1 == '\0' || *eqaul2 == '\0' ){break;}
      equal1++;
      eqaul2++;
   }
   if(*eqaul1 == '\0' && *eqaul2 == '\0' ){return 0;}
   else {return -1};
}
```

## C#

**Syntax**
```
  ```c#
  using System;
  using System.Collections.Generic;
  using System.Linq;
  using System.Text;

  namespace Inheritance
  {

      class Program
      {
          static void Main(string[] args)
          {
              Teacher d = new Teacher();
              d.Teach();
              Student s = new Student();
              s.Learn();
              s.Teach();
              Console.ReadKey();
          }

          class Teacher
          {
              public void Teach()
              {
                  Console.WriteLine("Teach");
              }
          }

          class Student : Teacher
          {
              public void Learn()
              {
                  Console.WriteLine("Learn");
              }
          }
      }
  }
  ```
```
```


**Example**

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Inheritance
{

    class Program
    {
        static void Main(string[] args)
        {
            Teacher d = new Teacher();
            d.Teach();
            Student s = new Student();
            s.Learn();
            s.Teach();
            Console.ReadKey();
        }

        class Teacher
        {
            public void Teach()
            {
                Console.WriteLine("Teach");
            }
        }

        class Student : Teacher
        {
            public void Learn()
            {
                Console.WriteLine("Learn");
            }
        }
    }
}
```

## JABA

**Syntax**

```
  ```go
  package main

  import "fmt"

  func main() {
     var greeting =  "Hello world!"

     fmt.Printf("normal string: ")
     fmt.Printf("%s", greeting)
     fmt.Printf("\n")
     fmt.Printf("hex bytes: ")

     for i := 0; i < len(greeting); i++ {
         fmt.Printf("%x ", greeting[i])
     }

     fmt.Printf("\n")
     const sampleText = "\xbd\xb2\x3d\xbc\x20\xe2\x8c\x98"

     /*q flag escapes unprintable characters, with + flag it escapses non-ascii
     characters as well to make output unambigous */
     fmt.Printf("quoted string: ")
     fmt.Printf("%+q", sampleText)
     fmt.Printf("\n")  
  }
  ```
```
```


**Example**

```go
package main

import "fmt"

func main() {
   var greeting =  "Hello world!"

   fmt.Printf("normal string: ")
   fmt.Printf("%s", greeting)
   fmt.Printf("\n")
   fmt.Printf("hex bytes: ")

   for i := 0; i < len(greeting); i++ {
       fmt.Printf("%x ", greeting[i])
   }

   fmt.Printf("\n")
   const sampleText = "\xbd\xb2\x3d\xbc\x20\xe2\x8c\x98"

   /*q flag escapes unprintable characters, with + flag it escapses non-ascii
   characters as well to make output unambigous */
   fmt.Printf("quoted string: ")
   fmt.Printf("%+q", sampleText)
   fmt.Printf("\n")  
}
```

## Swift

**Syntax**

```

  ```swift
  let password = "HelloWorld"
  let repeatPassword = "HelloWorld"
  if ((password.elementsEqual(repeatPassword)) == true)
  {
     print("Passwords are equal")
  } else {
     print("Passwords are not equal")
  }
  ```

```
```

**Example**

```swift
let password = "HelloWorld"
let repeatPassword = "HelloWorld"
if ((password.elementsEqual(repeatPassword)) == true)
{
   print("Passwords are equal")
} else {
   print("Passwords are not equal")
}
```

# 7. Strketthrough(취소선)

**Syntax**
```
  ~~ 이것은 취소선 입니다. ~~
```

**Example**

~~ 이것은 취소선 입니다. ~~

# 8. Bold, Italic

**Syntax**
```
  [Italic]          * 기울임 *
  [Bold]           ** 강조 **
  [Bold & Italic] *** 강조와 기울임 ***
```
**Example**

*기울임*
**강조**
***강조와 기울임***

# 9. Image

**Syntax**
```
  <a href="https://postimg.cc/Kkz1fJFW"><img src="https://i.postimg.cc/9MPyWnkc/Capture.jpg" width="700px" title="source: imgur.com" /><a>
```
**Example**

<a href="https://postimg.cc/Kkz1fJFW"><img src="https://i.postimg.cc/9MPyWnkc/Capture.jpg" width="700px" title="source: imgur.com" /><a>
