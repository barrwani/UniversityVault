# Relocatable Builds with Spack
#cs 

## Overview

- Spack tracks paths during builds, making packages non-relocatable by default.
- Tools like `patchelf` and `libelf` allow binaries to be adjusted for portability.
- Relocatable builds are essential for:
    - Moving software between systems.
    - Using build caches effectively.

---

# Spack Build Cache

## What is a Build Cache?

- A repository of pre-built binaries for reuse on compatible systems.
- Reduces build times by avoiding redundant dependency builds.
- Enables portable software delivery.

## Using Build Caches

- Build caches can be hosted on any OCI-compliant container registry (e.g., Docker Hub, GitLab Container Registry).
- Commands:
    
    ```bash
    spack buildcache create -a -d /path/to/cache my-package
    spack mirror add my-cache /path/to/cache
    spack buildcache update-index
    ```


- [Spack Binary Cache Tutorial](https://spack-tutorial.readthedocs.io/en/hpcic24/tutorial_binary_cache.html)

---

# Automating Spack Builds with GitLab CI

## Overview

- GitLab CI/CD pipelines automate Spack builds and upload results to registries.

## Example `.gitlab-ci.yml`

```yaml
stages:
  - build
  - cache

build:
  image: spack/ubuntu-bionic
  script:
    - spack install my-package
    - spack buildcache create -a -d /path/to/cache my-package

cache:
  image: spack/ubuntu-bionic
  script:
    - spack mirror add my-cache /path/to/cache
    - spack buildcache update-index
  only:
    - tags
```

---

# Containerization with Spack

## Building Spack Containers

- Use `spack containerize` to create a container configuration file:

    ```bash
    spack containerize --output container.yaml
    ```


## OCI Registry Integration

- Containers can pull pre-built binaries from Spack build caches:

    ```yaml
    stages:
      - pull-cache
      - build-image
    
    pull-cache:
      script:
        - spack mirror add buildcache https://my-cache-url
        - spack buildcache install -a my-package
    
    build-image:
      script:
        - docker build -t my-container:latest .
      only:
        - tags
    ```


---

# Hosting Build Caches in Registries

## Supported Registries

- GitLab Container Registry.
- Any other OCI-compliant registry.

## Example for GitLab Container Registry

```bash
spack buildcache create -r registry.gitlab.com/my-project/spack-buildcache
```

## Permissions

- Ensure CI runners have the appropriate credentials to push to the registry.

---

# Tips for Using Spack with Build Caches

- Update the Spack index regularly to reflect new builds:

    ```bash
    spack buildcache update-index
    ```

- Configure `packages.yaml` for target-specific builds:
    - Example: Enable optimizations for AVX2 or SSE2.

