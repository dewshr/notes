# jq - a tool for manipulating JSON

- [jq - a tool for manipulating JSON](#jq---a-tool-for-manipulating-json)
  - [Selecting Attributes](#selecting-attributes)
  - [Formatting Output](#formatting-output)
  - [Editing JSON](#editing-json)

JSON (JavaScript Object Notation) is a common structured data format. It is often used as a replacement for XML, to transmit data between a server and a browser. JSON transmits human-readable data as objects composed of attribute-value pairs.

Example JSON (https://www.w3schools.com/Js/json_demo.txt):

```json
{
    "name":"John",
    "age":31,
    "pets":[
        { "animal":"dog", "name":"Fido" },
        { "animal":"cat", "name":"Felix" },
        { "animal":"hamster", "name":"Lightning" }
    ]
}
```

Here, we take a look at the **jq** UNIX tool for manipulating JSON data. 

To get started, we can pipe the json text above into `jq` using `curl`:

```bash
curl -s https://www.w3schools.com/Js/json_demo.txt | jq
```

## Selecting Attributes

Selecting a single scalar value:

```bash
 curl -s https://www.w3schools.com/Js/json_demo.txt | jq '.name'
```

```json
"John"
```

Selecting multiple values:

```bash
 curl -s https://www.w3schools.com/Js/json_demo.txt | \
 jq '.name, .age'
```

```json
"John"
31
```

Selecting a single value from an array (index 0 returns the first element)

```bash
curl -s https://www.w3schools.com/Js/json_demo.txt | jq '.pets[0]'
```

```json
{
  "animal": "dog",
  "name": "Fido"
}
```

More complicated queries can be achieved by combining dot operators, or using a pipe "|".

```bash
# Multiple dot operations
curl -s https://www.w3schools.com/Js/json_demo.txt | \
jq '.pets[0].name'

# Same query using a pipe
curl -s https://www.w3schools.com/Js/json_demo.txt | \
jq '.pets[0] | .name'

```

```json
"Fido"
```

Selecting elements from an array when an attribute matches a condition:

```bash
curl -s https://www.w3schools.com/Js/json_demo.txt | \
jq '.pets | map(select(.animal == "hamster"))'
```

```json
[
  {
    "animal": "hamster",
    "name": "Lightning"
  }
]
```

Select also supports boolean operators:

```bash
curl -s https://www.w3schools.com/Js/json_demo.txt | \
jq '.pets | map(select(.animal == "hamster" or .animal == "cat"))'
```

```json
[
  {
    "animal": "cat",
    "name": "Felix"
  },
  {
    "animal": "hamster",
    "name": "Lightning"
  }
]
```


## Formatting Output

Selected attributes can be formatted:

```bash
curl -s https://www.w3schools.com/Js/json_demo.txt | \
jq '.name + " is " + (.age | tostring)'
```

```json
"John is 31"
```

## Editing JSON

Replace a value:

```bash
curl -s https://www.w3schools.com/Js/json_demo.txt | \
jq ' .name = "Jessica" '
```

```json
i{
  "name": "Jessica",
  "age": 31,
  "pets": [
    {
      "animal": "dog",
      "name": "Fido"
    },
    {
      "animal": "cat",
      "name": "Felix"
    },
    {
      "animal": "hamster",
      "name": "Lightning"
    }
  ]
}
```

Adding a new key-value pair:

```bash
curl -s https://www.w3schools.com/Js/json_demo.txt | \
jq ' .pets += [{ "animal" : "piggy", "name": "Chrorizo" }] '
```

```json
{
  "name": "John",
  "age": 31,
  "pets": [
    {
      "animal": "dog",
      "name": "Fido"
    },
    {
      "animal": "cat",
      "name": "Felix"
    },
    {
      "animal": "hamster",
      "name": "Lightning"
    },
    {
      "animal": "piggy",
      "name": "Chrorizo"
    }
  ]
}
```
More extensive modifications:

```bash
curl -s https://www.w3schools.com/Js/json_demo.txt | \
jq ' .pets += [{ "animal" : "piggy", "name": "Chrorizo" } ] | 
     .pets[0] += {"breed" : "Chihuahua" } |
     .name = "Ana" |
     .age = 28 |
     .hobbies = "ukulele" '
```

```json
{
  "name": "Ana",
  "age": 28,
  "pets": [
    {
      "animal": "dog",
      "name": "Fido",
      "breed": "Chihuahua"
    },
    {
      "animal": "cat",
      "name": "Felix"
    },
    {
      "animal": "hamster",
      "name": "Lightning"
    },
    {
      "animal": "piggy",
      "name": "Chrorizo"
    }
  ],
  "hobbies": "ukulele"
}
```
