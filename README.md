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
Type news_article as 'News Article'
    id integer
        unique
        programmatic
    published dateTime
        programmatic
    title text
        limit: 100
    author reference
        target: author
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

- [`boolean`](#boolean) - a logical binary operator `0/1` or `yes/no`
- [`date`](#date) - a date-time or part thereof
- [`duration`](#duration) - a time interval or length
- [`email`](#email) - an email address
- [`media`](#media) - a media object or objects, such as an image, video, file, or document
- [`number`](#number) - a number or range of numbers
- [`password`](#password) - a concealed or encrypted string
- [`reference`](#reference) - a reference to one or more content types or taxonomies
- [`select`](#select) - a choice between one or more options
- [`text`](#text) - a text string
- [`time`](#time) - shorthand for `date` formatted for time

### Field Attributes
Each field can have various attributes attached which modify or limit the field values.

`choices` *(array)*
- an array of available choices or options
- the first choice in the array is the default choice
`format` *(string)*
- a string format defining the field value
- especially for date and time formatting
`file_type` *(string or array)*
- limit values to a single file type extension, or an array of file type extensions
`file_size_min` *(float)*
- the minimum file size for media fields
`file_size_max` *(float)*
- the maximum file size for media fields
`file_size` *(array)*
- an array representing min and max file sizes for media fields

#### `boolean`
A logical operator used for `yes/no` or `on/off` or similar logic. This can be considered a type of `select` field with exactly two options.

**Shorthand:**
- `bool`

**Attributes:**
- `choices: [0, 1]` *(optional)*
    - default: `choices: [0, 1]`
    - alternate choice values can include: `[no, yes]`, `[on, off]`

```
published boolean
    choices: [no, yes]
```

#### `date`
A date-time field.

**Attributes:**
- `format: YYYY-MM-DDThh:mm:ss+00:00` *(optional)*
    - default: `YYYY-MM-DDThh:mm:ss+00:00`
    - date format using ISO 8601 standard

```
publish_date date
    format: YYYY-MM-DD
```

#### `duration`
A time interval (duration) field.

**Attributes:**
- `format: PnYnMnDTnHnMnS` *(optional)*
    - default: `PnYnMnDTnHnMnS`
    - duration format in ISO 8601 format

```
event_duration duration
    format: PTnHnM
```

#### `email`
An email address field.

```
email email
```

#### `media`
A media object or objects, such as an image, video, document, or other file type.

**Sub-types:**
- [`file`](#file) (default) - any file type
- [`document`](#document) - a document file, such as a PDF
- [`image`](#image) - an image file
- [`video`](#video) - a video file

**Attributes:**
- [`file_type`](#file_type)
- [`file_size`](#file_size)

##### `file`
Any file type.

```
download media
```

##### `document`
A document file

```
brochure media/document
```

##### `image`
An image

```
featured_image media/image
```


#### `number`
A number field.

**Sub-types:**
- [`integer`](#integer) (default)
- [`decimal`](#decimal)

**Attributes:**
- `min` *(integer or float, optional)* - the minimum field value
    - default: `no minimum`
- `max` *(integer or float, optional)* - the maximum field value
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
- `scale` *(integer, optional)* - the total number of digits after the decimal point
    - default: `2`

```
price number/decimal
    min: 0.00
    scale: 2
```

#### `password`
**Sub-types:**

**Attributes:**

#### `reference`
A reference to another content type or taxonomy.

**Sub-types:**

**Attributes:**
- `target: content_type` *(type or tax, required)* - reference target can be a type or tax.
- `quantity_min` or `qty_min` *(integer, optional)* - the minimum number of objects to be referenced
    - default: `0`
- `quantity_max` or `qty_max` *(integer, optional)* - the maximum number of objects to be referenced
    - default: `infinite`
- `quantity` or `qty` *(integer or array, optional)* - the min/max number of objects to be referenced
    - default: `[0, infinit]`
    - an array of the same two integers such as `[2, 2]` sets the required amount at that number
    - a single integer can be used in place of an array, such as `2` instead of `[2, 2]`

```
related_news reference
    target: news_article
    qty: [1, 2]
```

#### `select`
**Sub-types:**
- [`single`](#single)
- [`multi`](#multi)

**Attributes:**

##### `single`
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
    - default: `hh:mm:ss.sss`

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
