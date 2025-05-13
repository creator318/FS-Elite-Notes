#MongoDB/AggregateQuery 

Write a MongoDB aggregation query to determine how frequently each medicine is prescribed. List medicines in descending order of their usage frequency.


## Hospital and Research Management MongoDB Schema

1. Staff Collection
	```json
	{
		_id: ObjectId,
		staffId: String, // Unique ID (e.g., D 001, N 001)
		Name: String,
		ContactInfo: { Phone: String, Email: String },
		JoiningDate: Date,
		Department: [String],
		Role: { enum: ["Doctor", "Nurse", "Technician", "Pharmacist", "Admin"] },
		Specializations: [String], // for Doctors
		ConsultationHours: [{ day: String, start: String, end: String }], // for Doctors
		AssignedWardICU: String, // for Nurses
		SupervisorId: { type: String, ref: 'staff', default: null }
	}
	```
	
2. Patients Collection
	```json
	{
		_id: ObjectId,
		PatientId: String,
		Name: String,
		Age: Number,
		Gender: { enum: ["Male", "Female", "Other"] },
		Address: String,
		Contact: String,
		PatientType: { enum: ["Inpatient", "Outpatient"] },
		Insurance: { Company: String, PolicyNumber: String, 
								 Coverage: { enum: ["Full", "Partial", "None"] } }
	}
	```
	
3. Medicines Collection
	```json
	{
		_id: ObjectId,
		MedicineId: String,
		Name: String,
		DosageForm: String,
		StockLevel: Number,
		ReorderThreshold: Number,
		SupplierName: String
	}
	```
	
4. Appointments Collection
	```json
	{
		_id: ObjectId,
		AppointmentId: String,
		PatientId: { type: String, ref: 'patients' },
		DoctorId: { type: String, ref: 'staff' },
		AppointmentDate: Date,
		AppointmentTime: String,
		Status: { enum: ["Booked", "Completed", "Canceled"] }
	}
	```
	
5. Prescriptions Collection
	```json
	{
		_id: ObjectId,
		PrescriptionId: String,
		AppointmentId: { type: String, ref: 'appointments' },
		PatientId: { type: String, ref: 'patients' },
		DoctorId: { type: String, ref: 'staff' },
		Medicines: [{ MedicineId: {type: String, ref: 'medicines'},
									Name: String, Dosage: String, Duration: String }],
		Date: Date
	}
	```

6. Lab Tests Collection
	```json
	{
		_id: ObjectId,
		TestId: String,
		Type: String,
		PatientId: { type: String, ref: 'patients' },
		TechnicianId: { type: String, ref: 'staff' },
		Description: String,
		Cost: Number,
		Date: Date
	}
	```

7. Billing Collection
	```json
	{
		_id: ObjectId,
		BillId: String,
		PatientId: { type: String, ref: 'patients' },
		Items: [{ Description: String, Amount: Number }],
		TotalAmount: Number,
		InsuranceCovered: Boolean,
		InsuranceDetails: { Company: String, PolicyNumber: String,
												Coverage: { enum: ["Full", "Partial", "None"] } },
		BillingDate: Date
	}
	```

Sample output:
--------------
```
[
  { prescriptionCount: 3, medicineName: 'Atorvastatin' },
  { prescriptionCount: 3, medicineName: 'Hydrocortisone' },
  { prescriptionCount: 3, medicineName: 'Iohexol' }
]
```

Query Reference:
-------------------
```
printjson() : JS library function to display the JSON Object data.
    In db.<collection>.find()/aggregate():
    	db -> Refers to the database.
    	<collection> -> Your collection name
```

## Solution:

```js
printjson(
  db.prescriptions.aggregate([
    {
      $unwind: "$medicines"
    },
    {
      $group: {
        _id: "$medicines.name",
        prescriptionCount: { $sum: 1 }
      } 
    },
    {
      $project: {
        _id: 0,
        prescriptionCount: 1,
        medicineName: "$_id"
      }
    },
    {
      $sort: { prescriptionCount: -1 }
    }
  ])
)
```
