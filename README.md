# Genesys Cloud GDPR Requests App

This repository contains code for a static web site that can be embedded inside Genesys Cloud to process GDPR requests. This is provided as developped by the Genesys EMEA Cloud Competence Centre as an example of a frontend to the PureCloud GDPR API.

**PLEASE NOTE: This is not an official Genesys application**

**Do not send requests to our Customer Care about this application**

![Screenshot](https://bytebucket.org/eccemea/gdpr-requests-app/raw/72372b2139166a0a90eaf250f9d3965788923d45/assets/images/screenshot.jpg)

## Installation

- Clone this repository
- Run `npm install`
- Create a new `Token Implicit Grant (Browser)` OAuth entry in your PureCloud org
  - Go to Admin/OAuth
  - Click on `Add Client`
  - Give it a meaningful name (e.g. `GDPR App`)
  - Description is optional
  - Set the grant type to `Token Implicit Grant (Browser)`
  - In `Authorized redirect URIs`, enter the URL to reach the `index.html` file (see `Start a web server` section for more info)
  - Click on `Save` and note the `Client ID` string value
- Edit the `static/js/genesyscloud.js` file and update the following variables with your own Genesys Cloud credentials
  - clientId: The new OAuth Client Id you just generated
  - redirectUrl: Corresponds to the full URL to your index.html and must be the same than the one you set in the OAuth credentials above (e.g. `http://localhost`)
  - environment: Either `mypurecloud.ie`, `mypurecloud.de`, `mypurecloud.com`, `mypurecloud.com.au` or `mypurecloud.jp` depending on where your PureCloud org is located
- Start a web server
  - Under windows, run `start.cmd` (requires Python)
  - Or use your own local web server
- Browse to http://localhost (or use your own URL)

You can host this static web site using Github Pages if you have a Github Account. The credentials in `static/js/genesyscloud.js` are not critical as it requires anyone who uses it to also specify a username and password to authenticate. A sample page is available [here](https://github.com/purecloudgdprrequests/purecloudgdprrequests.github.io). In this case, the `Authorized redirect URI` is `https://purecloudgdprrequests.github.io/index.html`

## How to embed in Genesys Cloud

To embed your web site directly inside PureCloud, you need to create a new `Integration`

- Go to `Admin/Integrations` and click on `+Integrations` on the top right to add a new client application
- In the `Details` tab:
  - Enter a name for the new client application
- In the `Configuration` tab, enter the following:
  - Application URL: The full path to your site (e.g. https://purecloudgdprrequests.github.io)
  - Application Type: widget
  - Application Category: (leave empty)
  - Iframe Sandbox Options: allow-scripts,allow-same-origin,allow-forms,allow-modals (default value)
  - Group Filtering: Select a group of users who can access this client application. You might have to create a new group first in `Admin/Groups`
  - Click on `Save`
- Go back to the `Details` tab and click on `Active` to activate this application

## Known issues/limitations

- Requests are not sorted by `Created Date` because each request to the Users API does not take the same amount of time and come back in a random order
