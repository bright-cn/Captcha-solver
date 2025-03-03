# CAPTCHA 解码器

[![Promo](https://github.com/luminati-io/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://www.bright.cn/)

使用 [Bright Data 的 CAPTCHA 解码器](https://www.bright.cn/products/web-unlocker/captcha-solver) 轻松解决包括 reCAPTCHA、hCaptcha、PX Captcha、GeeTest 等在内的各种 CAPTCHA，提供用户模拟、指纹管理以及强大的代理服务器基础设施。  
我们的 CAPTCHA 解码器是内置于 [抓取浏览器](https://www.bright.cn/products/scraping-browser) 和 [Web Unlocker](https://www.bright.cn/products/web-unlocker) 的默认功能。

想了解更多自定义 CDP 功能，请点击 [这里](https://docs.brightdata.com/scraping-automation/scraping-browser/cdp-functions/custom#captcha-solver)。

--------------------------------------------------------------------------------

## 目录
1. [功能特点](#功能特点)  
2. [为什么选择 CAPTCHA 解码器](#为什么选择-captcha-解码器)  
3. [CAPTCHA 解码器的工作原理](#captcha-解码器的工作原理)  
   1. [自动识别 CAPTCHA](#自动识别-captcha)  
      - [指令概览](#指令概览)  
      - [NodeJS 示例](#example-nodejs-puppeteer)  
      - [事件监控](#事件监控)  
   2. [手动管理 CAPTCHA](#手动管理-captcha)  
4. [支持的 CAPTCHA 类型](#支持的-captcha-类型)  
5. [高级定制](#高级定制)  
6. [价格](#价格)  
7. [为什么开发者喜欢 CAPTCHA 解码器](#为什么开发者喜欢-captcha-解码器)  
8. [常见问题 FAQ](#常见问题-faq)  

--------------------------------------------------------------------------------

## 功能特点  
- 快速且自动化的 CAPTCHA 解决方案  
- 兼容 reCAPTCHA、hCaptcha、PX Captcha、GeeTest、SimpleCaptcha 等  
- 智能用户模拟与指纹管理，轻松绕过检测  
- 由拥有超 1 亿 IP 的获奖 [代理网络](https://www.bright.cn/proxy-types) 提供支持  
- 按实际结果付费，99.9% 在线率并提供 24/7 全天候支持  

--------------------------------------------------------------------------------

## 为什么选择 CAPTCHA 解码器

- **超过 20,000+ 全球客户的信赖**  
- **为开发者而打造**  
  - 基于 AI 的解锁逻辑  
  - 自动化 CAPTCHA 解码及重试  
  - 内置 JavaScript 渲染  
  - 易于与 Puppeteer、Playwright、Selenium 等工具集成  

 > **📚 了解更多网络爬虫相关知识：**  
 >> [**Puppeteer**](https://www.bright.cn/blog/how-tos/web-scraping-puppeteer)<br>
 >> [**Playwright**](https://www.bright.cn/blog/how-tos/playwright-web-scraping)<br>
 >> [**Selenium**](https://www.bright.cn/blog/how-tos/using-selenium-for-web-scraping)

- **无可匹敌的可靠性**  
  - 99.9% 成功率  
  - 超过 4 年的研发投入，80+ 专职工程师  
  - 每年处理超过 5.5 万亿次数据请求  

--------------------------------------------------------------------------------

# CAPTCHA 解码器的工作原理

Bright Data 的 CAPTCHA 解码器已整合到 **Scraping Browser** 和 **Web Unlocker** 中，默认为**自动解码 CAPTCHA**。您可以：  
- 在代码中监控解码过程  
- 使用 Chrome DevTools Protocol (CDP) 命令手动控制 CAPTCHA 解码行为  
- 完全禁用 CAPTCHA 解码功能（如有需要）  

--------------------------------------------------------------------------------

## 自动识别 CAPTCHA

使用 `Captcha.solve` 指令可自动检测并解决 CAPTCHA。Python 版本示例可点击 [这里](https://docs.brightdata.com/scraping-automation/scraping-browser/cdp-functions/custom#captcha-solver) 查看。

### 指令概览

```javascript
Captcha.solve({
    detectTimeout?: number // CAPTCHA 检测的超时时间（毫秒）
    options?: CaptchaOptions[] // CAPTCHA 解码的配置项
}) : SolveResult
```

### Example: NodeJS (Puppeteer)

```javascript
(async () => {
  const page = await browser.newPage();
  const client = await page.target().createCDPSession();
  await page.goto('https://site-with-captcha.com');
  try {
    // 自动解决 CAPTCHA
    const { status } = await client.send('Captcha.solve', { detectTimeout: 30000 });
    console.log(`CAPTCHA solve status: ${status}`);
  } catch (error) {
    console.error('Error solving CAPTCHA:', error);
  }
})();
```

### 事件监控

您可以监听特定的 CAPTCHA 解码事件来处理高级用例：

- **`Captcha.detected`**：检测到 CAPTCHA 并开始解码  
- **`Captcha.solveFinished`**：成功解码  
- **`Captcha.solveFailed`**：解码失败  
- **`Captcha.waitForSolve`**：等待解码器完成  

#### NodeJS 示例 - 监听事件

```javascript
const client = await page.target().createCDPSession();
await new Promise((resolve, reject) => {
  client.on('Captcha.solveFinished', (result) => {
    if (result.status === 'success') {
      resolve();
    } else {
      reject(new Error('CAPTCHA solving failed with status: ' + result.status));
    }
  });
  client.on('Captcha.solveFailed', () => reject(new Error('CAPTCHA solving failed')));
  setTimeout(() => reject(new Error('CAPTCHA solve timeout')), 300000); // 超时设置为5分钟，可根据需求调整
});
```

--------------------------------------------------------------------------------

## 手动管理 CAPTCHA

需要更高的灵活度？您可以自行配置解码行为，或完全禁用自动解码。

### 禁用自动 CAPTCHA 解码

```javascript
Captcha.setAutoSolve({  
  autoSolve: false // 禁用自动解码
});
```

### 针对特定类型禁用自动解码

```javascript
Captcha.setAutoSolve({  
  autoSolve: true,  
  options: [{  
    type: 'usercaptcha', // 对该类型禁用自动解码
    disabled: true  
  }]  
});
```

### 手动执行 CAPTCHA 解码

```javascript
(async () => {
  const page = await browser.newPage();
  const client = await page.target().createCDPSession();
  await client.send('Captcha.setAutoSolve', { autoSolve: false });
  await page.goto('https://site-with-captcha.com');
  try {
    const { status } = await client.send('Captcha.solve', { detectTimeout: 30000 });
    console.log('CAPTCHA solve status:', status);
  } catch (error) {
    console.error('Error solving CAPTCHA:', error);
  }
})();
```

--------------------------------------------------------------------------------

## 支持的 CAPTCHA 类型

我们的解码器支持多种 CAPTCHA，包括：

- [**reCAPTCHA**](https://www.bright.cn/products/web-unlocker/captcha-solver/recaptcha)  
- [**Click Captcha**](https://www.bright.cn/products/web-unlocker/captcha-solver/click-captcha)  
- [**hCaptcha**](https://www.bright.cn/products/web-unlocker/captcha-solver/hcaptcha)  
- [**PerimeterX**](https://www.bright.cn/products/web-unlocker/captcha-solver/perimeterx)  
- [**SimpleCaptcha**](https://www.bright.cn/products/web-unlocker/captcha-solver/simplecaptcha)  
- [**FunCaptcha**](https://www.bright.cn/products/web-unlocker/captcha-solver/funcaptcha)  
- [**Cloudflare Turnstile**](https://www.bright.cn/products/web-unlocker/captcha-solver/cloudflare-turnstile)  
- [**AWS WAF Captcha**](https://www.bright.cn/products/web-unlocker/captcha-solver/aws-waf-captcha)  
- [**GeeTest CAPTCHA**](https://www.bright.cn/products/web-unlocker/captcha-solver/geetest-captcha)  
- [**KeyCAPTCHA**](https://www.bright.cn/products/web-unlocker/captcha-solver/keycaptcha)  
- [**Puzzle CAPTCHA**](https://www.bright.cn/products/web-unlocker/captcha-solver/puzzle-captcha)  
- [**Yandex CAPTCHA**](https://www.bright.cn/products/web-unlocker/captcha-solver/yandex-captcha)  
- [**Image CAPTCHA**](https://www.bright.cn/products/web-unlocker/captcha-solver/image-captcha)  
- [**Text CAPTCHA**](https://www.bright.cn/products/web-unlocker/captcha-solver/text-captcha)  

--------------------------------------------------------------------------------

## 高级定制

使用高级设置可对 CAPTCHA 解码逻辑进行细化调整。

### 示例：用于 Cloudflare 挑战的自定义选项

```javascript
const cfOptions = {
  timeout: 40000,
  selector: '#challenge-body-text, .challenge-form',
  check_timeout: 300,
  success_selector: '#challenge-success[style*=inline]',
  wait_networkidle: { timeout: 500 }
};
```

--------------------------------------------------------------------------------

## 价格

| **套餐**             | **价格（每 1K 次结果）** | **月度费用**     | **描述**                                                                 |  
|----------------------|--------------------------|------------------|--------------------------------------------------------------------------|  
| **按量付费**         | $1.50                   | 无固定承诺       | 适用于不定期的爬虫需求。                                                 |  
| **Growth** (成长)    | $1.27                   | $499             | 适合正在扩张的团队。                                                     |  
| **Business** (商务)  | $1.12                   | $999             | 适用于大规模爬虫业务。                                                   |  
| **Premium** (高级)   | $1.05                   | $1,999           | 适合关键业务，提供高级功能及优先支持。                                   |  
| **Enterprise** (企业)| 自定义报价             | 联系我们         | 专属高端方案，高级 SLA、专属客户经理、SSO 及个性化解决方案。             |  

🚀 **特别优惠**：初次充值最高可享受 **$500** 等额匹配！

--------------------------------------------------------------------------------

## 为什么开发者喜欢 CAPTCHA 解码器

- **集成简易**：可与 Puppeteer、Playwright、Selenium 无缝对接。  
- **先进的 AI 逻辑**：自动处理重试、解码、指纹管理、IP 轮换及高级请求头等。  
- **内置浏览器**：无需额外管理 JavaScript 渲染环境。  
- **实时监控**：通过实时仪表板监控网络性能。  
- **无与伦比的支持**：24/7 全球客户支持，每日都有新功能发布。  

--------------------------------------------------------------------------------

## 常见问题 FAQ

### **CAPTCHA 解码器如何工作？**  
CAPTCHA 解码器使用先进的基于 AI 的逻辑，自动检测、分析并解码 CAPTCHA。  

### **能同时处理多个 CAPTCHA 吗？**  
可以。该方案可同时处理多种类型的 CAPTCHA，并具备极佳的扩展性。  

### **如果解码失败怎么办？**  
系统会自动进行重新尝试。如果问题依旧，可以联系我们 24/7 全天候支持团队以进行排查。  

--------------------------------------------------------------------------------

**🌟 立即开始，告别 CAPTCHA 的困扰！**  

[开始免费试用](https://www.bright.cn/products/web-unlocker/captcha-solver)  
