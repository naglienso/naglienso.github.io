![enso_logo](https://raw.githubusercontent.com/naglienso/naglienso.github.io/main/images/Logo%20vertical%20-%20dark%403x.png)

# Posta
Posta is a tool for researching postMessage communication, it allows you to track, explore and exploit postMessages vulnerabilities, with many abilities, like replay messages sent between windows of any attached browser.

![main_page](https://raw.githubusercontent.com/naglienso/naglienso.github.io/main/images/main_page.png)

![exploit_page](https://raw.githubusercontent.com/naglienso/naglienso.github.io/main/images/exploit_page.png)

# Install
1. Run posta in full dev environment with dedicated browser (Chromium) - Recommended
Node required to be pre installed
```bash
git clone https://github.com/gourarie/post-apocalypse.git ## change
cd post-apocalypse ## change
npm install
node posta **URL**
```
2. Run posta as a Chrome Extension ## to edit
```bash
git clone https://github.com/gourarie/post-apocalypse.git ## change
cd post-apocalypse ## change
npm install
node posta **URL**
```

# How to use ## to change
1. Make sure posta server is up and running (via `node posta.js`)
2. Use one of the [proxy-helpers](https://github.com/gourarie/post-apocalypse/tree/master/proxy-helpers) to inject [agent.js](https://github.com/gourarie/post-apocalypse/blob/master/src/agent.js)
3. Make sure your browser is passing through the proxy and agent.js found in responses `head` tag
4. Browse to http://localhost:28010/ and hack


 
## Known Issues
due to browser extension limitations there are some known issues
1. Using posta with alot of tabas might lag, we recommend dedicated browser
2. Sometimes you might need ot refresh the page
3. posta wont be enabled to inject itself in some cases (For example with data:uri)
# Authors ## to edit
Chen Gour Arie<br>
[Barak Tawily](https://quitten.github.io/)<br>
Gal Nagli
Omer Yaron
