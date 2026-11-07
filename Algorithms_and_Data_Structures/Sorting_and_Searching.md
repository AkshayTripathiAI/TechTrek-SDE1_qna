# Sorting and Searching

## Sorting Algorithms

### 1. Bubble Sort

**Description:**
Bubble sort is a simple sorting algorithm that repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order.

**Time Complexity:**
- Best Case: O(n)
- Worst Case: O(n^2)
- Average Case: O(n^2)

**Space Complexity:** O(1)

### 2. Selection Sort

**Description:**
Selection sort is a simple sorting algorithm that repeatedly selects the minimum (or maximum) element from the unsorted portion of the array and places it at the beginning.

**Time Complexity:**
- Best Case: O(n^2)
- Worst Case: O(n^2)
- Average Case: O(n^2)

**Space Complexity:** O(1)

### 3. Merge Sort

**Description:**
Merge sort is a divide-and-conquer algorithm that divides the input array into two halves, recursively sorts them, and then merges the sorted halves.

**Time Complexity:**
- Best Case: O(n log n)
- Worst Case: O(n log n)
- Average Case: O(n log n)

**Space Complexity:** O(n)

## Searching Algorithms

### 1. Binary Search

**Description:**
Binary search is an efficient searching algorithm that works on sorted arrays. It repeatedly divides the search interval in half until the target element is found or the interval is empty.

**Time Complexity:**
- Best Case: O(1)
- Worst Case: O(log n)
- Average Case: O(log n)

**Space Complexity:** O(1)

## Common Interview Questions

### 1. Find Peak Element

**Description:**
Given an array of integers, find a peak element, which is greater than or equal to its neighbors.

**Example:**
Input: `[1, 3, 5, 4, 7, 6]`
Output: `5` or `7` (indices 2 or 4)

**Solution:**
- Binary Search Approach

### 2. Merge Sorted Arrays

**Description:**
Merge two sorted arrays into a single sorted array.

**Example:**
Input: `[1, 3, 5]` and `[2, 4, 6]`
Output: `[1, 2, 3, 4, 5, 6]`

**Solution:**
- Merge Sort Approach

## Additional Resources

- [GeeksforGeeks - Sorting Algorithms](https://www.geeksforgeeks.org/sorting-algorithms/)
- [GeeksforGeeks - Searching Algorithms](https://www.geeksforgeeks.org/searching-algorithms/)
- [LeetCode - Sorting Problems](https://leetcode.com/tag/sorting/)
- [LeetCode - Binary Search Problems](https://leetcode.com/tag/binary-search/)

## Conclusion

Sorting and searching algorithms are fundamental concepts in computer science and are frequently tested in technical interviews. Understanding different sorting and searching techniques, along with their time and space complexities, is crucial for SDE1 interview preparation.

---

Feel free to expand upon each section with more detailed explanations, additional examples, and alternative solutions. Customizing the content according to your preferences and the needs of your audience will make the `Sorting_and_Searching.md` file a valuable resource for SDE1 interview preparation.