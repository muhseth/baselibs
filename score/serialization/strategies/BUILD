# *******************************************************************************
# Copyright (c) 2026 Contributors to the Eclipse Foundation
#
# See the NOTICE file(s) distributed with this work for additional
# information regarding copyright ownership.
#
# This program and the accompanying materials are made available under the
# terms of the Apache License Version 2.0 which is available at
# https://www.apache.org/licenses/LICENSE-2.0
#
# SPDX-License-Identifier: Apache-2.0
# *******************************************************************************

load("@rules_cc//cc:defs.bzl", "cc_library")
load("@score_baselibs//:bazel/unit_tests.bzl", "cc_gtest_unit_test")

cc_library(
    name = "unsigned_integral_only",
    srcs = ["unsigned_integral_only_strategy.cpp"],
    hdrs = ["unsigned_integral_only_strategy.h"],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    tags = ["FFI"],
    # can be extended if users directly want to depend on it (encouraged)
    visibility = ["//visibility:public"],  # platform_only
    deps = [
        "//score/serialization:strategy_interface",
        "//score/language/futurecpp",
    ],
)

cc_library(
    name = "uds_strategy",
    srcs = ["uds_strategy.cpp"],
    hdrs = ["uds_strategy.h"],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    tags = ["FFI"],
    # can be extended if users directly want to depend on it (encouraged)
    visibility = ["//visibility:public"],  # platform_only
    deps = [
        ":unsigned_integral_only",
        "//score/memory:endianness",
        "//score/serialization:strategy_interface",
        "//score/serialization/types:length_prefixed_vector",
    ],
)

# Example SOME/IP serialization strategy: big-endian scalars + compact
# (actual-size, no capacity padding) length-prefixed arrays and strings.
cc_library(
    name = "someip_strategy",
    srcs = ["someip_strategy.cpp"],
    hdrs = [
        "someip_array.h",
        "someip_layout.h",
        "someip_max_size.h",
        "someip_scalar.h",
        "someip_strategy.h",
        "someip_string.h",
        "someip_wire_format.h",
    ],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    tags = ["FFI"],
    visibility = ["//score/serialization:__subpackages__"],
    deps = [
        "//score/static_reflection_with_serialization/visitor",
        "//score/memory:endianness",
        "//score/serialization:strategy_interface",
        "//score/serialization/types:length_prefixed_vector",
        "//score/language/futurecpp",
    ],
)

cc_library(
    name = "hex_string_to_bytes",
    srcs = [
        "hex_string_to_bytes.cpp",
    ],
    hdrs = [
        "hex_string_to_bytes.h",
    ],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    tags = ["FFI"],
    visibility = ["//visibility:public"],  # platform_only
    deps = ["//score/language/futurecpp"],
)

cc_library(
    name = "uds_strategy_serialization_test_helper",
    srcs = ["uds_strategy_serialization_test_helper.cpp"],
    hdrs = ["uds_strategy_serialization_test_helper.h"],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    visibility = ["//score:__subpackages__"],
    deps = [
        ":hex_string_to_bytes",
        ":uds_strategy",
        "//score/static_reflection_with_serialization/visitor",
        "//score/serialization",
        "@googletest//:gtest",
        "//score/language/futurecpp",
    ],
)

cc_gtest_unit_test(
    name = "unit_test_hex_string_to_bytes",
    srcs = [
        "hex_string_to_bytes_test.cpp",
    ],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    visibility = ["//score/serialization:__pkg__"],
    deps = [
        ":hex_string_to_bytes",
    ],
)

cc_gtest_unit_test(
    name = "unit_test",
    srcs = [
        "uds_strategy_test.cpp",
    ],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    visibility = ["//score/serialization:__pkg__"],
    deps = [
        ":uds_strategy",
        "//score/static_reflection_with_serialization/visitor",
        "//score/serialization",
    ],
)

cc_library(
    name = "someip_strategy_test_fixtures",
    testonly = True,
    hdrs = ["someip_strategy_test_fixtures.h"],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    visibility = ["//score/serialization:__subpackages__"],
    deps = [
        ":someip_strategy",
        "//score/serialization",
        "//score/static_reflection_with_serialization/visitor",
        "@googletest//:gtest",
        "//score/language/futurecpp",
    ],
)

cc_gtest_unit_test(
    name = "unit_test_someip",
    srcs = [
        "someip_strategy_test.cpp",
    ],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    visibility = ["//score/serialization:__pkg__"],
    deps = [
        ":someip_strategy",
        ":someip_strategy_test_fixtures",
        "//score/static_reflection_with_serialization/visitor",
        "//score/serialization",
        "//score/serialization/types:length_prefixed_vector",
        "//score/language/futurecpp",
    ],
)

cc_gtest_unit_test(
    name = "unit_test_someip_scalar",
    srcs = [
        "someip_scalar_test.cpp",
    ],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    visibility = ["//score/serialization:__pkg__"],
    deps = [
        ":someip_strategy",
        "//score/serialization",
        "//score/language/futurecpp",
    ],
)

cc_gtest_unit_test(
    name = "unit_test_someip_wire_format",
    srcs = [
        "someip_wire_format_test.cpp",
    ],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    visibility = ["//score/serialization:__pkg__"],
    deps = [
        ":someip_strategy",
        "//score/serialization",
        "//score/language/futurecpp",
    ],
)

cc_gtest_unit_test(
    name = "unit_test_someip_array",
    srcs = [
        "someip_array_test.cpp",
    ],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    visibility = ["//score/serialization:__pkg__"],
    deps = [
        ":someip_strategy",
        ":someip_strategy_test_fixtures",
        "//score/static_reflection_with_serialization/visitor",
        "//score/serialization",
        "//score/serialization/types:length_prefixed_vector",
        "//score/language/futurecpp",
    ],
)

cc_gtest_unit_test(
    name = "unit_test_someip_string",
    srcs = [
        "someip_string_test.cpp",
    ],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    visibility = ["//score/serialization:__pkg__"],
    deps = [
        ":someip_strategy",
        ":someip_strategy_test_fixtures",
        "//score/static_reflection_with_serialization/visitor",
        "//score/serialization",
        "//score/language/futurecpp",
    ],
)

cc_gtest_unit_test(
    name = "unit_test_someip_max_size",
    srcs = [
        "someip_max_size_test.cpp",
    ],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    visibility = ["//score/serialization:__pkg__"],
    deps = [
        ":someip_strategy",
        ":someip_strategy_test_fixtures",
        "//score/static_reflection_with_serialization/visitor",
        "//score/serialization",
        "//score/language/futurecpp",
    ],
)
