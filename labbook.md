Q1 


grid.cpp 

```c++
#include "Grid.h"
#include <fstream>

void Grid::LoadGrid(const char filename[])
{
	std::ifstream inputFile(filename);
	for (int y = 0; y <= 8; y++)
	{
		for (int x = 0; x <= 8; x++)
		{
			inputFile >> m_grid[x][y];
		}
	}
	inputFile.close();
}

void Grid::SaveGrid(const char filename[])
{
	std::ofstream outputFile(filename);
	for (int y = 0; y <= 8; y++)
	{
		for (int x = 0; x <= 8; x++)
		{
			outputFile << m_grid[x][y];
			if (x < 8)
				outputFile << " "; 
		}
		outputFile << std::endl;  
	}
	outputFile.close();
}

std::ostream& operator<<(std::ostream& os, const Grid& grid)
{
	for (int y = 0; y <= 8; y++)
	{
		for (int x = 0; x <= 8; x++)
		{
			os << grid.m_grid[x][y];
			if (x < 8)
				os << " ";
		}
		os << std::endl;
	}
	return os;
}

std::istream& operator>>(std::istream& is, Grid& grid)
{
	for (int y = 0; y <= 8; y++)
	{
		for (int x = 0; x <= 8; x++)
		{
			// Read the next integer into the grid
			is >> grid.m_grid[x][y];
		}
	}
	return is;
}
```


Grid.h 

```c++
#pragma once
#include <iostream> // Added for ostream and istream

class Grid
{
public:
	Grid();
	~Grid();

	void LoadGrid(const char filename[]);
	void SaveGrid(const char filename[]);

	// Lab E - Operator Overloading
	friend std::ostream& operator<<(std::ostream& os, const Grid& grid);
	friend std::istream& operator>>(std::istream& is, Grid& grid);

private:
	int m_grid[9][9];
};
```


Q2

fraction.cpp

```cpp
#include "Fraction.h"

Fraction::Fraction() : m_num(0), m_den(1) {}

Fraction::Fraction(int num, int den) : m_num(num), m_den(den) {}

Fraction Fraction::Add(const Fraction& other) {
    // Formula fro
    #][
    
    #
    m Equation 1
    int n = (m_num * other.m_den) + (other.m_num * m_den);
    int d = m_den * other.m_den;
    return Fraction(n, d);
}

Fraction Fraction::Subtract(const Fraction& other) {
    // ubtracting the numerators
    int n = (m_num * other.m_den) - (other.m_num * m_den);
    int d = m_den * other.m_den;
    return Fraction(n, d);
}

Fraction Fraction::Multiply(int multiplier) {
    // Multiplying a fraction by an integer - only affects the numerator
    return Fraction(m_num * multiplier, m_den);
}

void Fraction::Write(std::ostream& sout) {
    sout << m_num << "/" << m_den;
}

void Fraction::Read(std::istream& sin) {
    // Reads two integers
    sin >> m_num >> m_den;
}
```

Fraction.h

```cpp
#include <iostream>

class Fraction {
public:
    Fraction(); 
    Fraction(int num, int den); 

    // Arithmetic Behaviours
    Fraction Add(const Fraction& other);
    Fraction Subtract(const Fraction& other);
    Fraction Multiply(int multiplier);

    // Read and Write 
    void Write(std::ostream& sout);
    void Read(std::istream& sin);

    // Inspectors and Mutators
    int GetNum() const { return m_num; }
    int GetDen() const { return m_den; }
    void SetNum(int num) { m_num = num; }
    void SetDen(int den) { m_den = den; }

private:
    // Private data members 
    int m_num;
    int m_den;
};
```

source.cpp

```cpp
#include "Fraction.h"
#include <iostream>
using namespace std;

int main(int args, char** argv)
{
    Fraction f1(1, 2);        // 1/2
    Fraction f2(3, 4);        // 3/4
    Fraction result;

    result = f1.Add(f2);      // 1/2 + 3/4 = 10/8
    cout << "1/2 + 3/4 = ";
    result.Write(cout);
    cout << endl;

    result = f2.Subtract(f1); // 3/4 - 1/2 = 2/8
    cout << "3/4 - 1/2 = ";
    result.Write(cout);
    cout << endl;

    result = f2.Multiply(3);  // 3/4 * 3 = 9/4
    cout << "3/4 * 3 = ";
    result.Write(cout);
    cout << endl;

    Fraction f3;
    f3.Read(cin);            
    cout << "Read = ";
    f3.Write(cout);
    cout << endl;

    system("pause");
}
```

Q3


fraction.cpp

