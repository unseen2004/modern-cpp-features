# C++11/14/17/20/23

## Overview
Many of these descriptions and examples come from various resources (see [Acknowledgements](#acknowledgements) section), summarized in my own words.

C++23 includes the following new language features:
- [deducing this](#deducing-this)
- [if consteval](#if-consteval)
- [multidimensional subscript operator](#multidimensional-subscript-operator)
- [auto(x) and auto{x}](#autox-and-autox)
- [[[assume]] attribute](#assume-attribute)

C++23 includes the following new library features:
- [std::expected](#stdexpected)
- [std::print](#stdprint)
- [std::mdspan](#stdmdspan)
- [std::flat_map/flat_set](#stdflat_mapflat_set)
- [std::byteswap](#stdbyteswap)

C++20 includes the following new language features:
- [concepts](#concepts)
- [designated initializers](#designated-initializers)
- [templates with auto](#templates-with-auto)
- [three-way comparison operator (<=>)](#three-way-comparison-operator)
- [range-based for loop with initializer](#range-based-for-loop-with-initializer)
- [consteval and constinit](#consteval-and-constinit)
- [coroutines](#coroutines)
- [modules](#modules)

C++20 includes the following new library features:
- [std::ranges](#stdranges)
- [std::span](#stdspan)
- [std::format](#stdformat)
- [std::numbers](#stdnumbers)
- [std::source_location](#stdsource_location)

C++17 includes the following new language features:
- [template argument deduction for class templates](#template-argument-deduction-for-class-templates)
- [declaring non-type template parameters with auto](#declaring-non-type-template-parameters-with-auto)
- [folding expressions](#folding-expressions)
- [new rules for auto deduction from braced-init-list](#new-rules-for-auto-deduction---from---braced---init---list)
- [constexpr lambda](#constexpr-lambda)
- [inline variables](#inline-variables)
- [nested namespaces](#nested-namespaces)
- [structured bindings](#structured-bindings)
- [selection statements with initializer](#selection-statements-with-initializer)
- [constexpr if](#constexpr-if)

C++17 includes the following new library features:
- [std::variant](#stdvariant)
- [std::optional](#stdoptional)
- [std::any](#stdany)
- [std::string_view](#stdstring_view)
- [std::invoke](#stdinvoke)
- [std::apply](#stdapply)
- [splicing for maps and sets](#splicing-for-maps-and-sets)

C++14 includes the following new language features:
- [binary literals](#binary-literals)
- [generic lambda expressions](#generic-lambda-expressions)
- [return type deduction](#return-type-deduction)
- [decltype(auto)](#decltypeauto)
- [relaxing constraints on constexpr functions](#relaxing-constraints-on-constexpr-functions)

C++14 includes the following new library features:
- [user-defined literals for standard library types](#user-defined-literals-for-standard-library-types)
- [compile-time integer sequences](#compile---time-integer-sequences)

C++11 includes the following new language features:
- [move semantics](#move-semantics)
- [variadic templates](#variadic-templates)
- [rvalue references](#rvalue-references)
- [initializer lists](#initializer-lists)
- [static assertions](#static-assertions)
- [auto](#auto)
- [lambda expressions](#lambda-expressions)
- [decltype](#decltype)
- [template aliases](#template-aliases)
- [nullptr](#nullptr)
- [strongly-typed enums](#strongly---typed-enums)
- [attributes](#attributes)
- [constexpr](#constexpr)
- [delegating constructors](#delegating-constructors)
- [user-defined literals](#user---defined-literals)
- [explicit virtual overrides](#explicit-virtual-overrides)
- [default functions](#default-functions)
- [deleted functions](#deleted-functions)
- [range-based for loops](#range---based-for-loops)
- [special member functions for move semantics](#special-member-functions-for-move-semantics)

C++11 includes the following new library features:
- [std::move](#stdmove)
- [std::forward](#stdforward)
- [std::to_string](#stdto_string)
- [type traits](#type-traits)
- [smart pointers](#smart-pointers)
- [std::chrono](#stdchrono)
- [tuples](#tuples)
- [std::tie](#stdtie)
- [std::array](#stdarray)
- [unordered containers](#unordered-containers)
- [memory model](#memory-model)

## C++23 Language Features

### Deducing this
Explicit object parameters allow you to explicitly define the `this` parameter in a member function, which enables deducing the value category of the object it was called on.
```c++
struct Y {
  template <typename Self>
  void Foo(this Self&& self) {
    // `self` is the object the function was called on.
  }
};
```

### if consteval
A new way to check if a function is being evaluated in a constant-evaluated context.
```c++
constexpr int Compute(int n) {
  if consteval {
    return n * n; // Compile-time version
  } else {
    return n + n; // Run-time version
  }
}
```

### Multidimensional subscript operator
Allows `operator[]` to take multiple arguments.
```c++
struct Matrix {
  double& operator[](std::size_t row, std::size_t col);
};

Matrix m;
m[1, 2] = 3.0;
```

### auto(x) and auto{x}
Cast an expression to a decayed type, as if passed by value to a function.
```c++
void Foo(auto x) {
  auto y = auto(x); // y is a copy of x, decayed
}
```

### [[assume]] attribute
Provides a hint to the optimizer that a certain condition is true.
```c++
void Process(int x) {
  [[assume(x > 0)]];
  // Optimizer can assume x is positive.
}
```

## C++23 Library Features

### std::expected
A type that represents either an expected value of type `T` or an unexpected error of type `E`.
```c++
std::expected<int, std::string> ParseInt(std::string_view s) {
  if (s == "42") return 42;
  return std::unexpected("failed to parse");
}
```

### std::print
A more efficient and safer way to print formatted text to standard output.
```c++
std::print("Hello, {}!\n", "world");
```

### std::mdspan
A non-owning view that provides multidimensional access to a contiguous sequence of elements.
```c++
std::vector<int> data(100);
std::mdspan view(data.data(), 10, 10);
view[1, 2] = 42;
```

### std::flat_map/flat_set
Container adaptors that provide the interface of a map or set but use underlying sequence containers (like `std::vector`) to store elements in a sorted manner.
```c++
std::flat_map<int, std::string> m = {{1, "one"}, {2, "two"}};
```

### std::byteswap
A utility to swap the byte order of an integer value.
```c++
uint32_t val = 0x12345678;
uint32_t swapped = std::byteswap(val); // 0x78563412
```

## C++20 Language Features

### Concepts
A way to specify requirements on template arguments.
```c++
template <typename T>
concept Hashable = requires(T t) {
  { std::hash<T>{}(t) } -> std::convertible_to<std::size_t>;
};

template <Hashable T>
void Process(T t) { /* ... */ }
```

### Designated initializers
Allow initializing members of an aggregate by name.
```c++
struct Point {
  int x;
  int y;
};

Point p = {.x = 10, .y = 20};
```

### Templates with auto
Using `auto` in template parameter lists.
```c++
void Foo(auto x) { /* ... */ } // Equivalent to template <typename T> void Foo(T x)
```

### Three-way comparison operator
The "spaceship" operator (`<=>`) allows for consistent and efficient comparisons.
```c++
struct Point {
  int x;
  int y;
  auto operator<=>(const Point&) const = default;
};
```

### Range-based for loop with initializer
Allows an initializer in a range-based for loop, similar to `if` and `switch`.
```c++
for (auto v = GetVector(); int x : v) {
  // ...
}
```

### consteval and constinit
`consteval` specifies that a function MUST be evaluated at compile-time. `constinit` ensures that a variable with static or thread storage duration has static initialization.
```c++
consteval int Square(int n) { return n * n; }
constinit int kLimit = 100;
```

### Coroutines
Functions that can be suspended and resumed.
```c++
Generator<int> GetNumbers() {
  for (int i = 0; i < 10; ++i) {
    co_yield i;
  }
}
```

### Modules
A modern way to organize and share code, replacing header files.
```c++
// math.ixx
export module math;
export int Add(int a, int b) { return a + b; }

// main.cpp
import math;
int main() { return Add(1, 2); }
```

## C++20 Library Features

### std::ranges
New algorithms and views for working with ranges of elements.
```c++
std::vector<int> v = {3, 1, 4, 1, 5, 9};
std::ranges::sort(v);
auto even = v | std::views::filter([](int n) { return n % 2 == 0; });
```

### std::span
A non-owning view of a contiguous sequence of elements.
```c++
void Process(std::span<int> s) {
  for (int x : s) { /* ... */ }
}
```

### std::format
Type-safe and extensible string formatting.
```c++
std::string s = std::format("The answer is {}.", 42);
```

### std::numbers
Mathematical constants (e.g., pi, e).
```c++
double area = std::numbers::pi * r * r;
```

### std::source_location
A utility to capture information about the source code (file name, line number, etc.).
```c++
void Log(std::string_view message, std::source_location location = std::source_location::current()) {
  std::print("{}:{}: {}\n", location.file_name(), location.line(), message);
}
```

## C++17 Language Features

### Template argument deduction for class templates
Automatic template argument deduction much like how it's done for functions, but now including class constructors.
```c++
template <typename T = float>
struct MyContainer {
  T val;
  MyContainer() : val() {}
  MyContainer(T val) : val(val) {}
  // ...
};
MyContainer c1{ 1 }; // OK MyContainer<int>
MyContainer c2; // OK MyContainer<float>
```

### Declaring non-type template parameters with auto
Following the deduction rules of `auto`, while respecting the non-type template parameter list of allowable types[\*], template arguments can be deduced from the types of its arguments:
```c++
// Explicitly pass type `int` as template argument.
auto seq = std::integer_sequence<int, 0, 1, 2>();
// Type is deduced to be `int`.
auto seq2 = my_integer_sequence<0, 1, 2>();
```
\* - For example, you cannot use a `double` as a template parameter type, which also makes this an invalid deduction using `auto`.

### Folding expressions
A fold expression performs a fold of a template parameter pack over a binary operator.
* An expression of the form `(... op e)` or `(e op ...)`, where `op` is a fold-operator and `e` is an unexpanded parameter pack, are called _unary folds_.
* An expression of the form `(e1 op1 ... op2 e2)`, where `op1` and `op2` are fold-operators, is called a _binary fold_. Either `e1` or `e2` are unexpanded parameter packs, but not both.
```c++
template <typename... Args>
bool LogicalAnd(Args... args) {
  // Binary folding.
  return (true && ... && args);
}
bool b = true;
bool& b2 = b;
LogicalAnd(b, b2, true); // == true
```
```c++
template <typename... Args>
auto Sum(Args... args) {
  // Unary folding.
  return (... + args);
}
Sum(1.0, 2.0f, 3); // == 6.0
```

### New rules for auto deduction from braced-init-list
Changes to `auto` deduction when used with the uniform initialization syntax. Previously, `auto x{ 3 };` deduces a `std::initializer_list<int>`, which now deduces to `int`.
```c++
auto x1{ 1, 2, 3 }; // error: not a single element
auto x2 = { 1, 2, 3 }; // decltype(x2) is std::initializer_list<int>
auto x3{ 3 }; // decltype(x3) is int
auto x4{ 3.0 }; // decltype(x4) is double
```

### constexpr lambda
Compile-time lambdas using `constexpr`.
```c++
auto identity = [] (int n) constexpr { return n; };
static_assert(identity(123) == 123);
```
```c++
constexpr auto add = [] (int x, int y) {
  auto l = [=] { return x; };
  auto r = [=] { return y; };
  return [=] { return l() + r(); };
};

static_assert(add(1, 2)() == 3);
```
```c++
constexpr int AddOne(int n) {
  return [n] { return n + 1; }();
}

static_assert(AddOne(1) == 2);
```

### Inline variables
The inline specifier can be applied to variables as well as to functions. A variable declared inline has the same semantics as a function declared inline.
```c++
// Disassembly example using compiler explorer.
struct S { int x; };
inline S x1 = S{321}; // mov esi, dword ptr [x1]
                      // x1: .long 321

S x2 = S{123};        // mov eax, dword ptr [.L_ZZ4mainE2x2]
                      // mov dword ptr [rbp - 8], eax
                      // .L_ZZ4mainE2x2: .long 123
```

### Nested namespaces
Using the namespace resolution operator to create nested namespace definitions.
```c++
namespace A {
  namespace B {
    namespace C {
      int i;
    }
  }
}
// vs.
namespace A::B::C {
  int i;
}
```

### Structured bindings
A proposal for de-structuring initialization, that would allow writing `auto {x, y, z} = expr;` where the type of `expr` was a tuple-like object, whose elements would be bound to the variables `x`, `y`, and `z` (which this construct declares). _Tuple-like objects_ include `std::tuple`, `std::pair`, `std::array`, and aggregate structures.
```c++
using Coordinate = std::pair<int, int>;
Coordinate Origin() {
  return Coordinate{0, 0};
}

const auto [ x, y ] = Origin();
x; // == 0
y; // == 0
```

### Selection statements with initializer
New versions of the `if` and `switch` statements which simplify common code patterns and help users keep scopes tight.
```c++
{
  std::lock_guard<std::mutex> lk(mx);
  if (v.empty()) v.push_back(val);
}
// vs.
if (std::lock_guard<std::mutex> lk(mx); v.empty()) {
  v.push_back(val);
}
```
```c++
Foo Gadget(args);
switch (auto s = Gadget.status()) {
  case OK: Gadget.zip(); break;
  case Bad: throw BadFoo(s.message());
}
// vs.
switch (Foo Gadget(args); auto s = Gadget.status()) {
  case OK: Gadget.zip(); break;
  case Bad: throw BadFoo(s.message());
}
```

### constexpr if
Write code that is instantiated depending on a compile-time condition.
```c++
template <typename T>
constexpr bool IsIntegral() {
  if constexpr (std::is_integral<T>::value) {
    return true;
  } else {
    return false;
  }
}
static_assert(IsIntegral<int>() == true);
static_assert(IsIntegral<char>() == true);
static_assert(IsIntegral<double>() == false);
struct S {};
static_assert(IsIntegral<S>() == false);
```

## C++17 Library Features

### std::variant
The class template `std::variant` represents a type-safe `union`. An instance of `std::variant` at any given time holds a value of one of its alternative types (it's also possible for it to be valueless).
```c++
std::variant<int, double> v{ 12 };
std::get<int>(v); // == 12
std::get<0>(v); // == 12
v = 12.0;
std::get<double>(v); // == 12.0
std::get<1>(v); // == 12.0
```

### std::optional
The class template `std::optional` manages an optional contained value, i.e. a value that may or may not be present. A common use case for optional is the return value of a function that may fail.
```c++
std::optional<std::string> Create(bool b) {
  if (b) {
    return "Godzilla";
  } else {
    return {};
  }
}

Create(false).value_or("empty"); // == "empty"
Create(true).value(); // == "Godzilla"
// optional-returning factory functions are usable as conditions of while and if
if (auto str = Create(true)) {
  // ...
}
```

### std::any
A type-safe container for single values of any type.
```c++
std::any x{ 5 };
x.has_value() // == true
std::any_cast<int>(x) // == 5
std::any_cast<int&>(x) = 10;
std::any_cast<int>(x) // == 10
```

### std::string_view
A non-owning reference to a string. Useful for providing an abstraction on top of strings (e.g. for parsing).
```c++
// Regular strings.
std::string_view cppstr{ "foo" };
// Wide strings.
std::wstring_view wcstr_v{ L"baz" };
// Character arrays.
char array[3] = {'b', 'a', 'r'};
std::string_view array_v(array, sizeof array);
```
```c++
std::string str{ "   trim me" };
std::string_view v{ str };
v.remove_prefix(std::min(v.find_first_not_of(" "), v.size()));
str; //  == "   trim me"
v; // == "trim me"
```

### std::invoke
Invoke a `Callable` object with parameters. Examples of `Callable` objects are `std::function` or `std::bind` where an object can be called similarly to a regular function.
```c++
template <typename Callable>
class Proxy {
  Callable c_;
 public:
  Proxy(Callable c) : c_(c) {}
  template <class... Args>
  decltype(auto) operator()(Args&&... args) {
    // ...
    return std::invoke(c_, std::forward<Args>(args)...);
  }
};
auto add = [] (int x, int y) {
  return x + y;
};
Proxy<decltype(add)> p{ add };
p(1, 2); // == 3
```

### std::apply
Invoke a `Callable` object with a tuple of arguments.
```c++
auto add = [] (int x, int y) {
  return x + y;
};
std::apply(add, std::make_tuple( 1, 2 )); // == 3
```

### Splicing for maps and sets
Moving nodes and merging containers without the overhead of expensive copies, moves, or heap allocations/deallocations.

Moving elements from one map to another:
```c++
std::map<int, string> src{ { 1, "one" }, { 2, "two" }, { 3, "buckle my shoe" } };
std::map<int, string> dst{ { 3, "three" } };
dst.insert(src.extract(src.find(1))); // Cheap remove and insert of { 1, "one" } from `src` to `dst`.
dst.insert(src.extract(2)); // Cheap remove and insert of { 2, "two" } from `src` to `dst`.
// dst == { { 1, "one" }, { 2, "two" }, { 3, "three" } };
```

Inserting an entire set:
```c++
std::set<int> src{1, 3, 5};
std::set<int> dst{2, 4, 5};
dst.merge(src);
// src == { 5 }
// dst == { 1, 2, 3, 4, 5 }
```

Inserting elements which outlive the container:
```c++
auto ElementFactory() {
  std::set<...> s;
  s.emplace(...);
  return s.extract(s.begin());
}
s2.insert(ElementFactory());
```

Changing the key of a map element:
```c++
std::map<int, string> m{ { 1, "one" }, { 2, "two" }, { 3, "three" } };
auto e = m.extract(2);
e.key() = 4;
m.insert(std::move(e));
// m == { { 1, "one" }, { 3, "three" }, { 4, "two" } }
```

## C++14 Language Features

### Binary literals
Binary literals provide a convenient way to represent a base-2 number.
It is possible to separate digits with `'`.
```c++
0b110 // == 6
0b1111'1111 // == 255
```

### Generic lambda expressions
C++14 now allows the `auto` type-specifier in the parameter list, enabling polymorphic lambdas.
```c++
auto identity = [](auto x) { return x; };
int three = identity(3); // == 3
std::string foo = identity("foo"); // == "foo"
```

### Return type deduction
Using an `auto` return type in C++14, the compiler will attempt to deduce the type for you. With lambdas, you can now deduce its return type using `auto`, which makes returning a deduced reference or rvalue reference possible.
```c++
// Deduce return type as `int`.
auto F(int i) {
  return i;
}
```
```c++
template <typename T>
auto& F(T& t) {
  return t;
}

// Returns a reference to a deduced type.
auto g = [](auto& x) -> auto& { return F(x); };
int y = 123;
int& z = g(y); // reference to `y`
```

### decltype(auto)
The `decltype(auto)` type-specifier also deduces a type like `auto` does. However, it deduces return types while keeping their references or "const-ness", while `auto` will not.
```c++
const int x = 0;
auto x1 = x; // int
decltype(auto) x2 = x; // const int
int y = 0;
int& y1 = y;
auto y2 = y; // int
decltype(auto) y3 = y; // int&
int&& z = 0;
auto z1 = std::move(z); // int
decltype(auto) z2 = std::move(z); // int&&
```
```c++
// Note: Especially useful for generic code!

// Return type is `int`.
auto F(const int& i) {
  return i;
}

// Return type is `const int&`.
decltype(auto) G(const int& i) {
  return i;
}

int x = 123;
static_assert(std::is_same<const int&, decltype(F(x))>::value == 0);
static_assert(std::is_same<int, decltype(F(x))>::value == 1);
static_assert(std::is_same<const int&, decltype(G(x))>::value == 1);
```

### Relaxing constraints on constexpr functions
In C++11, `constexpr` function bodies could only contain a very limited set of syntax, including (but not limited to): `typedef`s, `using`s, and a single `return` statement. In C++14, the set of allowable syntax expands greatly to include the most common syntax such as `if` statements, multiple `return`s, loops, etc.
```c++
constexpr int Factorial(int n) {
  if (n <= 1) {
    return 1;
  } else {
    return n * Factorial(n - 1);
  }
}
Factorial(5); // == 120
```

## C++14 Library Features

### User-defined literals for standard library types
New user-defined literals for standard library types, including new built-in literals for `chrono` and `basic_string`. These can be `constexpr` meaning they can be used at compile-time. Some uses for these literals include compile-time integer parsing, binary literals, and imaginary number literals.
```c++
using namespace std::chrono_literals;
auto day = 24h;
day.count(); // == 24
std::chrono::duration_cast<std::chrono::minutes>(day).count(); // == 1440
```

### Compile-time integer sequences
The class template `std::integer_sequence` represents a compile-time sequence of integers. There are a few helpers built on top:
* `std::make_integer_sequence<T, N...>` - creates a sequence of `0, ..., N - 1` with type `T`.
* `std::index_sequence_for<T...>` - converts a template parameter pack into an integer sequence.

Convert an array into a tuple:
```c++
template<typename Array, std::size_t... I>
decltype(auto) a2t_impl(const Array& a, std::integer_sequence<std::size_t, I...>) {
  return std::make_tuple(a[I]...);
}

template<typename T, std::size_t N, typename Indices = std::make_index_sequence<N>>
decltype(auto) a2t(const std::array<T, N>& a) {
  return a2t_impl(a, Indices());
}
```

## C++11 Language Features

### Move semantics
Move semantics is mostly about performance optimization: the ability to move an object without the expensive overhead of copying. The difference between a copy and a move is that a copy leaves the source unchanged, and a move will leave the source either unchanged or radically different -- depending on what the source is. For plain old data, a move is the same as a copy.

To move an object means to transfer ownership of some resource it manages to another object. You could think of this as changing pointers held by the source object to be moved, or now held, by the destination object; the resource remains in its location in memory. Such an inexpensive transfer of resources is extremely useful when the source is an `rvalue`, where the potentially dangerous side-effect of changing the source after the move is redundant since the source is a temporary object that won't be accessible later.

Moves also make it possible to transfer objects such as `std::unique_ptr`s, smart pointers that are designed to hold a pointer to a unique object, from one scope to another.

See the sections on: rvalue references, defining move special member functions, `std::move`, `std::forward`.

### Rvalue references
C++11 introduces a new reference termed the _rvalue reference_. An rvalue reference to `A` is created with the syntax `A&&`. This enables two major features: move semantics; and _perfect forwarding_, the ability to pass arguments while maintaining information about them as lvalues/rvalues in a generic way.

`auto` type deduction with lvalues and rvalues:
```c++
int x = 0; // `x` is an lvalue of type `int`
int& xl = x; // `xl` is an lvalue of type `int&`
int&& xr = x; // compiler error -- `x` is an lvalue
int&& xr2 = 0; // `xr2` is an lvalue of type `int&&`
auto& al = x; // `al` is an lvalue of type `int&`
auto&& al2 = x; // `al2` is an lvalue of type `int&`
auto&& ar = 0; // `ar` is an lvalue of type `int&&`
```

See also: `std::move`, `std::forward`.

### Variadic templates
The `...` syntax creates a _parameter pack_ or expands one. A template _parameter pack_ is a template parameter that accepts zero or more template arguments (non-types, types, or templates). A template with at least one parameter pack is called a _variadic template_.
```c++
template <typename... T>
struct Arity {
  constexpr static int kValue = sizeof...(T);
};
static_assert(Arity<>::kValue == 0);
static_assert(Arity<char, short, int>::kValue == 3);
```

### Initializer lists
A lightweight array-like container of elements created using a "braced list" syntax. For example, `{ 1, 2, 3 }` creates a sequences of integers, that has type `std::initializer_list<int>`. Useful as a replacement to passing a vector of objects to a function.
```c++
int Sum(const std::initializer_list<int>& list) {
  int total = 0;
  for (auto& e : list) {
    total += e;
  }

  return total;
}

auto list = { 1, 2, 3 };
F(list); // == 6
F({ 1, 2, 3 }); // == 6
F({}); // == 0
```

### Static assertions
Assertions that are evaluated at compile-time.
```c++
constexpr int x = 0;
constexpr int y = 1;
static_assert(x == y, "x != y");
```

### auto
`auto`-typed variables are deduced by the compiler according to the type of their initializer.
```c++
auto a = 3.14; // double
auto b = 1; // int
auto& c = b; // int&
auto d = { 0 }; // std::initializer_list<int>
auto&& e = 1; // int&&
auto&& f = b; // int&
auto g = new auto(123); // int*
const auto h = 1; // const int
auto i = 1, j = 2, k = 3; // int, int, int
auto l = 1, m = true, n = 1.61; // error -- `l` deduced to be int, `m` is bool
auto o; // error -- `o` requires initializer
```

Extremely useful for readability, especially for complicated types:
```c++
std::vector<int> v = ...;
std::vector<int>::const_iterator cit = v.cbegin();
// vs.
auto cit = v.cbegin();
```

Functions can also deduce the return type using `auto`. In C++11, a return type must be specified either explicitly, or using `decltype` like so:
```c++
template <typename X, typename Y>
auto Add(X x, Y y) -> decltype(x + y) {
  return x + y;
}
Add(1, 2); // == 3
Add(1, 2.0); // == 3.0
Add(1.5, 1.5); // == 3.0
```
The trailing return type in the above example is the _declared type_ (see section on `decltype`) of the expression `x + y`. For example, if `x` is an integer and `y` is a double, `decltype(x + y)` is a double. Therefore, the above function will deduce the type depending on what type the expression `x + y` yields. Notice that the trailing return type has access to its parameters, and `this` when appropriate.

### Lambda expressions
A `lambda` is an unnamed function object capable of capturing variables in scope. It features: a _capture list_; an optional set of parameters with an optional trailing return type; and a body. Examples of capture lists:
* `[]` - captures nothing.
* `[=]` - capture local objects (local variables, parameters) in scope by value.
* `[&]` - capture local objects (local variables, parameters) in scope by reference.
* `[this]` - capture `this` pointer by value.
* `[a, &b]` - capture objects `a` by value, `b` by reference.

```c++
int x = 1;

auto get_x = [=]{ return x; };
get_x(); // == 1

auto add_x = [=](int y) { return x + y; };
add_x(1); // == 2

auto get_x_ref = [&]() -> int& { return x; };
get_x_ref(); // int& to `x`
```

### decltype
`decltype` is an operator which returns the _declared type_ of an expression passed to it. Examples of `decltype`:
```c++
int a = 1; // `a` is declared as type `int`
decltype(x) b = a; // `decltype(x)` is `int`
const int& c = a; // `c` is declared as type `const int&`
decltype(c) d = a; // `decltype(c)` is `const int&`
decltype(123) e = 123; // `decltype(123)` is `int`
int&& f = 1; // `f` is declared as type `int&&`
decltype(f) g = 1; // `decltype(f) is `int&&`
decltype((a)) h = x; // `decltype((a))` is int&
```
```c++
template <typename X, typename Y>
auto Add(X x, Y y) -> decltype(x + y) {
  return x + y;
}
Add(1, 2.0); // `decltype(x + y)` => `decltype(3.0)` => `double`
```

### Template aliases
Semantically similar to using a `typedef` however, template aliases with `using` are easier to read and are compatible with templates.
```c++
template <typename T>
using Vec = std::vector<T>;
Vec<int> v{}; // std::vector<int>

using String = std::string;
String s{"foo"};
```

### nullptr
C++11 introduces a new null pointer type designed to replace C's `NULL` macro. `nullptr` itself is of type `std::nullptr_t` and can be implicitly converted into pointer types, and unlike `NULL`, not convertible to integral types except `bool`.
```c++
void Foo(int);
void Foo(char*);
Foo(NULL); // error -- ambiguous
Foo(nullptr); // calls Foo(char*)
```

### Strongly-typed enums
Type-safe enums that solve a variety of problems with C-style enums including: implicit conversions, inability to specify the underlying type, scope pollution.
```c++
// Specifying underlying type as `unsigned int`
enum class Color : unsigned int { Red = 0xff0000, Green = 0xff00, Blue = 0xff };
// `Red`/`Green` in `Alert` don't conflict with `Color`
enum class Alert : bool { Red, Green };
Color c = Color::Red;
```

### Attributes
Attributes provide a universal syntax over `__attribute__(...)`, `__declspec`, etc.
```c++
// `noreturn` attribute indicates `F` doesn't return.
[[ noreturn ]] void F() {
  throw "error";
}
```

### constexpr
Constant expressions are expressions evaluated by the compiler at compile-time. Only non-complex computations can be carried out in a constant expression. Use the `constexpr` specifier to indicate the variable, function, etc. is a constant expression.
```c++
constexpr int Square(int x) {
  return x * x;
}

int Square2(int x) {
  return x * x;
}

int a = Square(2);  // mov DWORD PTR [rbp-4], 4

int b = Square2(2); // mov edi, 2
                    // call Square2(int)
                    // mov DWORD PTR [rbp-8], eax
```

`constexpr` values are those that the compiler can evaluate at compile-time:
```c++
const int x = 123;
constexpr const int& y = x; // error -- constexpr variable `y` must be initialized by a constant expression
```

Constant expressions with classes:
```c++
struct Complex {
  constexpr Complex(double r, double i) : re(r), im(i) { }
  constexpr double Real() { return re; }
  constexpr double Imag() { return im; }

private:
  double re;
  double im;
};

constexpr Complex kI(0, 1);
```

### Delegating constructors
Constructors can now call other constructors in the same class using an initializer list.
```c++
struct Foo {
  int foo;
  Foo(int foo) : foo(foo) {}
  Foo() : Foo(0) {}
};

Foo foo{};
foo.foo; // == 0
```

### User-defined literals
User-defined literals allow you to extend the language and add your own syntax. To create a literal, define a `T operator "" X(...) { ... }` function that returns a type `T`, with a name `X`. Note that the name of this function defines the name of the literal. Any literal names not starting with an underscore are reserved and won't be invoked. There are rules on what parameters a user-defined literal function should accept, according to what type the literal is called on.

Converting Celsius to Fahrenheit:
```c++
// `unsigned long long` parameter required for integer literal.
long long operator "" _celsius(unsigned long long temp_celsius) {
  return std::llround(temp_celsius * 1.8 + 32);
}
24_celsius; // == 75
```

String to integer conversion:
```c++
// `const char*` and `std::size_t` required as parameters.
int operator "" _int(const char* str, std::size_t) {
  return std::stoi(str);
}

"123"_int; // == 123, with type `int`
```

### Explicit virtual overrides
Specifies that a virtual function overrides another virtual function. If the virtual function does not override a parent's virtual function, throws a compiler error.
```c++
struct A {
  virtual void Foo();
  void Bar();
};

struct B : A {
  void Foo() override; // correct -- B::Foo overrides A::Foo
  void Bar() override; // error -- A::Bar is not virtual
  void Baz() override; // error -- B::Baz does not override A::Baz
};
```

### Default functions
A more elegant, efficient way to provide a default implementation of a function, such as a constructor.
```c++
struct A {
  A() = default;
  A(int x) : x(x) {}
  int x{ 1 };
};
A a{}; // a.x == 1
A a2{ 123 }; // a.x == 123
```

With inheritance:
```c++
struct B {
  B() : x(1);
  int x;
};

struct C : B {
  // Calls B::B
  C() = default;
};

C c{}; // c.x == 1
```

### Deleted functions
A more elegant, efficient way to provide a deleted implementation of a function. Useful for preventing copies on objects.
```c++
class A {
  int x;

public:
  A(int x) : x(x) {};
  A(const A&) = delete;
  A& operator=(const A&) = delete;
};

A x{ 123 };
A y = x; // error -- call to deleted copy constructor
y = x; // error -- operator= deleted
```

### Range-based for loops
Syntactic sugar for iterating over a container's elements.
```c++
std::array<int, 5> a{ 1, 2, 3, 4, 5 };
for (int& x : a) x *= 2;
// a == { 2, 4, 6, 8, 10 }
```

Note the difference when using `int` as opposed to `int&`:
```c++
std::array<int, 5> a{ 1, 2, 3, 4, 5 };
for (int x : a) x *= 2;
// a == { 1, 2, 3, 4, 5 }
```

### Special member functions for move semantics
The copy constructor and copy assignment operator are called when copies are made, and with C++11's introduction of move semantics, there is now a move constructor and move assignment operator for moves.
```c++
struct A {
  std::string s;
  A() : s("test") {}
  A(const A& o) : s(o.s) {}
  A(A&& o) : s(std::move(o.s)) {}
  A& operator=(A&& o) {
    s = std::move(o.s);
    return *this;
  }
};

A F(A a) {
  return a;
}

A a1 = F(A{}); // move-constructed from rvalue temporary
A a2 = std::move(a1); // move-constructed using std::move
A a3 = A{};
a2 = std::move(a3); // move-assignment using std::move
a1 = F(A{}); // move-assignment from rvalue temporary
```

## C++11 Library Features

### std::move
`std::move` indicates that the object passed to it may be moved, or in other words, moved from one object to another without a copy. The object passed in should not be used after the move in certain situations.

A definition of `std::move` (performing a move is nothing more than casting to an rvalue):
```c++
template <typename T>
typename remove_reference<T>::type&& move(T&& arg) {
  return static_cast<typename remove_reference<T>::type&&>(arg);
}
```

Transferring `std::unique_ptr`s:
```c++
std::unique_ptr<int> p1{ new int };
std::unique_ptr<int> p2 = p1; // error -- cannot copy unique pointers
std::unique_ptr<int> p3 = std::move(p1); // move `p1` into `p2`
                                         // now unsafe to dereference object held by `p1`
```

### std::forward
Returns the arguments passed to it as-is, either as an lvalue or rvalue references, and includes cv-qualification. Useful for generic code that need a reference (either lvalue or rvalue) when appropriate, e.g factories. Forwarding gets its power from _template argument deduction_:
* `T& &` becomes `T&`
* `T& &&` becomes `T&`
* `T&& &` becomes `T&`
* `T&& &&` becomes `T&&`

A definition of `std::forward`:
```c++
template <typename T>
T&& forward(typename remove_reference<T>::type& arg) {
  return static_cast<T&&>(arg);
}
```

An example of a function `wrapper` which just forwards other `A` objects to a new `A` object's copy or move constructor:
```c++
struct A {
  A() = default;
  A(const A& o) { std::cout << "copied" << std::endl; }
  A(A&& o) { std::cout << "moved" << std::endl; }
};

template <typename T>
A wrapper(T&& arg) {
  return A{ std::forward<T>(arg) };
}

wrapper(A{}); // moved
A a{};
wrapper(a); // copied
wrapper(std::move(a)); // moved
```

### std::to_string
Converts a numeric argument to a `std::string`.
```c++
std::to_string(1.2); // == "1.2"
std::to_string(123); // == "123"
```

### Type traits
Type traits defines a compile-time template-based interface to query or modify the properties of types.
```c++
static_assert(std::is_integral<int>::value == 1);
static_assert(std::is_same<int, int>::value == 1);
static_assert(std::is_same<std::conditional<true, int, double>::type, int>::value == 1);
```

### Smart pointers
C++11 introduces new smart(er) pointers: `std::unique_ptr`, `std::shared_ptr`, `std::weak_ptr`. `std::auto_ptr` now becomes deprecated and then eventually removed in C++17. `std::unique_ptr` is a non-copyable, movable smart pointer that properly manages arrays and STL containers.
```c++
std::unique_ptr<Foo> p1(new Foo);  // `p1` owns `Foo`
if (p1) p1->Bar();

{
  std::unique_ptr<Foo> p2(std::move(p1));  // Now `p2` owns `Foo`
  F(*p2);

  p1 = std::move(p2);  // Ownership returns to `p1` -- `p2` gets destroyed
}

if (p1) p1->Bar();
// `Foo` instance is destroyed when `p1` goes out of scope
```

### std::chrono
The chrono library contains a set of utility functions and types that deal with _durations_, _clocks_, and _time points_. One use case of this library is benchmarking code:
```c++
std::chrono::time_point<std::chrono::system_clock> start, end;
start = std::chrono::system_clock::now();
// Some computations...
end = std::chrono::system_clock::now();

std::chrono::duration<double> elapsed_seconds = end-start;

elapsed_seconds.count(); // t number of seconds, represented as a `double`
```

### Tuples
Tuples are a fixed-size collection of heterogeneous values. Access the elements of a `std::tuple` by unpacking using `std::tie` (see section), or using `std::get`.
```c++
// `playerProfile` has type `std::tuple<int, std::string, std::string>`.
auto player_profile = std::make_tuple(51, "Frans Nielsen", "NYI");
std::get<0>(player_profile); // 51
std::get<1>(player_profile); // "Frans Nielsen"
std::get<2>(player_profile); // "NYI"
```

### std::tie
Creates a tuple of lvalue references. Useful for unpacking `std::pair` and `std::tuple` objects. Use `std::ignore` as a placeholder for ignored values. In C++17, structured bindings should be used instead.
```c++
// With tuples...
std::string player_name;
std::tie(std::ignore, player_name, std::ignore) = std::make_tuple(91, "John Tavares", "NYI");

// With pairs...
std::string yes, no;
std::tie(yes, no) = std::make_pair("yes", "no");
```

### std::array
`std::array` is a container built on top of a C-style array. Supports common container operations such as sorting.
```c++
std::array<int, 3> a = {2, 1, 3};
std::sort(a.begin(), a.end()); // a == { 1, 2, 3 }
for (int& x : a) x *= 2; // a == { 2, 4, 6 }
```

### Unordered containers
These containers maintain average constant-time complexity for search, insert, and remove operations. In order to achieve constant-time complexity, sacrifices order for speed by hashing elements into buckets. There are four unordered containers:
* `unordered_set`
* `unordered_multiset`
* `unordered_map`
* `unordered_multimap`

### Memory model
C++11 introduces a memory model for C++, which means library support for threading and atomic operations. Some of these operations include (but aren't limited to) atomic loads/stores, compare-and-swap, atomic flags, promises, futures, locks, and condition variables.

## Acknowledgements
* [cppreference](http://en.cppreference.com/w/cpp) - especially useful for finding examples and documentation of new library features.
* [C++ Rvalue References Explained](http://thbecker.net/articles/rvalue_references/section_01.html) - a great introduction I used to understand rvalue references, perfect forwarding, and move semantics.
* [clang](http://clang.llvm.org/cxx_status.html) and [gcc](https://gcc.gnu.org/projects/cxx-status.html)'s standards support pages. Also included here are the proposals for language/library features that I used to help find a description of, what it's meant to fix, and some examples.
* [Compiler explorer](https://godbolt.org/)
* [Scott Meyers' Effective Modern C++](https://www.amazon.com/Effective-Modern-Specific-Ways-Improve/dp/1491903996) - highly recommended book!
* [Jason Turner's C++ Weekly](https://www.youtube.com/channel/UCxHAlbZQNFU2LgEtiqd2Maw) - nice collection of C++-related videos.
* [What can I do with a moved-from object?](http://stackoverflow.com/questions/7027523/what-can-i-do-with-a-moved-from-object)
* [What are some uses of decltype(auto)?](http://stackoverflow.com/questions/24109737/what-are-some-uses-of-decltypeauto)
* And many more SO posts I'm forgetting...

## Author
Anthony Calandra

## License
MIT
