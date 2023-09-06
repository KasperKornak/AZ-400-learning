# Implementing a versioning strategy
## Release views
Feeds in **Azure Artifacts** have three different views by default. These views are added when a new feed is created. The three views are:

- **Local**: The `@Local` view contains all release and prerelease packages and the packages downloaded from upstream sources.
- **Prerelease**: The `@Prerelease` view contains all packages that have a label in their version number.
- **Release**: The `@Release` view contains all packages that are considered official releases.

### Using views
You can use views to offer help consumers of a package feed filter between released and unreleased versions of packages. Essentially, it allows a consumer to make a conscious decision to choose from released packages or opt-in to prereleases of a certain quality level.

By default, the `@Local` view is used to offer the list of available packages. The format for this URI is:

```html
https://pkgs.dev.azure.com/{yourteamproject}/_packaging/{feedname}/nuget/v3/index.json
```

## Promoting packages
Release and Prerelease's two views might be sufficient, but you can create more views when you want finer-grained quality levels if necessary, such as `alpha` and `beta`.

**Packages** will always show in the Local view, but only in a particular view after being promoted. Depending on the URL used to connect to the feed, the available packages will be listed.

**Upstream** sources will only be evaluated when using the @Local view of the feed.