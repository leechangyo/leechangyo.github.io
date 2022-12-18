---
layout: post
title: colorized std::cout and definition of inline function and typename and using and typedef and header
category: C++
tag: C++
---

[colored-output-in-c](https://stackoverflow.com/questions/9158150/colored-output-in-c/9158263)

```c++
#ifndef UTILS_COLOR_HPP_
#define UTILS_COLOR_HPP_

#define CLI_NORMAL      "\033[1m\033[0m"
#define CLI_RED         "\033[1m\033[31m"
#define CLI_GREEN       "\033[1m\033[32m"
#define CLI_YELLOW      "\033[1m\033[33m"
#define CLI_BLUE        "\033[1m\033[34m"
#define CLI_MAGENTA     "\033[1m\033[35m"
#define CLI_CYAN        "\033[1m\033[36m"
#define CLI_WHITE       "\033[1m\033[37m"
#define CLI_LIGHT_RED         "\033[1m\033[91m"
#define CLI_LIGHT_GREEN       "\033[1m\033[92m"
#define CLI_LIGHT_YELLOW      "\033[1m\033[93m"
#define CLI_LIGHT_BLUE        "\033[1m\033[94m"
#define CLI_LIGHT_MAGENTA     "\033[1m\033[95m"
#define CLI_LIGHT_CYAN        "\033[1m\033[96m"
#define CLI_LIGHT_WHITE       "\033[1m\033[97m"
#endif  // UTILS_COLOR_HPP_
```

[inline-function-in-cpp](https://www.simplilearn.com/tutorials/cpp-tutorial/inline-function-in-cpp)

[typename](https://www.geeksforgeeks.org/templates-cpp/)

[difference between class and typename](https://stackoverflow.com/questions/2023977/difference-of-keywords-typename-and-class-in-templates)

[difference between typedef and using](https://stackoverflow.com/questions/10747810/what-is-the-difference-between-typedef-and-using-in-c11)


### Haeder print method

```

void PrintHeading1(const std::string& heading) {

std::cout << std::endl << std::string(78, '=') << std::endl;

std::cout << heading << std::endl;

std::cout << std::string(78, '=') << std::endl << std::endl;

}

void PrintHeading2(const std::string& heading) {

std::cout << std::endl << heading << std::endl;

std::cout << std::string(std::min<int>(heading.size(), 78), '-') << std::endl;

}

```
