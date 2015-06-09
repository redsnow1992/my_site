What are `abstract`, `class` and `extends`?
`abstract` class introduces a datatype,
`class` introduces a variant, and
`extends` connects a variant to a datatype.

1. The First Bit of Advice
When specifying a collection of data,
use abstract classes for datatypes and
extended classes for variants.

2. The Second Bit of Advice
When writing a function over a
data type, place a method in each of the
variants that make up the datatype. If
a field of a variant belongs to the same
datatype, the method may call the
corresponding method of the field in
computing the function.
