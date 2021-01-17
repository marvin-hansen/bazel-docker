# Convert Dockerfile to bazel:
# https://dev.to/bamnet/dockerfile-to-bazel-6jh

# Define the container image to build
load("@io_bazel_rules_docker//container:container.bzl", "container_image")
container_image(
    name = "swift_build",
    base = "@swift_linux//image",
    cmd = ["Hello SwiftWorld!"],
    entrypoint = ["echo"],
)

# Define the push commando to publish container to registry
load("@io_bazel_rules_docker//container:container.bzl", "container_push")
container_push(
   name = "push_swift_build",
   image = ":swift_build",
   format = "Docker",
   registry = "gcr.io",
   repository = "future-281805/swift_build",
   tag = "5.3.2", # "5.3.2", "swift:5.3.2", "5.3.2-xenial",
)