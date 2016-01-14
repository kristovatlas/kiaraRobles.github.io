---
layout: post
comments: true
title: Sorted Arrays to Binary Search Trees
tags: [swift]
---
# Sorted Arrays to Binary Search Trees


**Q. Why was the left node sad?**

**A. Because he had the least value in his family, and had no children.**


There are those who understand binary search trees and those who don't. I'll admit that I sometimes fail to remember the details. For a great many of us hobbyists turned programmers, the thought of traversing a binary search tree rarely (if ever) crosses my mind. But considering the simplicity and usefulness of these data structures they are worth familiarizing yourself with. Binary trees are implemented in nearly every 3D video game, wireless networking, and even the compression algorithms of .gifs.

#### Binary tree vs. Binary search tree

The distinction between a binary tree and a binary search tree is similar to that of a rectangle and a square. A square is always a rectangle, but a rectangle isn't always a square. A binary search tree is a binary tree, where each node has up to two children or leaves. But the search functionality on the data structure comes with the additional requirement that it is also sorted with the following properties:

- All values on the left subtree are less than the root
- All values on the right subtree are greater than the root
- Each subtree is itself, a binary search tree

![](http://sourcecodemania.com/wp-content/uploads/2012/05/binary-tree-vs-binary-search-tree.jpg)

#### Inventing the Binary Search Tree

Suppose you have an array of sorted numbers. Divide the array in half and each subsequent half repeatedly, while keeping the middle value separate. You can visualize the fold in the array expanding upwards into a tree.

![](https://blog.penjee.com/wp-content/uploads/2015/12/optimal-binary-search-tree-from-sorted-array.gif)

Searching this kind of data structure becomes significantly faster than searching each individual element from the initial array.

![](https://blog.penjee.com/wp-content/uploads/2015/11/binary-search-tree-sorted-array-animation.gif)

#### A Swift Implementation

If you know your array is ordered as a binary search tree, then you can search for a value and its position in the tree. This is done by repeatedly halving the search interval. Each iteration compares the middle value of the search interval against the input value or key. The inequality determines which half of the tree to continue searching, eliminating the other half of the search interval each time. If the resulting value matches the key, the function returns true and its position in the height of the tree. 

    func binarySearch(var arrayOfInts: [Int], key: Int) -> (Bool, Int)
    {
    
    var isBinarySearch: Bool = false
    var leftNode: Int = 0
    var rightNode: Int
    var root: Int = 0
    var mid: Int
    var treeHeight: Int
    let maxTreeHeight = arrayOfInts.count / 2
    
    /*                                                         */
    /*  Iterate thru the tree at the largest possible height   */
    /*                                                         */
    
    recursiveSearch: for treeHeight = 0; treeHeight < maxTreeHeight; treeHeight++
    {
        print(arrayOfInts)
        leftNode = 0
        rightNode = arrayOfInts.count - 1
        
        /*                                   */
        /*  Get the middle of the array,     */
        /*  and make its value the new root  */
        /*                                   */
        mid = (leftNode + rightNode)/2
        root = arrayOfInts[mid]
        
        /*                                   */
        /*  Eliminate half of the tree,      */
        /*  or break if the value is found   */
        /*                                   */
        if (root == key)
        {
            isBinarySearch = true
            break recursiveSearch
        }
        else if (root > key)
        {
            arrayOfInts.removeRange(mid...rightNode)
        }
        else if (root < key)
        {
            arrayOfInts.removeRange(leftNode...mid)
        }
    }
    
        /*                                                   */
        /*  Check if the sorted value matches the input key  */
        /*                                                   */
        if (root == key)
       {
           isBinarySearch = true
       }
      else {
          isBinarySearch = false
    }
    
    return (isBinarySearch, ++treeHeight)
    }


    var arrayOfInts: [Int] = [1, 3, 4, 5 , 6, 8, 9]
    var key = 4
    var result = binarySearch(arrayOfInts, key: key)
    print(result)

*WARNING: The following code was tested with a limited number of inputs, please do not implement into any actual search engines.*

The above code traverses the following path:

**5** <br>
**3**       8        
1   **4**   6   9

And prints the result below:

    [1, 3, 4, 5, 6, 8, 9]
    [1, 3, 4]
    [4]
    (true, 3)


