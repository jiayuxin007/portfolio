# Jia Yuxin · Portfolio

个人作品集站点。纯静态、无构建步骤，直接编辑 HTML 就能看到效果。

## 页面

| 文件 | 说明 |
| --- | --- |
| `index.html` | 首页。全屏循环视频背景层（`z-index: -1`），内容层待填 |
| `piano.html` | **Grand Piano · 粒子流变装置**。three.js 点云钢琴：形态演化 / 条纹溶解 / curl-noise 星云 |
| `video-options.html` | 星空视频素材候选页：Mixkit 免费素材清单，附冷调、留白、杂乱度抽帧数据 |

## 本地预览

页面用到 ES Module、import map 和 `.glb`，必须通过 HTTP 打开（`file://` 会被 CORS 拦掉，钢琴加载不出来）：

```bash
npx serve .
# 或者
python -m http.server 8777
```

然后访问 <http://localhost:8777/piano.html>。

## 素材

- `asset/model/grand-piano.glb`（15.7 MB）—— 钢琴点云的采样源，`piano.html` 必需；缺失时页面会给出兜底提示
- `asset/video/12336965-hd_1920_1028_60fps.mp4`（14.1 MB）—— 首页背景视频
- `works/video/*.mp4` —— 成片素材，单个文件 193 MB 超过 GitHub 的 100 MB 单文件上限，**未纳入版本库**。
  需要时用 Git LFS 接管：

```bash
git lfs install
git lfs track "works/video/*.mp4"
git add .gitattributes works/video
```

## piano.html 技术要点

- Three.js r160（CDN import map，不装 npm，无打包）
- 自定义 GLSL：点云形态演化、条纹溶解、curl noise 星云、鼠标力场扰动
- 后期链 `RenderPass → UnrealBloomPass → Grade(暗角/颗粒) → OutputPass`，ACES Filmic 色调映射
- `CONFIG` 集中放可调参数：相机 `liftPx`（构图抬升）、溶解时长、配色区间等
- 中英（EN / IT）切换只替换 DOM 文案并清空测量缓存，不重建场景

## 素材来源

`video-options.html` 中的视频均来自 [Mixkit](https://mixkit.co/free-stock-video/space/) 免费素材库，
条款见 [Mixkit License](https://mixkit.co/license/)。
