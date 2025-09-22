# Transpiler Config API

## name_map.json

location: `.pypp/transpiler_config/name_map.json`

```json
{
    "to_string": to_string_object or null or unspecified,
    "custom_mapping": custom_mapping_object or null or unspecified,
    "custom_mapping_starts_with": custom_mapping_object or null or unspecified
}
```

## call_map.json

```json
{
    "left_and_right": left_and_right_object or null or unspecified,
    "to_string": to_string_object or null or unspecified,
    "custom_mapping": custom_mapping_object or null or unspecified,
    "custom_mapping_starts_with": custom_mapping_object or null or unspecified
}
```

## attr_map.json

```json
{
    "to_string": to_string_object or null or unspecified,
    "custom_mapping": custom_mapping_object or null or unspecified,
    "custom_mapping_starts_with": custom_mapping_object or null or unspecified
}
```

## ann_assign_map.json

```json
{
    "custom_mapping": custom_mapping_object or null or unspecified,
    "custom_mapping_starts_with": custom_mapping_object or null or unspecified
}
```

## always_pass_by_value.json

```json
{
    "any_string": required_py_import_object or null
}
```

## subscriptable_types.json

```json
{
    "any_string": required_py_import_object or null
}
```

## cmake_lists.json

```json
{
    "add_lines": list of strings or null or unspecified,
    "link_libraries": list of strings or null or unspecified,
}
```

## to_string_object

```json
{
    "any_string": {
        "to": string,
        "quote_includes": list of strings or null or unspecified,
        "angle_includes": list of strings or null or unspecified,
        "required_py_import": required_py_import_object or null or unspecified
    }
}
```

## custom_mapping_object

```json
{
    "any_string": {
        "mapping_function": string,
        "quote_includes": list of strings or null or unspecified,
        "angle_includes": list of strings or null or unspecified,
        "required_py_import": required_py_import_object or null or unspecified
    }
}
```

## left_and_right_object

```json
{
    "any_string": {
        "left": string,
        "right": string,
        "quote_includes": list of strings or null or unspecified,
        "angle_includes": list of strings or null or unspecified,
        "required_py_import": required_py_import_object or null or unspecified
    }
}
```

## required_py_import_object

```json
{
    "name": string,
    "module": string
}
```