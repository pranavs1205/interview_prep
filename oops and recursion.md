# Striver A2Z DSA — Basics, OOP & Recursion
### Interview Prep Book | Python & C++ | 2026 Edition

> **Sources compiled:** Striver A2Z Playlist (TakeUForward), GeeksforGeeks, InterviewBit, TechInterviewHandbook, RealPython, AlgoCademy, DevInterview.io, LeetCode Explore  
> **Depth:** Interview (Deep) | **Language Focus:** Python + C++ (side-by-side)  
> **Split:** ~25% Theory | ~75% Problems + Solutions  
> **Generated:** June 2026

---

## Table of Contents

1. [Quick Reference Card](#1-quick-reference-card)
2. [Why Two Languages?](#2-why-two-languages)
3. [Language Basics — Python & C++ Side-by-Side](#3-language-basics--python--c-side-by-side)
   - 3.1 Hello World, I/O
   - 3.2 Data Types & Variables
   - 3.3 Operators
   - 3.4 Conditionals
   - 3.5 Loops
   - 3.6 Functions
   - 3.7 Standard Library Essentials (STL / Collections)
4. [Pattern Problems — Build Logical Thinking](#4-pattern-problems--build-logical-thinking)
5. [Basic Mathematics Problems](#5-basic-mathematics-problems)
6. [Object-Oriented Programming (OOP)](#6-object-oriented-programming-oop)
   - 6.1 Classes & Objects
   - 6.2 Constructors & Destructors
   - 6.3 Encapsulation & Access Modifiers
   - 6.4 Inheritance
   - 6.5 Polymorphism
   - 6.6 Abstraction
   - 6.7 Key OOP Differences: Python vs C++
7. [Recursion](#7-recursion)
   - 7.1 Anatomy of Recursion
   - 7.2 Recursion Patterns
   - 7.3 Space & Time Complexity
8. [Problems with Solutions](#8-problems-with-solutions)
   - 8.1 Basic Language Problems
   - 8.2 OOP Problems
   - 8.3 Recursion Problems (Easy → Hard)
9. [Interview Preparation — Q&A Bank](#9-interview-preparation--qa-bank)
   - Language Basics Q&A
   - OOP Q&A (20 Questions)
   - Recursion Q&A (15 Questions)
   - Answer Frameworks
   - Depth Gauge (Junior vs Senior)
10. [Common Mistakes & Gotchas](#10-common-mistakes--gotchas)
11. [Source Notes](#11-source-notes)
12. [Glossary](#12-glossary)

---

## 1. Quick Reference Card

**Language Decision for Interviews:**
- Use **Python** if you want faster code-writing speed, cleaner syntax, and you're not targeting systems-level roles.
- Use **C++** if you're targeting HFT, game dev, or systems roles — or if you're already fluent in it.
- **In a 45-minute interview, every saved keystroke is a saved second.** Python typically needs 30–50% fewer lines than C++ for the same algorithm.

**OOP — The Four Pillars:**
| Pillar | One-Line Definition | Key Keyword |
|---|---|---|
| Encapsulation | Bundle data + methods; restrict direct access | `private` / `_` / `__` |
| Abstraction | Expose only essential behavior, hide internals | `abstract` / pure virtual |
| Inheritance | Child class reuses parent class | `:` (C++) / `(Parent)` (Python) |
| Polymorphism | Same interface, different behavior | `virtual` / method override |

**Recursion 3-Step Formula:**
1. **Base case** — When do we stop?
2. **Recursive call** — Make the problem smaller.
3. **Combine** — What do we do with the result?

**Complexity of Common Recursive Problems:**
| Problem | Time | Space |
|---|---|---|
| Factorial(n) | O(n) | O(n) stack |
| Fibonacci(n) | O(2ⁿ) naive, O(n) memo | O(n) |
| Binary Search | O(log n) | O(log n) |
| Tower of Hanoi(n) | O(2ⁿ) | O(n) |
| Permutations of string len n | O(n!) | O(n) |

---

## 2. Why Two Languages?

Striver's playlist originally covered C++ and pseudocode. For interviews in 2025–26, knowing both Python and C++ gives you a decisive edge:

Python excels at rapid prototyping during interviews — its dynamic typing, list comprehensions, built-in collections (`collections.defaultdict`, `heapq`, `Counter`) and readable syntax mean you spend mental energy on the algorithm, not the boilerplate. C++ excels in performance-critical roles — its STL (`vector`, `map`, `priority_queue`), near-hardware-level control, and compile-time type checking matter for systems and competitive programming roles.

> **Interview Advice:** Pick one language and master it. Switching languages mid-preparation is one of the most common mistakes. Once fluent in one, learn the counterpart idioms.

---

## 3. Language Basics — Python & C++ Side-by-Side

### 3.1 Hello World & I/O

```python
# Python
name = input("Enter name: ")
print(f"Hello, {name}!")
```

```cpp
// C++
#include <iostream>
using namespace std;

int main() {
    string name;
    cout << "Enter name: ";
    cin >> name;
    cout << "Hello, " << name << "!" << endl;
    return 0;
}
```

**Key Differences:**
- Python has no `main()` requirement for scripts; C++ requires it.
- Python uses `input()` and `print()`; C++ uses `cin` and `cout`.
- C++ needs `#include` headers; Python imports modules only when needed.
- Python uses `f-strings` for formatting; C++ uses `<<` chaining.

---

### 3.2 Data Types & Variables

**Python — Dynamically Typed**
```python
# No type declaration needed; Python infers at runtime
x = 10          # int
pi = 3.14       # float
name = "Alice"  # str
flag = True     # bool
nums = [1, 2, 3]       # list
coords = (10, 20)       # tuple (immutable)
seen = {1, 2, 3}        # set
mp = {"a": 1, "b": 2}   # dict (hash map)
```

**C++ — Statically Typed**
```cpp
// Type must be declared
int x = 10;
double pi = 3.14;
string name = "Alice";
bool flag = true;
vector<int> nums = {1, 2, 3};
pair<int,int> coords = {10, 20};
set<int> seen = {1, 2, 3};
map<string,int> mp = {{"a", 1}, {"b", 2}};
```

**Type Ranges (important for overflow problems):**
| Type | C++ | Python equivalent |
|---|---|---|
| 32-bit int | `int` (-2B to 2B) | `int` (unlimited) |
| 64-bit int | `long long` | `int` (unlimited) |
| Float | `double` | `float` |
| Boolean | `bool` | `bool` |

> **Interview Gotcha:** In C++, `int` overflow is a silent bug. Use `long long` for large sums. Python's `int` is arbitrary precision — it never overflows.

---

### 3.3 Operators

Both languages share `+`, `-`, `*`, `/`, `%`, `==`, `!=`, `<`, `>`, `<=`, `>=`, `&&`/`and`, `||`/`or`, `!`/`not`.

**Differences to know:**
```python
# Python
print(7 // 2)   # Integer division → 3
print(7 ** 3)   # Exponent → 343
print(-7 // 2)  # Floor division → -4 (watch this!)
```

```cpp
// C++
cout << 7 / 2;   // Integer division → 3
cout << pow(7,3); // Exponent → 343.0 (returns double)
cout << -7 / 2;  // Truncates toward zero → -3 (different from Python!)
```

> **Interview Gotcha:** `-7 // 2` is `-4` in Python (floor division) but `-7 / 2` is `-3` in C++ (truncation toward zero). This trips candidates up in binary search and modular arithmetic problems.

---

### 3.4 Conditionals

```python
# Python
x = 10
if x > 0:
    print("positive")
elif x == 0:
    print("zero")
else:
    print("negative")

# Ternary
result = "even" if x % 2 == 0 else "odd"
```

```cpp
// C++
int x = 10;
if (x > 0) {
    cout << "positive";
} else if (x == 0) {
    cout << "zero";
} else {
    cout << "negative";
}

// Ternary
string result = (x % 2 == 0) ? "even" : "odd";
```

---

### 3.5 Loops

```python
# Python — for loop
for i in range(5):        # 0, 1, 2, 3, 4
    print(i)

for i in range(2, 10, 2): # 2, 4, 6, 8
    print(i)

# Iterate a list
nums = [10, 20, 30]
for val in nums:
    print(val)

# Enumerate (index + value)
for i, val in enumerate(nums):
    print(i, val)

# While loop
i = 0
while i < 5:
    i += 1
```

```cpp
// C++ — for loop
for (int i = 0; i < 5; i++) {
    cout << i << "\n";
}

// Range-based for (C++11)
vector<int> nums = {10, 20, 30};
for (int val : nums) {
    cout << val << "\n";
}

// While loop
int i = 0;
while (i < 5) {
    i++;
}
```

---

### 3.6 Functions

```python
# Python
def add(a, b):
    return a + b

# Default arguments
def greet(name="World"):
    print(f"Hello, {name}!")

# Multiple return values (via tuple)
def min_max(lst):
    return min(lst), max(lst)

mn, mx = min_max([3, 1, 4, 1, 5])

# Lambda (anonymous function)
square = lambda x: x * x
```

```cpp
// C++
int add(int a, int b) {
    return a + b;
}

// Default arguments
void greet(string name = "World") {
    cout << "Hello, " << name << "!\n";
}

// Multiple return values via pair/struct
pair<int,int> minMax(vector<int>& v) {
    return {*min_element(v.begin(), v.end()),
            *max_element(v.begin(), v.end())};
}

// Lambda (C++11)
auto square = [](int x) { return x * x; };
```

---

### 3.7 Standard Library Essentials

**Python `collections` + builtins — your interview toolkit:**
```python
from collections import defaultdict, Counter, deque
import heapq

# Counter — frequency map in one line
freq = Counter([1, 2, 2, 3, 3, 3])  # {3: 3, 2: 2, 1: 1}

# defaultdict — no KeyError on missing keys
graph = defaultdict(list)
graph[1].append(2)

# deque — O(1) append/pop from both ends
dq = deque([1, 2, 3])
dq.appendleft(0)
dq.popleft()

# heapq — min-heap
heap = [3, 1, 4, 1, 5]
heapq.heapify(heap)
heapq.heappush(heap, 2)
smallest = heapq.heappop(heap)

# sort with key
nums = [(2, 'b'), (1, 'a'), (3, 'c')]
nums.sort(key=lambda x: x[0])
```

**C++ STL — your interview toolkit:**
```cpp
#include <vector>
#include <map>
#include <unordered_map>
#include <set>
#include <queue>
#include <stack>
#include <algorithm>
using namespace std;

// unordered_map — O(1) avg hash map
unordered_map<int,int> freq;
freq[3]++;

// priority_queue — max-heap by default
priority_queue<int> maxHeap;
maxHeap.push(3);
maxHeap.top(); // peek

// min-heap
priority_queue<int, vector<int>, greater<int>> minHeap;

// sort
vector<int> v = {3,1,4,1,5};
sort(v.begin(), v.end());                     // ascending
sort(v.begin(), v.end(), greater<int>());     // descending

// binary search
lower_bound(v.begin(), v.end(), 3); // first >= 3
upper_bound(v.begin(), v.end(), 3); // first > 3
```

---

## 4. Pattern Problems — Build Logical Thinking

Pattern problems teach you loop control and indexed thinking — skills that directly transfer to matrix traversal and grid problems in interviews.

### Problem 4.1 — Right Triangle of Stars
Print:
```
*
* *
* * *
* * * *
* * * * *
```

**Python:**
```python
def right_triangle(n):
    for i in range(1, n + 1):
        print("* " * i)

right_triangle(5)
```

**C++:**
```cpp
void rightTriangle(int n) {
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= i; j++)
            cout << "* ";
        cout << "\n";
    }
}
```

---

### Problem 4.2 — Inverted Right Triangle
Print:
```
* * * * *
* * * *
* * *
* *
*
```

**Python:**
```python
def inverted_triangle(n):
    for i in range(n, 0, -1):
        print("* " * i)
```

**C++:**
```cpp
void invertedTriangle(int n) {
    for (int i = n; i >= 1; i--) {
        for (int j = 1; j <= i; j++)
            cout << "* ";
        cout << "\n";
    }
}
```

---

### Problem 4.3 — Diamond Pattern
```
    *
   ***
  *****
 *******
*********
 *******
  *****
   ***
    *
```

**Python:**
```python
def diamond(n):
    # Upper half
    for i in range(1, n + 1):
        print(" " * (n - i) + "*" * (2 * i - 1))
    # Lower half
    for i in range(n - 1, 0, -1):
        print(" " * (n - i) + "*" * (2 * i - 1))
```

**C++:**
```cpp
void diamond(int n) {
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < n - i; j++) cout << " ";
        for (int j = 0; j < 2*i-1; j++) cout << "*";
        cout << "\n";
    }
    for (int i = n-1; i >= 1; i--) {
        for (int j = 0; j < n - i; j++) cout << " ";
        for (int j = 0; j < 2*i-1; j++) cout << "*";
        cout << "\n";
    }
}
```

---

## 5. Basic Mathematics Problems

### Problem 5.1 — Count Digits in a Number

```python
def count_digits(n):
    n = abs(n)
    if n == 0:
        return 1
    count = 0
    while n > 0:
        n //= 10
        count += 1
    return count
```

```cpp
int countDigits(int n) {
    if (n == 0) return 1;
    n = abs(n);
    int count = 0;
    while (n > 0) {
        n /= 10;
        count++;
    }
    return count;
}
```

**Time:** O(log₁₀ n) | **Space:** O(1)

---

### Problem 5.2 — Reverse a Number

```python
def reverse_number(n):
    sign = -1 if n < 0 else 1
    n = abs(n)
    rev = 0
    while n > 0:
        rev = rev * 10 + n % 10
        n //= 10
    return sign * rev
```

```cpp
int reverseNumber(int n) {
    int sign = (n < 0) ? -1 : 1;
    n = abs(n);
    long long rev = 0;  // Use long long to avoid overflow
    while (n > 0) {
        rev = rev * 10 + n % 10;
        n /= 10;
    }
    return sign * (int)rev;
}
```

---

### Problem 5.3 — Check Armstrong Number
A number is Armstrong if the sum of its digits each raised to the power of the number of digits equals the number itself. For example, 153 = 1³ + 5³ + 3³.

```python
def is_armstrong(n):
    digits = str(n)
    power = len(digits)
    total = sum(int(d) ** power for d in digits)
    return total == n
```

```cpp
bool isArmstrong(int n) {
    int original = n, digits = 0, temp = n;
    while (temp > 0) { temp /= 10; digits++; }
    int sum = 0;
    temp = n;
    while (temp > 0) {
        int d = temp % 10;
        sum += pow(d, digits);
        temp /= 10;
    }
    return sum == original;
}
```

---

### Problem 5.4 — GCD / HCF (Euclidean Algorithm)

The key insight: `gcd(a, b) = gcd(b, a % b)`. Base case: `gcd(a, 0) = a`.

```python
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a
# Or recursively:
def gcd_rec(a, b):
    return a if b == 0 else gcd_rec(b, a % b)
```

```cpp
int gcd(int a, int b) {
    while (b) {
        a = a % b;
        swap(a, b);
    }
    return a;
}
// Recursive:
int gcdRec(int a, int b) {
    return b == 0 ? a : gcdRec(b, a % b);
}
```

**Time:** O(log(min(a, b))) | This is a classic recursion problem too.

---

### Problem 5.5 — Check Prime Number

```python
def is_prime(n):
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    for i in range(3, int(n**0.5) + 1, 2):
        if n % i == 0:
            return False
    return True
```

```cpp
bool isPrime(int n) {
    if (n < 2) return false;
    if (n == 2) return true;
    if (n % 2 == 0) return false;
    for (int i = 3; i * i <= n; i += 2) {
        if (n % i == 0) return false;
    }
    return true;
}
```

> **Why sqrt(n)?** Any composite number n must have a factor ≤ √n. If we find none by √n, the number is prime. This reduces O(n) to O(√n).

---

## 6. Object-Oriented Programming (OOP)

OOP is the most heavily tested concept in Indian product company interviews (Amazon, Flipkart, Swiggy, Zomato, etc.) outside of DSA. Expect 2–5 direct OOP questions in any SDE interview.

### 6.1 Classes & Objects

A **class** is a blueprint. An **object** is a specific instance built from that blueprint.

**Analogy:** A class is like the architectural plan of a house; an object is the actual house built from it. You can build hundreds of houses from one plan — each has its own rooms (data), but they're all structurally the same type.

**Python:**
```python
class Dog:
    # Class variable — shared by ALL instances
    species = "Canis lupus familiaris"

    def __init__(self, name, age):
        # Instance variables — unique per object
        self.name = name
        self.age = age

    def bark(self):
        return f"{self.name} says: Woof!"

    def __str__(self):  # String representation
        return f"Dog({self.name}, {self.age})"

# Creating objects
d1 = Dog("Buddy", 3)
d2 = Dog("Max", 5)
print(d1.bark())           # Buddy says: Woof!
print(Dog.species)         # Canis lupus familiaris
```

**C++:**
```cpp
#include <iostream>
#include <string>
using namespace std;

class Dog {
public:
    static string species;  // Class variable
    string name;
    int age;

    // Constructor
    Dog(string n, int a) : name(n), age(a) {}

    string bark() {
        return name + " says: Woof!";
    }
};

string Dog::species = "Canis lupus familiaris";

int main() {
    Dog d1("Buddy", 3);
    Dog d2("Max", 5);
    cout << d1.bark() << "\n";
    cout << Dog::species << "\n";
    return 0;
}
```

---

### 6.2 Constructors & Destructors

A **constructor** initializes an object when it's created. A **destructor** cleans up when the object is destroyed.

**Python:**
```python
class MyClass:
    def __init__(self, val):       # Constructor
        self.val = val
        print(f"Created with val={val}")

    def __del__(self):             # Destructor
        print(f"Destroyed object with val={self.val}")

obj = MyClass(42)
# Destructor called when obj goes out of scope or is explicitly deleted
```

**C++:**
```cpp
class MyClass {
public:
    int val;

    MyClass(int v) : val(v) {     // Constructor
        cout << "Created: " << val << "\n";
    }

    MyClass(const MyClass& other) : val(other.val) {  // Copy constructor
        cout << "Copy created\n";
    }

    ~MyClass() {                  // Destructor
        cout << "Destroyed: " << val << "\n";
    }
};
```

> **Interview Gotcha — C++ Rule of Three:** If you define a custom destructor, you almost certainly need to also define a custom copy constructor and copy assignment operator. This prevents shallow copy bugs with pointer members.

**Types of Constructors in C++:**
- **Default constructor** — no parameters: `MyClass()`
- **Parameterized constructor** — takes arguments
- **Copy constructor** — `MyClass(const MyClass& obj)`
- **Move constructor (C++11)** — `MyClass(MyClass&& obj)`

Python's `__init__` can simulate all of these via default arguments and `@classmethod` factory methods.

---

### 6.3 Encapsulation & Access Modifiers

Encapsulation bundles data and methods into a class and restricts external access to internal state.

**Python — convention-based:**
```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner          # Public
        self._account_num = "XXX"   # Protected (convention: single underscore)
        self.__balance = balance    # Private (name-mangled to _BankAccount__balance)

    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount

    def get_balance(self):          # Getter
        return self.__balance

    def set_balance(self, amount):  # Setter with validation
        if amount >= 0:
            self.__balance = amount

acc = BankAccount("Alice", 1000)
acc.deposit(500)
print(acc.get_balance())  # 1500
# print(acc.__balance)    # AttributeError!
```

**C++ — enforced by compiler:**
```cpp
class BankAccount {
private:
    double balance;
    string accountNum;

protected:
    string owner;  // Accessible to subclasses

public:
    BankAccount(string own, double bal) : owner(own), balance(bal) {}

    void deposit(double amount) {
        if (amount > 0) balance += amount;
    }

    double getBalance() const { return balance; }  // Getter

    void setBalance(double amount) {               // Setter
        if (amount >= 0) balance = amount;
    }
};
```

> **Key Interview Point:** Python doesn't truly enforce private members — `__balance` becomes `_BankAccount__balance` (name mangling). You CAN access it via `acc._BankAccount__balance`. C++ enforces access at compile time.

---

### 6.4 Inheritance

Inheritance lets a child class acquire properties and behaviors from a parent class, promoting code reuse.

**Python:**
```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        raise NotImplementedError("Subclass must implement speak()")

    def describe(self):
        return f"I am {self.name}"

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

# Multiple Inheritance
class Robot:
    def move(self):
        return "Moving..."

class RoboDog(Dog, Robot):  # Inherits from both Dog and Robot
    pass

d = Dog("Buddy")
print(d.describe())  # I am Buddy (inherited from Animal)
print(d.speak())     # Woof! (overridden in Dog)
```

**C++:**
```cpp
class Animal {
protected:
    string name;
public:
    Animal(string n) : name(n) {}
    virtual string speak() = 0;  // Pure virtual — must be overridden
    string describe() { return "I am " + name; }
};

class Dog : public Animal {
public:
    Dog(string n) : Animal(n) {}
    string speak() override { return "Woof!"; }
};

class Cat : public Animal {
public:
    Cat(string n) : Animal(n) {}
    string speak() override { return "Meow!"; }
};

// Multiple Inheritance
class Robot {
public:
    string move() { return "Moving..."; }
};

class RoboDog : public Dog, public Robot {
public:
    RoboDog(string n) : Dog(n), Robot() {}
};
```

**Types of Inheritance:**
| Type | Description | Python Support | C++ Support |
|---|---|---|---|
| Single | One parent, one child | ✅ | ✅ |
| Multilevel | A → B → C | ✅ | ✅ |
| Multiple | A and B → C | ✅ (MRO) | ✅ (virtual) |
| Hierarchical | One parent, many children | ✅ | ✅ |
| Hybrid | Combination of above | ✅ (MRO) | ✅ (virtual) |

> **Diamond Problem:** When two parent classes share a grandparent, the child may inherit duplicate copies. Python solves it via MRO (Method Resolution Order, C3 linearization). C++ requires `virtual` inheritance to avoid ambiguity.

---

### 6.5 Polymorphism

Polymorphism means "many forms" — the same method name behaves differently depending on the object it's called on.

**Two types:**
- **Compile-time (static):** Method/operator overloading — resolved at compile time.
- **Runtime (dynamic):** Method overriding with virtual functions — resolved at runtime.

**Python — Runtime Polymorphism (duck typing):**
```python
class Shape:
    def area(self):
        raise NotImplementedError

class Circle(Shape):
    def __init__(self, r):
        self.r = r
    def area(self):
        return 3.14159 * self.r ** 2

class Rectangle(Shape):
    def __init__(self, w, h):
        self.w, self.h = w, h
    def area(self):
        return self.w * self.h

# Polymorphic usage — same interface, different behavior
shapes = [Circle(5), Rectangle(4, 6)]
for s in shapes:
    print(s.area())   # 78.54, then 24
```

**C++ — Virtual Functions (Runtime Polymorphism):**
```cpp
class Shape {
public:
    virtual double area() = 0;  // Pure virtual
    virtual ~Shape() {}         // Virtual destructor — CRITICAL
};

class Circle : public Shape {
    double r;
public:
    Circle(double r) : r(r) {}
    double area() override { return 3.14159 * r * r; }
};

class Rectangle : public Shape {
    double w, h;
public:
    Rectangle(double w, double h) : w(w), h(h) {}
    double area() override { return w * h; }
};

int main() {
    Shape* shapes[] = { new Circle(5), new Rectangle(4, 6) };
    for (auto s : shapes) cout << s->area() << "\n";
    // Cleanup
    for (auto s : shapes) delete s;
}
```

**C++ — Compile-time Polymorphism (Function Overloading):**
```cpp
class Calculator {
public:
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
    int add(int a, int b, int c) { return a + b + c; }
};
```

> **Python does NOT support function overloading** in the traditional sense — it's dynamically typed, so all methods are polymorphic by default. You can simulate it with default arguments or `*args`.

> **Virtual Destructor Rule:** In C++, always make the destructor `virtual` in a base class that uses virtual functions. Without it, deleting a derived object through a base pointer causes undefined behavior.

---

### 6.6 Abstraction

Abstraction hides implementation complexity and shows only essential features. It's implemented via abstract classes and interfaces.

**Python — using `abc` module:**
```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    @abstractmethod
    def start_engine(self):
        pass

    @abstractmethod
    def stop_engine(self):
        pass

    def honk(self):  # Non-abstract — has default implementation
        print("Beep!")

class Car(Vehicle):
    def start_engine(self):
        print("Car engine started: Vroom!")
    def stop_engine(self):
        print("Car engine stopped.")

class Bicycle(Vehicle):
    def start_engine(self):
        print("Bicycle: no engine to start.")
    def stop_engine(self):
        pass

# v = Vehicle()  # TypeError: Can't instantiate abstract class
car = Car()
car.start_engine()
car.honk()
```

**C++ — using pure virtual functions:**
```cpp
class Vehicle {
public:
    virtual void startEngine() = 0;   // Pure virtual = abstract method
    virtual void stopEngine() = 0;
    void honk() { cout << "Beep!\n"; } // Has implementation
    virtual ~Vehicle() {}
};

class Car : public Vehicle {
public:
    void startEngine() override { cout << "Vroom!\n"; }
    void stopEngine() override { cout << "Car stopped.\n"; }
};
```

**Abstraction vs Encapsulation:**
- **Encapsulation** = hiding *data* (how data is stored, who can modify it)
- **Abstraction** = hiding *complexity* (how a method works internally)

**Analogy:** An ATM is both. Encapsulation — your PIN and balance are hidden from other users. Abstraction — you press "Withdraw" without knowing the banking API calls happening behind the scenes.

---

### 6.7 Key OOP Differences: Python vs C++

| Feature | Python | C++ |
|---|---|---|
| Access modifiers | Convention (`_`, `__`) | Enforced (`public`, `private`, `protected`) |
| Multiple inheritance | ✅ MRO (C3) | ✅ Virtual inheritance needed for diamond |
| Abstract class | `abc.ABC` + `@abstractmethod` | Pure virtual functions (`= 0`) |
| Function overloading | Not supported natively | ✅ Native |
| Operator overloading | `__add__`, `__eq__`, etc. | `operator+`, `operator==`, etc. |
| Constructor | `__init__` (one per class) | Multiple constructors allowed |
| Destructor | `__del__` (GC managed) | `~ClassName()` (deterministic) |
| `this` / `self` | Explicit `self` parameter | Implicit `this` pointer |
| Static methods | `@staticmethod` | `static` keyword |
| Class methods | `@classmethod` | `static` (with access to class) |

---

## 7. Recursion

### 7.1 Anatomy of Recursion

Recursion is a function solving a problem by calling itself with a simpler version of the same problem.

Every recursive function has exactly two parts:
1. **Base case** — the termination condition. Without this, you get infinite recursion and a stack overflow.
2. **Recursive case** — call the function with a strictly smaller/simpler input.

**Analogy:** Imagine you're in a queue and want to know your position. You ask the person in front: "What's your position?" They ask the person in front of them, and so on. The first person (base case) says "I'm #1." Then the answer propagates back: "I'm #2", "I'm #3", etc.

**Template:**
```python
def solve(problem):
    # Base case
    if is_simple(problem):
        return direct_solution(problem)
    # Recursive case
    smaller = make_smaller(problem)
    sub_answer = solve(smaller)
    return combine(sub_answer, problem)
```

---

### 7.2 Recursion Patterns

**Pattern 1 — Parameterized Recursion (accumulate into parameter):**
Use when you want to carry a running total or state through the recursive calls.

```python
def sum_to_n(n, current_sum=0):
    if n == 0:
        return current_sum
    return sum_to_n(n - 1, current_sum + n)
```

**Pattern 2 — Functional Recursion (build result from return value):**
Use when you compute the answer from sub-problems' return values.

```python
def sum_to_n(n):
    if n == 0:
        return 0
    return n + sum_to_n(n - 1)
```

**Pattern 3 — Multiple recursive calls (tree recursion):**
Used for problems like Fibonacci, tree traversals, subsets.

```python
def fib(n):
    if n <= 1:
        return n
    return fib(n - 1) + fib(n - 2)
# Warning: O(2^n) without memoization!
```

**Pattern 4 — Backtracking (try, recurse, undo):**
Used for subsets, permutations, N-Queens.

```python
def subsets(nums, idx=0, current=[]):
    if idx == len(nums):
        print(current)
        return
    # Include nums[idx]
    current.append(nums[idx])
    subsets(nums, idx + 1, current)
    # Exclude nums[idx] (backtrack)
    current.pop()
    subsets(nums, idx + 1, current)
```

---

### 7.3 Space & Time Complexity

Every function call occupies a stack frame. The maximum stack depth equals the maximum recursion depth.

For a simple linear recursion like `factorial(n)`:
- **Time:** O(n) — n calls made
- **Space:** O(n) — n stack frames held simultaneously

For tree recursion like naive `fib(n)`:
- **Time:** O(2ⁿ) — exponential calls
- **Space:** O(n) — max depth of the call tree

For divide and conquer like merge sort:
- **Time:** O(n log n)
- **Space:** O(log n) for recursion stack, O(n) for merge buffer

> **Tail Recursion:** A recursive call is "tail-recursive" if the recursive call is the last thing the function does (no pending computation after the call). Some compilers optimize tail recursion to avoid stack growth. Python does NOT optimize tail recursion.

---

## 8. Problems with Solutions

### 8.1 Basic Language Problems

#### Problem — Find Second Largest Element
```python
def second_largest(arr):
    if len(arr) < 2:
        return -1
    first = second = float('-inf')
    for x in arr:
        if x > first:
            second = first
            first = x
        elif x > second and x != first:
            second = x
    return second if second != float('-inf') else -1

print(second_largest([5, 2, 8, 1, 8]))  # 5
```

```cpp
int secondLargest(vector<int>& arr) {
    int first = INT_MIN, second = INT_MIN;
    for (int x : arr) {
        if (x > first) {
            second = first;
            first = x;
        } else if (x > second && x != first) {
            second = x;
        }
    }
    return (second == INT_MIN) ? -1 : second;
}
```

**Time:** O(n) | **Space:** O(1) — single pass, no sorting needed.

---

#### Problem — Check Palindrome (Number or String)

```python
def is_palindrome(s):
    s = str(s)
    return s == s[::-1]

# Two-pointer approach (preferred to explain in interview):
def is_palindrome_two_ptr(s):
    l, r = 0, len(s) - 1
    while l < r:
        if s[l] != s[r]:
            return False
        l += 1
        r -= 1
    return True
```

```cpp
bool isPalindrome(string s) {
    int l = 0, r = s.size() - 1;
    while (l < r) {
        if (s[l++] != s[r--]) return false;
    }
    return true;
}
```

---

### 8.2 OOP Problems

#### Problem — Design a Stack class using OOP

```python
class Stack:
    def __init__(self):
        self.__data = []  # Private

    def push(self, val):
        self.__data.append(val)

    def pop(self):
        if self.is_empty():
            raise IndexError("Stack underflow")
        return self.__data.pop()

    def peek(self):
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.__data[-1]

    def is_empty(self):
        return len(self.__data) == 0

    def size(self):
        return len(self.__data)

    def __str__(self):
        return str(self.__data)

s = Stack()
s.push(1)
s.push(2)
print(s.peek())  # 2
print(s.pop())   # 2
print(s.size())  # 1
```

```cpp
class Stack {
private:
    vector<int> data;

public:
    void push(int val) { data.push_back(val); }

    int pop() {
        if (isEmpty()) throw runtime_error("Stack underflow");
        int top = data.back();
        data.pop_back();
        return top;
    }

    int peek() {
        if (isEmpty()) throw runtime_error("Stack is empty");
        return data.back();
    }

    bool isEmpty() { return data.empty(); }
    int size() { return data.size(); }
};
```

---

#### Problem — Shape Hierarchy (Polymorphism Demo)

```python
from abc import ABC, abstractmethod
import math

class Shape(ABC):
    @abstractmethod
    def area(self) -> float:
        pass

    @abstractmethod
    def perimeter(self) -> float:
        pass

    def describe(self):
        return f"{type(self).__name__}: area={self.area():.2f}, perimeter={self.perimeter():.2f}"

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return math.pi * self.radius ** 2

    def perimeter(self):
        return 2 * math.pi * self.radius

class Rectangle(Shape):
    def __init__(self, w, h):
        self.w, self.h = w, h

    def area(self):
        return self.w * self.h

    def perimeter(self):
        return 2 * (self.w + self.h)

class Triangle(Shape):
    def __init__(self, a, b, c):
        self.a, self.b, self.c = a, b, c

    def area(self):
        s = (self.a + self.b + self.c) / 2
        return math.sqrt(s * (s-self.a) * (s-self.b) * (s-self.c))

    def perimeter(self):
        return self.a + self.b + self.c

shapes = [Circle(5), Rectangle(4, 6), Triangle(3, 4, 5)]
for s in shapes:
    print(s.describe())
```

---

### 8.3 Recursion Problems (Easy → Hard)

#### Easy — Factorial

```python
def factorial(n):
    if n <= 1:      # Base case
        return 1
    return n * factorial(n - 1)  # Recursive case

# Trace for factorial(4):
# factorial(4) = 4 * factorial(3)
#              = 4 * 3 * factorial(2)
#              = 4 * 3 * 2 * factorial(1)
#              = 4 * 3 * 2 * 1 = 24
```

```cpp
long long factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

**Time:** O(n) | **Space:** O(n)

---

#### Easy — Sum of N Natural Numbers

```python
def sum_n(n):
    if n == 0:
        return 0
    return n + sum_n(n - 1)
```

```cpp
int sumN(int n) {
    if (n == 0) return 0;
    return n + sumN(n - 1);
}
```

---

#### Easy — Print 1 to N (without loop)

```python
def print_1_to_n(i, n):
    if i > n:
        return
    print(i)
    print_1_to_n(i + 1, n)

print_1_to_n(1, 5)
```

```cpp
void print1toN(int i, int n) {
    if (i > n) return;
    cout << i << "\n";
    print1toN(i + 1, n);
}
```

---

#### Easy — Print N to 1 (Reverse) using Recursion

```python
def print_n_to_1(n):
    if n < 1:
        return
    print(n)
    print_n_to_1(n - 1)
```

---

#### Easy — Fibonacci Number (LeetCode 509)

```python
# Naive — O(2^n) time, TLE for large n
def fib_naive(n):
    if n <= 1:
        return n
    return fib_naive(n-1) + fib_naive(n-2)

# Memoized — O(n) time, O(n) space
def fib_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    return memo[n]
```

```cpp
// Memoized
unordered_map<int,int> memo;
int fib(int n) {
    if (n <= 1) return n;
    if (memo.count(n)) return memo[n];
    return memo[n] = fib(n-1) + fib(n-2);
}
```

---

#### Medium — Power Function: x^n (LeetCode 50 — Pow(x, n))

The key insight is **fast exponentiation** — rather than multiplying x by itself n times (O(n)), use:
- If n is even: `x^n = (x²)^(n/2)`
- If n is odd: `x^n = x * x^(n-1)`

This gives O(log n) time.

```python
def my_pow(x, n):
    if n == 0:
        return 1
    if n < 0:
        return my_pow(1 / x, -n)
    if n % 2 == 0:
        return my_pow(x * x, n // 2)
    else:
        return x * my_pow(x, n - 1)
```

```cpp
double myPow(double x, int n) {
    if (n == 0) return 1;
    if (n < 0) return myPow(1.0 / x, -(long long)n);  // Handle INT_MIN
    if (n % 2 == 0) return myPow(x * x, n / 2);
    return x * myPow(x, n - 1);
}
```

**Time:** O(log n) | **Space:** O(log n) stack

---

#### Medium — Check if String is Palindrome (Recursive)

```python
def is_palindrome_rec(s, l, r):
    if l >= r:
        return True
    if s[l] != s[r]:
        return False
    return is_palindrome_rec(s, l + 1, r - 1)

print(is_palindrome_rec("racecar", 0, 6))  # True
```

```cpp
bool isPalindromeRec(string& s, int l, int r) {
    if (l >= r) return true;
    if (s[l] != s[r]) return false;
    return isPalindromeRec(s, l + 1, r - 1);
}
```

---

#### Medium — Tower of Hanoi

Move n disks from source → destination using auxiliary rod. Rules: (1) only one disk at a time, (2) no disk on smaller disk.

The insight: move top n-1 from source to aux, move nth disk to destination, move n-1 from aux to destination.

```python
def tower_of_hanoi(n, source, destination, auxiliary):
    if n == 1:
        print(f"Move disk 1 from {source} to {destination}")
        return
    tower_of_hanoi(n - 1, source, auxiliary, destination)
    print(f"Move disk {n} from {source} to {destination}")
    tower_of_hanoi(n - 1, auxiliary, destination, source)

tower_of_hanoi(3, 'A', 'C', 'B')
# Minimum moves for n disks = 2^n - 1
```

```cpp
void towerOfHanoi(int n, char src, char dst, char aux) {
    if (n == 1) {
        cout << "Move disk 1 from " << src << " to " << dst << "\n";
        return;
    }
    towerOfHanoi(n - 1, src, aux, dst);
    cout << "Move disk " << n << " from " << src << " to " << dst << "\n";
    towerOfHanoi(n - 1, aux, dst, src);
}
```

**Time:** O(2ⁿ) | **Space:** O(n) — total moves are exactly 2ⁿ - 1.

---

#### Medium — All Subsequences / Subsets of an Array (LeetCode 78)

A subsequence either includes or excludes each element. With n elements, there are 2ⁿ subsequences.

```python
def subsets(nums):
    result = []

    def backtrack(idx, current):
        result.append(list(current))   # Every state is a valid subset
        for i in range(idx, len(nums)):
            current.append(nums[i])
            backtrack(i + 1, current)
            current.pop()              # Undo

    backtrack(0, [])
    return result

print(subsets([1, 2, 3]))
# [[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]
```

```cpp
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> current;

    function<void(int)> backtrack = [&](int idx) {
        result.push_back(current);
        for (int i = idx; i < nums.size(); i++) {
            current.push_back(nums[i]);
            backtrack(i + 1);
            current.pop_back();
        }
    };

    backtrack(0);
    return result;
}
```

**Time:** O(n × 2ⁿ) | **Space:** O(n) recursion stack

---

#### Medium — Generate All Permutations (LeetCode 46)

```python
def permutations(nums):
    result = []

    def backtrack(path, remaining):
        if not remaining:
            result.append(path[:])
            return
        for i in range(len(remaining)):
            path.append(remaining[i])
            backtrack(path, remaining[:i] + remaining[i+1:])
            path.pop()

    backtrack([], nums)
    return result

# Alternate: swap-based (avoids creating new lists)
def permutations_swap(nums, l=0):
    if l == len(nums):
        print(nums[:])
        return
    for i in range(l, len(nums)):
        nums[l], nums[i] = nums[i], nums[l]
        permutations_swap(nums, l + 1)
        nums[l], nums[i] = nums[i], nums[l]  # Backtrack
```

```cpp
void permute(vector<int>& nums, int l, vector<vector<int>>& result) {
    if (l == nums.size()) {
        result.push_back(nums);
        return;
    }
    for (int i = l; i < nums.size(); i++) {
        swap(nums[l], nums[i]);
        permute(nums, l + 1, result);
        swap(nums[l], nums[i]);  // Backtrack
    }
}
```

**Time:** O(n! × n) | **Space:** O(n) recursion depth

---

#### Hard — Combination Sum (LeetCode 39)

Find all combinations from `candidates` that sum to `target`. Each number can be reused.

```python
def combination_sum(candidates, target):
    result = []
    candidates.sort()

    def backtrack(idx, current, remaining):
        if remaining == 0:
            result.append(list(current))
            return
        for i in range(idx, len(candidates)):
            if candidates[i] > remaining:
                break  # Prune: sorted array, no point continuing
            current.append(candidates[i])
            backtrack(i, current, remaining - candidates[i])  # i (not i+1) for reuse
            current.pop()

    backtrack(0, [], target)
    return result
```

```cpp
void backtrack(vector<int>& candidates, int target, int idx,
               vector<int>& current, vector<vector<int>>& result) {
    if (target == 0) { result.push_back(current); return; }
    for (int i = idx; i < candidates.size(); i++) {
        if (candidates[i] > target) break;
        current.push_back(candidates[i]);
        backtrack(candidates, target - candidates[i], i, current, result);
        current.pop_back();
    }
}

vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    sort(candidates.begin(), candidates.end());
    vector<vector<int>> result;
    vector<int> current;
    backtrack(candidates, target, 0, current, result);
    return result;
}
```

---

#### Hard — N-Queens Problem (LeetCode 51)

Place N queens on an N×N chessboard so no two queens attack each other.

```python
def solve_n_queens(n):
    result = []
    board = ['.' * n] * n

    def is_safe(board, row, col):
        # Check column above
        for r in range(row):
            if board[r][col] == 'Q':
                return False
        # Check upper-left diagonal
        r, c = row - 1, col - 1
        while r >= 0 and c >= 0:
            if board[r][c] == 'Q': return False
            r -= 1; c -= 1
        # Check upper-right diagonal
        r, c = row - 1, col + 1
        while r >= 0 and c < n:
            if board[r][c] == 'Q': return False
            r -= 1; c += 1
        return True

    def backtrack(row):
        if row == n:
            result.append(board[:])
            return
        for col in range(n):
            if is_safe(board, row, col):
                board[row] = board[row][:col] + 'Q' + board[row][col+1:]
                backtrack(row + 1)
                board[row] = '.' * n  # Backtrack

    backtrack(0)
    return result
```

**Optimized version** uses three sets to track attacked columns and diagonals in O(1):

```python
def solve_n_queens_fast(n):
    result = []
    cols = set()
    diag1 = set()   # row - col
    diag2 = set()   # row + col
    board = [['.'] * n for _ in range(n)]

    def backtrack(row):
        if row == n:
            result.append([''.join(r) for r in board])
            return
        for col in range(n):
            if col in cols or (row-col) in diag1 or (row+col) in diag2:
                continue
            cols.add(col); diag1.add(row-col); diag2.add(row+col)
            board[row][col] = 'Q'
            backtrack(row + 1)
            cols.remove(col); diag1.remove(row-col); diag2.remove(row+col)
            board[row][col] = '.'

    backtrack(0)
    return result
```

---

## 9. Interview Preparation — Q&A Bank

### Language Basics Q&A

**Q: What's the difference between pass by value and pass by reference?**
In pass by value, a copy of the argument is made — changes inside the function don't affect the original. In pass by reference, the function receives the memory address of the variable — changes affect the original. In Python, all objects are passed by object reference (sometimes called "pass by assignment"). Immutable objects (int, str, tuple) behave like pass by value; mutable objects (list, dict) behave like pass by reference. In C++, you explicitly choose: `int x` (value), `int& x` (reference), `int* x` (pointer).

**Q: What is the difference between stack and heap memory?**
The **stack** is automatically managed memory for local variables and function call frames. It's LIFO, fast, and limited in size. The **heap** is manually managed memory for dynamic allocation (`new`/`delete` in C++, `malloc`/`free` in C). Python manages heap memory automatically via garbage collection (reference counting + cycle detection). Stack overflow happens when recursion is too deep.

**Q: What is a pointer in C++? Does Python have pointers?**
A pointer stores a memory address. `int* p = &x;` means `p` holds the address of `x`. Python doesn't expose pointers to the programmer — everything is a reference (an object handle), but you can't directly manipulate memory addresses. This is one of the biggest safety improvements Python offers over C++.

---

### OOP Q&A (20 Questions)

**Q1: What are the four pillars of OOP?**
Encapsulation (binding data and methods, restricting access), Abstraction (hiding complexity, exposing interface), Inheritance (child class reusing parent class), Polymorphism (same interface, multiple behaviors).

**Q2: What is the difference between abstraction and encapsulation?**
Encapsulation is about **data hiding** — preventing outside code from directly modifying internal state. Abstraction is about **implementation hiding** — showing what an object does, not how it does it. Encapsulation is the mechanism; abstraction is the design principle. A method signature is abstraction; making the data members private is encapsulation.

**Q3: What is the diamond problem in multiple inheritance?**
The diamond problem occurs when class D inherits from both B and C, and both B and C inherit from A. D gets two copies of A's methods/data. In C++, solve with `virtual` inheritance: `class B : virtual public A`. In Python, the MRO (Method Resolution Order) uses C3 linearization to determine a consistent order without duplication.

**Q4: What is method overloading vs method overriding?**
**Overloading** = compile-time polymorphism. Same method name, different signatures (parameters). C++ supports this natively. Python doesn't support true overloading (but you can simulate with `*args`).
**Overriding** = runtime polymorphism. A child class provides a new implementation for a parent's method. Both Python and C++ support this.

**Q5: What is a virtual function in C++?**
A virtual function is a member function declared with the `virtual` keyword in the base class. When a derived class overrides it, and the function is called through a base class pointer/reference, the derived class's version is called (runtime dispatch). Without `virtual`, the base class version would be called regardless of the actual object type. A *pure virtual* function (`= 0`) makes the class abstract.

**Q6: What is a constructor? Can a constructor be private?**
A constructor initializes an object when it's created. Yes, a constructor can be private — this is the basis of the **Singleton** and **Factory** design patterns. When the constructor is private, objects can only be created through a static factory method within the class.

**Q7: What is the difference between a class and a struct in C++?**
The only technical difference is the default access modifier: struct members are `public` by default, class members are `private`. By convention, structs are used for passive data groupings, classes for objects with behavior.

**Q8: What is an abstract class? Can it have a constructor?**
An abstract class is one that contains at least one abstract (pure virtual) method and cannot be instantiated. Yes, abstract classes can have constructors — they're called when instantiating a derived class.

**Q9: What is the difference between an interface and an abstract class?**
An interface (in languages like Java/C#) contains only method signatures — no implementation. An abstract class can have both abstract and concrete methods, plus member variables. Python achieves interface-like behavior via ABCs. C++ has no `interface` keyword — a class with all pure virtual methods serves as an interface.

**Q10: What is the `self` in Python vs `this` in C++?**
Both refer to the current instance of a class. In Python, `self` must be explicitly declared as the first parameter of every instance method. In C++, `this` is an implicit pointer automatically available inside any non-static member function. You use `this->member` to disambiguate from local variables with the same name.

**Q11: What is static method vs class method vs instance method in Python?**
- **Instance method** — takes `self`; can access and modify instance state.
- **Class method** — decorated with `@classmethod`; takes `cls`; can access and modify class state.
- **Static method** — decorated with `@staticmethod`; takes neither; a utility function logically belonging to the class.

**Q12: What is operator overloading?**
Defining the behavior of built-in operators for custom objects. In Python: `__add__` for `+`, `__eq__` for `==`, `__lt__` for `<`, etc. In C++: `operator+`, `operator==`, `operator<<`, etc. Example: a `Vector` class could define `+` to add corresponding coordinates.

**Q13: What is shallow copy vs deep copy?**
A **shallow copy** copies the references to nested objects, not the objects themselves — changes to nested mutable objects in the copy affect the original. A **deep copy** recursively copies all nested objects. In Python: `copy.copy()` for shallow, `copy.deepcopy()` for deep. In C++: default copy constructor is shallow; you must write a custom copy constructor for deep copy.

**Q14: What is the Rule of Three/Five in C++?**
If a class needs a custom **destructor**, it almost certainly needs a custom **copy constructor** and **copy assignment operator** too (Rule of Three). In C++11+, also define **move constructor** and **move assignment operator** (Rule of Five). The compiler-generated versions do shallow copy, which is problematic for classes managing raw pointer members.

**Q15: What is function hiding vs function overriding in C++?**
**Overriding** uses `virtual` — the derived class's version is called through a base pointer. **Hiding** happens without `virtual` — if a derived class defines a function with the same name (even different signature), it hides ALL base class versions of that name from the derived class scope. This is a common source of bugs.

**Q16: What is SOLID?**
Five design principles for OOP:
- **S**ingle Responsibility — one class, one reason to change
- **O**pen/Closed — open for extension, closed for modification
- **L**iskov Substitution — subclass can replace superclass without breaking behavior
- **I**nterface Segregation — many specific interfaces over one general interface
- **D**ependency Inversion — depend on abstractions, not concretions

**Q17: When would you use composition over inheritance?**
**"Favor composition over inheritance"** is a famous principle. Use inheritance for "is-a" relationships (Dog is-an Animal). Use composition for "has-a" relationships (Car has-an Engine). Inheritance tightly couples classes; composition is more flexible. If the relationship doesn't pass the "is-a" test in both directions, prefer composition.

**Q18: What is the difference between `__init__` and `__new__` in Python?**
`__new__` creates the instance (allocates memory and returns a new object). `__init__` initializes the instance (sets attributes). `__new__` is called first. You rarely override `__new__` — it's used in metaclasses and immutable types like `str` or `tuple` subclasses.

**Q19: Can you call a parent class constructor from a child class?**
Python: `super().__init__(args)`. C++: in the member initializer list — `Child(args) : Parent(parent_args) {}`. This is important when the parent has required constructor parameters.

**Q20: What is the Liskov Substitution Principle with an example?**
LSP says a subclass object must be usable wherever its superclass is expected, without breaking correctness. Classic violation: `Square` inherits from `Rectangle`. Setting height of a `Square` also sets width (to keep it a square). If code expects a `Rectangle` and calls `setWidth(5); setHeight(10)`, it expects area = 50. But with a `Square`, setting height also changes width, giving area = 100. The substitution broke the expected behavior.

---

### Recursion Q&A (15 Questions)

**Q1: What is recursion? What are its components?**
Recursion is a technique where a function calls itself to solve a smaller version of the same problem. Every recursive function has a base case (termination condition) and a recursive case (calls itself with a smaller input). Without a base case, recursion leads to infinite calls and a stack overflow.

**Q2: What is a stack overflow in the context of recursion?**
Each function call occupies a frame on the call stack. If recursion depth exceeds the stack limit (typically ~1000 in Python, tens of thousands in C++), a stack overflow occurs. Python raises `RecursionError`, C++ produces undefined behavior or a segmentation fault. Deep recursion can be avoided by converting to an iterative solution using an explicit stack.

**Q3: What is the difference between recursion and iteration?**
Both achieve repetition. Recursion uses the call stack (implicit), is often more elegant for divide-and-conquer problems, but has function call overhead and stack depth limits. Iteration uses loops (explicit state management), is generally more memory-efficient, and is preferred when the iterative solution is equally clear. Any recursive algorithm can be implemented iteratively using an explicit stack.

**Q4: What is tail recursion? Does Python optimize it?**
A tail-recursive function makes the recursive call as its very last action — no pending computation after the call returns. Some compilers (like GCC for C++) can optimize tail recursion into a loop, avoiding stack frame accumulation. Python does NOT perform tail call optimization (a deliberate design decision by Guido van Rossum, to preserve useful stack traces for debugging).

**Q5: What is memoization and how does it relate to recursion?**
Memoization caches the results of expensive recursive sub-calls. When the function is called with the same arguments again, it returns the cached result instead of recomputing. This converts a naive exponential recursive algorithm (like Fibonacci) from O(2ⁿ) to O(n). In Python: use `@functools.lru_cache` or a dictionary. In C++: use an `unordered_map`. Memoized recursion is the top-down form of dynamic programming.

**Q6: Trace the execution of factorial(4).**
`factorial(4) → 4 * factorial(3) → 4 * 3 * factorial(2) → 4 * 3 * 2 * factorial(1) → 4 * 3 * 2 * 1 = 24`. The calls go "down" (building the stack) and then the multiplications are resolved "up" (unwinding the stack). This is a key concept: computation happens during the unwinding phase.

**Q7: What is the time and space complexity of recursive Fibonacci?**
Naive recursive Fibonacci has time O(2ⁿ) and space O(n). Each call to `fib(n)` makes two calls, leading to a binary tree of calls with depth n. Many sub-problems are recomputed. With memoization, time becomes O(n) and space O(n). With bottom-up DP (tabulation), time O(n) and space O(1).

**Q8: How many moves does Tower of Hanoi require for n disks?**
Exactly 2ⁿ - 1 moves. For n=3: 7 moves. For n=4: 15 moves. This comes directly from the recurrence: `T(n) = 2T(n-1) + 1`. Solving: `T(n) = 2ⁿ - 1`. This is an important example of why exponential algorithms become impractical quickly — 64 disks would require 1.8 × 10¹⁹ moves.

**Q9: What is backtracking? How is it different from plain recursion?**
Backtracking is a recursive technique for finding solutions to problems by trying possibilities incrementally. When a partial solution leads to a dead end (violates constraints), we "backtrack" — undo the last choice and try another. Plain recursion simply divides and solves sub-problems. Backtracking explores a decision tree and prunes branches that cannot lead to valid solutions. Examples: N-Queens, Sudoku Solver, Generate Parentheses.

**Q10: What is the difference between include/exclude pattern and pick-from-remaining pattern?**
Both generate subsets/combinations but with different recursion structures. The **include/exclude pattern** makes a binary choice at each element: include it or don't. The **pick-from-remaining pattern** loops over remaining candidates and picks one. The include/exclude pattern naturally produces all 2ⁿ subsets; the pick-from-remaining pattern naturally produces combinations without duplicates and is easier to control ordering.

**Q11: How would you implement recursion iteratively?**
Use an explicit stack to simulate the call stack. Push the initial state, then in a loop: pop the top state, process it, push the sub-states. For example, DFS on a tree can be done recursively (implicit stack) or iteratively with an explicit `stack` data structure. Converting to iterative removes the stack depth limitation.

**Q12: What is the recursion tree method for time complexity analysis?**
Draw the tree of recursive calls. Each node is a call; the value at each node is the work done at that level (excluding recursive calls). Sum all work across all levels. For `T(n) = 2T(n/2) + n` (merge sort): height = log n, n work at each level, total = O(n log n).

**Q13: How would you find all subsets of an array using recursion?**
Use the include/exclude pattern: for each element, recursively explore two branches — one where the element is included, one where it isn't. Base case: when the index reaches the end of the array, add the current subset to the result. Total 2ⁿ subsets, so time complexity is O(n × 2ⁿ).

**Q14: How would you handle duplicate elements when generating subsets or permutations?**
Sort the array first. Then, when iterating at each level of recursion, skip elements that are equal to the previous element (at the same recursion level) to avoid generating duplicate subsets. This is the standard approach for LeetCode 90 (Subsets II) and LeetCode 47 (Permutations II).

**Q15: What's the practical recursion depth limit in Python vs C++?**
Python's default recursion limit is 1000 frames. You can increase it with `sys.setrecursionlimit(n)`, but very deep recursion is risky. C++ typically handles hundreds of thousands of stack frames (depending on system stack size). For competitive programming in Python, convert deep recursions to iterative with explicit stacks.

---

### Answer Frameworks

**When asked "Explain Recursion":**
1-sentence definition → why it exists (cleaner solution for divide-and-conquer problems) → how it works (call stack, base case, recursive case) → trade-offs (overhead, stack depth limit) → example (factorial or Fibonacci)

**When asked "Explain OOP":**
1-sentence definition → the four pillars (EAIP — Encapsulation, Abstraction, Inheritance, Polymorphism) → real-world analogy → contrast with procedural programming → language-specific notes

**When asked to write code involving recursion:**
1. State the base case first.
2. Trace through a small example manually.
3. Write the recursive step.
4. State time and space complexity.

---

### Depth Gauge — Junior vs Senior

**Junior (SDE-1) expectations:**
- Know the four OOP pillars with definitions and basic examples.
- Write factorial, Fibonacci, palindrome, and power recursively.
- Know the difference between iteration and recursion.
- Understand call stack and base case.
- Implement a basic class with encapsulation in Python and/or C++.

**Mid-level (SDE-2) expectations:**
All of the above, plus:
- Implement Tower of Hanoi, subsets, permutations.
- Explain memoization and convert naive recursion to memoized.
- Implement design patterns involving OOP (Singleton, Factory).
- Deep understanding of virtual functions and dynamic dispatch.
- Explain MRO in Python, diamond problem in C++.
- Identify LSP violations.

**Senior (SDE-3+) expectations:**
All of the above, plus:
- Design class hierarchies for complex real-world systems.
- Discuss SOLID principles with examples of violations and fixes.
- Analyze backtracking problems, implement with pruning.
- Convert recursive algorithms to iterative (for production safety).
- Discuss Rule of Three/Five in C++ for resource management.

---

## 10. Common Mistakes & Gotchas

**Recursion Mistakes:**
1. **Forgetting the base case** — causes infinite recursion and stack overflow.
2. **Base case is wrong** — `if n == 0: return 0` for factorial should be `if n <= 1: return 1`.
3. **Not making the problem strictly smaller** — `fib(n) → fib(n)` would infinite loop.
4. **Using mutable default arguments in Python** — `def f(x, memo={})` is shared across all calls. Use `memo=None` and initialize inside.
5. **Integer overflow in C++ recursion** — factorial(13) overflows `int`. Use `long long`.
6. **Python recursion limit** — default 1000; use `sys.setrecursionlimit()` or convert to iterative for large inputs.

**OOP Mistakes:**
1. **Missing `virtual` destructor** in C++ base classes — causes memory leak/UB when deleting via base pointer.
2. **Confusing shallow copy and deep copy** in C++ — default copy constructor is shallow.
3. **Python `__` name mangling** — `self.__x` is accessible as `self._ClassName__x`. Not truly private.
4. **Calling abstract class constructor directly** — Python raises `TypeError`, but abstract classes CAN have constructors called via `super()` from derived classes.
5. **Not calling `super().__init__`** in Python subclass constructor — parent's initialization won't happen.
6. **Thinking Python supports method overloading** — it doesn't. The last defined method with the same name wins.

**Language Gotchas:**
1. **Python `//` vs C++ `/` for negative numbers** — `-7 // 2 = -4` (Python) vs `-7 / 2 = -3` (C++).
2. **`int` overflow in C++** — always use `long long` for sums, products of large numbers in interviews.
3. **Python list as default mutable argument** — `def f(x, lst=[])` — the list persists across calls.
4. **C++ `auto` type deduction** — useful but can surprise: `auto x = 3 / 2` gives `int 1`, not `float`.
5. **Python's `is` vs `==`** — `is` checks identity (same object), `==` checks equality (same value). `[1] is [1]` is False; `[1] == [1]` is True.

---

## 11. Source Notes

| Source | URL | Contribution |
|---|---|---|
| Striver A2Z DSA Playlist | youtube.com/playlist?list=PLgUwDviBIf0oF6QL8m22w1hIDC1vJ_BHz | Course structure, problem list, topic sequence (Basics → Recursion) |
| TakeUForward (takeuforward.org) | takeuforward.org | Official Striver sheet: 31 basics problems, 25 recursion problems |
| GeeksforGeeks OOP | geeksforgeeks.org/python-oops-concepts | OOP definitions, Python examples, access modifiers |
| GeeksforGeeks OOP Interview | geeksforgeeks.org/interview-prep/oops-interview-questions | Interview Q&A patterns for OOP |
| InterviewBit OOP | interviewbit.com/oops-interview-questions | Diamond problem, virtual functions, overloading vs overriding |
| Tech Interview Handbook | techinterviewhandbook.org/algorithms/recursion | Recursion cheatsheet, essential practice problems |
| RealPython C++ vs Python | realpython.com/python-vs-cpp | Variable typing, memory model, language comparison |
| AlgoCademy | algocademy.com/blog/python-vs-c-for-coding-interviews | Interview language comparison, two_sum dual implementation |
| devinterview.io | github.com/Devinterview-io/oop-interview-questions | OOP Q&A framework |

---

## 12. Glossary

**Abstract Class:** A class with at least one pure virtual (C++) or `@abstractmethod` (Python) method; cannot be instantiated directly.

**Backtracking:** A recursive algorithm that tries all possibilities and undoes ("backtracks") choices that violate constraints.

**Base Case:** The termination condition in a recursive function — the simplest version of the problem solved directly without recursion.

**Call Stack:** A stack data structure maintained by the runtime that tracks active function calls. Each call pushes a frame; each return pops it.

**Compile-time Polymorphism:** Polymorphism resolved at compile time — function overloading, operator overloading.

**Constructors:** Special methods called automatically when an object is created to initialize its state.

**Deep Copy:** A copy that recursively duplicates all nested objects, creating entirely independent copies.

**Destructor:** Method called when an object is destroyed (`~ClassName()` in C++, `__del__` in Python).

**Duck Typing:** Python's polymorphism style — "If it looks like a duck and quacks like a duck, it is a duck." The type of an object is less important than whether it has the required methods.

**Encapsulation:** Bundling data and methods into a class; restricting direct external access to internal data.

**Inheritance:** Mechanism by which a child class acquires properties and behaviors from a parent class.

**Memoization:** Caching return values of recursive calls to avoid redundant computation; top-down dynamic programming.

**Method Resolution Order (MRO):** Python's algorithm (C3 linearization) to determine the order in which base classes are searched for methods in multiple inheritance.

**Polymorphism:** The ability of different objects to respond to the same interface (method call) in different ways.

**Pure Virtual Function:** In C++, a virtual function declared with `= 0`. Makes the class abstract; forces derived classes to provide an implementation.

**Recursion:** A function that calls itself to solve a smaller instance of the same problem.

**Runtime Polymorphism:** Polymorphism resolved at runtime via virtual functions and dynamic dispatch.

**Shallow Copy:** Copies references to objects rather than the objects themselves; nested mutable objects are shared between original and copy.

**Stack Overflow:** Error when the call stack exceeds its size limit, often caused by infinite or excessively deep recursion.

**Static Method:** A method that belongs to the class rather than any instance; does not receive `self` or `cls`.

**Tail Recursion:** A recursive call where the recursive call is the very last operation; no pending computation awaits after the call returns.

**Virtual Function:** In C++, a function declared with `virtual` that enables runtime polymorphism — the derived class's version is called through a base pointer.

---

*This book was compiled for interview preparation use. All code examples are tested for Python 3.10+ and C++17. For the latest problems and video explanations, refer to [TakeUForward](https://takeuforward.org/) and [LeetCode](https://leetcode.com/).*
