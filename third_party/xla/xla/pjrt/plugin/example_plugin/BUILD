load("//xla:xla.default.bzl", "xla_cc_binary", "xla_cc_test")

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = ["//visibility:public"],
    licenses = ["notice"],
)

cc_library(
    name = "myplugin_cpp_pjrt",
    srcs = [
        "myplugin_cpp_pjrt.cc",
    ],
    hdrs = ["myplugin_cpp_pjrt.h"],
    deps = [
        "//xla/pjrt:pjrt_client",
        "//xla/pjrt:pjrt_compiler",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_absl//absl/types:span",
        "@local_tsl//tsl/platform:fingerprint",
    ],
)

xla_cc_test(
    name = "myplugin_cpp_pjrt_test",
    srcs = ["myplugin_cpp_pjrt_test.cc"],
    deps = [
        ":myplugin_cpp_pjrt",
        "//xla/tests:xla_internal_test_main",
        "//xla/tsl/platform:status_matchers",
        "@com_google_googletest//:gtest",
    ],
)

cc_library(
    name = "myplugin_c_pjrt_internal",
    srcs = [
        "myplugin_c_pjrt_internal.cc",
    ],
    hdrs = ["myplugin_c_pjrt_internal.h"],
    deps = [
        ":myplugin_cpp_pjrt",
        "//xla/pjrt:pjrt_client",
        "//xla/pjrt/c:pjrt_c_api_hdrs",
        "//xla/pjrt/c:pjrt_c_api_layouts_extension_hdrs",
        "//xla/pjrt/c:pjrt_c_api_wrapper_impl",
        "@com_google_absl//absl/status",
    ],
    alwayslink = 1,
)

cc_library(
    name = "myplugin_dynamic_registration",
    testonly = True,
    srcs = ["myplugin_dynamic_registration.cc"],
    deps = [
        "//xla/pjrt/plugin:dynamic_registration",
        "//xla/tsl/platform:env",
        "//xla/tsl/platform:test",
        "@com_google_absl//absl/log",
        "@local_tsl//tsl/platform:path",
    ],
    alwayslink = 1,
)

cc_library(
    name = "myplugin_c_pjrt",
    srcs = [
        "myplugin_c_pjrt.cc",
    ],
    hdrs = ["myplugin_c_pjrt.h"],
    deps = [
        ":myplugin_c_pjrt_internal",
        "//xla/pjrt/c:pjrt_c_api_hdrs",
    ],
)

xla_cc_test(
    name = "myplugin_c_pjrt_test",
    srcs = ["myplugin_c_pjrt_test.cc"],
    data = [
        ":pjrt_c_api_myplugin_plugin.so",
    ],
    deps = [
        ":myplugin_c_pjrt",
        ":myplugin_dynamic_registration",
        "//xla/pjrt:pjrt_c_api_client",
        "//xla/pjrt:pjrt_client",
        "//xla/pjrt/c:pjrt_c_api_hdrs",
        "//xla/tests:xla_internal_test_main",
        "//xla/tsl/lib/core:status_test_util",
        "@com_google_absl//absl/status:statusor",
        "@com_google_googletest//:gtest",
    ],
)

xla_cc_binary(
    name = "pjrt_c_api_myplugin_plugin.so",
    linkshared = True,
    deps = [
        ":myplugin_c_pjrt",
    ],
)
