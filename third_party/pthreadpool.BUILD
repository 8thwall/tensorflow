load("@rules_cc//cc:defs.bzl", "cc_binary", "cc_library", "cc_test")

licenses(["notice"])

############################## pthreadpool library #############################

INTERNAL_HDRS = [
    "src/threadpool-atomics.h",
    "src/threadpool-common.h",
    "src/threadpool-object.h",
    "src/threadpool-utils.h",
]

PORTABLE_SRCS = [
    "src/memory.c",
    "src/portable-api.c",
]

PTHREADS_IMPL_SRCS = PORTABLE_SRCS + ["src/pthreads.c"]

GCD_IMPL_SRCS = PORTABLE_SRCS + ["src/gcd.c"]

SHIM_IMPL_SRCS = ["src/shim.c"]

INTERNAL_HDRS = [
    "src/threadpool-atomics.h",
    "src/threadpool-common.h",
    "src/threadpool-object.h",
    "src/threadpool-utils.h",
]

PORTABLE_SRCS = [
    "src/memory.c",
    "src/portable-api.c",
]

PTHREADS_IMPL_SRCS = PORTABLE_SRCS + ["src/pthreads.c"]

GCD_IMPL_SRCS = PORTABLE_SRCS + ["src/gcd.c"]

WINDOWS_IMPL_SRCS = PORTABLE_SRCS + ["src/windows.c"]

SHIM_IMPL_SRCS = ["src/shim.c"]

cc_library(
    name = "pthreadpool",
    srcs = select({
        ":pthreadpool_sync_primitive_explicit_condvar": INTERNAL_HDRS + PTHREADS_IMPL_SRCS,
        ":pthreadpool_sync_primitive_explicit_futex": INTERNAL_HDRS + PTHREADS_IMPL_SRCS,
        ":pthreadpool_sync_primitive_explicit_gcd": INTERNAL_HDRS + GCD_IMPL_SRCS,
        ":pthreadpool_sync_primitive_explicit_event": INTERNAL_HDRS + WINDOWS_IMPL_SRCS,
        ":emscripten_with_threads": INTERNAL_HDRS + PTHREADS_IMPL_SRCS,
        ":emscripten": INTERNAL_HDRS + SHIM_IMPL_SRCS,
        ":macos_x86": INTERNAL_HDRS + GCD_IMPL_SRCS,
        ":macos_x86_64": INTERNAL_HDRS + GCD_IMPL_SRCS,
        ":ios": INTERNAL_HDRS + GCD_IMPL_SRCS,
        ":windows_x86_64": INTERNAL_HDRS + WINDOWS_IMPL_SRCS,
        ":windows_x86_64_msvc": INTERNAL_HDRS + WINDOWS_IMPL_SRCS,
        "//conditions:default": INTERNAL_HDRS + PTHREADS_IMPL_SRCS,
    }),
    copts = [
        "-std=gnu11",
    ] + select({
        ":optimized_build": ["-O2"],
        "//conditions:default": [],
    }) + select({
        ":linux_arm": ["-DPTHREADPOOL_USE_CPUINFO=1"],
        ":linux_armhf": ["-DPTHREADPOOL_USE_CPUINFO=1"],
        ":linux_aarch64": ["-DPTHREADPOOL_USE_CPUINFO=1"],
        ":android_armv7": ["-DPTHREADPOOL_USE_CPUINFO=1"],
        ":android_arm64": ["-DPTHREADPOOL_USE_CPUINFO=1"],
        "//conditions:default": ["-DPTHREADPOOL_USE_CPUINFO=0"],
    }) + select({
        ":pthreadpool_sync_primitive_explicit_condvar": [
            "-DPTHREADPOOL_USE_CONDVAR=1",
            "-DPTHREADPOOL_USE_FUTEX=0",
            "-DPTHREADPOOL_USE_GCD=0",
            "-DPTHREADPOOL_USE_EVENT=0",
        ],
        ":pthreadpool_sync_primitive_explicit_futex": [
            "-DPTHREADPOOL_USE_CONDVAR=0",
            "-DPTHREADPOOL_USE_FUTEX=1",
            "-DPTHREADPOOL_USE_GCD=0",
            "-DPTHREADPOOL_USE_EVENT=0",
        ],
        ":pthreadpool_sync_primitive_explicit_gcd": [
            "-DPTHREADPOOL_USE_CONDVAR=0",
            "-DPTHREADPOOL_USE_FUTEX=0",
            "-DPTHREADPOOL_USE_GCD=1",
            "-DPTHREADPOOL_USE_EVENT=0",
        ],
        ":pthreadpool_sync_primitive_explicit_event": [
            "-DPTHREADPOOL_USE_CONDVAR=0",
            "-DPTHREADPOOL_USE_FUTEX=0",
            "-DPTHREADPOOL_USE_GCD=0",
            "-DPTHREADPOOL_USE_EVENT=1",
        ],
        "//conditions:default": [],
    }),
    hdrs = [
        "include/pthreadpool.h",
    ],
    defines = [
        "PTHREADPOOL_NO_DEPRECATED_API",
    ],
    includes = [
        "include",
    ],
    linkopts = select({
        ":emscripten_with_threads": [
            "-s ALLOW_BLOCKING_ON_MAIN_THREAD=1",
            "-s PTHREAD_POOL_SIZE=8",
        ],
        "//conditions:default": [],
    }),
    strip_include_prefix = "include",
    deps = [
        "@FXdiv",
    ] + select({
        ":linux_arm": ["@cpuinfo"],
        ":linux_armhf": ["@cpuinfo"],
        ":linux_aarch64": ["@cpuinfo"],
        ":android_armv7": ["@cpuinfo"],
        ":android_arm64": ["@cpuinfo"],
        "//conditions:default": [],
    }),
    visibility = ["//visibility:public"],
)

