---
layout: single
title:  "Interacting With Your SQLite DB in a Jupyter Notebook"
---

Recently I used MySQL for *Introduction to Software Engineering* at Cal Poly. I didn't master it, but I gained some good exposure. I wanted to learn more about databases in preparation for this upcoming quarter while building on what I've been studying. So I started two tutorials called *A Gentle Introduction to SQL Using SQLite* and *Working with SQLite Databases using Python and Pandas*, by [Troy Thibodeaux](https://github.com/tthibo) and [Vik Paruchuri](https://www.dataquest.io/blog/author/vik/) respectively. The first is available on Github [here](https://github.com/tthibo/SQL-Tutorial) and the latter is available on Dataquest [here](https://www.dataquest.io/blog/python-pandas-databases/).

Without further ado, here is a brief introduction to my independent studies this holiday season.

To begin, we will import `pandas` and `sqlite3`. Then we'll create a connection to our **SQLite** database. (For the sake of this tutorial, I will be using a SQLite database which I setup using [SQLite DB Browser](https://sqlitebrowser.org/).) With inspiration from *A Gentle Introduction to SQL Using SQLite*, this database contains several gigabytes of United States campaign finance data downloaded from [fec.gov](https://www.fec.gov/data/browse-data/?tab=bulk-data).   
*Note:* I've placed the database in a folder in the same directory which we're working in. The directory tree looks like so:

```
.
├── cheat_sheets   
│   ├── numpy.jpg   
│   └── python-sklearn.png   
├── sqlite_notebook_intro.ipynb   
└── sqlite_tutorial   
    ├── candidates.txt   
    ├── contributors_candidates.db   
    ├── contributors_with_candidate_id.txt
```


```python
import pandas as pd
import sqlite3

conn = sqlite3.connect("./sqlite_tutorial/contributors_candidates.db")
```

**Create a cursor object:**


```python
cur = conn.cursor()
```

**Execute a SQL query:**


```python
cur.execute("select * from candidates limit 5;")
```




    <sqlite3.Cursor at 0x119236ab0>



**And fetch the results:**


```python
results = cur.fetchall()
print(results)
```

    [(16, 'Mike', 'Huckabee', '', 'R'), (20, 'Barack', 'Obama', '', 'D'), (22, 'Rudolph', 'Giuliani', '', 'R'), (24, 'Mike', 'Gravel', '', 'D'), (26, 'John', 'Edwards', '', 'D')]


**Let's create a Pandas dataframe from the database:**


```python
df = pd.read_sql_query("SELECT * FROM candidates", conn)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>first_name</th>
      <th>last_name</th>
      <th>middle_name</th>
      <th>party</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16</td>
      <td>Mike</td>
      <td>Huckabee</td>
      <td></td>
      <td>R</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20</td>
      <td>Barack</td>
      <td>Obama</td>
      <td></td>
      <td>D</td>
    </tr>
    <tr>
      <th>2</th>
      <td>22</td>
      <td>Rudolph</td>
      <td>Giuliani</td>
      <td></td>
      <td>R</td>
    </tr>
    <tr>
      <th>3</th>
      <td>24</td>
      <td>Mike</td>
      <td>Gravel</td>
      <td></td>
      <td>D</td>
    </tr>
    <tr>
      <th>4</th>
      <td>26</td>
      <td>John</td>
      <td>Edwards</td>
      <td></td>
      <td>D</td>
    </tr>
    <tr>
      <th>5</th>
      <td>29</td>
      <td>Bill</td>
      <td>Richardson</td>
      <td></td>
      <td>D</td>
    </tr>
    <tr>
      <th>6</th>
      <td>30</td>
      <td>Duncan</td>
      <td>Hunter</td>
      <td></td>
      <td>R</td>
    </tr>
    <tr>
      <th>7</th>
      <td>31</td>
      <td>Dennis</td>
      <td>Kucinich</td>
      <td></td>
      <td>D</td>
    </tr>
    <tr>
      <th>8</th>
      <td>32</td>
      <td>Ron</td>
      <td>Paul</td>
      <td></td>
      <td>R</td>
    </tr>
    <tr>
      <th>9</th>
      <td>33</td>
      <td>Joseph</td>
      <td>Biden</td>
      <td></td>
      <td>D</td>
    </tr>
    <tr>
      <th>10</th>
      <td>34</td>
      <td>Hillary</td>
      <td>Clinton</td>
      <td>R.</td>
      <td>D</td>
    </tr>
    <tr>
      <th>11</th>
      <td>35</td>
      <td>Mitt</td>
      <td>Romney</td>
      <td></td>
      <td>R</td>
    </tr>
    <tr>
      <th>12</th>
      <td>36</td>
      <td>Samuel</td>
      <td>Brownback</td>
      <td></td>
      <td>R</td>
    </tr>
    <tr>
      <th>13</th>
      <td>37</td>
      <td>John</td>
      <td>McCain</td>
      <td></td>
      <td>R</td>
    </tr>
    <tr>
      <th>14</th>
      <td>38</td>
      <td>Tom</td>
      <td>Tancredo</td>
      <td></td>
      <td>R</td>
    </tr>
    <tr>
      <th>15</th>
      <td>39</td>
      <td>Christopher</td>
      <td>Dodd</td>
      <td>J.</td>
      <td>D</td>
    </tr>
    <tr>
      <th>16</th>
      <td>41</td>
      <td>Fred</td>
      <td>Thompson</td>
      <td>D.</td>
      <td>R</td>
    </tr>
  </tbody>
</table>
</div>



**Finally, we will close the cursor and database connection**


```python
cur.close()
conn.close()
```

# Concluding Remarks
This notebook walks through some of the basics of the tools I studied over the holidays, i.e. Jupyter Notebooks, SQLite, the sqlite3 package, and pandas. In an upcoming post, I will show more of what I've been up to.
