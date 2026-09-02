# Map Tile Downloader · 地图瓦片下载软件

![Version](https://img.shields.io/badge/download-v2.5.1-blue)
![Platform](https://img.shields.io/badge/platform-Windows-0078D6)
![Distribution](https://img.shields.io/badge/distribution-binary_only-64748B)
![License](https://img.shields.io/badge/use-registration_required-orange)

**[English](README.md) | 简体中文**

**Map Tile Downloader（MTDL）** 是一款面向测绘、遥感、GIS 数据准备工作的 Windows 桌面工具，集 **多源在线底图瓦片、历史影像、卫星影像、矢量数据、土地覆盖、高程数据及本地时序影像处理** 于一体。软件重点解决多来源地理空间数据检索、下载、拼接、格式整理和成果准备过程中操作分散、重复配置较多的问题，为日常数据浏览、资料准备、变化对比和辅助分析提供统一工作界面。

> **本软件为注册授权软件。首次使用需完成机器码注册并取得有效授权码，未注册状态下不能正常使用软件功能。** 软件授权仅针对 MTDL 程序本身，不代表用户自动获得任何第三方地图、影像、矢量、DEM 或在线服务的数据使用权。

> **[直接下载 MTDL v2.5.1 Windows 程序包（ZIP）](https://github.com/zhangyhrs/map_tile_downloader/releases/download/map_tile_downloader_v2_5_1/MTDL_v2.5.1.zip)**
>
> 下载后完整解压，运行包内 EXE。不要将 GitHub 自动生成的 **Source code** 或 **Code → Download ZIP** 当作程序包。

## 软件界面

![MTDL v2.5.1 软件界面](assets/interface.png)

## 软件定位

MTDL 本质上是一个 **地理空间数据访问与处理客户端**。软件通过公开接口、用户自行配置的服务地址、第三方 API、STAC 服务、WFS/Overpass 等方式访问外部数据，并将下载、拼接、导出和格式整理等常用操作集成到统一界面中。

软件本身 **不生产、不拥有，也不转让第三方地图与遥感数据的著作权、数据库权、商标权或其他知识产权**。不同图源的数据版权、许可方式、访问规则、调用频率、署名要求、商业使用限制等，均以相应数据提供方或服务平台公布的条款为准。

## 图源、分辨率与时相

软件整合底图、历史影像、卫星场景和专题数据，但不同数据的“分辨率”和“日期”含义不同。

| 数据类别 | 示例 | 选用时重点检查 |
|---|---|---|
| 底图瓦片 | 高德、Esri、Google、天地图、OSM、Carto、腾讯 | 层级不等于原始影像分辨率，拼接底图可能混合时相 |
| 历史影像 | Esri Wayback | 版本发布日期不等于当地拍摄日期 |
| 卫星场景 | Sentinel-2 L2A、Landsat 8/9、MODIS | 原始波段分辨率、拍摄日期及选区实际云量 |
| 专题 / 高程 | 年度土地覆盖、WorldCover、AW3D30、Copernicus DSM | 参考年度、类别定义与原始数值是否保留 |
| 矢量要素 | OSM、天地图 WFS、OpenBuildingMap、农田地块 | 来源精度与完整性，不能统一标注栅格分辨率 |

**[查看详细图源目录：分辨率、层级换算、时相与使用限制 →](docs/DATA_SOURCES_ZH.md)**

> 所检查的卫星导出流程会进行 8 位拉伸，并将长边限制到 4096 像素，属于可视化 GeoTIFF，**不保留原始科学数值，也不保证原始像元间距**。定量分析请使用提供方原始产品。

## 主要功能

| 模块 | 操作内容 | 输出 |
|---|---|---|
| 底图瓦片 | 选择图源和层级，下载并拼接瓦片，可选输出坐标系 | GeoTIFF |
| 历史影像 | 加载 Esri Wayback 版本清单，通过日期滑块选择版本 | GeoTIFF |
| 卫星影像 | 按范围、日期、云量检索配置的 STAC 数据集，选择场景与波段下载 | 可视化 GeoTIFF |
| 矢量数据 | 获取配置的天地图 WFS、OSM/Overpass、OpenBuildingMap 数据 | GeoJSON |
| 土地覆盖 | IO-LULC、ESA WorldCover；MODIS 入口配置待纠正与验证 | GeoTIFF |
| 农田地块 | 按范围获取配置的 Fields of the World 地块数据 | GeoJSON / GeoPackage |
| 高程数据 | 计算 JAXA AW3D30 所需图幅，下载并可选自动解压 | DEM 压缩包 / 栅格 |
| 时序 GIF | 由本地 GeoTIFF 序列制作动画，可叠加日期标签 | GIF |

以上根据作者提供的代码梳理，具体可用性取决于程序包配置、数据覆盖、账号权限和网络状态，不代表所有在线服务均已实测可用。

## 注册与授权

1. 下载并完整解压软件包后运行 EXE。
2. 首次启动或授权失效时，软件会显示机器码/注册信息。
3. 用户需将机器码发送给开发者，并取得对应的授权码后完成注册。
4. 注册信息仅用于软件授权控制；授权码与机器码请勿公开发布或上传至 Issues。
5. 软件注册授权与第三方数据服务授权相互独立。即使 MTDL 已成功注册，天地图、JAXA 或其他需要账号、Token、API Key 的服务仍需用户自行取得合法访问权限。

本仓库仅提供使用文档和打包程序下载入口，**不公开 Python 源码及私人配置**。公开下载不等同于软件开源，也不等同于获得永久、无限制或可转授权的软件使用许可。

## 快速使用

1. 下载 **MTDL_v2.5.1.zip**，完整解压到有写入权限的本地目录，保留全部配套文件。
2. 启动包内 EXE，按软件提示完成注册授权。
3. 通过地图框选、当前视图、输入边界坐标或导入矢量设置范围。
4. 选择模块，设置相应参数及输出位置。
5. 启动任务，查看进度和日志；需要中止时点击停止当前任务。
6. 在 GIS 软件中检查结果范围、位置、缺失数据、时相、分辨率和属性信息，再用于后续工作。

**导入矢量用于读取外接矩形，不等同于按多边形边界精确裁剪。** 代码支持读取 SHP、GeoJSON、KML、GPKG、GML 范围，请确保源数据坐标系正确。

## 数据版权与使用责任

- MTDL 是数据访问、下载与整理工具，**开发者不是第三方地图、卫星影像、矢量数据、DEM 或在线服务的数据提供方**。
- 软件中出现的第三方平台名称、服务名称、数据集名称及商标，仅用于说明兼容的数据来源或访问方式，其相关权利归各自权利人所有。
- 用户应自行确认目标数据源是否允许访问、下载、缓存、拼接、转换、二次处理、成果发布或商业使用，并遵守数据提供方的许可协议、服务条款、署名要求、API 调用限制和当地适用法律法规。
- 因第三方服务调整、接口变更、账号权限、密钥失效、网络限制、限流、数据下线、覆盖差异、时相差异或精度差异导致的无法访问、下载失败或成果差异，不属于软件开发者对第三方数据内容与持续可用性的保证范围。
- 开发者不对第三方数据的版权归属、完整性、现势性、精度、合法授权状态或特定用途适用性作出保证。用户将下载数据用于项目成果、公开发布、商业用途或其他用途前，应自行完成必要的版权、许可、保密及合规审查。
- 用户不得利用本软件绕过访问控制、突破服务限制、侵犯第三方知识产权或从事违反法律法规及数据提供方服务条款的行为。

**简而言之：软件授权只解决“能否使用 MTDL”的问题；数据授权解决“能否使用某个具体数据源及其成果”的问题，两者必须分别遵守。**

## 配置说明

- 天地图相关服务可能需要用户自己的有效密钥及相应服务权限。
- AW3D30 模块使用用户自己的 JAXA 账号。
- 其他需要 Token、API Key、账号或访问许可的服务，由用户自行申请并妥善保管。
- 配置管理支持打开配置目录和重新加载图源；重置前先备份自定义配置。
- 不要在公开 Issues 中上传访问密钥、密码、授权码、机器码或私人配置。

## 使用边界

- **内存占用：**瓦片在内存中拼接，大范围、高层级任务可能耗尽内存，建议先小范围测试，再分区下载。
- **缺失瓦片：**下载失败的位置可能显示灰色，生成文件不代表数据完整，应检查日志。
- **空间精度：**地图瓦片是渲染底图，并非原始科学遥感数据；坐标偏移处理属于近似处理，选择输出 EPSG 不代表达到测量级精度。
- **范围处理：**各模块按矩形范围或相交图幅、要素获取数据，并非统一按任意多边形裁切。
- **历史日期：**Wayback 版本发布日期不一定是选区影像的实际拍摄日期。
- **服务访问：**图源可能限流、要求认证或失效；遇到证书问题应排查网络和证书，不建议关闭安全校验。
- **成果使用：**下载成功不代表数据适用于测绘成果、工程设计、自然资源调查、行政管理或其他专业成果，正式使用前应结合项目技术要求进行检查与验证。

## 下载与帮助

已发布程序包：**MTDL_v2.5.1.zip**，330,969,935 字节。[发布页](https://github.com/zhangyhrs/map_tile_downloader/releases/tag/map_tile_downloader_v2_5_1) · [SHA-256 校验文件](checksums/SHA256SUMS.txt)

在 PowerShell 中校验：

```powershell
Get-FileHash .\MTDL_v2.5.1.zip -Algorithm SHA256
```

校验值采用 GitHub 提供的附件摘要，哈希一致只证明下载文件完整性，不代表第三方数据质量、成果精度或特定用途适用性已得到验证。

[使用指南](docs/USAGE_ZH.md) · [问题反馈](https://github.com/zhangyhrs/map_tile_downloader/issues)

## 关注与交流

微信公众号 **测绘地信** · 知识星球 **测绘地理信息共享中心**。点击图片可查看原图。

<table>
<tr><th width="50%">微信公众号<br>测绘地信</th><th width="50%">知识星球<br>测绘地理信息共享中心</th></tr>
<tr><td align="center" valign="middle"><a href="assets/wechat-official-account.png"><img src="assets/wechat-official-account.png" alt="测绘地信微信公众号二维码" height="140"></a></td><td align="center" valign="middle"><a href="assets/knowledge-planet.jpg"><img src="assets/knowledge-planet.jpg" alt="测绘地理信息共享中心知识星球二维码" height="140"></a></td></tr>
</table>

**Zhang Y.H.** · [@zhangyhrs](https://github.com/zhangyhrs)

相关工具：[GeoStar Selector for QGIS](https://github.com/zhangyhrs/GeoStar-Selector-QGIS) · [SHP2KMZ Tool](https://github.com/zhangyhrs/SHP2KMZ_Tool)
