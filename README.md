# 关于本仓库（中文说明）

本仓库是 [openlayers/ol-cesium](https://github.com/openlayers/ol-cesium) 的 fork，仅用于个人定制与调试，不用于替代上游发布。ol-cesium 是 OpenLayers 与 Cesium 的集成库：用 [OpenLayers](https://openlayers.org/) 创建二维地图，并可在 [Cesium](https://cesiumjs.org) 三维地球上可视化与双向同步。许可证遵循上游 BSD-2-Clause（见 [LICENSE](LICENSE)），上游 README 原文保留在本文件下方。

## 本仓库相对上游的改动

- `OLCesium` 类增加 `viewer` 参数，支持传入外部构造好的 `Cesium.Viewer` 三维地球实例（便于外部自行创建和管控 viewer）。

## 目录结构

| 目录 / 文件 | 说明 |
| --- | --- |
| `src/` | 库源码（ES6 modules） |
| `examples/` | 示例程序 |
| `buildtools/` | webpack 构建配置脚本 |
| `css/` | 库样式文件（olcs.css） |
| `Makefile`、`webpack.config.js` | 构建入口配置 |

## 常用命令（来自 package.json scripts 与 Makefile）

npm scripts：

| 命令 | 说明 |
| --- | --- |
| `npm run build` | 构建库（development + production 两种模式） |
| `npm run build-library` | production 模式构建库 |
| `npm run build-library-debug` | development 模式构建库（未压缩，便于调试） |
| `npm run build-examples` | 构建示例 |
| `npm run build-umd` | 构建 UMD 版本（Angular 等场景） |
| `npm start` | 启动示例开发服务器（http://localhost:3000/examples） |
| `npm run typecheck` | TypeScript 类型检查 |
| `npm run doc` | 生成 API 文档（typedoc） |

Makefile 目标：

- `make serve`：启动示例开发服务器（内部执行 `npm start`）
- `make dist`：生成库与示例的发布产物
- `make lint`：运行 eslint 检查
- `make clean` / `make cleanall`：清理构建产物 / 连同 node_modules 一并清理
- `make help`：查看帮助

---

## ol-cesium 原始说明

OpenLayers - Cesium integration library. Create your map using [OpenLayers](https://openlayers.org/), and visualize it on a globe with [Cesium](https://cesiumjs.org).
See [live examples](https://openlayers.org/ol-cesium/examples/).
 



ES6 modules
-----------

Since version 2.0, the code is entirely based on ES6 modules and syntax.
It requires OpenLayers 5.x.
A convenient ES6 package `olcs` is available on npm.

Features
--------
Switch smoothly between 2D and 3D and synchronize:

- Map context (bounding box and zoom level);
- Raster data sources;
- Vector data sources in 2D and 3D;
- Map selection (selected items);
- Animated transitions between map and globe view.

The library is configurable and extensible and allows:

- Lazy or eager loading of Cesium
- Limiting Cesium resource consumption (idle detection)

For synchronization of maps in projections other than EPSG:4326 and EPSG:3857 you need 2 datasets, see the customProj example.

Integration in your application
-------------------------------

There are several ways to use OL-Cesium in your application.

### As an ES6 library (recommended method)
```bash
npm i --save olcs
```

Then import the parts you need. Example:
```js
import OLCesium from 'olcs/OLCesium.js';
const ol3d = new OLCesium({map: ol2dMap}); // ol2dMap is the ol.Map instance
ol3d.setEnabled(true);
```

In addition, you need to expose the Cesium library as `window.Cesium`.
For this, simply add the Cesium script to your html:
```html
<script type="text/javascript" src="..your_path../Cesium.js"></script>
```

For Cesium integration with Webpack, see [ol-cesium-webpack-example](https://github.com/gberaudo/ol-cesium-webpack-example).

### As an old-fashioned independent library (need update)

- build the library in dist/olcs.js:
```bash
npm i --save olcs
npm run build-library
```

- get the CSS and JS from the full build at https://openlayers.org/download/

- use as follow:
```js
const ol3d = new olcs.OLCesium({map: ol2dMap}); // ol2dMap is the ol.Map instance
ol3d.setEnabled(true);
```

For the remaining steps, see the [old fashioned example](https://openlayers.org/ol-cesium/examples/oldfashioned.html).
Notably, you need the Cesium library.

### As an UMD library (Angular, ...)
```bash
npm i --save ol-cesium
```
The UMD-specific build is located here: `node_modules/ol-cesium/dist/olcesium.umd.js`


Then import the parts you need. Example:
```js
import OLCesium from 'ol-cesium';
const ol3d = new OLCesium({map: ol2dMap}); // ol2dMap is the ol.Map instance
ol3d.setEnabled(true);
```

In addition, you need to expose the Cesium library as `window.Cesium`.
For this, simply add the Cesium script to your html:
```html
<script type="text/javascript" src="..your_path../Cesium.js"></script>
```

Going further
-------------

See the [examples](https://openlayers.org/ol-cesium/examples/).

If you are new to Cesium, you should also check the [Cesium tutorials](https://cesiumjs.org/tutorials).


Running the examples in debug mode
----------------------------------

This is useful for contributing to Ol-Cesium, because it loads the
source files instead of a minified build:

    $ make serve

will make the distribution examples available at http://localhost:3000/examples

Running the unminified version of Cesium
----------------------------------------

Passing the parameter `?mode=dev` to an example will load the debug version of
Cesium instead of the minified one. This is helpful when something breaks inside
Cesium. In distribution mode, an unminified version of OpenLayers and Ol-Cesium is
also loaded.

Limitations
-----------

- OpenLayers unmanaged layers are not discoverable and as a consequence not
supported. Plain layers should be used instead of the synchronization managed
manually. See https://github.com/openlayers/ol-cesium/issues/350.

- OpenLayers interactions are not supported in 3d. See https://github.com/openlayers/ol-cesium/issues/655.

Release process
---------------

See [RELEASE.md](https://github.com/openlayers/ol-cesium/blob/master/RELEASE.md).
