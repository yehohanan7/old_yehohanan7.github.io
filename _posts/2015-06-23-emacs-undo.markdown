---
layout: post
title:  Emacs undo/redo
date:   2015-06-23
categories: Emacs
---

Having used emacs for quite a while, I never paid full attention to how emacs' undo worked. I was very annoyed initially when I couldn't figure out how to redo an undo, but managed to work around with it without fully understanding how it worked. <br/>

This blog is just to shed some light on the emacs' undo ring with an example. <br/><br/>

You can imagine that emacs undo history is like a singly linked list of actions with the head pointing to the current action. Suppose that you open a new buffer and enter the text "123", the linked list will look like below. where, the <u>underlined</u> node is the current action node<br/>

[<u>123</u>] -> [::empty::] <br/>

Emacs does 2 things every time you do an undo - it moves the current action node to the right and whatever it sees there, emacs adds that again to the front of the change list for later use! <br/>

Confused? Let's see that with some more actions to the current buffer! <br/>

Now suppose that you undo your last change and enter the text "abc", i.e, you performed 2 actions <br/>

<ul>
<li/> "undo" action - this results in the action list as shown below. Notice that the current node is the ::empty:: node as each undo action moves the current node pointer to the right. <br/>Also notice that ::empty:: is added again to the front of the list <br/><br/>

[::empty::] -> [123] -> [<u>::empty::</u>] <br/><br/>

<li/> entering the text "abc" - this results in the action list as shown below. Notice that any new action resets the current node pointer back to the first node <br/><br/>

[<u>abc</u>] -> [::empty::] -> [123] -> [::empty::] <br/><br/> <br/>

</ul>


Now let's undo 3 times and enter the text "xyz". i.e, we perfomed 4 actions <br/>

<ul>
<li/> First "undo" action - which results in the action list as below <br/>

[::empty::] -> [abc] -> [<u>::empty::</u>] -> [123] -> [::empty::] <br/> <br/>

<li/> Second "undo" action - which results in the action list as below, notice that "123" is added to the front again! <br/>

[123] -> [::empty::] -> [abc] -> [::empty::] -> [<u>123</u>] -> [::empty::] <br/> <br/>

<li/> Third "undo" action - which results in the action list as below <br/>

[::empty::] -> [123] -> [::empty::] -> [abc] -> [::empty::] -> [123] -> [<u>::empty::</u>] <br/> <br/>

<li/> entering the text "xyz" - this results in the action list as shown below <br/>

[<u>xyz</u>] -> [::empty::] -> [123] -> [::empty::] -> [abc] -> [::empty::] -> [123] -> [<u>::empty::</u>] <br/> <br/>

</ul>

<br/>

If you followed the example and tried the sequence of actions in a new buffer, then your current change node should be the first node in the list which is "xyz". Now it's your time to start pressing C-/ as you traverse the list one by one from left to right. notice why you see "123" twice!





