`lib/extra.h` file is not included in any of the source files.


## Single library Mode (#159b230155fa2b14f05686d482db66db90371502)

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


## Indirection

Added `slib` as intermidiate library between `app` and `lib` (`app` -> `slib` -> `lib`).

This did not make difference `lib/extra.h` still shows up as change in in all three compilation units.


## Change to implementation_deps

Converted `lib` to private dependency of `slib` using `implementation_deps` experimental feature of
`cc_library`.

Now `main.cpp` does not recompile and `lib/extra.h` does not show up in its key. However, it still
affects `slib`.

