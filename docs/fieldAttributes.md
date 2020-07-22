# CMML Field Attributes
Each field type supports various attributes which modify or restrict the field values. Some field attributes are global and can apply to any field, whereas other attributes are specific to one or more field types.

- global field attributes will be marked as **[global]**
- each field attribute will show how many arguments are required or permitted, indicated by a fixed number or a range
    - fixed: *(1)*
    - range: *(1 ... 10)*
    - `n` represents any number greater than zero: *(1 ... n)*

**Available Attributes**

- [`choices`](#choices)
- [`default`](#default) **[global]**
- [`file_size`](#file_size)
- [`file_size_max`](#file_size_max)
- [`file_size_min`](#file_size_min)
- [`file_type`](#file_type)
- [`format`](#format)
- [`lat`](#lat)
- [`latlong`](#latlong)
- [`length`](#length)
- [`limit`](#limit)
- [`long`](#long)
- [`max`](#max)
- [`min`](#min)
- [`notes`](#notes) **[global]**
- [`range`](#range)
- [`required`](#required) **[global]**
- [`scale`](#scale)
- [`supports`](#supports)
- [`target`](#target)
- [`unique`](#unique) **[global]**

## `choices`
*(1 ... n)*

- an array of available choices or options
- the first choice in the array is the default
    - you can use `NULL` to represent an empty or unselectable default choice

```
choices
    NULL
    option1 as 'Option One'
    option2 as 'Option Two'
    option3 as 'Option Three'
```

## `default`
*(1 ... n)*

- define the default value or values of a field

```
default
    This is some default text
```

## `file_size`
*(1 ... 2)*

- the min and max file sizes for media fields
- if a single value is given that is the max file size, and the min file size is zero (shorthand for `file_size_max`)
- size definitions require units of measurement, for example: `kB`, `MB`, `GB`, or `TB`
    - a size of zero does not require a unit of measurement
- negative sizes are not supported

```
file_size
    10MB
```

```
file_size
    1MB
    10MB
```

## `file_size_max`
*(1)*

- the maximum file size for media fields
- requires a unit of measurement
- if not defined, then there is no maximum

```
file_size_max
    10MB
```

## `file_size_min`
*(1)*

- the minimum file size for media fields
- requires a unit of measurement
- if not defined, the minimum is zero

```
file_size_min
    100kB
```

## `file_type`
*(1 ... n)*

- limit values to one or more file types by extension
- see this [list of file formats](https://en.wikipedia.org/wiki/List_of_file_formats) for more information

```
file_type
    jpg
    jpeg
    png
```

## `format`
*(1)*

- a string format defining the field value
- for date and time formatting (example `YYYY-MM-DD`)
- for regular expression formatting (example `[a-zA-Z0-9].*`)

```
format
    YYYY-MM-DD
```

## `lat`
*(1)*

- a lattitudinal coordinate

```
lat
    32.715736
```

## `latlong`
*(2)*

- shorthand for `lat` and `long` combined

```
latlong
    32.715736
    -117.161087
```

## `length`
*(1 ... 2)*

- define the min and max length of text field
- if a single value is given, that is the max length and the min length is zero
- if two values given, the first is the min length, the second is the max length

```
length
    100
```

```
length
    8
    20
```

## `limit`
*(1)*

- limit the field value
- can be used to limit the following:
    - number of characters in a text, email, or password field
    - number of items selectable in a select field
    - number of reference items in a reference field

```
limit
    3
```

## `long`
*(1)*

- a longitudinal coordinate

```
long
    -117.161087
```

## `max`
*(1)*

- set the maximum field value
- sets the maximum value for a number field
- acts like `limit` on a select field

```
max
    10
```

## `min`
*(1)*

- set the minimum field value
- negative values supported on number fields
- acts like `range` minimum on a select field

```
min
    2
```

## `notes`
*(1 ... n)*

- add notes to a field

```
notes
    This field has notes
    Multiple lines of notes can be added
```

## `range`
*(2)*

- shorthand for `min` and `max` together
- can be used to define number of selectable choices, or
- set a range of values for number fields

```
range
    1
    10
```

## `required`
*(0 ... 1)*

- mark a field as required
- if a value is provided, that exact value is required (shorthand for `range: [value, value]`)
- if no value is provided, then any value greater than zero is required
- to require values in a range greater than one, use `required` and `range` together

```
required
```

```
required
    2
```

```
required
range
    1
    5
```

## `scale`
*(1)*

- define the number of digits after a decimal place

```
scale
    2
```

## `supports`
*(1 ... n)*

- the field supports custom features

```
supports
    html
    newline
    wysiwyg
    ...
```

## `target`
*(1)*

- reference target
- can be a content type or taxonomy

```
target
    taxonomy_name
```

## `unique`
*(0)*

- requires the field value to be unique

```
unique
```
