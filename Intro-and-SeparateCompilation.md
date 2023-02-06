# Introduction & Separate Compilation

### Algorithm 

A set of instructions/steps that solves a particular problem 

- Each algorithm operates on *input data* and produces an *output result* 

- Can be classified by how long it takes to run on a particular input & by the quality of results

### Data Structures 

The set of variables(s) that an algorithm uses to solve a problem

### The Abstract Data Type 

A coordinated group of data structures, algorithms and interface functions 

- In an ADT, the data structures and algorithms are secret 

- The ADT provides an interface (a simple set of functions) to enable the rest of the program to use it 

- C++: uses classes to define ADTS 

    - Each C++ class holds data structures, algorithms, and interface functions 

        ```
        class DNADatabase
        {
            public: 
            //our interface functions go here 
            void findSequence(...); 
            void addSequence(...); 

            private:
                //secret algorithms go here 
                //secret data structures go here 
        }
        ```
    - The rest of the program just need to use the class's public interface 

### Object Oriented Programming (OOP) 

A programming model based on the ADT concept

- Programs are constructed from multiple self-contained classes 

- Each class holds a set of data structures & algorithms

    - Access the class using set of interface functions 

- Classes talk to each other only by using public interface functions 

### Define a Class 

Write class shell, public interface, and private variables and functions 
```
class Nerd
{
    public: 
        Nerd(int stink, int IQ) 
        {
            myStinkiness = stink; 
            myIQ = IQ: 
        }

        void study(int hours)
        {
            myStinkiness += 3*hours; 
            myIQ *= 1.01; 
        }
    private:
        int myStinkiness, myIQ; 
}
```
Instantiation 
```
#include "nerd.h" 
int main()
{
    int num_nerds = 1; 
    Nerd david(30, 150); 
    david..study(10); 
}
```
- When you define your `david` variable, it gets its own copy of all the functions and member variables defined in your class

- A class's primitive member variables (e.g. ints, duobles) all start out with random values 

    - Constructor then initializes the variables 

Member variables are used to store permanent attributes of the class 

- `Stinkiness` and `IQ` are inherent attibutes of every `Nerd` 
---
## Constructors, Destructors 

### **Constructor**

Used to reset an object's member variables when the object is first created; otherwise random 

Example: Gassy People 

    ```
    class Gassy 
    {
        public: 
            Gassy(int age, bool ateBeans)
            {
                m_age = age; 
                m_ateBeans = ateBeans; 
            }

            int getFartsPerHr()
            {
                if (m_ateBeans == true)
                {
                    return 100; 
                }
                return (3 * m_age); 
            }
        private: 
            int m_age; 
            bool m_ateBeans; 
    }; 
    ```

- If a constructor has parameters -> **must pass values** 

    ```
    int main()
    {
        Gassy betty(18, true); //OK 
        Gassy alan; //NOT OK 
    }
    ```
- **Constructor Overloading**: Class can have many different constructos 

    - Each *must* have different parameters and/or types 

    ```
    Gassy()
    {
        m_age = 0; 
        m_ateBeans = false; 
    }
    ```
    - Now we can pass no parameters 

- Implicit default constructor: if you don't create ANY constructor, C++ generates one for you 

    - Does NOT initializes primitive member variables 

    ``` 
    Gassy()
    {}
    ```

When Constructors Are Called...

- Called any time you create a **new variable** of a class 

- Called *N* times when you create an array of size *N*

- Called when you use `new` to dynamically allocate a new variable 

- If variable declared in a loop -> newly constructed during each iteration 

- **NOT CALLED** when you define a pointer variable to this type

**Default Parameters** 
```
String::String(const char* value = "")
{
    ...
}
```
- If a constructor *can be called with no arguments*, then it is a default constructor 

- Only specific in declaration, not implementation

### **Destructor**

An object will often reserve memory slots from the operating system while it runs 

A destructor guarantees that reserved memory is freed when an object goes away

```
~Gassy()
{
    //destructor code 
}
```
- Must NOT have any parameters

- Must NOT return a value 

- Implicit default destructor: C++ will define a destructor that ensures objects properly go away when they go out of scope

Scenarios that need destructor: any time a class allocates a system resource

