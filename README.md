# livelink-app-template-config

A repository to expose a public github page serving configuration files for player.demo.3dverse.dev &amp; player.demo.3dverse.com.

The application configuration is stored inside [config.json file](./public/config.json) and [json files insice the scenes directory](./public/scenes)
inside the [./public](./public) directory. The `config.json` file is the base default configuration of any scene including the scene list:

- The `scenes` array is the list of available scenes and each entry must have a `scenes/[scene].json` file.
- All the other propeties are scene configuration that can be overloaded in the `scenes/[scene].json` files.

Scene configuration:
| Config option key | Description |
| ------------- | ------------- |
| `public_token` | The public token. See your console.3dverse.com project. |
| `scene` | The asset UUIDs of the scene. See your console.3dverse.com project. |
| `character` | The asset UUIDs of the character scene. See your console.3dverse.com project. |
| `default_camera_mode` | A number to set the default came mdoe: <ul><li>0: none</li><li>1: chracter</li><li>2: orbital</li><li>3: fly</li></ul> |
| `visibility` | `"public" \| "private"` for the scene to be listed or not on home page. |
| `no_simulation_start` | default: `false`, set tot `true` to prevent auto start of the simulation so it can be started another way. |
| `click_to_focus_entity` | default: `false`, set tot `true` to select & auto focus an entity on a single click or tap. |
| `click_to_watch_entity_cameras` | default: `false`, set to `true` to watch the cameras of a picked entity in a mini viewport, and travel to that entity. Also required for the entity selector to react to a click. |
| `simulated_camera_step_back_factor` | default: `1.5`, how far the camera stops short of the camera pose of a simulated entity, along its forward axis, as a multiple of the size of that entity. Raise it to leave more room around the entity. See [Entity selector](#entity-selector). |
| `hidden_header` | default: `false`, set tot `true` to hide the the top header bar. |
| `entity_names.default_orbit_pivot` | The name of the entity whose local_transform is used as the default pivot point for the orbital camera. Shall be at the root of the scene graph.|
| `entity_names.character_spawn` | The name of the entity whose local_transform is used as the character spawn point. Shall be at the root of the scene graph.|
| `entity_names.default_camera_transform` | The name of the entity whose local_transform is used as the default transform of the regular camera, instead of the one of the settings of the scene asset.|
| `entity_tags.controller_script` | The value to search in the `tags` component of entities owning a `script_map` component, so those can be used as "controller script" to be used by the client.|
| `entity_selector` | An array of entities to always list in the entity selector panel. See [Entity selector](#entity-selector).|
| `entity_selector[].entity_id` | The UUID of the entity. |
| `entity_selector[].linkage` | Optional array of UUIDs identifying the instance of the entity when it comes from a sub scene reference. |
| `entity_selector[].label` | Optional text of the entry, defaults to the name of the entity. |
| `entity_selector[].simulated` | default: `false`, set to `true` when the entity is driven by the simulation. See [Entity selector](#entity-selector).|
| `camera.default_speeds` & `camera.default_sensitivity` | The speeds in meters per second:<br><ul><li> `regular` is used by default.</li><li>`focused` is used when the user focus to a label point of view.</li><ul> |
| `info.title` | The title of the scene. |
| `info.description` | The description of the scene. |
| `info.steps` | An array of actions suggested to exlore the scene. |
| `info.steps.id` | The id of the action. |
| `info.steps.title` | The title of the action. |
| `info.steps.description` | The description of the action. |
| `info.tags` | An array of tags related to the scene. |

### Entity selector

The entity selector panel lists entities of interest. Clicking one of them
watches its cameras in a mini viewport, when it has any, and travels the main
camera to it. It requires `click_to_watch_entity_cameras` to be enabled.

Its entries come from two places:

-   the `entity_selector` key of the scene config, always listed;
-   the `tags` component of the label entities, listed under the name of the last
    focused label.

A label declares the entities it relates to with tags of the form:

```
entity:<uuid>[:<linkage-uuid>[,<linkage-uuid>...]]
entity-simulated:<uuid>[:<linkage-uuid>[,<linkage-uuid>...]]
```

where the linkage part is optional and only needed for an entity coming from a
sub scene reference. A label may declare several of them, and the `label`
component is still required: the entities are related to that label.

Depending on how many entity tags a label declares, clicking it:

-   **none**: travels to the point of view of its `label` component, as usual;
-   **exactly one**: ignores the camera transform of its `label` component and
    travels to the declared entity instead;
-   **several**: travels to the point of view of its `label` component and lists
    the declared entities in the entity selector, for the user to pick one.

The `entity-simulated:` form, like `entity_selector[].simulated`, flags an
entity driven by the server side simulation. Such an entity must own at least
one camera as a direct child, and the camera transforms are not broadcast to the
entities for performance reasons: the pose to travel to is read from the meta
data of the rendered frames instead. The camera then stops short of that pose
along its forward axis, by `simulated_camera_step_back_factor` times the size
of the entity, so it doesn't end up inside its geometry.
