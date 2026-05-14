# 帆向格陵兰 Web App - 部署备忘
**Greenland Sailing 2026 - Deployment Notes**

---

## 1. 腾讯云 + 微信生产环境部署

### 1.1 微信 JS-SDK 分享卡片配置

index.html 底部 `<script>` 中已预埋 `wx.updateAppMessageShareData` 和 `wx.updateTimelineShareData` 钩子代码。

**需要做的：**

1. 在微信公众平台 (mp.weixin.qq.com) 注册你的域名为 JS-SDK 安全域名
2. 后端生成 `wx.config` 所需的 signature（需要 appId + appSecret + 当前URL）
3. 在 index.html 的 `<head>` 中引入微信 JS-SDK:
   ```html
   <script src="https://res.wx.qq.com/open/js/jweixin-1.6.0.js"></script>
   ```
4. 在页面加载时调用 `wx.config({...})` 完成鉴权
5. 分享卡片的标题/描述已设置好:
   - **标题:** 帆向格陵兰 · 2026北极私属远征
   - **描述:** 红帆穿越冰海，全船仅八席。一场留给世界尽头的私属史诗。
   - **缩略图:** `assets/page1_img1.jpeg`

### 1.2 OG Image 元标签

- 当前 `og:image` 使用相对路径 `assets/page1_img1.jpeg`
- **部署后必须改为完整绝对URL**，例如:
  ```html
  <meta property="og:image" content="https://yourdomain.com/assets/page1_img1.jpeg">
  ```
- 同样修改 `twitter:image`

### 1.3 腾讯云推荐方案

**首选：腾讯云 COS (对象存储) + CDN 加速**

1. 创建 COS 存储桶，开启静态网站托管
2. 上传 `greenland-app` 文件夹内所有文件（保持目录结构）
3. 绑定自定义域名 + 开启 HTTPS（微信要求 HTTPS）
4. 配置 CDN 加速（腾讯云 CDN 在国内速度极佳）

**备选：** 腾讯云 CloudBase 静态网站托管（更简单，一键部署）

### 1.4 微信内浏览器注意事项

| 项目 | 状态 |
|------|------|
| 无重型JS框架、纯vanilla JS | 已优化 |
| `-webkit-overflow-scrolling: touch` | 已添加 |
| Service Worker / CSS Grid 高级特性 | 已规避 |
| `tel:` 链接直接拨打 | 已支持 |
| QR码长按识别（白底padding） | 已支持 |

---

## 2. 字体备忘

### 2.1 当前字体方案

| 用途 | 字体 | 来源 |
|------|------|------|
| 正文 body | OPPO Sans | 第三方CDN (`db.quike.com.cn`) |
| 标题 headings | Noto Sans SC | Google Fonts (≈未来荧黑替代) |
| 手记 diary | Noto Serif SC | Google Fonts (= 思源宋体) |
| 英文装饰 | Cinzel | Google Fonts |

### 2.2 OPPO Sans 风险与备案

- **来源:** `https://db.quike.com.cn/_nuxt/fonts/OPPOSans-*.woff2`
- **风险:** 第三方CDN，可能下线或变更URL
- **备选方案:** 将 woff2 文件下载到本地 `assets/fonts/` 目录自托管

**下载地址：**

| 字重 | URL |
|------|-----|
| Light (300) | `https://db.quike.com.cn/_nuxt/fonts/OPPOSans-L.03bf498.woff2` |
| Regular (400) | `https://db.quike.com.cn/_nuxt/fonts/OPPOSans-R.468eaab.woff2` |
| Medium (500) | `https://db.quike.com.cn/_nuxt/fonts/OPPOSans-M.7116b96.woff2` |
| Bold (700) | `https://db.quike.com.cn/_nuxt/fonts/OPPOSans-B.d74d1bd.woff2` |

自托管时修改 index.html 中 `@font-face` 的 src url 为:
```css
url("assets/fonts/OPPOSans-R.woff2")
```

**Fallback 链:** `OPPOSans` → `Noto Sans SC` → `PingFang SC` → `Hiragino Sans GB` → `Microsoft YaHei`

### 2.3 Google Fonts 在国内访问

- `fonts.googleapis.com` 和 `fonts.gstatic.com` 在国内大部分地区可访问
- 如遇问题，可替换为国内镜像:

| 原始域名 | 国内镜像 |
|---------|---------|
| `fonts.googleapis.com` | `fonts.googleapis.cn` |
| `fonts.gstatic.com` | `fonts.gstatic.cn` |

只需修改 index.html 中 `<link>` 标签的 href

### 2.4 未来荧黑 (Glow Sans) 说明

- 原册使用未来荧黑，但该字体无CDN，单个字重 8-10MB
- 当前用 **Noto Sans SC** 替代（同为现代几何无衬线中文字体，视觉接近）
- 如需完美还原，可使用 [cn-font-split](https://github.com/KonghaYao/cn-font-split) 工具对未来荧黑做字体分包，分包后每个子集仅几十KB，按需加载

---

## 3. 文件结构

```
greenland-app/
├── index.html              # 单文件应用 (HTML + CSS + JS 全部内联)
├── DEPLOY-NOTES.md         # 本文件
└── assets/                 # 63张图片，从PDF提取
    ├── page1_img1.jpeg     # 封面: 红帆船+冰山 (用于分享卡片缩略图)
    ├── page24_img3.jpeg    # 微信二维码
    └── ...                 # 其余61张
```

---

## 4. 快速测试

**本地测试：**
```bash
cd greenland-app
python -m http.server 8080
# 浏览器打开 http://localhost:8080
```

**手机测试：**
1. 确保手机和电脑在同一WiFi
2. 查看电脑IP: `ipconfig` (Windows) 或 `ifconfig` (Mac)
3. 手机浏览器打开 `http://你的电脑IP:8080`
