![enso_logo](https://raw.githubusercontent.com/naglienso/naglienso.github.io/main/images/Logo%20vertical%20-%20dark%403x.png)

# Posta
Posta is a tool for researching postMessage communication, it allows you to track, explore and exploit postMessages vulnerabilities, with many abilities, like replay messages sent between windows of any attached browser.

![main_page](https://raw.githubusercontent.com/naglienso/naglienso.github.io/main/images/main_page.png)

![exploit_page](https://raw.githubusercontent.com/naglienso/naglienso.github.io/main/images/exploit_page.png)

# Install
There are 2 options to install posta and getting started with our postMessage tracking journey.

**1. Run posta in full dev environment with dedicated browser (Chromium) - Recommended**

[Node.js](https://nodejs.org/en/download/) is required to be installed for this option

```bash
git clone https://github.com/benso-io/posta
cd posta
npm install
node posta **URL**
```

**2. Run posta as a Chrome Extension**

```bash
git clone https://github.com/gourarie/post-apocalypse.git
navigate to chrome://extensions/
Click on "Load unpacked"
Choose the "chrome-extension" folder inside posta
Click on Posta's icon at the extensions bar.
```

# How to use
1. Navigate to a website
2. Open the posta extension and inspect the postMessages traffic.

## Tabs:
![tabs](https://raw.githubusercontent.com/naglienso/naglienso.github.io/main/images/tabs.png)

At the tabs section we can find our main origin, with the Iframes he hosts and communicates with through the session.

We can choose the specific frame by clicking on it, and observe the postMessages related to that frame only.

## Messages:
![messages](https://raw.githubusercontent.com/naglienso/naglienso.github.io/main/images/messages.png)

At the messages section, we can inspect the entire postMessage traffic which is being sent from the origin to it's Iframes, and the opposite way.

We can select specific communication for further examination by clicking on it.

The Listeners area presents the code which is in charge of handeling the communication, we can click and copy it's contents for the JS code observation.

## Console
![console](https://raw.githubusercontent.com/naglienso/naglienso.github.io/main/images/console.png)

At the console sections, we can modify the original postMessage traffic, and replay the messages with the tampered values which will get sent from the Origin to it's Iframe.

We should make tests and see if we can affect the behaviour of the website by changing the postMessage content, If we manage to do so, It's time to try and exploit if from different origin, by Clicking the "Simulate exploit"

## Exploit
![exploit_phase](https://raw.githubusercontent.com/naglienso/naglienso.github.io/main/images/exploit_phase.png)

At the exploit page, Posta will try and host the specified origin as Iframe to initiate postMessage communication, Most of the time we won't be able to do so due to Strict X Frame Options enabled on the origin website.

Therefore, In order to continue with our exploitation we will need to gain communication referance with our origin by initiating the window.open method, which is being done by clicking on "Open as tab"

We have the console to our right which will help us to modify and craft our specified payloads and test them in Cross Origin Communication, initiated by clicking on the "Exploit" Button.



 
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
