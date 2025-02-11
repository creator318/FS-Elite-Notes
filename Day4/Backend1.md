#JS #JS/Express #Web #Web/Backend

Order Management - using JSON Object
Objective: Implement the CRUD operations.

Requirements:
----
| Method | Endpoint    | Description          |
| ------ | ----------- | -------------------- |
| POST   | /orders     | Create a new order   |
| GET    | /orders     | Get all orders       |
| GET    | /orders/:id | Get a order by ID    |
| PUT    | /orders/:id | Update a order by ID |
| DELETE | /orders/:id | Delete a order by ID |

Reference JSON format for Object:
---------------------------------
```
{ 
	Id: 1,
	CustomerName: "Azar",
	TotalPrice: 1	50.0
}
```

NOTE: id value starts with 1, and increments by 1, for every new entry.

1. Implementation Requirements:

	Create a JSON Object (local)
	Implement proper error handling
	Add data validation

----------

2. API Response Format:
- **Method**: POST
	**Path**: `/orders`
	
	Response:
	- If successful: `res.status(201).send(order);` 
  
- **Method**: GET
	**Path**: `/orders`

	Response:
	- If successful: `res.status(200).send(orders);`
  
- **Method**: GET
	**Path**: /orders/:id

	*NOTE*: pass id value as URI params

	Response:
	- If successful: `res.status(200).send(order);`
	- If not found: `res.status(404).send();`
  
- **Method**: PUT
	**Path**: /orders/:id

	*NOTE*: pass id value as URI params

	Response:
	- If successful: `res.status(200).send(order);`
	- If not found: `res.status(404).send();`

- **Method**: DELETE
	**Path**: /orders/:id

	*NOTE*: pass id value as URI params

	Response:
    - If successful: `res.status(200).send();`

## Solution:

```js
import e from "express";
import cors from "cors";

const app = e();
const orders = [];

app.use(e.json());
app.use(cors());

app.post("/orders", (req, res) => {
  try {
    const order = req.body;
    order.id = orders.length + 1;
    orders.push(order);
    res.status(201).send(order);
  } catch (e) {
    res.status(400).send(e);
  }
});

app.get("/orders", (req, res) => {
  res.status(200).send(orders);
});

app.get("/orders/:id", (req, res) => {
  const id = req.params.id;
  const order = orders.find((order) => order.id === parseInt(id));
  if (!order) return res.status(404).send("Order not found");
  res.status(200).send(order);
});

app.put("/orders/:id", (req, res) => {
  const id = req.params.id;
  const order = orders.find((order) => order.id === parseInt(id));
  if (!order) return res.status(404).send("Order not found");
  try {
    const newOrder = req.body;
    orders[id - 1] = newOrder;
    res.status(200).send(newOrder);
  } catch (e) {
    res.status(400).send(e);
  }
});

app.delete("/orders/:id", (req, res) => {
  const id = req.params.id;
  const order = orders.find((order) => order.id === parseInt(id));
  if (!order) return res.send("Order not found");
  orders.splice(id - 1, 1);
  res.status(200).send("Deleted successfully");
});

app.listen(5000, () => {
  console.log("Server started on port 5000");
});
```