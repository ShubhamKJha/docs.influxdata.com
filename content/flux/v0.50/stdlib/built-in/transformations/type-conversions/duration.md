---
title: duration() function
description: The `duration()` function converts a single value to a duration.
menu:
  flux_0_50:
    name: duration
    parent: built-in-type-conversions
weight: 2
aliases:
  - /flux/v0.50/functions/built-in/transformations/type-conversions/duration/
---

The `duration()` function converts a single value to a duration.

_**Function type:** Type conversion_  
_**Output data type:** Duration_

```js
duration(v: "1m")
```

## Parameters

### v
The value to convert.

## Examples

{{% note %}}
Flux does not support duration column types.
The example below converts an integer to a duration and stores the value as a string.
{{% /note %}}

```js
from(bucket: "sensor-data")
  |> range(start: -1m)
  |> filter(fn:(r) => r._measurement == "system" )
  |> map(fn:(r) => ({ r with uptime: string(v: duration(v: r.uptime)) }))
```
