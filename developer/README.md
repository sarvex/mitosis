# Local Development

Welcome ⚡️!! If you've found a bug, or have an idea to add a feature we'd love to hear from you. It may save time to first ping the group on [Mitosis' Discord channel](https://discord.com/channels/842438759945601056/935218469799071835) to talk through any ideas or any issues that may be a bug.

## Project Structure

Mitosis is structured as a mono-repo using Yarn (v3) Workspaces. The packages
live under `packages/` and `examples/`:

- `core` (`@builder.io/mitosis`): contains the Mitosis engine
- `cli` (`@builder.io/mitosis-cli`): contains the Mitosis CLI, and _depends_ on `core`
- `fiddle`: contains the code for the interactive Mitosis fiddle, which is hosted at mitosis.builder.io
- `eslint-plugin` (`@builder.io/eslint-plugin-mitosis`): contains the Mitosis eslint rules to enforce valid Mitosis component syntax. Yet to be released.

## Installation

First, you should run `yarn` in the root of the project to install all the dependencies.

For all packages, the below steps to develop locally are the same:

```bash
# run local development server
yarn start

# run unit tests
yarn test
```

## Submitting Issues And Writing Tests

We need your help! If you found a bug, it's best to [create an issue](https://github.com/BuilderIO/mitosis/issues/new/choose) and follow the template we've created for you. Afterwards, create a [Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request) that replicates the issue using a test.

## Developing for Core & Testing

In `core`, we use vitest snapshots & integeration tests for test coverage. If you are solving a problem that is reproducible by a fiddle in [mitosis.builder.io](https://mitosis.builder.io), we highly recommend the following flow:

### Snapshot test

- copy your fiddle component into a file in `packages/core/src/__tests__/data`. See [packages/core/src/**tests**/data/basic.raw.tsx](/packages/core/src/__tests__/data/basic.raw.tsx) as an example.
- add that test to the [test generator](/packages/core/src/__tests__/test-generator.ts), most likely in `BASIC_TESTS`.
- run `yarn test:watch` in the `packages/core` directory to run the snapshot tests in watch mode

PS: don't worry about failing imports in the raw test TSX files. These are not an issue, since the files are standalone and don't actually belong to a cohesive project.

### Integration test

- copy your fiddle component into a `.lite.tsx` Mitosis component in the [e2e app](/e2e/e2e-app/src)
- add your component to the [e2e-app component map](/e2e/e2e-app/src/component-map.ts)
- add an integration test in [e2e/e2e-app/tests](/e2e/e2e-app/tests) that makes sure your component works as expected
- this integration test will run against every server that exists in [e2e/](/e2e/).
- run `yarn ci:e2e-prep` to install playwright browsers
- run `yarn ci:e2e` to run the integration tests against all servers

### Test your changes

From there, you can keep iterating until the snapshots look as expected, and the integration tests pass!

### Pre-submit

- format: `yarn run fmt:prettier`
