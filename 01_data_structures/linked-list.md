# Computer Algorithms: Linked List

## Introduction

The linked list is a data structure in which the items are ordered in a linear way. Although modern programming languages support very flexible and rich libraries that works with arrays, which use arrays to represent lists, the principles of building a linked list remain very important. The way linked lists are implemented is a ground level in order to build more complex data structures such as trees. 

It’s true that almost every operation we can perform on a linked list can be done with an array. It’s also true that some operations can be faster on arrays compared to linked lists. 

However understanding how to implement the basic operations of a linked list such as INSERT, DELETE, INSERT_AFTER, PRINT and so on is crucial in order to implement data structures as rooted trees, B-trees, red-black trees, etc.

## Overview

Unlike arrays where we don’t have pointers to the next and the previous item, the linked list is designed to support such pointers. In some implementations there is only one pointer pointing to the successor of the item. This kind of data structures are called singly linked lists. In this case the the last element doesn’t have a successor, so the pointer to its next element usualy is NULL. However the most implemented version of a linked list supports two pointers. These are the so called doubly linked lists.

[![Arrays vs. linked list](/wp-content/uploads/2012/06/0.-Arrays-vs.-linked-list.png)](/wp-content/uploads/2012/06/0.-Arrays-vs.-linked-list.png)Arrays items are defined by their indices, while the linked list item contains a pointer to its predecessor and his successor!

Let’s take a look at some examples. Here’s an array:

```php
A[2, 'hello', 'world', 13, 31]
```

We know that A[0] contains the number 2, while A[1] contains the word (string) “hello”. This is the very basic array representation where the indices are consecutive and start from 0.

In that case we know that if we’re looking at the item with index i, its next element has an index i+1, while his predecessor’s index is i-1. Thus we can easily iterate over items. However if we have an array with only 5 elements A[5] doesn’t exists, and in most of the cases is NULL. In other words array items are defined by their indices, and can be accessed directly with their index.

In the other hand most of the programming languages support associative arrays also called hash maps.

An associative array in PHP can be something like this.

```php
$a = array(
	'hello' => 'world',
	31	=> 12,
	'b7'	=> 144,
);
```

Now because the indices are not consecutive integers we don’t know the successor and the predecessor of the current item. Yes, there is some internal pointer that can give us the link to the previous and the next item, but explicitly we can’t refer them.

