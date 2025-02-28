#Mongodb/AggregateQuery 

Write a MongoDB query to calculate the average score from the "final" exam 
for each subject across all students, print the output in the sorted order 
of the subject names.

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
    subject: 'C',                                                               
    averageFinalScore: 75                                                       
  },                                                                            
  {                                                                             
    subject: 'C++',                                                             
    averageFinalScore: 68.75                                                    
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
	  { $match: { "exams.scores.type": "final" } },
	  { $group: {
	    _id: "$exams.subject",
	    averageFinalScore: { $avg: "$exams.scores.score" },
	  }},
	  { $sort: { _id: 1 } },
	  { $project: {
	    _id: 0,
	    subject: "$_id",
	    averageFinalScore: { $round: ["$averageFinalScore", 2] },
	  }}
	])
)

```