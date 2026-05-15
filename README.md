# dbms

-- =====================================================
-- PROBLEM STATEMENT 8 (TRIGGERS)
-- =====================================================

-- =====================================================
-- CREATE EMPLOYEE TABLE
-- =====================================================

CREATE TABLE Employee (
    emp_id NUMBER PRIMARY KEY,
    dept_id NUMBER,
    emp_name VARCHAR2(50),
    DoJ DATE,
    salary NUMBER,
    commission NUMBER,
    job_title VARCHAR2(50)
);

-- =====================================================
-- INSERT DATA
-- =====================================================

INSERT INTO Employee VALUES
(101,1,'Rahul',
TO_DATE('10-05-2015','DD-MM-YYYY'),
25000,NULL,'Manager');

INSERT INTO Employee VALUES
(102,2,'Amit',
TO_DATE('15-06-2018','DD-MM-YYYY'),
18000,NULL,'Developer');

COMMIT;

-- =====================================================
-- 1. TRIGGER TO PREVENT SALARY DECREASE
-- =====================================================

CREATE OR REPLACE TRIGGER trg_salary_check
BEFORE UPDATE OF salary
ON Employee
FOR EACH ROW

BEGIN

    IF :NEW.salary < :OLD.salary THEN
        RAISE_APPLICATION_ERROR(
        -20001,
        'Salary Cannot Be Decreased');
    END IF;

END;
/

-- =====================================================
-- 2. CREATE JOB_HISTORY TABLE
-- =====================================================

CREATE TABLE job_history (
    emp_id NUMBER,
    old_job_title VARCHAR2(50),
    old_department_id NUMBER,
    start_date DATE,
    end_date DATE
);

-- =====================================================
-- TRIGGER FOR JOB TITLE CHANGE
-- =====================================================

CREATE OR REPLACE TRIGGER trg_job_history
AFTER UPDATE OF job_title
ON Employee
FOR EACH ROW

BEGIN

    INSERT INTO job_history
    VALUES(
        :OLD.emp_id,
        :OLD.job_title,
        :OLD.dept_id,
        :OLD.DoJ,
        SYSDATE
    );

END;
/

-- =====================================================
-- TEST TRIGGER
-- =====================================================

UPDATE Employee
SET job_title = 'Senior Manager'
WHERE emp_id = 101;

--------------------------------------------------------
-- DISPLAY JOB HISTORY
--------------------------------------------------------

SELECT * FROM job_history;

Problem Statement 9 (CRUD Using MongoDB)
Create a collection Social_Media having fields as User_Id, User_Name, No_of_Posts, No_of_Friends, Friends_List, Interests. (Hint: Friends_List and Interests can be of array type) Insert 20 documents in the collection Social_Media. Write queries for following.
1.	List all the users from collection Social_Media in formatted manner.
2.	Find all users having number of posts greater than 100.
3.	List the user names and their respective Friens_List
4.	Display the user ids and Friends list of users who have more than 5 friends.
5.	Display all users with no of posts in descending order.
// =====================================================
// PROBLEM STATEMENT 9 (CRUD USING MONGODB)
// =====================================================

// =====================================================
// CREATE COLLECTION
// =====================================================

db.createCollection("Social_Media")

// =====================================================
// INSERT 20 DOCUMENTS
// =====================================================

