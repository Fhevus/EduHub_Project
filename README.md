**MongoDB Project for an Edu Startup**

A MongoDB-based analytics project for managing and analyzing online education platform data using PyMongo.

**Project Setup Instructions**

1. Install MongoDB
Download and install MongoDB Community Edition from
https://www.mongodb.com/try/download/community

2. Set Up the Database

	Open MongoDB Compass or connect via Mongo shell


	Create a database named: eduhub_db


	Import the collections: users, courses, enrollments, lessons, assignments, submissions, etc.


	You can import sample data using Compass or mongo import command.


4. Set Up Your Python Environment

	Create a virtual environment (optional but recommended):
	
	
	python -m venv venv source venv/bin/activate # On Windows: venv\Scripts\activate
	
	
	Install dependencies:
	
	
	pip install pymongo
	

4. Open and Run Code in VSCode or Jupyter Notebook

	Open eduhub_queries.py (or your Jupyter notebook version)
	
	
	Make sure your MongoDB server is running
	
	
	Run each section to:
	
	
	Create indexes
	
	
	Perform aggregations
	
	
	Handle errors


	Analyze performance



5. Use Jupyter Notebook
	Launch Jupyter in VSCode:


***Database Schema Documentation***


	#CREATE USER SCHEMA
	
	#Preselected values

roles = ["student", "instructor"]

bio = [ "Passionate about lifelong learning and sharing knowledge.", "Loves# to solve problems through technology and data.", "Driven by curiosity and a desire to build meaningful things.", "Helping students grow into world-class developers.", "Committed to creating impactful learning experiences." ]

avatar = [ "https://api.dicebear.com/6.x/thumbs/svg?seed=Alpha", "https://api.dicebear.com/6.x/thumbs/svg?seed=Beta", "https://api.dicebear.com/6.x/thumbs/svg?seed=Gamma", "https://api.dicebear.com/6.x/thumbs/svg?seed=Delta", "https://api.dicebear.com/6.x/thumbs/svg?seed=Echo", "https://api.dicebear.com/6.x/thumbs/svg?seed=Zeta", "https://api.dicebear.com/6.x/thumbs/svg?seed=Orion", "https://api.dicebear.com/6.x/thumbs/svg?seed=Nova", "https://api.dicebear.com/6.x/thumbs/svg?seed=Pixel", "https://api.dicebear.com/6.x/thumbs/svg?seed=Rocket" ]

skills = [ "Python", "MongoDB", "Data Engineering", "ETL", "Machine Learning", "Cloud Computing", "Docker", "JavaScript", "SQL", "APIs" ]

Generate users_schema
users_schema = []

for i in range(1, 101): r_user = { "_id" : ObjectId(), "user_id" : f"U{str(i).zfill(3)}", "email" : fake.email(), "first_name" : fake.first_name(), "last_name" : fake.last_name(), "date_joined" : fake.date_time_between(start_date="-1y", end_date="now"), "role" : random.choice(roles), "profile" : { "bio" : random.choice(bio), "avatar" : random.choice(avatar), "skills" : random.sample(skills, k=3) },

    "is_active" : random.choice([True, False])
}

users_schema.append(r_user)
#Create course schema

Preselected options
categories = ["Data Engineering", "Web Development", "Cloud Computing", "AI", "DevOps"] levels = ["beginner", "intermediate", "advanced"] tags = [ "project-based", "career-ready", "hands-on", "real-world", "certification", "mentor-supported", "interactive", "video-tutorials", "downloadable-resources", "quiz-included", "assignment-driven", "lifetime-access", "community-support", "interview-prep", "beginner-friendly" ] descriptions = [ "Learn how to build scalable data pipelines using modern tools and best practices.", "This course introduces you to web development using HTML, CSS, and JavaScript.", "Master cloud infrastructure and deployment using AWS, Docker, and Kubernetes.", "Understand core machine learning concepts with hands-on Python projects.", "Get started with databases and SQL for data analysis and backend development.", "Develop a strong foundation in data engineering with real-world ETL scenarios.", "Explore modern DevOps workflows, CI/CD pipelines, and monitoring strategies.", "Learn to build REST APIs and microservices with Flask and Django.", "Gain experience in big data technologies like Spark and Hadoop.", "Prepare for a career in AI with deep learning and neural network fundamentals." ]

titles = [ "Python for Beginners", "Data Engineering with Python", "MongoDB for Developers", "Big Data Processing with Spark", "Machine Learning with scikit-learn", "Deep Learning with TensorFlow", "Data Visualization with Seaborn", "AWS Cloud Fundamentals", "Building REST APIs with FastAPI", "Kubernetes for Developers", "Unit Testing in Python", "Git & GitHub for Collaboration", "Clean Code and Refactoring", "Computer Vision with OpenCV", "Deploying Applications with Docker" ]

Simulated instructor user_ids (replace with real ones later)
instructor_ids = [f"U{str(i).zfill(3)}" for i in range(1, 41)]

Generate courses
course_schema = []

