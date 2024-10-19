# Pickr Tech Documentation

## 1. About Pickr

​	The project is a topic selection system for senior students at Chengdu University of Technology Oxford Brooks College. Students can select three topics or propose a custom one, which are saved and submitted within a deadline set by the manager. The system automatically assigns supervisors based on submission time, while custom topics require manager review and potential adjustments. Supervisors can create topics, view selections, and download topic reports. The manager oversees the system, managing users, handling custom topics, setting deadlines, and configuring notifications, ensuring a smooth operation of the program.

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

​	**Warning! Delete the `migrations` folder first after cloning the project! **

​	Before run the project, we need to migrate database first, so that it will automatically generate tables. Type these commands below in the CMD.

```python
flask db init
```

​	This will add a migrations folder to your application. **Note: You only need to run this command once**

```
flask db migrate
flask db upgrade
```

​	**Note: Each time the database models change, repeat the `migrate` and `upgrade` commands.**

### (4). Run project

​	you can either run the command **python -m flask run** or click the green button in the upper right of the PyCharm IDE.  

Finally, you can view the website [Pickr | Home](http://127.0.0.1:5000/) locally

## 3. Technology Stack

**Back-end**

- **Flask**: a lightweight web application framework in **Python**. It is designed to make getting started quick and easy, with the ability to scale up to complex applications. One of the significant advantages of Flask is that it supports a monolithic architecture where the <u>*front-end and back-end are not separated*</u>. This means that the front-end pages and back-end logic are deployed on the same server, often under the same domain. Furthermore, developers don't need to deal with the complexities of a front-end and back-end separation architecture, such as API design, data interaction, and Cross-Origin Resource Sharing (CORS) issues.

**Front-end**

- **Jinja2**: a modern and designer-friendly web templating language for Python, modelled after Django’s templates. It is fast, widely used, and secure with the ability to integrate well with web frameworks Flask.
- **JQuery**: a fast, small, and feature-rich JavaScript library. It makes things like HTML document traversal and manipulation, event handling, animation, and Ajax much simpler with an easy-to-use API that works across a multitude of browsers

**Database**

- Mysql

**Version Control**

- Git

**IDE**

- PyCharm

**Important Dependecies**:

- **Flask-SQLAlchemy**: an extension that adds support for SQLAlchemy to Flask applications. It provides a higher level, ORM-based database abstraction that *<u>simplifies database interactions and eliminates the need to write raw SQL queries</u>*.
- **Flask-Migrate**: an extension that handles SQLAlchemy database migrations. It helps in *<u>managing the evolution of the database schema over time and can automatically generate migration scripts</u>* based on the changes in the data models.
- **pandas**: a powerful data manipulation and analysis library for Python. It provides data structures and functions needed to manipulate structured data, such as spreadsheets and SQL tables, and can be used to *<u>import and read Excel files</u>*.
- **reportlab**: a powerful Python library for *<u>generating PDFs and graphics</u>*.


## 4. Project File Structure

```python
Pickr/
├─blueprints/
├─GitHub_Resource/
├─migrations/
├─models/
├─static/
├─templates/
├─tests/
├─app.py
├─config.py
├─exts.py
├─requirements.txt
```

### blueprints/

> ​	Blueprints can greatly simplify how large applications work and provide a central means for Flask extensions to register operations on applications. A [Blueprint](https://flask.palletsprojects.com/en/2.3.x/api/#flask.Blueprint) object works similarly to a [Flask](https://flask.palletsprojects.com/en/2.3.x/api/#flask.Flask) application object, but it is not actually an application. Rather it is a blueprint of how to construct or extend an application.

​	In short, nearly every functions are written in app.py without blueprints, which can lead to a bloated, complex file that is challenging to maintain. Blueprints offer a solution by allowing us to divide functions up based on categories such as type, purpose or role. The structure figure is shown below.

![](images/blueprint-app.png)

​	Here's a simple example on how to use blueprint

​	First, in `blueprints/student.py`, import `Blueprint` from Flask and create a `bp` object with a name, root path, and URL prefix.

```python
from flask import Blueprint, render_template
bp = Blueprint("student", __name__, url_prefix="/student")
```

​	Then, define a route for a view function, e.g., `index` with `@app.route('/home')`, to display `student/index.html` at `http://localhost:3000/student/home`

```python
@bp.route('/home')
def index():
    return render_template('student/index.html')
```

​	At the end, In `app.py`, import the `student.py` blueprint as `student_bp` and register it globally with `register_blueprint`

```python
from flask import Flask
from blueprints.student import bp as student_bp

app = Flask(__name__)
app.register_blueprint(student_bp)
```

### migrations/

​	 After running `flask init db` , it initializes the database and creates a `migrations` folder in the root path. This folder contains several files that are crucial for managing database migrations.

### models/

​	Models typically contains various data models. Each model represents a database table and is used to interact with the database in an object-oriented manner using SQLAlchemy

​	Here's what the `student.py` file looks like:

```python
from exts import db  # import SQLAlchemy()


class Student(db.Model):
    __tablename__ = 'students'  # define table name
    id = db.Column(db.Integer, primary_key=True)  # db.Column defines the column of the table
    chinese_name = db.Column(db.String(20))
    english_name = db.Column(db.String(20))
    class_number = db.Column(db.String(30))
    email = db.Column(db.String(100))
    password = db.Column(db.String(100))
    user_name = db.Column(db.String(20))

    # initialise a Student object
    def __init__(self, chinese_name, english_name, email, password, user_name, class_number):
        self.chinese_name = chinese_name
        self.english_name = english_name
        self.email = email
        self.password = password
        self.user_name = user_name
        self.class_number = class_number
    
    # add the student instance to the database
    def add(self):
        db.session.add(self)
        db.session.commit()
    
    # update the student instance in the database
    def update(self, chinese_name, english_name, email, user_name, class_number):
        self.chinese_name = chinese_name
        self.english_name = english_name
        self.email = email
        self.user_name = user_name
        self.class_number = class_number
        db.session.commit()
	
    # delete the student instance in the database
    def delete(self):
        db.session.delete(self)
        db.session.commit()
```

### static/

​	This directory contains static files: CSS stylesheets, JavaScript files, fonts, files, images

### templates/

​	This directory holds the HTML templates that are rendered by Flask using the Jinja2 templating engine. These templates are used to generate the HTML responses that are sent to the client.

### tests/

​	This directory is for unit tests and integration tests

### app.py

​	This is the main entry point of the Flask application. It initializes the Flask application instance, configures it, register blueprints, migrate database and error handling

### config.py

​	This file defines configuration classes for three different environments, test, prod and dev

### exts.py

​	This file used to solve the problem of database circular references

```python
from flask_sqlalchemy import SQLAlchemy
db = SQLAlchemy()
```

### requirements.txt

​	This file lists all the Python packages that the application depends on, along with their versions

## 5. APIs(blueprints)

### Base

- login

  - The user's input password is encrypted with **SHA-256** on the frontend to transmission to the server. Once the server verifies the encrypted password against the stored data, the user is directed to their personalized homepage based on their roles.

    ```js
     window.crypto.subtle.digest('SHA-256', new TextEncoder().encode(password))
         .then(hashBuffer => {
             const hashArray = Array.from(new Uint8Array(hashBuffer));
             const hashHex = hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
             sendData(user_name, hashHex);
     })
    ```

- **logout**

  - When user requests to log out, it removes the 'user_name' and 'user_type' from the session and guide user to homepage

- error

  - There are some simple error handlers ensure that when errors occur, the user is presented with a user-friendly error message rather than a raw error page. 

    ```python
    @bp.route('/error')
    def error():
        message = request.args.get('message')
        return render_template('error.html', message=message) 
    @app.errorhandler(404)
    def page_not_found(e):
        return render_template('error.html', message='404 NOT FOUND'), 404
    @app.errorhandler(500)
    def internal_server_error(e):
        return render_template('error.html', message='Internal server error'), 500
    ```

- **topic**

  - topic_list: get topic list
  - topic_search: search topic list by id or name 
  - topic_detail: get detail of a topic by topic id
  - topic_detail_custom: get detail of a custom topic by topic id

### Student

- **index**: Redirect to student homepage.

- **submit**:  Submit all topics selection. <u>Importantly, students can only submit within the specified submission time, and once submitted, they cannot make any further changes</u>.

- **update_custom_topic**: Students update custom selection.

- **update_selection**: It's a quite powerful where student can search topic by topic id using regular expressions,  reset topic selection, update topic selection.

  - ```python
    @bp.route('/update_selection', methods=['POST'])
    @require_student
    def update_selection():
        student_id = Student.get_id(english_name=session['user_name'])
        topic_id = request.form.get('topic_id')
        choice_number = int(request.form.get('choice_number'))
        reset = request.form.get('reset') == 'true'
    
        selection = Selection.get_by_student_id(student_id=student_id)
        if not selection:
            selection = Selection(student_id=student_id)
            selection.add()
    	# reset selection
        if reset:
            if choice_number == 1:
                selection.first_topic_id = None
            elif choice_number == 2:
                selection.second_topic_id = None
            elif choice_number == 3:
                selection.third_topic_id = None
            db.session.commit()
            return json.dumps({'success': True, 'reset': True})
        # re: regular expressions
        match = re.search(r'\d+', topic_id)
        formatted_topic_id = int(match.group())
    	# student has selected this topic
        if formatted_topic_id in [selection.first_topic_id, selection.second_topic_id, selection.third_topic_id]:
            return json.dumps({'success': False, 'error': 'You have already selected this topic'})
        # search topic by topic id
        topic = Topic.get_by_id(id=formatted_topic_id)
        if not topic:
            return json.dumps({'success': False, 'error': 'Topic does not exist'})
        if topic.get_selected_num_final() == topic.quota:
            return json.dumps({'success': False, 'error': 'This topic is full'})
    	# update selection
        if topic:
            if match:
                if choice_number == 1:
                    selection.first_topic_id = formatted_topic_id
                elif choice_number == 2:
                    selection.second_topic_id = formatted_topic_id
                elif choice_number == 3:
                    selection.third_topic_id = formatted_topic_id
            db.session.commit()
            return json.dumps({'success': True, 'topic_name': topic.name})
        else:
            return json.dumps({'success': False, 'error': 'Topic does not exist'})
    ```

### Supervisor

- **index**: Redirect to supervisor homepage

- **new_topic**: render new_topic.html with all types

- **add_topic**: <u>Importantly, when supervisor is creating a new topic, the system will first get total quota of all topics of this supervisor, and plus the number of the position of this new topic. If the sum is less than the position of the supervisor, then this new topic will be added successfully, otherwise not</u>.

  - ```python
    # Supervisor add new topic
    @bp.route('/add_topic', methods=['POST'])
    @require_supervisor
    def add_topic():
        ...
        total_quota = 0
        for topic_in in topics:
            total_quota += topic_in.quota
    
            if total_quota + int(position) > supervisor.position:
                return render_template('supervisor/topic/new_topic.html',
                                       message='Excess quota, you only have ' + str(supervisor.position) + ' positions.', types=types, topic_name=topic_name, type_id=type_id, position=position, description=description, required_skills=required_skills, reference=reference)
       	...
    ```

- **delete_topic**: delete a topic by topic id. However if there are students have selected this topic, it cannot be deleted

  - ```python
    @bp.route('/delete_topic/<int:topic_id>')
    @require_supervisor
    def delete_topic(topic_id):
        topic = Topic.get_by_id(id=topic_id)
        if topic:
            if topic.get_selected_num_final() > 0 or topic.get_selected_num() > 0:
                return jsonify(success=False, error='Can not delete this topic, students have selected this topic.')
            else:
                topic.delete()
                return jsonify(success=True)
        else:
            return jsonify(success=False, error='Topic does not exist')
    ```

- **edit_topic**: render edit_topic.html with topic and all types

- **update_topic**: update topic by topic id. Similar to add_topic, <u>if current total position of all topics(total_quota + int(position) - topic.quota) exceeds the position of the supervisor, or less than the number of students that have selected this topic, it won't update correctly</u>.

  - ```python
    @bp.route('/update_topic/<int:topic_id>', methods=['POST'])
    @require_supervisor
    def update_topic(topic_id):
        ...
        total_quota = 0
        for topic_in in topics:
            total_quota += topic_in.quota
    	# total_quota + int(position) - topic.quota = current total position of all topics
        if total_quota + int(position) - topic.quota > supervisor.position:
            return render_template('supervisor/topic/edit_topic.html',
                                   message='Can not save your modify, excess quota, you only have ' + str(
                                       supervisor.position) + ' positions.',
                                   topic=topic, types=types)
    	# 2.
        if selected_num is not None and selected_num > int(position):
            return render_template('supervisor/topic/edit_topic.html',
                                   message='Can not save your modify, excess quota, ' + str(selected_num) + ' students have selected this topic.',
                                   topic=topic, types=types)
    	...
    ```

- **topic_poster**: generate topic poster

- **export_student_list**: export an excel list of students that are members of supervisor. Columns contains: 'Topic name', 'Chinese Name', 'English Name', 'Class Number', 'Email'.

### Manager

- **index**: Redirect to manager homepage

- **process** 

  - Get all selection records that are waiting to be processed (status=1)

  - Iterates over all selection records. 

  - For each record, it assigns topics based on priority ('first', 'second', 'third').

    - For each priority level, it checks if the topic for that priority is not full. 
      - If the topic is not full, mark this topic selection as success(status=4)
      - otherwise go to next priority level
    - If all topic are full,  mark this selection record as fail(status=5)

    ```python
    @bp.route('/process')
    @require_manager
    def process():
        # get all selection records waiting for processing(status=1)
        selections = Selection.get_all_order_by_submit_time() 
        success = 0
        fail = 0
        for selection in selections:
            # for each selection record process topics by priority
            for priority in ['first', 'second', 'third']: 
                selection_successful = False
                if not selection.topic_is_full(priority): # if current topic is not full
                    selection.update_status(status=4) # mark this selection as success(status=5)
                    selection.update_final_topic_id(topic_id=getattr(selection, f'{priority}_topic_id')) # and this topic_id will be the final topic
                    success += 1
                    selection_successful = True
                    break
            if not selection_successful: # if all topics are full, this selection record fails
                selection.update_status(status=5) # 5:fail
                fail += 1
        return redirect(url_for('manager.index'))
    ```

- **refresh**: reset all the failed selections
  
- **resetting**: reset the selections and students

- **reset_password**: reset user password to default 123456

- **update_deadline**: update selection deadline

- **student**
  
  - **new_student**: render the `new_student.html` template
  - **add_student**: add a new student with default password 123456
  - **delete_student**: delete a student **<u>that hasn't select any topics</u>** by student id
  - **edit_student**: render `edit_student.html`
  - **update_student**: update a student by student id
  - **import_students**: import students from excel file with default password 123456
  - **student_status**: get students status of topic selections
  - **fail_students**: get all failed students
  - **get_template_student**: download students template excel 
  - **check_custom_selection**: render `check_custom_selection.html`
  - **update_custom_selection**: manager assigns the supervisor for the student manually
  
- **supervisor**
  
  - **new_supervisor**: render `new_supervisor.html`
  
  - **add_supervisor**: add a new supervisor with default password 123456
  
  - **delete_supervisor**: delete a supervisor **<u>that hasn't any topics</u>** by supervisor id
  
  - **edit_supervisor**: render `edit_supervisor.html`
  
  - **update_supervisor**: similar to update_topic, i<u>f the new postion of this supervisor less than total quota of all topics</u>, it won't update successfully.
  
    - ```python
      # Manager update supervisor
      @bp.route('/update_supervisor/<int:supervisor_id>', methods=['POST'])
      @require_manager
      def update_supervisor(supervisor_id):
          ...
          total_quota = 0
          for topic in topics:
              total_quota += topic.quota
          if total_quota > int(position):
              return render_template('manager/supervisor/edit_supervisor.html',
                                     message='Can not save your modify, excess quota, supervisor already have ' + str(
                                         total_quota) + ' quta.',
                                     supervisor=supervisor)
      
          supervisor.update(first_name=first_name, last_name=last_name, position=position,
                            user_name=user_name, email=email)
          return redirect(url_for('manager.index', pre='supervisor'))
      ```
  
  - **import_supervisors**: import supervisors from excel file with default password 123456
  
  - **supervisor_status**: get all information about the supervisor and detailed information about all topics under his supervision
  
  - **get_template_supervisor**:  download supervisors template excel 
  
- **type**
  
  - **new_type**: render `new_type.html`
  - **add_type**: add a type
  - **delete_type**: delete a type that has no one topics
  - **edit_type**: render `edit_type.html`
  - **update_type**: update a type
  
- **note**

  - **edit_note**: edit a note
  - **delete_note**: delete a note by note id
  - **new_note**: render the `new_note.html` template
  - **add_note**: add a new note
  - **update_note**: update a note by note id