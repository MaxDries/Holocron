---
theme: simple
highlightTheme: zenburn
css: css/holocronSlides.css
---

# Data Structures

## Organising Your Data!



---

# What are Data Structures?

* Imagine you have a messy room. Data structures are like organizing your belongings into drawers, boxes, or shelves.
* They provide a way to store and manage data efficiently.
* Choosing the right data structure can make your programs faster and easier to understand.

---

# Arrays: Ordered Lists

* **Think of it as a numbered list:**
    * 1. Apples
    * 2. Bananas
    * 3. Oranges
* **Key Features:**
    * Elements are stored in a specific order.
    * Each element has an index (position).
    * You can access elements by their index.
* **Example:** A list of student names, a sequence of numbers, or a series of game scores.

---

# Arrays start at 0

- When counting the elements in an array, start at 0.

---

# Arrays: Visual Example

```
[ "Apple", "Banana", "Orange" ]
```

* "Apple" is at index 0.
* "Banana" is at index 1.
* "Orange" is at index 2.

---

# Real-Life Examples: Arrays

--

# Example 1: To-Do List

* A to-do list is a perfect example of an array.
* Each task is an element in the list.
* The order of the tasks matters.
* You can add, remove, and access tasks by their position in the list.

```
[ 
	"Buy groceries", 
	"Finish homework", 
	"Call a friend", 
	"Go for a run" 
]
```

--

# Example 2: Music Playlist

* A music playlist is another array.
* Each song is an element.
* The order of the songs determines the playback sequence.
* You can shuffle (rearrange) the array, add songs, or remove them.

```
[ 
	"Song A", 
	"Song B", 
	"Song C", 
	"Song D" 
]
```

--

# Example 3: Game Board

* A game board (like a chessboard or tic-tac-toe board) can be represented as a 2D array (an array of arrays).
* Each element represents a square on the board.
* The position of the element corresponds to its location on the board.

```
[ 
	["X", "O", " "], 
	[" ", "X", "O"], 
	["O", " ", "X"] 
]
```


---
# Arrays: Common Operations

```
[ "Apple", "Banana", "Orange" ]
```

* **Accessing an element:** Get the element at a specific index.
    * Example: Get the fruit at index 1 (Banana).
* **Adding an element:** Insert a new element into the array.
    * Example: Add "Grapes" to the end of the array.
* **Removing an element:** Delete an element from the array.
    * Example: Remove "Banana" from the array.
* **Finding the length:** Determine the number of elements in the array.

---

# Dictionaries (or Maps): Key-Value Pairs

* **Think of it as a real-world dictionary:**
    * "Apple": A round fruit with red or green skin.
    * "Banana": A long, curved yellow fruit.
* **Key Features:**
    * Stores data as key-value pairs.
    * Keys are unique identifiers.
    * Values are the associated data.

---
# Dictionaries

**Examples:** 
* A phone book (name: phone number), 
* a product catalog (product ID: product details), or 
* a user profile (username: user information).

---

# Dictionaries: Visual Example

```
{
"name": "Alice",
"age": 16,
"grade": "10th"
}
```

* "name" is the key, "Alice" is the value.
* "age" is the key, 16 is the value.
* "grade" is the key, "10th" is the value.

---

# Real-Life Examples: Dictionaries

--

# Example 1: Contact List

* A contact list on your phone is a dictionary.
* Each contact has a name (key) and associated information (value) like phone number, email, and address.

```

{ 
	"Alice": { 
		"phone": "123-456-7890",
		"email": "[email address removed]" 
	}, 
	"Bob": { 
		"phone": "987-654-3210", 
		"email": "[email address removed]" 
	} 
}
```

--
# Example 2: Online Shopping Cart

* An online shopping cart uses a dictionary.
* Each product ID (key) is associated with the quantity of that product (value).

```
{ 
	"product123": 2, 
	"product456": 1, 
	"product789": 3 
}
```

--

# Example 3: Website Configuration

* Website settings or configuration files use dictionaries.
* Each setting name (key) is associated with its value.

```
{ 
	"theme": "dark", 
	"language": "en", 
	"notifications": true 
}
```


---

# Dictionaries: Common Operations

* **Accessing a value:** Get the value associated with a specific key.
    * Example: Get the age of "Alice" (16).
* **Adding a key-value pair:** Insert a new key-value pair into the dictionary.
    * Example: Add "city": "New York" to the dictionary.
* **Removing a key-value pair:** Delete a key-value pair from the dictionary.
    * Example: Remove the "grade" key-value pair.
* **Checking if a key exists:** Determine if a key is present in the dictionary.

---

# Arrays vs. Dictionaries

| Feature        | Arrays                               | Dictionaries (Maps)                     |
| -------------- | ------------------------------------ | --------------------------------------- |
| Order          | Ordered by index                     | Unordered (usually)                      |
| Access         | By index (number)                    | By key (any unique identifier)          |
| Use Cases      | Ordered lists, sequences             | Key-value data, lookups, associations |
| Example        | List of scores, queue of tasks       | Phone book, configuration settings      |

---

# Why are Data Structures Important?

* **Efficiency:** They help programs run faster and use less memory.
* **Organization:** They make code easier to understand and maintain.
* **Problem Solving:** They provide tools to solve complex problems effectively.
* **Real-World Applications:** They are used in countless applications, from databases to games.
