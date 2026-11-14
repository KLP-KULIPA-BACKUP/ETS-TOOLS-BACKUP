# 下一会话开场提示词（直接复制使用）——展示页宣传视频专场

---

你在维护 E:\EtsHelper 的「E听说助手」项目（Flutter 3.47，Windows + Android，作者苦力怕.KULIPA，github.com/KLP-KULIPA-24）。上一会话完成了展示页 `web/index.html` v2（规范整改 + B1 three.js 玻璃流体球 + 图标统一），本会话任务是**给展示页做 three.js 宣传视频并回填网页**。

【动手前必读（按顺序）】
1. C:\Users\Admin\.zcode\cli\memories\projects\e-d482abe8c26bd562\memory\ets-helper-project.md —— 项目环境坑（重点看 2026-10-01 各轮：出包必须核对 data/app.so 时间戳而非 exe；flutter build windows 需要 tools/third_party 在 PATH 含 nuget.exe；web 页深浅色/响应式记录）
2. C:\Users\Admin\.zcode\cli\memories\projects\e-d482abe8c26bd562\memory\kulipa-work-protocol.md —— 干活协议（UI 先报方案、改完必报、一个会话一件事）
3. E:\EtsHelper\docs\style-pack\ —— tokens.json / components.md / copy-tone.md（深色令牌是规范原值，浅色是推导值；间距只用 4/8/12/16/24/32/48；字号只用 12/13/14/18/24；玻璃只给导航控制层；禁 emoji/渐变按钮）
4. E:\EtsHelper\web\index.html —— 展示页现状（B1 玻璃流体球场景代码在文件尾部 `<script type="module">`，可直接复用其材质/环境贴图写法；three r160 ESM 在 web/vendor/three.module.js，importmap 引入）

【背景（不要重做）】
- 展示页已三档响应式实测通过：桌面 1440 / 平板 834（双列）/ 手机 390（单列+全宽按钮）；深浅色切换按钮在导航（localStorage 'ets-theme'，切换时重建 PMREM 环境贴图）。
- 下载区三渠道（蓝奏云 / GitHub 解析 / GitHub 直连）链接是 `href="#"` 占位，等用户提供后替换（搜 `TODO 替换 href`）。
- morphicons 无 npm/CDN 分发，页面用自绘内联 SVG 图标精灵（24px 描边），不要引入 emoji。
- 浏览器自动化：IAB 不能开 file://，需本地服务（cd web && nohup python -m http.server 8899 --bind 127.0.0.1，后台 shell 退出会杀掉它，用 nohup）；录屏用 tab.recording.start/status（WebM，outputPath 相对工作区，maxDurationMs 上限 120000），截图超时是动画每帧重绘所致，先 evaluate 暂停循环再截。
- 上一会话已在页面加过临时 window error 诊断钩子又删掉；module 脚本的 TDZ/解析错误是静默的，排查时先加回钩子再读 window.__errs。

【本会话任务：宣传视频（按顺序）】
1. 新建 `web/promo/promo.html`：全屏 three.js 宣传动画，1920×1080，12–15 秒无缝循环。内容建议：玻璃球从粒子汇聚成形 → 环绕光标扫过 → 标题「E听说助手」淡入 → 定格；复用 index.html 的玻璃材质与程序化环境贴图写法；配色守 style-pack（primary #4F7CFF）；`prefers-reduced-motion` 与 WebGL 不可用时给静态降级。
2. 用浏览器自动化录制：起本地服务 → 打开 promo.html → `tab.recording.start({ viewport:{width:1920,height:1080}, fps:30, settleMs:… })` → 等动画播完 → 停止并落盘 → 把 WebM 复制为 `web/media/promo.webm`；同时截一帧存 `web/media/poster.png`。
3. 回填 `web/index.html`：在下载区之前加「观看演示」区块，`<video src="./media/promo.webm" poster="./media/poster.png" preload="metadata" muted loop playsinline>`，点击播放；移动端同样可用（video 不用玻璃容器，遵守内容层不玻璃）。
4. 更新 `web/README.md`（录屏复现步骤 + 视频再生成方法）。
5. 收尾：`flutter analyze` + `flutter test`（54 个全过）+ `powershell -File tools/build/build_release.ps1`，**出包后核对 `产物/.../data/app.so` 时间戳**（exe 时间戳不代表 Dart 改动）。

【纪律】
UI 方案先报后写；改动最小化；不碰 `D:\ETS`（DLL 实验已归档到 tools/legacy/dll_inject_experiment/，勿复启）；全程中文；完成后必报（改了哪些文件/如何验证/遗留问题）；一个会话只做这一件事，做完提醒我开新会话。

【另请提醒我（非本会话任务）】
修改模块 r8 包还没实机验证：装新包 → 开引擎 → 装证书 → 设系统代理 → 完全退出 E听说 重开 → 做一道题 → 看修改页「最近事件」是否出现 `MITM 接管 → 命中 sync-v2 → 已改写`。
