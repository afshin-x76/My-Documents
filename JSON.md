# Json In Python
##### First question is, what is Json and why should we use it?
- Json is a Syntax for storing and exchanging data
- Json is text and written with javascript object notation

Json is readable by any C-style Language, like pyton.


an example of Json File:
```
{
    "firstName": "Jane",
    "lastName": "Doe",
    "hobbies": ["running", "sky diving", "singing"],
    "age": 35,
    "children": [
        {
            "firstName": "Alice",
            "age": 6
        },
        {
            "firstName": "Bob",
            "age": 8
        }
    ]
}

```
Json supports primitive types like **strings** and **numbers** as lists and objects even.

Python comes with a built-in package called Json for encoding and decoding Json data.

The process of encoding JSON is called serialization and this term is refers for transformation data to a series of bytes to be stored or transmitted accros a network.and in the other hand we have deserialization that you can guess what is it.
## Serilizing Json
Json librarry use dump() method for writing data to files.

You can convert python objects to json 

|Python|Json|
|:-:|:-:|
|dict|object|
|list, tuple|array|
|str|string|
|int, float, long|number
|True, False|true, false
|None|null|

### Simple serilaliztion example

imagin that you'r working with python object and you want to save it to disk.
```
data = {
    "president": {
        "name": "Zaphod Beeblebrox",
        "species": "Betelgeusian"
    }
}
```
You can use **python context manager** for writing data to json files. for example:
```
with open("data_file.json", "w") as write_file:
    json.dump(data, write_file)
```
> JSON files conveniently end in a .json extension.

note that dump method takes 2 argument:
1. data object that must be serialized
2. file name

### Some Useful Arguments

- **indent**: `json.dump(data, indent=4)`
- **seperators**: `json.dump(data, separators=(',', ': ')`

> #### you can find more in [here](https://docs.python.org/3/library/json.html)

### Deserializing Json
> note: if you encode a data to JSON and decode it, you mat not get the same object back. it's look like to get friend to tranlate a word to japanese and another one tranlate it back to english. for example encoding a tuple and getting back a list after decoding

### A Simple Deserialization Example
This time we want to read from JSON file, lets use the same JSON file that we made in previuos example in here.
```
with open("data_file.json", "r") as read_file:
    data = json.load(read_file)
```
Note that we use `"r"` that's mean read mode and instead of `dump()` method we used `load()` method and for ther args we gave it only the file that we want to read(*we can use deserilize or decode terms too*)


[Resouce](https://realpython.com/python-json/)



