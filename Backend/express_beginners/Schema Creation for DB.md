# Creation of Schema
#### Now the question arises, we did make the notes array, but i want the data to be persistent and saved, so how would we tell the db, which and what data would we want to save.

(Thus showing the database how does my Note contents will look like-telling schema how to store my note.)

#### This is done in `models/` folder

`note.model.js`
```js
const mongoose = require('mongoose');

const noteSchema = new mongoose.Schema({
    "title": String,//we create a schema and give it a certain datatype to occupy
    "description": String,
})
const noteModel = mongoose.model('note', noteSchema) //

module.exports = noteModel;
```

## 🔹 What is `noteModel`?

`noteModel` is a **Mongoose model**  essentially a **JavaScript interface to your MongoDB collection**.

It lets you **create, read, update, and delete notes in the database**.

---

## 🔹 Breaking Your Code Down

### 1. Import Mongoose

```js
const mongoose = require('mongoose');
```

Mongoose is an ODM (Object Data Modeling) library for **MongoDB**.

---

### 2. Define Schema

```js
const noteSchema = new mongoose.Schema({
  title: String,
  description: String,
});
```

👉 Schema = **structure of your data**

It defines:

- Fields (`title`, `description`)
    
- Data types (`String`)
    

Think of it like:

> “Every note must look like this.”

---

### 3. Create Model

```js
const noteModel = mongoose.model('note', noteSchema);
```

👉 This is the key line.

It does 2 things:

1. Creates a **Model class**
    
2. Links it to a MongoDB collection
    

### Behind the scenes:

- Model name: `"note"`
    
- Collection created: `"notes"` (pluralized automatically)

---

## 🔹 What `noteModel` Actually Is

👉 It’s a **constructor + API combined**

You can:

- Create new notes
    
- Query existing notes
    
- Update/delete notes

---

## 🔹 Example Usage

### Create a note

```js
const newNote = new noteModel({
  title: "Study",
  description: "Revise backend"
});

await newNote.save();
```

---

### Read notes

```js
const notes = await noteModel.find();
```

---

### Update

```js
await noteModel.updateOne(
  { title: "Study" },
  { description: "Revise Express" }
);
```

---

### Delete

```js
await noteModel.deleteOne({ title: "Study" });
```

---

## 🔹 Analogy (Important)

Think of it like this:

|Concept|Real-world analogy|
|---|---|
|Schema|Blueprint of a form|
|Model|Machine that creates & manages forms|
|Document|One filled form|

So:  
 `noteModel` = **machine that handles all notes**

---

## 🔹 Why We Need Models

Without models:

- No structure
    
- No validation
    
- Manual DB queries
    

With models:

- Clean code
    
- Built-in validation
    
- Easy CRUD operations
    

---

## 🔹 One-Line Definition

`noteModel` = **a Mongoose object used to interact with the "notes" collection using the defined schema**

---



---



# Usage of the Model Defined in the `note.model.js`

### We import the model in the `app.js`

```js
const noteModel = require('./src/model/note.model')
```

#### Lets Discuss of Certain API and works that can be done by this 