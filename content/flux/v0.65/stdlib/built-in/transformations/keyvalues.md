---
title: keyValues() function
description: The keyValues() function returns a table with the input table's group
  key plus two columns, _key and _value, that correspond to unique column + value
  pairs from the input table.
aliases:
  - /flux/v0.65/functions/transformations/keyvalues
  - /flux/v0.65/functions/built-in/transformations/keyvalues/
menu:
  flux_0_65:
    name: keyValues
    parent: Transformations
    weight: 1
---

The `keyValues()` function returns a table with the input table's group key plus two columns,
`_key` and `_value`, that correspond to unique column + value pairs from the input table.

_**Function type:** Transformation_  
_**Output data type:** Object_

```js
keyValues(keyColumns: ["usage_idle", "usage_user"])

// OR

keyValues(fn: (schema) => schema.columns |> filter(fn: (r) =>  r.label =~ /usage_.*/))
```

## Parameters

> `keyColumns` and `fn` are mutually exclusive. Only one may be used at a time.

### keyColumns
A list of columns from which values are extracted.
All columns indicated must be of the same type.
Each input table must have all of the columns listed by the `keyColumns` parameter.

_**Data type:** Array of strings_

### fn
Function used to identify a set of columns.
All columns indicated must be of the same type.

_**Data type:** Function_

{{% note %}}
Make sure `fn` parameter names match each specified parameter.
To learn why, see [Match parameter names](/flux/v0.65/language/data-model/#match-parameter-names).
{{% /note %}}

## Additional requirements

- Only one of `keyColumns` or `fn` may be used in a single call.
- All columns indicated must be of the same type.
- Each input table must have all of the columns listed by the `keyColumns` parameter.

## Examples

##### Get key values from explicitly defined columns
```js
from(bucket: "telegraf/autogen")
  |> range(start: -30m)
  |> filter(fn: (r) => r._measurement == "cpu")
  |> keyValues(keyColumns: ["usage_idle", "usage_user"])
```

##### Get key values from columns matching a regular expression
```js
from(bucket: "telegraf/autogen")
  |> range(start: -30m)
  |> filter(fn: (r) => r._measurement == "cpu")
  |> keyValues(fn: (schema) => schema.columns |> filter(fn: (r) =>  r.label =~ /usage_.*/))
```

<hr style="margin-top:4rem"/>

##### Related InfluxQL functions and statements:
[SHOW MEASUREMENTS](/influxdb/latest/query_language/schema_exploration/#show-measurements)  
[SHOW FIELD KEYS](/influxdb/latest/query_language/schema_exploration/#show-field-keys)  
[SHOW TAG KEYS](/influxdb/latest/query_language/schema_exploration/#show-tag-keys)  
[SHOW TAG VALUES](/influxdb/latest/query_language/schema_exploration/#show-tag-values)  
[SHOW SERIES](/influxdb/latest/query_language/schema_exploration/#show-series)  