for i in range(1, 101): r_course = { "_id": ObjectId(), "course_id": f"C{str(i).zfill(3)}", "title": random.choice(titles), "description": random.choice(descriptions), "instructor_id": random.choice(instructor_ids), "category": random.choice(categories), "level": random.choice(levels), "duration": random.randint(5, 60), # hours "price": random.choice([0, 50, 100, 150, 200, 250, 300]), "tags": random.sample(tags, k=3), "created_at": fake.date_time_between(start_date="-6M", end_date="now"), "updated_at": datetime.now(), "is_published": random.choice([True, False]) }

course_schema.append(r_course)
Query Explanations
This section explains key MongoDB queries used in the project and their purpose.

User Email Lookup db.users.find_one({"email": "john@example.com"})
Purpose: Quickly find a user by their email.
Optimization: Indexed the email field to ensure fast lookups.

Search Courses by Title and Category db.courses.find({"title": "Python Basics", "category": "Programming"})
Purpose: Allows users to search for courses based on their title and category.
Optimization: Compound index on title and category.

Find Assignments by Due Date db.assignments.find({"due_date": {"$lt": ISODate("2025-07-01")}})
Purpose: Retrieve upcoming or overdue assignments.
Optimization: Index on due_date to speed up range queries.

Total Students per Instructor Aggregation to count unique students per instructor
Purpose: Measure reach of each instructor's courses.
Method: Join enrollments and courses, then group by instructor_id.

Revenue Per Instructor Aggregation summing up price paid for all enrollments
Purpose: Calculate total earnings for each instructor.
Method: Join enrollments with courses, then sum pricePaid.

Monthly Enrollment Trends Group by year and month from enrollment date
Purpose: Visualize enrollment activity over time.
Method: $year, $month, and $group aggregation.

Most Popular Course Categories Group enrollments by course category and sort by count
Purpose: Identify top-performing course categories.

Student Engagement Average number of submissions per student
Purpose: Track how actively students participate in assignments.

Lesson Completion Rate Group completions by student
Purpose: Understand how many lessons students are finishing.

Active vs Inactive Students Based on days since last submission
Purpose: Distinguish engaged students from inactive ones using activity timestamps.

Each query is optimized for performance with proper indexing and structured aggregation. These operations power the analytics dashboard and ensure smooth data access for both users and instructors.

***Performance Analysis Results***


Query 1: Find User by Email

Before Optimization:

No index on the email field
Full collection scan (COLLSCAN) observed using .explain()
Slow response time when collection grew large
Optimization Applied: #python db.users.create_index([("email", 1)], unique=True)

After optimization

• IXSCAN used instead of COLLSCAN • Query time reduced significantly

Query 2: Search Courses by Title and Category

Before Optimization db.courses.find({"title": "Mongo Basics", "category": "Database"})

After Optimization db.courses.create_index([("title", 1), ("category", 1)])

Result: • Query performance improved • Execution plan shows efficient use of compound index

Query 3: Filter Assignments by Due Date

Before Optimization db.assignments.find({"due_date": {"$lt": datetime(2025, 7, 1)}})

After Optimization db.assignments.create_index([("due_date", 1)])

Result: • Better response time for due-date range filters • Suitable for dashboards and deadline-based alerts

Performance Measurement Method • Used .explain() in PyMongo to analyze query plans • Used Python time module to measure response times

import time start = time.time() db.users.find_one({"email": "john@example.com"}) end = time.time() print("Query time:", end - start, "seconds")

Conclusion: Adding indexes drastically improved query speed and reduced system load. Each index was tied to actual query patterns for efficiency.

***Challenges Encountered & Solutions***


Data Consistency Between Collections


Challenge: 

Ensuring relationships (like student–enrollment–course–assignment) remained valid across multiple collections.


Solution:


Used consistent _id references between collections.


Enforced schema design where student_id, course_id, etc. were clearly linked to users, courses.
Handling Duplicate Entries


Challenge: Inserting users with existing emails caused DuplicateKeyError.


Solution:
Created unique index on email field.
Used try...except blocks in Python to catch and handle the error gracefully.


Data Validation Issues


Challenge: Accidental insertion of wrong data types (e.g., string instead of datetime) or missing required fields.


Solution:


Defined strict schema validation rules for each collection.


Handled errors with pymongo.errors.WriteError.


Slow Query Performance


Challenge: Queries became slower as data volume increased.


Solution:


Used .explain() to identify performance bottlenecks.


Created indexes on email, due_date, title, category, student_id and course_id to speed up queries.


Understanding Aggregation Pipelines


Challenge: Writing correct and efficient $lookup, $group, and $project stages in aggregation pipelines.


Solution:


Broke down complex queries into steps.


Tested each stage incrementally using MongoDB Compass and Jupyter Notebook.


Exporting Results for Documentation


Challenge: MongoDB Compass does not easily export full query results in desired format.


Solution:

Used PyMongo in Python to fetch and export data.
Documented outputs clearly in .md files and Python scripts.
Working in Jupyter Notebook (VSCode)


Challenge: 

Display formatting (markdown, bold text, font sizes) wasn't intuitive.


Solution:


Used Markdown cells for explanations.
Used HTML tags where needed to adjust text formatting.
