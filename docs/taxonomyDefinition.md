# Taxonomy Definition

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
- `Relation`s cannot be used on `Type` - instead, if multiple `Type`s are included the relationship is always considred to be `'or'`

For example, here is a Taxonomy that might define all news articles on the topic of sports or leisure:
```
Tax sportsAndLiesure as 'Sports and Leisure'
    news_article
        topic
            'or'
            sports
            leisure
```

To take it one step further, we might also include upcoming sporting and leisure events in the taxonomy like so:
```
Tax sports_and_leisure as 'Sports and Leisure'
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
