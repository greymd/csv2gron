
# csv2gron

csv2gron is a tool to convert CSV files to [gron](https://github.com/tomnomnom/gron) format.

```
Usage: csv2gron [options] -f <file>
```

Read CSV file from stdin and write gron format to stdout.
Header starts with a dot `.` to indicate the path in the JSON object.

## Example

test.csv
```
.name,.age,.address[0],.address[1],.address[2]
Alice,30,123 Main St,My City,My State
Bob,40,456 Elm St,Your City,Your State
```

```
$ cat input.csv | python3 ../csv2gron
json[0].name = "Alice";
json[0].age = "30";
json[0].address[0].city = "123 Main St";
json[0].address[0] = "My City";
json[0].address[1] = "My State";
json[1].name = "Bob";
json[1].age = "40";
json[1].address[0].city = "456 Elm St";
json[1].address[0] = "Your City";
json[1].address[1] = "Your State";
```

`gron --ungron` command can convert the output to JSON format.

```
$ cat input.csv | python3 ../csv2gron | gron --ungron
[
  {
    "address": [
       "123 Main St",
       "My City",
       "My State"
    ],
    "age": "30",
    "name": "Alice"
  },
  {
    "address": [
       "456 Elm St",
       "Your City",
       "Your State"
    ],
    "age": "40",
    "name": "Bob"
  }
]
```

-l option can specify the root object name.

```
$ cat input.csv | python3 ../csv2gron -l people
json.people[0].name = "Alice";
json.people[0].age = "30";
json.people[0].address[0] = "123 Main St";
json.people[0].address[1] = "My City";
json.people[0].address[2] = "My State";
json.people[1].name = "Bob";
json.people[1].age = "40";
json.people[1].address[0] = "456 Elm St";
json.people[1].address[1] = "Your City";
json.people[1].address[2] = "Your State";
```


