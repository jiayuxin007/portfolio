# Jia Yuxin · Portfolio

个人作品集站点。纯静态、无构建步骤，直接编辑 HTML 就能看到效果。

## 页面

| 文件 | 说明 |
| --- | --- |
| `index.html` | **首页（= Grand Piano 粒子流变装置）**。three.js 点云钢琴：形态演化 / 条纹溶解 / curl-noise 星云 |
| `video-bg.html` | 早先的视频背景草稿（全屏循环视频层，内容层待填），当前未接入首页 |
| `piano.html` | 旧链接的重定向壳：`meta refresh` → `/`，以前分享过的 `/piano.html` 仍能打开 |
| `video-options.html` | 星空视频素材候选页：Mixkit 免费素材清单，附冷调、留白、杂乱度抽帧数据 |

## 本地预览

页面用到 ES Module、import map 和 `.glb`，必须通过 HTTP 打开（`file://` 会被 CORS 拦掉，钢琴加载不出来）：

```bash
npx serve .
# 或者
python -m http.server 8777
```

然后访问 <http://localhost:8777/>（首页就是钢琴装置）。

## 部署

代码改完 `git push` 后，两个平台各自重建，互不影响：

| 平台 | 地址 | 配置要点 |
| --- | --- | --- |
| **Vercel（主）** | <https://jiayuxin.vercel.app> | 项目 `jiayuxin`（team `jiayuxin`）；`vercel.json` 里配了静态资源缓存头和 `/piano.html → /` 的 307；`.vercelignore` 挡住 `works/` 那个 193MB 成片 |
| GitHub Pages（镜像） | <https://jiayuxin007.github.io/portfolio/> | 仓库 Settings → Pages，源 = `main` 分支根目录 |

Vercel 上重新部署：

```powershell
npx vercel@latest deploy --prod --yes --archive=tgz --scope jiayuxin
```

### 自定义域名

目前用 Vercel 免费送的二级域名 `jiayuxin.vercel.app`，**没有买独立域名**。
（`jiayuxin.com` 是别人 2011 年就注册的，不属于本项目，别指向它；`jiayuxin.space` 目前未被注册。）

以后买了域名，在阿里云/腾讯云云解析里加这几条，就能接上 Vercel：

| 记录类型 | 主机记录 | 记录值 |
| --- | --- | --- |
| CNAME | `www` | 到 Vercel 项目 Settings → Domains 里复制（形如 `xxxxx.vercel-dns-017.com`） |

apex（`@`）用 A 记录，值同样在该页面给出（现在是一组 Vercel anycast IP）。
记录值**项目专属**，和别的项目不通用；建议 `www` 作主域、apex 308 跳到 `www`。
自定义域名**只绑一个平台**：两个平台同时绑同一个域，会互抢 CNAME 和 `_acme-challenge` 记录，导致证书反复校验失败。

## 素材

- `asset/model/grand-piano.glb`（15.7 MB）—— 钢琴点云的采样源，首页 `index.html` 必需；缺失时页面会给出兜底提示
- `asset/video/12336965-hd_1920_1028_60fps.mp4`（14.1 MB）—— 首页背景视频
- `works/video/*.mp4` —— 成片素材，单个文件 193 MB 超过 GitHub 的 100 MB 单文件上限，**未纳入版本库**。
  需要时用 Git LFS 接管：

```bash
git lfs install
git lfs track "works/video/*.mp4"
git add .gitattributes works/video
```

## 首页（index.html）技术要点

- Three.js r160（CDN import map，不装 npm，无打包）
- 自定义 GLSL：点云形态演化、条纹溶解、curl noise 星云、鼠标力场扰动
- 后期链 `RenderPass → UnrealBloomPass → Grade(暗角/颗粒) → OutputPass`，ACES Filmic 色调映射
- `CONFIG` 集中放可调参数：相机 `liftPx`（构图抬升）、溶解时长、配色区间等
- 中英（EN / IT）切换只替换 DOM 文案并清空测量缓存，不重建场景

## 素材来源

`video-options.html` 中的视频均来自 [Mixkit](https://mixkit.co/free-stock-video/space/) 免费素材库，
条款见 [Mixkit License](https://mixkit.co/license/)。
