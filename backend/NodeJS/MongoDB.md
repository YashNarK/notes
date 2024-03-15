# MongoDB

**MongoDB**, a powerful NoSQL database system:

1. **What is MongoDB?**

   - MongoDB is an **open-source document-oriented database** designed to handle large-scale data efficiently.
   - It falls under the category of **NoSQL databases** because it doesn't use traditional tables for storage and retrieval.
   - Instead of rows and columns, MongoDB stores data in **collections** and **documents**.
   - Developed by **MongoDB, Inc.**, it was initially released in February 2009.
   - MongoDB provides official driver support for popular languages like C, C++, C#, .NET, Go, Java, Node.js, Perl, PHP, Python, Motor, Ruby, Scala, and Swift.
   - Notable companies like Facebook, Nokia, eBay, and Google use MongoDB to manage their vast amounts of data.

2. **How It Works:**

   - MongoDB operates as a database server, allowing you to create multiple databases.
   - Key components:
     - **Database**: Contains collections (similar to tables in MySQL).
     - **Collection**: A group of related documents.
     - **Document**: The actual data stored in MongoDB.
   - Documents are created using **fields**, which are key-value pairs (similar to columns in relational databases).
   - MongoDB is **schema-less**, meaning each document can have a different structure.
   - Data is stored in **BSON format** (Binary JSON), which is more efficient for storage and querying.
   - Nested data allows complex relations within a single document, making data retrieval more efficient than SQL joins.
   - The maximum size of a BSON document is **16MB**.

3. **Features and Characteristics:**
   - **Scalability**: MongoDB scales horizontally, allowing for distributed data across multiple servers.
   - **Flexibility**: Schema-less design accommodates dynamic and unstructured data.
   - **Performance**: Efficient querying and indexing.
   - **Cross-Platform**: MongoDB works on various operating systems.
   - **JSON-Like Documents**: Data is stored in a JSON-like format called BSON.
   - **High Availability**: Supports replica sets for fault tolerance.
   - **Load Balancing**: Distributes read and write operations across nodes.
   - **Aggregation Framework**: Powerful data aggregation capabilities.
   - **Geospatial Indexing**: Supports geospatial queries.
   - **Text Search**: Full-text search functionality.
   - **TTL Indexes**: Automatic data expiration.
   - **Encryption**: Data encryption at rest and in transit.

In summary, MongoDB's flexibility, scalability, and efficient handling of unstructured data make it a popular choice for modern applications.

# Installing MongoDB (Windows)

The steps to **install MongoDB Community Edition on Windows**.

You can follow this guide to set up MongoDB for your development environment:

1. **Download MongoDB Installer**:

   - Visit the [official MongoDB website](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-windows/) and click on the **"Download"** button.
   - Under the **"Community Server"** section, select the version of MongoDB you want to download (e.g., **Version 7.0.4**).
   - Choose **"Windows x64"** as the operating system.
   - Click the **"Download (MSI)"** button.

2. **Run the Installer**:

   - Locate the downloaded MSI file (usually in your **Downloads** folder).
   - Double-click the MSI file to launch the installation wizard.
   - Read and accept the **Terms of Agreement**.

3. **Installation Steps**:

   - Follow the steps in the installation wizard:
     - Choose the installation directory (default is recommended).
     - Select the components you want to install (e.g., MongoDB Server, MongoDB Compass).
     - Configure the **data directory path** (where MongoDB will store its data files).
     - Choose whether to install MongoDB as a **Windows service** (recommended for production use).
     - Review your selections and click **"Install"**.

4. **Start MongoDB**:

   - After installation, MongoDB will be available in the specified installation directory (e.g., `C:\Program Files\MongoDB\Server\7.0\bin`).
   - To start MongoDB, open a command prompt and navigate to the bin directory:
     ```
     cd C:\Program Files\MongoDB\Server\7.0\bin
     ```
   - Run the following command to start the MongoDB server:
     ```
     mongod
     ```

