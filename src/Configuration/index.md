# Configuration

To change Vinegar's behavior, edit the `config.toml` file at `~/.config/vinegar/config.toml` (or `~/.var/app/io.github.vinegarhq.Vinegar/config/vinegar/config.toml` if you're using the Flatpak), ensure that you have created the configuration directory first, as it is not created by default (eg. `~/.config/vinegar`)

For your convenience, you may use the editor feature by running `vinegar edit` (or `flatpak run io.github.vinegarhq.Vinegar edit` if you're using the Flatpak).

Anything added to the configuration file is an override over the default values (the default configuration file can be seen [here](https://github.com/vinegarhq/vinegar/blob/master/internal/config/config.toml)) which are designed to be the best for the average user.

> **Note:** You only need to edit the configuration file if you want to fine tune Roblox's behavior. Having a configuration file is completely optional!

## Configuration Values
This section will explain what each value in the global configuration file represents.

| Option        | Description                                                                                                | Default   |
| ------------- | ---------------------------------------------------------------------------------------------------------- | --------- |
| `launcher`    | the program that is used to launch Wine when launching Roblox; this can be set to `gamemoderun`.           | none      |
| `wineroot`    | the path to a valid Wine 'root' installation directory.                                                    | none      |

For Studio or Player configurations, you will need to specify it as a table, an example can be seen below.

| Option             | Description                                                                                     | Default                         |
| ------------------ | ------------------------------------------------------------------------------------------------| ------------------------------- |
| `channel`          | the Roblox release channel                                                                      | `"live"`                        |
| `renderer`         | selects the rendering engine to be used by Roblox via FFlags.                                   | `"D3D11"`                       |
| `forced_version`   | forces Vinegar to use a specific version, the release channel must be adjusted for the version. | none                            |
| `auto_kill_prefix` | tells Vinegar to automatically kill the wineprefix after the application closes.                | Player: `true`, Studio: `false` |
| `dxvk`             | automatically uses DXVK for the application and installs if necessary.                          | Player: `true`, Studio: `false` |
| `[app.fflags]`     | the table used to set FFlags for the given application type.                                    | See below.                      |
| `[app.env]`        | the table used to set environment variables for the given application type.                     | Player: `DXVK_HUD=fps`          |

### Notes
* `renderer` must be one of the following: `"OpenGL"`, `"D3D11FL10"`, `"D3D11"`, `"Vulkan"`.
* Ensure that when setting a string, the value must be in quotes: `channel = "zintegration"`
* When using DXVK, ensure that the renderer is `"D3D11"`, otherwise Roblox will not utilize DXVK.
* `app` in `[app.fflags]` or `[app.env]`, should be renamed to `player` or `studio`, eg. `[player.fflags]`.
* `DFIntTaskSchedulerTargetFps` is set to `640` by default for Player; if you also want to change the FPS limit you may set the FFlag's value to your requested limit (eg. `30`)
* If you're using the Flatpak, ensure that the path of the `wineroot` configuration option is allowed access from the Flatpak, as if it is a path outside of `~/.var/app/io.github.vinegarhq.Vinegar` Vinegar won't be able to access the directory: `flatpak override --user --filesystem=/path/to/wineroot`

### Example configuration

> **Note:** The following configuration file is not meant to be copied or used. It is only shown for demonstrating how the configuration values above are laid out in the actual file.

```toml
wineroot = "/home/meow/wine-ge"
launcher = "gamemoderun"

[env]
WINEFSYNC = "1"

[player]
dxvk = false
renderer = "Vulkan"
channel = "zcanary"

[player.env]
DXVK_HUD = "0"

[player.fflags]
DFIntTaskSchedulerTargetFps = 144

[studio]
renderer = "OpenGL"
```
