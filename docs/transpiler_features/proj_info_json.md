# proj_info.json

In your projects `.pypp` directory, you will have a `proj_info.json` file. There are configuration parameters in this file that control the Py++ transpiler behavior.

```json
{
    "namespace": "me",
    "override_cpp_write_dir": null,
    "write_metadata_to_dir": null,
    "ignored_files": [],
    "cmake_minimum_required_version": "4.0",
    "cpp_dir_is_dirty": false
}
```

# namespace

This option specifies the namespace that all your generated C++ code is wrapped in. 

Default: `"me"`

# ignored_files

This option allows you to specify Python files that the Py++ transpiler will ignore. It can be useful if you have some files that are a work in progress, but you still want to transpile/build/run your other code. Wildcard file matching is supported.

Default: `[]`

# override_cpp_write_dir

With this option, you can specify a different directory for the Py++ transpiler to write C++ code to. When `null`, it writes to `.pypp/cpp`.

This option is useful when [creating a Py++ library](../external_libraries/pure_library_example.md).

Default: `null`

# write_metadata_to_dir

This option specifies where the Py++ transpiler will write metadata about your project. It is only needed when [creating a Py++ library](../external_libraries/pure_library_example.md).

Default: `null`

# cmake_minimum_required_version

This option specifies what version the Py++ transpiler will put as the minimum required version in the CMakeLists.txt file. For example:

`cmake_minimum_required(VERSION 4.0)`

Default: `4.0`

# cpp_dir_is_dirty

When this option is `true`, the Py++ transpiler will copy the C++ template project to your C++ directory. After the template has been copied, the transpiler will automatically set this option to `false`.

This option needs to be set to `true` if you are transpiling to a directory for the first time.

Default: `true` (but automatically becomes `false` after C++ template is copied)
