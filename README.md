## Documentation

This repository is the Unreal integration into Autodesk's Flow Production Tracking (formerly ShotGrid) Pipeline Toolkit and contains the source code for SG TK (ShotGrid Tank) Unreal engine.

The simplest way to add the Flow Production Tracking Toolkit integration to Unreal is to start from the available standard Toolkit configurations:
- Unreal default2 configuration, based on the Flow Production Tracking Toolkit tk-config-Default2 configuration: https://github.com/ue4plugins/tk-config-unreal
- Unreal basic configuration, based on the Flow Production Tracking Toolkit tk-config-basic configuration: https://github.com/ue4plugins/tk-config-unrealbasic

However, it is possible to manually add the integration to an existing Flow Production Tracking configuration from the files provided by the [default2 based Unreal config](https://github.com/ue4plugins/tk-config-unreal) or the [basic based Unreal config](https://github.com/ue4plugins/tk-config-unrealbasic).

### Configuring the file system schema for a default2 base configuration
If your Flow Production Tracking configuration needs a file system schema, copy the following files and directories from `core/schema` of [this config](https://github.com/ue4plugins/tk-config-unreal) under the `core/schema` folder of your existing configuration.
```
schema/
└── project
    ├── assets
    │   └── asset_type
    │       └── asset
    │           └── step
    │               ├── publish
    │               │   └── fbx
    │               │       └── placeholder
    │               └── work
    │                   └── fbx
    │                       └── placeholder
    └── sequences
        └── sequence
            └── editorial
                └── placeholder
```

### Adding the Flow Production Tracking Unreal components
Both configs have a self-contained `env/includes/unreal` folder, which contains all definitions needed by the integration
```
unreal/
├── frameworks.yml
├── settings
│   ├── tk-multi-loader2.yml
│   ├── tk-multi-publish2.yml
│   ├── tk-multi-shotgunpanel.yml
│   └── tk-unreal.yml
├── templates.yml
└── tk-unreal-location.yml
```

This folder should be copied to the `env/includes` folder of your existing configuration.

Then the following files (or similar) need to be modified to add the Unreal integration:

- *core/templates.yml*:
  - add `include: ../env/includes/unreal/templates.yml`
- *env/project.yml*: 
  - add `- ./includes/unreal/settings/tk-unreal.yml` to *includes*
  - add `tk-unreal: "@settings.tk-unreal.project"` to *engines*
- *env/asset_step.yml*: 
  - add `- ./includes/unreal/settings/tk-unreal.yml` to *includes*
  - add `tk-unreal: "@settings.tk-unreal.asset_step"` to *engines*
- *env/includes/settings/tk-maya.yml* or *env/includes/maya/asset_step.yml*:
  - add `- ../unreal/settings/tk-multi-publish2.yml` to *includes*
  - replace *tk-multi-publish2* asset step setting with: `tk-multi-publish2: "@settings.tk-multi-publish2.maya.asset_step.unreal"`
- *env/includes/frameworks* or *env/includes/common/frameworks.yml*:
  - add `include: unreal/frameworks.yml` 
- *env/includes/settings/tk-desktop.yml* or *env/includes/desktop/project.yml*: 
  - add "*Unreal*" to Creative Tools group.



## See also:
For more information on how to run the Unreal Flow Production Tracking integration, please see the [support documentation](https://docs.unrealengine.com/5.0/en-US/using-unreal-engine-with-autodesk-shotgrid/).
