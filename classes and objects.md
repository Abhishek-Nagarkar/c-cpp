# Classes & Objects
 ### Class vehicle contains data members -> engineNo, vCount, model, color
 ### getters/setters
 ### default constructor, param constructor, copy constructor, destructor
 ### display method
	// basics of classes and objects
	#include <iostream>
	#include <cstring>   // for string functions
	#include <string>    // for string dtype
	using namespace std; // avoid using in the marathon

	// creating a simple vehicle class
	class Vehicle
	{
	    // PRIVATE DATA MEMBERS
	    int engineNo; // engine number is private

		public:
		    static int vCount;

	    // PUBLIC DATA MEMBERS
	    char *model;  // must be allocated memory in the ctor using new; just a POINTER to a STRING
	    string color; // no need to allocate memory; ACTUAL STRING

	    // GETTERS AND SETTERS (for both private and public data members)
	    int getEngineNo() const { return engineNo; }
	    void setEngineNo(int engineNo_) { engineNo = engineNo_; }

	    char *getModel() const { return model; }
	    void setModel(char *model_) { model = model_; }

	    string getColor() const { return color; }
	    void setColor(const string &color_) { color = color_; }

	    // CONSTRUCTORS - DEFAULT, PARAMETERIZED, COPY
	    Vehicle();
	    Vehicle(int, const char *, string);

	    // DESTRUCTOR
	    ~Vehicle()
	    {
	        delete model;
	        cout << "DESTRUCTOR CALLED - " << engineNo << " address=" << this << endl;
	    }

	    // COPY CTOR
	    Vehicle(const Vehicle &); // passing const object reference
	    //  using const keyword so that the passed reference itself cannot be changed

	    // DISPLAY FUNCTION
	    void display();
	};

	// static variable initialization
	int Vehicle::vCount = 0;

	Vehicle::Vehicle() : // assigning data members using member initializer list
	                     engineNo(vCount), color("pearl white")
	{
	    vCount++;
	    model = new char[20];
	    strcpy(model, "suzuki");
	    // since "" strings are const char* we cannot directly assign model using = operator
	    // bcoz its a char* and not const char*
	    // we can make it const char* but then cannot modify it further
	    cout << "DEFAULT CTOR - " << engineNo << " address=" << this << endl;
	}
	// ctor definition
	Vehicle::Vehicle(int engineNo_, const char *model_, string color_)
	{
	    vCount++;
	    engineNo = engineNo_;
	    model = new char[20];
	    strcpy(model, model_);
	    color = color_;
	    cout << "PARAM CTOR - " << engineNo << " address=" << this <<  endl;
	}

	Vehicle::Vehicle(const Vehicle &v)
	{
	    engineNo = vCount;
	    model = new char[20];
	    strcpy(model, v.model);
	    color = v.color;
	    cout << "COPY CTOR - " << engineNo << " address=" << this<< endl;
	    vCount++;
	}

	void Vehicle::display()
	{
	    cout << "engNo=" << this->engineNo << " model=" << this->model << " color=" << this->color << " \n";
	}
---
	// for testing class codes
	int main()
	{
	    Vehicle v1;
	    return 0;
	}
	// output
    DEFAULT CTOR - 0 address=0x61ff00
	DESTRUCTOR CALLED - 0 address=0x61ff00 

-- In above main method, the v1 object is created on stack using default constructor. Destructor is called automatically in this case.

---
	int main()
	{
		Vehicle *v2 = new Vehicle();
		v2->display();
		cout << v2 << endl;
		cout << &v2 << endl;
		return 0;
	}
	// output
	DEFAULT CTOR - 0 address=0x14a1a38     
	engNo=0 model=suzuki color=pearl white 
	0x14a1a38
	0x61fefc
	
-- Here, the vehicle object is created on heap. Since v2 is a pointer to an object, we use arrow notation to access methods and data members.
-- You can notice that the address where pointer v2 points is `0x14a1a38` and v2 itself is stored at `0x61fefc`.
-- Use `delete v2` to delete the object ( destructor is called )

---
	int main()
	{
		Vehicle v1;
		Vehicle v3 = v1; // copy constructor called
		v1.display();
		v3.display();
	}
	// output
	DEFAULT CTOR - 0 address=0x61fef0      
	COPY CTOR - 1 address=0x61fed0
	engNo=0 model=suzuki color=pearl white 
	engNo=1 model=suzuki color=pearl white 
	DESTRUCTOR CALLED - 1 address=0x61fed0 
	DESTRUCTOR CALLED - 0 address=0x61fef0
-- Here v1 is created and used to create v3 object. Creation of v3 calls the copy constructor as shown in the output. Since both are created on stack, the destructors are called automatically.

---
	int main()
	{
		Vehicle *v1 = new Vehicle(100, "bmw", "red");
	    Vehicle v2 = *v1; // since v1 points to where the object is located, *v1 can be thought of like the object itself
	    v1->display();
	    v2.display();
	    delete v1;
	}
	// output
	PARAM CTOR - 100 address=0x1271a38       
	COPY CTOR - 1 address=0x61fec0
	engNo=100 model=bmw color=red
	engNo=1 model=bmw color=red
	DESTRUCTOR CALLED - 100 address=0x1271a38
	DESTRUCTOR CALLED - 1 address=0x61fec0 
-- V1 is created using param constructor. Since new is used, it is created on heap.
-- V2 is created using copy constructor. Here, since v1 is a POINTER to OBJECT, we are using *V1 to access the object stored in heap so that it can be used to create V2.
-- Again the destructor for V2 is called automatically.

---
	int main()
	{
		Vehicle  *v1  =  new  Vehicle(100, "bmw", "red");
		cout  <<  v1->color  <<  endl;
		cout  << (*v1).color  <<  endl;
		v1->display();
		(*v1).display();
	}
	// output
	PARAM CTOR - 100 address=0x971a38
	red
	red
	engNo=100 model=bmw color=red    
	engNo=100 model=bmw color=red  
	
-- Here we are creating an object using new, which returns the address where object is stored in heap.
-- To access it's data members and functions, we normally use -> notation since V1 is pointer to object.
-- But we can also use (*V1), since V1 contains the address where object is stored, *V1 can be thought of like using the object itself, that's why `.` operator can be used to access data members and functions directly.

---

