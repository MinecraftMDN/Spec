# Spec

WORK IN PROGRESS, SPEC VERSION 0.3

#### Notes
- All files are JSON files with the `.json` extension.
- All keys use camelCase, only [a-zA-Z0-9].
- All values follow the pattern `todo` unless otherwise noted.
- All values default to an empty String or 0, unless otherwise noted.
- All values marked as `Required` are **Required**.
- All values marked as `IgnoreCase` are checked with `equalsIgnoreCase` or equivalent.
- See `Extra Information` for information on preset values.

A Package could represent a Mod, Mod Loader, Library, or anything else that has `artifacts` that could be downloaded, installed or used.
A Version represents a single artifact, such as a .jar file.

## ALL Files
###### These are things EVERY file MUST contain.
- `specVersion` (Integer) (Required): The spec version the file follows.


## Package File
###### package.json inside a folder named after the `id` of the `Package`.

- `type` (String) (IgnoreCase): The type of `Package` this represents. See `Package Type` for more information.
- `id` (String) (Required): A unique id that identifies this `Package` within the network. Does not have to be the mod id.
- `name` (String): A simple name that can be displayed to the user.
- `description` (String): A short description on what the `Package` is.
- `license` (String or Array of Strings): The license identifier(s) the `Package` `Artifacts` are licensed under (Use https://spdx.org/licenses/ to lookup yours).
- `authors` (Array of Strings): An array of names of people who are considered authors of this `Package`.
- `projectLinks` (Array): An Array of ProjectLink Objects, that contains urls that could be displayed to the user.
  ##### ProjectLink Object
  - `type` (String) (Required) (IgnoreCase): An identifier that describes the url, such as `Discord` for a discord invite link.
  - `url` (String) (Required): A url that the user can go to. Follows the pattern `todo`.
- `detectors` (Array): An Array of Detector Objects, that contains information on how `Artifacts` could be automatically discovered by a bot that generates `Version` files.
  ##### Detector Object
  - `type` (String) (Required) (IgnoreCase): The identifier of the detector service.
  - `data` (Object) (Required): Service specific data that is used by the service. This is outside the control of this spec.


## Version File
###### id.json inside the `Package` folder. Where `id` is the version id.

- `id` (String) (Required): A `Package` unique identifier that identifies this `Version` within the network. Should match the `id` of the file.
- `releasetype` (String): One of `release` or `beta`.
- `changelog` (String): A url to a changelog file.
- `side` (String): One of `client`, `server` or `universal`. Default is universal.
- `relationships` (Array): An Array of Relationship Objects.
  ##### Relationship Object (See Relationship Management for more information)
  - `type` (String) (Required) (IgnoreCase): Either `required`, `recommended`, `suggested`, `conflicts`, `breaks`.
  - `id` (String) (Required): The `Package` id.
  - `version` (String or Array of Strings): The version(s) to match. See `Version Match` below for more information.
- `artifacts` (Array) (Required): An Array of `Artifact` Objects.
  ##### Artifact Objects
  - `type` (String) (Required) (IgnoreCase): The type of download or download service the `Artifact` can be gotten from, such as `direct` for a direct url download.
  - `id` (String) (Required): The id that identifies the `Artifact` with the service, could be a URL/file id/maven id.
- `hashes` (Array) (Required): An Array of Hash Objects, used to verify downloaded artifact.
  ##### Hash Object
  - `type` (String) (Required) (IgnoreCase): The type of hash the hash is, for example `sha256`.
  - `id` (String) (Required): The hash.


## (WIP) Modloader Version Extensions
###### Extends Version File. Package MUST have `type` set to `modloader`
- `mainclass` (Object) (IgnoreCase): 
  - `common`: Main class used for both client and server.
  - `client`: Main class used for client only.
  - `server`: Main class used for server only.
- `libraries` (Array): Array of `Library` Objects.
  ##### Library Object
  - `id` (String) (Required): Maven ID.
  - `url` (String) (Required): Maven Repository its stored in.
  - `side` (String): Either `client`, `server`, or `common`. Default `common`.
  - `hashes` (Array) (Required): An Array of Hash Objects, used to verify downloaded artifact. At least one hash MUST be provided. Prefer sha256.
    ##### Hash Object
    - `type` (String) (Required) (IgnoreCase): The type of hash the hash is, for example `sha256`.
    - `id` (String) (Required): The hash.
- `launchargs` (Object):
  - `client` (String): Launch arguments for the client only.
  - `server` (String): Launch arguments for the server only.
  - `common` (String): Launch arguments for the client & server.
- `tweakers` (Object):
  - `client` (Array of Strings): Tweakers for the client only.
  - `server` (Array of Strings): Tweakers for the server only.
  - `common` (Array of Strings): Tweakers for the client & server.

## (WIP) Minecraft Version Extensions
###### Extends Version File. Package MUST have `type` set to `minecraft`.
- `releasetype` (String): One of `release`, `snapshot`, `beta`, `alpha`.
- `mainclass` (Object) (IgnoreCase): 
  - `common`: Main class used for both client and server.
  - `client`: Main class used for client only.
  - `server`: Main class used for server only.
- `libraries` (Array): Array of `Library` Objects.
  ##### Library Object
  - `id` (String) (Required): Maven ID.
  - `url` (String) (Required): Maven Repository its stored in.
  - `side` (String): Either `client`, `server`, or `common`. Default `common`.
  - `hashes` (Array) (Required): An Array of Hash Objects, used to verify downloaded artifact. At least one hash MUST be provided. Prefer sha256.
    ##### Hash Object
    - `type` (String) (Required) (IgnoreCase): The type of hash the hash is, for example `sha256`.
    - `id` (String) (Required): The hash.
- `launchargs` (Object):
  - `client` (String): Launch arguments for the client only.
  - `server` (String): Launch arguments for the server only.
  - `common` (String): Launch arguments for the client & server.

## Extra Information
### Mod Type
- `Mod`: The default type. The `Artifact` would be placed in the `mods` folder.
- `Modloader`: Specifies this is a modloader and uses the `Modloader Version Extensions` spec.
- `Library` (WIP): If a mod is declared as a Library, then it can only be used by modloaders using the `Libraries` key in the `Modloader Version Extensions` spec.
- `Minecraft`: Only used by Minecraft files.
It would allow the user to select a different version, but CANNOT be installed manually.

### Relationship Management
There are 5 types of relationships a `Package` can have.
(X is the dependency, Y is the dependent)
- `Required`: X MUST be included for a successful install.
- `Recommended`: X SHOULD be included, a warning message should be shown to the user asking confirmation to install X along with Y.
- `Suggested`: If X is included, use the version match provided.
- `Conflicts`: If X is included, a warning message should be shown to the user stating there may be a conflict between X and Y.
- `Breaks`: If X is included, a error message should be shown to the user stating Y can not be installed with X.

### (WIP) Version Match
maybe something like https://fabricmc.net/wiki/format:modjson#versionrange


### TODO/Notes
- Modloader spec requirements
- Modpack spec
- Alpha/Beta/Stable/Recommended version types
- Tags for Mods
- `specVersion` vs `formatVersion`
- `id` vs `uid`
- hash to package+version database (something like "sha256:somehash":"amod:1.0.0")
- cf slug to package database
- cf id to package database
- Remove `discoverers` from the spec and only use for upstream/intermediary usage?

##### Fabric
Fabric is split into three parts:
- Fabric Loader (fabricloader) (loads fabric/mappings/mods)
- Yarn (yarn) (mappings)
- Fabric API (fabric) (useful api for mod developers, not required but almost always used)
Fabric Loader will need a custom script to get all the information required.

##### Forge
Forge requires alot more than fabric, investigation pending...
- https://github.com/MinecraftForge/Installer
- https://github.com/MinecraftForge/InstallerTools
