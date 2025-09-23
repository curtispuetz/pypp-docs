# ann_assign_map.json - [api link](API.md/#ann_assign_mapjson)

ann_assign_map.json can only be used with the `custom_mapping` and `custom_mapping_starts_with` [options](mapping_functions.md).

If you put a file `.pypp/transpiler_config/ann_assign_map.json` in your project:

```json
{
    "custom_mapping": {
        "CustomType[int]": {
            "mapping_function": "ann_assign.py"
        }
    }
}
```

```python
# ann_assign.py
def mapping_function(
    _type_cpp: str, target_str: str, _value_str: str, value_str_stripped: str
):
    return (
        "CustomType<int> "
        + target_str
        + "="
        + "CustomType<int>("
        + value_str_stripped
        + ")"
    )

```

then, when you write an annotated assignment, like `a: CustomType[int] = CustomType(4, 2)`, it will be converted to `CustomType<int> a = CustomType<int>(4, 2)` in the generated C++ code.
