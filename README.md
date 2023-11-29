# Changes from the original code

---
- ### Add two method in Table class
```py
def insert_row(self, dict):
    self.table.append(dict)
```
```py
def update_row(self, primary_attribute, primary_attribute_value, update_attribute, update_value):
    for i in self.table:
        if i[primary_attribute] == primary_attribute_value:
            i[update_attribute] = update_value
```

- ### Implement the queries in main part
```py
db = DB()
movies = []
with open(os.path.join(__location__, 'movies.csv')) as f:
    rows = csv.DictReader(f)
    for r in rows:
        movies.append(dict(r))

table1 = Table("movies", movies)
db.insert(table1)
movies_table = db.search("movies")
comedy = movies_table.filter(lambda x: x["Genre"] == "Comedy").select(["Worldwide Gross"])
average_wwg_comedy = sum([float(i["Worldwide Gross"]) for i in comedy]) / \
                     len([i["Worldwide Gross"] for i in comedy])
print("Average value of ‘Worldwide Gross’ for ‘Comedy’ movies is", average_wwg_comedy)
print()
ad_score_dict = movies_table.select(["Audience score %"])
min_ad_score = min([float(i["Audience score %"]) for i in ad_score_dict])
print("Minimum ‘Audience score %’ for ‘Drama’ movies is", min_ad_score)
print()
fantasy = movies_table.filter(lambda x: x["Genre"] == "Fantasy").table
print("Fantasy movie count =", len(fantasy))
print()
dict = {}
dict['Film'] = 'The Shape of Water'
dict['Genre'] = 'Fantasy'
dict['Lead Studio'] = 'Fox'
dict['Audience score %'] = '72'
dict['Profitability'] = '9.765'
dict['Rotten Tomatoes %'] = '92'
dict['Worldwide Gross'] = '195.3'
dict['Year'] = '2017'
movies_table.insert_row(dict)
fantasy = movies_table.filter(lambda x: x["Genre"] == "Fantasy").table
print("Now fantasy movie count =", len(fantasy))
print()
movies_table.update_row('Film', 'A Serious Man', 'Year', '2022')
print(movies_table.table)
print()
asm = movies_table.filter(lambda x: x["Film"] == "A Serious Man").table
print(asm)
```