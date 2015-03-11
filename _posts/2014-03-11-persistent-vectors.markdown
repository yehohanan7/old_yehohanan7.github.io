---
layout: post_page
title:  "Vectors in clojure"
date:   2015-03-11
categories: clojure
---

My only assumption about clojure's vector was that it uses arrays under the hood. Although it uses arrays underneath, it uses a trie of arrays! you can read more about trie on the internet, but in simple words - its a tree of nodes where the path to any particular node is determined by a sequence of values.

Example:

<img src="http://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Trie_example.svg/250px-Trie_example.svg.png">

As you can see in the above image, data (to, tea, ted, ten, inn, a) is stored in the tree in such a way that you can reach to any node by taking the corresponding path from root for each character of the string you are looking for. for example, if you are looking for "ten" then you take a path of left-right-right from the root.

Clojure's persistent data structure is implemented with a similar idea, but you must be already wondering how does clojure guarantee O(1) access to vectors if it stores data in a tree structure?

You are right, navigating any "balanced" tree from the root to the leaf node will be in the order of O(log n). But the implementation in clojure is smart, the height of the tree is kept very minimal by packing more data in each node and the height grows as you add more data.

I default, each node in the tree is an array of size 32. Let's assume you create a vector of 1024 elements, which will result in a structure as shown below.

                                    [. . . .................. ] => 32 childrens
                                    /           |             \
                                   /            |              \
                                  /             |               \   
                        [1 2 3 ... 32]    [33 35 ... 64] ...... [993 994 ... 1024]=> 1024 elements

If you now want to find any number in a vector of size less than 1024, it's a 2 step process:

<ul>
<li> Find the child in which the element belongs using an index
<li> Get the element in the child node (an array) using another index
</ul>

<br><br>
As you observe, even though the 1024 elements are stored in a tree, it takes just 2 steps to get an element and hence guarantees O(1) access to any element.

Although the above illustration is shown for 1024 elements. The tree will grow in height as you add more elements! This does not mean you can't guarantee O(1) because as the tree grows in height, you can pack more elements in each level. for example, level 3 in the above example will hold 1024 * 32 elements. <br/><br/>

How does clojure determine various indices to traverse the tree? <br/><br/>

It's simple, clojure uses the index that you want to lookup in a vector to derive the indices that is required to traverse the tree. <br/>

For example, if you are looking for an element at position 2 in a vector of size 1024, Clojure will convert the index into its binary value and group them into sets of 5. where each set represents an index at each level from the root node. <br/>
Hence the index 2 in its binary form 00000 00001 is used to arrive at a simple expression "root[0000][00001]", which is used to get the element you are looking for.


You might be wondering why would we need this kind of a trie at the first place? the answer is to achieve immutability! Yes, now any modification you do on the vector doesn't require you to create a complete copy of the entire vector. Rather, you only copy the nodes that needs to change and reuse the remaining nodes as they are. you can read more about structural sharing to understand how to achieve immutability by sharing structures.
