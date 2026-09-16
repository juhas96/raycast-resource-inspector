# Raycast Store submission

Publishing this GitHub repository does **not** publish an extension in the Raycast Store. Raycast reviews an extension pull request in [raycast/extensions](https://github.com/raycast/extensions); merging that pull request makes the extension available in the Store.

## Account setup still needed

- Replace `package.json` → `author` with the maintainer's **Raycast username**, not their GitHub or Mac account name. The current `jkbjhs` value is a local-development placeholder and fails the Store's author lookup.
- Run `npx ray login` to authenticate the Raycast CLI. Do not commit tokens or credentials.
- The public publishing flow also requests GitHub authentication to prepare the Store pull request.

## Prepare and submit

```sh
npm ci
npm run build
npm test
npm run lint
npm run lint:store
npm run publish
```

Do not submit until all validation passes. Review the generated pull request and respond to Raycast's review. Publication is not immediate and approval is not guaranteed.

Recommended before submission: add at least three 2000 × 1250 PNG screenshots using Raycast's Window Capture and Save to Metadata. Use demonstration data so screenshots do not expose private app, project, or worktree names. No screenshots of the development machine's private project lists are included here.

## Native helper provenance

`assets/inspector` is generated entirely from `native/Inspector.swift` using Apple's Swift compiler, the macOS SDK, and system SQLite/AppKit/Darwin frameworks. `scripts/build-native.sh` compiles arm64 and x86_64 for macOS 13+, combines them with `lipo`, and applies a local ad-hoc signature. Rebuild it using `npm run build:native`. The helper has no privileged installation, Keychain access, network access code, separate daemon, or runtime download.

The Store reviewer should examine both the Swift source and the build recipe. The included binary is traceable to this source, but Swift versions and SDK changes can change its bytes. The target supports both architectures; Intel and macOS versions older than the development machine have not yet been runtime-tested. Raycast may request changes to how this helper is packaged.

The single-target destructive actions are explicit and confirmed. Force actions remain separate. Tests create disposable worktrees; live process/container safety fixtures are opt-in. Resource history and diagnostics remain local and must never be included in a submission.

Official references: [Prepare an Extension for Store](https://developers.raycast.com/basics/prepare-an-extension-for-store), [Publish an Extension](https://developers.raycast.com/basics/publish-an-extension).
