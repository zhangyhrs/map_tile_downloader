# 图源、分辨率与时相说明

**[English](DATA_SOURCES.md) | 简体中文** · [返回首页](../README_ZH.md)

本页依据作者提供的实现与配置整理，不代表所有服务已联网实测。当前发布包仍为 v2.5.1，另行提供的源码文件名为 v2.5.2，尚未验证二者完全一致。源码、私人图源地址及凭据均不公开。

## 选数据前先区分三个概念

- **瓦片层级、像元间距，不等于原始影像分辨率或定位精度。** 放大、插值不能生成原图不存在的细节。
- **影像拍摄日期、底图发布日期、产品年度不是一回事。** 时相应以具体场景或产品元数据为准。
- **重要输出限制：**所检查的卫星影像导出流程会将长边限制到 4096 像素，并按各波段 2%–98% 分位数拉伸为 8 位。其 GeoTIFF 属于可视化输出，未保留原始科学数值，不能直接用于定量反射率指数、温度或高程分析。卫星页签中的 Copernicus DEM 也受此影响；独立 AW3D30 下载模块与该流程不同。

## 一、常规底图瓦片

下表最高层级是**当前配置值**，不是图源服务能力、覆盖或精度保证。

| 配置图源 | 内容 | 配置最高层级 |
|---|---|---:|
| 高德 | 地图、卫星、卫星标注 | 18 |
| Esri | 卫星、街道、地形 | 19 |
| Esri 国家地理 | 渲染专题底图；旧服务需验证 | 16 |
| Google | 卫星、混合、地图、地形 | 20 |
| 天地图 | 卫星、矢量底图、影像注记 | 18 |
| 天地图地形 | 渲染地形底图 | 14 |
| OpenStreetMap | 渲染街道地图 | 19 |
| OpenTopoMap | 渲染地形地图 | 17 |
| CyclOSM | 骑行地图 | 19 |
| Carto | 浅色、深色、Voyager | 19 |
| 腾讯 | 卫星 | 18 |
| 自定义 URL | 用户有权访问的服务 | 默认 18，取决于图源 |

瓦片菜单中的“矢量地图”实际是**栅格化渲染底图**，不是矢量要素下载。高德、腾讯配置涉及 GCJ-02 处理，近似转换不代表测量级纠偏；天地图可能需要有效密钥。

普通卫星底图通常是多时相、多来源拼接，不能统一承诺“某年影像”或“0.5 米原始分辨率”。相邻区域的拍摄时间和细节可能不同，允许选择高层级也不代表该区域有相应精细影像。软件列出图源不等于获得批量下载许可，应遵守提供方署名、离线使用、配额和授权条件。

### 层级与像元间距

