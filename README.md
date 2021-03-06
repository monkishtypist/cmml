# CMML
CMML (content modeling markup language) is a general-purpose language (GPL) intended for use in content modeling. CMML can also be called a Content Management Modeling Language, and be used for modeling content management systems (CMS) architecture.

> CMML is influenced heavily by [DBML](https://github.com/holistics/dbml) and work done by the [Holistics](https://github.com/holistics) team. Thanks!

The *goal* of CMML is to simplify content modeling for developers. As such it is intended to be used along side design or functional spec to help identify content needs, structure, and relationships in web design and development.

> CMML is considered very pre-beta at the moment.

### Explore CMML

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
- a content type is made up of a collections of `field`s or `field_group`s

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
field_name field_type
    field_attribute
        field_attribute_option
```

- `field_name` is the name of the field and may be aliased with `field_name as 'Field Name'`
- `field_type` defines the type of field, such as `text` or `boolean`
- `field_attribute` defines a field's value(s) and other options
- `field_attribute_option`s are the available options for each attribute

### Field Types

Field types define what data a field can contain, and how that data should be structured. Some field types also include sub-types which define the field values even further. All fields also support [field attributes](#field-attributes) as described below.

Primary types:

- [`boolean`](docs/fieldTypes.md?#boolean) - a logical binary operator `0/1` or `yes/no`
- [`date`](docs/fieldTypes.md?#date) - a date-time or part thereof
- [`duration`](docs/fieldTypes.md?#duration) - a time interval or length
- [`media`](docs/fieldTypes.md?#media) - a media object or objects, such as an image, video, file, or document
- [`number`](docs/fieldTypes.md?#number) - a number or range of numbers
- [`reference`](docs/fieldTypes.md?#reference) - a reference to one or more content types or taxonomies
- [`select`](docs/fieldTypes.md?#select) - a choice between one or more options
- [`text`](docs/fieldTypes.md?#text) - a text string

Shorthand types:

- [`document`](docs/fieldTypes.md?#media) - shorthand for `media` with pre-defined document `file_type`s
- [`email`](docs/fieldTypes.md?#email) - shorthand for `text` with pre-defined `format`
- [`image`](docs/fieldTypes.md?#media) - shorthand for `media` with pre-defined image `file_type`s
- [`password`](docs/fieldTypes.md?#password) - shorthand for `text` with pre-defined `format`
- [`time`](docs/fieldTypes.md?#time) - shorthand for `date` formatted for time
- [`video`](docs/fieldTypes.md?#media) - shorthand for `media` with pre-defined video `file_type`s

### Field Attributes

Field attributes are settings or restrictions for a specific field type, and are used to define the data within a field type.

- [`choices`](docs/fieldAttributes.md?#choices) define selectable options
- [`default`](docs/fieldAttributes.md?#default) defines the default field value
- [`file_size`](docs/fieldAttributes.md?#file_size) defines the allowed file size of media objects
- [`file_size_max`](docs/fieldAttributes.md?#file_size_max) defines the maximum file size
- [`file_size_min`](docs/fieldAttributes.md?#file_size_min) defines the minimum file size
- [`file_type`](docs/fieldAttributes.md?#file_type) defines the allowed file types of a media objecy
- [`format`](docs/fieldAttributes.md?#format) defines the data format
- [`length`](docs/fieldAttributes.md?#length) defines the length of field data
- [`limit`](docs/fieldAttributes.md?#limit) defines the maximum value and is shorthand for a `range`'s upper value
- [`max`](docs/fieldAttributes.md?#max) defines the maximum field value or count
- [`min`](docs/fieldAttributes.md?#min) defines the minimum field value or count
- [`range`](docs/fieldAttributes.md?#range) is shorthand for `min`/`max`
- [`required`](docs/fieldAttributes.md?#required) defines a field as having a reuquired value
- [`scale`](docs/fieldAttributes.md?#scale) defines the number of decimal places in a number field
- [`target`](docs/fieldAttributes.md?#target) defines a reference target content type or taxonomy

## Taxonomy Definition

Taxonomies are used to group content types.

For example, you might want to group all products that are currently on sale and show them on a Sale page on your website. To do that you would create a Taxonomy grouping those product like so:

```
Tax sale_products as 'Products Currently on Sale'
    product
        'and'
        is_on_sale
        sale_price
            '>'
            0
```

[Click here to learn more about Taxonomy Definition](docs/taxonomyDefinition.md) and how to define your own custom taxonomies.

## Kitchen Sink

The following is an example of a *Product* content type definition:

```
Type product as 'Sample Product'
    id as 'SKU' number
        required
        unique
    title as 'Product Name' text
        length
            100
        notes
            Product names are limited to 100 characters
    description text
        supports
            wysiwyg
    size select
        choices
            NULL
            s as 'small'
            m as 'medium'
            l as 'large'
    featured_image as 'Featured Image' image
        file_size
            1MB
        required
    product_images as 'Slider Images' image
        file_size
            1MB
        min
            5
        notes
            Products must have at least 5 slider images or no images
    product_brochure as 'Brochure' document
        max
            1
        notes
            Products have the option for a brochure document
    price currency
        required
    sale_price currency
    is_on_sale as 'On Sale' boolean
        default
            false
    related_products reference
        target
            product
        max
            2
```

Alternately we can write our definitions in object/array format for compactness:
```
Type product as 'Sample Product'
    id as 'SKU' number [required, unique]
    title as 'Product Name' text [{length: 100}, {notes: 'Product names are limited to 100 characters'}]
    description text [{supports: wysiwyg}]
    size select [{choices: [NULL, s as 'small', m as 'medium', l as 'large']}]
    featured_image as 'Featured Image' image [{file_size: 1MB}, {required}]
    product_images as 'Slider Images' image [{file_size: 1MB}, {min: 5}, {notes: 'Products must have at least 5 slider images or no images'}]
    product_brochure as 'Brochure' document [{max: 1}, {notes: 'Products have the option for a brochure document'}]
    price currency [required]
    sale_price currency []
    is_on_sale as 'On Sale' boolean [{default: false}]
    related_products reference [{target: product}, {max: 2}]
```
