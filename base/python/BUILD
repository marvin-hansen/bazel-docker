# Define the container image to build
load("@io_bazel_rules_docker//container:container.bzl", "container_image")
container_image(
    name = "py_hello",
    base = "@alpine_linux_amd64//image",
    cmd = ["Hello PyWorld!"],
    entrypoint = ["echo"],
)

# Define the push commando to publish container to registry
load("@io_bazel_rules_docker//container:container.bzl", "container_push")
container_push(
   name = "push_hello",
   image = ":py_hello",
   format = "Docker",
   registry = "gcr.io",
   repository = "future-281805/py_hello",
   tag = "latest",
)