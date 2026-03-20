# Intialization of the DB

## Folder Structure:
```
├── src/
│   ├── db/
|	│   ├── db.js
│   └── app.js
```

## Requiring the Package:
```js
const mongoose = require('mongoose');
```

### We write a async function to connect with the DB in the db.js file

This file only contains the logic to connect with the db.

**`db.js`**
```js
//the file only contains the logic to connect with the db;

const mongoose = require('mongoose');

async function ConnectDB(){

    await mongoose.connect("mongodb+srv://sgcpu:<DB_pass>@express-backend.x58uztz.mongodb.net/pr1")

    console.log("Connected to DB")
}

module.exports = ConnectDB
```

### Once the connection logic is written it is exported to the `Server.js`

`server.js`
```js
///start the server
const app = require('./src/app')
const ConnectDB = require('./src/db/db')
const port = 3000

ConnectDB();

app.listen(port, ()=>{
    console.log(`Server Running on Port ${port}`)
})
```