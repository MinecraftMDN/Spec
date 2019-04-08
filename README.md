# Spec

WORK IN PROGRESS, SPEC VERSION 0

#### Notes
- All keys use camelCase, only [a-zA-Z0-9].
- All values follow the pattern `todo` unless otherwise noted.
- All values marked as `Required` are **Required**.
- All values marked as `IgnoreCase` are checked with `equalsIgnoreCase` or equivalent.

## Mod File

- `specVersion` (Integer) (Required): The spec version this file follows.
- `type` (String) (IgnoreCase): The type this "mod" is, `mod`, `modloader`, `jarmod`. Defaults to `mod`
- `id` (String) (Required): A unique id that identifies this mod within the network. Does not have to be the mod id but it can be.
- `name` (String): A simple name that can be displayed to the user.
- `description` (String): A short description on what the mod does or is.
- `license` (String or Array of Strings): The license identifier(s) the mod is licensed after (Use https://spdx.org/licenses/ to lookup yours).
- `authors` (Array of Strings): An array of names of people who are considered authors of this mod.
- `projectLinks` (Array): An Array of ProjectLink Objects, that contains urls that could be displayed to the user.
  ##### ProjectLink Object
  - `type` (String) (Required) (IgnoreCase): An identifier that describes the url, such as `Discord` for a discord invite link.
  - `url` (String) (Required): A url that the user can go to. Follows the pattern `todo`.

- `indexers` (Array): An Array of Indexer Objects, that contains information on how mod versions could be automatically discovered by a bot.
  ##### Indexer Object
  - `type` (String) (Required) (IgnoreCase): The identifier of the service to search, such as `Curseforge`.
  - `id` (String) (Required): The identifier or key to use when searching for mod versions, such as the Curseforge project id.
  - `regex` (String): A regex pattern used when detecting and matching applicable mod versions.


## Version File

- `specVersion` (Integer) (Required): The spec version this file follows.
- `id` (String) (Required): A mod unique identifier that identifies this version within the network. Should match the version found within the file.
- `changelog` (String): A url to a changelog file.
- `side` (String): One of `client`, `server` or `universal`. Default is universal
- `relationships` (Array): An Array of Relationship Objects.
  ##### Relationship Object
  - `type` (String) (Required) (IgnoreCase): Either `required`, `recommended`, `suggested`, `conflicts`, `breaks`.
  - `id` (String) (Required): The mod id.
  - `version` (String or Array of Strings): The version to match. See `Version Match` below for more information

- `artifacts` (Array) (Required): An Array of Artifact Objects.
  ##### Artifact Objects
  - `type` (String) (Required) (IgnoreCase): The type of download or download service the artifact can be gotten from, such as `direct` for a direct url download.
  - `id` (String) (Required): The id that identifies the artifact with the service, could be a URL/file id/maven id.

- `hashes` (Array) (Required): An Array of Hash Objects, used to verify downloaded artifact.
  ##### Hash Object
  - `type` (String) (Required) (IgnoreCase): The type of hash this is, for example `sha256`.
  - `id` (String) (Required): The hash.


## Modloader Extensions (Extends Version File)
WIP
- MainClass
- Libraries
- 

## Extra Information
### Version Match
WIP
maybe something like https://fabricmc.net/wiki/format:modjson#versionrange

##### Fabric
Fabric is split into three parts:
- Fabric Loader (fabricloader) (loads fabric/mappings/mods)
- Yarn (yarn) (mappings)
- Fabric API (fabric) (useful api for mod developers, not required but almost always used)

##### Forge
Forge requires alot more than fabric, investigation pending...
- https://github.com/MinecraftForge/Installer
- https://github.com/MinecraftForge/InstallerTools

### TODO/Notes
- Modloader spec requirements
- Alpha/Beta/Stable/Recommended version types
- `mods` to `packages`, since a package could be not a `mod`
- `specVersion` vs `formatVersion`
- `id` vs `uid`
- `modname.` vs `package.`
- `versions.json` vs `index.json`
