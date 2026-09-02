# Map Tile Downloader · 地图瓦片下载软件

![Version](https://img.shields.io/badge/download-v2.5.1-blue)
![Platform](https://img.shields.io/badge/platform-Windows-0078D6)
![Distribution](https://img.shields.io/badge/distribution-binary_only-64748B)

**[English](README.md) | 简体中文**

集底图瓦片、历史影像、卫星影像与地理空间数据获取于一体的桌面工具，服务于测绘、遥感和 GIS 数据准备。

> **[直接下载 MTDL v2.5.1 Windows 程序包（ZIP）](https://github.com/zhangyhrs/map_tile_downloader/releases/download/map_tile_downloader_v2_5_1/MTDL_v2.5.1.zip)**
>
> 下载后完整解压，运行包内 EXE。不要将 GitHub 自动生成的 **Source code** 或 **Code → Download ZIP** 当作程序包。

## 主要功能

| 模块 | 操作内容 | 输出 |
|---|---|---|
| 底图瓦片 | 选择图源和层级，下载并拼接瓦片，可选输出坐标系 | GeoTIFF |
| 历史影像 | 加载 Esri Wayback 版本清单，通过日期滑块选择版本 | GeoTIFF |
| 卫星影像 | 按范围、日期、云量检索配置的 STAC 数据集，选择场景与波段下载 | GeoTIFF |
| 矢量数据 | 获取配置的天地图 WFS、OSM/Overpass、OpenBuildingMap 数据 | GeoJSON |
| 土地覆盖 | 选择配置的 IO-LULC、ESA WorldCover、MODIS 产品及可用年份 | GeoTIFF |
| 农田地块 | 按范围获取配置的 Fields of the World 地块数据 | GeoJSON / GeoPackage |
| 高程数据 | 计算 JAXA AW3D30 所需图幅，下载并可选自动解压 | DEM 压缩包 / 栅格 |
| 时序 GIF | 由本地 GeoTIFF 序列制作动画，可叠加日期标签 | GIF |

以上根据作者提供的代码梳理，具体可用性取决于程序包配置、数据覆盖、账号权限和网络状态，不代表所有在线服务均已实测可用。

## 快速使用

1. 下载 **MTDL_v2.5.1.zip**，完整解压到有写入权限的本地目录，保留全部配套文件。
2. 启动包内 EXE。如出现注册窗口，复制本机机器码，私下联系开发者获取授权码。
3. 通过地图框选、当前视图、输入边界坐标或导入矢量设置范围。
4. 选择模块，设置相应参数及输出位置。
5. 启动任务，查看进度和日志；需要中止时点击停止当前任务。
6. 在 GIS 软件中检查结果范围、位置、缺失数据和属性，再用于后续工作。

**导入矢量用于读取外接矩形，不等同于按多边形边界精确裁剪。** 代码支持读取 SHP、GeoJSON、KML、GPKG、GML 范围，请确保源数据坐标系正确。

## 注册与配置

- 软件授权和数据服务授权是两回事，下载程序包不等于自动获得软件使用授权。
- 天地图相关服务可能需要用户自己的有效密钥及相应服务权限。
- AW3D30 模块使用用户自己的 JAXA 账号。
- 配置管理支持打开配置目录和重新加载图源；重置前先备份自定义配置。
- 不要在公开 Issues 中上传访问密钥、密码、授权码、机器码或私人配置。

本仓库仅提供使用文档和打包程序下载入口，**不公开 Python 源码及私人配置**。公开下载不等同于软件开源。

## 使用边界

- **内存占用：**瓦片在内存中拼接，大范围、高层级任务可能耗尽内存，建议先小范围测试，再分区下载。
- **缺失瓦片：**下载失败的位置可能显示灰色，生成文件不代表数据完整，应检查日志。
- **空间精度：**地图瓦片是渲染底图，并非原始科学遥感数据；坐标偏移处理属于近似处理，选择输出 EPSG 不代表达到测量级精度。
- **范围处理：**各模块按矩形范围或相交图幅、要素获取数据，并非统一按任意多边形裁切。
- **历史日期：**Wayback 版本发布日期不一定是选区影像的实际拍摄日期。
- **服务访问：**图源可能限流、要求认证或失效；遇到证书问题应排查网络和证书，不建议关闭安全校验。
- **验证情况：**本次未运行已发布 Windows ZIP，也未验证它与所提供源码完全一致。

请仅使用有权访问的数据源，遵守其署名及使用条件。

## 下载与帮助

已发布程序包：**MTDL_v2.5.1.zip**，330,969,935 字节。[发布页](https://github.com/zhangyhrs/map_tile_downloader/releases/tag/map_tile_downloader_v2_5_1) · [SHA-256 校验文件](checksums/SHA256SUMS.txt)

在 PowerShell 中校验：

```powershell
Get-FileHash .\MTDL_v2.5.1.zip -Algorithm SHA256
```

校验值采用 GitHub 提供的附件摘要，哈希一致只证明完整性，不代表软件安全或成果精度已验证。

[使用指南](docs/USAGE_ZH.md) · [问题反馈](https://github.com/zhangyhrs/map_tile_downloader/issues)

## 关注与交流

微信公众号 **测绘地信** · 知识星球 **测绘地理信息共享中心**。点击图片可查看原图。

<table>
<tr><th width="50%">微信公众号<br>测绘地信</th><th width="50%">知识星球<br>测绘地理信息共享中心</th></tr>
<tr><td align="center" valign="middle"><a href="assets/wechat-official-account.png"><img src="assets/wechat-official-account.png" alt="测绘地信微信公众号二维码" height="140"></a></td><td align="center" valign="middle"><a href="assets/knowledge-planet.jpg"><img src="assets/knowledge-planet.jpg" alt="测绘地理信息共享中心知识星球二维码" height="140"></a></td></tr>
</table>

**Zhang Y.H.** · [@zhangyhrs](https://github.com/zhangyhrs)

相关工具：[GeoStar Selector for QGIS](https://github.com/zhangyhrs/GeoStar-Selector-QGIS) · [SHP2KMZ Tool](https://github.com/zhangyhrs/SHP2KMZ_Tool)
