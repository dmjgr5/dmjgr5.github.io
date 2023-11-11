---
layout: default
title: Understanding of Python
parent: Python
grand_parent: Programming Language
permalink: /docs/programming-language/flutter-panabara/
nav_order: 10
---

# Understanding of Python
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}




---


![example](/assets/images/python.png)

{: .important-title}
This is the document for Python, which is basic instruction like Python Installation, Data Types and so on. Let me explain briefly about Python language.

 

 
### Python Installation

- Using python IDLE [https://www.python.org](https://www.python.org/)
- Using Visual Studio Code after extension installation [https://code.visualstudio.com] (https://code.visualstudio.com/)


### Python Type

- List [ ]
  - Ordered List
- Tuple ( )
  - Ordered List, not changed
- Set { }
  - Type with Set (used with Union, Intersection, Difference)
- Dictionary {"key":"value"}
  - Paired with Key and Value, No ordered list

### Shallow Copy and Deep Copy

- Shallow Copy

```python
a = [1,2,3]
b =a

import copy

a = [1,2,3]
b = copy.copy(a)

```

- Deep Copy

```python

a = [1,2,3]
b = a[:]  

import copy

a = [1,2,3]
b = copy.deepcopy(a)   

```


### Garbage Collection
- The data type of int, float, complex, String and tuple are immutable, therefore if the variable is updated, it make another new object, so old object which is not used, garbage collection executed. Mostly list and dictionary used because it is mutable type.
The immutable type and global variable should be used with 'global' to update its value.
- Test

```python
>>> g = 1
>>> def testScope(a):
    global g
    g = 2
    return g + a

print("testScoope(1) : ", testScoope(1))
print("g : ",g)

```

- Output

```python

testScoope(1) :  3
g :  1

```


### Variable Parameter
- Any number of parameters can be used by adding * (different a pointer from C)in front of parameters


```python
>>> def test(*args):
    print(type(args))

>>> test(1, 2)
<type 'tuple'> # tuple type

>>> def union(*ar):
    res = []
    for item in ar:
        for x in item:
            if not x in res:
                res.append(x)
    return res

>>> union("HAM", "EGG", "SPAM")
['H', 'A', 'M', 'E', 'G', 'S', 'P']

>>> union("gir", "generation", "gee")
['g', 'i', 'r', 'e', 'n', 'a', 't', 'o']
```

- Any number of parameters (key-value type) can be used by adding ** in front of parameters.
It is option where it requires or not. Option parameter is handled as a dictionary type.

```python
>>> def userURIBuilder(server, port, **user):
    str = "http://" + server + ":" + port + "/?"
    for key in user.keys():
        str += key + "=" + user[key] + "&"
    return str


>>> userURIBuilder("test.com", "8080", id='userid', passwd='1234')
'http://test.com:8080/?passwd=1234&id=userid&'
>>> userURIBuilder("test.com", "8080", id='userid', passwd='1234', name='mike', age='20')
'http://test.com:8080/?passwd=1234&age=20&id=userid&name=mike&' 

```


### Lambda Function
- There is no name, anonymous function. In case of sample function, it is better to use lambda function.
- Example : lambda <parameters> : <phrase>

```python
>>> g = lambda x, y : x * y
>>> g(2,3)
6
>>> (lambda x: x * x)(3)
9
```

### yield
- returns value in real time, used in RANGE phrase.

```python
>>> def reverse(data):
    for index in range(len(data) - 1, -1, -1):
        yield data[index]

>>> for char in reverse('gold'):
    print(char)
    
d
l
o
g 
```

### Private Member
- `Add __<member variable>, changed _<className>__<memberName>`

```python
class BankAccount:
def __init__(self, id, name, balance):

self. __ id = id
self. __ name = name 
self. __ balance = balance 
def deposit(self, amount):
 self. __ balance += amount 
def withdraw(self, amount):
 self. __ balance -= amount
def __str__(self):
 return "{0}, {1}, {2}".format(self.__id, self.__name, \
         self.__balance)
```

### Inheritance
- Example : Class Student(Person):

```python
class Person:
  …

class Student(Person):
    """sub class"""
    def __init__(self, name, phoneNumber, subject, studentID):
       
        Person.__init__(self, name, phoneNumber) # Call Person Init 
        self.subject = subject
        self.studentID = studentID
```


### PYTHONPATH in System Variables
- When creating module, we can allocate the module location in specific folder using system properties path named as PYTHONPATH and the absolute path.
Classes and Functions can be defined in Module.

- Module example

```python
from functools import *

def intersect(*ar):
    return reduce(__intersectSC,ar)

def __intersectSC(listX, listY):
    setList = []
    for x in listX:
        if x in listY:
            setList.append(x)
    return setList

def difference(*ar):
    setList = []
    intersectSet = intersect(*ar)
    unionSet = union(*ar)
    for x in unionSet:
        if not x in intersectSet:
            setList.append(x)
    return setList

def union(*ar):
    setList = []
    for item in ar:
        for x in item:
            if not x in setList:
                setList.append(x)
    return setList
```

- Module Importing Style
  - import simpleset
    - get from global namespace, get all functions
    - check with command like dir()
  - from simpleset import union
    - specific functions, ex) union function Only
    - check with command like dir(union)
  - from simpleset import * 
    - all functions, except the function start with __(double-underlined)
    - check with command like dir()
  - import simpleset as test1
    - make alias named as test1

### `__pycache__` folder
- After compile a pyc file, it is generated in `__pycache__`, as well we can share this file to others as a module, not to show the source code.
Pyinstaller is useful for this case :
  - pip3 install pyinstaller
  - python BankAccout.py
  - BankAccount in 'dist' folder


### Exception

```python
def divide(a,b):
    return a/b

try:
    c = divide(5, 2)
except ZeroDivisionError:
    print('0 is not allowed')
except TypeError:
    print("All parameters must be numbers")
except:
    print("Not defined")
else:
    print("Result : {0}", c)

finally:
    print("do anything")
```

### Input and Output
- Output

```python
import sys

f = open("C:\\work\\test.txt", "wt")
print("file write", file=f)
f.close()
```

- read() : returns string all from a file
- readline() : returns string line by line
- readlines() : returns a list with line by line at once

```python
>>> f = open("c:\\work\\demo.txt")
>>> result = f.read()
>>> result
'qqqq\nabcd\n1234\n'
>>> print(result)
qqqq
abcd
1234

>>> f.tell()
25
>>> f.seek(0)
0
>>> f.tell()
0
>>> f.readline()
'qqqq\n'
>>> f.readline()
'abcd\n'
>>> f.readline()
'1234\n'
>>> f.seek(0)
0
>>> lst = f.readlines()
>>> lst
['qqqq\n', 'abcd\n', '1234\n']
```
- Input
```
f = open("c:\\work\\demo1.txt", "wt")
f.write("qqqq\nabcd\n1234\n")
f.close()
```

### Pickle
- Save information temporarily

```python
import pickle

colors = ["red", "blue", "gree"]

# Write
f = open("c:\\work\\colors","wb") # write binary
pickle.dump(colors,f) # back up with .dump function
f.close()

# Read
f = open("c:\\work\\colors","rb") # read binary
colors = pickle.load(f) # open with .load function
print(colors)
f.close()

del colors # delete list

import os 
os.remove("c:\\work\\colors")  # delete file
```

### Regular Expression

```python
import re 

f=open('c:\\work\\PV3.txt','rt')
g=open('c:\\work\\PV3_copy.txt','wt')

 
line = f.readline()
while (line != ''):
    if (re.search("\d{4}", line)):
        g.write(line + "\n")
    line = f.readline()

f.close() 
g.close()
```

### Time, Math and Random

```python
>>> import random
>>> random.random()
0.35173868090650484
>>> random.random()
0.5306281074635111
>>> random.uniform(3,4) 
3.5888882103413913
>>> for i in range(3):  
 random.gauss(1, 1.0)

 
0.9384305864119867
1.8460570340312912
1.3857190077793522
>>> [random.randrange(20) for i in range(10)] 
[12, 2, 4, 11, 2, 15, 7, 13, 17, 15]
>>> random.sample(range(20), 10) 
[8, 6, 0, 7, 2, 4, 10, 11, 12, 18]

```

```python
>>> import random
>>> random.random()
0.35173868090650484
>>> random.random()
0.5306281074635111
>>> random.uniform(3,4) 
3.5888882103413913
>>> for i in range(3):  
 random.gauss(1, 1.0)

 
0.9384305864119867
1.8460570340312912
1.3857190077793522
>>> [random.randrange(20) for i in range(10)] 
[12, 2, 4, 11, 2, 15, 7, 13, 17, 15]
>>> random.sample(range(20), 10) 
[8, 6, 0, 7, 2, 4, 10, 11, 12, 18]
```

```python
>>> import random
>>> random.random()
0.35173868090650484
>>> random.random()
0.5306281074635111
>>> random.uniform(3,4) 
3.5888882103413913
>>> for i in range(3):  
 random.gauss(1, 1.0)

 
0.9384305864119867
1.8460570340312912
1.3857190077793522
>>> [random.randrange(20) for i in range(10)] 
[12, 2, 4, 11, 2, 15, 7, 13, 17, 15]
>>> random.sample(range(20), 10) 
[8, 6, 0, 7, 2, 4, 10, 11, 12, 18]
```

```python
>>> import random
>>> random.random()
0.35173868090650484
>>> random.random()
0.5306281074635111
>>> random.uniform(3,4) 
3.5888882103413913
>>> for i in range(3):  
 random.gauss(1, 1.0)

 
0.9384305864119867
1.8460570340312912
1.3857190077793522
>>> [random.randrange(20) for i in range(10)] 
[12, 2, 4, 11, 2, 15, 7, 13, 17, 15]
>>> random.sample(range(20), 10) 
[8, 6, 0, 7, 2, 4, 10, 11, 12, 18]
```

### OS

```python
>>> from os.path import *
>>> abspath('tmp')  
'C:\\Python35\\tmp'
>>> basename('c:\\python35\\tmp')  
'tmp'
>>> basename('c:\\python35\\tmp\\test.txt')
'test.txt'
>>> commonprefix(['c:\\python35\\lib', 'c:\\python35\tools', 'c:\\python35']) 
'c:\\python35'
>>> exists('c:\\python35\\tmp') 
False
>>> exists('c:\\python35')
True
>>> getsize('c:\\python35\\python.exe') 
26624
>>> isfile('c:\\python35\\python.exe') 
True
```

### Database with SQLite3
SQLite3 is an opensource, which is embedded in Python library.

- sqlite3.connect(database[, timeout, isolation_level, detect_types, factory])
- sqlite3.complete_statement(sql)
- sqlite3.register_adapter(type, callable) 
- sqlite3.register_converter(typename, callable) 
  - Connection Class
  - Connection.cursor()
  - Connection.rollback() 
  - Connection.close() 
  - Connection.isolation_level
  - Connection.execute(sql[, parameters])
- Cursor Class
  - Cursor.execute(sql[, parameters]) 
  - Cursor.executemany(sql, seq_of_parameters)
  - Cursor.executescript(sql_script)  
  - Cursor.fetchone()
  - Cursor.fetchmany([size=cursor.arraysize]) 
  - Cursor.fetchall() 
- Connection.iterdump()

- CRUD example

```python
import sqlite3

#generate connection object
con = sqlite3.connect(":memory:") # in memory
# con = sqlite3.connect("test.db") # in file

#generate cursor
cur = con.cursor()

# create ( use text intead of varchar2 in SQLite3)
cur.execute("create table PhoneBook (Name text, PhoneNum text);")
# insert
cur.execute("insert into PhoneBook values ('derick', '010-111');")

# insert using variables
name = "gil dong"
phoneNumber = "010-222"
cur.execute("insert into PhoneBook values ( ?, ?);", (name, phoneNumber)) # input using Tuple type

# multiple record
datalist = (("tom","010-123"),("dsp","010-567")) # tuple in tuple
cur.executemany("insert into PhoneBook values ( ?, ?);", datalist) # input using Tuple type, with executemany function


# search
cur.execute("select * from PhoneBook;")

print(cur.fetchone())
print(cur.fetchmany(2))
cur.execute("select * from PhoneBook;")
print(cur.fetchall())

# dump
print("--------- db dump ---------")
for i in con.iterdump():
    print(i)


# *** Result => results out as tuple type
# ('derick', '010-111')
# [('gil dong', '010-222'), ('tom', '010-123')]
# [('derick', '010-111'), ('gil dong', '010-222'), ('tom', '010-123'), ('dsp', '010-567')]
# --------- db dump ---------
# BEGIN TRANSACTION;
# CREATE TABLE PhoneBook (Name text, PhoneNum text);
# INSERT INTO "PhoneBook" VALUES('derick','010-111');
# INSERT INTO "PhoneBook" VALUES('gil dong','010-222');
# INSERT INTO "PhoneBook" VALUES('tom','010-123');
# INSERT INTO "PhoneBook" VALUES('dsp','010-567');
# COMMIT;
```

- executemany example with tuple data


```python
import sqlite3

#generate connection object
# con = sqlite3.connect(":memory:") # in memory
con = sqlite3.connect("c:\\work\\sample.db") # in file

#generate cursor
cur = con.cursor()

# create ( use text intead of varchar2 in SQLite3)
cur.execute("create table PhoneBook (Name text, PhoneNum text);")
# insert
cur.execute("insert into PhoneBook values ('derick', '010-111');")

# insert using variables
name = "gil dong"
phoneNumber = "010-222"
cur.execute("insert into PhoneBook values ( ?, ?);", (name, phoneNumber)) # input using Tuple type

# multiple record
datalist = (("tom","010-123"),("dsp","010-567")) # tuple in tuple
cur.executemany("insert into PhoneBook values ( ?, ?);", datalist) # input using Tuple type, with executemany function


# search
cur.execute("select * from PhoneBook;")
for row in cur:
    print(row)

# commit
con.commit()

# *** Result => results out as tuple type
# ('derick', '010-111')
# ('gil dong', '010-222')
# ('tom', '010-123')
# ('dsp', '010-567')

```


- Backup example

```python

import sqlite3

#generate connection object
con = sqlite3.connect(":memory:") # in memory
# con = sqlite3.connect("test.db") # in file

#generate cursor
cur = con.cursor()

# create ( use text intead of varchar2 in SQLite3)
cur.execute("create table PhoneBook (Name text, PhoneNum text);")
# insert
cur.execute("insert into PhoneBook values ('derick', '010-111');")

# insert using variables
name = "gil dong"
phoneNumber = "010-222"
cur.execute("insert into PhoneBook values ( ?, ?);", (name, phoneNumber)) # input using Tuple type

# multiple record
datalist = (("tom","010-123"),("dsp","010-567")) # tuple in tuple
cur.executemany("insert into PhoneBook values ( ?, ?);", datalist) # input using Tuple type, with executemany function



# database dump
with open("c:\\work\\dump.sql","wt") as f:
    for item in con.iterdump():    
        print(item)
        f.write("{0}\n".format(item))

print("backup completed")


# *** Result => results out as tuple type
# BEGIN TRANSACTION;
# CREATE TABLE PhoneBook (Name text, PhoneNum text);
# INSERT INTO "PhoneBook" VALUES('derick','010-111');
# INSERT INTO "PhoneBook" VALUES('gil dong','010-222');
# INSERT INTO "PhoneBook" VALUES('tom','010-123');
# INSERT INTO "PhoneBook" VALUES('dsp','010-567');
# COMMIT;
# backup completed

```

- Restore example

```python
# open new dump
>>> con = sqlite3.connect("c:\\work\\demo.db")

# read old dump
>>> with open("c:\\work\\dump.sql") as f:
 SQLScript = f.read()

# check dump script
>>> SQLScript
'BEGIN TRANSACTION;\nCREATE TABLE PhoneBook (Name text, PhoneNum text);\nINSERT INTO "PhoneBook" VALUES(\'derick\',\'010-111\');\nINSERT INTO "PhoneBook" VALUES(\'gil dong\',\'010-222\');\nINSERT INTO "PhoneBook" VALUES(\'tom\',\'010-123\');\nINSERT INTO "PhoneBook" VALUES(\'dsp\',\'010-567\');\nCOMMIT;\n'

# create cursor
>>> cur = con.cursor()

# execute script
>>> cur.executescript(SQLScript)
<sqlite3.Cursor object at 0x03F4B7E0>

# select
>>> cur.execute("select * from PhoneBook;")
<sqlite3.Cursor object at 0x03F4B7E0>

# fetch all
>>> cur.fetchall()
[('derick', '010-111'), ('gil dong', '010-222'), ('tom', '010-123'), ('dsp', '010-567')]

```


### Web Crawling with BeautifulSoup
- `pip3 list`
- `pip3 install BeautifulSoup4`
- `pip install requests `
- `pip list`

- Example of Webcrawling

```python
# request webserver
import urllib.request
from bs4 import BeautifulSoup

data = urllib.request.urlopen("http://comic.naver.com/webtoon/list.nhn?titleId=20853&weekday=fri")

soup = BeautifulSoup(data, "html.parser")

cartoons = soup.find_all("td", class_="title")

title = cartoons[0].find('a').text
link = cartoons[0].find('a')["href"]

# print(title)
# print(link)

for item in cartoons:
    title = item.find("a").text
    print(title.strip())

```

- Save file and web crawling for all pages

```python

# coding:utf-8
from bs4 import BeautifulSoup
import urllib.request
import re 

for n in range(0,10):
        #address of clien
        data ='https://www.clien.net/service/board/sold?&od=T31&po=' + str(n)
        req = urllib.request.Request(data)
        data = urllib.request.urlopen(req).read()
        page = data.decode('utf-8', 'ignore')
        soup = BeautifulSoup(page, 'html.parser')
        list = soup.find_all('span', attrs={'data-role':'list-title-text'})

        f = open("C:\\work\\clien.txt", "a+", encoding="utf-8") # incase of new, use wt

        for item in list:
                try:
                        title = item.text
                        if (re.search('갤럭시', title)):
                                print(title.strip())
                                f.write(title.strip() + "\n")
                except:
                        pass
        
f.close()
print("end of web crawling")
```

- Add user-agent in header

```python
# coding:utf-8
from bs4 import BeautifulSoup
import urllib.request
import re 

#User-Agent add
hdr = {'User-agent':'Mozila/5.0 (compatible; MSIE 5.5; Windows NT)'} # make request by adding client info with Internet explorer

for n in range(1,11):
        data ='https://dvdprime.com/g2/bbs/board.php?bo_table=comm&page=' + str(n)
        req = urllib.request.Request(data, \
                                          headers = hdr)
        data = urllib.request.urlopen(req).read()
        page = data.decode('utf-8', 'ignore')
        soup = BeautifulSoup(page, 'html.parser')

        # <span class='list_subject_span_pc'>contents</span>
        list = soup.find_all('span', attrs={'class':'list_subject_span_pc'})

        for item in list:
                try:
                        title = item.text
                        if (re.search('회사', title)):
                                print(title.strip())
                except:
                        pass
        
```

### PyQt5 GUI

Install `PyQt5-5.6-gpl-Py3.5-Qt5.6.0-x32-2.exe`, make sure same version with python
- `.ui` file
  - XML format
  - Make mapping signal and slot in Signal/Slot Editor on Qt designer

- `.py` file
  - Add login in py file

```python
# demo.ui (display) + demo.py(logic)

import sys
from PyQt5.QtWidgets import *
from PyQt5 import uic

import urllib.request
from bs4 import BeautifulSoup


form_class = uic.loadUiType("demo2.ui")[0]

#parent class((QMainWindow))
class DemoForm(QMainWindow, form_class):
    def __init__(self):
        super().__init__()
        self.setupUi(self)
    def firstClick(self):
        
        

        data = urllib.request.urlopen("http://comic.naver.com/webtoon/list.nhn?titleId=20853&weekday=fri")

        soup = BeautifulSoup(data, "html.parser")

        cartoons = soup.find_all("td", class_="title")

        # title = cartoons[0].find('a').text
        # link = cartoons[0].find('a')["href"]

        # print(title)
        # print(link)

        for item in cartoons:
            title = item.find("a").text
            print(title.strip())






    def secondClick(self):
        self.label.setText("clicked second")
    def thirdClick(self):
        self.label.setText("clicked third")    


if __name__ == "__main__":
    app = QApplication(sys.argv)
    demoWindow = DemoForm()
    demoWindow.show()
    app.exec_()
```

- Get data and show table and link to browser.


```python
import sys
from PyQt5.QtWidgets import *
import urllib.request
from bs4 import BeautifulSoup
import webbrowser    
import re 

class Form(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setupUI()

    def setupUI(self):
        # (x,y,width,height) 
        self.setGeometry(200, 200, 800, 800)
        
        # INPUT
        self.lineEdit = QLineEdit("", self)
        self.lineEdit.move(20, 20)

        #BUTTON
        self.btn = QPushButton("검색", self)
        self.btn.move(120, 20)
        self.btn.clicked.connect(self.setTableWidgetData)

        self.tableWidget = QTableWidget(self)
        self.tableWidget.move(20, 70)
        self.tableWidget.resize(800, 600)
        self.tableWidget.setRowCount(50)  
        self.tableWidget.setColumnCount(2)  
        
        self.tableWidget.setColumnWidth(0, 300)
        self.tableWidget.setColumnWidth(1, 300)
        
      
        self.tableWidget.doubleClicked.connect(self.doubleClicked)


    def setTableWidgetData(self):
        row = 0
        for n in range(0,5):
    
            data ='https://www.clien.net/service/board/sold?&od=T31&po=' + str(n)
            req = urllib.request.Request(data)
            data = urllib.request.urlopen(req).read()
            page = data.decode('utf-8', 'ignore')
            soup = BeautifulSoup(page, 'html.parser')
            list = soup.find_all('a', attrs={'class':'list_subject'})

            f = open("clien.txt", "a+", encoding="utf-8")
            for item in list:
                try:
                    span = item.contents[3]
                    title = item.text.strip()
     
                    if (re.search(self.lineEdit.text(), title)):
                        title = title.replace("\t", "")
                        title = title.replace("\n", "")
                        print(title)
                        link = 'https://www.clien.net'  + item['href'] 
                        print(link.strip())
                        f.write(title+"\n")
                        f.write(link + "\n")
            
                        self.tableWidget.setItem(row, 0, QTableWidgetItem(title))
                        self.tableWidget.setItem(row, 1, QTableWidgetItem(link))
                        row += 1
                        print("row: ", row) 
                except:
                    pass
             
            f.close()

    def doubleClicked(self):
        url = self.tableWidget.item(self.tableWidget.currentRow(), 1).text()
        webbrowser.open(url) 

if __name__ == "__main__":
    app = QApplication(sys.argv)
    mywindow = Form()
    mywindow.show()
    app.exec_()

```

### Pandas

  - Import type
    - from pandas import Series, DataFrame  
    - import pandas as pd
  - Datatypes
    - Series
      - It is similar to list
    - Dataframe
      - 2 array list
  - Read function
    - read_csv : based on csv from DB mainly
    - read_table : based on tab key
    - read_fwf : based on fixed width
    - read_excel : based on excel
  - Functions
    - plt.plot
    - plt.scatter
    - plt.bar


    ```python
    # make one frame
    years = range(1880,2011)
    pieces = []
    columns = ["name", "sex", "births"]

    for year in years:
        path = "c:\\work\\yob%d.txt" % year
        frame = pd.read_csv(path, names = columns)
        frame["year"] = year
        pieces.append(frame)
        names = pd.concat(pieces, ignore_index=True)
    ```


- Summary

    ```python
    total_births = names.pivot_table("births", index = 'year', columns='sex', aggfunc=sum)
    ```

- Plot

    ```python
    total_births.plot(title="년도별 성별 출생 숫자")
    ```


    ![example](/assets/images/pyplot.png)