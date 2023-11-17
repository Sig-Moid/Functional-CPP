# Concept
The purpose of this programme is to allow one to write C++ code with functional features (lambdas, first-class functions, currying) without ugly and complex syntax, as is required by the "functional" header included by most C++ compiles.

This provides features including a function type, lambda expressions, currying, and first-class functions. Simply write your code with these features, and run it through the `interpret` function. It will output your code, but as normal C++ code using the "functional" header. This has interop with UE5 and other C++-based engines, so long as those engines don't reserve the keywords "global" or "lambda."

# Syntax
To declare a function, write `func [fName] = lambda([type] [argumentName]).[expression];` where  `[fname]` is the name of your function, `[type]` is the input type, `[argumentName]` is the bound variable, and `[expression]` is the return value of your lambda. You can have multiple bound variables, seperated by `,` symbols. To call a function `fName`, merely write `fName(params)` where "parameters" are the inputs to `fName`. Currying isn't implemented by default, but can be added by returning a lambda expression from a lambda expression.

To pass in a lambda to another lambda, or a procedure, declare it's type as `lambda<[outputType] of [paramTypes]>` where `[outputType]` is the return type of the lambda, and `[returnTypes]` lists the types of the lambda inputs.

Note that this only works if you include the `<functional>` header and `std` namespace, as several of its features are required for this implementation. However, if you want to avoid using the `std` namespace, you can write the lambda type (`lambda<a of b>`) with the `std::` scoping (`std::lambda<a of b>`). Beyond that, the rest of the syntax remains the same.

C++ requirs special capture limitations for globally scoped lambdas, so for non-local lambdas, you must use the keyword "global" before the lambda declaration.

# Example Programme
This exemplary programme should serve as a useful reference. It demonstrates every feature of the program.
```
#include <functional>
#include <iostream>
using namespace std;

func run = global lambda(int x, lambda<int of int> f).f(x);
func add = global lambda(int a).lambda(int b).(a + b);

int main() {
	func addTwo = lambda(int x).add(2)(x);
	cout << addTwo(2);
}
```
Do note, however, that the code generated can be quite ugly. For example, the above program transpiles to
```
#include <functional>
#include <iostream>
using namespace std;
auto run =  [](int x, function<int(int)> f) {
        return f(x);
};

auto add =  [](int a) {
        return [=](int b) {
        return (a + b);
};

};


int main() {
        auto addTwo = [=](int x) {
        return add(2)(x);
};

        cout << addTwo(2);
}
```
# Planned Features
I'm planning on improving the aesthetics of generated code, and adding lambda calls (eg. `lambda(int x).(x+2) (2)` should return `4`).
