## Java Fundamentals

### The Basics

#### Java Runtime Environment (JRE) vs Java Development Kit (JDK)

- JRE is required to run Java apps, end users normally require only the JRE
- JDK provides tools required to create Java apps, developers normally require the JDK, which includes JRE

#### The workflow of Java

1. First we write the Java source code
2. We then use JDK to compile the source code into platform-independent byte codes
3. End-user can then use JRE to run the byte codes on a host environment, such as Windows, Linux, Mac, Browser or Android

#### Run Java in command line

First check you have Java installed properly

- `javac fileName` (.java) to compile the source code
- `java fileName` (.class) to run the Java program

#### Package Name Convention

Mostly we use reversed domain name + project name, such as `com.dennisxiao.todo`. In addition, most IDEs force us to have a folder structure that matches the package name.

#### Java Data Types

Primitive data types are stored by value.

- Integer: bypte, short, int, long
- Floating Point: float (26.2f), double (0.0001d)
- Character: char ('U' | '\u00DA')
- Boolean: boolean (true | false)

#### Type Conversion

Type conversion can be performed explicitly with cast operator.

long lVal = 50;
int iVal = (int) lVal;
