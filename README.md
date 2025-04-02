# Atlas demo application

## Contents

This repository hosts an example application that is intended to be deployed on
the Atlas platform.

In particular, it contains the following:

### Application code

A trivial python application with an equally trivial test suite.

The application here is in python, but could be any language of your choice. The
only requirement is the ability to produce a Docker image (either through a
Dockerfile, or language-specific tools like Spring boot's
[build-image](https://docs.spring.io/spring-boot/maven-plugin/build-image.html)
or Go's [ko](https://ko.build/).

### Deployment manifests

The ̀`deploy/` folder contains [cdk8s](https://cdk8s.io/) typescript source code
to produce raw Kubernetes manifests.

This example uses `cdk8s`, but any tool that produces raw yaml could be used,
such as [kcl](https://www.kcl-lang.io/), [timoni](https://timoni.sh/),
[helm](https://helm.sh/) and many others, including raw manifests themselves
with a "do-nothing" build process.

The Atlas platform expects raw manifests and as such has no opinion on the way
they are produced.

### CI/CD Pipeline

The `.github/workflows/ci.yml` contains a Github workflow combining the previous
elements.

On push, the pipeline will:

- Run tests
- Build and push a Docker image
- Render the kubernets manifests
- Clone the associated GitOps repository
- Commit and push the rendered manifests on a selected environment in the GitOps
  repository

The pipeline in this demo only pushes to the `dev` environment on the `main`
branch, but can and should be extended to suit your desired workflows. You could
for example deploy to a different environment when a git tag is present, or
create a pull request on the GitOps repository instead of pushing to `main`.

Atlas makes no requirement of a specific release management strategy, though if
pressed for a recommendation, would suggest a
[Continuous Delivery](https://www.continuousdelivery.com/) approach with
[Trunk Based Development](https://trunkbaseddevelopment.com/), as recommended by
the [DORA research group](https://dora.dev/research/).

## Getting Started

This project uses [devbox](https://github.com/jetify-com/devbox) to manage its
development environment.

Install devbox:

```sh
curl -fsSL https://get.jetpack.io/devbox | bash
```

Start the devbox shell:

```sh
devbox shell
```

Run a script in the devbox environment:

```sh
devbox run <script>
```

## Scripts

Scripts are custom commands that can be run using this project's environment.
This project has the following scripts:

- [k8s](#devbox-run-k8s)
- [test](#devbox-run-test)

## Packages

- [nodejs@latest](https://www.nixhub.io/packages/nodejs)
- [nodePackages.cdk8s-cli@latest](https://www.nixhub.io/packages/nodePackages.cdk8s-cli)
- [uv@latest](https://www.nixhub.io/packages/uv)
- [python@latest](https://www.nixhub.io/packages/python)

## Script Details

### devbox run k8s

```sh
cd deploy
npm install
cdk8s synth
```

&#8194;

### devbox run test

```sh
uv run pytest -v -s
```