- Reserve memory with `new` 

    - Free allocated memory with `delete`

- Open a disk file 

    - Close the disk file 

- Connect to another computer over the network 

    - Disconnect from other computer

When is a destructor called 

- Local objects destructed when they go out of scope 

- Local variables defined in a block destructed when the block finishes 

- Dynamically allocated variables destructued ONLY when `delete` is called 

- Called *N* times when you define an array of *N* items at the end of scope

---
## Class Composition 

When a class contains one or more member variables that are also classes

**Compiler Step for Constructing an Object**

- Data members are always initialized first 

    - Adds code to outer class's constructor to first call the **default** constructors of all member objects 

    ```
    HungryNerd()
        Call myBelly's default constructor 
        Call myBrain's default constructor 
    {
        myBelly.eat(); 
        myBrain.think(); 
    }
    ```
- After type data members are constructed... 
    
    - C++ executes body of outer object's constructor 

1. --
2. Construct data members 
    - consult the member initialization list 
    - if a data member is not listed in the initializationlist: 

        - if builtin type: left uninitialized (theninitialize in body) 
        - if class types: the class's default constructoris called  
3. Execute the body of the constructor 

**Compiler Step for Destruction**

- Object goes out of scope -> destructor called 
     
     ```
     int main()
     {
        HungryNerd carey; 
     } <- carey's destructor is called 
     ```

    - Carey's destructor (outer destructor ran first) 

        ```
        ~HungryNerd()
        {
            myBelly.eat(); //last meal
            myBrain.think(); //last thought 
        }
        ```
- Call destructor of member variables in reverse order of construction 

    ```
    ~HungryNerd()
    {
        myBelly.eat(); //last meal
        myBrain.think(); //last thought 
        //Member variables can still be used here 
    }  
    Call myBrain's destructor 
    Call myBelly's destructor  

### **Member Initialization List**

> Required when a class contains objects without a default constructor 

- Auto-generated C++ code always calls the default constructor of member variables (takes NO parameters) 

    - If you have a defined constructor -> will not be called -> BAD NEWS! 

    - **Compilation error** 

- Initialization list: code that explicitly calls member variable constructors WITH a parameter 

    ```
    Stomach(int startGas)
    {
        myGas = startGas; 
    }

    HungryNerd()
        : myBelly(10)
        Call myBrain's default constructor 
    {
        myBelly.eat(); 
        myBrain.think(); 
    }
    ```
Initializer List Accepts Constants & Variables 
```
HungryNerd(int startingGas)
    : myBelly(startingGas)
    Call myBrain's default constructor 
{
    myBelly.eat(); 
    myBrain.think(); 
}
```

Purpose of Initialize List 
    
- **Performance**: initialize primitive variables, string m_name` is a class type

    - if initialize in body, default constructor called irst and initializes it to empty string, then nitialized in body --> WASTE 
    - instead: use initializer list 

- **Mandatory**: head of stick figure won't compile if nitialize everything in body: **the circle has no default onstructor**

Further Example: 

```
class Circle
{
    public:
        Circle(double x, double y, double r);
    private:
        //Class invariant
        double m_x; 
        double m_y; 
        double m_r;
}

Circle::Circle(double x, double y, double r)
    : m_x(x), m_y(y), m_r(r) //no need to initialize these in the body
{
    if (r <= 0)
    {
        cout << m"Cannot create a circle with radius" << r << endl;
        exit(1);
    }
}
```
Example: Stick Figures 

```
class StickFigure
{
    public: 
        StickFigure(double body1, double hd, strinnm, double head, double heady);
    private: 
        string m_name; 
        Circle m_head; 
        double m_bodyLength; 
};
StickFigure:: StickFigure(double body1, double hdstring nm, double headx, double head y)
{
    if (b1 <= 0 || hd <= 0)
        ...error...
    m_name = nm; 
    m_bodyLength = b1; 
    /* 
    Initialize head 
    m_x = x; 
    m_y = y; 
    m_r = r;
    THIS DOES NOT COMPILE: the data members arprivate to Circle 
    */ 
```
```
StickFigure:: StickFigure(double body1, double hd, string nm, double headx, double head y)
    : m_name(nm), m_head(hx, hy, hd/2)
    {
        if (b1 <= 0 || hd <= 0)
            ...error...
        m_bodyLength = b1; 
    }

```
---
## Program Organization && Separate Compilation 
- Separate compilation for performance reasons 

    - Header files store declarations `.h`

        -public interface + private data members 
    
    - `.cpp` files for implementation 

