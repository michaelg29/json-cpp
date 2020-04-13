# json-cpp
Lightweight JSON parser and writer for C++

- [Installation](#installation)
- [Usage](#usage)
	- [Loading Data](#loading-data)
	- [Editing Data](#editing-data)
	- [Outputting Data](#outputting-data)

## installation
There are two ways to include this library.
1) Drag the contents of the *include* directory to the *include* directory of your project.
2) Or, simply place the *json.hpp* file in your project with the other source files.

And then in your project header or source files, use the following include statement:
```cpp
#include <jsoncpp/json.hpp> // if the library is in an include directory
#include "jsoncpp/json.hpp" // if the library is with the project source files
```

## usage
### loading data
You can load data in 3 ways:
1) Parse the data from a string:
```cpp
jsoncpp::parse("{\"employees\": []}");
```
2) Load the data from a file:
```cpp
jsoncpp::read("data.json");
```
3) Initialize a json object in code
```cpp
jsoncpp::json d = {
    { "employees", {
    	{
        	{ "name", "Michael" },
            { "age", 32 },
            { "manager", true }
        }
    }}
};

/*
This would translate to in JSON:
{
    "employees": [
    	{
        	"name": "Michael",
            "age": 32,
            "manager": true
        }
    ]
}
*/
```
*note that key-value pairs are separated by commas rather than colons since a pair in C++ can be represented as a list of two values*

### editing data
You can use the assignment operator to set values.
```cpp
// strings
jsoncpp::json s = "This is a string";

// numbers
jsoncpp::json d = 3.1415;
jsoncpp::json i = 3;
jsoncpp::json f = 3.1415f;

// boolean
jsoncpp::json b = true;

// lists
jsoncpp::json l = {
    5,
    "hello",
    false,
    {
    	6,
        true
    }
};

/*
This would translate to in JSON:
[
    5,
    "hello",
    false,
    [
    	6,
        true
    ]
]
*/

// objects (dictionaries)
jsoncpp::json o = {
    { "employees", {
    	{
        	{ "name", "Michael" },
            { "age", 32 },
            { "manager", true }
        }
    }}
};
```

Using the operators + and += are defined for strings, numbers, and lists.
```cpp
// strings
jsoncpp::json s = "Hello,";
jsoncpp::json s2 = " world";
jsoncpp::json s3 = s + s2;
s3 += "!"; // s3 = "Hello, worlds"

// numbers
jsoncpp::json d = 0;
for (int i = 1; i < INT_MAX; i++) {
	d += 4 * pow(-1, i + 1) / (2 * i - 1);
}
// d = 3.1415...

// lists
jsoncpp::json l = { 1, 2, 3, 4 };
l += 5; // l = { 1, 2, 3, 4, 5 }
// in order to add all elements of a list to a list
l.addAll({6, 7, 8, 9, 10}); // l = { 1, ..., 10 }
```

Using the operator [] is defined for lists and objects
```cpp
jsoncpp::json l = { 1, 2, 3, 4 };
l[3] = 18; // l = { 1, 2, 3, 18 };

jsoncpp::json d = {
	{ "name", "Michael" }
};
d["name"] = "Stalin";
d["age"] = INT_MAX;
//d = { "name": "Stalin", "age": 2147483647 }
```

### outputting data
To access the value of a json class:
```cpp
// string
jsoncpp::json s = "Hello";
s.val<std::string>(); // "Hello";

// number
jsoncpp::json n = 3.1415;
n.val<double>(); // 3.1415
n.val<int>(); // 3
n.val<float>(); // 3.1415f

// bool
jsoncpp::json b = true;
b.val<bool>(); // true
```

To get the string representation of a json class:
```cpp
jsoncpp::json o = {
    { "employees", {
    	{
            { "name", "Michael" },
            { "age", 32 },
            { "manager", true }
        }
    }}
};

// to get the string without spacing
o.dump(); // {"employees":[{"name":"Michael","age":32,"manager":true}]}

// to pretty print
o.dump(4); // 4 is the tab spacing
/*
{
    "employees": [
    	{
        	"name": "Michael",
            "age": 32,
            "manager": true
        }
    ]
}
*/
```

To write to a file:
```cpp
jsoncpp::json o;

// without pretty printing
o.dump("data.json");

// with pretty printing
o.dump("data.json", 4);
```