## Introduction 

### Scenario: Write a system that will let me draw pictures 

- Different shapes: circle, line segments, triangles, etc. 

- Assume that different shapes will have their separate properties -> willbe different classes 

- Picture is a collection of all these shapes 

- Pseudocode: 

    - pic (empty to start out) 
    - add a Circle to pic 
    - add a Rect to pic 
    - add a Cirlce to pic 

    - *Does your programming language allow you to use all theses differenttype?* 

        - Some languages do, some don't 

            - Interpreter languages know type only during run time  

            - Compile-time langauges: compile-time type checking 

                - What type is my picture variable? It's a collection o *shapes*? 
 
- Demo Code

    ```
    class Circle 
    {
        void move(double xnew, doubleynew); 
        void draw() const; 
        double m_x; 
        double m_y; 
        double m_r; 
    };

    class Rectangle
    {
        void move(double xnew, double ynew); 
        void draw() const; 
        double m_x; 
        double m_y; 
        double m_dx; 
        double m_dy; 
    };

    Circle* ca[100]; 
    Rectangle* ra[100]; 

    //Repeat all this code, so many repeated code...
    void f(Circle& x)
    {
        x.move(10, 20); //we need a different version of move for each shape
        x.draw(); 
    }

    void f(Rectangle& x)
    {
        x.move(10, 20); 
        x.draw(); 
    }

    Circle c; 
    c.move(15, 5); 
    c.draw(); 
    f(c); 

    ca[0] = new Circle; 
    ra[0] = new Rectangle; 
    ca[1] = new Circle; 

    for (int k = 0; ...; k++)
    {
        ca[k] -> draw(); 
    }
    for (int k = 0; ...; k++)
    {
        ra[k]->draw(); 
    }
    ```

- How to get rid of repeated code? 

    - Have only one array `??? pic[100];`

    - Have only one `f` function `void f(???& x)` 

    - Idea: `pic` is a collection of `shape`

        - A `circle` is a kind of `shape`, and a `rectangle` is a kind of`shape` 

- One type and another type are all kinds of some other type...

- Introduce the type `shape`

    - `pic` array will hold shapes

    - Can create any shapes in dynamic allocation 

    ```
    class Shape 
    {

    }; 

    class Circle : public Shape // A Circle is a kind of Shape 
    {

    }; 

    class Rectangle : public Shape // A Rectangle is a kind of Shape 

    Shape* pic[100]; 

    pic[0] = new Circle; 
    pic[1] = new Rectangle; 
    pic[2] = new Circle; 
    ```

- Relationship between `Shape` and the actual shape classes: 

    `Shape` : generalization 

    `Circle` and `Rectangle`: specialization

    - `Shape`: **base class**

    - `Circle`: **derived class**

---

## Single Inheritance

- **Heterogeneous Collection**: Pointer to a derived object can be automatically converted to a base object 

    ```
    Derived* ==> Base* 
    Derived& ==> Base&
    ``` 
    - An array of the base class can store objects of the derived class

    - If I say you can bring me a mammal, you can bring me a dog 

        - If I say you can bring me a Shape, you can bring me a Circle 

    - The opposite does NOT work 

        - If I say you can bring me a dog, you can't bring me other mammals! 

    - Underlying Compiler Logic 
        - Logic 1

            ![picture 2](images/2d7279076590f0f096e3e4b90e69174b000f753620bec9258982e1678fa0492f.png) 

            - Dynamically allocated collection of different typees 

            - Not right 

                - If these are `Shape*` pointers, they are NOT pointing to actual Shapes

        - Actual implementation 

            - The memory representation of every object of the derived type has an object of the base type embedded inside it 

                - Every `Circle` object will have embedded inside it a `Shape` object 

            - `pic[0] = new Circle` 

                - `new Circle` gives a pointer to the `Circle` object 

                - This pointer is converted to a pointer to a `Shape`, then assigned to `pic` array

                - Implementation: Compiler sees this, every Circle object will have a Shape object inside it 

                    - the Shape part of every Circle could be e.g. always 4 bytes down from the top of the object 

                    - Compiler generate code: 

                        - take the address of this circle, add 4 bytes to it, store into pic array 
                    
                    - Compiler wants this conversion to be CHEAP

                        - Add constant to an address is cheap already

                        - But if constant = 0 -> cheaper (no instructions) 

                            - Most compiler will put the base object at the top of the derived object 

                            The start of the base object inside is the same as the satrt of the derived object 
                
                - The pointer that is assigned to the `pic` array is *actually pointing to the Shape object inside the circle* 

                    ![picture 4](images/be2402b426d95e814383a170edfeae0a54c7df746e87210c01db5f9c17dcb8c2.png)  


