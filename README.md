# Chrome-Extension
The structure will be :
1/ Install Extension to Chrome
2/ Define your alert settings
3/ Import your profile
4/ Define the projects you want
5/ Wait for email alerts or check manually
6/ Apply to projects

-------------------
Creating a Google Chrome extension with the described structure involves several components, including background scripts, popup scripts, options pages, and content scripts. Below is an overview of how to implement such an extension with a simple design based on your requirements.
Project Directory Structure

my-chrome-extension/
├── src/
│   ├── background.js
│   ├── content.js
│   ├── popup.js
│   ├── options.js
│   ├── options.html
│   ├── popup.html
│   ├── style.css
├── manifest.json
└── icons/
    └── icon.png

1. manifest.json

This file defines the extension's metadata, permissions, and configurations.

{
  "manifest_version": 3,
  "name": "Project Application Alerts",
  "version": "1.0",
  "description": "A Chrome extension for project application alerts.",
  "permissions": [
    "activeTab",
    "storage",
    "notifications"
  ],
  "background": {
    "service_worker": "src/background.js"
  },
  "options_page": "src/options.html",
  "action": {
    "default_popup": "src/popup.html",
    "default_icon": {
      "16": "icons/icon.png",
      "48": "icons/icon.png",
      "128": "icons/icon.png"
    }
  },
  "storage": {
    "managed_schema": "storage-schema.json"
  },
  "host_permissions": [
    "http://*/*",
    "https://*/*"
  ]
}

2. Background Script (background.js)

This file handles core functionalities, including listening for changes in profile settings, projects, and alert triggers.

// src/background.js

chrome.runtime.onInstalled.addListener(() => {
  console.log('Extension installed and ready.');
});

// Listener for email alerts (simulate email system)
chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  if (message.type === 'sendEmailAlert') {
    // Simulate sending an email alert
    console.log(`Sending email alert to ${message.email}`);
    sendResponse({ status: 'Alert sent successfully' });
  }
});

3. Options Page (options.html)

This page will allow the user to define their profile, alert settings, and project preferences.

<!-- src/options.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Extension Options</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <h1>Define your settings</h1>

    <label for="profile">Profile:</label>
    <input type="text" id="profile" name="profile" />

    <label for="alertFrequency">Alert Frequency:</label>
    <select id="alertFrequency">
      <option value="daily">Daily</option>
      <option value="weekly">Weekly</option>
    </select>

    <label for="projects">Define Projects:</label>
    <textarea id="projects" rows="5" placeholder="Enter project names, one per line"></textarea>

    <button id="saveSettings">Save Settings</button>

    <script src="options.js"></script>
  </body>
</html>

4. Options Script (options.js)

This script saves user preferences (profile, alert frequency, projects) to Chrome storage.

// src/options.js

document.getElementById('saveSettings').addEventListener('click', () => {
  const profile = document.getElementById('profile').value;
  const alertFrequency = document.getElementById('alertFrequency').value;
  const projects = document.getElementById('projects').value.split('\n');

  // Save settings in Chrome storage
  chrome.storage.sync.set({ profile, alertFrequency, projects }, () => {
    alert('Settings saved!');
  });
});

5. Popup HTML (popup.html)

This is the popup interface of the extension that allows the user to check for alerts or trigger the project application manually.

<!-- src/popup.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Project Alerts</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <h1>Project Application</h1>
    <button id="checkAlerts">Check for Alerts</button>
    <button id="applyToProjects">Apply to Projects</button>

    <script src="popup.js"></script>
  </body>
</html>

6. Popup Script (popup.js)

This script handles interactions with the popup UI, allowing the user to manually trigger the checking of alerts or apply to projects.

// src/popup.js

document.getElementById('checkAlerts').addEventListener('click', () => {
  chrome.storage.sync.get(['profile', 'alertFrequency', 'projects'], (data) => {
    const { profile, alertFrequency, projects } = data;
    alert(`Checking for alerts for profile: ${profile}, alert frequency: ${alertFrequency}, projects: ${projects.join(', ')}`);
    
    // Simulate alert check
    chrome.runtime.sendMessage({ type: 'sendEmailAlert', email: 'user@example.com' }, (response) => {
      console.log(response.status);
    });
  });
});

document.getElementById('applyToProjects').addEventListener('click', () => {
  chrome.storage.sync.get(['profile', 'projects'], (data) => {
    const { profile, projects } = data;
    alert(`Applying to projects: ${projects.join(', ')} for profile: ${profile}`);
  });
});

7. Style CSS (style.css)

This file provides basic styling for the extension's pages.

/* src/style.css */
body {
  font-family: Arial, sans-serif;
  padding: 20px;
}

h1 {
  font-size: 18px;
  margin-bottom: 10px;
}

label {
  display: block;
  margin: 5px 0;
}

input, select, textarea {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
}

button {
  background-color: #4CAF50;
  color: white;
  padding: 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

8. Icons

Ensure that you add an appropriate icon (e.g., icon.png) in the icons/ folder.
Steps to Install the Extension

    Open Chrome and go to chrome://extensions/.
    Enable Developer Mode (top right).
    Click Load unpacked and select the extension directory (where the manifest.json is).
    The extension should appear in your Chrome toolbar.
    Right-click on the extension icon, select Options, and configure the extension with your profile, alert settings, and projects.
    Use the popup to check for alerts or manually apply to the projects.

Key Features Recap:

    Define Your Profile: Enter a profile that will be used for project applications.
    Alert Settings: Choose the frequency of email alerts (daily or weekly).
    Project Preferences: Define the projects you are interested in.
    Email Alerts: Receive email notifications (simulated in the code).
    Manual Project Application: Trigger project application manually or wait for alerts.

Conclusion:

This extension provides a basic framework for managing profile-based project applications with alert functionalities. You can extend it by adding features such as fetching live project data, integrating with external APIs for email alerts, and more advanced functionalities like tracking application statuses.
