#JS/WebSockets  #Web/Backend

Employee Management WebSocket Application

Objective:
-----------
Your task is to develop a WebSocket-based Employee Management System using Node. Js. 
This system will allow clients to:
  1. Insert Employee Records (`INSERT <name> <salary>`)
  2. Retrieve Employee List (`RETRIEVE`)
  3. Handle Invalid Commands (e.g., `INVALID` should return "Invalid command")

Your goal is to implement and test a WebSocket-based server and client, 
Ensuring that all operations work correctly.

Requirements:
-------------
Implement WebSocket Server
The server should:
 - Accept multiple client connections.
 - Process client messages and handle commands:
	1. `INSERT <name> <salary>` → Adds an employee to an in-memory array.
	2. `RETRIEVE` → Returns all stored employees.
	3. Any other command should return "Invalid command."
   
 - Maintain an in-memory array of employees (no database required).
 - Log each received command on the console.


Expected Behavior
-----------------
| Client Command     | Server Response                                                           |
| ------------------ | ------------------------------------------------------------------------- |
| INSERT Alice 50000 | "Employee inserted successfully."                                         |
| INSERT Bob 60000   | "Employee inserted successfully."                                         |
| RETRIEVE           | "ID: 1, Name: Alice, Salary: 50000" <br>"ID: 2, Name: Bob, Salary: 60000" |
| INVALID            | "Invalid command."                                                        |

Note:
---------------
 - The server should run on port 8080.
 - The system should allow multiple clients to connect.

## Solution:

```js
import { WebSocketServer } from "ws";

const server = new WebSocketServer({ port: 8080 });

let employees = [];
server.on("connection",(socket)=>{
    socket.on("connection",()=>{
        console.log("Connection established");
    });
    socket.on("message",(message)=>{
        let data = message.toString().split(" ");

        if(data[0] === "INSERT"){
            employees.push({ID : employees.length + 1, Name: data[1], Salary: data[2]});
            socket.send("Employee inserted successfully.");
        }
        else if(data[0] === "RETRIEVE"){
            employees.forEach((employee)=>{
                socket.send(`ID: ${employee.ID}, Name: ${employee.Name}, Salary: ${employee.Salary}`);
            })
        }
        else{
            socket.send("Invalid command.");
        }
    });
    socket.on("close",()=>{
        console.log("Connection closed");
    });
})
```