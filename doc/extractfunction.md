# extract()

Get a match for a [regular expression](./re2.md) from a text string. 

Optionally, it then converts the extracted substring to the indicated type.

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

**Syntax**

`extract(`*regex*`,` *captureGroup*`,` *text* [`,` *typeLiteral*]`)`

**Arguments**

* *regex*: A [regular expression](./re2.md).
* *captureGroup*: A positive `int` constant indicating the
capture group to extract. 0 stands for the entire match, 1 for the value matched by the first '('parenthesis')' in the regular expression, 2 or more for subsequent parentheses.
* *text*: A `string` to search.
* *typeLiteral*: An optional type literal (e.g., `typeof(long)`). If provided, the extracted substring is converted to this type. 

**Returns**

If *regex* finds a match in *text*: the substring matched against the indicated capture group *captureGroup*, optionally converted to *typeLiteral*.

If there's no match, or the type conversion fails: `null`. 

**Examples**

The example string `Trace` is searched for a definition for `Duration`. 
The match is converted to `real`, then multiplied it by a time constant (`1s`) so that `Duration` is of type `timespan`. In this example, it is equal to 123.45 seconds:

<!-- csl -->
```
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

This example is equivalent to `substring(Text, 2, 4)`:

<!-- csl -->
```
extract("^.{2,2}(.{4,4})", 1, Text)
```
