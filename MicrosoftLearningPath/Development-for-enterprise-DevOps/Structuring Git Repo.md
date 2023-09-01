# Structuring Git Repo
## Implementing a changelog
The typical breakdown is to separate a list of versions, and then within each version, show:

- Added features.
- Modified/Improved features.
- Deleted features.

### Changelog using git

```
git log [options] vX.X.X..vX.X.Y | helper-script > projectchangelogs/X.X.Y
```

### Using GitHub Changelog Generator

```
$ github_changelog_generator -u github-changelog-generator -p TimerTrend-3.0
```