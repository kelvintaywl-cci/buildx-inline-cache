# buildx-local-cache

Showcase using Docker Buildx local cache with CircleCI's cache

## Quick explanation

This utilizes Docker Buildx's `--cache-to` and `--cache-from` options.
Specifically, we take advantage of [CircleCI's cache](https://circleci.com/docs/caching/) to save and load the built layers.
This way, we do not need CircleCI DLC but can leverage caching when building images still!

In the workflow, you will see 2 jobs:

```yaml
workflows:
  main:
    jobs:
      - build-push-local-machine
      - test-pushed-image:
          requires:
            - build-push-local-machine
```

The `build-push-local-machine` job builds an image using Buildx local cache.
It then pushes to Docker Hub, before saving the image (as .tar file) to the [workspace](https://circleci.com/docs/workspaces/).

This allows the 2nd job, `test-pushed-image`, to load this .tar file as a Docker image.
In this way, the 2nd job then is able to use this pushed image.

## Related work

- https://github.com/kelvintaywl-cci/docker-buildx
