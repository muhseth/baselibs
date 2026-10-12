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
    name = "length_prefixed_vector",
    srcs = ["length_prefixed_vector.cpp"],
    hdrs = ["length_prefixed_vector.h"],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    tags = ["FFI"],
    visibility = ["//visibility:public"],  # platform_only
    deps = ["//score/language/futurecpp"],
)

cc_gtest_unit_test(
    name = "unit_test",
    srcs = [
        "length_prefixed_vector_test.cpp",
    ],
    features = [
        "treat_warnings_as_errors",
        "strict_warnings",
        "additional_warnings",
    ],
    deps = [
        ":length_prefixed_vector",
    ],
)
