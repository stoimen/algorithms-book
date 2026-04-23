# Computer Algorithms: Stack and Queue

## Introduction

Every developer knows that [computer algorithms](/category/algorithms/) are tightly related to data structures. Indeed many of the algorithms depend on a data structures and can be very effective for some data structures and ineffective for others. A typical example of this is the heapsort algorithm, which depends on a data structure called “heap”. In this case although the stack and the queue are data structures instead of pure algorithms it’s imporant to understand their structure and the way they operate over data. 

However, before we continue with the concrete realization of the stack and the queue, let’s first take a look on the definition of this term. A data structure is a logical abstraction that “models” the real world and presents (stores) our data in a specific format. The access to this data structure is often predefined thus we can access directly every item containing data. This help us to perform a different kind of tasks and operations over different kind of data structures – insert, delete, search, etc.. A typical data structures are the stack, the queue, the linked list and the tree.

All these structures help us perform specific operations effectively. For instance searching in a balanced tree is faster than searching in a linked list.

It is also very important to note that data structures can be represented in many different ways. We can model them using arrays or pointers, as shown in this post. In fact the most important thing is to represent the logical structure of the data structure you’re modeling. Thus the stack is a structure that follows the LIFO (Last In First Out) principle and it doesn’t matter how it is represented in our program (whether it will be coded with an array or with pointers). The important thing into a stack representation is to follow the LIFO principle correctly. In this case if the stack is an array only its top should be accessible and the only operation must be inserting new top of the stack.

## Overview

The stack and the queue are somehow related data structures as they represent two parts of somehow identical logics. Thus they are commonly described in pair.

## Stack

The stack data structure models the real-world stack. You can think of it as stack of boxes one above the other. Thus the only way to put another item into the stack is to put it above all other items (on its top). This operation is often called “push”. In the other hand taking an item from the stack is called pop, and also only the highest item can be “poped”. The following image describes better the structure of the stack and its operations – push and pop.

[![Stack Operations](/wp-content/uploads/2012/06/1.-Stack-Operations.png)](/wp-content/uploads/2012/06/1.-Stack-Operations.png)The operations of insert and delete an item from the stack are commonly called push and pop!

We see here how computer data structures model the real world. The stack data structure indeed makes no exception and models the real-world stacks.

## Stack Implementation

As I said a stack can be implemented in some different ways. The first approach is to use an array (using the specific language syntax of the programming language of your choice). Here’s the implementation of a stack using an array in [PHP](/category/php/).

```php
$stack = array();
 
function push($data, &$stack) {
	$stack[] = $data;
}
 
function pop(&$stack)
{
	$len = count($stack);
	$top = $stack[$len-1];
 
	unset($stack[$len-1]);
 
	return $top;
}
 
// array()
print_r($stack);
 
push(1, $stack);
push(2, $stack);
push('some test', $stack);
push(array(25,12,1999), $stack);
 
// [1, 2, 'some test', [25, 12, 1999]]
print_r($stack);
 
// [25, 12, 1999]
echo pop($stack);
// 'some test'
echo pop($stack);
 
// [1, 2]
print_r($stack);
```

However there are much easier ways to do the same thing with PHP since there are lots of predefined functions that work with stacks.

```php
$stack = array();
 
function push($data, &$stack) {
	$stack[] = $data;
}
 
function pop(&$stack)
{
	return array_pop($stack);
}
 
// array()
print_r($stack);
 
push(1, $stack);
push(2, $stack);
push('some test', $stack);
push(array(25,12,1999), $stack);
 
// [1, 2, 'some test', [25, 12, 1999]]
print_r($stack);
 
// [25, 12, 1999]
echo pop($stack);
// 'some test'
echo pop($stack);
 
// [1, 2]
print_r($ret);
```

As in many programming languages here the example makes use of integers but it can be modified to work with more complex data types as objects, mutli dimensional arrays, etc. However we can use a higher level abstraction in order to represent a stack. Here’s a short example of a stack using pointers. The stack class only holds a pointer to the top of the stack. Thus only the top can be “poped”. Also each elements points to its predecessor. Using this abstraction we’re sure that the programmer can perform only these two operations – “pop” and “push”.

