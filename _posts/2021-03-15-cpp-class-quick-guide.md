---
layout: post
title: C++ Class Cheatsheet
subtitle: Getting started with Class in C++
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumbnail/1.jpg
share-img: /assets/img/path.jpg
gh-repo: https://github.com/hadomanh/
gh-badge: [star, fork, follow]
tags: [C++, cheatsheet]
---
In this post, we will learn about the syntax of a **Class** in C++ through the **Rational** class building example.

## Prepare:
Our project will include these files:
- ***Rational.h***: header file, contains class definition, which is our data and method declarations
- ***Rational.cpp***: contains the implementations of our class's methods
- ***main.cpp***: driver code, example of Rational class usage
- ***Makefile***: scripting file, includes project structure and all commands to build project. This file is optional.

> **Note**: This is the convention of C/C++. The idea is to keep all function signatures and members in the header file. This will allow other project files to see how the class looks like without having to know the implementation. Instead of separating into many files as above, you can completely write our code into 1 file only. 

## Makefile:
Using **makefile** is convenient for the project development process. It's easy to execute and no need to compile entire program whenever you make a change to a functionality or a class. **Makefile** will automatically compile only those files where change has occurred.

Naming this file as ***makefile*** or ***MAKEFILE*** is acceptable.

```sh
CFLAGS = -c
CC = g++
all: main.o Rational.o
	$(CC) main.o Rational.o -o main
main.o: main.cpp
	$(CC) $(CFLAGS) main.cpp
Rational.o: Rational.cpp
	$(CC) $(CFLAGS) Rational.cpp
clean:
	rm -f main *.o
```


## Rational.h:
It's important to add **#ifndef** or **#pragma once** at beginning of this file. These line are called **include guard**.

{: .box-note}
**Note:** If you use **#ifndef**, you need to add **#endif** at end of file.

This file contain class defination and method signatures. They will be marked as **public** or **private**.

```cpp
#pragma once
#include <iostream>

using namespace std;

class Rational
{
private:
    int numerator;
    int denominator;

    int gcd(int, int);
    void reduce();

public:

	// No-argument constructor
    Rational(): numerator(0), denominator(1) {
        // cout << "No Args Constructor" << endl;
    }

	// All-argument constructor
    Rational(int _numerator, int _denominator): numerator(_numerator), denominator(_denominator) {
        // cout << "All Args Constructor: " << *this << endl;
    }

	// Destructor
    ~Rational() {
        // cout << "Destructor: " << *this << endl;
    }

	// Getters & Setters
    int getNumerator();

    int getDenominator();

    void setNumerator(int numerator);

    void setDenominator(int denominator);

	// Operator overloading
    Rational& operator+(Rational&);

    Rational& operator-(Rational&);

    Rational& operator*(Rational&);

    Rational& operator/(Rational&);

    void operator=(Rational&);

    void operator+=(Rational&);

    void operator-=(Rational&);

    void operator*=(Rational&);

    void operator/=(Rational&);

    bool operator==(Rational&);

    bool operator>=(Rational&);

    bool operator<=(Rational&);

    bool operator>(Rational&);

    bool operator<(Rational&);

    friend ostream& operator<<(ostream&, Rational&);

};

```

## Rational.cpp:
In this file, we will write the method body that has been declared signature in the **Rational.h**.

{: .box-note}
**Note:** ***this*** keyword refers to current class instance varialbe.

{: .box-note}
Remember to add **Rational::** before name of each method. It helps to distinguish methods in this class scope and method in other scopes.

```cpp
#include "Rational.h"

int Rational::gcd(int a, int b)
{
    if (b == 0)
        return a;
    return gcd(b, a % b);
}

void Rational::reduce()
{
    int gcd = this->gcd(numerator, denominator);
    
    if (gcd != 0)
    {
        numerator /= gcd;
        denominator /= gcd;
    }
}

int Rational::getNumerator()
{
    return this->numerator;
}

int Rational::getDenominator()
{
    return this->denominator;
}

void Rational::setNumerator(int numerator)
{
    this->numerator = numerator;
}

void Rational::setDenominator(int denominator)
{
    this->denominator = denominator;
}

Rational &Rational::operator+(Rational &that)
{
    Rational *result = new Rational;
    result->numerator = this->numerator * that.denominator + that.numerator * this->denominator;
    result->denominator = this->denominator * that.denominator;
    result->reduce();
    return *result;
}

Rational &Rational::operator-(Rational &that)
{
    Rational *result = new Rational();
    result->numerator = this->numerator * that.denominator - that.numerator * this->denominator;
    result->denominator = this->denominator * that.denominator;
    result->reduce();
    return *result;
}

Rational &Rational::operator*(Rational &that)
{
    Rational *result = new Rational();
    result->numerator = this->numerator * that.numerator;
    result->denominator = this->denominator * that.denominator;
    result->reduce();
    return *result;
}

Rational &Rational::operator/(Rational &that)
{
    Rational *result = new Rational();
    result->numerator = this->numerator * that.denominator;
    result->denominator = this->denominator * that.numerator;
    result->reduce();
    return *result;
}

void Rational::operator=(Rational &that)
{
    if (this != &that)
    {
        this->numerator = that.numerator;
        this->denominator = that.denominator;
        this->reduce();
    }
}

void Rational::operator+=(Rational &that) {
    *this = *this + that;
}

void Rational::operator-=(Rational &that) {
    *this = *this - that;
}

void Rational::operator*=(Rational &that) {
    *this = *this * that;
}

void Rational::operator/=(Rational &that) {
    *this = *this / that;
}

// this: lhs
// that: rhs

bool Rational::operator==(Rational &that)
{
    return (*this - that).numerator == 0;
}

bool Rational::operator>=(Rational &that)
{
    return (*this - that).numerator >= 0;
}

bool Rational::operator<=(Rational &that)
{
    return (*this - that).numerator <= 0;
}

bool Rational::operator>(Rational &that)
{
    return (*this - that).numerator > 0;
}

bool Rational::operator<(Rational &that)
{
    return (*this - that).numerator < 0;
}

ostream &operator<<(ostream &os, Rational &rationalObj)
{
    rationalObj.reduce();

    if (rationalObj.denominator == 1)
    {
        return os << rationalObj.numerator;
    }

    if (rationalObj.denominator == 0)
    {
        return os << "#div0";
    }

    return os << rationalObj.numerator << '/' << rationalObj.denominator;
}
```