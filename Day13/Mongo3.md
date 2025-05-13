#MongoDB/AggregateQuery 

Write a MongoDB query to compute each student’s overall average score (rounded 
to 2 decimals) from all exam score entries and sort the data by student_id.

Collection: 'stucollection'

Reference Document:
-------------------
```
{
    "student_id": "S1001",
    "name": "Alice Johnson",
    "age": 18,
    "contact": {
      "email": "alice@example.com",
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
      "contact": "robert.j@example.com"
    }
}
```

Sample Output:
---------------
```
[                                                                               
  {                                                                             
    student_id: 'S1001',                                                        
    name: 'Alice Johnson',                                                      
    averageScore: 77.5                                                          
  },                                                                            
  {                                                                             
    student_id: 'S1002',                                                        
    name: 'Bob Smith',                                                          
    averageScore: 57.5                                                          
  }
]
```
 
Query Reference:
----------------
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
  db.stucollection.aggregate([
    { $unwind: "$exams" },
    { $unwind: "$exams.scores" },
    { $group: {
      _id: "$student_id",
      name: { $first: "$name" },
      averageScore: { $avg: "$exams.scores.score" }
    }},
    { $sort: { _id: 1 } },
    { $project: {
      _id: 0,
      student_id: "$_id",
      name: "$name",
      averageScore: { $round: ["$averageScore", 2] }
    }}
  ])
)
```