# Quiz Tracker documentation

Source for **https://quiz-tracker.thally.app**, the documentation for
[Quiz Tracker](https://github.com/SECHUBSYSTEM/quiz-tracker): an open-source
Next.js and Postgres app for building quizzes, taking them, and tracking scores.
The site is built and hosted with [Thally](https://thally.io).

## What's in the docs

| Page | Source |
| --- | --- |
| Introduction: what Quiz Tracker is and who it's for | `src/content/introduction.mdx` |
| Quickstart: run it locally and score a first quiz | `src/content/quickstart.mdx` |
| Guide: drive the REST API with curl or fetch | `src/content/guides/api.mdx` |
| API reference: all eight endpoints | `openapi.yaml`, plus `src/content/api/*.mdx` |
| Changelog | `src/content/changelog.mdx` |

Navigation lives in `docs.json`, and the site name and links in `src/data/site.ts`.
Screenshots in `public/images/` were captured from the
[live demo](https://quiz-tracker-eight.vercel.app).

Every command, response and error message in these pages was captured from a
fresh clone of Quiz Tracker, not written from memory.

## Run the docs locally

```bash
pnpm install
pnpm dev        # http://localhost:3040
```

npm works too: `npm ci` then `npm run dev`. The repository currently carries
both lockfiles; see "Known issues".

## Check before you push

```bash
pnpm run check --ci
pnpm run typecheck
pnpm run build
```

`check` validates internal links and anchors, navigation entries, page
frontmatter and the OpenAPI file.

Pushing to `main` publishes the site through Thally Cloud. A rebuild took about
three minutes the first time.

## Known issues

- **`check` fails on Windows when Node is installed under `C:\Program Files`.**
  The Thally CLI launches `node.exe` through a shell without quoting the path,
  so you get `'C:\Program' is not recognized as an internal or external command`.
  Workaround: run the pinned CLI with a `node.exe` copied to a folder without
  spaces:

  ```bash
  npm ci --ignore-scripts --prefix .github/thally-tooling
  <folder-without-spaces>/node.exe .github/thally-tooling/node_modules/@thallylabs/cli/dist/index.js check --ci .
  ```

- **The API reference's Try it button can't reach `localhost`.** Thally's
  playground proxy returns `403 Private or unsafe targets are not allowed` for
  private addresses. The reference targets `http://localhost:3000` on purpose,
  so test writes never reach the shared demo database. Use the generated curl
  on each endpoint page instead.

- **Two lockfiles.** The project was scaffolded with
  `create-thally-docs --package-manager pnpm`, which still ships
  `package-lock.json` beside `pnpm-lock.yaml`. Both are kept until the hosted
  build's package manager is confirmed.

## License

The site runtime comes from the MIT-licensed Thally starter; see `LICENSE`.
Quiz Tracker itself is MIT licensed in its own repository.