> For each source file (.cpp) in a project, the compiler produces an object file (.o) containing: (1) the machine language translation of the code for each function; (2) storage for global objects (e.g. std::cout) (3) a list of global names defined (i.e. implemented) by this object file; (4) a list of global names used in this file that need a definition somewhere 

> The linker brings all these objects files together, along with needed object files that are part of the library, to produce an executable file. 

> Rules: (1) Nothing can be defined more than once (2) Every need must be satisfied by some definition (3) There must be (exactly) one main routine 

- Compilation

    - Source file `Circle.cpp` and  `myapp.cpp` 

    - Translate each `.cpp` file into machine language code (`.o` object files)

        - `Circle.o` and `myapp.o` 

    - Compiler also has two tables: names of things that the object files defines (functions) and names of things it needs 

        | Define | Needs | 
        | -------| ------|
        | Circle:: Circle | exit 
        | area | atan | 

        | Define | Needs | 
        | --- | -- | 
        | main | Circle:: Circle 
        |       | area 
        |       | cout | 
        |       | << | 

    - pulls definitions from the library that defines a bunch of things 
    - .o files and library linked by linker, produces executable file 

- Don't put implementations of functions in header files 

    - Suppose we put implementation in header file --> linker error 

        - implementation will be compiled in `Circle.cpp` as well as `myapp.cpp`

- You don't include `.cpp` files 

    - Double implementation 

- Standard header files 

    - All standard headers are protected against being included more than once in the same files 

        - You can `#include <string>` in both the `.h` and `.cpp` file 

- `Using namespace std;` 

    - Do not put this in the header file generally

        - There is no way to turn it off when you put this in the header file 

    - No issue in `.cpp` files

        - `.cpp` files are not included anywhere else

## More Issues with Separate Compilation...
---
## Include Guards 

> Prevents double declaration/include of header files e.g. main.cpp includes app.h and student.h while student.h also includes app.h (two app defs in main-> error) 

```
Point.h 
class Point 
{}; 

=== 
Circle.h 
class Circle 
{
    ...
    private: 
        Point m_center; 
        double m_radius; 
}

=== 
myapp.cpp 
#include "Circle.h" 
int main() 
{
    Circle c(-2, 5, 10); 
    ...
}
//This will not compile, because Circle.h does not know what Point is, we should include point.h in the circle file 
```
```
=== 
Circle.h 

#include "Point.h" 
class Circle 
{
    ...
    private: 
        Point m_center; 
        double m_radius; 
}
```
> But what if the user also wants to use a point? 
- Do NOT try switching order of `#include` in main routine file 
### Include Guarding 
```
Point.h 

//surround everything with included guard
//if the symbol has not yet been defined, then define it, then end 

#ifndef POINT_INCLUDED 
#define POINT_INCLUDED //this is just a symbol 

class Point 
{
    ...
}; 

#endif //POINT_INCLUDED 
```

- Compiler asks: has this symbol been defined yet? If not --> mark defined, if yes, skip 
- This will let the main routine know what the point is no matter how 
```
main 
#include "Circle.h" 
#include "Point.h" //by the time the compiler gets here, point.h has already been defined once, so it sees it, sees its marked defined, then skips everything 
```
- **Everything significant** should be inside the includ guard 
- Should be done with **every header file** 
- Symbol should be different from any other symbol 
- All standard headers have include guards 
---
## Circular Dependencies 
``` 
Student.h 

#ifndef STUDENT_INCLUDED 
#define STUDENT_INCLUDED 

#include "Course.h" //compiler first goes to Course.h for Course in this file

class Student 
{
    ...
    void enroll(Course* cp); 
    Course* m_studyLIst[10]; 
}; 

#endif 

Course.h 

#ifndef COURSE_INCLUDED 
#define COURSE_INCLUDED 

#include "Student.h" //as compiler comes here and needs Student, goes to Student.h again, thinks its already seen the file (include guard), skips everything, we never actually process the Student.h file  

class Course 
{
    int units() const; 
    Student* m_roster[1000]; 
}; 

#endif 

main.cpp 

#include "Student.h" 
void f(Student* s, Course* cp)
{
    s->enroll(cp); 
}

```
- Student depends on Course, and Course depends on student --> PROBLEM!!! 

 ### **Incomplete Type Declaration** 
 ```
 class A; //this exists as a type, but no details 

 //later on define the class somewhere else 
```
> Why does the compiler not need any details? 