标准 256 像素 Web Mercator 瓦片的近似地面像元间距为：
`156543.033928 × cos(纬度) / 2^层级` 米/像素。这只是瓦片网格计算，不是传感器分辨率。[微软瓦片网格文档](https://learn.microsoft.com/en-us/azure/azure-maps/zoom-levels-and-tile-grid)

| 层级 | 赤道附近，米/像素 | 北纬 30° 附近，米/像素 |
|---:|---:|---:|
| 14 | 9.55 | 8.27 |
| 15 | 4.78 | 4.14 |
| 16 | 2.39 | 2.07 |
| 17 | 1.19 | 1.03 |
| 18 | 0.60 | 0.52 |
| 19 | 0.30 | 0.26 |
| 20 | 0.15 | 0.13 |

重投影与重采样还会改变输出像元间距；EPSG:4326 栅格网格以度表示，不能直接当作米。

## 二、历史影像与时序 GIF

Esri Wayback 提供从 2014 年 2 月起的 World Imagery 历史发布版本。日期滑块选择的是**底图版本**，不一定是选区新拍摄的影像；不同版本在同一地点可能没有变化。[Esri Wayback 说明](https://www.esri.com/arcgis-blog/products/arcgis-living-atlas/mapping/use-world-imagery-wayback)

实际拍摄日期、影像来源和分辨率，应查看当地可用的影像元数据，不能只看滑块日期。[Esri 影像元数据说明](https://www.esri.com/arcgis-blog/products/arcgis-living-atlas/imagery/wayback-with-world-imagery-metadata)

GIF 模块组合本地 GeoTIFF。帧日期和播放间隔不代表卫星重访周期。变化对比前应统一配准、范围和尺度，并核验真实拍摄日期；季节、云和拼接更新都可能造成“看起来发生变化”。

## 三、卫星场景与卫星页签中的 DSM

下表为**原始产品特征**，不是本软件导出文件的分辨率承诺。必须结合前述 4096 像素与 8 位可视化输出限制理解。

| 数据集 | 原始分辨率 / 波段 | 时相与使用说明 |
|---|---|---|
| Sentinel-2 L2A | MSI 波段分 10/20/60 米；配置的 RGB B04/B03/B02、B08 为 10 米，B05、B12 为 20 米 | 任务始于 2015 年；双星标称重访 5 天，不保证每 5 天都有无云影像。L2A 实际覆盖以目录及场景日期为准，适合植被、地表可视化 |
| Landsat 8 Collection 2 Level-2 | 多光谱 30 米；热红外原始采样 100 米，产品重采样至 30 米 | Landsat 8 始于 2013 年，**不能写成 1972 年至今**；标称重访 16 天，可用场景受云和覆盖影响 |
| Landsat 9 Collection 2 Level-2 | 多光谱 30 米；热红外需区分原始与交付分辨率 | 任务时期从 2021 年起，实际可用日期以检索结果为准 |
| MODIS Terra MOD09GA | 地表反射率波段 500 米，部分辅助层 1 千米 | 日产品不等于每日无云观测；适合区域尺度，不适合地块精细识别 |
| Copernicus DEM GLO-30 | 约 30 米 / 1 角秒，**数字表面模型** | 主要采集于 2011–2015 年，不是年度影像，也不等同裸地地形；卫星导出流程不保留真实高程值 |

依据：[Sentinel-2 波段](https://dataspace.copernicus.eu/data-collections/copernicus-sentinel-missions/sentinel-2)、[重访周期](https://sentiwiki.copernicus.eu/web/s2-mission)、[任务历史](https://www.esa.int/Applications/Observing_the_Earth/Copernicus/Sentinel-2)、[Landsat 8](https://science.nasa.gov/mission/landsat-8/)、[Landsat 9](https://www.usgs.gov/landsat-missions/landsat-9)、[MOD09GA](https://ladsweb.modaps.eosdis.nasa.gov/missions-and-measurements/products/MOD09GA)、[Copernicus DEM](https://dataspace.copernicus.eu/explore-data/data-collections/copernicus-contributing-missions/collections-description/COP-DEM)、[DEM 产品手册](https://dataspace.copernicus.eu/sites/default/files/media/files/2024-06/geo1988-copernicusdem-spe-002_producthandbook_i5.0.pdf)。

真彩色、近红外、短波红外组合适用于不同的可视化任务，“植被配色”不自动等于 NDVI 产品。云量通常是场景级筛选条件，不一定代表所选矩形内的实际云量。

## 四、土地覆盖、矢量与高程

| 来源 / 模块 | 尺度与配置年份 | 解读与限制 |
|---|---|---|
| IO / Esri / Microsoft 年度土地覆盖 | 10 米、9 类；配置列出 2017–2023 年 | 年份是年度分类参考期，不是精确拍摄日，也不代表提供方最新年份。[数据集](https://planetarycomputer.microsoft.com/dataset/io-lulc-annual-v02) |
| ESA WorldCover | 10 米、11 类；2020、2021 年 | 两期算法版本不同，分类差异不全是真实变化。[数据说明](https://esa-worldcover.org/en/data-access) |
| MODIS 土地覆盖入口 | 预期 MCD12Q1：500 米、年度产品 | **配置待纠正与验证。**所给入口指向 MCD64A1 火烧迹地产品，而非 MCD12Q1 土地覆盖，不能宣传为已验证可用。[MCD12Q1](https://ladsweb.modaps.eosdis.nasa.gov/missions-and-measurements/products/MCD12Q1)、[MCD64A1](https://www.earthdata.nasa.gov/data/catalog/lpcloud-mcd64a1-061) |
| OSM / Overpass | 道路、铁路、水系、建筑、土地利用、兴趣点、行政边界等 | 矢量没有统一栅格分辨率；完整性、定位精度及编辑日期因地区而异 |
| 天地图 WFS | 配置的交通、水系、居民地等要素 | 覆盖和访问依赖服务权限，不承诺统一精度或采集年份 |
| OpenBuildingMap | 建筑矢量 | 聚合多来源数据，没有统一分辨率和时相，需检查来源与当地完整性 |
| Fields of the World | 模型预测农田地块；配置年份 2024 | 数据集年份未必是影像拍摄年份；预测边界不能当作法定地籍界线 |
| JAXA AW3D30 | 约 30 米 / 1 角秒 DSM | 主要基于 ALOS PRISM 2006–2011 年数据并含补充数据；产品更新不代表每年重新测量，建筑、植被会影响表面高程。[JAXA](https://www.eorc.jaxa.jp/ALOS/en/dataset/aw3d30/aw3d30_e.htm)、[产品手册](https://www.eorc.jaxa.jp/ALOS/en/dataset/aw3d30/data/aw3d30v4.1_product_e_1.0.pdf) |

所给配置中的独立 Microsoft Buildings 入口已弃用，不作为独立可用模块宣传。

## 五、如何选用与检查

- **制作工作底图：**选择有权使用的底图，先低层级、小范围测试，再分区下载。
- **历史对比：**Wayback 可作参考，但先查当地拍摄日期，不把发布日期当作观测日期。
- **按场景比较：**卫星检索按拍摄日期筛选，并检查选区云层；定量遥感分析请获取原始科学产品，使用相应处理流程。
- **年度分类比较：**先核验类别定义、算法版本和产品年度。
- **地形分析：**使用保留原始数值的高程栅格，核实单位、垂直基准和 NoData，不使用卫星页签中拉伸后的 DSM 可视化结果。

建议记录图源 / 产品、场景编号、采集或参考日期、下载日期、波段、坐标系、像元间距、处理过程及署名要求。导入矢量获取的是外接矩形，不是精确多边形掩膜；输出后应在 GIS 中检查位置、缺失瓦片和数值。
