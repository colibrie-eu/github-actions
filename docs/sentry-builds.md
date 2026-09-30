# Sentry source maps in application images

Applications can set the repository variables `SENTRY_ORG` and `SENTRY_PROJECT`
and the Actions secret `SENTRY_AUTH_TOKEN`. The organization/project values are
non-secret build arguments. The upload token is an optional BuildKit secret
named `sentry_auth_token`, available only to trusted branch/tag builds.

The application's Dockerfile mounts the secret only in the build step and gives
the Sentry Vite plugin the exact release identifier used by the application.
The token must never be passed as a Docker build argument or persisted in image
environment variables. Pull-request builds receive an empty Sentry secret.

Repositories without Sentry configuration continue using the same reusable
workflow. The application's plugin should stay disabled when its upload
configuration is absent. Runtime DSNs are configured independently of the image
build, using environment variables supplied through the deployment's secret
references.
