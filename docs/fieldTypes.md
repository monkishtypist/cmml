# CMML Field Types

The following fields are used to define data.

- [`boolean`](#boolean) - a logical binary operator `0/1` or `yes/no`
- [`currency`](#currency) - shorthand for `number`
- [`date`](#date) - a date-time or part thereof
- [`document`](#document) - shorthand for `media`
- [`duration`](#duration) - a time interval or length
- [`email`](#email) - an email address
- [`image`](#image) - shorthand for `media`
- [`location`](#location) - location data
- [`media`](#media) - a media object or objects, such as an image, video, file, or document
- [`number`](#number) - a number or range of numbers
- [`password`](#password) - a concealed or encrypted string
- [`reference`](#reference) - a reference to one or more content types or taxonomies
- [`select`](#select) - a choice between one or more options
- [`text`](#text) - a text string
- [`time`](#time) - shorthand for `date` formatted for time
- [`video`](#video) - shorthand for `media`

## `boolean`

A logical operator used for `yes/no` or `on/off` or similar logic.

- similar to a `select` field with exactly two options

### Attributes

- `default` (default: `0`)
    - `true`/`false` or `yes`/`no` can be used in place of `1`/`0` respectively

### Examples

Define an option to show a featured image by default:

```
show_featured as 'Show Featured Image' boolean
    default
        true
```

## `currency`

Shorthand for [`number`](#number)

- currency has two decimal places
- currency cannot be negative

```
number
    scale
        2
    min
        0
```

## `date`

A date-time field.

### Attributes

- `format` (default: `YYYY-MM-DDThh:mm:ss+00:00`)

### Examples

Define a published-on date:

```
publish_date as 'Published On' date
    format: YYYY-MM-DD
```

## `document`

Shorthand for [`media`](#media)

```
media
    file_type
        doc
        docx
        pdf
        ...
```

## `duration`

A time interval field.

### Attributes

- `format` (default: `PnYnMnWnDTnHnMnS`)
- `min`
- `max`

### Examples

Define the duration of an event in hours and minutes:

```
event_duration duration
    format
        PTnHnM
```

Define a duration of at least 1 day but no longer than 1 week:

```
limited_time duration
    format
        PnWnD
    max
        P1W0D
    min
        P0W1D
```

## `email`

Shorthand for [`text`](#text)

```
text
    format
        {email address}
```

## `image`

Shorthand for [`media`](#media)

```
media
    file_type
        gif
        jpg
        png
        svg
        ...
```

## `location`

A location object.

### Attributes

- `lat` (required)
- `long` (required)

or

- `latlong`

### Examples

Define a location on a map:

```
location
    lat
        32.715736
    long
        -117.161087
```

or

```
location
    latlong
        32.715736
        -117.161087
```

## `media`

A media object or objects, such as an image, video, document, or other file type.

### Attributes

- `file_size`
- `file_type`
- `limit`
- `max`
- `min`

### Examples

Define a brochure download field:

```
brochure media
    file_type
        pdf
        doc
        docx
    limit
        1
```

Define a product images field for one to five images:

```
product_images as 'Product Images' media
    file_type
        jpg
        png
    min
        1
    max
        5
```

## `number`

A number field.

### Attributes

- `min`
- `max`
- `range`
- `scale`

### Examples

Define a whole number greater than or equal to zero:

```
number
    min
        0
```

Define a decimal number with two decimal places:

```
number
    scale
        2
```

Define a number between one and ten:

```
number
    range
        1
        10
```

## `password`

Shorthand for [`text`](#text)

```
text
    format
        ^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])(?=.*[@#$%^&-+=()])(?=\\S+$).$
```

## `reference`

A reference to another content type or taxonomy.
- can be a single reference, or
- multiple references to the same content type or taxonomy
- references to multiple content types is not supported (instead use a taxonomy)

### Attributes

- `target` (required)
- `limit`
- `min`
- `max`
- `range`

### Examples

Define related news articles with up to 2 choices:

```
related as 'Related News' reference
    target
        news_article
    max
        2
```

## `select`

A selectable list.

### Attributes

- `choices` (required)
- `limit`
- `max`
- `min`
- `range`

### Examples

Define a select option to pick one day of the week:

```
day as "Day of the Week" select
    choices
        sunday
        monday
        tuesday
        wednesday
        thrusday
        friday
        saturday
    limit
        1
```

> A note on `required`
>
> If a select field is marked as required, the first choice will be the default value (unless `default` is defined). If the first choice is `NULL` then the field *must* be changed to a non-null value.

## `text`

For text or string data. Examples might include a page title, author name, or body paragraphs.

### Attributes

- `length`
- `format`

### Examples

Define a title limited to 100 characters:

```
title as 'Title' text
    length
        100
```

Define an page's introductory paragraph:

```
intro as 'Introduction Paragraph' text/long
```

## `time`

Shorthand for [`date`](#date)

```
date
    format
        hh:mm:ss.sss
```

## `video`

Shorthand for [`media`](#media)

```
media
    file_type
        mov
        mp4
        ogg
        webm
        ...
```
