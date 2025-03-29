#JS/WebSockets #Web/Backend #Mongodb 

Real-Time Group Chat using WebSockets:
----------------------------------
You are required to build a real-time group chat application using WebSockets, where multiple users (clients) can join a chatroom and exchange messages in real-time. The application must consist of a WebSocket server and a browser-based client. All messages sent by any client must be visible to all other connected clients, including the sender — similar to a WhatsApp group.

Functional Requirements:
=========================
1. WebSocket Server (Node.js):

You must implement a WebSocket server with the following behavior:
- Accept connections from multiple clients.
- Maintain a list of all connected clients.
- When a message is received from any client, broadcast that message to all connected clients.
- Handle client disconnections and remove them from the active list.
- The server should run on `ws://<ip-address>: 8080`

2. Web-based Client (HTML + JavaScript):

You must create a basic client interface with the following requirements:
 - Connect to the WebSocket server at `ws://<ip-address>:8080`.
 - The page should have:
	-  A `<div>` with id="chat" that shows all chat messages.
	- An `<input>` box with id "msg" to type the message.
	- A `<button>` that, when clicked, sends the message.
- When a message is received from the server:
	- It must be displayed as a new paragraph `<p>` inside the `#chat` area.
- When the user sends a message:
	- It should be sent to the server using WebSocket.
	- The input box should be cleared after sending.

## Solution:

index.js
```js
import { WebSocketServer } from "ws";

const server = new WebSocketServer({ port: 8080 });

let users = new Set();

server.on('connection', (socket) => {
    users.add(socket);
    console.log(`User connected: ${socket}`)
    
    socket.on('message', (message) => {
        console.log(message.toString());
        users.forEach((user) => user.send(message.toString())
    })

    socket.on('close', () => users.delete(socket))
})
```

index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WebSocket Group Chat</title>
  <script>
    const socket = new WebSocket("ws://<ip-address>:8080");
        
    socket.onmessage = (e) => {
      const msg = document.createElement('p');
      msg.textContent = e.data.toString();

      document.getElementById('chat').append(msg);
    }

    function sendMessage() {
      const msg = document.getElementById('msg').value;
      if (msg) socket.send(msg);
      document.getElementById('msg').value = '';
    }
  </script>
</head>
<body>
  <div id="chat"></div>
  <input type="text" id="msg"/>
  <button onclick="sendMessage()">Send</button>
</body>
</html>
```