
# Publish page
```
npx dendron publish init # init the publish
```

This will pull the [`nextjs-template`][nextjs-template] github repo to `<workspace>/.next` folder

[publish command file entry point][publishCLICommand] 
First, it will init the git repo
* initialize the `nextPath` (meaning the root path of this dendron project) if not initialized
* clone the [nextjs-template] with [NextjsExportPod] to root path of `.next` folder
* then install dependencies -> `npm install`

If initialized,
* it will update the [nextjs-template] repo instead,
* pull the latest commit of the main branch.
* then install dependencies -> `npm install`


[publishCLICommand]: https://github.com/dendronhq/dendron/blob/master/packages/dendron-cli/src/commands/publishCLICommand.ts
[NextjsExportPod]: https://github.com/dendronhq/dendron/blob/cfffd6a1988c372f7472bb2cd93126befd866a0d/packages/pods-core/src/builtin/NextjsExportPod.ts
[nextjs-template]: https://github.com/dendronhq/nextjs-template

```
npx dendron publish dev
```
The above will call `next dev` in [nextjs-template] repo. Test your built web page in development

```
npx dendron publish export
```
The above will call `next export` (meaning `next build && next export`) in [nextjs-template] repo.

```
npx dendron publish export --target github --yes
```
With `--target github` flag, this will create `docs/` folder and move the built app - `.next/out/` folder to `docs/` folder.
`--yes` is used to automatically remove `docs/` folder if existed.


### Found bug on github publish
-> https://github.com/dendronhq/nextjs-template/blob/main/pages/_app.tsx
change the 
```md
<link rel="icon" href="/favicon.ico" />
```
to (remove the '/' as Github takes the same level as `index.html` instead of root level (which is '/' of `/home/worker/<repo-name>/<repo-name>/<publish_dir>`))
```md
<link rel="icon" href="favicon.ico" />
```
Reference: [(CI/CD): Deploy Next.js (SSG) to GitHub Pages using GitHub Actions](https://youtu.be/yRz8D_oJMWQ?t=1068)
