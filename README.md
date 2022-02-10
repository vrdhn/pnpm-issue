# pnpm workspace typescript tool lifecycle

# Problem
`pnpm install` at top level, invokes the prepare step *after* installing the module.

    $ pnpm install
    Scope: all 2 workspace projects
    Lockfile is up-to-date, resolution step is skipped
    Packages: +1
    WARN Failed to create bin at node_modules/.bin/tool. The source file at node_modules/tool/dist/index.js does not exist.

    dependencies:
    + tool 1.0.0 <- tool

    tool prepare$ tsc
     Done in 964ms
    Progress: resolved 1, reused 1, downloaded 0, added 1, done

So, the tool is not available.

    $ pnpm tool

    > top@1.0.0 tool /home/v/x/x
    > tool

    sh: line 1: tool: command not found
    ELIFECYCLE Command failed.

However, a second run of `pnpm install` works, because of the files leftover from the delayed `prepare` step in previous run.
