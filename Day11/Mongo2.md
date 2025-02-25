#Mongodb/Query 

Write a MongoDB query to find students whose guardian is a "Father"
And display student_id, name only.

Collection: 'stucollection'

Reference Document:
----------------------
```
{
    "student_id": "S 1001",
    "name": "Alice Johnson",
    "age": 18,
    "contact": {
      "email": " alice@example.com ",
      "phone": "123-456-7890",
      "address": {
        "street": "123 Maple St",
        "city": "New York",
        "state": "NY",
        "zip": "10001"
      }
    },
    "enrolled_courses": ["Java", "Python"],
    "exams": [
      {
        "subject": "Java",
        "scores": [
          { "type": "quiz", "score": 80 },
          { "type": "midterm", "score": 75 },
          { "type": "final", "score": 90 }
        ],
        "passed": true
      },
      {
        "subject": "Python",
        "scores": [
          { "type": "quiz", "score": 70 },
          { "type": "midterm", "score": 65 },
          { "type": "final", "score": 85 }
        ],
        "passed": true
      }
    ],
    "skills": ["Java", "Spring Boot"],
    "guardian": {
      "name": "Robert Johnson",
      "relation": "Father",
      "contact": " robert.j@example.com "
    }
}
```

Sample Output:
--------------
```
[                                                                               
  {                                                                             
    Student_id: 'S 1001',                                                        
    Name: 'Alice Johnson'                                                       
  },                                                                            
  {                                                                             
    Student_id: 'S 1003',                                                        
    Name: 'Charlie Williams'                                                    
  }
]
```

Query Reference:
-------------------
```
printjson() : JS library function to display the JSON Object data.
In db.<collection>.find():
	db -> Refers to the database.
	<collection> -> Your collection name
	find() -> Method to write the selection and projection part of the query.
```

## Solution: 

```js
printjson(
	db.stucollection.find({"guardian.relation": "Father"}, {student_id: 1, name: 1, _id: 0})
)
```