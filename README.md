# CMML
CMML (content modeling markup language) is a general-purpose language (GPL) intended for use in content modeling. CMML can also be called a Content Management Modeling Language, and be used for modeling content management systems (CMS) architecture.

- [Project Definition](#project-definition)
- [Content Type Definition](#content-type-definition)
- [Field Definition](#content-type-definition)
- [Taxonomy Definition](#taxonomy-definition)

## Project Definition
Define the overall content model project.

```
Project project_name
    meta_field
        meta_value
```

- Project metadata is defined by `meta_field` and `meta_value`

Example:

```
Project mySite
    description
        This is a short description of my website
    url
        https://mysite.com
```

### Data Aliases
Most data can be aliased by using the `as` operator. For examle:

    data as 'Data Alias'

Extrapolating that format to our Project definition we have:

```
Project my_site as 'My Website'
    description as 'Site Description'
        This is a short description of my website
```

- aliases that include spaces or non-text characters must be encapsulated by quotes `''`

## Content Type Definition
Content types define a collection of data, such as a Page, Article, Event, or Author.

```
Type type_name as 'Type Name'
    field_name field_type
        field_attributes
```

- the name of the content type is defined by `type_name`
- a content type is made up of a collections of `field`s

An example of a news article content type might look something like this:
```
Type newsArticle as 'News Article'
    id integer
        unique
        programmatic
    published dateTime
        programmatic
    title text
        limit: 100
    author reference
        reference: author
    body longtext
```

## Field Definition
Fields are data sets defined most simply as `key:value` pairs.

```
field_name field_type/field_sub_type
    field_attributes
```

- `field_name` is the name of the field and may be aliased with `field_name as 'Field Name'`
- the type of field, such as `text` or `boolean` is defined by `field_type`
- `field_sub_type` is an optional qualifier that modifies the `field_type`
- `field_sub_type` qualifiers are unique to each `field_type` (see [Field Types](#field-types)) below)
- a field's value(s) and other options are defined in `field_attributes`
- along with supporting HTML field attributes, `field_attributes` also includes unique attributes for each field defined below

### Field Types
Field types define what data a field can contain, and how that data should be structured.

#### `boolean`
A logical operator used for `yes/no` or `on/off` or similar logic. This can be considered a type of `select` field with exactly two options.

**Attributes:**
- `choices: [0, 1]` *(array, optional)* - possible choices
    - default is `choices: [0, 1]`
    - alternate choice values can include: `[no, yes]`, `[on, off]`
    - the first element of the array is the default value, so `choices: [no, yes]` will default to `'no'`

```
published boolean
    choices: [no, yes]
```

#### `bool`
Shorthand for [`boolean`](#boolean).

```
published bool
    choices: [no, yes]
```

#### `date`
A date-time field.

**Attributes:**
- `format: YYYY-MM-DDThh:mm:ss+00:00` *(string, optional)* - date format using ISO 8601 standard
    - default value: `YYYY-MM-DDThh:mm:ss+00:00`

```
publish_date date
    format: YYYY-MM-DD
```

#### `duration`
A time interval (duration) field.

**Attributes:**
- `format: PnYnMnDTnHnMnS` *(string, optional)* - duration format in ISO 8601 format
    - default value: `PnYnMnDTnHnMnS`

```
event_duration duration
    format: PTnHnM
```

#### `email`
An email address field.

**Attributes:**

#### `number`
A number field.

**Sub-types:**
- [`integer`](#integer) (default)
- [`decimal`](#decimal)

**Attributes:**
- `min: n` *(integer or float, optional)* - the minimum field value
    - default: `no minimum`
- `max: n` *(integer or float, optional)* - the maximum field value
    - default: `no maximum`

##### `integer`
A whole number.

- `min/max` attributes support `integer` values

```
count number
    min: 0
```

##### `decimal`
A decimal number.

- `min/max` attributes support `float` values

**Attributes:**
- `scale: n` *(integer, optional)* - the total number of digits after the decimal point
    - default value: `2`

```
price number/decimal
    min: 0.00
    scale: 2
```

#### `media`
**Sub-types:**

**Attributes:**

#### `password`
**Sub-types:**

**Attributes:**

#### `reference`
**Sub-types:**

**Attributes:**

#### `select`
**Sub-types:**

**Attributes:**

##### `single` (or null)
##### `multi`

#### `text`
For text or string data. Examples might include a page title, author name, or body paragraphs.

**Sub-types:**
- [`short`](#short) (default)
- [`long`](#long)

**Attributes:**

##### `short`
A short text field for titles, names, and similar.

```
title as 'Title' text/short
```

##### `long`
A long text field for paragraphs of text or HTML markup. Long text could represent a *wysiwyg* field for example. Any time input would exceed multiple lines or require lne breaks, long text would be used.

    intro as 'Introduction Paragraph' text/long

#### `time`
Shorthand for [`date`](#date), but with a default format for time only.

**Attributes:**
- `format: hh:mm:ss.sss` *(string, optional)*
    - default value: `hh:mm:ss.sss`

```
start_time as 'Event Start Time' time
    format: hh:mm
```

## Taxonomy Definition
Taxonomies are used to group content by a specific classification, attribute, or data value.

```
Tax tax_name as 'Taxonomy Name'
    Type
        Field
            Relation
            Value
```

- content types included in a taxonomy are defined by `Type`
- multiple `Type`s may be included in a single taxonomy
- the data used to define if a content type is included in a a taxonomy is defined by the `Field` and its `Value`s
- `Relation`s can be used to define `Field`s or `Value`s with `'and'`, `'or'`, `'>'`, `'<'`, and more (see [Relations](#relations))
- `Relation`s cannot be used on `Type` - instead if multiple `Type`s are included the relationship is always considred to be `'or'`

For example, here is a Taxonomy that might define all news articles on the topic of sports or leisure:
```
Tax sportsAndLiesure as 'Sports and Leisure'
    newsArticle
        topic
            'or'
            sports
            leisure
```

To take it one step further, we might also include upcoming sporting and leisure events in the taxonomy like so:
```
Tax sportsAndLiesure as 'Sports and Leisure'
    newsArticle
        topic
            'or'
            sports
            leisure
    event
        'and'
        type
            'or'
            sports
            leisure
        start_time
            '>'
            now()
```
