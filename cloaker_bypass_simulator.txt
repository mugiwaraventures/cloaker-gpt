# 📂 Projeto: Simulador de Ambiente para Bypass de Cloaker

Este projeto tem como objetivo simular o ambiente real de um usuário legítimo clicando em anúncios (ex: Facebook Ads), com o intuito de acessar a página real de vendas escondida por sistemas de cloaking.

> ⚠️ **Uso Ético:** Este sistema deve ser utilizado exclusivamente para fins educacionais, de auditoria e pesquisa de segurança. O uso indevido pode violar leis locais e políticas de privacidade de plataformas.

---

## ✅ Objetivo
Desenvolver um sistema capaz de:
- Simular um navegador legítimo de usuário (mobile + Facebook app)
- Personalizar headers HTTP (User-Agent, Referer)
- Usar proxy residencial/4G para simular IP válido
- Renderizar a página e capturar/redirecionar o HTML real da página de destino

---

## 🧰 Tecnologias sugeridas

- Node.js + Puppeteer ou Playwright
- Express.js (API opcional)
- Proxy rotativo residencial (como Smartproxy, ProxyRack, Brightdata)
- Banco de dados (SQLite ou MongoDB para logs)

---

## 📁 Estrutura inicial

```
cloaker-bypass-simulator/
├── index.js
├── config.js
├── utils/
│   └── headers.js
├── logs/
│   └── page_logs.json
├── proxies/
│   └── active_proxies.txt
├── README.md
└── .env
```

---

## 🔧 Configurações básicas

### `config.js`
```js
module.exports = {
  proxy: 'http://user:pass@residential-proxy.com:port',
  userAgent: 'Mozilla/5.0 (iPhone; CPU iPhone OS 16_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/15E148 [FBAN/FBIOS;FBAV/379.0.0.30.112]',
  referer: 'https://www.facebook.com/',
  timeout: 30000
};
```

---

## 🧪 Exemplo de uso com Puppeteer
```js
const puppeteer = require('puppeteer-extra');
const StealthPlugin = require('puppeteer-extra-plugin-stealth');
const { proxy, userAgent, referer } = require('./config');
const fs = require('fs');

puppeteer.use(StealthPlugin());

(async () => {
  const browser = await puppeteer.launch({
    headless: true,
    args: [
      `--proxy-server=${proxy}`,
      '--no-sandbox',
      '--disable-setuid-sandbox'
    ]
  });

  const page = await browser.newPage();
  await page.setUserAgent(userAgent);
  await page.setExtraHTTPHeaders({ referer });

  const targetUrl = 'https://l.facebook.com/l.php?...';
  await page.goto(targetUrl, { waitUntil: 'networkidle2', timeout: 60000 });

  const content = await page.content();
  fs.writeFileSync('./logs/page.html', content);
  console.log('Página capturada com sucesso.');

  await browser.close();
})();
```

---

## 📄 `utils/headers.js`
```js
const defaultHeaders = (referer) => ({
  referer,
  'Accept-Language': 'en-US,en;q=0.9',
  'Upgrade-Insecure-Requests': '1'
});

module.exports = { defaultHeaders };
```

---

## 📌 Próximos passos
- [x] Captura de HTML e logs
- [ ] Adicionar captura de screenshots
- [ ] Armazenar HTML em banco
- [ ] Detecção de redirecionamentos em cadeia
- [ ] Dashboard para visualização de logs
- [ ] Integração com webhook para alertas

---

## 🛡️ Reforço legal
Este projeto é para **pesquisa e segurança**. Nunca utilize para:
- Clonar páginas sem permissão
- Espionar concorrência de forma antiética
- Fraudar sistemas de ads

---
