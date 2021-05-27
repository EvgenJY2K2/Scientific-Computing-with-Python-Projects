## Relational Databases and SQLite
###### Example 1: this application will read the mailbox data (mbox.txt) and count the number of email messages per organization (i.e. domain name of the email address) using a database with the following schema to maintain the counts.
```
import sqlite3

conn = sqlite3.connect('emaildb.sqlite')
cur = conn.cursor()

cur.execute('DROP TABLE IF EXISTS Counts')

cur.execute('''
CREATE TABLE Counts (domain TEXT, count INTEGER)''')

fname = input('Enter file name: ')
if (len(fname) < 1): fname = 'mbox.txt'
fh = open(fname)
for line in fh:
    if not line.startswith('From: '): continue
    pieces = line.split()
    email = pieces[1]
    domain = email.split('@')[1]
    
    cur.execute('SELECT count FROM Counts WHERE domain = ? ', (domain,))
    row = cur.fetchone()
    if row is None:
        cur.execute('''INSERT INTO Counts (domain, count)
                VALUES (?, 1)''', (domain,))
    else:
        cur.execute('UPDATE Counts SET count = count + 1 WHERE domain = ?',
                    (domain,))
    conn.commit()

# https://www.sqlite.org/lang_select.html
sqlstr = 'SELECT domain, count FROM Counts ORDER BY count DESC LIMIT 10'

for row in cur.execute(sqlstr):
    print(str(row[0]), row[1])

cur.close()
```
###### Example 2: this application will read an iTunes export file in XML and produce a properly normalized database.
```
import xml.etree.ElementTree as ET
import sqlite3
#connects to database, creates a sqlite file
conn = sqlite3.connect('trackdb.sqlite')
cur  = conn.cursor()

# Make some fresh tables using executescript()
cur.executescript('''
DROP TABLE IF EXISTS Artist;
DROP TABLE IF EXISTS Album;
DROP TABLE IF EXISTS Track;
DROP TABLE IF EXISTS Genre;

CREATE TABLE Artist (
    id  INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    name    TEXT UNIQUE
);

CREATE TABLE Album (
    id  INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    artist_id  INTEGER,
    title   TEXT UNIQUE
);

CREATE TABLE Track (
    id  INTEGER NOT NULL PRIMARY KEY
        AUTOINCREMENT UNIQUE,
    title TEXT  UNIQUE,
    album_id  INTEGER,
    genre_id  INTEGER,
    len INTEGER,
    rating INTEGER,
    count INTEGER
);


CREATE TABLE Genre (
    id  INTEGER NOT NULL PRIMARY KEY
    AUTOINCREMENT UNIQUE,
    name    TEXT UNIQUE
)

''')


fname = input('Enter file name: ')
if ( len(fname) < 1 ) : fname = 'Library.xml'

# <key>Track ID</key><integer>369</integer>
# <key>Name</key><string>Another One Bites The Dust</string>
# <key>Artist</key><string>Queen</string>
def lookup(d, key):
    found = False
    for child in d:
        if found : return child.text
        if child.tag == 'key' and child.text == key :
            found = True
    return None

#create element tree and find the branch
stuff = ET.parse(fname)
all = stuff.findall('dict/dict/dict')
print ('Dict count:', len(all))
for entry in all:
    if ( lookup(entry, 'Track ID') is None ) : continue

    name = lookup(entry, 'Name')
    artist = lookup(entry, 'Artist')
    album = lookup(entry, 'Album')
    count = lookup(entry, 'Play Count')
    rating = lookup(entry, 'Rating')
    length = lookup(entry, 'Total Time')
    genre_name = lookup(entry, 'Genre')

    if name is None or artist is None or album is None or genre_name is None:
        continue

    print (name, artist,genre_name, album, count, rating, length)

    #insert data into table
    cur.execute('''INSERT OR IGNORE INTO Artist (name)
        VALUES ( ? )''', ( artist, ) )
    cur.execute('SELECT id FROM Artist WHERE name = ? ', (artist, ))
    artist_id = cur.fetchone()[0]

    cur.execute('''INSERT OR IGNORE INTO Album (title, artist_id)
        VALUES ( ?, ? )''', ( album, artist_id ) )
    cur.execute('SELECT id FROM Album WHERE title = ? ', (album, ))
    album_id = cur.fetchone()[0]

    cur.execute('''INSERT OR IGNORE
    INTO Genre(name)
        VALUES (?)''', (genre_name, ))
    cur.execute('SELECT ID FROM Genre WHERE name = ?', (genre_name, ))
    genre_id = cur.fetchone()[0]

    cur.execute('''INSERT OR REPLACE INTO Track
        (title, album_id, genre_id, len, rating, count)
        VALUES ( ?, ?, ?, ?, ?, ? )''',
        ( name, album_id, genre_id, length, rating, count ) )
conn.commit()
```
###### Example 3: this application will read roster data in JSON format, parse the file, and then produce an SQLite database that contains a User, Course, and Member table and populate the tables from the data file.
```
import json
import sqlite3

conn = sqlite3.connect('rosterdb.sqlite')
cur = conn.cursor()

# Do some setup
cur.executescript('''
DROP TABLE IF EXISTS User;
DROP TABLE IF EXISTS Member;
DROP TABLE IF EXISTS Course;

CREATE TABLE User (
    id     INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    name   TEXT UNIQUE
);

CREATE TABLE Course (
    id     INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    title  TEXT UNIQUE
);

CREATE TABLE Member (
    user_id     INTEGER,
    course_id   INTEGER,
    role        INTEGER,
    PRIMARY KEY (user_id, course_id)
)
''')

fname = input('Enter file name: ')
if len(fname) < 1:
    fname = 'roster_data_sample.json'

# [
#   [ "Charley", "si110", 1 ],
#   [ "Mea", "si110", 0 ],

str_data = open(fname).read()
json_data = json.loads(str_data)

for entry in json_data:
	user = entry[0]
	course = entry[1]
	instructor = entry[2]

	#Inserting user
	user_statement = """INSERT OR IGNORE INTO User(name) VALUES( ? )"""
	SQLparams = (user, )
	cur.execute(user_statement, SQLparams)

	#Inserting course
	course_statement = """INSERT OR IGNORE INTO Course(title) VALUES( ? )"""
	SQLparams = (course, )
	cur.execute(course_statement, SQLparams)

	#Getting user and course id
	courseID_statement = """SELECT id FROM Course WHERE title = ?"""
	SQLparams = (course, )
	cur.execute(courseID_statement, SQLparams)
	courseID = cur.fetchone()[0]

	userID_statement = """SELECT id FROM User WHERE name = ?"""
	SQLparams = (user, )
	cur.execute(userID_statement, SQLparams)
	userID = cur.fetchone()[0]

	#Inserting the entry
	member_statement = """INSERT INTO Member(user_id, course_id, role)
		VALUES(?, ?, ?)"""
	SQLparams = (userID, courseID, instructor)
	cur.execute(member_statement, SQLparams)

#Saving the changes
conn.commit()

test_statement = """
SELECT hex(User.name || Course.title || Member.role ) AS X FROM 
    User JOIN Member JOIN Course 
    ON User.id = Member.user_id AND Member.course_id = Course.id
    ORDER BY X
"""
cur.execute(test_statement)
result = cur.fetchone()
print("RESULT: " + str(result))

#Closing the connection
cur.close()
conn.close()
```
