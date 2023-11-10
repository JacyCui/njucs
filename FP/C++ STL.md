# The C++ Standard Template Library

**Jacy（家才）**



[TOC]



## 1 Introduction

### 1.1 General Description

The Standard Template Library (`STL`) is **a set of C++ template classes to provide common programming data structures and functions such as lists, stacks, arrays, etc**. It is a library of container classes, algorithms, and iterators. It is a generalized library and so, its components are parameterized. A working knowledge of `template classes` is a prerequisite for working with `STL`.

#### `STL` has four components

- Algorithms
- Containers
- Functions
- Iterators

### 1.2 Algorithms

The header algorithm defines a collection of functions especially designed to be used on ranges of elements. They act on containers and provide means for various operations for the contents of the containers.

- Algorithm
  - Sorting
  - Searching
  - Important `STL` Algorithms
  - Useful Array algorithms
  - Partition Operations
- Numeric
  - valarray class

### 1.3 Containers

Containers or container classes store objects and data. There are in total seven standard “first-class” container classes and three container adaptor classes and only seven header files that provide access to these containers or container adaptors.

- Sequence Containers: implement data structures which can be accessed in a sequential manner.
  - `vector`
  - `list`
  - `deque`
  - `arrays`
  - `forward_list`( Introduced in C++11)
- Container Adaptors : provide a different interface for sequential containers.
  - `queue`
  - `priority_queue`
  - `stack`
- Associative Containers : implement sorted data structures that can be quickly searched (O(log n) complexity).
  - `set`
  - `multiset`
  - `map`
  - `multimap`
- Unordered Associative Containers : implement unordered data structures that can be quickly searched
  - `unordered_set`(Introduced in C++11)
  - `unordered_multiset` (Introduced in C++11)
  - `unordered_map` (Introduced in C++11)
  - `unordered_multimap` (Introduced in C++11)