db.Social_Media.insertMany([

{User_Id:1, User_Name:"Rahul", No_of_Posts:120, No_of_Friends:6,
Friends_List:["Amit","Sneha","Priya","Neha","Rohan","Kiran"],
Interests:["Music","Cricket"]},

{User_Id:2, User_Name:"Amit", No_of_Posts:80, No_of_Friends:4,
Friends_List:["Rahul","Priya","Rohan","Neha"],
Interests:["Movies","Football"]},

{User_Id:3, User_Name:"Sneha", No_of_Posts:150, No_of_Friends:7,
Friends_List:["Rahul","Amit","Priya","Neha","Kiran","Pooja","Riya"],
Interests:["Dance","Music"]},

{User_Id:4, User_Name:"Priya", No_of_Posts:60, No_of_Friends:3,
Friends_List:["Rahul","Sneha","Amit"],
Interests:["Reading","Travel"]},

{User_Id:5, User_Name:"Neha", No_of_Posts:200, No_of_Friends:8,
Friends_List:["Rahul","Amit","Sneha","Priya","Rohan","Kiran","Pooja","Riya"],
Interests:["Photography","Music"]},

{User_Id:6, User_Name:"Rohan", No_of_Posts:95, No_of_Friends:5,
Friends_List:["Rahul","Amit","Sneha","Neha","Priya"],
Interests:["Gaming","Cricket"]},

{User_Id:7, User_Name:"Kiran", No_of_Posts:130, No_of_Friends:6,
Friends_List:["Rahul","Sneha","Neha","Pooja","Riya","Rohan"],
Interests:["Travel","Music"]},

{User_Id:8, User_Name:"Pooja", No_of_Posts:40, No_of_Friends:2,
Friends_List:["Neha","Kiran"],
Interests:["Cooking","Dance"]},

{User_Id:9, User_Name:"Riya", No_of_Posts:175, No_of_Friends:7,
Friends_List:["Sneha","Neha","Kiran","Rahul","Amit","Priya","Rohan"],
Interests:["Art","Music"]},

{User_Id:10, User_Name:"Akash", No_of_Posts:55, No_of_Friends:3,
Friends_List:["Rahul","Rohan","Amit"],
Interests:["Football","Travel"]},

{User_Id:11, User_Name:"Vikas", No_of_Posts:110, No_of_Friends:6,
Friends_List:["Rahul","Sneha","Pooja","Neha","Amit","Rohan"],
Interests:["Gaming","Music"]},

{User_Id:12, User_Name:"Sonal", No_of_Posts:70, No_of_Friends:4,
Friends_List:["Priya","Neha","Pooja","Riya"],
Interests:["Dance","Cooking"]},

{User_Id:13, User_Name:"Anjali", No_of_Posts:145, No_of_Friends:7,
Friends_List:["Rahul","Sneha","Neha","Kiran","Rohan","Riya","Amit"],
Interests:["Travel","Photography"]},

{User_Id:14, User_Name:"Ramesh", No_of_Posts:90, No_of_Friends:5,
Friends_List:["Akash","Vikas","Rahul","Amit","Sneha"],
Interests:["Cricket","Movies"]},

{User_Id:15, User_Name:"Suresh", No_of_Posts:210, No_of_Friends:9,
Friends_List:["Rahul","Sneha","Neha","Priya","Rohan","Kiran","Pooja","Riya","Amit"],
Interests:["Music","Travel"]},

{User_Id:16, User_Name:"Meena", No_of_Posts:35, No_of_Friends:2,
Friends_List:["Pooja","Riya"],
Interests:["Cooking","Art"]},

{User_Id:17, User_Name:"Komal", No_of_Posts:125, No_of_Friends:6,
Friends_List:["Rahul","Sneha","Amit","Priya","Neha","Rohan"],
Interests:["Dance","Music"]},

{User_Id:18, User_Name:"Deepak", No_of_Posts:65, No_of_Friends:3,
Friends_List:["Akash","Vikas","Ramesh"],
Interests:["Gaming","Football"]},

{User_Id:19, User_Name:"Nikita", No_of_Posts:160, No_of_Friends:7,
Friends_List:["Rahul","Sneha","Neha","Kiran","Rohan","Riya","Pooja"],
Interests:["Photography","Travel"]},

{User_Id:20, User_Name:"Arjun", No_of_Posts:50, No_of_Friends:4,
Friends_List:["Rahul","Amit","Rohan","Priya"],
Interests:["Cricket","Movies"]}

])

// =====================================================
// 1. LIST ALL USERS IN FORMATTED MANNER
// =====================================================

db.Social_Media.find().pretty()

// =====================================================
// 2. USERS HAVING POSTS GREATER THAN 100
// =====================================================

db.Social_Media.find(
   {No_of_Posts: {$gt:100}}
)

// =====================================================
// 3. DISPLAY USER NAMES AND FRIENDS LIST
// =====================================================

db.Social_Media.find(
   {},
   {User_Name:1, Friends_List:1, _id:0}
)

// =====================================================
// 4. DISPLAY USER ID AND FRIENDS LIST
//    HAVING MORE THAN 5 FRIENDS
// =====================================================

db.Social_Media.find(
   {No_of_Friends: {$gt:5}},
   {User_Id:1, Friends_List:1, _id:0}
)