```php
class Struct
{
	protected $_data = null;
	protected $_next = null;
 
	public function __construct($data, $next)
	{
		$this->_data = $data;
		$this->_next = $next;
	}
 
	public function getData()
	{
		return $this->_data;
	}
 
	public function setData(&$data)
	{
		$this->_data = $data;
	}
 
	public function getNext()
	{
		return $this->_next;
	}
 
	public function setNext(&$next)
	{
		$this->_next = $next;
	}
}
 
class Stack
{
	protected $_top = null;
 
	public function push($data)
	{
		$item = new Struct($data, null);
 
		if ($this->_top == null) {
			$this->_top = $item;
		} else {
			$item->setNext($this->_top);
			$this->_top = $item;
		}
	}
 
	public function pop()
	{
		if ($this->_top) {
			$t = $this->_top;
			$data = $t->getData();
 
			$this->_top = $this->_top->getNext();
 
			$t = null;
 
			return $data;
		}
	}
 
	public function __toString()
	{
		$output = '';
		$t = $this->_top;
		while ($t) {
			$output .= $t->getData() . ' ';
			$t = $t->getNext();
		}
 
		return $output;
	}
}
 
$s = new Stack();
$s->push(1);
$s->push(2);
$s->push(3);
 
// 3 2 1
echo $s;
 
$s->pop();
$s->pop();
 
// 1
echo $s;
```

## Queue

As mentioned above the queue is somehow related to the stack data structure. However it follows a different principle – FIFO (First In First Out), which means that the item that has been in the queue for the longest time is retrieved first.

[![Queue Operations](/wp-content/uploads/2012/06/2.-Queue-Operations.png)](/wp-content/uploads/2012/06/2.-Queue-Operations.png)Inserting and deleting from a queue happen in the opposite sites of the queue!

This comes again from the real world, where we can think of a queue of people waiting in front of a movie theater. In this case the person that has waited the most takes its ticket first.

## Queue Implementation

An array representation of a queue isn’t a difficult task. However the only example of a queue here is using pointers. Indeed the following code syntax is very tightly related to PHP so only the main principles of supporting a queue functionality is important.

```php
class Item
{
	public $data = null;
	public $next = null;
	public $prev = null;
 
	public function __construct($data)
	{
		$this->data = $data;
	}
}
 
class Queue
{
	protected $_head = null;
	protected $_tail = null;
 
	public function insert($data)
	{
		$item = new Item($data);
 
		if ($this->_head == NULL) {
			$this->_head = $item;
		} else if ($this->_tail == NULL) {
			$this->_tail = $item;
			$this->_head->next = $this->_tail;
			$this->_tail->prev = $this->_head;
		} else {
			$this->_tail->next = $item;
			$item->prev = $this->_tail;
			$this->_tail = $item;
		}
	}
 
	public function delete()
	{
		if (isset($this->_head->data)) {
 
			$temp = $this->_tail;
			$data = $temp->data;
 
			$this->_tail = $this->_tail->prev;
 
			if (isset($this->_tail->next))
				$this->_tail->next = null;
			else 
				$this->_tail = $this->_head = null;
 
			return $data;
		}
 
		return FALSE;
	}
 
	public function __toString()
	{
		$output = '';
		$t = $this->_head;
		while ($t) {
			$output .= $t->data . ' | ';
			$t = $t->next;
		}
 
		return $output;
	}
}
 
 
$q = new Queue();
 
$q->insert(1);
$q->insert(2);
$q->insert(3);
 
// 1 2 3
echo $q;
 
$q->delete();
$q->delete();
 
// 1
echo $q;
 
$q->insert(15);
$q->insert('hello');
$q->insert('world');
$q->delete();
 
// 1 15 "hello"
echo $q;
```

## Application

Stacks and queues are widely used in programming. By defining stacks and queues we somehow predefine the way our data structure is accessed, thus we’re sure that our program will access the data in a specific manner. For instance if we code a queue for a list or upcomming commands, we’re sure that the most waited command will be executed first. In this case we predefine the order the commands are processed. In the web programming, especially in JavaScript, every developer knows what’s an event fired in a web browser environemtn. In case of many events, they’re putted into a queue and they are executed consecutively in the order they were fired by the user.

Another example is the execution stack of most of the programming compilers and interpreters. We know that in a OOP languages, such as PHP for instance, there’s a stack of function calls. In case of failure we can easily see the “stack trace”.

You see how many examples of queues and stacks there are in the real-world programming. These two structures are easy to implement yet very important in order to understand other more complex data structures as linked lists and trees.