> *Flowchart of Adaptive Containers and Unordered Containers*
>
> ![Lightbox](https://media.geeksforgeeks.org/wp-content/uploads/20191111161536/Screenshot-from-2019-11-11-16-13-18.png)
>
> *Flowchart of Sequence containers and ordered containers*
>
> ![Lightbox](https://media.geeksforgeeks.org/wp-content/uploads/20191111161627/Screenshot-from-2019-11-11-16-15-07.png)

### 1.4 Functions

The `STL` includes classes that overload the function call operator. Instances of such classes are called function objects or functors. Functors allow the working of the associated function to be customized with the help of parameters to be passed.

- Functors

### 1.5 Iterators

As the name suggests, iterators are used for working upon a sequence of values. They are the major feature that allow generality in `STL`.

- Iterators

### 1.6 Utility Library

Defined in header `<utility>`.

- `pair`



## 2 Algorithm

> The header algorithm defines a collection of functions especially designed to be used on ranges of elements.They act on containers and provide means for various operations for the contents of the containers.

### 2.1 Sort in C++ Standard Template Library (`STL`)

**Sorting** is one of the most basic functions applied to data. It means arranging the data in a particular fashion, which can be increasing or decreasing. There is a built-in function in C++ `STL` by the name of `sort()`. 
This function internally uses `IntroSort`. In more details it is implemented using hybrid of `QuickSort`, `HeapSort` and `InsertionSort` .By default, it uses `QuickSort` but if `QuickSort` is doing unfair partitioning and taking more than `N*logN` time, it switches to `HeapSort` and when the array size becomes really small, it switches to `InsertionSort`.

The prototype for sort is : 

```
sort(startAddress, endAddress)

startaddress: the address of the first element of the array
endaddress: the address of the next contiguous location of 
            the last element of the array.
So actually sort() sorts in the range of [startAddress,endAddress)
```

#### An Example for `sort`

 ```c++
// C++ progrma to sort an array
#include <algorithm>
#include <iostream>

using namespace std;

void show(int a[], int array_size) {
	for (int i = 0; i < array_size; ++i)
		cout << a[i] << " ";
}

// Driver code
int main() {
	int a[] = { 1, 5, 8, 9, 6, 7, 3, 4, 2, 0 };

	// size of the array
	int asize = sizeof(a) / sizeof(a[0]);
	cout << "The array before sorting is : \n";

	// print the array
	show(a, asize);

	// sort the array
	sort(a, a + asize);

	cout << "\n\nThe array after sorting is :\n";

	// print the array after sorting
	show(a, asize);

	return 0;
}
 ```

**Output**

```
The array before sorting is : 
1 5 8 9 6 7 3 4 2 0 

The array after sorting is :
0 1 2 3 4 5 6 7 8 9 
```

#### More details on `std::sort()` in C++ `STL`

`sort` generally takes two parameters , the first one being the point of the array/vector from where the sorting needs to begin and the second parameter being the length up to which we want the array/vector to get sorted. The third parameter is optional and can be used in cases such as if we want to sort the elements lexicographically.

By default, sort() function sorts the element in ascending order.

Below is a simple program to show working of `sort()`. 

```c++
// C++ program to demonstrate default behaviour of sort() in STL. 
#include <bits/stdc++.h> 
using namespace std; 

int main() { 
	int arr[] = { 1, 5, 8, 9, 6, 7, 3, 4, 2, 0 }; 
	int n = sizeof(arr) / sizeof(arr[0]); 

	/*Here we take two parameters, the beginning of the 
	array and the length n upto which we want the array to 
	be sorted*/
	sort(arr, arr + n); 

	cout << "\nArray after sorting using default sort is : \n"; 
	for (int i = 0; i < n; ++i) 
		cout << arr[i] << " "; 

	return 0; 
}
```

**Output :** 

```
Array after sorting using default sort is : 
0 1 2 3 4 5 6 7 8 9 
```

>  `<bits/stdc++.h>` is basically a header file that includes every standard library. 

##### How to sort in descending order?

`sort()` takes a third parameter that is used to specify the order in which elements are to be sorted. We can pass `greater()` function to sort in descending order. This function does a comparison in a way that puts greater element before. 

```c++
// C++ program to demonstrate descending order sort using greater<>(). 
#include <bits/stdc++.h> 
using namespace std; 

int main() { 
	int arr[] = { 1, 5, 8, 9, 6, 7, 3, 4, 2, 0 }; 
	int n = sizeof(arr) / sizeof(arr[0]); 

	sort(arr, arr + n, greater<int>()); 

	cout << "Array after sorting : \n"; 
	for (int i = 0; i < n; ++i) 
		cout << arr[i] << " "; 

	return 0; 
}
```

**Output:**

```
Array after sorting : 
9 8 7 6 5 4 3 2 1 0 
```

##### How to sort in particular order?

We can also write our own comparator function and pass it as a third parameter. This “comparator” function returns a value; convertible to `bool`, which basically tells us whether the passed “first” argument should be placed before the passed “second” argument or not. 
For example: In the code below, suppose intervals {6,8} and {1,9} are passed as arguments in the `compareInterval` function (comparator function). Now as `i1.first (=6) > i2.first (=1)`, so our function returns “false”, which tells us that “first” argument should not be placed before “second” argument and so sorting will be done in order like {1,9} first and then {6,8} as next. 

```c++
// A C++ program to demonstrate STL sort() using our own comparator 
#include <bits/stdc++.h> 
using namespace std; 

// An interval has a start time and end time 
struct Interval { 
	int start, end; 
}; 

// Compares two intervals 
// according to staring times. 
bool compareInterval(Interval i1, Interval i2) { 
	return (i1.start < i2.start); 
} 

int main() { 
	Interval arr[] 
		= { { 6, 8 }, { 1, 9 }, { 2, 4 }, { 4, 7 } }; 
	int n = sizeof(arr) / sizeof(arr[0]); 

	// sort the intervals in increasing order of 
	// start time 
	sort(arr, arr + n, compareInterval); 

	cout << "Intervals sorted by start time : \n"; 
	for (int i = 0; i < n; i++) 
		cout << "[" << arr[i].start << "," << arr[i].end << "] "; 

	return 0; 
}
```

**Output:** 
```
Intervals sorted by start time : 
[1,9] [2,4] [4,7] [6,8] 
```

### 2.2 Binary Search in C++ Standard Template Library (`STL`)

**Binary search** is a widely used searching algorithm that requires the array to be sorted before search is applied. The main idea behind this algorithm is to keep dividing the array in half (divide and conquer) until the element is found, or all the elements are exhausted.
It works by comparing the middle item of the array with our target, if it matches, it returns true otherwise if the middle term is greater than the target, the search is performed in the left sub-array. If the middle term is less than the target, the search is performed in the right sub-array.

The prototype for binary search is : 

```
binary_search(startaddress, endaddress, valuetofind)

Parameters :
startaddress: the address of the first element of the array.
endaddress: the address of the next contiguous location of 
            the last element of the array.
valuetofind: the target value which we have to search for.

Returns :
true if an element equal to valuetofind is found, else false.
```

#### An Example for `binary_search`

```c++
// CPP program to implement Binary Search in Standard Template Library (STL) 
#include <algorithm> 
#include <iostream> 

using namespace std; 

void show(int a[], int arraysize) { 
	for (int i = 0; i < arraysize; ++i) 
		cout << a[i] << ","; 
} 

int main() { 
	int a[] = { 1, 5, 8, 9, 6, 7, 3, 4, 2, 0 }; 
	int asize = sizeof(a) / sizeof(a[0]); 
	cout << "\nThe array is : \n"; 
	show(a, asize); 

	cout << "\n\nLet's say we want to search for "; 
	cout << "\n2 in the array So, we first sort the array"; 
	sort(a, a + asize); 
	cout << "\n\nThe array after sorting is : \n"; 
	show(a, asize); 
	cout << "\n\nNow, we do the binary search"; 
	if (binary_search(a, a + 10, 2)) 
		cout << "\nElement found in the array"; 
	else
		cout << "\nElement not found in the array"; 

	cout << "\n\nNow, say we want to search for 10"; 
	if (binary_search(a, a + 10, 10)) 
		cout << "\nElement found in the array"; 
	else
		cout << "\nElement not found in the array"; 

	return 0; 
}
```

**Output**

```
The array is : 
1,5,8,9,6,7,3,4,2,0,

Let's say we want to search for 
2 in the array So, we first sort the array

The array after sorting is : 
0,1,2,3,4,5,6,7,8,9,

Now, we do the binary search
Element found in the array

Now, say we want to search for 10
Element not found in the array
```

#### More details on `std::bsearch` in C++ `STL`

`std::bsearch` searches for an element in a sorted array. Finds an element equal to element pointed to by `key` in an array pointed to by `ptr`.
If the array contains several elements that `comp` would indicate as equal to the element searched for, then it is unspecified which element the function will return as the result.

**Syntax :**

```
void* bsearch( const void* key, const void* ptr, std::size_t count, std::size_t size, * comp );

Parameters :
key     -    element to be found
ptr     -    pointer to the array to examine
count    -    number of element in the array
size    -    size of each element in the array in bytes
comp    -    comparison function which returns a negative integer value if 
             the first argument is less than the second, a positive integer 
             value if the first argument is greater than the second and zero 
             if the arguments are equal.

Return value :
Pointer to the found element or null pointer if the element has not been found.
```

##### Implementing the binary predicate `comp` :

```c++
// Binary predicate which returns 0 if numbers found equal 
int comp(int* a, int* b) { 
	if (*a < *b) 
		return -1; 
	else if (*a > *b) 
		return 1; 
	// elements found equal 
	else
		return 0; 
} 
```

**Implementation**

```c++
// CPP program to implement std::bsearch 
#include <bits/stdc++.h> 

// Binary predicate 
int compare(const void* ap, const void* bp) { 
	// Typecasting 
	const int* a = (int*)ap; 
	const int* b = (int*)bp; 

	if (*a < *b) 
		return -1; 
	else if (*a > *b) 
		return 1; 
	else
		return 0; 
} 

// Driver code 
int main() { 
	// Given array 
	int arr[] = { 1, 2, 3, 4, 5, 6, 7, 8 }; 

	// Size of array 
	int ARR_SIZE = sizeof(arr) / sizeof(arr[0]); 

	// Element to be found 
	int key1 = 4; 

	// Calling std::bsearch 
	// Typecasting the returned pointer to int 
	int* p1 = (int*)std::bsearch(&key1, arr, ARR_SIZE, sizeof(arr[0]), compare); 

	// If non-zero value is returned, key is found 
	if (p1) 
		std::cout << key1 << " found at position " << (p1 - arr) << '\n'; 
	else
		std::cout << key1 << " not found\n"; 

	// Element to be found 
	int key2 = 9; 

	// Calling std::bsearch 
	// Typecasting the returned pointer to int 
	int* p2 = (int*)std::bsearch(&key2, arr, ARR_SIZE, sizeof(arr[0]), compare); 

	// If non-zero value is returned, key is found 
	if (p2) 
		std::cout << key2 << " found at position " << (p2 - arr) << '\n'; 
	else
		std::cout << key2 << " not found\n"; 
} 
```

**Output:**

```
4 found at position 3
9 not found
```

##### Where to use :

Binary search can be used on sorted data where a key is to be found. It can be used in cases like computing frequency of a key in a sorted list.

##### Why Binary Search?

Binary search is much more effective than linear search because it halves the search space at each step. This is not significant for our array of length 9. Here, linear search takes at most 9 steps and binary search takes at most 4 steps. But consider an array with 1000 elements, here linear search takes at most 1000 steps, while binary search takes at most 10 steps.
For 1 billion elements, binary search will find our key in at most 30 steps.

### 2.3 Algorithm Library | C++ Magicians STL Algorithm

For all those who aspire to excel in competitive programming, only having a knowledge about containers of STL is of less use till one is not aware what all STL has to offer. 
Some of the most used algorithms on vectors and most useful one’s in Competitive Programming are mentioned as follows :

#### Non-Manipulating Algorithms

1. `sort(first_iterator, last_iterator)` – To sort the given vector.
2. `reverse(first_iterator, last_iterator)` – To reverse a vector.
3. `*max_element (first_iterator, last_iterator)` – To find the maximum element of a vector.
4. `*min_element (first_iterator, last_iterator)` – To find the minimum element of a vector.
5. `accumulate(first_iterator, last_iterator, initial value of sum)` – Does the summation of vector elements

```c++
// A C++ program to demonstrate working of sort(), reverse()
#include <algorithm>
#include <iostream>
#include <vector>
#include <numeric> //For accumulate operation
using namespace std;

int main() {
	// Initializing vector with array values
	int arr[] = {10, 20, 5, 23 ,42 , 15};
	int n = sizeof(arr)/sizeof(arr[0]);
	vector<int> vect(arr, arr+n);

	cout << "Vector is: ";
	for (int i=0; i<n; i++)
		cout << vect[i] << " ";

	// Sorting the Vector in Ascending order
	sort(vect.begin(), vect.end());

	cout << "\nVector after sorting is: ";
	for (int i=0; i<n; i++)
	cout << vect[i] << " ";

	// Reversing the Vector
	reverse(vect.begin(), vect.end());

	cout << "\nVector after reversing is: ";
	for (int i=0; i<6; i++)
		cout << vect[i] << " ";

	cout << "\nMaximum element of vector is: ";
	cout << *max_element(vect.begin(), vect.end());

	cout << "\nMinimum element of vector is: ";
	cout << *min_element(vect.begin(), vect.end());

	// Starting the summation from 0
	cout << "\nThe summation of vector elements is: ";
	cout << accumulate(vect.begin(), vect.end(), 0);

	return 0;
}
```

**Output**

```
Vector is: 10 20 5 23 42 15 
Vector after sorting is: 5 10 15 20 23 42 
Vector after reversing is: 42 23 20 15 10 5 
Maximum element of vector is: 42
Minimum element of vector is: 5
The summation of vector elements is: 115
```

6. `count(first_iterator, last_iterator,x)` – To count the occurrences of x in vector.

7. `find(first_iterator, last_iterator, x)` – Returns an iterator to the first occurence of x in vector and points to last address of vector ((name_of_vector).end()) if element is not present in vector.

```c++
// C++ program to demonstrate working of count() and find()
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int main() {
	// Initializing vector with array values
	int arr[] = {10, 20, 5, 23 ,42, 20, 15};
	int n = sizeof(arr)/sizeof(arr[0]);
	vector<int> vect(arr, arr+n);

	cout << "Occurrences of 20 in vector : ";

	// Counts the occurrences of 20 from 1st to last element
	cout << count(vect.begin(), vect.end(), 20);

	// find() returns iterator to last address if element not present
	find(vect.begin(), vect.end(),5) != vect.end() ?
						cout << "\nElement found" :
					cout << "\nElement not found" ;
    //here <> ? <> : <> is a special operator with three operands a.k.a conditional operator
	return 0;
}
```

**Output**

```
Occurrences of 20 in vector : 2
Element found
```

8. `binary_search (first_iterator, last_iterator, x)` – Tests whether x exists in sorted vector or not.

9. `lower_bound(first_iterator, last_iterator, x)` – returns an iterator pointing to the first element in the range [first,last) which has a value not less than ‘x’.

10. `upper_bound(first_iterator, last_iterator, x)` – returns an iterator pointing to the first element in the range [first,last)          which has a value greater than ‘x’. 

```c++
// C++ program to demonstrate working of lower_bound()
// and upper_bound().
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int main() {
	// Initializing vector with array values
	int arr[] = {5, 10, 15, 20, 20, 23, 42, 45};
	int n = sizeof(arr)/sizeof(arr[0]);
	vector<int> vect(arr, arr+n);

	// Sort the array to make sure that lower_bound()
	// and upper_bound() work.
	sort(vect.begin(), vect.end());

	// Returns the first occurrence of 20
	auto q = lower_bound(vect.begin(), vect.end(), 20);

	// Returns the last occurrence of 20
	auto p = upper_bound(vect.begin(), vect.end(), 20);

	cout << "The lower bound is at position: ";
	cout << q-vect.begin() << endl;

	cout << "The upper bound is at position: ";
	cout << p-vect.begin() << endl;

	return 0;
}
```

**Output**

```
The lower bound is at position: 3
The upper bound is at position: 5
```

#### Some Manipulating Algorithms

1. `arr.erase(position to be deleted)` – This erases selected element in vector and shifts and resizes the vector elements accordingly.
2. `arr.erase(unique(arr.begin(),arr.end()),arr.end())` – This erases the duplicate occurrences in sorted vector in a single line.

```c++
// C++ program to demonstrate working of erase()
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int main()
{
	// Initializing vector with array values
	int arr[] = {5, 10, 15, 20, 20, 23, 42, 45};
	int n = sizeof(arr)/sizeof(arr[0]);
	vector<int> vect(arr, arr+n);

	cout << "Vector is :";
	for (int i=0; i<6; i++)
		cout << vect[i]<<" ";

	// Delete second element of vector
	vect.erase(vect.begin()+1);

	cout << "\nVector after erasing the element: ";
	for (int i=0; i<5; i++)
		cout << vect[i] << " ";

	// sorting to enable use of unique()
	sort(vect.begin(), vect.end());

	cout << "\nVector before removing duplicate "
			" occurrences: ";
	for (int i=0; i<5; i++)
		cout << vect[i] << " ";

	// Deletes the duplicate occurrences
	vect.erase(unique(vect.begin(),vect.end()),vect.end());

	cout << "\nVector after deleting duplicates: ";
	for (int i=0; i< vect.size(); i++)
		cout << vect[i] << " ";

	return 0;
}
```

**Output**

```
Vector is :5 10 15 20 20 23 
Vector after erasing the element: 5 15 20 20 23 
Vector before removing duplicate  occurrences: 5 15 20 20 23 
Vector after deleting duplicates: 5 15 20 23 42 45 
```

3. `next_permutation(first_iterator, last_iterator)` – This modified the vector to its next permutation.

4. `prev_permutation(first_iterator, last_iterator)` – This modified the vector to its previous permutation. 

```c++
// C++ program to demonstrate working of next_permutation() and prev_permutation()
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int main() {
	// Initializing vector with array values
	int arr[] = {5, 10, 15, 20, 20, 23, 42, 45};
	int n = sizeof(arr)/sizeof(arr[0]);
	vector<int> vect(arr, arr+n);

	cout << "Given Vector is:\n";
	for (int i=0; i<n; i++)
		cout << vect[i] << " ";

	// modifies vector to its next permutation order
	next_permutation(vect.begin(), vect.end());
	cout << "\nVector after performing
				next permutation:\n";
	for (int i=0; i<n; i++)
		cout << vect[i] << " ";

	prev_permutation(vect.begin(), vect.end());
	cout << "\nVector after performing prev
							permutation:\n";
	for (int i=0; i<n; i++)
		cout << vect[i] << " ";

	return 0;
}
```

**Output**

```
Given Vector is:
5 10 15 20 20 23 42 45 
Vector after performing next permutation:
5 10 15 20 20 23 45 42 
Vector after performing prev permutation:
5 10 15 20 20 23 42 45 
```

5. `distance(first_iterator,desired_position)` – It returns the distance of desired position from the first iterator.This function is very useful while finding the index. 

```c++
// C++ program to demonstrate working of distance()
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int main() {
	// Initializing vector with array values
	int arr[] = {5, 10, 15, 20, 20, 23, 42, 45};
	int n = sizeof(arr)/sizeof(arr[0]);
	vector<int> vect(arr, arr+n);

	// Return distance of first to maximum element
	cout << "Distance between first to max element: ";
	cout << distance(vect.begin(), max_element(vect.begin(), vect.end()));
	return 0;
}
```

**Output**

```
Distance between first to max element: 7
```

### 2.4 Array algorithms in C++ STL (`all_of`, `any_of`, `none_of`, `copy_n` and `iota`)

From C++11 onwards, some new and interesting algorithms are added in STL of C++. These algorithms operate on an array and are useful in saving time during coding and hence useful in competitive programming as well.

#### `all_of()`

This function operates on whole range of array elements and can save time to run a loop to check each elements one by one. It checks for a given property on every element and returns true when each element in range satisfies specified property, else returns false.

```c++
// C++ code to demonstrate working of all_of()
#include<iostream>
#include<algorithm> // for all_of()
using namespace std;
int main() {
	// Initializing array
	int ar[6] = {1, 2, 3, 4, 5, -6};

	// Checking if all elements are positive
	all_of(ar, ar+6, [](int x) { return x>0; })?
		cout << "All are positive elements" :
		cout << "All are not positive elements";

	return 0;

}
```

**Output:**

```
All are not positive elements
```

In the above code, -6 being a negative element negates the condition and returns false.

#### `any_of()`

This function checks for a given range if there’s even one element satisfying a given property mentioned in function. Returns true if at least one element satisfies the property else returns false.

```c++
// C++ code to demonstrate working of any_of()
#include<iostream>
#include<algorithm> // for any_of()
using namespace std;
int main() {
	// Initializing array
	int ar[6] = {1, 2, 3, 4, 5, -6};

	// Checking if any element is negative
	any_of(ar, ar+6, [](int x){ return x<0; })?
		cout << "There exists a negative element" :
		cout << "All are positive elements";

	return 0;

}
```

**Output:**

```
There exists a negative element
```

In above code, -6 makes the condition positive.

#### `none_of()`

This function returns true if none of elements satisfies the given condition else returns false.

```c++
// C++ code to demonstrate working of none_of()
#include<iostream>
#include<algorithm> // for none_of()
using namespace std;
int main() {
	// Initializing array
	int ar[6] = {1, 2, 3, 4, 5, 6};

	// Checking if no element is negative
	none_of(ar, ar+6, [](int x){ return x<0; })?
		cout << "No negative elements" :
		cout << "There are negative elements";

	return 0;
}
```

Output:

```
No negative elements
```

Since all elements are positive, the function returns true.

#### `copy_n()`

`copy_n()` copies one array elements to new array. This type of copy creates a deep copy of array. This function takes 3 arguments, source array name, size of array and the target array name.

```c++
// C++ code to demonstrate working of copy_n()
#include<iostream>
#include<algorithm> // for copy_n()
using namespace std;
int main() {
	// Initializing array
	int ar[6] = {1, 2, 3, 4, 5, 6};

	// Declaring second array
	int ar1[6];

	// Using copy_n() to copy contents
	copy_n(ar, 6, ar1);

	// Displaying the copied array
	cout << "The new array after copying is : ";
	for (int i=0; i<6 ; i++)
	cout << ar1[i] << " ";

	return 0;

}
```

**Output:**

```
The new array after copying is : 1 2 3 4 5 6
```

In the above code, the elements of ar are copied in `ar1` using `copy_n()`.

#### `iota()`

This function is used to assign continuous values to array. This function accepts 3 arguments, the array name, size, and the starting number.

```c++
// C++ code to demonstrate working of iota()
#include<iostream>
#include<numeric> // for iota()
using namespace std;
int main() {
	// Initializing array with 0 values
	int ar[6] = {0};

	// Using iota() to assign values
	iota(ar, ar+6, 20);

	// Displaying the new array
	cout << "The new array after assigning values is : ";
	for (int i=0; i<6 ; i++)
	cout << ar[i] << " ";

	return 0;

}
```

Output:

```
The new array after assigning values is : 20 21 22 23 24 25
```

In the above code, continuous values are assigned to array using `iota()`.

### 2.5 `std::partition` in C++ STL

C++ has a class in its STL algorithms library which allows us easy partition algorithms using certain inbuilt functions. Partition refers to act of dividing elements of containers depending upon a given condition. 

#### Partition operations :

1. `partition(beg, end, condition)` :- This function is used to **partition the elements** on **basis of condition** mentioned in its arguments.
2. `is_partitioned(beg, end, condition)` :- This function returns boolean **true if container is partitioned** else returns false.

```c++
// C++ code to demonstrate the working of
// partition() and is_partitioned()
#include<iostream>
#include<algorithm> // for partition algorithm
#include<vector> // for vector
using namespace std;
int main() {
	// Initializing vector
	vector<int> vect = { 2, 1, 5, 6, 8, 7 };
	
	// Checking if vector is partitioned
	// using is_partitioned()
	is_partitioned(vect.begin(), vect.end(), [](int x) { return x%2==0; })?
	cout << "Vector is partitioned":
	cout << "Vector is not partitioned";
	cout << endl;
	
	// partitioning vector using partition()
	partition(vect.begin(), vect.end(), [](int x) { return x%2==0; });
	
	// Checking if vector is partitioned
	// using is_partitioned()
	is_partitioned(vect.begin(), vect.end(), [](int x) { return x%2==0; })?
	cout << "Now, vector is partitioned after partition operation":
	cout << "Vector is still not partitioned after partition operation";
	cout << endl;
	
	// Displaying partitioned Vector
	cout << "The partitioned vector is : ";
	for (int &x : vect) cout << x << " ";
	
	return 0;
	
}
```

**Output:** 

```
Vector is not partitioned
Now, vector is partitioned after partition operation
The partitioned vector is : 2 8 6 5 1 7
```

In the above code, partition function partitions the vector depending on whether an element is even or odd, even elements are partitioned from odd elements in no particular order. 

3. `stable_partition(beg, end, condition)` :- This function is used to **partition the elements** on **basis of condition** mentioned in its arguments in **such a way that the relative order of the elements is preserved.**
4. `partition_point(beg, end, condition)` :- This function **returns an iterator pointing to the partition point** of container i.e. the first element in the partitioned range [beg,end) for which condition is not true. The container should already be partitioned for this function to work.

```c++
// C++ code to demonstrate the working of
// stable_partition() and partition_point()
#include<iostream>
#include<algorithm> // for partition algorithm
#include<vector> // for vector
using namespace std;
int main() {
	// Initializing vector
	vector<int> vect = { 2, 1, 5, 6, 8, 7 };
	
	// partitioning vector using stable_partition()
	// in sorted order
	stable_partition(vect.begin(), vect.end(), [](int x) { return x%2 == 0;	});
	
	// Displaying partitioned Vector
	cout << "The partitioned vector is : ";
	for (int &x : vect) cout << x << " ";
	cout << endl;
	
	// Declaring iterator
	vector<int>::iterator it1;
	
	// using partition_point() to get ending position of partition
	auto it = partition_point(vect.begin(), vect.end(), [](int x) { return x%2==0; });
	
	// Displaying partitioned Vector
	cout << "The vector elements returning true for condition are : ";
	for ( it1= vect.begin(); it1!=it; it1++)
	cout << *it1 << " ";
	cout << endl;
	
	return 0;
	
}
```

**Output:** 

```
The partitioned vector is : 2 6 8 1 5 7 
The vector elements returning true for condition are : 2 6 8
```

In the above code, even and odd elements are partitioned and in the increasing order (sorted). Not always in increasing order though, here the elements (even and odd) appeared in increased order so is the result after partition. if vect would have been { 2,1,7,8,6,5 } after stable_partition() it would be  { 2,8,6,1,7,5 }. The order of appearance is maintained.

5. `partition_copy(beg, end, beg1, beg2, condition)` :- This function **copies the partitioned elements** in the differenet containers mentioned in its arguments. It takes 5 arguments. **Beginning and ending position of container, beginning position of new container where elements have to be copied (elements returning true for condition), beginning position of new container where other elements have to be copied (elements returning false for condition) and the condition**. **Resizing** new containers **is necessary** for this function.

```c++
// C++ code to demonstrate the working of
// partition_copy()
#include<iostream>
#include<algorithm> // for partition algorithm
#include<vector> // for vector
using namespace std;
int main() {
	// Initializing vector
	vector<int> vect = { 2, 1, 5, 6, 8, 7 };
	
	// Declaring vector1
	vector<int> vect1;
	
	// Declaring vector1
	vector<int> vect2;
	
	// Resizing vectors to suitable size using count_if() and resize()
	int n = count_if (vect.begin(), vect.end(), [](int x) { return x%2==0; } );
	vect1.resize(n);
	vect2.resize(vect.size()-n);
	
	// Using partition_copy() to copy partitions
	partition_copy(vect.begin(), vect.end(), vect1.begin(), vect2.begin(), [](int x) { return x%2==0; });
	
	
	// Displaying partitioned Vector
	cout << "The elements that return true for condition are : ";
	for (int &x : vect1)
			cout << x << " ";
	cout << endl;
	
	// Displaying partitioned Vector
	cout << "The elements that return false for condition are : ";
	for (int &x : vect2)
			cout << x << " ";
	cout << endl;
	
	return 0;
}
```

**Output:** 

```
The elements that return true for condition are : 2 6 8 
The elements that return false for condition are : 1 5 7
```

### 2.6 Numeric  —— `std:: valarray` class in C++

C++98 introduced a special container called valarray to hold and provide mathematical operations on arrays efficiently.

- It supports element-wise mathematical operations and various forms of generalized subscript operators, slicing and indirect access.
- As compare to vectors, valarrays are efficient in certain mathematical operations than vectors also.

#### Public member functions in valarray class :

1. `apply()` :- This function **applies the manipulation** given in its arguments **to all** the valarray elements at once and **returns a new valarray** with manipulated values.

2. `sum()` :- This function **returns the summation** of all the elements of valarrays at once.

```c++
// C++ code to demonstrate the working of
// apply() and sum()
#include<iostream>
#include<valarray> // for valarray functions
using namespace std;
int main() {
	// Initializing valarray
	valarray<int> varr = { 10, 2, 20, 1, 30 };
	
	// Declaring new valarray
	valarray<int> varr1 ;
	
	// Using apply() to increment all elements by 5
	varr1 = varr.apply([](int x){return x=x+5;});
	
	// Displaying new elements value
	cout << "The new valarray with manipulated values is : ";
	for (int &x: varr1) cout << x << " ";
	cout << endl;
	
	// Displaying sum of both old and new valarray
	cout << "The sum of old valarray is : ";
	cout << varr.sum() << endl;
	cout << "The sum of new valarray is : ";
	cout << varr1.sum() << endl;

	return 0;
	
}
```

**Output:**

```
The new valarray with manipulated values is : 15 7 25 6 35 
The sum of old valarray is : 63
The sum of new valarray is : 88
```

3. `min()` :- This function returns the **smallest** element of valarray.

4. `max()` :- This function returns the **largest** element of valarray.

```c++
// C++ code to demonstrate the working of max() and min()
#include<iostream>
#include<valarray> // for valarray functions
using namespace std;
int main() {
	// Initializing valarray
	valarray<int> varr = { 10, 2, 20, 1, 30 };
	
	// Displaying largest element of valarray
	cout << "The largest element of valarray is : ";
	cout << varr.max() << endl;
	
	// Displaying smallest element of valarray
	cout << "The smallest element of valarray is : ";
	cout << varr.min() << endl;

	return 0;
	
}
```

**Output:**

```
The largest element of valarray is : 30
The smallest element of valarray is : 1
```

5. `shift()` :- This function returns the new valarray after **shifting elements** **by** the **number** mentioned in its argument. If the **number is positive**, **left-shift** is applied, if **number is negative**, **right-shift** is applied.

6. `cshift()` :- This function returns the new valarray after **circularly shifting(rotating)** elements **by** the **number** mentioned in its argument. If the **number is positive, left-circular** **shift** is applied, if **number is negative, right-circular shift** is applied.

```c++
// C++ code to demonstrate the working of shift() and cshift()
#include<iostream>
#include<valarray> // for valarray functions
using namespace std;
int main() {
	// Initializing valarray
	valarray<int> varr = { 10, 2, 20, 1, 30 };
	
	// Declaring new valarray
	valarray<int> varr1;
	
	// using shift() to shift elements to left shifts valarray by 2 position
	varr1 = varr.shift(2);
	
	// Displaying elements of valarray after shifting
	cout << "The new valarray after shifting is : ";
	for ( int&x : varr1) cout << x << " ";
	cout << endl;
	
	// using cshift() to circulary shift elements to right
	// rotates valarray by 3 position
	varr1 = varr.cshift(-3);
	
	// Displaying elements of valarray after circular shifting
	cout << "The new valarray after circular shifting is : ";
	for ( int&x : varr1) cout << x << " ";
	cout << endl;

	return 0;
	
}
```

**Output:**

```
The new valarray after shifting is : 20 1 30 0 0 
The new valarray after circular shifting is : 20 1 30 10 2 
```

7. `swap()` :- This function **swaps** one valarray with other.

```c++
// C++ code to demonstrate the working of swap()
#include<iostream>
#include<valarray> // for valarray functions
using namespace std;
int main() {
    // Initializing 1st valarray
	valarray<int> varr1 = {1, 2, 3, 4};
	
	// Initializing 2nd valarray
	valarray<int> varr2 = {2, 4, 6, 8};
	
	// Displaying valarrays before swapping
	cout << "The contents of 1st valarray "
			"before swapping are : ";
	for (int &x : varr1)
		cout << x << " ";
	cout << endl;
	cout << "The contents of 2nd valarray "
			"before swapping are : ";
	for (int &x : varr2)
		cout << x << " ";
	cout << endl;
	
	// Use of swap() to swap the valarrays
	varr1.swap(varr2);
	
	// Displaying valarrays after swapping
	cout << "The contents of 1st valarray "
			"after swapping are : ";
	for (int &x : varr1)
		cout << x << " ";
	cout << endl;
	
	cout << "The contents of 2nd valarray "
			"after swapping are : ";
	for (int &x : varr2)
		cout << x << " ";
	cout << endl;

	return 0;
	
}
```

**Output:**

```
The contents of 1st valarray before swapping are : 1 2 3 4 
The contents of 2nd valarray before swapping are : 2 4 6 8 
The contents of 1st valarray after swapping are : 2 4 6 8 
The contents of 2nd valarray after swapping are : 1 2 3 4 
```



## 3 Containers

> Containers or container classes store objects and data. There are in total seven standard “first-class” container classes and three container adaptor classes and only seven header files that provide access to these containers or container adaptors.

### 3.1 Sequence Containers

>  Implement data structures which can be accessed in a sequential manner.

#### 3.1.1 Vector in C++ STL

Vectors are same as dynamic arrays with the ability to resize itself automatically when an element is inserted or deleted, with their storage being handled automatically by the container. Vector elements are placed in contiguous storage so that they can be accessed and traversed using iterators. In vectors, data is inserted at the end. Inserting at the end takes differential time, as sometimes there may be a need of extending the array. Removing the last element takes only constant time because no resizing happens. Inserting and erasing at the beginning or in the middle is linear in time.

Certain functions associated with the vector are:

##### Iterators

1. `begin()` – Returns an iterator pointing to the first element in the vector
2. `end()` – Returns an iterator pointing to the theoretical element that follows the last element in the vector
3. `rbegin()` – Returns a reverse iterator pointing to the last element in the vector (reverse beginning). It moves from last to first element
4. `rend()` – Returns a reverse iterator pointing to the theoretical element preceding the first element in the vector (considered as reverse end)
5. `cbegin()` – Returns a constant iterator pointing to the first element in the vector.
6. `cend()` – Returns a constant iterator pointing to the theoretical element that follows the last element in the vector.
7. `crbegin()` – Returns a constant reverse iterator pointing to the last element in the vector (reverse beginning). It moves from last to first element
8. `crend()` – Returns a constant reverse iterator pointing to the theoretical element preceding the first element in the vector (considered as reverse end)

```c++
// C++ program to illustrate the iterators in vector
#include <iostream>
#include <vector>

using namespace std;

int main() {
	vector<int> g1;

	for (int i = 1; i <= 5; i++)
		g1.push_back(i);

	cout << "Output of begin and end: ";
	for (auto i = g1.begin(); i != g1.end(); ++i)
		cout << *i << " ";

	cout << "\nOutput of cbegin and cend: ";
	for (auto i = g1.cbegin(); i != g1.cend(); ++i)
		cout << *i << " ";

	cout << "\nOutput of rbegin and rend: ";
	for (auto ir = g1.rbegin(); ir != g1.rend(); ++ir)
		cout << *ir << " ";

	cout << "\nOutput of crbegin and crend : ";
	for (auto ir = g1.crbegin(); ir != g1.crend(); ++ir)
		cout << *ir << " ";

	return 0;
}
```

**Output:**

```
Output of begin and end: 1 2 3 4 5 
Output of cbegin and cend: 1 2 3 4 5 
Output of rbegin and rend: 5 4 3 2 1 
Output of crbegin and crend : 5 4 3 2 1
```

##### Capacity

1. `size()` – Returns the number of elements in the vector.
2. `max_size()` – Returns the maximum number of elements that the vector can hold.
3. `capacity()`– Returns the size of the storage space currently allocated to the vector expressed as number of elements.
4. `resize(n)` – Resizes the container so that it contains ‘n’ elements.
5. `empty()` – Returns whether the container is empty.
6. `shrink_to_fit()` – Reduces the capacity of the container to fit its size and destroys all elements beyond the capacity.
7. `reserve()` – Requests that the vector capacity be at least enough to contain n elements.

```c++
// C++ program to illustrate the capacity function in vector
#include <iostream>
#include <vector>

using namespace std;

int main() {
	vector<int> g1;

	for (int i = 1; i <= 5; i++)
		g1.push_back(i);

	cout << "Size : " << g1.size();
	cout << "\nCapacity : " << g1.capacity();
	cout << "\nMax_Size : " << g1.max_size();

	// resizes the vector size to 4
	g1.resize(4);

	// prints the vector size after resize()
	cout << "\nSize : " << g1.size();

	// checks if the vector is empty or not
	if (g1.empty() == false)
		cout << "\nVector is not empty";
	else
		cout << "\nVector is empty";

	// Shrinks the vector
	g1.shrink_to_fit();
	cout << "\nVector elements are: ";
	for (auto it = g1.begin(); it != g1.end(); it++)
		cout << *it << " ";

	return 0;
}
```

**Output:**

```
Size : 5
Capacity : 8
Max_Size : 4611686018427387903
Size : 4
Vector is not empty
Vector elements are: 1 2 3 4
```

##### Element access:

1. reference operator `[g]` – Returns a reference to the element at position ‘g’ in the vector
2. `at(g)` – Returns a reference to the element at position ‘g’ in the vector
3. `front()` – Returns a reference to the first element in the vector
4. `back()` – Returns a reference to the last element in the vector
5. `data()` – Returns a direct pointer to the memory array used internally by the vector to store its owned elements.

```c++
// C++ program to illustrate the element accesser in vector
#include <bits/stdc++.h>
using namespace std;

int main() {
	vector<int> g1;

	for (int i = 1; i <= 10; i++)
		g1.push_back(i * 10);

	cout << "\nReference operator [g] : g1[2] = " << g1[2];

	cout << "\nat : g1.at(4) = " << g1.at(4);

	cout << "\nfront() : g1.front() = " << g1.front();

	cout << "\nback() : g1.back() = " << g1.back();

	// pointer to the first element
	int* pos = g1.data();

	cout << "\nThe first element is " << *pos;
	return 0;
}
```

**Output:**

```
Reference operator [g] : g1[2] = 30
at : g1.at(4) = 50
front() : g1.front() = 10
back() : g1.back() = 100
The first element is 10
```

##### Modifiers:

1. `assign()` – It assigns new value to the vector elements by replacing old ones
2. `push_back()` – It push the elements into a vector from the back
3. `pop_back()` – It is used to pop or remove elements from a vector from the back.
4. `insert()` – It inserts new elements before the element at the specified position
5. `erase()`– It is used to remove elements from a container from the specified position or range.
6. `swap()` – It is used to swap the contents of one vector with another vector of same type. Sizes may differ.
7. `clear()` – It is used to remove all the elements of the vector container
8. `emplace()` – It extends the container by inserting new element at position
9. `emplace_back()` – It is used to insert a new element into the vector container, the new element is added to the end of the vector

```c++
// C++ program to illustrate the Modifiers in vector
#include <bits/stdc++.h>
#include <vector>
using namespace std;

int main() {
	// Assign vector
	vector<int> v;

	// fill the array with 10 five times
	v.assign(5, 10);

	cout << "The vector elements are: ";
	for (int i = 0; i < v.size(); i++)
		cout << v[i] << " ";

	// inserts 15 to the last position
	v.push_back(15);
	int n = v.size();
	cout << "\nThe last element is: " << v[n - 1];

	// removes last element
	v.pop_back();

	// prints the vector
	cout << "\nThe vector elements are: ";
	for (int i = 0; i < v.size(); i++)
		cout << v[i] << " ";

	// inserts 5 at the beginning
	v.insert(v.begin(), 5);

	cout << "\nThe first element is: " << v[0];

	// removes the first element
	v.erase(v.begin());

	cout << "\nThe first element is: " << v[0];

	// inserts at the beginning
	v.emplace(v.begin(), 5);
	cout << "\nThe first element is: " << v[0];

	// Inserts 20 at the end
	v.emplace_back(20);
	n = v.size();
	cout << "\nThe last element is: " << v[n - 1];

	// erases the vector
	v.clear();
	cout << "\nVector size after erase(): " << v.size();

	// two vector to perform swap
	vector<int> v1, v2;
	v1.push_back(1);
	v1.push_back(2);
	v2.push_back(3);
	v2.push_back(4);

	cout << "\n\nVector 1: ";
	for (int i = 0; i < v1.size(); i++)
		cout << v1[i] << " ";

	cout << "\nVector 2: ";
	for (int i = 0; i < v2.size(); i++)
		cout << v2[i] << " ";

	// Swaps v1 and v2
	v1.swap(v2);

	cout << "\nAfter Swap \nVector 1: ";
	for (int i = 0; i < v1.size(); i++)
		cout << v1[i] << " ";

	cout << "\nVector 2: ";
	for (int i = 0; i < v2.size(); i++)
		cout << v2[i] << " ";
}
```

**Output:**

```
The vector elements are: 10 10 10 10 10 
The last element is: 15
The vector elements are: 10 10 10 10 10 
The first element is: 5
The first element is: 10
The first element is: 5
The last element is: 20
Vector size after erase(): 0

Vector 1: 1 2 
Vector 2: 3 4 
After Swap 
Vector 1: 3 4 
Vector 2: 1 2
```

#### 3.1.2 List in C++ Standard Template Library (STL)

Lists are sequence containers that allow non-contiguous memory allocation. As compared to vector, list has slow traversal, but once a position has been found, insertion and deletion are quick. Normally, when we say a List, we talk about doubly linked list. For implementing a singly linked list, we use forward list.

Below is the program to show the working of some functions of List:

```c++
#include <iostream>
#include <list>
#include <iterator>
using namespace std;

//function for printing the elements in a list
void showlist(list <int> g)
{
	list <int> :: iterator it;
	for(it = g.begin(); it != g.end(); ++it)
		cout << '\t' << *it;
	cout << '\n';
}

int main()
{

	list <int> gqlist1, gqlist2;


	for (int i = 0; i < 10; ++i)
	{
		gqlist1.push_back(i * 2);
		gqlist2.push_front(i * 3);
	}
	cout << "\nList 1 (gqlist1) is : ";
	showlist(gqlist1);

	cout << "\nList 2 (gqlist2) is : ";
	showlist(gqlist2);

	cout << "\ngqlist1.front() : " << gqlist1.front();
	cout << "\ngqlist1.back() : " << gqlist1.back();

	cout << "\ngqlist1.pop_front() : ";
	gqlist1.pop_front();
	showlist(gqlist1);

	cout << "\ngqlist2.pop_back() : ";
	gqlist2.pop_back();
	showlist(gqlist2);

	cout << "\ngqlist1.reverse() : ";
	gqlist1.reverse();
	showlist(gqlist1);

	cout << "\ngqlist2.sort(): ";
	gqlist2.sort();
	showlist(gqlist2);

	return 0;

}
```

The output of the above program is :

```
List 1 (gqlist1) is :     0    2    4    6    
8    10    12    14    16    18

List 2 (gqlist2) is :     27    24    21    18    
15    12    9    6    3    0

gqlist1.front() : 0
gqlist1.back() : 18
gqlist1.pop_front() :     2    4    6    8    
10    12    14    16    18

gqlist2.pop_back() :     27    24    21    18    
15    12    9    6    3

gqlist1.reverse() :     18    16    14    12    
10    8    6    4    2

gqlist2.sort():     3    6    9    12    
15    18    21    24    27
```

#####  Functions used with List:

- `front()` – Returns the value of the first element in the list.
- `back()` – Returns the value of the last element in the list .
- `push_front(g)` – Adds a new element ‘g’ at the beginning of the list .
- `push_back(g)` – Adds a new element ‘g’ at the end of the list.
- `pop_front()` – Removes the first element of the list, and reduces size of the list by 1.
- `pop_back()` – Removes the last element of the list, and reduces size of the list by 1
- `begin()` – **begin()** function returns an iterator pointing to the first element of the list
- `end()` – **end()** function returns an iterator pointing to the theoretical last element which follows the last element.
- `rbegin()` and `rend()` – **rbegin()** returns a reverse iterator which points to the last element of the list. **rend()** returns a reverse iterator which points to the position before the beginning of the list.
- `cbegin()` and `cend()` – **cbegin()** returns a constant random access iterator which points to the beginning of the list. **cend()** returns a constant random access iterator which points to the end of the list.
- `crbegin()` and `crend()` – **crbegin()** returns a constant reverse iterator which points to the last element of the list i.e reversed beginning of container. **crend()** returns a constant reverse iterator which points to the theoretical element preceding the first element in the list i.e. the reverse end of the list.
- `empty()` – Returns whether the list is empty(1) or not(0).
- `insert()` – Inserts new elements in the list before the element at a specified position.
- `erase()` – Removes a single element or a range of elements from the list.
- `assign()` – Assigns new elements to list by replacing current elements and resizes the list.
- `remove()` – Removes all the elements from the list, which are equal to given element.
- `remove_if()`– Used to remove all the values from the list that correspond true to the predicate or condition given as parameter to the function.
- `reverse()` – Reverses the list.
- `size()`– Returns the number of elements in the list.
- `resize()`– Used to resize a list container.
- `sort()`– Sorts the list in increasing order.
- `max_size()`– Returns the maximum number of elements a list container can hold.
- `unique()`– Removes all duplicate consecutive elements from the list.
- `emplace_front()` and `emplace_back()`– **emplace_front()** function is used to insert a new element into the list container, the new element is added to the beginning of the list. **emplace_back()** function is used to insert a new element into the list container, the new element is added to the end of the list.
- `clear()`– **clear()** function is used to remove all the elements of the list container, thus making it size 0.
- `=`– This operator is used to assign new contents to the container by replacing the existing contents.
- `swap()`– This function is used to swap the contents of one list with another list of same type and size.
- `splice()`– Used to transfer elements from one list to another.
- `merge()`– Merges two sorted lists into one
- `emplace()`– Extends list by inserting new element at a given position.

> Above are all bound methods for list. For more detailed usage of these methods, feel free to turn to Google for help.

#### 3.1.3 Deque in C++ Standard Template Library (STL)

Double ended queues are sequence containers with the feature of expansion and contraction on both the ends.
They are similar to vectors, but are more efficient in case of insertion and deletion of elements. Unlike vectors, contiguous storage allocation may not be guaranteed.
Double Ended Queues are basically an implementation of the data structure double ended queue. A queue data structure allows insertion only at the end and deletion from the front. This is like a queue in real life, wherein people are removed from the front and added at the back. Double ended queues are a special case of queues where insertion and deletion operations are possible at both the ends.

The functions for deque are same as `vector`, with an addition of push and pop operations for both front and back.

```c++
#include <iostream>
#include <deque>

using namespace std;

void showdq(deque <int> g) {
	deque <int> :: iterator it;
	for (it = g.begin(); it != g.end(); ++it)
		cout << '\t' << *it;
	cout << '\n';
}

int main() {
	deque <int> gquiz;
	gquiz.push_back(10);
	gquiz.push_front(20);
	gquiz.push_back(30);
	gquiz.push_front(15);
	cout << "The deque gquiz is : ";
	showdq(gquiz);

	cout << "\ngquiz.size() : " << gquiz.size();
	cout << "\ngquiz.max_size() : " << gquiz.max_size();

	cout << "\ngquiz.at(2) : " << gquiz.at(2);
	cout << "\ngquiz.front() : " << gquiz.front();
	cout << "\ngquiz.back() : " << gquiz.back();

	cout << "\ngquiz.pop_front() : ";
	gquiz.pop_front();
	showdq(gquiz);

	cout << "\ngquiz.pop_back() : ";
	gquiz.pop_back();
	showdq(gquiz);

	return 0;
}

```

The output of the above program is :

```
The deque gquiz is :     15    20    10    30

gquiz.size() : 4
gquiz.max_size() : 4611686018427387903
gquiz.at(2) : 10
gquiz.front() : 15
gquiz.back() : 30
gquiz.pop_front() :     20    10    30

gquiz.pop_back() :     20    10
```

##### Methods of Deque:

- `deque::insert()`: Inserts an element. And returns an iterator that points to the first of the newly inserted elements.
- `deque::rbegin()`: Returns a reverse iterator which points to the last element of the deque (i.e., its reverse beginning).
- `deque::rend()`: Returns a reverse iterator which points to the position before the beginning of the deque (which is considered its reverse end).
- `deque cbegin()`: Returns a constant iterator pointing to the first element of the container, that is, the iterator cannot be used to modify, only traverse the deque.
- `deque::max_size()`: Returns the maximum number of elements that a deque container can hold.
- `deque::assign()`: Assign values to the same or different deque container.
- `deque::resize()`: Function which changes the size of the deque.
- `deque::push_front()`: This function is used to push elements into a deque from the front.
- `deque::push_back()`: This function is used to push elements into a deque from the back.
- `deque::pop_front()` and `deque::pop_back()`: **pop_front()** function is used to pop or remove elements from a deque from the front. **pop_back()** function is used to pop or remove elements from a deque from the back.
- `deque::front()` and `deque::back()`: **front()** function is used to reference the first element of the deque container. **back()** function is used to reference the last element of the deque container.
- `deque::clear()` and `deque::erase()`: **clear()** function is used to remove all the elements of the deque container, thus making its size 0. **erase()** function is used to remove elements from a container from the specified position or range.
- `deque::empty()` and `deque::size()` : **empty()** function is used to check if the deque container is empty or not. **size()** function is used to return the size of the deque container or the number of elements in the deque container.
- `deque::=` and `deque::[]`:
  **operator=** operator is used to assign new contents to the container by replacing the existing contents. **operator[]** operator is used to reference the element present at position given inside the operator.
- `deque::at()` and `deque::swap()`: **at()** function is used reference the element present at the position given as the parameter to the function. **swap()** function is used to swap the contents of one deque with another deque of same type and size.
- `deque::begin()` and `deque::end()`: **begin()** function is used to return an iterator pointing to the first element of the deque container. **end()** function is used to return an iterator pointing to the last element of the deque container.
- `deque::emplace_front()` and `deque::emplace_back()`: **emplace_front()** function is used to insert a new element into the deque container. The new element is added to the beginning of the deque. **emplace_back()** function is used to insert a new element into the deque container. The new element is added to the end of the deque.

#### 3.1.4 Array class in C++

The introduction of array class from C++11 has offered a better alternative for C-style arrays. The advantages of array class over C-style array are :-

- Array classes knows its own size, whereas C-style arrays lack this property. So when passing to functions, we don’t need to pass size of Array as a separate parameter.
- With C-style array there is more risk of [array being decayed into a pointer](https://www.geeksforgeeks.org/what-is-array-decay-in-c-how-can-it-be-prevented/). Array classes don’t decay into pointers
- Array classes are generally more efficient, light-weight and reliable than C-style arrays.

##### Operations on array :

1. `at()` :- This function is used to access the elements of array.
2. `get()` :- This function is also used to access the elements of array. This function is not the member of array class but overloaded function from class tuple.
3. `[]` :- This is similar to C-style arrays. This method is also used to access array elements.

```c++
// C++ code to demonstrate working of array,
// to() and get()
#include<iostream>
#include<array> // for array, at()
#include<tuple> // for get()
using namespace std;
int main() {
	// Initializing the array elements
	array<int,6> ar = {1, 2, 3, 4, 5, 6};

	// Printing array elements using at()
	cout << "The array elements are (using at()) : ";
	for ( int i=0; i<6; i++)
	cout << ar.at(i) << " ";
	cout << endl;

	// Printing array elements using get()
	cout << "The array elements are (using get()) : ";
	cout << get<0>(ar) << " " << get<1>(ar) << " ";
	cout << get<2>(ar) << " " << get<3>(ar) << " ";
	cout << get<4>(ar) << " " << get<5>(ar) << " ";
	cout << endl;

	// Printing array elements using operator[]
	cout << "The array elements are (using operator[]) : ";
	for ( int i=0; i<6; i++)
	cout << ar[i] << " ";
	cout << endl;

	return 0;

}
```

**Output:**

```
The array elemets are (using at()) : 1 2 3 4 5 6 
The array elemets are (using get()) : 1 2 3 4 5 6 
The array elements are (using operator[]) : 1 2 3 4 5 6 
```

4. `front()` :- This returns the first element of array.
5. `back()` :- This returns the last element of array.

```c++
// C++ code to demonstrate working of
// front() and back()
#include<iostream>
#include<array> // for front() and back()
using namespace std;
int main() {
	// Initializing the array elements
	array<int,6> ar = {1, 2, 3, 4, 5, 6};

	// Printing first element of array
	cout << "First element of array is : ";
	cout << ar.front() << endl;

	// Printing last element of array
	cout << "Last element of array is : ";
	cout << ar.back() << endl;

	return 0;

}
```

**Output:**

```
First element of array is : 1
Last element of array is : 6
```

6. `size()` :- It returns the number of elements in array. This is a property that C-style arrays lack.

7. `max_size()` :- It returns the maximum number of elements array can hold i.e, the size with which array is declared. The `size()` and `max_size()` return the same value.

```c++
// C++ code to demonstrate working of
// size() and max_size()
#include<iostream>
#include<array> // for size() and max_size()
using namespace std;
int main() {
	// Initializing the array elements
	array<int,6> ar = {1, 2, 3, 4, 5, 6};

	// Printing number of array elements
	cout << "The number of array elements is : ";
	cout << ar.size() << endl;

	// Printing maximum elements array can hold
	cout << "Maximum elements array can hold is : ";
	cout << ar.max_size() << endl;

	return 0;

}
```

**Output:**

```
The number of array elements is : 6
Maximum elements array can hold is : 6
```

8. `swap()` :- The swap() swaps all elements of one array with other.

```c++
// C++ code to demonstrate working of swap()
#include<iostream>
#include<array> // for swap() and array
using namespace std;
int main() {

	// Initializing 1st array
	array<int,6> ar = {1, 2, 3, 4, 5, 6};

	// Initializing 2nd array
	array<int,6> ar1 = {7, 8, 9, 10, 11, 12};

	// Printing 1st and 2nd array before swapping
	cout << "The first array elements before swapping are : ";
	for (int i=0; i<6; i++)
	cout << ar[i] << " ";
	cout << endl;
	cout << "The second array elements before swapping are : ";
	for (int i=0; i<6; i++)
	cout << ar1[i] << " ";
	cout << endl;

	// Swapping ar1 values with ar
	ar.swap(ar1);

	// Printing 1st and 2nd array after swapping
	cout << "The first array elements after swapping are : ";
	for (int i=0; i<6; i++)
	cout << ar[i] << " ";
	cout << endl;
	cout << "The second array elements after swapping are : ";
	for (int i=0; i<6; i++)
	cout << ar1[i] << " ";
	cout << endl;

	return 0;

}
```

**Output:**

```
The first array elements before swapping are : 1 2 3 4 5 6 
The second array elements before swapping are : 7 8 9 10 11 12 
The first array elements after swapping are : 7 8 9 10 11 12 
The second array elements after swapping are : 1 2 3 4 5 6 
```

9. `empty()` :- This function returns true when the array size is zero else returns false.

10. `fill()` :- This function is used to fill the entire array with a particular value.

```c++
// C++ code to demonstrate working of empty()
// and fill()
#include<iostream>
#include<array> // for fill() and empty()
using namespace std;
int main() {

	// Declaring 1st array
	array<int,6> ar;

	// Declaring 2nd array
	array<int,0> ar1;

	// Checking size of array if it is empty
	ar1.empty()? cout << "Array empty":
		cout << "Array not empty";
	cout << endl;

	// Filling array with 0
	ar.fill(0);

	// Displaying array after filling
	cout << "Array after filling operation is : ";
	for ( int i=0; i<6; i++)
		cout << ar[i] << " ";

	return 0;

}
```

**Output:**

```
Array empty
Array after filling operation is : 0 0 0 0 0 0 
```

#### 3.1.5 Forward List in C++ | Set 1 (Introduction and Important Functions)

Forward list in STL implements singly linked list. Introduced from C++11, forward list are more useful than other containers in insertion, removal and moving operations (like sort) and allows time constant insertion and removal of elements. 
It differs from `list` by the fact that forward list keeps track of location of only next element while list keeps track to both next and previous elements, thus increasing the storage space required to store each element. The drawback of forward list is that it cannot be iterated backwards and its individual elements cannot be accessed directly. 
Forward List is preferred over list when only forward traversal is required (same as singly linked list is preferred over doubly linked list) as we can save space. Some example cases are, chaining in hashing, adjacency list representation of graph, etc.

##### Operations on Forward List : 

1. `assign()`:- This function is used to assign values to forward list, its another variant is used to assign repeated elements. 

```c++
// C++ code to demonstrate forward list
// and assign()
#include<iostream>
#include<forward_list>
using namespace std;

int main() {
	// Declaring forward list
	forward_list<int> flist1;

	// Declaring another forward list
	forward_list<int> flist2;

	// Assigning values using assign()
	flist1.assign({1, 2, 3});

	// Assigning repeating values using assign()
	// 5 elements with value 10
	flist2.assign(5, 10);

	// Displaying forward lists
	cout << "The elements of first forward list are : ";
	for (int&a : flist1)
		cout << a << " ";
	cout << endl;
	
	cout << "The elements of second forward list are : ";
	for (int&b : flist2)
		cout << b << " ";
	cout << endl;

	return 0;
}
```

**Output**: 

```
The elements of first forward list are : 1 2 3 
The elements of second forward list are : 10 10 10 10 10
```

2. `push_front()` :- This function is used to insert the element at the first position on forward list. The value from this function is copied to the space before first element in the container. The size of forward list increases by 1.

3. `emplace_front()` :- This function is similar to the previous function but in this no copying operation occurs, the element is created directly at the memory before the first element of the forward list.

4. `pop_front()` :- This function is used to delete the first element of list. 

```c++
// C++ code to demonstrate working of
// push_front(), emplace_front() and pop_front()
#include<iostream>
#include<forward_list>
using namespace std;

int main() {
	// Initializing forward list
	forward_list<int> flist = {10, 20, 30, 40, 50};

	// Inserting value using push_front()
	// Inserts 60 at front
	flist.push_front(60);
	
	// Displaying the forward list
	cout << "The forward list after push_front operation : ";
	for (int&c : flist)
		cout << c << " ";
	cout << endl;
	
	// Inserting value using emplace_front()
	// Inserts 70 at front
	flist.emplace_front(70);
	
	// Displaying the forward list
	cout << "The forward list after emplace_front operation : ";
	for (int&c : flist)
	cout << c << " ";
	cout << endl;
	
	// Deleting first value using pop_front()
	// Pops 70
	flist.pop_front();
	
	// Displaying the forward list
	cout << "The forward list after pop_front operation : ";
	for (int&c : flist)
		cout << c << " ";
	cout << endl;

	return 0;
}
```

**Output**: 

```
The forward list after push_front operation : 60 10 20 30 40 50 
The forward list after emplace_front operation : 70 60 10 20 30 40 50 
The forward list after pop_front operation : 60 10 20 30 40 50
```

4. `insert_after()` This function gives us a choice to insert elements at any position in forward list. The arguments in this function are copied at the desired position.
5. `emplace_after()` This function also does the same operation as above function but the elements are directly made without any copy operation.
6. `erase_after()` This function is used to erase elements from a particular position in the forward list.

```c++
// C++ code to demonstrate working of
// insert_after(), emplace_after() and erase_after()
#include<iostream>
#include<forward_list>
using namespace std;

int main() {
	// Initializing forward list
	forward_list<int> flist = {10, 20, 30} ;
	
	// Declaring a forward list iterator
	forward_list<int>::iterator ptr;

	// Inserting value using insert_after()
	// starts insertion from second position
	ptr = flist.insert_after(flist.begin(), {1, 2, 3});
	
	// Displaying the forward list
	cout << "The forward list after insert_after operation : ";
	for (int&c : flist)
		cout << c << " ";
	cout << endl;
	
	// Inserting value using emplace_after()
	// inserts 2 after ptr
	ptr = flist.emplace_after(ptr,2);
	
	// Displaying the forward list
	cout << "The forward list after emplace_after operation : ";
	for (int&c : flist)
		cout << c << " ";
	cout << endl;
	
	// Deleting value using erase.after Deleted 2
	// after ptr
	ptr = flist.erase_after(ptr);
	
	// Displaying the forward list
	cout << "The forward list after erase_after operation : ";
	for (int&c : flist)
		cout << c << " ";
	cout << endl;

	return 0;
}
```

Output: 

```
The forward list after insert_after operation : 10 1 2 3 20 30 
The forward list after emplace_after operation : 10 1 2 3 2 20 30 
The forward list after erase_after operation : 10 1 2 3 2 30
```

7. `remove()` :- This function removes the particular element from the forward list mentioned in its argument.
8. `remove_if()` :- This function removes according to the condition in its argument.

```c++
// C++ code to demonstrate working of remove() and
// remove_if()
#include<iostream>
#include<forward_list>
using namespace std;

int main() {
	// Initializing forward list
	forward_list<int> flist = {10, 20, 30, 25, 40, 40};
	
	// Removing element using remove()
	// Removes all occurrences of 40
	flist.remove(40);
	
	// Displaying the forward list
	cout << "The forward list after remove operation : ";
	for (int&c : flist)
		cout << c << " ";
	cout << endl;
	
	// Removing according to condition. Removes
	// elements greater than 20. Removes 25 and 30
	flist.remove_if([](int x){ return x>20;});
	
	// Displaying the forward list
	cout << "The forward list after remove_if operation : ";
	for (int&c : flist)
	cout << c << " ";
	cout << endl;

	return 0;

}
```

**Output**: 

```
The forward list after remove operation : 10 20 30 25 
The forward list after remove_if operation : 10 20
```

9. `splice_after()`:- This function transfers elements from one forward list to other. 

```c++
// C++ code to demonstrate working of
// splice_after()
#include<iostream>
#include<forward_list> // for splice_after()
using namespace std;

int main() {
	// Initializing forward list
	forward_list<int> flist1 = {10, 20, 30};
	
	// Initializing second list
	forward_list<int> flist2 = {40, 50, 60};
	
	// Shifting elements from first to second
	// forward list after 1st position
	flist2.splice_after(flist2.begin(),flist1);
	
	// Displaying the forward list
	cout << "The forward list after splice_after operation : ";
	for (int&c : flist2)
	cout << c << " ";
	cout << endl;

	return 0;
}
```

Output: 

```
The forward list after splice_after operation : 40 10 20 30 50 60
```

##### Some more methods of forward_list: 

- `front()`– This function is used to reference the first element of the forward list container.
- `begin()` – begin() function is used to return an iterator pointing to the first element of the forward list container.
- `end()`– end() function is used to return an iterator pointing to the last element of the list container.
- `cbegin()` – Returns a constant iterator pointing to the first element of the forward_list.
- `cend()` – Returns a constant iterator pointing to the past-the-last element of the forward_list.
- `before_begin()` – Returns a iterator which points to the position before the first element of the forward_list.
- `cbefore_begin()` – Returns a constant random access iterator which points to the position before the first element of the forward_list.
- `max_size()` – Returns the maximum number of elements can be held by forward_list.
- `resize()` – Changes the size of forward_list.

#### 3.1.6 Forward List in C++ | Set 2 (Manipulating Functions)

Some of the operations other than insertions and deletions that can be used in forward lists are as follows :

1. `merge()` :- This function is used to merge one forward list with other. If both the lists are sorted then the resulted list returned is also sorted.

2. `=` :- This operator copies one forward list into other. The copy made in this case is deep copy.

```c++
// C++ code to demonstrate the working of
// merge() and operator=
#include<iostream>
#include<forward_list>
using namespace std;

int main() {	
	// Initializing 1st forward list
	forward_list<int> flist1 = {1, 2, 3};
	
	// Declaring 2nd forward list
	forward_list<int> flist2;
	
	// Creating deep copy using "="
	flist2 = flist1;
	
	// Displaying flist2
	cout << "The contents of 2nd forward list"
			" after copy are : ";
	for (int &x : flist2)
		cout << x << " ";
	cout << endl;
	
	// Using merge() to merge both list in 1
	flist1.merge(flist2);
	
	// Displaying merged forward list
	// Prints sorted list
	cout << "The contents of forward list "
			"after merge are : ";
	for (int &x : flist1)
		cout << x << " ";
	cout << endl;
	
	return 0;	
}
```

**Output**:

```
The contents of 2nd forward list after copy are : 1 2 3 
The contents of forward list after merge are : 1 1 2 2 3 3 
```

3. `sort()`:- This function is used to sort the forward list.

4. `unique()` :- This function deletes the multiple occurrences of a number and returns a forward list with unique elements. The forward list should be sorted for this function to execute successfully.

```c++
// C++ code to demonstrate the working of
// sort() and unique()
#include<iostream>
#include<forward_list> // for sort() and unique()
using namespace std;

int main() {
	// Initializing 1st forward list
	forward_list<int> flist1 = {1, 2, 3, 2, 3, 3, 1};

	// Sorting the forward list using sort()
	flist1.sort();

	// Displaying sorted forward list
	cout << "The contents of forward list after "
			"sorting are : ";
	for (int &x : flist1)
		cout << x << " ";
	cout << endl;

	// Use of unique() to remove repeated occurrences
	flist1.unique();

	// Displaying forward list after using unique()
	cout << "The contents of forward list after "
			"unique operation are : ";
	for (int &x : flist1)
		cout << x << " ";
	cout << endl;

	return 0;
}
```

**Output**:

```
The contents of forward list after sorting are : 1 1 2 2 3 3 3 
The contents of forward list after unique operation are : 1 2 3 
```

5. `reverse()` :- This function is used to reverse the forward list.

6. `swap()`:- This function swaps the content of one forward list with other.

```c++
// C++ code to demonstrate the working of
// reverse() and swap()
#include<iostream>
#include<forward_list> // for reverse() and swap()
using namespace std;
int main() {
	// Initializing 1st forward list
	forward_list<int> flist1 = {1, 2, 3,};

	// Initializing 2nd forward list
	forward_list<int> flist2 = {4, 5, 6};

	// Using reverse() to reverse 1st forward list
	flist1.reverse();

	// Displaying reversed forward list
	cout << "The contents of forward list after"
			" reversing are : ";
	for (int &x : flist1)
		cout << x << " ";
	cout << endl << endl;

	// Displaying forward list before swapping
	cout << "The contents of 1st forward list "
			"before swapping are : ";
	for (int &x : flist1)
		cout << x << " ";
	cout << endl;
	cout << "The contents of 2nd forward list "
			"before swapping are : ";
	for (int &x : flist2)
		cout << x << " ";
	cout << endl;

	// Use of swap() to swap the list
	flist1.swap(flist2);

	// Displaying forward list after swapping
	cout << "The contents of 1st forward list "
			"after swapping are : ";
	for (int &x : flist1)
		cout << x << " ";
	cout << endl;

	cout << "The contents of 2nd forward list "
			"after swapping are : ";
	for (int &x : flist2)
		cout << x << " ";
	cout << endl;

	return 0;
}
```

**Output**:

```
The contents of forward list after reversing are : 3 2 1 

The contents of 1st forward list before swapping are : 3 2 1 
The contents of 2nd forward list before swapping are : 4 5 6 
The contents of 1st forward list after swapping are : 4 5 6 
The contents of 2nd forward list after swapping are : 3 2 1 
```

7. `clear()`:- This function clears the contents of forward list. After this function, the forward list becomes empty.

8. `empty()` :- This function returns true if the list is empty otherwise false.

```c++
// C++ code to demonstrate the working of
// clear() and empty()
#include<iostream>
#include<forward_list> // for clear() and empty()
using namespace std;
int main() {	
	// Initializing forward list
	forward_list<int> flist1 = {1, 2, 3,};
	
	// Displaying forward list before clearing
	cout << "The contents of forward list are : ";
	for (int &x : flist1)
		cout << x << " ";
	cout << endl;
	
	// Using clear() to clear the forward list
	flist1.clear();
	
	// Displaying list after clear() performed
	cout << "The contents of forward list after "
		<< "clearing are : ";
	for (int &x : flist1)
		cout << x << " ";
	cout << endl;
	
	// Checking if list is empty
	flist1.empty() ? cout << "Forward list is empty" :
					cout << "Forward list is not empty";
	
	return 0;	
}
```

**Output**:

```
The contents of forward list  are : 1 2 3 
The contents of forward list after clearing are : 
Forward list is empty
```

### 3.2 Container Adaptors

#### 3.2.1 Queue in Standard Template Library (STL)

Queues are a type of container adaptors which operate in a first in first out (FIFO) type of arrangement. Elements are inserted at the back (end) and are deleted from the front. Queues use an encapsulated object of `deque`(sequential container class) as its underlying container, providing a specific set of member functions to access its elements.

#####  The functions supported by queue are :

1. `empty()`– Returns whether the queue is empty.
2. `size()` – Returns the size of the queue.
3. `queue::swap()` : Exchange the contents of two queues but the queues must be of same type, although sizes may differ.
4. `queue::emplace()` : Insert a new element into the queue container, the new element is added to the end of the queue.
5. `queue::front()` and `queue::back()` – **front()** function returns a reference to the first element of the queue. **back()** function returns a reference to the last element of the queue.
6. `push(g)` and `pop()` – **push()** function adds the element ‘g’ at the end of the queue. **pop()** function deletes the first element of the queue.

```c++
// CPP code to illustrate
// Queue in Standard Template Library (STL)
#include <iostream>
#include <queue>

using namespace std;

// Print the queue
void showq(queue<int> gq) {
	queue<int> g = gq;
	while (!g.empty()) {
		cout << '\t' << g.front();
		g.pop();
	}
	cout << '\n';
}

// Driver Code
int main() {
	queue<int> gquiz;
	gquiz.push(10);
	gquiz.push(20);
	gquiz.push(30);

	cout << "The queue gquiz is : ";
	showq(gquiz);

	cout << "\ngquiz.size() : " << gquiz.size();
	cout << "\ngquiz.front() : " << gquiz.front();
	cout << "\ngquiz.back() : " << gquiz.back();

	cout << "\ngquiz.pop() : ";
	gquiz.pop();
	showq(gquiz);

	return 0;
}
```

**Output**

```
The queue gquiz is :     10    20    30

gquiz.size() : 3
gquiz.front() : 10
gquiz.back() : 30
gquiz.pop() :     20    30
```

#### 3.2.2 Priority Queue in C++ Standard Template Library (STL)

Priority queues are a type of container adapters, specifically designed such that the first element of the queue is the greatest of all elements in the queue and elements are in non increasing order (hence we can see that each element of the queue has a priority {fixed order}).

```c++
// Note that by default C++ creates a max-heap
// for priority queue
#include <iostream>
#include <queue>

using namespace std;

void showpq(priority_queue<int> gq) {
	priority_queue<int> g = gq;
	while (!g.empty()) {
		cout << '\t' << g.top();
		g.pop();
	}
	cout << '\n';
}

int main() {
	priority_queue<int> gquiz;
	gquiz.push(10);
	gquiz.push(30);
	gquiz.push(20);
	gquiz.push(5);
	gquiz.push(1);

	cout << "The priority queue gquiz is : ";
	showpq(gquiz);

	cout << "\ngquiz.size() : " << gquiz.size();
	cout << "\ngquiz.top() : " << gquiz.top();

	cout << "\ngquiz.pop() : ";
	gquiz.pop();
	showpq(gquiz);

	return 0;
}
```

**Output:** 

```
The priority queue gquiz is :     30    20    10    5    1

gquiz.size() : 5
gquiz.top() : 30
gquiz.pop() :     20    10    5    1
```

##### How to create a min heap for priority queue? 

C++ provides below syntax for the same. 

```c++
// Syntax to create a min heap for priority queue 
priority_queue <int, vector<int>, greater<int>> g = gq;  
```

```c++
// C++ program to demonstrate min heap
#include <iostream>
#include <queue>

using namespace std;

void showpq(priority_queue<int, vector<int>, greater<int> > gq) {
	priority_queue<int, vector<int>, greater<int> > g = gq;
	while (!g.empty()) {
		cout << '\t' << g.top();
		g.pop();
	}
	cout << '\n';
}

int main() {
	priority_queue<int, vector<int>, greater<int> > gquiz;
	gquiz.push(10);
	gquiz.push(30);
	gquiz.push(20);
	gquiz.push(5);
	gquiz.push(1);

	cout << "The priority queue gquiz is : ";
	showpq(gquiz);

	cout << "\ngquiz.size() : " << gquiz.size();
	cout << "\ngquiz.top() : " << gquiz.top();

	cout << "\ngquiz.pop() : ";
	gquiz.pop();
	showpq(gquiz);

	return 0;
}
```

**Output:** 

```
The priority queue gquiz is :     1    5    10    20    30

gquiz.size() : 5
gquiz.top() : 1
gquiz.pop() :     5    10    20    30
```

**Note :** The above syntax may be difficult to remember, so in case of numeric values, we can multiply the values with -1 and use max heap to get the effect of min heap.

##### Methods of priority queue are:

- `priority_queue::empty()`– **empty()** function returns whether the queue is empty.
- `priority_queue::size()` – **size()** function returns the size of the queue.
- `priority_queue::top()` – Returns a reference to the top most element of the queue
- `priority_queue::push()` - **push(g)** function adds the element ‘g’ at the end of the queue.
- `priority_queue::pop()` – **pop()** function deletes the first element of the queue.
- `priority_queue::swap()` – This function is used to swap the contents of one priority queue with another priority queue of same type and size.
- `priority_queue::emplace()` – This function is used to insert a new element into the priority queue container, the new element is added to the top of the priority queue.
- `priority_queue::value_type` – Represents the type of object stored as an element in a priority_queue. It acts as a synonym for the template parameter.

#### 3.2.3 Stack in C++ STL

Stacks are a type of container adaptors with LIFO(Last In First Out) type of working, where a new element is added at one end and (top) an element is removed from that end only.  Stack uses an encapsulated object of either `vector`(by default) or `list` (sequential container class) as its underlying container, providing a specific set of member functions to access its elements.

##### The functions associated with stack are: 

- `empty()` – Returns whether the stack is empty – Time Complexity : O(1) 
- `size()` – Returns the size of the stack – Time Complexity : O(1) 
- `top()` – Returns a reference to the top most element of the stack – Time Complexity : O(1) 
- `push(g)` – Adds the element ‘g’ at the top of the stack – Time Complexity : O(1) 
- `pop()` – Deletes the top most element of the stack – Time Complexity : O(1) 

```c++
// CPP program to demonstrate working of STL stack
#include <bits/stdc++.h>
using namespace std;

void showstack(stack <int> s) {
	while (!s.empty()) {
		cout << '\t' << s.top();
		s.pop();
	}
	cout << '\n';
}

int main () {
	stack <int> s;
	s.push(10);
	s.push(30);
	s.push(20);
	s.push(5);
	s.push(1);

	cout << "The stack is : ";
	showstack(s);

	cout << "\ns.size() : " << s.size();
	cout << "\ns.top() : " << s.top();


	cout << "\ns.pop() : ";
	s.pop();
	showstack(s);

	return 0;
}
```

**Output:**

```
The stack is :     1    5    20    30    10

s.size() : 5
s.top() : 1
s.pop() :     5    20    30    10
```

##### List of functions of Stack: 

- `stack::top()`
- `stack::empty()` and `stack::size()` 
- `stack::push()` and `stack::pop()`
- `stack::swap()`
- `stack::emplace()`

### 3.3 Associative Containers

#### 3.3.1 Set in C++ Standard Template Library (STL)

Sets are a type of associative containers in which each element has to be unique, because the value of the element identifies it. The value of the element cannot be modified once it is added to the set, though it is possible to remove and add the modified value of that element. 

##### Some basic functions associated with Set:

- `begin()` – Returns an iterator to the first element in the set.
- `end()` – Returns an iterator to the theoretical element that follows last element in the set.
- `size()` – Returns the number of elements in the set.
- `max_size()` – Returns the maximum number of elements that the set can hold.
- `empty()` – Returns whether the set is empty.

```c++
#include <iostream>
#include <iterator>
#include <set>

using namespace std;

int main() {
	// empty set container
	set<int, greater<int> > s1;

	// insert elements in random order
	s1.insert(40);
	s1.insert(30);
	s1.insert(60);
	s1.insert(20);
	s1.insert(50);
	
	// only one 50 will be added to the set
	s1.insert(50);
	s1.insert(10);

	// printing set s1
	set<int, greater<int> >::iterator itr;
	cout << "\nThe set s1 is : \n";
	for (itr = s1.begin(); itr != s1.end(); itr++) {
		cout << *itr<<" ";
	}
	cout << endl;

	// assigning the elements from s1 to s2
	set<int> s2(s1.begin(), s1.end());

	// print all elements of the set s2
	cout << "\nThe set s2 after assign from s1 is : \n";
	for (itr = s2.begin(); itr != s2.end(); itr++) {
		cout << *itr<<" ";
	}
	cout << endl;

	// remove all elements up to 30 in s2
	cout
		<< "\ns2 after removal of elements less than 30 :\n";
	s2.erase(s2.begin(), s2.find(30));
	for (itr = s2.begin(); itr != s2.end(); itr++) {
		cout <<*itr<<" ";
	}

	// remove element with value 50 in s2
	int num;
	num = s2.erase(50);
	cout << "\ns2.erase(50) : ";
	cout << num << " removed\n";
	for (itr = s2.begin(); itr != s2.end(); itr++)
	{
		cout <<*itr<<" ";
	}

	cout << endl;

	// lower bound and upper bound for set s1
	cout << "s1.lower_bound(40) : \n"
		<< *s1.lower_bound(40)
		<< endl;
	cout << "s1.upper_bound(40) : \n"
		<< *s1.upper_bound(40)
		<< endl;

	// lower bound and upper bound for set s2
	cout << "s2.lower_bound(40) :\n"
		<< *s2.lower_bound(40)
		<< endl;
	cout << "s2.upper_bound(40) : \n"
		<< *s2.upper_bound(40)
		<< endl;

	return 0;
}
```

**Output**

```
The set s1 is : 
60 50 40 30 20 10 

The set s2 after assign from s1 is : 
10 20 30 40 50 60 

s2 after removal of elements less than 30 :
30 40 50 60 
s2.erase(50) : 1 removed
30 40 60 
s1.lower_bound(40) : 
40
s1.upper_bound(40) : 
30
s2.lower_bound(40) :
40
s2.upper_bound(40) : 
60
```

##### Methods of set:

- `begin()` – Returns an iterator to the first element in the set.
- `end()` – Returns an iterator to the theoretical element that follows last element in the set.
- `rbegin()` – Returns a reverse iterator pointing to the last element in the container.
- `rend()` – Returns a reverse iterator pointing to the theoretical element right before the first element in the set container.
- `crbegin()` – Returns a constant iterator pointing to the last element in the container.
- `crend()` – Returns a constant iterator pointing to the position just before the first element in the container.
- `cbegin()` – Returns a constant iterator pointing to the first element in the container.
- `cend()` – Returns a constant iterator pointing to the position past the last element in the container.
- `size()` – Returns the number of elements in the set.
- `max_size()` – Returns the maximum number of elements that the set can hold.
- `empty()` – Returns whether the set is empty.
- `insert(const g)` – Adds a new element ‘g’ to the set.
- `iterator insert (iterator position, const g)` – Adds a new element ‘g’ at the position pointed by iterator.
- `erase(iterator position)` – Removes the element at the position pointed by the iterator.
- `erase(const g)` – Removes the value ‘g’ from the set.
- `clear()` – Removes all the elements from the set.
- `key_comp()` / `value_comp()`– Returns the object that determines how the elements in the set are ordered (‘<‘ by default).
- `find(const g)` – Returns an iterator to the element ‘g’ in the set if found, else returns the iterator to end.
- `count(const g)` – Returns 1 or 0 based on the element ‘g’ is present in the set or not.
- `lower_bound(const g)` – Returns an iterator to the first element that is equivalent to ‘g’ or definitely will not go before the element ‘g’ in the set.
- `upper_bound(const g)` – Returns an iterator to the first element that will go after the element ‘g’ in the set.
- `equal_range()` – The function returns an iterator of pairs. (key_comp). The pair refers to the range that includes all the elements in the container which have a key equivalent to k.
- `emplace()` – This function is used to insert a new element into the set container, only if the element to be inserted is unique and does not already exists in the set.
- `emplace_hint()` – Returns an iterator pointing to the position where the insertion is done. If the element passed in the parameter already exists, then it returns an iterator pointing to the position where the existing element is.
- `swap()` – This function is used to exchange the contents of two sets but the sets must be of same type, although sizes may differ.
- `=` – The ‘=’ is an operator in C++ STL which copies (or moves) a set to another set and set::operator= is the corresponding operator function.
- `get_allocator()` – Returns the copy of the allocator object associated with the set.

#### 3.3.2 Multiset in C++ Standard Template Library (STL)

Multisets are a type of associative containers similar to set, with an exception that multiple elements can have same values.
Some Basic Functions associated with multiset: 

- `begin()` – Returns an iterator to the first element in the multiset 

- `end()` – Returns an iterator to the theoretical element that follows last element in the multiset 

- `size()`– Returns the number of elements in the multiset 

- `max_size()` – Returns the maximum number of elements that the multiset can hold 

- `empty()` – Returns whether the multiset is empty

**Implementation**: 

```c++
#include <iostream>
#include <set>
#include <iterator>

using namespace std;

int main() {
	// empty multiset container
	multiset <int, greater <int> > gquiz1;		

	// insert elements in random order
	gquiz1.insert(40);
	gquiz1.insert(30);
	gquiz1.insert(60);
	gquiz1.insert(20);
	gquiz1.insert(50);
	
	// 50 will be added again to
	// the multiset unlike set
	gquiz1.insert(50);
	gquiz1.insert(10);

	// printing multiset gquiz1
	multiset <int, greater <int> > :: iterator itr;
	cout << "\nThe multiset gquiz1 is : \n";
	for (itr = gquiz1.begin(); itr!= gquiz1.end(); ++itr) {
		cout << *itr << " ";
	}
	cout << endl;

	// assigning the elements from gquiz1 to gquiz2
	multiset <int> gquiz2(gquiz1.begin(), gquiz1.end());

	// print all elements of the multiset gquiz2
	cout << "\nThe multiset gquiz2 \n"
			"after assign from gquiz1 is : \n";
	for (itr = gquiz2.begin(); itr
		!= gquiz2.end(); ++itr)
	{
		cout << *itr <<" ";
	}
	cout << endl;

	// remove all elements up to element with value 30 in gquiz2
	cout << "\ngquiz2 after removal \n"
			"of elements less than 30 : \n";
	gquiz2.erase(gquiz2.begin()
				, gquiz2.find(30));
	for (itr = gquiz2.begin(); itr!= gquiz2.end(); ++itr) {
		cout << *itr << " " ;
	}

	// remove all elements with value 50 in gquiz2
	int num;
	num = gquiz2.erase(50);
	cout << "\ngquiz2.erase(50) : \n";
	cout << num << " removed \n" ;
	for (itr = gquiz2.begin(); itr!= gquiz2.end(); ++itr) {
		cout << *itr << " ";
	}

	cout << endl;

	//lower bound and upper bound for multiset gquiz1
	cout << "\ngquiz1.lower_bound(40) : \n"
		<< *gquiz1.lower_bound(40) << endl;
	cout << "gquiz1.upper_bound(40) : \n"
		<< *gquiz1.upper_bound(40) << endl;

	//lower bound and upper bound for multiset gquiz2
	cout << "gquiz2.lower_bound(40) : \n"
		<< *gquiz2.lower_bound(40) << endl;
	cout << "gquiz2.upper_bound(40) : \n"
		<< *gquiz2.upper_bound(40) << endl;
		
		return 0;

}
```

**Output**

```
The multiset gquiz1 is : 
60 50 50 40 30 20 10 

The multiset gquiz2 
after assign from gquiz1 is : 
10 20 30 40 50 50 60 

gquiz2 after removal 
of elements less than 30 : 
30 40 50 50 60 
gquiz2.erase(50) : 
2 removed 
30 40 60 

gquiz1.lower_bound(40) : 
40
gquiz1.upper_bound(40) : 
30
gquiz2.lower_bound(40) : 
40
gquiz2.upper_bound(40) : 
60
```

#####  Removing Element from multiset which have same value

- `a.erase()` – Remove all instance of element from multiset having same value
- `a.erase(a.find())` – Remove only one instance of element from multiset having same value

```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
	multiset<int> a;
	a.insert(10);
	a.insert(10);
	a.insert(10);

	// it will give output 3
	cout << a.count(10) << endl;

	// removing single instance from multiset
	
	// it will remove only one value of 10 from multiset
	a.erase(a.find(10));
	
	// it will give output 2
	cout << a.count(10) << endl;

	// removing all instance of element from multiset it will remove all instance of value 10
	a.erase(10);
	
	// it will give output 0 because all instance of value is removed from mulitset
	cout << a.count(10)
		<< endl;

	return 0;
}
```

**Output**

```
3
2
0
```

##### List of functions of Multiset:

- `begin()` – Returns an iterator to the first element in the multiset.
- `end()` – Returns an iterator to the theoretical element that follows last element in the multiset.
- `size()` – Returns the number of elements in the multiset.
- `max_size()` – Returns the maximum number of elements that the multiset can hold.
- `empty()` – Returns whether the multiset is empty.
- `pair insert(const g)` – Adds a new element ‘g’ to the multiset.
- `iterator insert (iterator position,const g)` – Adds a new element ‘g’ at the position pointed by iterator.
- `erase(iterator position)` – Removes the element at the position pointed by the iterator.
- `erase(const g)` – Removes the value ‘g’ from the multiset.
- `clear()` – Removes all the elements from the multiset.
- `key_comp()` / `value_comp()` – Returns the object that determines how the elements in the multiset are ordered (‘<‘ by default).
- `find(const g)` – Returns an iterator to the element ‘g’ in the multiset if found, else returns the iterator to end.
- `count(const g)` – Returns the number of matches to element ‘g’ in the multiset.
- `lower_bound(const g)` – Returns an iterator to the first element that is equivalent to ‘g’ or definitely will not go before the element ‘g’ in the multiset.
- `upper_bound(const g)` – Returns an iterator to the first element that is equivalent to ‘g’ or definitely will go after the element ‘g’ in the multiset.
- `multiset::swap()` – This function is used to exchange the contents of two multisets but the sets must be of same type, although sizes may differ.
- `multiset::operator=` – This operator is used to assign new contents to the container by replacing the existing contents.
- `multiset::emplace()` – This function is used to insert a new element into the multiset container.
- `multiset equal_range()` – Returns an iterator of pairs. The pair refers to the range that includes all the elements in the container which have a key equivalent to k.
- `multiset::emplace_hint()` – Inserts a new element in the multiset.
- `multiset::rbegin()` – Returns a reverse iterator pointing to the last element in the multiset container.
- `multiset::rend()` – Returns a reverse iterator pointing to the theoretical element right before the first element in the multiset container.
- `multiset::cbegin()` – Returns a constant iterator pointing to the first element in the container.
- `multiset::cend()` – Returns a constant iterator pointing to the position past the last element in the container.
- `multiset::crbegin()` – Returns a constant reverse iterator pointing to the last element in the container.
- `multiset::crend()` – Returns a constant reverse iterator pointing to the position just before the first element in the container.
- `multiset::get_allocator()` – Returns a copy of the allocator object associated with the multiset.

#### 3.3.3 Map in C++ Standard Template Library (STL)

Maps are associative containers that store elements in a mapped fashion. Each element has a key value and a mapped value. No two mapped values can have same key values.

##### Some basic functions associated with Map:

- `begin()` – Returns an iterator to the first element in the map

- `end()`– Returns an iterator to the theoretical element that follows last element in the map

- `size()` – Returns the number of elements in the map

- `max_size()` – Returns the maximum number of elements that the map can hold

- `empty()` – Returns whether the map is empty

- `pair insert(keyvalue, mapvalue)` – Adds a new element to the map

- `erase(iterator position)` – Removes the element at the position pointed by the iterator

- `erase(const g)` – Removes the key value ‘g’ from the map

- `clear()` – Removes all the elements from the map

```c++
#include <iostream>
#include <iterator>
#include <map>

using namespace std;

int main() {

	// empty map container
	map<int, int> gquiz1;

	// insert elements in random order
	gquiz1.insert(pair<int, int>(1, 40));
	gquiz1.insert(pair<int, int>(2, 30));
	gquiz1.insert(pair<int, int>(3, 60));
	gquiz1.insert(pair<int, int>(4, 20));
	gquiz1.insert(pair<int, int>(5, 50));
	gquiz1.insert(pair<int, int>(6, 50));
	gquiz1.insert(pair<int, int>(7, 10));

	// printing map gquiz1
	map<int, int>::iterator itr;
	cout << "\nThe map gquiz1 is : \n";
	cout << "\tKEY\tELEMENT\n";
	for (itr = gquiz1.begin(); itr != gquiz1.end(); ++itr) {
		cout << '\t' << itr->first
			<< '\t' << itr->second << '\n';
	}
	cout << endl;

	// assigning the elements from gquiz1 to gquiz2
	map<int, int> gquiz2(gquiz1.begin(), gquiz1.end());

	// print all elements of the map gquiz2
	cout << "\nThe map gquiz2 after"
		<< " assign from gquiz1 is : \n";
	cout << "\tKEY\tELEMENT\n";
	for (itr = gquiz2.begin(); itr != gquiz2.end(); ++itr) {
		cout << '\t' << itr->first
			<< '\t' << itr->second << '\n';
	}
	cout << endl;

	// remove all elements up to
	// element with key=3 in gquiz2
	cout << "\ngquiz2 after removal of"
			" elements less than key=3 : \n";
	cout << "\tKEY\tELEMENT\n";
	gquiz2.erase(gquiz2.begin(), gquiz2.find(3));
	for (itr = gquiz2.begin(); itr != gquiz2.end(); ++itr) {
		cout << '\t' << itr->first
			<< '\t' << itr->second << '\n';
	}

	// remove all elements with key = 4
	int num;
	num = gquiz2.erase(4);
	cout << "\ngquiz2.erase(4) : ";
	cout << num << " removed \n";
	cout << "\tKEY\tELEMENT\n";
	for (itr = gquiz2.begin(); itr != gquiz2.end(); ++itr) {
		cout << '\t' << itr->first
			<< '\t' << itr->second << '\n';
	}

	cout << endl;

	// lower bound and upper bound for map gquiz1 key = 5
	cout << "gquiz1.lower_bound(5) : "
		<< "\tKEY = ";
	cout << gquiz1.lower_bound(5)->first << '\t';
	cout << "\tELEMENT = "
		<< gquiz1.lower_bound(5)->second << endl;
	cout << "gquiz1.upper_bound(5) : "
		<< "\tKEY = ";
	cout << gquiz1.upper_bound(5)->first << '\t';
	cout << "\tELEMENT = "
		<< gquiz1.upper_bound(5)->second << endl;

	return 0;
}
```

**Output:**

```
The map gquiz1 is : 
    KEY    ELEMENT
    1    40
    2    30
    3    60
    4    20
    5    50
    6    50
    7    10


The map gquiz2 after assign from gquiz1 is : 
    KEY    ELEMENT
    1    40
    2    30
    3    60
    4    20
    5    50
    6    50
    7    10


gquiz2 after removal of elements less than key=3 : 
    KEY    ELEMENT
    3    60
    4    20
    5    50
    6    50
    7    10

gquiz2.erase(4) : 1 removed 
    KEY    ELEMENT
    3    60
    5    50
    6    50
    7    10

gquiz1.lower_bound(5) :     KEY = 5        ELEMENT = 50
gquiz1.upper_bound(5) :     KEY = 6        ELEMENT = 50
```

##### List of all functions of Map:

- `map::insert()`– Insert elements with a particular key in the map container. .
- `map::count()` – Returns the number of matches to element with key value ‘g’ in the map.
- `map::equal_range()` – Returns an iterator of pairs. The pair refers to the bounds of a range that includes all the elements in the container which have a key equivalent to k.
- `map::erase()` – Used to erase element from the container.
- `map::rend()` – Returns a reverse iterator pointing to the theoretical element right before the first key-value pair in the map(which is considered its reverse end).
- `map::rbegin()` – Returns a reverse iterator which points to the last element of the map.
- `map::find()` – Returns an iterator to the element with key value ‘g’ in the map if found, else returns the iterator to end.
- `map::crbegin()` and `crend()` – **crbegin()** returns a constant reverse iterator referring to the last element in the map container. **crend()** returns a constant reverse iterator pointing to the theoretical element before the first element in the map.
- `map::cbegin()` and `cend()` – **cbegin()** returns a constant iterator referring to the first element in the map container. **cend()** returns a constant iterator pointing to the theoretical element that follows last element in the multimap.
- `map::emplace()` – Inserts the key and its element in the map container.
- `map::max_size()` – Returns the maximum number of elements a map container can hold.
- `map::upper_bound()` – Returns an iterator to the first element that is equivalent to mapped value with key value ‘g’ or definitely will go after the element with key value ‘g’ in the map
- `map::operator=` – Assigns contents of a container to a different container, replacing its current content.
- `map::lower_bound()` – Returns an iterator to the first element that is equivalent to mapped value with key value ‘g’ or definitely will not go before the element with key value ‘g’ in the map.
- `map::emplace_hint()` – Inserts the key and its element in the map container with a given hint.
- `map::value_comp()` – Returns the object that determines how the elements in the map are ordered (‘<' by default).
- `map::key_comp()` – Returns the object that determines how the elements in the map are ordered (‘<' by default).
- `map::size()` – Returns the number of elements in the map.
- `map::empty()` – Returns whether the map is empty.
- `map::begin()` and `end()` – **begin()** returns an iterator to the first element in the map. **end()** returns an iterator to the theoretical element that follows last element in the map
- `map::operator[]` – This operator is used to reference the element present at position given inside the operator.
- `map::clear()` – Removes all the elements from the map.
- `map::at()` and `map::swap()` – **at()** function is used to return the reference to the element associated with the key k. **swap()** function is used to exchange the contents of two maps but the maps must be of same type, although sizes may differ.

#### 3.3.4 Multimap in C++ Standard Template Library

Multimap is similar to `map` with an addition that multiple elements can have same keys. Also, it is NOT required that the key value and mapped value pair has to be unique in this case. One important thing to note about multimap is that multimap keeps all the keys in sorted order always. These properties of multimap makes it very much useful in competitive programming.

##### Some Basic Functions associated with multimap:

- `begin()` – Returns an iterator to the first element in the multimap
- `end()` – Returns an iterator to the theoretical element that follows last element in the multimap
- `size()` – Returns the number of elements in the multimap
- `max_size()` – Returns the maximum number of elements that the multimap can hold
- `empty()` – Returns whether the multimap is empty
- `pair insert(keyvalue,multimapvalue` – Adds a new element to the multimap

##### C++ implementation to illustrate above functions

```c++
#include <iostream>
#include <map>
#include <iterator>

using namespace std;

int main() {
	multimap <int, int> gquiz1; // empty multimap container

	// insert elements in random order
	gquiz1.insert(pair <int, int> (1, 40));
	gquiz1.insert(pair <int, int> (2, 30));
	gquiz1.insert(pair <int, int> (3, 60));
	gquiz1.insert(pair <int, int> (6, 50));
	gquiz1.insert(pair <int, int> (6, 10));

	// printing multimap gquiz1
	multimap <int, int> :: iterator itr;
	cout << "\nThe multimap gquiz1 is : \n";
	cout << "\tKEY\tELEMENT\n";
	for (itr = gquiz1.begin(); itr != gquiz1.end(); ++itr) {
		cout << '\t' << itr->first
			<< '\t' << itr->second << '\n';
	}
	cout << endl;

	//adding elements randomly,
	// to check the sorted keys property
	gquiz1.insert(pair <int, int> (4, 50));
	gquiz1.insert(pair <int, int> (5, 10));
	
	// printing multimap gquiz1 again
	
	cout << "\nThe multimap gquiz1 after adding extra elements is : \n";
	cout << "\tKEY\tELEMENT\n";
	for (itr = gquiz1.begin(); itr != gquiz1.end(); ++itr) {
		cout << '\t' << itr->first
			<< '\t' << itr->second << '\n';
	}
	cout << endl;



	// assigning the elements from gquiz1 to gquiz2
	multimap <int, int> gquiz2(gquiz1.begin(), gquiz1.end());

	// print all elements of the multimap gquiz2
	cout << "\nThe multimap gquiz2 after assign from gquiz1 is : \n";
	cout << "\tKEY\tELEMENT\n";
	for (itr = gquiz2.begin(); itr != gquiz2.end(); ++itr) {
		cout << '\t' << itr->first
			<< '\t' << itr->second << '\n';
	}
	cout << endl;

	// remove all elements up to
	// element with value 30 in gquiz2
	cout << "\ngquiz2 after removal of elements less than key=3 : \n";
	cout << "\tKEY\tELEMENT\n";
	gquiz2.erase(gquiz2.begin(), gquiz2.find(3));
	for (itr = gquiz2.begin(); itr != gquiz2.end(); ++itr) {
		cout << '\t' << itr->first
			<< '\t' << itr->second << '\n';
	}

	// remove all elements with key = 4
	int num;
	num = gquiz2.erase(4);
	cout << "\ngquiz2.erase(4) : ";
	cout << num << " removed \n" ;
	cout << "\tKEY\tELEMENT\n";
	for (itr = gquiz2.begin(); itr != gquiz2.end(); ++itr) {
		cout << '\t' << itr->first
			<< '\t' << itr->second << '\n';
	}

	cout << endl;

	//lower bound and upper bound for multimap gquiz1 key = 5
	cout << "gquiz1.lower_bound(5) : " << "\tKEY = ";
	cout << gquiz1.lower_bound(5)->first << '\t';
	cout << "\tELEMENT = " << gquiz1.lower_bound(5)->second << endl;
	cout << "gquiz1.upper_bound(5) : " << "\tKEY = ";
	cout << gquiz1.upper_bound(5)->first << '\t';
	cout << "\tELEMENT = " << gquiz1.upper_bound(5)->second << endl;

	return 0;
}
```


**Output**

```
The multimap gquiz1 is : 
    KEY    ELEMENT
    1    40
    2    30
    3    60
    6    50
    6    10


The multimap gquiz1 after adding extra elements is : 
    KEY    ELEMENT
    1    40
    2    30
    3    60
    4    50
    5    10
    6    50
    6    10


The multimap gquiz2 after assign from gquiz1 is : 
    KEY    ELEMENT
    1    40
    2    30
    3    60
    4    50
    5    10
    6    50
    6    10


gquiz2 after removal of elements less than key=3 : 
    KEY    ELEMENT
    3    60
    4    50
    5    10
    6    50
    6    10

gquiz2.erase(4) : 1 removed 
    KEY    ELEMENT
    3    60
    5    10
    6    50
    6    10

gquiz1.lower_bound(5) :     KEY = 5        ELEMENT = 10
gquiz1.upper_bound(5) :     KEY = 6        ELEMENT = 50
```

**List of Functions of Multimap:** 

- `multimap::operator=` – It is used to assign new contents to the container by replacing the existing contents.
- `multimap::crbegin()` and `multimap::crend()` – **crbegin()** returns a constant reverse iterator referring to the last element in the multimap container. **crend()** returns a constant reverse iterator pointing to the theoretical element before the first element in the multimap.
- `multimap::emplace_hint()` – Inserts the key and its element in the multimap container with a given hint.
- `multimap::clear()` – Removes all the elements from the multimap.
- `multimap::empty()` – Returns whether the multimap is empty.
- `multimap::maxsize()` – Returns the maximum number of elements a multimap container can hold.
- `multimap::value_comp()` – Returns the object that determines how the elements in the multimap are ordered (‘<‘ by default)
- `multimap::rend` – Returns a reverse iterator pointing to the theoretical element preceding to the first element of the multimap container.
- `multimap::cbegin()` and `multimap::cend()` – **cbegin()** returns a constant iterator referring to the first element in the multimap container. **cend()** returns a constant iterator pointing to the theoretical element that follows last element in the multimap.
- `multimap::swap()` – Swap the contents of one multimap with another multimap of same type and size.
- `multimap::rbegin` – Returns an iterator pointing to the last element of the container.
- `multimap::size()` – Returns the number of elements in the multimap container.
- `multimap::emplace()` – Inserts the key and its element in the multimap container.
- `multimap::begin()` and `multimap::end()` – **begin()** returns an iterator referring to the first element in the multimap container. **end()** returns an iterator to the theoretical element that follows last element in the multimap.
- `multimap::upper_bound()` – Returns an iterator to the first element that is equivalent to multimapped value with key value ‘g’ or definitely will go after the element with key value ‘g’ in the multimap.
- `multimap::count()` – Returns the number of matches to element with key value ‘g’ in the multimap.
- `multimap::erase()` – Removes the key value from the multimap.
- `multimap::find()` – Returns an iterator to the element with key value ‘g’ in the multimap if found, else returns the iterator to end.
- `multimap::equal_range()` – Returns an iterator of pairs. The pair refers to the bounds of a range that includes all the elements in the container which have a key equivalent to k.
- `multimap::insert()` – Used to insert elements in the multimap container.
- `multimap::lower_bound()` – Returns an iterator to the first element that is equivalent to multimapped value with key value ‘g’ or definitely will not go before the element with key value ‘g’ in the multimap.
- `multimap::key_comp()` – Returns the object that determines how the elements in the multimap are ordered (‘<‘ by default).

### 3.4 Unordered Associative Containers

#### 3.4.1 Unordered Sets in C++ Standard Template Library(Introduced in C++11)

An **unordered_set** is implemented using a hash table where keys are hashed into indices of a hash table so that the insertion is always randomized. All operations on the **unordered_set** takes constant time **O(1)** on an average which can go up to linear time **O(n)** in worst case which depends on the internally used hash function, but practically they perform very well and generally provide a constant time lookup operation. 
The **unordered_set** can contain key of any type – predefined or user-defined data structure but when we define key of type user define the type, we need to specify our comparison function according to which keys will be compared. 

##### Sets vs Unordered Sets

`Set` is an ordered sequence of unique keys whereas unordered_set is a set in which key can be stored in any order, so unordered. Set is implemented as a balanced tree structure that is why it is possible to maintain order between the elements (by specific tree traversal). The time complexity of set operations is O(log n) while for unordered_set, it is O(1). 

##### Methods on Unordered Sets:

For unordered_set many functions are defined among which most users are the size and empty for capacity, find for searching a key, insert and erase for modification. 
The Unordered_set allows only unique keys, for duplicate keys unordered_multiset should be used. 
Example of declaration, find, insert and iteration in unordered_set is given below :

```c++
// C++ program to demonstrate various function of unordered_set
#include <bits/stdc++.h>
using namespace std;

int main() {
	// declaring set for storing string data-type
	unordered_set <string> stringSet ;

	// inserting various string, same string will be stored once in set

	stringSet.insert("code") ;
	stringSet.insert("in") ;
	stringSet.insert("c++") ;
	stringSet.insert("is") ;
	stringSet.insert("fast") ;

	string key = "slow" ;

	// find returns end iterator if key is not found,
	// else it returns iterator to that key

	if (stringSet.find(key) == stringSet.end())
		cout << key << " not found" << endl << endl ;
	else
		cout << "Found " << key << endl << endl ;

	key = "c++";
	if (stringSet.find(key) == stringSet.end())
		cout << key << " not found\n" ;
	else
		cout << "Found " << key << endl ;

	// now iterating over whole set and printing its content
	cout << "\nAll elements : ";
	unordered_set<string> :: iterator itr;
	for (itr = stringSet.begin(); itr != stringSet.end(); itr++)
		cout << (*itr) << endl;
}
```

**Output:** 

```
slow not found

Found c++

All elements : 
is
fast
c++
in
code
```

`find`, `insert` and `erase` take constant amount of time on average. The `find()` function returns an iterator to `end()` if key is not there in set, otherwise an iterator to the key position is returned. The iterator works as a pointer to the key values so that we can get the key by dereferencing them using * operator. 
**A practical problem based on unordered_set** – Given an array(list) of integers, find all the duplicates among them. 

```
Input  : arr[] = {1, 5, 2, 1, 4, 3, 1, 7, 2, 8, 9, 5}
Output : Duplicate item are : 5 2 1 
```

Below is C++ solution using unordered_set. 

```c++
// C++ program to find duplicate from an array using
// unordered_set
#include <bits/stdc++.h>
using namespace std;

// Print duplicates in arr[0..n-1] using unordered_set
void printDuplicates(int arr[], int n) {
	// declaring unordered sets for checking and storing
	// duplicates
	unordered_set<int> intSet;
	unordered_set<int> duplicate;

	// looping through array elements
	for (int i = 0; i < n; i++) {
		// if element is not there then insert that
		if (intSet.find(arr[i]) == intSet.end())
			intSet.insert(arr[i]);

		// if element is already there then insert into
		// duplicate set
		else
			duplicate.insert(arr[i]);
	}

	// printing the result
	cout << "Duplicate item are : ";
	unordered_set<int> :: iterator itr;

	// iterator itr loops from begin() till end()
	for (itr = duplicate.begin(); itr != duplicate.end(); itr++)
		cout << *itr << " ";
}

// Driver code
int main() {
	int arr[] = {1, 5, 2, 1, 4, 3, 1, 7, 2, 8, 9, 5};
	int n = sizeof(arr) / sizeof(int);

	printDuplicates(arr, n);
	return 0;
}
```

**Output :** 

```
Duplicate item are : 5 1 2 
```

**Methods of unordered_set:** 

- `insert()` – Insert a new {element} in the unordered_set container.
- `begin()` – Return an iterator pointing to the first element in the unordered_set container.
- `end()` – Returns an iterator pointing to the past-the-end-element.
- `count()` – Count occurrences of a particular element in an unordered_set container.
- `find()` – Search for an element in the container.
- `clear()` – Removes all of the elements from an unordered_set and empties it.
- `cbegin()` – Return a const_iterator pointing to the first element in the unordered_set container.
- `cend()` – Return a const_iterator pointing to past-the-end element in the unordered_set container or in one of it’s bucket.
- `bucket_size()` – Returns the total number of elements present in a specific bucket in an unordered_set container.
- `erase()` – Remove either a single element of a range of elements ranging from start(inclusive) to end(exclusive).
- `size()` – Return the number of elements in the unordered_set container.
- `swap()` – Exchange values of two unordered_set containers.
- `emplace()` – Insert an element in an unordered_set container.
- `max_size()` – Returns maximum number of elements that an unordered_set container can hold.
- `empty()` – Check if an unordered_set container is empty or not.
- `equal_range` – Returns range that includes all elements equal to given value.
- `operator=` – Copies (or moves) an unordered_set to another unordered_set and unordered_set::operator= is the corresponding operator function.
- `hash_function()` – This hash function is a unary function which takes asingle argument only and returns a unique value of type size_t based on it.
- `reserve()` – Used to request capacity change of unordered_set.
- `bucket()` – Returns the bucket number of a specific element.
- `bucket_count()` – Returns the total number of buckets present in an unordered_set container.
- `load_factor()` – Returns the current load factor in the unordered_set container.
- `rehash()` – Set the number of buckets in the container of unordered_set to given size or more.
- `max_load_factor()` – Returns(Or sets) the current maximum load factor of the unordered set container.
- `emplace_hint()` – Inserts a new element in the unordered_set only if the value to be inserted is unique, with a given hint.
- `== operator` – The ‘==’ is an operator in C++ STL performs equality comparison operation between two unordered sets and unordered_set::operator== is the corresponding operator function for the same.
- `key_eq()` – Returns a boolean value according to the comparison. It returns the key equivalence comparison predicate used by the unordered_set.
- `operator!=` – The != is a relational operator in C++ STL which compares the equality and inequality between unordered_set containers.
- `max_bucket_count()` – Find the maximum number of buckets that unordered_set can have.

#### 3.4.2 unordered_multiset and its uses(Introduced in C++11)

I have discussed about `unordered_set` in my previous post the problem with unordered_set is that, it is not possible to store duplicate entries in that data structure. For example if we have some value v already in unordered_set, inserting v again will have no effect.
To handle this duplication unordered_mulitset should be used, it can store duplicate elements also. Internally when an existing value is inserted, the data structure increases its count which is associated with each value. As count of each value is stored in unordered_multiset, it takes more space than unordered_set (if all values are distinct).
The internal implementation of unordered_multiset is same as that of unordered_set and also uses hash table for searching, just the count value is associated with each value in former one. Due to hashing of elements it has no particular order of storing the elements so all element can come in any order but duplicate element comes together. All operation on unordered_multiset takes constant time on average but can go upto linear in worst case.
Unordered_multiset supports many function which are demonstrated in below code :

```c++
// C++ program to demonstrate various function
// of unordered_multiset
#include <bits/stdc++.h>
using namespace std;

// making typedef for short declaration
typedef unordered_multiset<int>::iterator umit;

// Utility function to print unordered_multiset
void printUset(unordered_multiset<int> ums) {
	// begin() returns iterator to first element of set
	umit it = ums.begin();
	for (; it != ums.end(); it++)
		cout << *it << " ";
	cout << endl;
}

// Driver program to check all function
int main() {
	// empty initialization
	unordered_multiset<int> ums1;

	// Initialization by intializer list
	unordered_multiset<int> ums2 ({1, 3, 1, 7, 2, 3, 4, 1, 6});

	// Initialization by assignment
	ums1 = {2, 7, 2, 5, 0, 3, 7, 5};

	// empty() function return true if set is empty
	// otherwise false
	if (ums1.empty())
		cout << "unordered multiset 1 is empty\n";
	else
		cout << "unordered multiset 1 is not empty\n";

	// size() function returns total number of elements
	// in data structure
	cout << "The size of unordered multiset 2 is : "
		<< ums2.size() << endl;

	printUset(ums1);

	ums1.insert(7);

	printUset(ums1);

	int val = 3;

	// find function returns iterator to first position
	// of val, if exist otherwise it returns iterator
	// to end
	if (ums1.find(val) != ums1.end())
		cout << "unordered multiset 1 contains "
			<< val << endl;
	else
		cout << "unordered multiset 1 does not contains "
			<< val << endl;

	// count function returns total number of occurrence in set
	val = 5;
	int cnt = ums1.count(val);
	cout << val << " appears " << cnt
		<< " times in unordered multiset 1 \n";

	val = 9;

	// if count return >0 value then element exist otherwise not
	if (ums1.count(val))
		cout << "unordered multiset 1 contains "
			<< val << endl;
	else
		cout << "unordered multiset 1 does not contains "
			<< val << endl;

	val = 1;

	// equal_range returns a pair, where first is iterator
	// to first position of val and second it iterator to
	// last position to val
	pair<umit, umit> erange_it = ums2.equal_range(val);
	if (erange_it.first != erange_it.second)
		cout << val << " appeared atleast once in "
						"unoredered_multiset \n";


	printUset(ums2);

	// erase function deletes all instances of val
	ums2.erase(val);

	printUset(ums2);

	// clear function deletes all entries from set
	ums1.clear();
	ums2.clear();

	if (ums1.empty())
		cout << "unordered multiset 1 is empty\n";
	else
		cout << "unordered multiset 1 is not empty\n";
}
```

**Output** :

```
unordered multiset 1 is not empty
The size of unordered multiset 2 is : 9
3 0 5 5 7 7 2 2 
3 0 5 5 7 7 7 2 2 
unordered multiset 1 contains 3
5 appears 2 times in unordered multiset 1 
unordered multiset 1 does not contains 9
1 appeared atleast once in unoredered_multiset 
6 4 2 7 3 3 1 1 1 
6 4 2 7 3 3 
unordered multiset 1 is empty
```

As we can see most of the operation work similar to that of unordered_set but some things to note are: `equal_range(val)` function returns a pair of type where first iterator points to first position of val and second points to last position of val in data structure.

`erase(val)` function deletes all its instances from the data structure for example if some value v has occurred t times in unordered_multiset and when erase is called, v is deleted completely which is not a expected behavior many times.

We can delete only one copy of some value by using find function and iterator version of erase, as find function returns iterator to first position of found value we can pass this iterator to erase instead of actual value to delete only one copy, the code for doing this is shown below :

```c++
// C++ program to delete one copy from unordered set
#include <bits/stdc++.h>
using namespace std;

// making typedef for short declaration
typedef unordered_multiset<int>::iterator umit;

// Utility function to print unordered_multiset
void printUset(unordered_multiset<int> ums) {
	// begin() returns iterator to first element of
	// set
	umit it = ums.begin();
	for (; it != ums.end(); it++)
		cout << *it << " ";
	cout << endl;
}

// function to delete one copy of val from set
void erase_one_entry(unordered_multiset<int>& ums, int val) {
	// find returns iterator to first position
	umit it = ums.find(val);

	// if element is there then erasing that
	if (it != ums.end())
	ums.erase(it);
}

// Driver program to check above function
int main() {
	// initializing multiset by initializer list
	unordered_multiset<int> ums ({1, 3, 1, 7, 2, 3, 4, 1, 6});

	int val = 1;
	printUset(ums);
	erase_one_entry(ums, val);
	printUset(ums);
}
```

Output :

```
6 4 2 7 3 3 1 1 1 
6 4 2 7 3 3 1 1 
```

##### Methods of unordered_multiset:

- `insert()` – Inserts new elements in the unordered_multiset. Thus increases the container size.
- `begin()` – Returns an iterator pointing to the first element in the container or to the first element in one of its bucket.
- `end()` – Returns an iterator pointing to the position immediately after the last element in the container or to the position immediately after the last element in one of its bucket.
- `empty()` – It returns true if the unordered_multiset container is empty. Otherwise, it returns false.
- `find()` – Returns an iterator which points to the position which has the element val.
- `cbegin()` – Returns a constant iterator pointing to the first element in the container or to the first element in one of its bucket.
- `cend()` – Returns a constant iterator pointing to the position immediately after the last element in the container or to the position immediately after the last element in one of its bucket.
- `equal_range()` – Returns the range in which all the elements are equal to a given value.
- `emplace()` – Inserts a new element in the unordered_multiset container.
- `clear()` – Clears the contents of the unordered_multiset container.
- `count()` – Returns the count of elements in the unordered_multiset container which is equal to a given value.
- `size()` – The size() method of unordered_multiset is used to count the number of elements of unordered_set it is called with.
- `max_size` – The max_size() of unordered_multiset takes the maximum number of elements that the unordered_multiset container is able to hold.
- `swap()` – Swaps the contents of two unordered_multiset containers.
- `erase()` – Used to remove either a single element or, all elements with a definite value or, a range of elements ranging from start(inclusive) to end(exclusive).
- `bucket()` – Returns the bucket number in which a given element is. Bucket size varies from 0 to bucket_count-1.
- `bucket_size()` – Returns the number of elements in the bucket which has the element val.
- `reserve()` – The reverse() function of unordered_multiset sets the number of buckets in the container (bucket_count) to the most appropriate to contain at least n elements.
- `max_bucket_count()` – Returns the maximum number of buckets that the unordered multiset container can have.
- `load_factor()` – Returns the current load factor in the unordered_multiset container.
- `max_load_factor()` – Returns the maximum load factor of the unordered_multiset container.
- `bucket_count()` – Returns the total number of buckets in the unordered_multiset container.
- `hash_function()` – This hash function is a unary function which takes a single argument only and returns a unique value of type size_t based on it.
- `rehash()` – Sets the number of buckets in the container to N or more.
- `key_eq()` – Returns a boolean value according to the comparison.
- `emplace_hint()` – Inserts a new element in the unordered_multiset container.
- `get_allocator` – This function gets the stored allocator object and returns the allocator object which is used to construct the container.
- `operator =` – The ‘=’ is an operator in C++ STL which copies (or moves) an unordered_multiset to another unordered_multiset and unordered_multiset::operator= is the corresponding operator function.

#### 3.4.3 unordered_map in C++ STL (Introduced in C++11)

`unordered_map` is an associated container that stores elements formed by combination of key value and a mapped value. The key value is used to uniquely identify the element and mapped value is the content associated with the key. Both key and value can be of any type predefined or user-defined. 
Internally unordered_map is implemented using `Hash Table`, the key provided to map are hashed into indices of hash table that is why performance of data structure depends on hash function a lot but on an average the cost of **search, insert and delete** from hash table is O(1). 

```c++
// C++ program to demonstrate functionality of unordered_map
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
	// Declaring umap to be of <string, int> type
	// key will be of string type and mapped value will
	// be of int type
	unordered_map<string, int> umap;

	// inserting values by using [] operator
	umap["GeeksforGeeks"] = 10;
	umap["Practice"] = 20;
	umap["Contribute"] = 30;

	// Traversing an unordered map
	for (auto x : umap)
	cout << x.first << " " << x.second << endl;

}
```

**Output:**

```
Contribute 30
GeeksforGeeks 10
Practice 20
```

**unordered_map vs unordered_set:** 
In unordered_set, we have only key, no value, these are mainly used to see presence/absence in a set. For example, consider the problem of counting frequencies of individual words. We can’t use unordered_set (or set) as we can’t store counts. 
**unordered_map vs map :** 
map (like `set`) is an ordered sequence of unique keys whereas in unordered_map key can be stored in any order, so unordered. 
Map is implemented as balanced tree structure that is why it is possible to maintain an order between the elements (by specific tree traversal). Time complexity of map operations is O(Log n) while for unordered_map, it is O(1) on average. 

##### Methods on unordered_map

A lot of function are available which work on unordered_map. most useful of them are – operator =, operator [], empty and size for capacity, begin and end for iterator, find and count for lookup, insert and erase for modification. 
The C++11 library also provides function to see internally used bucket count, bucket size and also used hash function and various hash policies but they are less useful in real application. 
We can iterate over all elements of unordered_map using Iterator. Initialization, indexing and iteration is shown in below sample code :

```c++
// C++ program to demonstrate functionality of unordered_map
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
	// Declaring umap to be of <string, double> type
	// key will be of string type and mapped value will
	// be of double type
	unordered_map<string, double> umap;

	// inserting values by using [] operator
	umap["PI"] = 3.14;
	umap["root2"] = 1.414;
	umap["root3"] = 1.732;
	umap["log10"] = 2.302;
	umap["loge"] = 1.0;

	// inserting value by insert function
	umap.insert(make_pair("e", 2.718));

	string key = "PI";

	// If key not found in map iterator to end is returned
	if (umap.find(key) == umap.end())
		cout << key << " not found\n\n";

	// If key found then iterator to that key is returned
	else
		cout << "Found " << key << "\n\n";

	key = "lambda";
	if (umap.find(key) == umap.end())
		cout << key << " not found\n";
	else
		cout << "Found " << key << endl;

	// iterating over all value of umap
	unordered_map<string, double>:: iterator itr;
	cout << "\nAll Elements : \n";
	for (itr = umap.begin(); itr != umap.end(); itr++) {
		// itr works as a pointer to pair<string, double>
		// type itr->first stores the key part and
		// itr->second stroes the value part
		cout << itr->first << " " << itr->second << endl;
	}
}
```

**Output:**

```
Found PI

lambda not found

All Elements : 
loge  1
e  2.718
log10  2.302
root3  1.732
PI  3.14
root2  1.414
```

**A practical problem based on unordered_map** – given a string of words, find frequencies of individual words.

```
Input :  str = "geeks for geeks geeks quiz practice qa for";
Output : Frequencies of individual words are
   (practice, 1)
   (for, 2)
   (qa, 1)
   (quiz, 1)
   (geeks, 3)
```

Below is a C++ solution using unordered_map. 

```c++
// C++ program to find freq of every word using
// unordered_map
#include <bits/stdc++.h>
using namespace std;

// Prints frequencies of individual words in str
void printFrequencies(const string &str) {
	// declaring map of <string, int> type, each word
	// is mapped to its frequency
	unordered_map<string, int> wordFreq;

	// breaking input into word using string stream
	stringstream ss(str); // Used for breaking words
	string word; // To store individual words
	while (ss >> word)
		wordFreq[word]++;

	// now iterating over word, freq pair and printing
	// them in <, > format
	unordered_map<string, int>:: iterator p;
	for (p = wordFreq.begin(); p != wordFreq.end(); p++)
		cout << "(" << p->first << ", " << p->second << ")\n";
}

// Driver code
int main() {
	string str = "geeks for geeks geeks quiz "
				"practice qa for";
	printFrequencies(str);
	return 0;
}
```

**Output:**

```
(qa, 1)
(quiz, 1)
(practice, 1)
(geeks, 3)
(for, 2)
```

#####  Methods of unordered_map :

- `at()`: This function in C++ unordered_map returns the reference to the value with the element as key k.
- `begin()`: Returns an iterator pointing to the first element in the container in the unordered_map container
- `end()`: Returns an iterator pointing to the position past the last element in the container in the unordered_map container
- `bucket()`: Returns the bucket number where the element with the key k is located in the map.
- `bucket_count`: bucket_count is used to count the total no. of buckets in the unordered_map. No parameter is required to pass into this function.
- `bucket_size`: Returns number of elements in each bucket of the unordered_map.
- `count()`: Count the number of elements present in an unordered_map with a given key.
- `equal_range`: Return the bounds of a range that includes all the elements in the container with a key that compares equal to k.

#### 3.4.4 unordered_multimap and its application(Introduced in C++11)

##### Allows Duplicates: 

I have discussed unordered_map in my previous post, but there is a limitation, we can not store duplicates in unordered_map, that is if we have a key-value pair already in our unordered_multimap and another pair is inserted, then both will be there whereas in case of unordered_map the previous value corresponding to the key is updated by the new value that is only would be there. Even can exist in unordered_multimap twice.

##### Internal Representation:*

The internal implementation of unordered_multimap is the same as that of unordered_map but for duplicate keys, another count value is maintained with each key-value pair. As pairs are stored in the hash table, there is no particular order among them but pair with same keys come together in data structure whereas pair with same values are not guaranteed to come together. 

##### Time Complexity:

All operation on unordered_multimap takes a constant amount of time on an average but time can go to linear in the worst case depending on internally used hash function but in long run unordered_multimap outperforms multimap (tree-based multimap). 

##### Functions: 

unordered_multimap supports many functions which are demonstrated in the below code : 

```c++
// C++ program to demonstrate various function of
// unordered_multimap
#include <bits/stdc++.h>
using namespace std;

// making typedef for short declaration
typedef unordered_multimap<string, int>::iterator umit;

// Utility function to print unordered_multimap
void printUmm(unordered_multimap<string, int> umm) {
	// begin() returns iterator to first element of map
	umit it = umm.begin();

	for (; it != umm.end(); it++)
		cout << "<" << it->first << ", " << it->second
			<< "> ";

	cout << endl;
}

// Driver code
int main() {
	// empty initialization
	unordered_multimap<string, int> umm1;

	// Initialization bu intializer list
	unordered_multimap<string, int> umm2(
		{ { "apple", 1 },
		{ "ball", 2 },
		{ "apple", 10 },
		{ "cat", 7 },
		{ "dog", 9 },
		{ "cat", 6 },
		{ "apple", 1 } });

	// Initialization by assignment operation
	umm1 = umm2;
	printUmm(umm1);

	// empty returns true, if container is empty else it
	// returns false
	if (umm2.empty())
		cout << "unordered multimap 2 is empty\n";
	else
		cout << "unordered multimap 2 is not empty\n";

	// size returns total number of elements in container
	cout << "Size of unordered multimap 1 is "
		<< umm1.size() << endl;

	string key = "apple";

	// find and return any pair, associated with key
	umit it = umm1.find(key);
	if (it != umm1.end()) {
		cout << "\nkey " << key << " is there in unordered "
			<< " multimap 1\n";
		cout << "\none of the value associated with " << key
			<< " is " << it->second << endl;
	}
	else
		cout << "\nkey " << key
			<< " is not there in unordered"
			<< " multimap 1\n";

	// count returns count of total number of pair
	// associated with key
	int cnt = umm1.count(key);
	cout << "\ntotal values associated with " << key
		<< " are " << cnt << "\n\n";

	printUmm(umm2);

	// one insertion by makeing pair explicitly
	umm2.insert(make_pair("dog", 11));

	// insertion by initializer list
	umm2.insert({ { "alpha", 12 }, { "beta", 33 } });
	cout << "\nAfter insertion of <apple, 12> and <beta, "
			"33>\n";
	printUmm(umm2);

	// erase deletes all pairs corresponding to key
	umm2.erase("apple");
	cout << "\nAfter deletion of apple\n";
	printUmm(umm2);

	// clear deletes all pairs from container
	umm1.clear();
	umm2.clear();

	if (umm2.empty())
		cout << "\nunordered multimap 2 is empty\n";
	else
		cout << "\nunordered multimap 2 is not empty\n";
}
```

**Output**

```
<dog, 9>  <cat, 6>  <cat, 7>  <ball, 2>  <apple, 1>  <apple, 10>  <apple, 1>  
unordered multimap 2 is not empty
Size of unordered multimap 1 is 7

key apple is there in unordered  multimap 1

one of the value associated with apple is 1

total values associated with apple are 3

<dog, 9>  <cat, 6>  <cat, 7>  <ball, 2>  <apple, 1>  <apple, 10>  <apple, 1>  

After insertion of <apple, 12> and <beta, 33>
<alpha, 12>  <dog, 11>  <dog, 9>  <beta, 33>  <cat, 6>  <cat, 7>  <ball, 2>  <apple, 1>  <apple, 10>  <apple, 1>  

After deletion of apple
<alpha, 12>  <dog, 11>  <dog, 9>  <beta, 33>  <cat, 6>  <cat, 7>  <ball, 2>  

unordered multimap 2 is empty
```

As we can see in above code most of the operation work similar to unordered_map but some things to note are : 
We can use the initializer list for initializing and inserting many pairs at once. 
There is no [] operator for unordered_multimap because values corresponding to a key are not unique, there can be many values associated with a single key so [] operator can not be applied to them. 
Erase function deletes all instances of values associated with the supplied key. 
Find function returns an iterator to any instance of key-value pair among all pairs associated with that key. 

##### How to access/delete if a specific value for a key?

If we want to check whether a specific is there or not, we need to loop over all pairs of key-value corresponding to k, in a similar way we can erase one copy of a specific from the data structure. There is no specified order in which all values of a key are stored. 

```c++
// C++ program to implement find and erase for specific
// key-value pair for unordered_multimap
#include <bits/stdc++.h>
using namespace std;

// making typedef for short declaration
typedef unordered_multimap<string, int>::iterator umit;

// function to check whether p is there in map or not
bool find_kv(unordered_multimap<string, int>& umm, pair<string, int> p){
	// equal_range returns pair of iterator of first and
	// last position associated with key
	pair<umit, umit> it = umm.equal_range(p.first);
	umit it1 = it.first;

	pair<string, int> tmp;

	// looping over all values associated with key
	while (it1 != it.second) {
		tmp = *it1;
		if (tmp == p)
			return true;
		it1++;
	}
	return false;
}

// function to delete one copy of pair p from map umm
void erase_kv(unordered_multimap<string, int>& umm, pair<string, int> p) {
	// equal_range returns pair of iterator of first and
	// last position associated with key
	pair<umit, umit> it = umm.equal_range(p.first);
	umit it1 = it.first;
	pair<string, int> tmp;

	// looping over all values associated with key
	while (it1 != it.second) {
		tmp = *it1;
		if (tmp == p) {
			// iterator version of erase : deletes pair
			// at that position only
			umm.erase(it1);
			break;
		}
		it1++;
	}
}

// Utility function to print unordered_multimap
void printUmm(unordered_multimap<string, int> umm) {
	// begin() returns iterator to first element of map
	umit it = umm.begin();
	for (; it != umm.end(); it++)
		cout << "<" << it->first << ", " << it->second
			<< "> ";
	cout << endl;
}

// Driver code
int main() {
	// initializing multimap by initializer list
	unordered_multimap<string, int> umm({ { "apple", 1 },
										{ "ball", 2 },
										{ "apple", 10 },
										{ "cat", 7 },
										{ "dog", 9 },
										{ "cat", 6 },
										{ "apple", 1 } });

	cout << "Initial content\n";
	printUmm(umm);
	pair<string, int> kv = make_pair("apple", 1);

	// inserting one more <apple, 1> pair
	umm.insert({ "apple", 1 });
	cout << "\nAfter insertion of one more <apple, 1>\n";
	printUmm(umm);

	if (find_kv(umm, kv))
		erase_kv(umm, kv);
	else
		cout << "key-value pair is not there in unordered "
				"multimap\n";

	cout << "\nAfter deletion one occurrence of <apple, "
			"1>\n";
	printUmm(umm);
}
```

**Output**

```
Initial content
<dog, 9> <cat, 6> <cat, 7> <ball, 2> <apple, 1> <apple, 10> <apple, 1> 

After insertion of one more <apple, 1>
<dog, 9> <cat, 6> <cat, 7> <ball, 2> <apple, 1> <apple, 1> <apple, 10> <apple, 1> 

After deletion one occurrence of <apple, 1>
<dog, 9> <cat, 6> <cat, 7> <ball, 2> <apple, 1> <apple, 10> <apple, 1> 
```

##### Methods of unordered_multimap: 

- `begin()`– Returns an iterator pointing to the first element in the container or to the first element in one of its bucket.
- `end()` – Returns an iterator pointing to the position after the last element in the container or to the position after the last element in one of its bucket.
- `count()` – Returns the number of elements in the container whose key is equal to the key passed in the parameter.
- `cbegin()` – Returns a constant iterator pointing to the first element in the container or to the first element in one of its bucket.
- `cend()` – Returns a constant iterator pointing to the position after the last element in the container or to the position after the last element in one of its bucket.
- `clear()` – Clears the contents of the unordered_multimap container.
- `size()` – Returns the size of the unordered_multimap. It denotes the number of elements in that container.
- `swap()` – Swaps the contents of two unordered_multimap containers. The sizes can differ of both the containers.
- `find()` – Returns an iterator which points to one of the elements which have the key k.
- `bucket_size()` – Returns the number of elements in the bucket n.
- `empty()` – It returns true if the unordered_multimap container is empty. Otherwise, it returns false.
- `equal_range()` – Returns the range in which all the element’s key is equal to a key.
- `operator=` – Copy/Assign/Move elements from different container.
- `max_size()` – Returns the maximum number of elements that the unordered_multimap container can hold.
- `load_factor()` – Returns the current load factor in the unordered_multimap container.
- `key_eq()` – Returns a boolean value according to the comparison.
- `emplace()` – Inserts a new {key, element} in the unordered_multimap container.
- `emplace_hint()` – Inserts a new {key:element} in the unordered_multimap container.
- `bucket_count()` – Returns the total number of buckets in the unordered_multimap container.
- `bucket()` – Returns the bucket number in which a given key is.
- `max_load_factor()` – Returns the maximum load factor of the unordered_multimap container.
- `rehash()` – Sets the number of buckets in the container to N or more.
- `reserve()` – Sets the number of buckets in the container (bucket_count) to the most appropriate number so that it contains at least n elements.
- `hash_function()` – This hash function is a unary function that takes a single argument only and returns a unique value of type size_t based on it.
- `max_bucket_count()` – Returns the maximum number of buckets that the unordered multimap container can have.



## 4 Functions

> The STL includes classes that overload the function call operator. Instances of such classes are called function objects or functors. Functors allow the working of the associated function to be customized with the help of parameters to be passed.

### 4.1 Functors in C++

Please note that the title is **Functors** (Not Functions)!!

Consider a function that takes only one argument. However, while calling this function we have a lot more information that we would like to pass to this function, but we cannot as it accepts only one parameter. What can be done?

One obvious answer might be global variables. However, good coding practices do not advocate the use of global variables and say they must be used only when there is no other alternative.

**Functors** are objects that can be treated as though they are a function or function pointer. Functors are most commonly used along with STLs in a scenario like following:

Below program uses `transform()` in STL to add 1 to all elements of arr[].

```c++
// A C++ program uses transform() in STL to add
// 1 to all elements of arr[]
#include <bits/stdc++.h>
using namespace std;

int increment(int x) { return (x+1); }

int main() {
	int arr[] = {1, 2, 3, 4, 5};
	int n = sizeof(arr)/sizeof(arr[0]);

	// Apply increment to all elements of
	// arr[] and store the modified elements
	// back in arr[]
	transform(arr, arr+n, arr, increment);

	for (int i=0; i<n; i++)
		cout << arr[i] <<" ";

	return 0;
}
```

**Output**:

```
2 3 4 5 6
```

This code snippet adds only one value to the contents of the arr[]. Now suppose, that we want to add 5 to contents of arr[].

See what’s happening? As transform requires a unary function(a function taking only one argument) for an array, we cannot pass a number to increment(). And this would, in effect, make us write several different functions to add each number. What a mess. This is where functors come into use.

A functor (or function object) is a C++ class that acts like a function. Functors are called using the same old function call syntax. To create a functor, we create a object that overloads the *operator()*.

```
The line,
MyFunctor(10);

Is same as
MyFunctor.operator()(10);
```

Let’s delve deeper and understand how this can actually be used in conjunction with STLs.

```c++
// C++ program to demonstrate working of functors.
#include <bits/stdc++.h>
using namespace std;

// A Functor
class increment
{
private:
	int num;
public:
	increment(int n) : num(n) { }

	// This operator overloading enables calling
	// operator function () on objects of increment
	int operator () (int arr_num) const {
		return num + arr_num;
	}
};

// Driver code
int main() {
	int arr[] = {1, 2, 3, 4, 5};
	int n = sizeof(arr)/sizeof(arr[0]);
	int to_add = 5;

	transform(arr, arr+n, arr, increment(to_add));

	for (int i=0; i<n; i++)
		cout << arr[i] << " ";
}
```

**Output**:

```
6 7 8 9 10
```

Thus, here, Increment is a functor, a c++ class that acts as a function.

```
The line,
transform(arr, arr+n, arr, increment(to_add));

is the same as writing below two lines,
// Creating object of increment
increment obj(to_add); 

// Calling () on object
transform(arr, arr+n, arr, obj); 
```

Thus, an object *a* is created that overloads the *operator()*. Hence, functors can be used effectively in conjunction with C++ STLs.

## 5 Iterators

> As the name suggests, iterators are used for working upon a sequence of values. They are the major feature that allow generality in STL.

### 5.1 Introduction to Iterators in C++

An **iterator** is an object (like a pointer) that points to an element inside the container. We can use iterators to move through the contents of the container. They can be visualized as something similar to a pointer pointing to some location and we can access the content at that particular location using them.

Iterators play a critical role in connecting algorithm with containers along with the manipulation of data stored inside the containers. The most obvious form of an iterator is a pointer. A pointer can point to elements in an array and can iterate through them using the increment operator (++). But, all iterators do not have similar functionality as that of pointers.

Depending upon the functionality of iterators they can be classified into five categories, as shown in the diagram below with the outer one being the most powerful one and consequently the inner one is the least powerful in terms of functionality.

![img](https://media.geeksforgeeks.org/wp-content/uploads/C_Iterator.jpg)

Now each one of these iterators are not supported by all the containers in STL, different containers support different iterators, like vectors support **Random-access iterators**, while lists support **bidirectional iterators**. The whole list is as given below:

![Lightbox](https://media.geeksforgeeks.org/wp-content/uploads/C_Iterator_Support.jpg)

#### Types of iterators

Based upon the functionality of the iterators, they can be classified into five major categories:

1. **Input Iterators**: They are the weakest of all the iterators and have very limited functionality. They can only be used in a single-pass algorithms, i.e., those algorithms which process the container sequentially, such that no element is accessed more than once.
2. **Output Iterators**: Just like input iterators, they are also very limited in their functionality and can only be used in single-pass algorithm, but not for accessing elements, but for being assigned elements.
3. **Forward Iterator**: They are higher in the hierarachy than input and output iterators, and contain all the features present in these two iterators. But, as the name suggests, they also can only move in a forward direction and that too one step at a time.
4. **Bidirectional Iterators**: They have all the features of forward iterators along with the fact that they overcome the drawback of forward iterators, as they can move in both the directions, that is why their name is bidirectional.
5. **Random-Access Iterators**: They are the most powerful iterators. They are not limited to moving sequentially, as their name suggests, they can randomly access any element inside the container. They are the ones whose functionality are same as pointers.

The following diagram shows the difference in their functionality with respect to various operations that they can perform.

![Lightbox](https://media.geeksforgeeks.org/wp-content/uploads/iteratorOperation.jpg)

#### Benefits of Iterators

There are certainly quite a few ways which show that iterators are extremely useful to us and encourage us to use it profoundly. Some of the benefits of using iterators are as listed below:

1. **Convenience in programming:** It is better to use iterators to iterate through the contents of containers as if we will not use an iterator and access elements using [ ] operator, then we need to be always worried about the size of the container, whereas with iterators we can simply use member function end() and iterate through the contents without having to keep anything in mind.

```c++
// C++ program to demonstrate iterators

#include <iostream>
#include <vector>
using namespace std;
int main() {
	// Declaring a vector
	vector<int> v = { 1, 2, 3 };

	// Declaring an iterator
	vector<int>::iterator i;

	int j;

	cout << "Without iterators = ";
	
	// Accessing the elements without using iterators
	for (j = 0; j < 3; ++j) {
		cout << v[j] << " ";
	}

	cout << "\nWith iterators = ";
	
	// Accessing the elements using iterators
	for (i = v.begin(); i != v.end(); ++i) {
		cout << *i << " ";
	}

	// Adding one more element to vector
	v.push_back(4);

	cout << "\nWithout iterators = ";
	
	// Accessing the elements without using iterators
	for (j = 0; j < 4; ++j) {
		cout << v[j] << " ";
	}

	cout << "\nWith iterators = ";
	
	// Accessing the elements using iterators
	for (i = v.begin(); i != v.end(); ++i) {
		cout << *i << " ";
	}

	return 0;
}
```

Output:

```
Without iterators = 1 2 3
With iterators = 1 2 3
Without iterators = 1 2 3 4
With iterators = 1 2 3 4
```

**Explanation:** As can be seen in the above code that without using iterators we need to keep track of the total elements in the container. In the beginning there were only three elements, but after one more element was inserted into it, accordingly the for loop also had to be amended, but using iterators, both the time the for loop remained the same. So, iterator eased our task.

2. Code reusability: Now consider if we make a list in place of vector in the above program and if we were not using iterators to access the elements and only using [ ] operator, then in that case this way of accessing was of no use for list (as they don’t support random-access iterators).

However, if we were using iterators for vectors to access the elements, then just changing the vector to list in the declaration of the iterator would have served the purpose, without doing anything else.

So, iterators support reusability of code, as they can be used to access elements of any container.

3. **Dynamic processing of the container:** Iterators provide us the ability to dynamically add or remove elements from the container as and when we want with ease.

```c++
// C++ program to demonstrate iterators

#include <iostream>
#include <vector>
using namespace std;
int main() {
	// Declaring a vector
	vector<int> v = { 1, 2, 3 };

	// Declaring an iterator
	vector<int>::iterator i;

	int j;

	// Inserting element using iterators
	for (i = v.begin(); i != v.end(); ++i) {
		if (i == v.begin()) {
			i = v.insert(i, 5);
			// inserting 5 at the beginning of v
		}
	}
	
	// v contains 5 1 2 3

	// Deleting a element using iterators
	for (i = v.begin(); i != v.end(); ++i) {
		if (i == v.begin() + 1) {
			i = v.erase(i);
			// i now points to the element after the
			// deleted element
		}
	}
	
	// v contains 5 2 3

	// Accessing the elements using iterators
	for (i = v.begin(); i != v.end(); ++i) {
		cout << *i << " ";
	}

	return 0;
}
```

Output:

```
5 2 3
```

**Explanation:** As seen in the above code, we can easily and dynamically add and remove elements from the container using iterator, however, doing the same without using them would have been very tedious as it would require shifting the elements every time before insertion and after deletion.

### 5.2 Iterators in C++ STL

Iterators are used to point at the memory addresses of STL containers. They are primarily used in sequence of numbers, characters etc. They reduce the complexity and execution time of program.

#### Operations of iterators:

1. `begin()`:- This function is used to return the **beginning position** of the container.

2. `end()`:- This function is used to return the ***after* end position** of the container.

```c++
// C++ code to demonstrate the working of
// iterator, begin() and end()
#include<iostream>
#include<iterator> // for iterators
#include<vector> // for vectors
using namespace std;
int main() {
	vector<int> ar = { 1, 2, 3, 4, 5 };
	
	// Declaring iterator to a vector
	vector<int>::iterator ptr;
	
	// Displaying vector elements using begin() and end()
	cout << "The vector elements are : ";
	for (ptr = ar.begin(); ptr < ar.end(); ptr++)
		cout << *ptr << " ";
	
	return 0;	
}
```

**Output:**

```
The vector elements are : 1 2 3 4 5 
```

3. `advance()` :- This function is used to **increment the iterator position** till the specified number mentioned in its arguments.

```c++
// C++ code to demonstrate the working of
// advance()
#include<iostream>
#include<iterator> // for iterators
#include<vector> // for vectors
using namespace std;
int main() {
	vector<int> ar = { 1, 2, 3, 4, 5 };
	
	// Declaring iterator to a vector
	vector<int>::iterator ptr = ar.begin();
	
	// Using advance() to increment iterator position
	// points to 4
	advance(ptr, 3);
	
	// Displaying iterator position
	cout << "The position of iterator after advancing is : ";
	cout << *ptr << " ";
	
	return 0;
	
}
```

**Output:**

```
The position of iterator after advancing is : 4 
```

4. `next()`:- This function **returns the new iterator** that the iterator would point after **advancing the positions** mentioned in its arguments.

5. `prev()`:- This function **returns the new iterator** that the iterator would point **after decrementing the positions** mentioned in its arguments.

```c++
// C++ code to demonstrate the working of
// next() and prev()
#include<iostream>
#include<iterator> // for iterators
#include<vector> // for vectors
using namespace std;
int main() {
	vector<int> ar = { 1, 2, 3, 4, 5 };
	
	// Declaring iterators to a vector
	vector<int>::iterator ptr = ar.begin();
	vector<int>::iterator ftr = ar.end();
	
	
	// Using next() to return new iterator
	// points to 4
	auto it = next(ptr, 3);
	
	// Using prev() to return new iterator
	// points to 3
	auto it1 = prev(ftr, 3);
	
	// Displaying iterator position
	cout << "The position of new iterator using next() is : ";
	cout << *it << " ";
	cout << endl;
	
	// Displaying iterator position
	cout << "The position of new iterator using prev() is : ";
	cout << *it1 << " ";
	cout << endl;
	
	return 0;
}
```

**Output:**

```
The position of new iterator using next() is : 4 
The position of new iterator using prev()  is : 3 
```

6. `inserter()`:- This function is used to **insert the elements at any position** in the container. It accepts **2 arguments, the container and iterator to position where the elements have to be inserted**.

```c++
// C++ code to demonstrate the working of
// inserter()
#include<iostream>
#include<iterator> // for iterators
#include<vector> // for vectors
using namespace std;
int main() {
	vector<int> ar = { 1, 2, 3, 4, 5 };
	vector<int> ar1 = {10, 20, 30};
	
	// Declaring iterator to a vector
	vector<int>::iterator ptr = ar.begin();
	
	// Using advance to set position
	advance(ptr, 3);
	
	// copying 1 vector elements in other using inserter()
	// inserts ar1 after 3rd position in ar
	copy(ar1.begin(), ar1.end(), inserter(ar,ptr));
	
	// Displaying new vector elements
	cout << "The new vector after inserting elements is : ";
	for (int &x : ar)
		cout << x << " ";
	
	return 0;	
}
```

**Output:**

```
The new vector after inserting elements is : 1 2 3 10 20 30 4 5 
```

## 6 Utility Library

> Defined in header <utility>.

### 6.1 Pair in C++ Standard Template Library (STL)

The pair container is a simple container defined in `<utility>` header consisting of two data elements or objects. 

- The first element is referenced as ‘first’ and the second element as ‘second’ and the order is fixed (first, second).
- Pair is used to combine together two values which may be different in type. Pair provides a way to store two heterogeneous objects as a single unit.
- Pair can be assigned, copied and compared. The array of objects allocated in a map or hash_map are of type ‘pair’ by default in which all the ‘first’ elements are unique keys associated with their ‘second’ value objects.
- To access the elements, we use variable name followed by dot operator followed by the keyword first or second.

**Syntax :** 

```
pair (data_type1, data_type2) Pair_name
```

```c++
// CPP program to illustrate pair STL
#include <iostream>
#include <utility>
using namespace std;

int main() {
	pair<int, char> PAIR1;

	PAIR1.first = 100;
	PAIR1.second = 'G';

	cout << PAIR1.first << " ";
	cout << PAIR1.second << endl;

	return 0;
}
```

**Output**

```
100 G
```

#### lnitializing a pair

We can also initialize a pair. 

**Syntax :**

```
pair (data_type1, data_type2) Pair_name (value1, value2) ;
```

Different ways to initialize pair: 

```
pair  g1;         //default
pair  g2(1, 'a');  //initialized,  different data type
pair  g3(1, 10);   //initialized,  same data type
pair  g4(g3);    //copy of g3
```

Another way to initialize a pair is by using the make_pair() function. 

```
g2 = make_pair(1, 'a');
```

```c++
// CPP program to illustrate
// Initializing of pair STL
#include <iostream>
#include <utility>
using namespace std;

int main() {
	pair<string, double> PAIR2("GeeksForGeeks", 1.23);

	cout << PAIR2.first << " ";
	cout << PAIR2.second << endl;

	return 0;
}
```

**Output**

```
GeeksForGeeks 1.23
```

**Note:** If not initialized, the first value of the pair gets automatically initialized. 

```c++
// CPP program to illustrate
// auto-initializing of pair STL
#include <iostream>
#include <utility>

using namespace std;

int main() {
	pair<int, double> PAIR1;
	pair<string, char> PAIR2;

	// it is initialised to 0
	cout << PAIR1.first;
	
	// it is initialised to 0
	cout << PAIR1.second;

	cout << " ";

	// // it prints nothing i.e NULL
	cout << PAIR2.first;
	
	// it prints nothing i.e NULL
	cout << PAIR2.second;

	return 0;
}
```

**Output:** 

```
00
```

#### Member Functions

1. `make_pair()`: This template function allows to create a value pair without writing the types explicitly. 
   Syntax :

```
Pair_name = make_pair (value1,value2);
```

```c++
#include <iostream>
#include <utility>
using namespace std;

int main() {
	pair <int, char> PAIR1 ;
	pair <string, double> PAIR2 ("GeeksForGeeks", 1.23) ;
	pair <string, double> PAIR3 ;

	PAIR1.first = 100;
	PAIR1.second = 'G' ;

	PAIR3 = make_pair ("GeeksForGeeks is Best",4.56);

	cout << PAIR1.first << " " ;
	cout << PAIR1.second << endl ;

	cout << PAIR2.first << " " ;
	cout << PAIR2.second << endl ;

	cout << PAIR3.first << " " ;
	cout << PAIR3.second << endl ;

	return 0;
}
```

**Output:** 

```
100 G
GeeksForGeeks 1.23
GeeksForGeeks is Best 4.56
```

**operators(=, ==, !=, >=, <=) :** We can use operators with pairs as well. 

- **using equal(=) **: It assigns new object for a pair object. 

**Syntax** :

```
pair& operator= (const pair& pr);
```

- This Assigns pr as the new content for the pair object. The first value is assigned the first value of pr and the second value is assigned the second value of pr .
- **Comparison (==) operator with pair :** For given two pairs say pair1 and pair2, the comparison operator compares the first value and second value of those two pairs i.e. if pair1.first is equal to pair2.first or not AND if pair1.second is equal to pair2.second or not .
- **Not equal (!=) operator with pair :** For given two pairs say pair1 and pair2, the != operator compares the first values of those two pairs i.e. if pair1.first is equal to pair2.first or not, if they are equal then it checks the second values of both.
- **Logical( >=, <= )operators with pair :** For given two pairs say pair1 and pair2, the =, >, can be used with pairs as well. It returns 0 or 1 by only comparing the first value of the pair.
  For pairs like p1=(1,20) and p2=(1,10) 
  p2<p1 should give 0 (as it compares 1st element only & they are equal so its definitely not less), but that isn’t true. Here the pair compares the second element and if it satisfies then returns 1 
  (this is only the case when the first element gets equal while using a relational operator > or < only, otherwise these operators work as mentioned above) 

2. `swap`: This function swaps the contents of one pair object with the contents of another pair object. The pairs must be of same type. 

**Syntax** :

```
pair1.swap(pair2) ;
```

For two given pairs say pair1 and pair2 of same type, swap function will swap the pair1.first with pair2.first and pair1.second with pair2.second. 

```c++
#include <iostream>
#include<utility>

using namespace std;

int main() {
	pair<char, int>pair1 = make_pair('A', 1);
	pair<char, int>pair2 = make_pair('B', 2);

	cout << "Before swapping:\n " ;
	cout << "Contents of pair1 = "
		<< pair1.first << " " << pair1.second ;
	cout << "Contents of pair2 = "
		<< pair2.first << " " << pair2.second ;
	pair1.swap(pair2);

	cout << "\nAfter swapping:\n ";
	cout << "Contents of pair1 = "
		<< pair1.first << " " << pair1.second ;
	cout << "Contents of pair2 = "
		<< pair2.first << " " << pair2.second ;

	return 0;
}
```

**Output**: 

```
Before swapping:
Contents of pair1 = (A, 1)
Contents of pair2 = (B, 2)

After swapping:
Contents of pair1 = (B, 2)
Contents of pair2 = (A, 1)
```

3. `tie()` : This function works the same as in `tuples`. It creates a tuple of lvalue references to its arguments i.e., to unpack the tuple (or here pair) values into separate variables. Just like in tuples, here are also two variants of tie, with and without “ignore”. “ignore” keyword ignores a particular tuple element from getting unpacked. 
   However, tuples can have multiple arguments but pairs only have two arguments. So, in case of pair of pairs, unpacking needs to be explicitly handled. 

**Syntax :** 

```
tie(int &, int &) = pair1; 
```

```c++
// CPP code to illustrate tie() in pairs
#include <bits/stdc++.h>
using namespace std;

int main() {
	pair<int, int> pair1 = { 1, 2 };
	int a, b;
	tie(a, b) = pair1;
	cout << a << " " << b << "\n";

	pair<int, int> pair2 = { 3, 4 };
	tie(a, ignore) = pair2;
	
	// prints old value of b
	cout << a << " " << b << "\n";

	// Illustrating pair of pairs
	pair<int, pair<int, char> > pair3
				= { 3, { 4, 'a' } };
	int x, y;
	char z;
	
	// tie(x,y,z) = pair3; Gives compilation error
	// tie(x, tie(y,z)) = pair3; Gives compilation error
	// Each pair needs to be explicitly handled
	x = pair3.first;
	tie(y, z) = pair3.second;
	cout << x << " " << y << " " << z << "\n";	
	
}
```

**Output :** 

```
1 2
3 2
3 4 a
```

```c++
//CPP program to illustrate pair in STL
#include <iostream>
#include <utility>
#include <string>
using namespace std;

int main() {
	pair <string, int> g1;
	pair <string, int> g2("Quiz", 3);
	pair <string, int> g3(g2);
	pair <int, int> g4(5, 10);

	g1 = make_pair(string("Geeks"), 1);
	g2.first = ".com";
	g2.second = 2;

	cout << "This is pair g" << g1.second << " with "
		<< "value " << g1.first << "." << endl << endl;

	cout << "This is pair g" << g3.second
		<< " with value " << g3.first
		<< "This pair was initialized as a copy of "
		<< "pair g2" << endl << endl;

	cout << "This is pair g" << g2.second
		<< " with value " << g2.first
		<< "\nThe values of this pair were"
		<< " changed after initialization."
		<< endl << endl;

	cout << "This is pair g4 with values "
		<< g4.first << " and " << g4.second
		<< " made for showing addition. \nThe "
		<< "sum of the values in this pair is "
		<< g4.first+g4.second
		<< "." << endl << endl;

	cout << "We can concatenate the values of"
		<< " the pairs g1, g2 and g3 : "
		<< g1.first + g3.first + g2.first
		<< endl << endl;

	cout << "We can also swap pairs "
		<< "(but type of pairs should be same) : "
		<< endl;
	cout << "Before swapping, " << "g1 has "
		<< g1.first
		<< " and g2 has " << g2.first << endl;
	swap(g1, g2);
	cout << "After swapping, "
		<< "g1 has " << g1.first << " and g2 has "
		<< g2.first;

	return 0;
}
```

**Output:** 

```
This is pair g1 with value Geeks.
This is pair g3 with value Quiz
This pair was initialized as a copy of pair g2
This is pair g2 with value .com
The values of this pair were changed 
after initialization.
This is pair g4 with values 5 and 10 made 
for showing addition. 
The sum of the values in this pair is 15.
We can concatenate the values of the pairs g1, 
g2 and g3 : GeeksQuiz.com
We can also swap pairs (but type of pairs should be same) : 
Before swapping, g1 has Geeks and g2 has .com
After swapping, g1 has .com and g2 has Geeks
```



























































































