# Contributing

This is the Cast receiver of [Streamyfin](https://github.com/streamyfin/streamyfin). Bugs and ideas go to the
[Streamyfin issue tracker](https://github.com/streamyfin/streamyfin/issues).

## Tools

You need Node 24 (see `.nvmrc`) and npm. Code style is set by editorconfig, prettier and eslint.

| Command                 | What it does                                          |
| ----------------------- | ----------------------------------------------------- |
| `npm ci`                | Install the dependencies                              |
| `npm start`             | Build a development version and serve it on port 9000 |
| `npm run test -- --run` | Run the tests once                                    |
| `npm run test:scripts`  | Run the tests of the deployment scripts               |
| `npm run build`         | Build the production version into `dist`              |
| `npm run lint`          | Run eslint, the TypeScript check and stylelint        |

## Trying a change on a TV

A Cast device only loads a receiver from the address registered for an application ID, over HTTPS. The two
public addresses serve `main` and the releases, so a change that is not merged yet needs an address of its own.

1. **A test address, in your fork.** In your fork, turn Actions on, enable GitHub Pages with the source "GitHub
   Actions" and allow the `streamyfin` branch in the `github-pages` environment. Then push your change to a
   branch named `streamyfin`: every push to it is tested, built and linted by the `Streamyfin` workflow, then
   deployed to the Pages address of the fork. The workflow can also be started by hand to deploy any branch,
   tag or commit, which is how an earlier build is put back. It never deploys in this repository. A build
   reports the first seven characters of its commit as its version, so the Jellyfin server shows which build a
   TV has loaded.
2. **A test application ID.** [Register an application](https://developers.google.com/cast/docs/registration)
   of the type "Custom Receiver" that points to the Pages address of your fork, and register your Cast device
   by its serial number. An application that is not published only starts on registered devices.
3. **The bench.** The Streamyfin repository has a Cast bench under `e2e/`: a sender that talks to the receiver
   as the app does (`e2e/bench/sender.py`) and a matrix of playback cases (`e2e/suites/cast`). Both drive the
   official Jellyfin receiver unless `--app-id` names another application. Pass the test application ID to
   measure a build of this repository, and run the same cases with and without it to compare the two.

A TV keeps the page it has loaded until the receiver starts again. Before you trust a result, read the version
the TV reports to the server.

## Pull requests

Branch from `main` and open the pull request against it. The `Build` workflow tests, builds and lints a pull
request, and deploys nothing. Run `npm run test -- --run`, `npm run test:scripts`, `npm run build` and
`npm run lint` before you push. A commit message is one sentence in the imperative, without a prefix. Commits
are signed. Once merged, a change is online on the beta channel a few minutes later: `RELEASING.md` has the
rest.
