workspace(name = "bazel_example")

# "http_archive" is a Bazel rule that loads Bazel repositories &
# makes its targets available for execution.
# It is deprecated however, so we need to manually load it to use it.
# See https://docs.bazel.build/versions/master/be/workspace.html#http_archive
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "1698624e878b0607052ae6131aa216d45ebb63871ec497f26c67455b34119c80",
    strip_prefix = "rules_docker-0.15.0",
    urls = ["https://github.com/bazelbuild/rules_docker/releases/download/v0.15.0/rules_docker-v0.15.0.tar.gz"],
)

################################################################################
# Container dependencies
################################################################################

# Instantiate the "repositories" Bazel rule in rules_docker as "container_repositories"
load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)
# Download dependencies for container rules
container_repositories()

load(
    "@io_bazel_rules_docker//repositories:deps.bzl",
    container_deps = "deps"
)
container_deps()

################################################################################
# Containers to pull required to build.
################################################################################
load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
)
# docker pull alpine:3.13.0
# https://hub.docker.com/_/alpine
container_pull(
    name = "alpine_linux_amd64",
    registry = "index.docker.io",
    repository = "library/alpine",
    tag = "3.13.0",
)

# docker pull swift:5.3.2-xenial
# https://hub.docker.com/_/swift
container_pull(
    name = "swift_linux",
    registry = "index.docker.io",
    repository = "library/swift",
    tag = "5.3.2-xenial",
)

################################################################################
# Docker authentication to push into container registry
################################################################################
# Load the macro that allows you to customize the docker toolchain configuration.
load("@io_bazel_rules_docker//toolchains/docker:toolchain.bzl",
    docker_toolchain_configure="toolchain_configure"
)

# For authentication, run
# gcloud auth login
# gcloud auth configure-docker
# https://cloud.google.com/container-registry/docs/advanced-authentication#gcloud-helper
# docker custom configuration
# https://github.com/bazelbuild/rules_docker#container_push-custom-client-configuration
docker_toolchain_configure(
  name = "docker_config",
  # Replace this with an absolute path to a directory which has a custom docker client config.json.
  # Docker allows you to specify custom authentication credentials in the client configuration JSON file.
  # See https://docs.docker.com/engine/reference/commandline/cli/#configuration-files
  client_config="/home/marvin/.docker",
  # OPTIONAL: Path to the docker binary.
  # Should be set explicitly for remote execution.
  docker_path="/usr/bin/docker",
  # OPTIONAL: Path to the gzip binary.
  # gzip_path="<enter absolute path to the gzip binary (in the remote exec env) here>",
)

################################################################################
# Kubernetes dependencies
################################################################################

http_archive(
    name = "io_bazel_rules_k8s",
    strip_prefix = "rules_k8s-0.6",
    urls = ["https://github.com/bazelbuild/rules_k8s/archive/v0.6.tar.gz"],
    sha256 = "51f0977294699cd547e139ceff2396c32588575588678d2054da167691a227ef",
)

load("@io_bazel_rules_k8s//k8s:k8s.bzl", "k8s_repositories")
k8s_repositories()

load("@io_bazel_rules_k8s//k8s:k8s_go_deps.bzl", k8s_go_deps="deps")
k8s_go_deps()

# Load a couple rules we need from the K8s Bazel repo
load("@io_bazel_rules_k8s//toolchains/kubectl:kubectl_configure.bzl", "kubectl_configure")

# Set up some default attributes when the K8s rule "k8s_object" is called later
# This only applies to "k8s_object" called with kind = "deployment"
# This also exposes a Bazel rule called "k8s_deploy"
# which we use in java-spring-boot/BUILD.bazel and js-client/BUILD.bazel.
# See https://github.com/bazelbuild/rules_k8s#k8s_defaults
load("@io_bazel_rules_k8s//k8s:k8s.bzl", "k8s_defaults")
k8s_defaults(
    name = "k8s_deploy",
    kind = "deployment",
    namespace = "default",
)

# Set up some default attributes when the K8s rule "k8s_object" is called later
# This only applies to "k8s_object" called with kind = "service"
# See https://github.com/bazelbuild/rules_k8s#k8s_defaults
k8s_defaults(
    name = "k8s_service",
    kind = "service",
    namespace = "default",
)