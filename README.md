# plugson (plugin.json) spec

## Rationale
Every Minecraft modding platform has designed some sort of way for plugins or mods to bundle information when they are distributed. Forge has mcmod.info, Sponge has the plugin annotation, and Bukkit and Spigot have plugin.yml. While the individual systems are different, they all rely on distributing plugins or mods in JARs with some sort of description file. This file is consumed by the modding platforms themselves, as well as other systems that deal with the distribution, storage, or management of plugins and mods. plugin.json is a platform-neutral description file for Minecraft plugins and modifications that unifies these efforts. It is designed to be simple to read, simple to create, and simple to consume.

## Specification
As by the name, plugin.json is a [JSON](http://json.org/) file. It is located in the root of a plugin or mod JAR file. The root of the file is a JSON object, with several keys and values. Some of these are required and some of these are not.

### Required Keys

- `"name"` - String. The name of the mod or plugin. Must be an alphanumeric string including dashes and underscores, without spaces. In other words, must match the regex `/[a-zA-Z0-9-_]+/`.

- `"owner"` - String. The name of the owner of the mod or plugin. This can be the sole developer of the mod or plugin, the name of the organization that maintains the mod or plugin. This field must be semi-unique. It is recommended to use a group id or domain name owned by the maintainer(s) of the mod or plugin, like `"org.example"`.

- `"version"` - String. A human-readable version descriptor of the mod or plugin. It is _highly_ recommended to use [semver](http://semver.org/) for describing versions (in order to allow versions to be easily compared and ordered), but it is not necessary. A version could also be an incrementing build number.

#### Example

```js
{
  "name": "my-plugin",
  "owner": "com.example",
  "version": "2.3.0"
}
```

### Optional Keys

- `"description"` - String. A human-readable description of the plugin. There are no restrictions on length; however, the length of the description should be reasonable (within 500 characters). Helps people to discover the mod or plugin.

- `"keywords"` - Array of strings. A list of keywords describing the plugin. Also aids people in discovering the mod or plugin. -- _This may be unecessary bloat._

- `"links"` - Object. Describes relevant links to the project. Each value of the array is a URI describing the location of a specific link. There are several recommended links to associate with the mod or plugin:

  - `"homepage"` - The homepage of the mod or plugin.
  - `"issues"` - Where to report issues.

- `"dependencies"` - Array of arrays. Describes the dependencies on other mods or plugins. Each element in the dependencies array describes a different mod or plugin dependency. Each individual dependency is an array of strings, containing the owner, name, and version, in that order. Absence of this key implies that the plugin has no dependencies.

#### Example

```js
{
  "description": "An example plugin.json file that shows how plugin.json is used.",
  "keywords": ["example", "plugin.json", "plugson", "usage"],
  "links": {
    "homepage": "https://example.com/my-plugin",
    "issues": "https://github.com/example-owner/my-plugin/issues"
  },
  "dependencies": [
    ["org.other-example", "other-plugin", "0.5.6"],
    ["com.example-three", "yet-another-plugin", "2.4.0"]
  ]
}
```

### The 'maven' key

#### Example

```js
{
  "maven": {
    "repositories": {
      "example-repo": "https://example-repo.com/repo",
      "central": "https://central.maven.org"
    },
    "dependencies": [
      ["org.example", "ExampleAPI", "1.0.0-SNAPSHOT"],
    ]
  }
}
```
