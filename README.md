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

### Aliases
Most data can be aliased by using the `as` operator. For examle:

```
Project mySite as 'My Website'
    description as 'Site Description'
        This is a short description of my website
```

- aliases that include spaces or non-text characters must be encapsulated by quotes `''`

## Content Type Definition
Content types define a collection of data, such as a Page, Article, Event, or Author.

```
Type type_name as 'Type Name'
    field field_type
        field_options
```

- name of the content type is defined by `type_name`
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
        target: author
    body longtext
```

## Field Definition
Fields are data sets with `key:value` pairs.

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
        dateTime
            '>'
            now()
```
