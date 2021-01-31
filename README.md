# Cloudflare DNS Collector

<br /><p align="center"><img height="320" src="./images/1200px-Cloudflare-logo-vector.svg.png" alt="Cloudflare Collector"></a></p>


## Prerequisites
In order for our DNS Collector CloudFlare collector to function correctly we will need DNS Zone Read only API token from the customer

## API Key Creation

1. Navigate to https://dash.cloudflare.com/profile/api-tokens
1. Click on "Create Token"
1. Use the "Edit zone DNS" template
1. Use the following permissions like the image below:
   * ZONE DNS Read
   * Include Specific Zone with the specified TLD
   * IP Address filtering (optional)
   
<br /><p align="center"><img height="320" src="./images/TOKEN.png" alt="Cloudflare Collector"></a></p>   

### Development Environment

Run Posta in full dev environment with dedicated browser (Chromium)

1. Install Posta
   ```bash
   git clone https://github.com/benso-io/posta
   cd posta
   npm install
   ```
1. Launch the dedicated Chromium session using the following command:
   ```bash
   node posta <URL>
   ```
1. Click on the Posta extension to navigate to the UI

### Chrome Extension

#### Run Posta as a Chrome / Chromium Extension

1. Clone the repo:
   ```bash
   git clone https://github.com/benso-io/posta.git
   ```
1. Navigate to `chrome://extensions`
1. Make sure **Developer mode** is enabled
1. Click on **Load unpacked**
1. Choose the `chrome-extension` directory inside Posta and upload it to your browser
1. Load the extension
1. Pin the extension to your browser
1. Browse to the website you would like to examine
1. Click on the *Posta* extension to navigate to the UI

## Tabs
In the **Tabs** section we can find our main Origin, with the iframes it hosts and communicates with through the session.
We can choose the specific frame by clicking on it, and observe the postMessages related to that frame only.

![tabs](./images/tabs.png)

## Messages
In the *Messages* section, we can inspect all `postMessage` traffic being sent from the origin to its iframes, and vice versa.
We can select specific communication for further examination by clicking on it.
The 8Listeners* area presents the code which is in charge of handling the communication, we can click and copy its contents for JS code observation.

![messages](./images/messages.png)
![listener](./images/listeners.png)

## Console
At the console sections, we can modify the original postMessage traffic, and replay the messages with the tampered values which will get sent from the Origin to it's Iframe.

We should make tests and see if we can affect the behaviour of the website by changing the postMessage content, If we manage to do so, It's time to try and exploit if from different origin, by Clicking the "Simulate exploit"

![console](./images/console.png)


## Exploit

At the exploit page, Posta will try and host the specified origin as Iframe to initiate postMessage communication, Most of the time we won't be able to do so due to X-Frame-Options enabled on the origin website.

Therefore, In order to continue with our exploitation we will need to gain communication referance with our origin by initiating the window.open method, which is being done by clicking on "Open as tab"

We have the console to our right which will help us modify and craft our specified payloads and test them in Cross Origin Communication, initiated by clicking on the "Exploit" Button.

![exploit_content](./images/exploit_content.png)

# Authors
- Chen Gour Arie
- [Barak Tawily](https://quitten.github.io/)
- [Gal Nagli](https://github.com/NagliNagli)
- Omer Yaron
