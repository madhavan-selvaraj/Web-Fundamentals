/*=========================================================
                    QUESTIONS
=========================================================*/

---------------------------------------------------------
-- QUESTION 1
---------------------------------------------------------
-- Create the employees collection and insert sample documents.
-- Hint: Use insertMany().

db.employees.insertMany([
  {
    employeeId: 2,
    employeeName: "Priya",
    department: "HR",
    salary: 60000,
    joiningDate: ISODate("2021-03-15")
  },
  {
    employeeId: 3,
    employeeName: "Arun",
    department: "IT",
    salary: 95000,
    joiningDate: ISODate("2020-07-20")
  },
  {
    employeeId: 4,
    employeeName: "Meena",
    department: "Finance",
    salary: 75000,
    joiningDate: ISODate("2023-02-12")
  },
  {
    employeeId: 5,
    employeeName: "Rahul",
    department: "Sales",
    salary: 55000,
    joiningDate: ISODate("2022-09-05")
  },
  {
    employeeId: 6,
    employeeName: "Kavya",
    department: "IT",
    salary: 90000,
    joiningDate: ISODate("2021-11-18")
  },
  {
    employeeId: 7,
    employeeName: "Vijay",
    department: "HR",
    salary: 65000,
    joiningDate: ISODate("2020-05-25")
  },
  {
    employeeId: 8,
    employeeName: "Divya",
    department: "Finance",
    salary: 85000,
    joiningDate: ISODate("2019-08-14")
  },
  {
    employeeId: 9,
    employeeName: "Karthik",
    department: "Sales",
    salary: 70000,
    joiningDate: ISODate("2023-06-01")
  },
  {
    employeeId: 10,
    employeeName: "Anjali",
    department: "IT",
    salary: 95000,
    joiningDate: ISODate("2022-04-22")
  }
])

---------------------------------------------------------
-- QUESTION 2
---------------------------------------------------------
-- Add an email field to employeeId = 1.
-- Hint: Use updateOne() with $set.

db.employees.updateOne(
  {
    employeeId:1
  },
  {
    $set:{email:'john.it@gmail.com'}
  }
)

---------------------------------------------------------
-- QUESTION 3
---------------------------------------------------------
-- Increase salary of all IT employees by 10%.
-- Hint: Use updateMany() and $mul.

db.employees.updateMany(
  {
    department:"IT"
  },
  [
    {
      $set:{
        salary:{
          $round:[
            {
              $multiply:["$salary",1.10]
            },
            0
          ]
        }
      }
    }
  ]
)

---------------------------------------------------------
-- QUESTION 4
---------------------------------------------------------
-- Find employees earning more than 75,000.
-- Hint: Use $gt.

db.employees.find(
  {
    salary:{$gt:75000}
  },
  {
    _id:0,employeeName:1,salary:1
  }
)

---------------------------------------------------------
-- QUESTION 5
---------------------------------------------------------
-- Find employees whose department is IT.
-- Hint: Use find() with equality filter.

db.employees.find(
  {
    department:"IT"
  },
  {_id:0,employeeName:1,department:1}
)
---------------------------------------------------------
-- QUESTION 6
---------------------------------------------------------
-- Find employees sorted by salary descending.
-- Hint: Use sort().

db.employees.find({},{_id:0,employeeName:1,salary:1}).sort({salary:-1})
---------------------------------------------------------
-- QUESTION 7
---------------------------------------------------------
-- Find top 2 highest-paid employees.
-- Hint: Use sort() and limit().

db.employees.find({},{_id:0,employeeName:1,salary:1}).sort({salary:-1}).limit(2)
---------------------------------------------------------
-- QUESTION 8
---------------------------------------------------------
-- Count the number of employees in each department.
-- Hint: Use aggregation with $group.

db.employees.aggregate(
  [
    {
      $group:{
        _id:"$department",
        totalEmployees:{$sum:1}
      }
    }
  ]
)
---------------------------------------------------------
-- QUESTION 9
---------------------------------------------------------
-- Find the average salary for each department.
-- Hint: Use $avg inside $group.

db.employees.aggregate(
  [
    {
      $group:{
        _id:"$department",
        averageSalary:{$avg:"$salary"}
      }
    }
  ]
)

---------------------------------------------------------
-- QUESTION 10
---------------------------------------------------------
-- Find the department with the highest average salary.
-- Hint: Use $group and sort descending.

db.employees.aggregate(
  [
    {
      $group:{
        _id:"$department",
        averageSalary:{$avg:"$salary"}
      }
    },
    {
      $sort:{
        averageSalary:-1
      }
    }
  ]
)

---------------------------------------------------------
-- QUESTION 11
---------------------------------------------------------
-- Find customers whose total purchase amount exceeds 300.
-- Hint: Group orders by customerName and use $sum.

db.orders.aggregate(
  [
    {
      $group:{
        _id:"$customerName",
        totalAmount:{$sum:"$amount"}
      }
    },
    {
      $match:{
        totalAmount:{$gt:300}
      }
    }
    
    
  ]
)
---------------------------------------------------------
-- QUESTION 12
---------------------------------------------------------
-- Find duplicate customer names.
-- Hint: Use $group and count documents.

db.customers.aggregate(
  [
    {
      $group:{
        _id:"$customerName",
        count:{$sum:1}
      }
    },
    {
      $match:{
        count:{$gt:1}
      }
    }
  ]
)

---------------------------------------------------------
-- QUESTION 13
---------------------------------------------------------
-- Find the second highest salary among employees.
-- Hint: Sort descending and skip one document.

db.employees.find({},{_id:0,employeeName:1,salary:1}).sort({salary:-1}).limit(1).skip(1)
---------------------------------------------------------
-- QUESTION 14
---------------------------------------------------------
-- Find employees who joined before 2022.
-- Hint: Use $lt with ISODate().

db.employees.find(
  {
    joiningDate:{$lt:ISODate("2022-01-01")}
  }
)
---------------------------------------------------------
-- QUESTION 15
---------------------------------------------------------
-- Find the total salary expenditure of the company.
-- Hint: Use aggregation with $sum.

db.employees.aggregate(
  [
    {
      $group:{
        _id:null,
        Total:{$sum:"$salary"}
      }
    }
  ]
)
