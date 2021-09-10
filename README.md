`lib/extra.h` file is not included in any of the source files.

Build logs:
- `clean.build.log` clean build
- `commented_extra_header.build.log`  changed the `lib/BUILD.bazel` file
```
cc_library(
    name = "lib",
    srcs = [
        # "extra.h",
        "lib.cpp",
    ],
    hdrs = ["lib.h"],
    visibility = ["//visibility:public"],
)
```
- `uncommented_extra_header.build.log` uncommented the line from above


The above actions cause `app/main.cpp` to recompile, which I don't expect to happen. I suspect it
due to `Declared include source: lib/extra.h` being used as a key for `main.cpp` file.
