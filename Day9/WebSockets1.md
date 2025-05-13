#JS/WebSockets #Web/Backend #MongoDB 

Employee Management WebSocket Application with MongoDB

Objective:
----------
Your task is to develop a WebSocket-based Employee Management System using Node. Js and MongoDB. The system should allow multiple clients to interact with a database to perform the following operations:
1. Insert Employee Records (`INSERT <name> <salary> <role> <department> <experience>`)
2. Retrieve Employee List (`RETRIEVE`)
3. Retrieve Employee List who belongs to a department (`RETRIEVE_BY_DEPT <department>`)
	
The WebSocket server should be capable of handling multiple concurrent clients and persist employee data in MongoDB.

MongoDB Employee Schema
---------------
```
const employeeSchema = new mongoose.Schema ({
    name: String,
    salary: Number,
    role: String,
    department: String,
    experience: Number
});
```

Requirements:
-------------
Implement WebSocket Server
The server should:
 - Accept multiple client connections. (give a response as "Connected" )
 - Process incoming commands from clients as discussed above.
 - Log each received command on the console.
 - Ensure proper error handling (e.g., invalid salary, missing name, etc.).
		
Expected Behaviour:
-----------------

| Client Command                    | Server Response                                                                                                                                                                     |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| INSERT Alice 50000 Developer IT 5 | "Employee inserted successfully."                                                                                                                                                   |
| INSERT Bob 60000 Manager IT 5     | "Employee inserted successfully."                                                                                                                                                   |
| RETRIEVE                          | "ID: 1, Name: Alice, Salary: 50000, Role: Developer, Department: IT, Experience: 5 years" <br>"ID: 2, Name: Bob, Salary: 60000, Role: Manager, Department: IT, Experience: 5 years" |
| RETRIEVE_BY_DEPT IT               | "ID: 1, Name: Alice, Salary: 50000, Role: Developer, Department: IT, Experience: 5 years"<br>"ID: 2, Name: Bob, Salary: 60000, Role: Manager, Department: IT, Experience: 5 years"  |
| INVALID                           | "Invalid command."                                                                                                                                                                  | 

Note: 
---------------
 - Your implementation must use MongoDB for data persistence.
 - The server should run on port 8080.
 - The system should allow multiple clients to connect.

## Solution:

```js
import { WebSocketServer } from "ws";
import { connect, model, Schema, connections } from "mongoose";
import initSequence from "mongoose-sequence";

connect("mongodb://localhost:27017/fs")
  .then(() => console.log("Connected to MongoDB\n"))
  .catch(() => console.err("Failed connecting to MongoDB\n"));

const sequence = initSequence(connections[0]);
const employeeSchema = new Schema({
	_id: Number,
	name: String,
	salary: Number,
	role: String,
	department: String,
	experience: Number
}, { _id: false });

employeeSchema.plugin(sequence, { inc_field: "_id" });
const Employee = model("Employee", employeeSchema);

const server = new WebSocketServer({ port: 5000 });

server.on("connection", (socket) => {
  console.log("Connection established");

  socket.on("message", async (message) => {
    const data = message.toString().split(" ");
    if (data[0] === "INSERT" && data.length == 6) {
      const emp = new Employee({
        name: data[1],
        salary: data[2],
        role: data[3],
        department: data[4],
        experience: data[5]
      });
      await emp.save();
      socket.send(`Employee inserted successfully.ID: ${emp._id}`);
    } else if (data[0] === "RETRIEVE" && data.length == 1) {
      const employees = await Employee.find();
      employees.forEach((element) =>
        socket.send(
          `ID: ${element._id}, Name: ${element.name}, Salary: ${element.salary}, Role: ${element.role}, Department: ${element.department}, Experience: ${element.experience}`
        )
      );
    } else if (data[0] === "RETRIEVE_BY_DEPT" && data.length == 2) {
      const employees = await Employee.find({ department: data[1] });
      employees.forEach((element) =>
        socket.send(
          `ID: ${element._id}, Name: ${element.name}, Salary: ${element.salary}, Role: ${element.role}, Department: ${element.department}, Experience: ${element.experience}`
        )
      );
    } else socket.send("Invalid command.");
  });

  socket.on("close", () => console.log("Connection closed"));
});

```