- Compiler needs to figure out the **size** of objects: how big a Student is (an array of 10 Course pointers)

    - In C++, all pointers are 4 bytes long 
    - So the compiler knows the size without any other details 
    - `studyList` is 40 bytes long 
    - Compiler only needs to know name of the type to know the size of something 

```
Student.h 

#ifndef STUDENT_INCLUDED 
#define STUDENT_INCLUDED 

class Course; 
...

Course.h 

...
class Student; 
```
- In main routine: cannot do incomplete type declaration 

    - because we are depending that we know Student has a member function called `enroll` 
    - for class Course, we can do  incomplete type (because it only takes a pointer)

    ```
    #include "Student.h" 
    class Course; //could also just say #include, since .cpp file  

    void f(Student* s, Course* cp)
    {
        s->enroll(cp); 
    }
    ```

### **Basically...**

>If the file Foo.h defines the class Foo, when does another file you to say `#include "Foo.h" and when can you do incomplete type declaration? 

You have the `#include` the header file defining a class when:

- you declare a data member of that class type 

    ```
    FirstClass y; 
    ```

- you declare a container (e.g. an array/vector) of objects of that class type 

    ```
    FirstClass b[10]; 
    ```
- you create an object of that class type 
- you use a member function of that class type
- you use the variable in any way (call a method on it, return it, etc.)

    ```
    y.someFunc(); 
    return(y); 
    ```

If all the file needs to know is the *name* of the type, then you can do incomplete type declaration 

- When you use the class to define a parameter to a function 

    ```
    void goober(FirstClass p1); 
    ```

- When you use the class as the return type for a function 

    ```
    FirstClass hoober(int a); 
    ```
- When you use the class to define a pointer or reference variable 

    ```
    void joober(FirstClass &p1); 
    void koober(FirstClass *p1); 
    
    void loober()
    {
        FirstClass *ptr; 
    }

    FirstClass *ptr1, *z[10]; 
    ```

Example: 

```
class Blah 
{
    void g(Foo f, Foo& fr, Foo* fp); //incomplete, just prototype of function , even if passing by value is copying (not in prototype)
    Foo* m_fp; //incomplete, since only pointer and we know size 
    Foo* m_fpa[10]; //incomplete
    vector<Foo*> m_fpv; //incomplete 

    Foo m_f; //Must include 
    Foo m_fa[10]; //Must include
    vector<Foo> m_fv; //Must include
}; 

void Blah::g(Foo f, Foo& fr, Foo* fp)
{
    Foo f2(10,20); //must include, declaring local data member 
    f.gleep(); //must include 
    fr.gleep(); //must include 
    fp-> gleep(); //must include 
}
```
> Some illegal stuff: Circular dependencies with actual objects... 
```
Struct A
{
    B b; //for this to compile, compiler has to see full declaration of B
}; 

Struct B 
{
    int i; 
    A a; //but if we put B above A the same problem will occur here 
}; 
```
This is **illegal** anyways!!!
``` 
A a; 
```
This `a` object contains a B object, which has an integer and an `a` object, infinity...

How big is this `a` object? 

size of A = size of B 

size of B = 4 (int) + size of A 

hence size of A = 4 + size of A --> infinite!!!

---
### Default Arguments 
```
void fart(int length = 10, int volume = 50)
{
    ...
}

int main()
{
    fart(20, 5); 
    fart(5); 
    fart(); 
    fart(, 30); //ILLEGAL 
}
```
> If the *j*th parameter has a default value, then ALL of the following parameters (*j+1* to *n*) must also have default values
        