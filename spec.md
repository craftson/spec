```js
{
  "name": "my-plugin",
  "owner": "gratimax",
  "version": "1.0-SNAPSHOT",
  "dependencies": [
    ["Lapis", "Commons", "1.0-SNAPSHOT"]
  ],
  "maven": {
    "repositories": {
      "sponge": "https://repo.spongepowered.org/maven",
      "central": "https://central.maven.org"
    },
    "dependencies": [
      ["org.spongepowered", "SpongeAPI", "1.0.0-SNAPSHOT"],
      ["org.gradle", "gradle", "my-version"],
      ["org.hamcrest", "core", "0.0.1"],
      ["com.legious", "grid", "1.0-SNAPSHOT"]
    ]
  }
}
```