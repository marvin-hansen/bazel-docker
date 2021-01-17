
# Instantiate the 'k8s_deploy' Bazel rule we need to use below
load("@k8s_deploy//:defaults.bzl", "k8s_deploy")

# The package rule declares this directory as a Bazel package
# which scopes targets defined in this Build file under this package.
# The visibility parameter declares which packages can call targets
# in this package. In this case, we're saying anyone can call these targets.
# See https://docs.bazel.build/versions/master/be/functions.html#package
package(default_visibility = ["//visibility:public"])

