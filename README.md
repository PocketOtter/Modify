# Modify

A client-side in-game mod browser, installer, and updater for **Fabric on Minecraft 1.21.4** -
think Resourcify, but for mods instead of resource packs/shaders/datapacks. Search, browse,
install, and update Fabric mods from [Modrinth](https://modrinth.com) without leaving the game.

## Requirements

- **Java 21** (a JDK, not just a JRE)
- **[Fabric Loader](https://fabricmc.net/use/)** 0.16.10+
- **[Fabric API](https://modrinth.com/mod/fabric-api)** (required)
- **[Mod Menu](https://modrinth.com/mod/modmenu)** (optional - adds a settings entry; Modify works fine without it)

## Building

This repo doesn't ship a Gradle wrapper jar (it's a binary file, generated locally instead of
checked in). Pick one:

- **IntelliJ IDEA (recommended):** File -> Open -> select this folder. IntelliJ detects the
  Gradle project and offers to use its own bundled Gradle automatically.
- **Command line:** install Gradle 8.11+ once (e.g. via [SDKMAN](https://sdkman.io)), then from
  this folder:
  ```
  gradle wrapper --gradle-version 8.11.1
  ./gradlew build
  ```
  That generates `gradlew`/`gradlew.bat`/the wrapper jar locally, so future builds don't need
  Gradle installed separately. The finished mod jar lands in `build/libs/`.

To run a dev instance directly instead of building a jar:
```
./gradlew runClient
```
This launches Minecraft 1.21.4 with Modify, Fabric API, and Mod Menu all loaded.

## Installing (once built)

Drop the jar from `build/libs/modify-<version>.jar` into your `mods/` folder alongside Fabric
Loader and Fabric API, same as any other Fabric mod.

## Using Modify

- **Open the browser:** click the **Modify** button in the top-right corner of the title screen or
  the pause menu (Escape while in a world), press its keybinding (unbound by default - set one in
  *Options -> Controls -> Key Binds -> Modify* to open it from gameplay, including with the pause
  menu already open), or use Mod Menu's entry for Modify.
- **Search and filter:** type to search (results update automatically), and use the **Sort** and
  **Category** buttons to narrow things down. **Prev**/**Next** page through results.
- **Install or update:** click a result to open its page - description, image gallery, every
  Fabric/1.21.4-compatible version, and its dependencies. Pick a version and hit the big button at
  the bottom, or just hit **Install**/**Update** directly on a card in the list. Required
  dependencies are resolved and installed automatically (toggle this in settings).
- **Restart when prompted:** installing or updating only takes effect on the next launch. A
  **Restart Game** button is always available on both the browser and detail screens - click it,
  confirm, and Modify closes Minecraft and automatically relaunches it with the same arguments it
  was started with (best-effort - see `docs/ARCHITECTURE.md` for what that does and doesn't cover
  across launchers). The title-screen and pause-menu buttons flag whether a restart is actually
  pending (and show a mod update count, if any turned up during Modify's optional startup check),
  so you'll know without a permanent on-screen banner nagging about it.
- **Settings:** open Modify's entry in Mod Menu for a small settings screen (default sort, whether
  to hide beta/alpha versions, auto-resolve dependencies, check for updates on startup).

## Project structure

```
com.modify
├── api/          Modrinth REST client + data models
├── download/      download -> hash-verify -> install pipeline
├── update/        installed-mod detection + update checking
├── gui/           the two screens + their custom list widgets
├── config/        persisted settings
├── util/          paths, async helpers, keybindings, game restart, local mod scanning
├── mixin/         title-screen entry point
└── integration/   Mod Menu entry point
```

See [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) for the full system-by-system breakdown,
including the "still to do" list and an important note on this project's mappings/compile-check
limitations - the short version: this was built and reviewed carefully but **without a live
Gradle/Loom workspace or a JDK compiler available to verify it against**, so treat the first
`./gradlew build` as the real first test, not a formality.

## License

MIT - see `LICENSE`.