################################## Unit tests ##################################

EMSCRIPTEN_TEST_LINKOPTS = [
    "-s ASSERTIONS=2",
    "-s ERROR_ON_UNDEFINED_SYMBOLS=1",
    "-s DEMANGLE_SUPPORT=1",
    "-s EXIT_RUNTIME=1",
    "-s ALLOW_MEMORY_GROWTH=0",
    "-s TOTAL_MEMORY=67108864",  # 64M
]

cc_test(
    name = "pthreadpool_test",
    srcs = ["test/pthreadpool.cc"],
    linkopts = select({
        ":emscripten": EMSCRIPTEN_TEST_LINKOPTS,
        "//conditions:default": [],
    }),
    deps = [
        ":pthreadpool",
        "@com_google_googletest//:gtest_main",
    ],
)

################################## Benchmarks ##################################

EMSCRIPTEN_BENCHMARK_LINKOPTS = [
    "-s ASSERTIONS=1",
    "-s ERROR_ON_UNDEFINED_SYMBOLS=1",
    "-s EXIT_RUNTIME=1",
    "-s ALLOW_MEMORY_GROWTH=0",
]

cc_binary(
    name = "latency_bench",
    srcs = ["bench/latency.cc"],
    linkopts = select({
        ":emscripten": EMSCRIPTEN_BENCHMARK_LINKOPTS,
        "//conditions:default": [],
    }),
    deps = [
        ":pthreadpool",
        "@com_google_benchmark//:benchmark",
    ],
)

cc_binary(
    name = "throughput_bench",
    srcs = ["bench/throughput.cc"],
    linkopts = select({
        ":emscripten": EMSCRIPTEN_BENCHMARK_LINKOPTS,
        "//conditions:default": [],
    }),
    deps = [
        ":pthreadpool",
        "@com_google_benchmark//:benchmark",
    ],
)

############################# Build configurations #############################

# Synchronize workers using pthreads condition variable.
config_setting(
    name = "pthreadpool_sync_primitive_explicit_condvar",
    define_values = {"pthreadpool_sync_primitive": "condvar"},
)

# Synchronize workers using futex.
config_setting(
    name = "pthreadpool_sync_primitive_explicit_futex",
    define_values = {"pthreadpool_sync_primitive": "futex"},
)

# Synchronize workers using Grand Central Dispatch.
config_setting(
    name = "pthreadpool_sync_primitive_explicit_gcd",
    define_values = {"pthreadpool_sync_primitive": "gcd"},
)

# Synchronize workers using WinAPI event.
config_setting(
    name = "pthreadpool_sync_primitive_explicit_event",
    define_values = {"pthreadpool_sync_primitive": "event"},
)

config_setting(
    name = "optimized_build",
    values = {
        "compilation_mode": "opt",
    },
)

config_setting(
    name = "linux_arm",
    values = {"cpu": "arm"},
)

config_setting(
    name = "linux_armhf",
    values = {"cpu": "armhf"},
)

config_setting(
    name = "linux_aarch64",
    values = {"cpu": "aarch64"},
)

config_setting(
    name = "android_armv7",
    values = {
        "crosstool_top": "//external:android/crosstool",
        "cpu": "armeabi-v7a",
    },
)

config_setting(
    name = "android_arm64",
    values = {
        "crosstool_top": "//external:android/crosstool",
        "cpu": "arm64-v8a",
    },
)

# Note: we need to individually match x86 and x86-64 macOS rather than use
# catch-all "apple_platform_type": "macos" because that option defaults to
# "macos" even when building on Linux!
config_setting(
    name = "macos_x86",
    values = {
        "apple_platform_type": "macos",
        "cpu": "darwin",
    },
)

config_setting(
    name = "macos_x86_64",
    values = {
        "apple_platform_type": "macos",
        "cpu": "darwin_x86_64",
    },
)

config_setting(
    name = "ios",
    values = {
        "crosstool_top": "@bazel_tools//tools/cpp:toolchain",
        "apple_platform_type": "ios",
    },
)

config_setting(
    name = "windows_x86_64",
    values = {
        "cpu": "x64_windows",
    },
)

config_setting(
    name = "windows_x86_64_msvc",
    values = {
        "cpu": "x64_windows_msvc",
    },
)

config_setting(
    name = "emscripten",
    values = {
        "cpu": "js",
    }
)

config_setting(
    name = "emscripten_with_threads",
    values = {
        "crosstool_top": "//toolchain:emscripten",
        "copt": "-pthread",
    }
)
