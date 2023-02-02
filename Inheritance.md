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

## Inheritance

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
        

                