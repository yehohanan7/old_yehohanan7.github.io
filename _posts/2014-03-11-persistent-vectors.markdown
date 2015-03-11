---
layout: post_page
title:  "Vectors in clojure"
date:   2015-03-11
categories: clojure
---

Clojure's vectors are "immutable" collection of ordered elements with almost constant time access (log to base 32 N) to any element. This lead me to open the lid and look at the source code to figure out how clojure achieves immutability and constant time access to the ordered elements without copy on modification.

What I learnt from the source was that clojure vectors uses a "trie" of arrays! you can read more about tries on the internet, but in simple words - its a tree of nodes where the path to any particular node is determined by a sequence of predetermined values/indices.

Example:

<img src="http://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Trie_example.svg/250px-Trie_example.svg.png">

As you can see in the above image, data (to, tea, ted, ten, inn, a) is stored in the tree in such a way that you can lookup any element by building a traversal path just by using the characters of lookup element. Example, if you are looking for "ten" then you take a path of left-right-right from the root.

Clojure's persistent vector is also implemented with a similar idea, but you must be wondering how clojure guarantees almost constant time access to vectors if it stores data in a tree structure?

You are right, traversing any "balanced" tree from the root to the leaf node will be in the order of O(log n). But the implementation in clojure is smart, the height of the tree is kept very minimal by packing more data in each node and hence the height grows only for very large vectors.

In clojure, each node in the tree is an array of size 32. Let's assume you create a vector of 1024 elements, which will result in a structure as shown below.

                                    [. . . .................. ] => 32 childrens
                                    /           |             \
                                   /            |              \
                                  /             |               \   
                        [1 2 3 ... 32]    [33 35 ... 64] ...... [993 994 ... 1024]=> 1024 elements

If you now want to lookup any element in a vector of size less than 1024, it's a 2 step process:

<ul>
<li> Find the child in which the element belongs using an index - i
<li> Find the index in the child node using another index - j
</ul>

<br/>
Now with those indices, you can get any element you want using root[i][j]


<br><br>
As you notice, even though the 1024 elements are stored in a tree, you can look up any element in O(1) if you could figure out the indices (i & j).

The above illustration was for a vector of size 1024, which required 2 indices to access any element. However, As you add more elements to the vector, the tree will grow taller and you would need to determine more indices to lookup any element. But since we can pack a lot of elements at each level, the tree remains very shallow and hence provides constant time access to any element in the tree as long as you can calculate the indices "efficiently"

For example, level 3 in the above example will hold 1024 * 32 elements, but you can access any element in constant time using 3 indices <br/><br/>

How does clojure determine the indices to access element in the tree? <br/><br/>

It's simple, clojure translates the index that you want to lookup in a vector into multiple indices to traverse the tree. <br/>

For example, if you are looking for an element at position 2 in a vector of size 1024, Clojure will translate the index into its binary value and group them into sets of 5. where each set represents an index at each level from the root node. <br/>
Hence the index 2 in its binary form 00000 00001 is used to arrive at a simple expression "root[0000][00001]", which is used to get the element you are looking for. <br/><br/>

Because clojure stores the elements in a trie, it's easy to provide immutability using structural sharing.
