---
layout: post
maintag: C++
tags: C++ sorting algorithms
comments: true
title: Sorting in C++
date: Dec 20 2016
---

This post is about different built-in sort functions available in `<algorithm>` library in C++.

There are different functions available for sorting in C++:

1. [`std::is_sorted` / `std::is_sorted_until`](#is_sorted)
2. [`std::sort`](#sort)
3. [`std::stable_sort`](#stable_sort)
4. [`std::partial_sort`](#partial_sort)
5. [`std::nth_element`](#nth_element)

We will discuss each one of the above one-by-one :

------------------------
<p><a name="is_sorted"></a></p>
## [1.](#is_sorted) `std::is_sorted` / `std::is_sorted_until`

### A. Syntax

```cpp
template <typename ForwardIterator>
ForwardIt is_sorted_until(ForwardIterator first, ForwardIterator last);

template<typename ForwardIterator, typename Compare>
bool is_sorted(ForwardIterator first, ForwardIterator last, Compare comp);

template<typename ForwardIterator>
ForwardIterator is_sorted_until(ForwardIterator first, ForwardIterator last);

template<typename ForwardIterator, typename Compare>
ForwardIterator is_sorted_until(ForwardIterator first, ForwardIterator last, Compare comp);
```

### B. Parameters

- `first`, `last`:  The range of values to examine.
- `comp`: Functor taking two parameters and returning `true` if the first argument is less than second.

### C. Return Values

- `std::is_sorted`: Returns `true` if all elements in `[first, last)` is sorted in ascending order.
- `std::is_sorted_until`: Returns an Iterator `it`, such that the range `[first, it)` is largest sorted length from beginning.

### D. Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cassert>
int main()
{
	std::vector<int> ar = {1, 2, 3, 5, 0};
	
	// clearly, ar is not sorted
	assert(not std::is_sorted(ar.begin(), ar.end()));

	// {1, 2, 3, 5} is largest possible sorted range
	// so std::is_sorted_until will return an iterator pointing to 
	// element 0 i.e. ar.begin() + 4
	assert(std::is_sorted_until(ar.begin(), ar.end()) == ar.begin() + 4);

	// lets sort ar
	std::sort(ar.begin(), ar.end());

	// Now ar is sorted
	assert(std::is_sorted(ar.begin(), ar.end()));

	// Now ar is sorted to its end
	assert(std::is_sorted_until(ar.begin(), ar.end()) == ar.end());

	return 0;
}
```

### E. Complexity

Both `std::is_sorted` and `std::is_sorted_until` has the complexity of `O(N)` where `N = std::distance(first, last)`.

------------------------

<p><a name="sort"></a></p>
## [2.](#sort) `std::sort`

### A. Syntax

```cpp
template<typename RandomIterator>
void sort(RandomIterator first, RandomIterator last);

template<typename RandomIterator, typename Compare>
void sort(RandomIterator first, RandomIterator last, Compare comp);
```

### B. Parameters

- `first`, `last`:  The range of values to sort.
- `comp`: Functor taking two parameters and returning `true` if the first argument is less than second.

### C. Return Values

`std::sort`: Returns **nothing**. Instead, it just sorts the range `[first, last)`.

### D. Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cassert>
int main()
{	
	std::vector<int> ar = {1, 3, 4, 1, 2, 4, 5, 0};

	// ar is not sorted
	assert(not std::is_sorted(ar.begin(), ar.end()));

	// lets sort ar
	std::sort(ar.begin(), ar.end());

	// now ar is sorted
	assert(std::is_sorted(ar.begin(), ar.end()));

	return 0;
}
```
------------------------
<p><a name="stable_sort"></a></p>
## [3.](#stable_sort) `std::stable_sort`

### A. Syntax

```cpp
template<typename RandomIterator>
void stable_sort(RandomIterator first, RandomIterator last);

template<typename RandomIterator, typename Compare>
void stable_sort(RandomIterator first, RandomIterator last, Compare comp);
```

### B. Parameters

- `first`, `last`:  The range of values to sort.
- `comp`: Functor taking two parameters and returning `true` if the first argument is less than second.

### C. Return Values

`std::stable_sort`: Returns **nothing**. Instead, it just sorts the range `[first, last)`.

### D. Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cassert>
int main()
{	
	std::vector<int> ar = {1, 3, 4, 1, 2, 4, 5, 0};

	// ar is not sorted
	assert(not std::is_sorted(ar.begin(), ar.end()));

	// lets sort ar
	std::stable_sort(ar.begin(), ar.end());

	// now ar is sorted
	assert(std::is_sorted(ar.begin(), ar.end()));

	return 0;
}
```

### E. Difference between `std::sort` and `std::stable_sort`

- In `std::stable_sort` the order of equal elements are preserved while this is not the case with `std::sort`.
- Complexity of `std::sort` is `O(N·log(N))` whereas complexity of `std::stable_sort` is `O(N·log(N).log(N))`, where `N = std::distance(first, last)`

------------------------
<p><a name="partial_sort"></a></p>
## [4.](#partial_sort) `std::partial_sort`

### A. Syntax

```cpp
template<typename RandomIterator>
void partial_sort(RandomIterator first, RandomIterator middle, RandomIterator last);

template<typename RandomIterator, typename Compare>
void partial_sort(RandomIterator first, RandomIterator middle, RandomIterator last, Compare comp);
```

### B. Parameters

- `first`, `last`:  The range of values to sort.
- `comp`: Functor taking two parameters and returning `true` if the first argument is less than second.

### C. Return Values

`std::partial_sort`: Returns **nothing**. Instead, it rearranges elements such that the range `[first, middle)` contains the sorted `middle - first` smallest elements in the range `[first, last)`. The order of the remaining elements in the range `[middle, last)` is unspecified.


### D. Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
void print(std::vector<int> ar)
{
	for(auto x : ar) std::cout << x << " ";
	std::cout << std::endl;
}
int main()
{	
	std::vector<int> ar = {1, 3, 7, 1, 2, 4, 5, 0};

	// will print 1 3 7 1 2 4 5 0
	print(ar); 

	// mid = 5th element (ar.begin() + 4)
	auto mid = ar.begin() + std::distance(ar.begin(), ar.end()) / 2;
	// lets partial_sort ar to mid
	std::partial_sort(ar.begin(), mid, ar.end());

	// will print 0 1 1 2 7 4 5 3
	print(ar);

	return 0;
}
```

------------------------
<p><a name="nth_element"></a></p>
## [5.](#nth_element) `std::nth_element`

### A. Syntax

```cpp
template<typename RandomIterator>
void nth_element(RandomIterator first, RandomIterator nth, RandomIterator last);

template<typename RandomIterator, typename Compare>
void nth_element(RandomIterator first, RandomIterator nth, RandomIterator last, Compare comp);
```

### B. Parameters

- `first`, `last`:  The range of values to sort.
- `comp`: Functor taking two parameters and returning `true` if the first argument is less than second.

### C. Return Values

`std::nth_element` is a partial sorting algorithm that rearranges elements in `[first, last)` such that:

- The element pointed at by nth is changed to whatever element would occur in that position if `[first, last)` was sorted.
- All of the elements before this new nth element are less than or equal to the elements after the new nth element.

### D. Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
void print(std::vector<int> ar)
{
	for(auto x : ar) std::cout << x << " ";
	std::cout << std::endl;
}
int main()
{	
	std::vector<int> ar = {1, 3, 6, 1, 2, 4, 7, 0};

	// will print 1 3 6 1 2 4 7 0
	print(ar); 

	// mid = 5th element (ar.begin() + 4)
	auto mid = ar.begin() + std::distance(ar.begin(), ar.end()) / 2;

	// lets nth_element ar to mid
	std::nth_element(ar.begin(), mid, ar.end());

	// will print 2 0 1 1 3 4 7 6
	// mid points to element 3
	print(ar);

	return 0;
}
```

## Wrapping Up

So we learned different sorting function available in C++ standard library, which can be useful for various problems.

------------------------

My Original Post: [Sorting In C++ - Algorithms](https://www.codementor.io/mnciitbhu/sorting-in-c_plus_plus-algorithms-mxs9g8bhe)

------------------------