# 职业大楼 · 3D 个人网站

一座五层卡通职业大楼，外侧是连续盘旋车道，一辆卡通小车担任导览员。
点击楼层或项目展板，小车会沿真实车道开过去，相机平滑跟随；内容面板为 HTML 渲染。

> 本站为**模拟简历演示案例**：所有内容为模拟数据，仅供展示。
> 本仓库只放构建后的**产物**（`index.html` + `audio/`），**源码不公开**。
> `index.html` 不含任何源码映射，JS 为构建后压缩产物。

## 在线访问

**https://330789468-jasmine.github.io/resume-3D/**

（首次打开如果看到旧版本，加 `?v=6` 之类的参数避开缓存。）

## 截图

| | |
| :---: | :---: |
| ![全景](docs/img/01-overview.jpg) | ![3F 老电视](docs/img/02-tv-hud.jpg) |
| ![小红书](docs/img/03-book.jpg) | ![夜间](docs/img/04-night.jpg) |
| ![手机端](docs/img/05-mobile.jpg) | ![手机端阅读](docs/img/06-mobile-book.jpg) |

## 使用说明

- **在线**：直接打开上面的链接（首次约 1.1MB，音频按需加载）。
- **离线**：下载**整个仓库**（`index.html` 与 `audio/` 必须放在同一层）后双击 `index.html` 即可。
  若只需要一个可单发他人的文件，请用单文件版（音频内嵌在 HTML 里，体积约 8.3MB）。
- 若打开后页面空白并提示 WebGL 不可用，请换最新版 Chrome / Edge，并确认浏览器已开启「硬件加速」。
- 桌面端体验最佳；手机端也可用：
  - 手机点开 3F 老电视后，屏幕**上方三张、下方三张**是六个频道卡片；
  - 屏幕下方会出现两个**大旋钮**（CH− / CH+）用来换台，不用去点电视上那两个小旋钮；
  - 点任意卡片 = 打开对应的工作经历。

## 部署（GitHub Pages）

1. 仓库 Settings → **Pages** → Source 选 `Deploy from a branch`；
2. 分支选 `main`、目录选 `/ (root)`，保存；
3. 等 1 分钟左右，`https://330789468-jasmine.github.io/resume-3D/` 就能访问。

## 文件结构

```
index.html     构建产物（不含源码、不含 sourcemap）
audio/
  day.mp3      白天背景音乐
  night.mp3    夜间背景音乐
  holo.mp3     全息面板点击音效
docs/img/      README 用展示图（页面不引用）
```

> `index.html` 与 `audio/` 必须同层部署；把音频拆出去是为了首屏体积从 8.3MB 降到 1.1MB。

## 发布规则（pre-push 钩子会强制检查）

本仓库是公开仓库，推之前会自动过一道 `pre-push` 检查，不符合就拦下来：

| 检查 | 规则 |
| --- | --- |
| **文件白名单** | 仓库里只允许 `README.md`、`index.html`、`audio/*.mp3`、`docs/img/*.jpg` |
| **线上跑无薪版** | `index.html` 里不能出现「期望薪资」（带薪版会被挡下） |
| **无源码特征** | `index.html` 里不能有 `sourceMappingURL` / `export function` / `import {` |
| **暂存区地雷** | 暂存了带薪版 `index.html` 也会拦（哪怕还没 commit） |

> ⚠️ 最容易踩的坑：跑过 `npm run site` 之后，暂存区里会躺着一份**带薪资**的
> `index.html`，顺手 `git commit && git push` 就会把带薪版推上线。
> 钩子就是为了拦这一下。

**正确的发布流程**（源码仓库在 `E:\Project\career-tower`，不公开）：

```bash
cd E:\Project\career-tower
npm run nosalary:split                        # 构建无薪拆分版
cp 无薪资版-拆分\index.html  ..\career-tower-site\index.html
cp 无薪资版-拆分\audio\*.mp3 ..\career-tower-site\audio\
cd ..\career-tower-site && git commit && git push
```

确实需要绕过时：`git push --no-verify`（或 `CT_SKIP_HOOK=1 git push`）。