Of course some libraries may have functions that can point us to the next item, such a function is [next()](http://php.net/manual/en/function.next.php) in [PHP](/category/php/).

As we see arrays can replace the typical pointer-like representation of a linked list, but as I said already this data structure is crucial in order to understand more complex data structures.

Typically the linked list is implemented with two classes. The first one describing the item and the other one containing the main operations of the list such as search, insert, delete, etc.

The first class, typically called Item is a normal class containing various info about an item. In terms of persons, for instance, this class can contain the first and last names of the person, his address, phone etc. However it’s very important that this class contains pointers to its previous and its next objects.

In our terms each object of class Person will contain a pointer to a next object of the same class Person.

```php
class Person
{
	public $next = null;
	public $prev = null;
 
	protected $_firstName = '';
	protected $_lastName = '';
	protected $_phone = '';
 
 
	public function __construct($firstName, $lastName, $phone)
	{
		$this->_firstName = $firstName;
		$this->_lastName = $lastName;
		$this->_phone = $phone;
	}
 
	public function __toString()
	{
		return 'First name: ' . $this->_firstName 
			 . ', Last name: ' . $this->_lastName 
			 . ', Phone: ' . $this->_phone;
	}
}
```

So we must first design the structure of an item. 

The following thing to do is to define a linked list as an abstraction over a real world list, just like the stack and the queue. We can implement various operations for a linked list like insert, delete, insertBefore, insertAfter, sort, search, insertSorted etc. It’s up to the developer to decide which operations he wants to use. And of course this depends on the application.

## Implementation

## Operations

We can perform and define as much operations as we need. For instance we can code a linked list only with the basic insert, delete and print, but we can also go for insertBefore, insertAfter, deleteBefore, deleteAfter, insertSorted, which are described on the diagrams below.

## Insert

Inserting in the front of a list seems much like inserting into a queue. In this case the new item becomes the head of the list and its successor is the previous head of the list.

[![Inserting at the front of a Linked List](/wp-content/uploads/2012/06/1.-Insert-at-the-front-of-a-Linked-List.png)](/wp-content/uploads/2012/06/1.-Insert-at-the-front-of-a-Linked-List.png)Inserting at the front of the list

```php
class LList
{
	public $head;
	public $tail;
 
	public function insert($item) 
	{
		$item->next = $this->head;
 
		if ($this->head != null) {
			$this->head->prev = $item;
		}
 
		$this->head = $item;
	}
 
	public function __toString()
	{
		$cur = $this->head;
 
		$output = '';
		while ($cur) {
			$output .= $cur . ' ';
 
			$cur = $cur->next;
		}
 
		return $output . "\n";
	}
}
 
$ll = new LList();
 
$a = new Person('John', 'Smith', '555 9401');
$b = new Person('James', 'Johnes', '555 2454');
 
$ll->insert($a);
$ll->insert($b);
 
// James Johnes 555 2454
// John Smith 555 9401
echo $ll;
```

## Delete

Delete the head of the list is much like deleting an item from a queue. However we can implement also deleting a custom element that’s inside the list.

[![Delete an item](/wp-content/uploads/2012/06/4.-Delete-an-item.png)](/wp-content/uploads/2012/06/4.-Delete-an-item.png)Deleting an item from the inside of the list!

```php
class LList
{
	public $head;
	public $tail;
 
	public function insert($item) 
	{
		$item->next = $this->head;
 
		if ($this->head != null) {
			$this->head->prev = $item;
		}
 
		$this->head = $item;
	}
 
	public function delete($item) 
	{
		$cur = $this->head;
 
		while ($cur) {
 
			if ($cur == $item) {
				$prev = $cur->prev;
				$next = $cur->next;
 
				if ($prev != null) {
					$prev->next = $next;
				} else {
					// because head points to the first item
					$this->head = $next;
				}
 
				if ($next != null) {
					$next->prev = $prev;
				}
 
				return $cur;
			}
 
			$cur = $cur->next;
		}
	}
 
	public function __toString()
	{
		$cur = $this->head;
 
		$output = '';
		while ($cur) {
			$output .= $cur . ' ';
 
			$cur = $cur->next;
		}
 
		return $output . "\n";
	}
}
 
$ll = new LList();
 
$a = new Person('John', 'Smith', '555 9401');
$b = new Person('James', 'Johnes', '555 2454');
$c = new Person('Jeanne', 'Francois', '333 2323');
 
$ll->insert($a);
$ll->insert($b);
$ll->insert($c);
 
// prints objects $c, $b, $a:
// Jeanne Francois 333 2323
// James Johnes 555 2454
// John Smith 555 9401
echo $ll;
 
$ll->delete($b);
 
// prints objects $c, $a:
// Jeanne Francois 333 2323
// John Smith 555 9401
echo $ll;
```

## Insert before & insert after

Inserting before and after needs a reference to an already existing list item. After that the new object is inserted into it right place, as shown on the images below.

[![Insert after a given item of a Linked List](/wp-content/uploads/2012/06/2.-Insert-after-a-given-item-of-a-Linked-List.png)](/wp-content/uploads/2012/06/2.-Insert-after-a-given-item-of-a-Linked-List.png)Insert after splits the list after the pointed item and inserts the new object there!

```php
class LList
{
	public $head;
	public $tail;
 
	public function insert($item) 
	{
		$item->next = $this->head;
 
		if ($this->head != null) {
			$this->head->prev = $item;
		}
 
		$this->head = $item;
	}
 
	public function insertAfter($newItem, $item) 
	{
		$cur = $this->head;
		while ($cur) {
			if ($cur == $item) {
				$next = $cur->next;
 
				$cur->next = $newItem;
				$newItem->prev = $cur;
 
				if ($next != null) {
					$newItem->next = $next;
					$next->prev = $newItem;
				}
				return;
			}
			$cur = $cur->next;
		}
 
		$this->head = $this->tail = $item;
	}
 
	public function __toString()
	{
		$cur = $this->head;
 
		$output = '';
		while ($cur) {
			$output .= $cur . ' ';
 
			$cur = $cur->next;
		}
 
		return $output . "\n";
	}
}
 
$ll = new LList();
 
$a = new Person('John', 'Smith', '555 9401');
$b = new Person('James', 'Johnes', '555 2454');
$c = new Person('Jeanne', 'Francois', '333 2323');
 
$ll->insert($a);
$ll->insert($c);
 
// inserts $b after $a
$ll->insertAfter($b, $c);
 
// Jeanne Francois 333 2323
// James Johnes 555 2454
// John Smith 555 9401
echo $ll;
```

Insert a new object before an item is the same as insert after, but instead of splitting the list after the given item, we split it before it.

[![Insert before a given item of a Linked List](/wp-content/uploads/2012/06/3.-Insert-before-a-given-item-of-a-Linked-List.png)](/wp-content/uploads/2012/06/3.-Insert-before-a-given-item-of-a-Linked-List.png)Insert before and insert after are very similar operations!

## Search

Searching into a list can be modified depending on the application needs. Here we just compare the searched object with a list item.

[![Search for an item](/wp-content/uploads/2012/06/5.-Search-for-an-item.png)](/wp-content/uploads/2012/06/5.-Search-for-an-item.png)

```php
class LList
{
	public $head;
	public $tail;
 
	public function insert($item) 
	{
		$item->next = $this->head;
 
		if ($this->head != null) {
			$this->head->prev = $item;
		}
 
		$this->head = $item;
	}
 
	public function insertAfter($item, $key) 
	{
		$cur = $this->head;
		while ($cur) {
			if ($cur->data == $key) {
				$next = $cur->next;
 
				$cur->next = $item;
				$item->prev = $cur;
 
				if ($next != null) {
					$item->next = $next;
					$next->prev = $item;
				}
				return;
			}
			$cur = $cur->next;
		}
 
		$this->head = $this->tail = $item;
	}
 
	public function search($item)
	{
		$cur = $this->head;
 
		while ($cur) {
			if ($cur == $item) {
				return 1;
			}
 
			$cur = $cur->next;
		}
 
		return 0;
	}
 
	public function __toString()
	{
		$cur = $this->head;
 
		$output = '';
		while ($cur) {
			$output .= $cur . ' ';
 
			$cur = $cur->next;
		}
 
		return $output . "\n";
	}
}
 
$ll = new LList();
 
$a = new Person('John', 'Smith', '555 9401');
$b = new Person('James', 'Johnes', '555 2454');
$c = new Person('Jeanne', 'Francois', '333 2323');
 
$ll->insert($a);
$ll->insert($b);
 
// 1, because $b is in the list
echo $ll->search($b);
 
// 0, cause $c isn't in the list
echo $ll->search($c);
```

The operations here are only one sub-set of all possible operations you can implement on a linked list. It all depends on our needs. However the principles are important, so we can implement more complex data structures.

## Application

It’s true that linked list are a very basic data structure. Someone may wonder why we should code and implement linked list since we can do the same (with almost every modern programming language/library) with arrays. The answer is that data structures as trees are difficult to implement with arrays, so it’s easy to code them using pointers. Thus linked lists is a ground level for understanding them. Indeed each linked list is a tree with only one branch as shown on the image below.

[![Linked List & Trees](/wp-content/uploads/2012/06/6.-Linked-List-Trees.png)](/wp-content/uploads/2012/06/6.-Linked-List-Trees.png)Each linked list is a single branch tree!