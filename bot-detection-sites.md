# 浏览器自动化 / Bot 检测网站清单

> 配合 fingerprint-chromium（148.0.7778.215 自编译版）测试用
> 标注 ✔️ 的是 fingerprint-chromium 作者在 README 中自测通过的项目

## 一、综合指纹 + Bot 评分

| 网站 | 检测内容 | 作者自测 |
|---|---|---|
| [CreepJS](https://abrahamjuliot.github.io/creepjs/) | 指纹一致性、谎言检测（lie detection） | ✔️ 51.5% |
| [PixelScan](https://pixelscan.net/fingerprint-check) | 指纹一致性 + bot 评分 | ✔️ 音频通过 |
| [BrowserScan](https://browserscan.net/) | 指纹 + 自动化特征，中文界面 | ✔️（GPU 项注意） |
| [BrowserLeaks](https://browserleaks.com/) | 十几个维度的指纹泄漏 | ✔️ |
| [FingerprintJS Demo](https://fingerprint.com/demo/) | 商业级指纹 + BotD 识别 | — |
| [Cover Your Tracks (EFF)](https://coveryourtracks.eff.org/) | 唯一性 / 可追踪性 | — |
| [DeviceInfo.me](https://www.deviceinfo.me/) | 全量设备信息汇总 | — |
| [AmIUnique](https://amiunique.org/) | 指纹唯一性分析 | — |

## 二、自动化 / WebDriver 特征专项

| 网站 | 检测内容 |
|---|---|
| [bot.sannysoft.com](https://bot.sannysoft.com/) | 经典项：`navigator.webdriver`、Chrome 对象残留、权限伪装等十余项 |
| [Intoli Headless 测试页](https://intoli.com/blog/not-possible-to-block-chrome-headless/chrome-headless-test.html) | Headless 特有特征 |
| [Fingerprint BotD](https://fingerprint.com/products/bot-detection/) | CDP 检测、自动化框架特征 |

## 三、人机验证（真刀真枪过盾）

| 网站 | 说明 |
|---|---|
| [NopeCHA Turnstile](https://nopecha.com/demo/turnstile) | Cloudflare Turnstile demo，作者已验证 ✔️ |
| [2Captcha Turnstile](https://2captcha.com/demo/cloudflare-turnstile) | Cloudflare Turnstile demo |
| [2Captcha reCAPTCHA](https://2captcha.com/demo/recaptcha-v2) | reCAPTCHA v2 demo |
| [reCAPTCHA v2 demo](https://www.google.com/recaptcha/api2/demo) | Google 官方 demo |
| [nowsecure.nl](https://nowsecure.nl/) | Cloudflare 盾实测站 |

## 四、网络栈指纹（容易被忽略的层）

| 网站 | 检测内容 |
|---|---|
| [tls.peet.ws](https://tls.peet.ws/) | JA3 / JA4、HTTP/2 帧指纹 |
| [BrowserLeaks TLS](https://browserleaks.com/tls) | JA3 |
| [Scrapfly HTTP/2 指纹](https://scrapfly.io/web-scraping-tools/http2-fingerprint) | Akamai 指纹 |
| [IPQualityScore](https://www.ipqualityscore.com/free-ip-lookup-proxy-vpn-test) | IP 信誉 / 代理 / 欺诈评分 |
| [Scamalytics](https://scamalytics.com/) | IP 欺诈评分 |

## 五、实战提示

1. **fingerprint-chromium 的能力边界**
   - 它改的是 JS/设备层：UA、Canvas、WebGL、字体、音频、CDP 特征、`navigator.webdriver` 等
   - TLS/JA3 和 HTTP/2 指纹由 Chromium 原生网络栈产生，本来就是真 Chrome 的指纹，无需担心
   - 真正容易翻车的两个点：
     - **出口 IP**：数据中心 IP 直接被 IPQS/DataDome 标红，需配住宅代理
     - **行为层**：无鼠标轨迹的瞬间点击、匀速滚动等自动化行为特征

2. **建议测试顺序**
   1. bot.sannysoft.com —— 最快见分晓（一眼看出哪些自动化特征泄漏）
   2. BrowserScan / PixelScan —— 看综合 bot 评分
   3. nopecha Turnstile —— 真实过 Cloudflare 盾
   4. IPQualityScore —— 单独查出口 IP 质量

3. **对比基线方法**
   - 同一个网站：日常 Chrome 开一次 vs 指纹浏览器开一次
   - 差异项就是需要调参的地方，常用参数：
     - `--fingerprint=<seed>` 启用指纹
     - `--fingerprint-platform=windows|macos|linux` 改 OS
     - `--fingerprint-brand=Chrome|Edge|Opera|Vivaldi` 改品牌
     - `--timezone=<tz>` / `--lang` / `--accept-lang` 时区语言
     - `--disable-spoofing=gpu` GPU 指纹项出问题时可尝试禁用

---
*生成于 fingerprint-chromium 本地编译完成之后（2025-10-24）*
