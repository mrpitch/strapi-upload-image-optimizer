# strapi v5 upload-image-optimizer

This plugin is a PoC for migration to strapi v5 of existing (awesome) plugin https://github.com/marlokessler/strapi-plugin-image-optimizer

## Configure & Setup

Configuration and setup is same as described in https://github.com/marlokessler/strapi-plugin-image-optimizer/readme.md


`config/plugins.ts`

```
import { Config as ImageOptimizerConfig } from "upload-image-optimizer/dist/server/src/models/config";

export default () => ({
    ...
  "upload-image-optimizer": {
    enabled: true,
    config: {
      include: ["jpeg", "jpg", "png"],
      exclude: ["gif"],
      formats: ["original", "webp", "avif"],
      sizes: [
        {
          name: "hurdi",
          width: 300,
        },
        {
          name: "sm",
          width: 768,
        },
        {
          name: "md",
          width: 1280,
        },
        {
          name: "lg",
          width: 1920,
        },
        {
          name: "xl",
          width: 2840,
          // Won't create an image larger than the original size
          withoutEnlargement: true,
        },
        {
          // Uses original size but still transforms for formats
          name: "original",
        },
      ],
      additionalResolutions: [1.5, 3],
      quality: 70,
    } satisfies ImageOptimizerConfig,
  },

});

```

`src/extensions/upload/strapi-server.ts`

```
import imageOptimizerService from "upload-image-optimizer/dist/server/src/services/image-optimizer-service"
import { LoadedPlugin } from "@strapi/types/dist/plugin"

module.exports = (plugin: LoadedPlugin) => {
  plugin.services["image-manipulation"] = imageOptimizerService;
  return plugin;
};
```
## Issues

* currently this is not working with strapiv5.rc23 (or any other I tried)
* the plugin is build fine, but when integrated in strapi v5 following error occurs:
```
  Error: Could not load js config file /Users/.../strapi-playground-docker/src/plugins/upload-image-optimizer/dist/server/index.js:                                       ││   Something went wrong installing the "sharp" module                                                                                                                             ││                                                                                                                                                                                  ││   Could not dynamically require "../build/Release/sharp-darwin-x64.node". Please configure the dynamicRequireTargets or/and ignoreDynamicRequires option of                      ││   @rollup/plugin-commonjs appropriately for this require call to work. 
```

