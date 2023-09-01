# Managing Git repositories
## Why repositories get large?

- Long history.
- Large binary files.

## Workarounds
### Shallow clone

```sh
git clone --depth [depth] [clone-url]
```

### VFS for Git
VFS for Git helps with large repositories. It requires a Git LFS client.
Typical Git commands are unaffected, but the Git LFS works with the standard filesystem to download necessary files in the background when you need files from the server.

### Scalar
It achieves by enabling some advanced Git features, such as:

- **Partial clone**: reduces time to get a working repository by not downloading all Git objects right away.
- **Background prefetch**: downloads Git object data from all remotes every hour, reducing the time for foreground git fetch calls.
- **Sparse-checkout**: limits the size of your working directory.
- **File system monitor**: tracks the recently modified files and eliminates the need for Git to scan the entire work tree.
- **Commit-graph**: accelerates commit walks and reachability calculations, speeding up commands like git log.
- **Multi-pack-index**: enables fast object lookups across many pack files.
- **Incremental repack**: Repacks the packed Git data into fewer pack files without disrupting concurrent commands using the multi-pack-index.

## Purging repository data
If you commit sensitive data (for example, password, key) to Git, it can be removed from history. Two tools are commonly used:

- Git filter-repo tool.
- BFG Repo-Cleaner.

## Managing releases in GitHub
Releases in GitHub are based on Git tags.

### Creating a release
To create a release, use the `gh` release create command. Replace the tag with the desired tag name for the release and follow the interactive prompts.

```sh
gh release create tag
```

To create a prerelease with the specified title and notes.
```sh
gh release create v1.2.1 --title
```

