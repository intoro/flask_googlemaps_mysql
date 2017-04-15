0. setup
sudo apt-get -y update
sudo apt-get -y install python-pip
pip install --upgrade pip
sudo apt-get -y update
sudo apt-get -y install git
sudo apt-get -y install nginx

######Install###############################################################

1.install mysql
sudo apt-get update
sudo apt-get install -y mysql-server

  give a password  ex:"gotime"

2.log into mysql
mysql -uroot -pgotime
  mysql -u<user name> -p<password entered in at install>

INSERT into crimes (latitude, longitude, date, category, description) VALUES ('33','113','2016-2-27 04:58:30','orange','foobar');


3.Logout of mysql
quit

4. Install pymsql so that python can communicate with mysql

sudo pip install --upgrade pip
sudo pip install pymysql
sudo pip install flask

######Set up Database###############################################################
1. create a db_setup.py file
  this creates the structure of the mysql database

2.create a dbconfig.py file
sudo vim dbconfig.py
  this contains the database credentials. keep this off github as it holds the address and password to the
  database

3.commit to git
git init
git config --global user.email "tmcs.brian@gmail.com"
git config --global user.name "intoro"
git remote add origin https://github.com/intoro/<>
git pull origin master
git add db_setup.py
git commit –m "database setup script"
git push origin master

4.create .gitignore file
sudo vim .gitignore
  dbconfig.py
  *.pyc
  basically name every file you don't want added to git

5.Run the database setup script
python db_setup.py

######Set up app ###############################################################
1.set up file structure locally first
cd crimemap
git pull origin master
mkdir templates
touch templates/home.html
touch crimemap.py
touch dbhelper.py

2.put the code in crimemap.py
  create a global DBHelper instance right after initializing our application and
  then use it in the relevant methods to grab data from the database, insert data
  into the database, or delete all data from the database.

3.put the code in dbhelper.py
  As in our setup script, we need to make a connection with our database and then get a
  cursor from our connection in order to do anything meaningful. Again, we will do all
  our operations in try: finally: blocks in order to ensure that the connection is closed.
  we will consider three of the four main database operations. CRUD (Create, Read, Update, and Delete)
  describes the basic database operations.

######create view code###############################################################
1.put form code and jinja code to display the results in the html:
<form action="/add" method="POST">      ##########uses the /add function
  <input type="text" name="userinput">
  <input type="submit" value="Submit">
  </form>
<a href="/clear">clear</a>
{% for userinput in data %}
  <p>{{userinput}}</p>
  {% endfor %}


2.commit to get_all_inputs
git add .
git commit –am "Skeleton CrimeMap"
git push origin master



######create view code###############################################################
1.Mitigating against SQL injection, change add_input code

def add_input(self, data):
    connection = self.connect()
  try:
      query = "INSERT INTO crimes (description) VALUES (%s);"
      with connection.cursor() as cursor:
          cursor.execute(query, data)
          connection.commit()
      finally:
          connection.close()




######adding google maps with javascript###############################################################










.