```
#include "Fraction.h"


Fraction::Fraction() : m_num(0), m_den(1) {
}

Fraction::Fraction(int num, int den) : m_num(num), m_den(den) {
}


// Member operator+
Fraction Fraction::operator+(const Fraction& rhs) const {
    int n = (m_num * rhs.m_den) + (rhs.m_num * m_den);
    int d = m_den * rhs.m_den;
    return Fraction(n, d);
}

// Member operator-
Fraction Fraction::operator-(const Fraction& rhs) const {
    int n = (m_num * rhs.m_den) - (rhs.m_num * m_den);
    int d = m_den * rhs.m_den;
    return Fraction(n, d);
}

// Member Operator* (Fraction * int)
Fraction Fraction::operator*(int rhs) const {
    return Fraction(m_num * rhs, m_den);
}

// Auxiliary Operator* (int * Fraction)
Fraction operator*(int lhs, const Fraction& rhs) {
    // We can call the member operator* because multiplication is commutative
    return rhs * lhs;
}

// Output stream operator
std::ostream& operator<<(std::ostream& os, const Fraction& f) {
    os << f.m_num << "/" << f.m_den;
    return os;
}

// Input stream operator
std::istream& operator>>(std::istream& is, Fraction& f) {
    is >> f.m_num >> f.m_den;
    return is;
}
```

fraction.h

```
#include <iostream>

class Fraction {
public:
    Fraction();
    Fraction(int num, int den);

    // Member operators
    Fraction operator+(const Fraction& rhs) const;
    Fraction operator-(const Fraction& rhs) const;
    Fraction operator*(int rhs) const; // Fraction * int

    // Auxiliary operators
    // need to be friends to access private m_num and m_den
    friend Fraction operator*(int lhs, const Fraction& rhs); // int * Fraction
    friend std::ostream& operator<<(std::ostream& os, const Fraction& f);
    friend std::istream& operator>>(std::istream& is, Fraction& f);

private:
    int m_num;
    int m_den;
};
```

source.cpp


```cpp

#include "Fraction.h"
#include <iostream>
using namespace std;

int main(int args, char** argv)
{
    Fraction f1(1, 2);        // 1/2
    Fraction f2(3, 4);        // 3/4
    Fraction result;

    // 1. Using operator+
    result = f1 + f2;
    cout << "1/2 + 3/4 = " << result << endl; 

    // 2. Using operator- 
    result = f2 - f1;
    cout << "3/4 - 1/2 = " << result << endl;

    // 3. Using member operator* (Fraction * int)
    result = f2 * 3;
    cout << "3/4 * 3 = " << result << endl;

    // 4. Using auxiliary operator* (int * Fraction)
    result = 3 * f2;
    cout << "3 * 3/4 = " << result << endl;

    // 5. Using operator>>
    Fraction f4;
    cout << "Enter a fraction (e.g., 1 2 for 1/2): ";
    cin >> f4;
    cout << "Read = " << f4 << endl;

    system("pause");
    return 0;
}
```


Q4

```cpp
#include <iostream>
using namespace std;

// Updated to Pass by Reference
void myswap(int& lhs, int& rhs) {
	int temp = lhs;
	lhs = rhs;
	rhs = temp;
}

int main(int, char**) {
	int a = 10;
	int b = 20;

	cout << "Before swap: a=" << a << ", b=" << b << endl;

	myswap(a, b); // this will actually swap the values

	cout << "After swap: a=" << a << ", b=" << b << endl;

	system("pause");
	return 0;
}
```

reflection:
When I ran the first version (Pass by Value), I was confused why a and b stayed the same even though the function code looked correct. After opening the disassembly and memory windows, I saw exactly why - the program was pushing the values 10 and 20 onto the stack. The function swapped those copies, but the original variables back in main were never touched.

Once I changed it to Pass by Reference (int&), the assembly code changed. Instead of pushing the numbers, it pushed the memory addresses of a and b. I followed the address in the Memory window and actually saw the numbers 0a (10) and 14 (20) flip-flop in real-time as I stepped through with F11. It made me realize that references aren't just a syntax trick; they actually change how the CPU handles the data by pointing to the original "home" of the variable instead of making a duplicate.


Q5

Return-by-Value Code:
```cpp
int clamp(int value, int low, int high) {
    if (value < low) return low;
    if (value > high) return high;
    return value;
}
```

Return-by-Reference Code:
```cpp
int& clamp(int& value, int low, int high) {
    if (value < low) return low;
    if (value > high) return high;
    return value;
}
```

Reflection:
Returning by value is safe because it just sends a copy of the number back. Returning by reference is dangerous here because if it returns "low" or "high", those are local variables that disappear when the function ends. My second test gave weird results because I was trying to use a memory address that wasn't valid anymore.
