# livelink-app-template-config
A repository to expose a public github page serving configuration files for player.demo.3dverse.dev &amp; player.demo.3dverse.com.

The configuration is stored inside [config.json file](./public/config.json) and [json files insice the scenes directory](./public/scenes).
The `config.json` file is the base default configuration of any scene including the scene list:

-   The `scenes` array is the list of available scenes and each entry must have a `./public/config/[scene].json` file.
-   All the other propeties are scene configuration that can be overloaded in the `./public/config/[scene].json` files.

Scene configuration:
| Config option key | Description |
| ------------- | ------------- |
| `scene` | The asset UUIDs of the scene. See your console.3dverse.com project. |
| `character` | The asset UUIDs of the character scene. See your console.3dverse.com project. |
| `public_token` | The public token. See your console.3dverse.com project. |
| `entity_names.default_orbit_pivot` | The name of the entity whose local_transform is used as the default pivot point for the orbital camera. Shall be at the root of the scene graph.|
| `entity_names.character_spawn` | The name of the entity whose local_transform is used as the character spawn point. Shall be at the root of the scene graph.|
| `entity_names.default_camera_transform` | The name of the entity whose local_transform is used as the default transform of the regular camera, instead of the one of the settings of the scene asset.|
| `entity_tags.controller_script` | The value to search in the `tags` component of entities owning a `script_map` component, so those can be used as "controller script" to be used by the client.|
| `camera.default_speeds` & `camera.default_sensitivity` | The speeds in meters per second:<br><li> `regular` is used by default<li>`focused` is used when the user focus to a label point of view. |
