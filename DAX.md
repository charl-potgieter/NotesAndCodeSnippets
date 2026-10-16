#  A  list of my commonly utilised DAX measures

#### Check for items in Fact table not in lookup table

```
Error check (missing lookups) - values in FactTable not In LookupTable:= VAR _tbl = 
    EXCEPT(VALUES(FactTable[LookupField]), VALUES(LookupTable[LookupField]))

VAR _ReturnValue = 
    CONCATENATEX(_tbl, FactTable[LookupField] & "|" )

RETURN
    _ReturnValue
```