5. **Access MongoDB Shell (mongosh)**:

   - The MongoDB Shell (mongosh) is not installed with MongoDB Server by default.
   - Download and install mongosh separately from the [mongosh installation instructions](https://docs.mongodb.com/mongodb-shell/install/).

6. **Verify Installation**:

   - Open another command prompt and navigate to the same bin directory.
   - Run the following command to start the MongoDB shell:
     ```
     mongosh
     ```
   - You should see the MongoDB shell prompt (`mongosh>`) if everything is set up correctly.

7. **Additional Considerations**:
   - MongoDB Atlas is a hosted MongoDB service in the cloud that requires no installation overhead. Consider using it for a hassle-free experience.
   - Before deploying MongoDB in production, review the [Production Notes](https://docs.mongodb.com/manual/administration/production-notes/) for best practices.

Remember to adjust the installation paths and configurations based on your preferences.

# Installing MongoDB Compass (Windows)

The steps to **install MongoDB Compass** on Windows.

MongoDB Compass is a powerful graphical tool that allows you to explore and manipulate your MongoDB data. Here's how you can set it up:

1. **Download MongoDB Compass**:

   - Visit the [official MongoDB Compass download page](https://www.mongodb.com/try/download/compass).
   - Choose the appropriate version of MongoDB Compass for your Windows operating system (64-bit or 32-bit).
   - Click the **"Download"** button to start the download.

2. **Install MongoDB Compass**:

   - Locate the downloaded installer (usually in your **Downloads** folder).
   - Double-click the installer to launch the installation process.
   - Follow the installation prompts:
     - Choose the installation directory (you can keep the default).
     - Select the components you want to install (MongoDB Compass and MongoDB Compass Community Edition).
     - Customize the installation settings if needed.
     - Click **"Install"** to begin the installation.

3. **Configure MongoDB Compass**:

   - During installation, you'll be prompted to configure MongoDB Compass settings:
     - **Data Directory**: Choose where MongoDB Compass will store its data files.
     - **Enable Performance Monitoring**: Decide whether to participate in anonymous usage statistics.
     - **Start MongoDB Compass**: Choose whether to start MongoDB Compass after installation.

4. **Launch MongoDB Compass**:

   - Once the installation is complete, you can find MongoDB Compass in your Start menu or search for it.
   - Launch MongoDB Compass.
   - Connect to your MongoDB deployment (local or remote) by providing the necessary connection details (host, port, authentication, etc.).

5. **Explore Your Data**:
   - MongoDB Compass provides an intuitive interface to explore your databases, collections, and documents.
   - Use the GUI to query data, create indexes, manage users, and visualize your schema.

Remember that MongoDB Compass is a powerful tool for developers and administrators. It allows you to interact with MongoDB in a user-friendly way, making it easier to manage your databases.

# Mongoose

**Mongoose**, a powerful package that helps you work with MongoDB in Node.js. Here are the key points:

1. **What is Mongoose?**

   - **Mongoose** is an **ODM (Object Data Modeling)** library for MongoDB.
   - It provides an elegant way to interact with MongoDB by defining schemas, models, and queries.
   - Mongoose allows you to create and manage MongoDB documents in an asynchronous environment.

2. **Key Features of Mongoose**:

   - **Schema Definition**: Define the structure of your data using schemas.
   - **Model Creation**: Create models based on schemas to interact with MongoDB collections.
   - **Validation**: Define validation rules for your data.
   - **Middleware**: Use pre and post hooks for document operations.
   - **Query Building**: Construct complex queries using Mongoose methods.
   - **Population**: Populate references between documents.
   - **Plugins**: Extend Mongoose functionality with plugins.
   - **Connection Management**: Handle MongoDB connections easily.

3. **Installing Mongoose**:

   - To install Mongoose, use npm (Node Package Manager):
     ```
     npm install mongoose
     ```

4. **Basic Usage Example**:

   - Here's a simple example of using Mongoose to connect to a MongoDB database and define a schema:

     ```javascript
     const mongoose = require("mongoose");

     // Connect to MongoDB
     mongoose.connect("mongodb://localhost/mydb", {
       useNewUrlParser: true,
       useUnifiedTopology: true,
     });

     // Define a schema
     const userSchema = new mongoose.Schema({
       name: String,
       age: Number,
       email: String,
     });

     // Create a model based on the schema
     const User = mongoose.model("User", userSchema);

     // Create a new user
     const newUser = new User({
       name: "John Doe",
       age: 30,
       email: "john@example.com",
     });

     // Save the user to the database
     newUser.save((err, savedUser) => {
       if (err) {
         console.error("Error saving user:", err);
       } else {
         console.log("User saved successfully:", savedUser);
       }
     });
     ```

# Mongoose Schema

In the context of **Mongoose**, a **schema** is a configuration object that defines the structure of a document (or model) in MongoDB. Let's explore what a Mongoose schema is and how it works:

1. **What is a Mongoose Schema?**

   - A **schema** in Mongoose acts as a blueprint for defining the shape of documents that will be stored in a MongoDB collection.
   - It specifies the fields (properties) that each document can have, along with their data types and additional options.
   - Schemas allow you to enforce consistency and validation rules for your data.

2. **Creating a Mongoose Schema**:

   - To create a schema, use the `mongoose.Schema` constructor.
   - Define the fields and their data types within the schema.
   - Example:

     ```javascript
     const mongoose = require("mongoose");

     // Define a user schema
     const userSchema = new mongoose.Schema({
       name: { type: String, required: true },
       age: { type: Number, min: 18 },
       email: { type: String, unique: true },
     });

     // Create a model based on the schema
     const User = mongoose.model("User", userSchema);
     ```

3. **Schema Fields and Options**:

   - Each field in the schema is defined as a key-value pair.
   - Common field options include:
     - `type`: Specifies the data type (e.g., `String`, `Number`, `Date`, etc.).
     - `required`: Indicates whether the field is mandatory.
     - `unique`: Ensures that the field value is unique across documents.
     - Other options like `min`, `max`, `default`, and custom validation functions can also be used.

4. **Using the Schema in Models**:

   - Once you have a schema, create a **model** based on it.
   - Models allow you to interact with MongoDB collections using the schema definition.
   - Example:

     ```javascript
     // Create a new user document
     const newUser = new User({
       name: "Alice",
       age: 25,
       email: "alice@example.com",
     });

     // Save the user to the database
     newUser.save((err, savedUser) => {
       if (err) {
         console.error("Error saving user:", err);
       } else {
         console.log("User saved successfully:", savedUser);
       }
     });
     ```

5. **Schema Validation and Middleware**:
   - Schemas allow you to define custom validation logic for fields.
   - You can also use **middleware** (pre and post hooks) to execute code before or after certain operations (e.g., saving a document).

In summary, Mongoose schemas provide a structured way to define the shape of your data, ensuring consistency and validation in your MongoDB documents.

## Mongoose Scheme Types

In Mongoose, there are various Schema Types that you can use to define the structure of your MongoDB documents. Here's a list of commonly used Schema Types in Mongoose:

1. **String**: Represents a string value.
2. **Number**: Represents a numeric value (integer or floating-point).

3. **Date**: Represents a date value.

4. **Buffer**: Represents binary data.

5. **Boolean**: Represents a boolean value (`true` or `false`).

6. **Mixed**: Represents a flexible/undefined schema type.

7. **ObjectId**: Represents a MongoDB ObjectId.

8. **Array**: Represents an array of values.

9. **Map**: Represents a map of nested schema types.

10. **Schema.Types.Decimal128**: Represents a 128-bit decimal value.

11. **Schema.Types.ObjectId**: Represents a MongoDB ObjectId. (Can be accessed through `mongoose.Schema.Types.ObjectId`)

12. **Schema.Types.Mixed**: Represents a flexible/undefined schema type. (Can be accessed through `mongoose.Schema.Types.Mixed`)

These are the basic Schema Types provided by Mongoose. Additionally, you can also define custom schema types or use plugins to extend the functionality of your schemas. Each schema type has its own set of options and methods that can be used to define the behavior of the field in your MongoDB documents.

# Mongoose Models

In Mongoose, a model is a constructor function that provides an interface to MongoDB collections. Models define the structure of documents within a collection, enforce schema validation, and provide methods for querying and manipulating data in the database.

Here's how you can define and use a Mongoose model:

1. **Define a Schema**:
   Before creating a model, you define a schema that represents the structure of documents in the collection. You can specify the fields, their types, default values, validation rules, and more using the Mongoose Schema constructor.

   Example:

   ```javascript
   const mongoose = require("mongoose");

   const userSchema = new mongoose.Schema({
     name: { type: String, required: true },
     email: { type: String, required: true, unique: true },
     age: { type: Number, default: 0 },
   });

   // Compile the schema into a model
   const User = mongoose.model("User", userSchema);
   ```

2. **Create a Model**:
   Use the `mongoose.model()` method to compile the schema into a model. The first argument is the singular name of the collection (Mongoose will automatically pluralize it for the collection name), and the second argument is the schema object.

3. **Use the Model**:
   Once you have a model, you can use it to perform CRUD operations on the MongoDB collection it represents. Models provide methods like `find()`, `findOne()`, `create()`, `updateOne()`, `deleteOne()`, etc., for querying and manipulating data.

   Example:

   ```javascript
   // Create a new user document
   const newUser = new User({ name: "John", email: "john@example.com" });

   // Save the user document to the database
   newUser
     .save()
     .then((savedUser) => console.log("User saved:", savedUser))
     .catch((err) => console.error("Error saving user:", err));

   // Query users
   User.find({}) // Find all users
     .then((users) => console.log("All users:", users))
     .catch((err) => console.error("Error finding users:", err));
   ```

4. **Model Methods**:
   Mongoose models come with built-in static and instance methods that you can use for common database operations. Additionally, you can define custom methods on your model schema.

   Example:

   ```javascript
   userSchema.methods.getFullName = function () {
     return `${this.firstName} ${this.lastName}`;
   };

   const User = mongoose.model("User", userSchema);

   const user = new User({ firstName: "John", lastName: "Doe" });
   console.log(user.getFullName()); // Output: John Doe
   ```

In summary, Mongoose models are used to define the structure of documents in MongoDB collections and provide an interface for interacting with the database. They encapsulate data validation, query building, and other database operations, making it easier to work with MongoDB in Node.js applications.

# Connection using mongoose

`mongoose.connect()` and `mongoose.connection.close()` are two important methods in Mongoose for managing database connections. Let's discuss each of them:

1. **mongoose.connect()**:

   - This method is used to establish a connection to the MongoDB database. It takes a MongoDB connection string URI as an argument, along with optional configuration options.
   - Example:

     ```javascript
     const mongoose = require("mongoose");

     // MongoDB connection string
     const uri = "mongodb://localhost/mydatabase";

     // Connect to MongoDB
     mongoose
       .connect(uri, { useNewUrlParser: true, useUnifiedTopology: true })
       .then(() => console.log("Connected to MongoDB"))
       .catch((err) => console.error("Error connecting to MongoDB:", err));
     ```

   - Once the connection is established, Mongoose will manage the connection and allow you to perform CRUD operations on the database.

2. **mongoose.connection.close()**:

   - This method is used to close the connection to the MongoDB database. It releases resources associated with the connection and terminates the connection with the server.
   - Example:

     ```javascript
     const mongoose = require("mongoose");

     // Close the MongoDB connection
     mongoose.connection
       .close()
       .then(() => console.log("MongoDB connection closed"))
       .catch((err) => console.error("Error closing MongoDB connection:", err));
     ```

   - It's important to close the connection when it's no longer needed, especially in long-running applications or when shutting down the server, to prevent resource leaks and ensure proper handling of database connections.

In summary, `mongoose.connect()` is used to establish a connection to the MongoDB database, while `mongoose.connection.close()` is used to close the connection. These methods are essential for managing database connections in Mongoose-based applications.

# CRUD using mongoose

- **CRUD (Create, Read, Update, Delete)** operations using **Mongoose** with MongoDB.
- These operations allow you to work with data stored in MongoDB collections.

1. **Create (Insert) Operation**:

   - To add a new document to a collection, create an instance of your Mongoose model and save it.
   - Example:

     ```javascript
     const User = require("./models/User"); // Assuming you have a User model

     const newUser = new User({
       name: "Alice",
       age: 25,
       email: "alice@example.com",
     });

     newUser.save((err, savedUser) => {
       if (err) {
         console.error("Error saving user:", err);
       } else {
         console.log("User saved successfully:", savedUser);
       }
     });
     ```

2. **Read (Retrieve) Operation**:

   - To retrieve documents from a collection, use Mongoose queries.
   - Example:

     ```javascript
     // Find all users
     User.find({}, (err, users) => {
       if (err) {
         console.error("Error fetching users:", err);
       } else {
         console.log("All users:", users);
       }
     });

     // Find a user by email
     User.findOne({ email: "alice@example.com" }, (err, user) => {
       if (err) {
         console.error("Error fetching user:", err);
       } else {
         console.log("Found user:", user);
       }
     });
     ```

3. **Update Operation**:

   - To modify an existing document, find it first and then update its properties.
   - Example:
     ```javascript
     // Find a genre by id and update their name
     async function updateGenre(id, name) {
       try {
         await connectDB();
         const result = await Genre.findOneAndUpdate(
           { _id: id },
           {
             $set: { name: name },
           },
           { runValidators: true, new: true }
         );
         return result;
       } catch (ex) {
         if (ex.errors)
           for (field in ex.errors) console.error(ex.errors[field].message);
         else console.error(ex.message);
       }
     }
     ```

4. **Delete Operation**:
   - To remove a document, find it and then delete it.
   - Example:
     ```javascript
     // Find and delete a user by email
     User.findOneAndDelete(
       { email: "alice@example.com" },
       (err, deletedUser) => {
         if (err) {
           console.error("Error deleting user:", err);
         } else {
           console.log("Deleted user:", deletedUser);
         }
       }
     );
     ```

# Operators

You can find the complete list of all MongoDB query operators and update operators in the official MongoDB documentation. Here are the links to the relevant sections:

1. **Query and Projection Operators**:

   - MongoDB Query Operators: https://docs.mongodb.com/manual/reference/operator/query/
   - MongoDB Projection Operators: https://docs.mongodb.com/manual/reference/operator/projection/

2. **Update Operators**:
   - MongoDB Update Operators: https://docs.mongodb.com/manual/reference/operator/update/

These documentation pages provide comprehensive lists of all available query and update operators in MongoDB, along with detailed descriptions and examples of how each operator can be used. Additionally, the MongoDB documentation is regularly updated to reflect changes and additions to the MongoDB query language and features.

# Comparison Query Operators

1. **$eq**:

   - Description: Matches values that are equal to a specified value.
   - Example Query:
     ```javascript
     db.users.find({ age: { $eq: 30 } });
     ```
   - This query matches documents in the `users` collection where the `age` field is equal to 30.

2. **$gt**:

   - Description: Matches values that are greater than a specified value.
   - Example Query:
     ```javascript
     db.products.find({ price: { $gt: 100 } });
     ```
   - This query matches documents in the `products` collection where the `price` field is greater than 100.

3. **$gte**:

   - Description: Matches values that are greater than or equal to a specified value.
   - Example Query:
     ```javascript
     db.orders.find({ totalAmount: { $gte: 500 } });
     ```
   - This query matches documents in the `orders` collection where the `totalAmount` field is greater than or equal to 500.

4. **$in**:

   - Description: Matches any of the values specified in an array.
   - Example Query:
     ```javascript
     db.products.find({ category: { $in: ["Electronics", "Clothing"] } });
     ```
   - This query matches documents in the `products` collection where the `category` field contains either "Electronics" or "Clothing".

5. **$lt**:

   - Description: Matches values that are less than a specified value.
   - Example Query:
     ```javascript
     db.customers.find({ age: { $lt: 18 } });
     ```
   - This query matches documents in the `customers` collection where the `age` field is less than 18.

6. **$lte**:

   - Description: Matches values that are less than or equal to a specified value.
   - Example Query:
     ```javascript
     db.orders.find({ quantity: { $lte: 10 } });
     ```
   - This query matches documents in the `orders` collection where the `quantity` field is less than or equal to 10.

7. **$ne**:

   - Description: Matches all values that are not equal to a specified value.
   - Example Query:
     ```javascript
     db.products.find({ status: { $ne: "out_of_stock" } });
     ```
   - This query matches documents in the `products` collection where the `status` field is not equal to "out_of_stock".

8. **$nin**:
   - Description: Matches none of the values specified in an array.
   - Example Query:
     ```javascript
     db.customers.find({ city: { $nin: ["New York", "Los Angeles"] } });
     ```
   - This query matches documents in the `customers` collection where the `city` field does not contain "New York" or "Los Angeles".

These examples demonstrate how to use each comparison operator in MongoDB to construct queries that filter documents based on specific criteria.

# Logical Operators

1. **$and**:

```javascript
const result = await YourModel.find({
  $and: [{ age: { $gte: 18 } }, { age: { $lte: 30 } }],
});
```

This will find documents where the "age" field is greater than or equal to 18 and less than or equal to 30.

2. **$not**:

```javascript
const result = await YourModel.find({
  age: { $not: { $gt: 30 } },
});
```

This will find documents where the "age" field is not greater than 30.

3. **$nor**:

```javascript
const result = await YourModel.find({
  $nor: [{ age: { $gte: 18 } }, { age: { $lte: 30 } }],
});
```

This will find documents where the "age" field does not fall within the range of 18 to 30.

4. **$or**:

```javascript
const result = await YourModel.find({
  $or: [{ age: { $lt: 18 } }, { age: { $gt: 30 } }],
});
```

This will find documents where the "age" field is either less than 18 or greater than 30.

Replace `YourModel` with the actual Mongoose model you're working with.

# Element Query Operators

1. **$exists**:

```javascript
const result = await YourModel.find({
  fieldName: { $exists: true },
});
```

This will find documents where the field "fieldName" exists.

2. **$type**:

```javascript
const result = await YourModel.find({
  fieldName: { $type: "string" },
});
```

This will find documents where the field "fieldName" is of type string.

Replace `YourModel` with the actual Mongoose model you're working with, and `fieldName` with the actual field name you're querying against.

# Evaluation Query Operators

1. **$expr**:

```javascript
const result = await YourModel.find({
  $expr: { $gt: ["$field1", "$field2"] },
});
```

This will find documents where the value of "field1" is greater than the value of "field2".

2. **$jsonSchema**:

```javascript
const result = await YourModel.find({
  field: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name"],
      properties: {
        name: {
          bsonType: "string",
        },
      },
    },
  },
});
```

This will find documents where the "field" adheres to the specified JSON schema.

3. **$mod**:

```javascript
const result = await YourModel.find({
  field: { $mod: [5, 0] },
});
```

This will find documents where the value of "field" modulo 5 equals 0.

4. **$regex**:

```javascript
const result = await YourModel.find({
  field: { $regex: /pattern/ },
});
```

This will find documents where the value of "field" matches the specified regular expression pattern.

5. **$text**:

```javascript
const result = await YourModel.find({
  $text: { $search: "search term" },
});
```

This will find documents where the indexed text fields contain the specified search term.

6. **$where**:

```javascript
const result = await YourModel.find({
  $where: function () {
    return this.field1 > this.field2;
  },
});
```

This will find documents where the value of "field1" is greater than the value of "field2", evaluated using a JavaScript function.

Replace `YourModel` with the actual Mongoose model you're working with, and adjust the field names and conditions accordingly.

# Array Query Operators

1. **$all**:

```javascript
const result = await YourModel.find({
  arrayField: { $all: ["element1", "element2"] },
});
```

This will find documents where the array field "arrayField" contains all the specified elements "element1" and "element2".

2. **$elemMatch**:

```javascript
const result = await YourModel.find({
  arrayField: { $elemMatch: { field1: value1, field2: value2 } },
});
```

This will find documents where the array field "arrayField" contains at least one element that matches all the specified conditions.

3. **$size**:

```javascript
const result = await YourModel.find({
  arrayField: { $size: 3 },
});
```

This will find documents where the array field "arrayField" has a size of exactly 3 elements.

Replace `YourModel` with the actual Mongoose model you're working with, and adjust the field names and conditions accordingly.

# Field Update Operators

Here are examples for each of the field update operators:

1. **$currentDate**:
```javascript
const result = await YourModel.updateOne(
  { _id: documentId },
  { $currentDate: { lastModified: true } }
);
```
This will set the value of the "lastModified" field to the current date for the document with the specified ID.

2. **$inc**:
```javascript
const result = await YourModel.updateOne(
  { _id: documentId },
  { $inc: { quantity: 5 } }
);
```
This will increment the value of the "quantity" field by 5 for the document with the specified ID.

3. **$min**:
```javascript
const result = await YourModel.updateOne(
  { _id: documentId },
  { $min: { price: 10 } }
);
```
This will update the "price" field to 10 only if the existing value is greater than 10 for the document with the specified ID.

4. **$max**:
```javascript
const result = await YourModel.updateOne(
  { _id: documentId },
  { $max: { price: 20 } }
);
```
This will update the "price" field to 20 only if the existing value is less than 20 for the document with the specified ID.

5. **$mul**:
```javascript
const result = await YourModel.updateOne(
  { _id: documentId },
  { $mul: { quantity: 2 } }
);
```
This will multiply the value of the "quantity" field by 2 for the document with the specified ID.

6. **$rename**:
```javascript
const result = await YourModel.updateOne(
  { _id: documentId },
  { $rename: { "oldFieldName": "newFieldName" } }
);
```
This will rename the field "oldFieldName" to "newFieldName" for the document with the specified ID.

7. **$set**:
```javascript
const result = await YourModel.updateOne(
  { _id: documentId },
  { $set: { status: "active" } }
);
```
This will set the value of the "status" field to "active" for the document with the specified ID.

8. **$setOnInsert**:
```javascript
const result = await YourModel.updateOne(
  { _id: documentId },
  { $setOnInsert: { createdAt: new Date() } },
  { upsert: true }
);
```
This will set the value of the "createdAt" field to the current date if the update operation results in an insert of a document.

9. **$unset**:
```javascript
const result = await YourModel.updateOne(
  { _id: documentId },
  { $unset: { status: "" } }
);
```
This will remove the "status" field from the document with the specified ID.

Replace `YourModel` with the actual Mongoose model you're working with, `documentId` with the ID of the document you want to update, and adjust the field names and values accordingly.