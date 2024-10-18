# Pickr Tech Documentation

## 1. About Pickr

​	The project is a topic selection system for senior students at Chengdu University of Technology Oxford Brooks College. Students can select three topics or propose a custom one, which are saved and submitted within a deadline set by the manager. The system automatically assigns supervisors based on submission time, while custom topics require manager review and potential adjustments. Supervisors can create topics, view selections, and download reports. The manager oversees the system, managing users, handling custom topics, setting deadlines, and configuring notifications, ensuring a smooth operation of the program.

## 2. Quick Start

### (1). Install all dependencies

Enter this command below in CMD, and all the dependencies are shown in the **requirement.txt**

```python
pip install -r requirements.txt
```

### (2). Modify database configuretion information

First of all, make sure you have installed **MySQL 8.0**

Second, create a database called **pickr**

Then, click **config.py**, which shows the MySQL configuration information.

​	Modify the **username**, **password** of the **SQLALCHEMY_DATABASE_URI**, the format is shown below

```
mysql+pymysql://username:password@127.0.0.1:3306/pickr
```

### (3). Flask-Migrate:

​	Before run the project, we need to migrate database first, so that it will automatically generate tables. Type these commands below in the CMD.

```python
flask db init
```

​	This will add a migrations folder to your application. The contents of this folder need to be added to version control along with your other source files. **Note: You only need to run this command once**

```
flask db migrate
flask db upgrade
```

​	**Note: Each time the database models change, repeat the `migrate` and `upgrade` commands.**

### (4). Run project

​	you can either run the command 

```
python -m flask run
```

 	or click the green button in the upper right of the PyCharm IDE. 

 	Finally, you can view the website [Pickr | Home](http://127.0.0.1:5000/) locally

## 3. Technology Stack

**Back-end**

- Python3.8
- Flask

**Front-end**

- Jinja2
- HTML
- CSS
- JS, JQuery

**Database**

- Mysql

**Version Control**

- Git

**IDE**

- PyCharm

**Important Dependecies**:

- Flask-Migrate

- Flask-SQLAlchemy


## 4. Project Structure

Pickr/

├─blueprints/

│  ├─base.py

│  ├─manager.py

│  ├─student.py

│  ├─supervisor.py

├─GitHub_Resource/

│  └─logo.png

├─migrations/

├─models/

│  ├─deadline.py

│  ├─note.py

│  ├─pdf_generator.py

│  ├─selection.py

│  ├─student.py

│  ├─supervisor.py

│  ├─topic.py

│  ├─type.py

├─static/

│  ├─css/

│  ├─file/

│  ├─font/

│  ├─image/

│  └─js/

├─templates/

│  ├─base/

│  ├─manager/

│  │  ├─note/

│  │  ├─selection/

│  │  ├─student/

│  │  ├─supervisor/

│  │  └─type/

│  ├─student/

│  └─supervisor/

│      └─topic/

├─tests/

├─app.py

├─config.py

├─exts.py

├─README.md

├─requirements.txt

## 5. APIs(blueprints)

### Base

- login
- logout
- error
- topic_list
- topic_search
- topic_detail
- topic_detail_custom
- process 
- refresh 
- resetting 
- reset_password 
- update_deadline

### Student

- index
- submit
- update_custom_topic
- update_selection

### Supervisor

- index
- new_topic
- add_topic
- delete_topic
- edit_topic
- update_topic
- topic_poster
- export_student_list

### Manager

- index
- note
  - edit_note
  - delete_note
  - new_note
  - add_note
  - update_note

- student
  - delete_student
  - new_student
  - add_student
  - edit_student
  - update_student
  - import_students
  - student_status
  - fail_students # manager get all failed students
  - get_template_student
  - check_custom_selection  
  - update_custom_selection
- supervisor
  - delete_supervisor
  - new_supervisor
  - add_supervisor
  - edit_supervisor
  - update_supervisor
  - import_supervisors
  - supervisor_status
  - get_template_supervisor
- type
  - delete_type
  - new_type
  - add_type
  - edit_type
  - update_type

## 6.Q&A

