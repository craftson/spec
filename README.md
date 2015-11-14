# craftson (craft.json) spec

## Rationale
Every Minecraft modding platform has designed some sort of way for packages (plugins or mods) to
bundle information when they are distributed. Forge has mcmod.info, Sponge has the `@Plugin`
annotation, and Bukkit and Spigot have plugin.yml. While the individual systems are different, they
all rely on distributing packages in JARs with some sort of description file. This file is consumed
by the modding platforms themselves, as well as other systems that deal with the distribution,
storage, or management of packages. craft.json is a platform-neutral description file for
Minecraft packages that unifies these efforts. It is designed to be simple to read, simple to
create, and simple to consume.

## Specification
As by the name, craft.json is a [JSON](http://json.org/) file. It is located in the root of an
archive file, most commonly JAR or ZIP. The root of the file is often a JSON object, called the
_package object_. The package object describes the package within the archive file. Several fields
in the package object format are required, and most are optional.

### Required Fields in a Package Object

- `"id"` - String. The machine-readable name of the package. Must be an alphanumeric string
including dashes and underscores, without spaces. In other words, must match the regex
`/[a-zA-Z0-9-_]+/`.

- `"group"` - String. The semi-unique identifier that identifies the group behind the package. It
is recommended to use a domain name owned by the maintainer(s) of the package, like
`"org.example"`.

- `"version"` - String. A human-readable version descriptor of the package. It is _highly_
recommended to use [semver](http://semver.org/) for describing versions (in order to allow versions
to be easily compared and ordered), but it is not necessary. A version could also be an incrementing
build number.

#### Example

```js
{
  "id": "my-package",
  "group": "com.example",
  "version": "2.3.0"
}
```

### Optional Fields in a Package Object

- `"title"` - String. A short, human-readable name. How the project is referred to in speech or
text. There are no restrictions on length; but it is recommended to keep the title under 50
characters.

- `"description"` - String. A human-readable description of the package. There are no restrictions
on length; however, the length of the description should be reasonable, or within 500 characters.
Helps people to discover the package.

- `"links"` - Object with values as strings. Describes relevant links to the project. Each value of
the object is a URI describing the location of a specific link. There are several recommended links
to associate with the package:

  - `"homepage"` - The homepage of the package.
  - `"issues"` - Where to report issues.
  - `"sources"` - The location of the sources of the project. It is recommended to refer a source
  control repository, such as a git repository.

- `"dependencies"` - Array of arrays. Describes the dependencies on other packages. Each element in
the dependencies array describes a different package dependency. Absence of the `"dependencies"` key
implies that the package has no dependencies.

  Each individual dependency is an array of strings, containing the group, id, and version of that
  dependency, in that order. The version of a dependency may be excluded, indicating that any
  version of that dependency is compatible with the package.

- `"authors"` - Array of strings. Describes the direct authors or maintainers of the package. Each
string in the array describes a different author, and must adhere to the _person/entity format_.

- `"contributors"` - Array of strings. Describes contributors who helped in the creation or
maintenance of the package, but are not considered to be its authors or maintainers. Each string in
the array describes a different contributor, and must adhere to the person/entity format.

- `"license"` - String. The license that files in the package are under. Must be either a license
identifier from https://spdx.org/licenses/ or a link to a webpage that describes the license, like
the license file.

#### Person/Entity Format

```
name <email> (website)
```

- The `name` describes the name or username of the author. It is required.
- The `email` describes the email address of the author. It must be enclosed in horizontal carets.
It is not required.
- The `website` describes the website or page of the author. It must be enclosed in parentheses.
It is not required.

Only the `name` is required. If other elements are given, they must be in the order of: name, email
if it is given, and website if it is given.

#### Example

```js
{
  "title": "My Package",
  "description": "An example craft.json file that shows how craft.json is used.",
  "links": {
    "homepage": "https://example.com/my-package",
    "issues": "https://github.com/example-owner/my-package/issues",
    "sources": "https://github.com/example-owner/my-package"
  },
  "dependencies": [
    ["org.other-example", "other-package", "0.5.6"],
    ["com.example-three", "yet-another-package", "2.4.0"]
  ],
  "authors": [
    "My Organization <contact@example.com> (http://example.com)",
    "A Fake Person (http://example-web.org)",
    "Another Fake Person <fakeemail@example.org>",
    "Just a Fake Person"
  ],
  "license": "MIT"
}
```

### Specifying Multiple Packages

Specify multiple packages by using an array at the root of the craft.json file. Each object inside
the array must be a package object, and follow the package object format. There is no limit to the
number of package objects that may be within the array.

#### Example

```js
[
  {
    "id": "package-one",
    "group": "com.example",
    "version": "2.3.0",
    "license": "MIT"
  },
  {
    "id": "package-other",
    "group": "org.my-group",
    "version": "0.0.1",
    "description": "Another package"
  }
]
```