// =====================================================
// 5. DISPLAY USERS WITH POSTS
//    IN DESCENDING ORDER
// =====================================================

db.Social_Media.find().sort({No_of_Posts:-1})
Problem Statement 10 (Aggregation & Indexing)
Create the Collection Movies_Data( Movie_ID, Movie_Name, Director, Genre, BoxOfficeCollection) and solve the following:
1.	Display a list stating how many Movies are directed by each “Director”.
2.	Display list of Movies with the highest BoxOfficeCollection in each Genre.
3.	Display list of Movies with the highest BoxOfficeCollection in each Genre in ascending order of BoxOfficeCollection.
4.	Create an index on field Movie_ID.
5.	Create an index on fields ” Movie_Name” and ” Director”.
6.	Drop an index on field Movie_ID.
7.	Drop an index on fields ” Movie_Name” and ” Director”.
// =====================================================
// PROBLEM STATEMENT 10 (AGGREGATION & INDEXING)
// =====================================================

// =====================================================
// CREATE COLLECTION
// =====================================================

db.createCollection("Movies_Data")

// =====================================================
// INSERT DOCUMENTS
// =====================================================

db.Movies_Data.insertMany([

{Movie_ID:1, Movie_Name:"KGF", Director:"Prashanth Neel",
Genre:"Action", BoxOfficeCollection:250},

{Movie_ID:2, Movie_Name:"3 Idiots", Director:"Rajkumar Hirani",
Genre:"Comedy", BoxOfficeCollection:400},

{Movie_ID:3, Movie_Name:"Dangal", Director:"Nitesh Tiwari",
Genre:"Sports", BoxOfficeCollection:500},

{Movie_ID:4, Movie_Name:"Bahubali", Director:"S. S. Rajamouli",
Genre:"Action", BoxOfficeCollection:650},

{Movie_ID:5, Movie_Name:"PK", Director:"Rajkumar Hirani",
Genre:"Comedy", BoxOfficeCollection:350},

{Movie_ID:6, Movie_Name:"Chak De India", Director:"Shimit Amin",
Genre:"Sports", BoxOfficeCollection:200},

{Movie_ID:7, Movie_Name:"Pushpa", Director:"Sukumar",
Genre:"Action", BoxOfficeCollection:450},

{Movie_ID:8, Movie_Name:"Zindagi Na Milegi Dobara",
Director:"Zoya Akhtar",
Genre:"Drama", BoxOfficeCollection:300}

])

// =====================================================
// 1. NUMBER OF MOVIES DIRECTED BY EACH DIRECTOR
// =====================================================

db.Movies_Data.aggregate([

{
   $group:
   {
      _id:"$Director",
      Total_Movies: {$sum:1}
   }
}

])

// =====================================================
// 2. MOVIES WITH HIGHEST BOX OFFICE COLLECTION
//    IN EACH GENRE
// =====================================================

db.Movies_Data.aggregate([

{
   $sort:{BoxOfficeCollection:-1}
},

{
   $group:
   {
      _id:"$Genre",
      Movie_Name: {$first:"$Movie_Name"},
      Highest_Collection: {$first:"$BoxOfficeCollection"}
   }
}

])

// =====================================================
// 3. HIGHEST COLLECTION MOVIES IN EACH GENRE
//    IN ASCENDING ORDER OF COLLECTION
// =====================================================

db.Movies_Data.aggregate([

{
   $sort:{BoxOfficeCollection:-1}
},

{
   $group:
   {
      _id:"$Genre",
      Movie_Name: {$first:"$Movie_Name"},
      Highest_Collection: {$first:"$BoxOfficeCollection"}
   }
},

{
   $sort:{Highest_Collection:1}
}

])

// =====================================================
// 4. CREATE INDEX ON Movie_ID
// =====================================================

db.Movies_Data.createIndex({Movie_ID:1})

// =====================================================
// 5. CREATE INDEX ON Movie_Name AND Director
// =====================================================

db.Movies_Data.createIndex(
   {Movie_Name:1, Director:1}
)

// =====================================================
// 6. DROP INDEX ON Movie_ID
// =====================================================

db.Movies_Data.dropIndex({Movie_ID:1})

// =====================================================
// 7. DROP INDEX ON Movie_Name AND Director
// =====================================================

db.Movies_Data.dropIndex(
   {Movie_Name:1, Director:1}
)