- What is common for all derived objects -> base type 

    - All `Shapes` can moved, can be drawn, have `x` and `y` 

        ```
        class Shape
        {
            void move(double xnew, double ynew); 
            void draw() const; 
            double m_x; 
            double m_y; 
        }

        class Circle: public Shape //in addition to all properties of shape, has this
        {
            double m_r; 
        }

        class Rectangle : public Shape 
        {
            double m_dx; 
            double m_dy; 
        }
        ```
    
- Inheritance: The derived class inherits all properties of the base class 

    - Functions and data members 

### Implementations of Base and Derived Functions 

Consider this code... `move()` 
```
void Shape::move(double xnew, double ynew)
{
    m_x = xnew; 
    m_y = ynew; 
}

Circle c; 
c.move(15, 5); 
f(c); 
```
- Circle object `c` is declared 
- `c.move` will change the x and y coordinates of `c` 
- `f(c)` takes reference to a Shape object -> automatic conversion 

    - In the function, `x` will be a reference to the Shape part of the ciccle 

        - `x.move()` changes the x and y 

- This works perfectly! 

    - All it takes to move all shapes is moving the reference points, so there is no difference between moving any shapes, this implementation works 
- Same `move()` function for every type of Shape 

Consider this... `draw()` 

- Every `draw()` function is different 

```
class Circle : public Shape
{
    void draw() const; 
    double m_r; 
}; 

class Rectangle : public Shape
{
    void draw() const; 
    double m_dx; 
    double m_dy; 
}; 

void Circle::draw() const 
{
    ...draw a circle of radius m_r...
}

void Shape::draw() const 
{
    ...for now, just draw a vague cloud centered at m_x, m_y...
}

c.move(15, 5); 
```
- We have to implement a `Shape::draw()` function because we declared it

    - Even though we don't know how to draw just a shape

    - If we don't need to draw a shape, can we just not declare it in Shape? **NO**

        - `pic` holds a collection of Shapes, compiler does not know whether every Shape object can be drawn or not 

            - Due to separate compilation 

        - Code will not compile 


- `Circle::draw()` needs to override the `Shape` implemention 

### **Static Binding Versus Dynamic Binding**

- When the compiler sees that there is a base reference or pointer that calls a member function that each derived type has defined differently, how does it know which one to call? 

    - We only know which version to call at runtime since the array can have different types stored 

    - Decision: 

        Should the compiler compile code that will at compile-time already has built-into it exactly which function to call or should it compile code that will make a decision at runtime to decide which function to call?  

- Static binding 

    - Compile-time, bind which function body each call will execute

    - `move()`: the same across all  

    - `draw()` when something like `c.draw()` since caller is a Circle object, obvious to bind Circle's draw function 

- Dynamic binding 

    - Decide during runtime, compiler generate code that decide during execution which function body to call 

    - `draw()` when called through a pointer/reference 

        - when we don't know what kind of shape the caller is referring to

- Most programming languages do not have this issue 

    - Types are known at runtime 

    - Or does dynamic binding built-in 

        - In cases where it is not needed -> costly

    - In C++ static binding is the default 

        - For the `draw()` function: whenever any reference calls draw, Shape's draw() will be called -> PROBLEM! 

Ensuring Dynamic Binding... The `virtual` keyword 
```
class Shape
{
    void move(double xnew, double ynew); 
    virtual void draw() const; 
    double m_x; 
    double m_y;
}
```
- Keyword `virtual` tells compiler that this function will be dynamically bound 

    - Required only in the base class, but convention to write it in ALL classes 

        ```
        class Cirlce : public Shape
        {
            virtual void draw() const; 
            double m_r; 
        }
        ```

### **Calling Base Class Function from Derived Class Function** 

Consider this code... 

I create a new type `warningSymbol` that needs a new `move()` function 
```
void warningSymbol::move(double xnew, double ynew)
{
    Shape::move(xnew, ynew); 
    //or this->Shape::move(xynew, ynew); 
    ...flash three times...
}

warningSymbol ws; 
ws.move(15, 5); //static binding 
```

However...
```
void f(Shape& x)
{
    x.move(10, 20); 
}

f(ws);
```
- Warning symbol will not move correctly 

    - Will not flash when call through reference/pointer of base class 

    - Inconsistency! 

- This is because we overrided a non-virtual function: we have to change the `move()` function to `virtual` 

> **Never override a non-virtual function**







