# Strapi Plugin Dynamic.xyz Authentication

Welcome to the Strapi plugin for Firebase Authentication! This plugin seamlessly integrates Firebase Authentication with
your Strapi Headless CMS, allowing you to manage and authenticate Firebase users directly from the Strapi moderation
panel. This guide will take you through the installation and configuration process and provide information on how to use
this plugin with iOS and Android apps. This plugin would be enabled by default for super admins only.

This package is based on the Strapi Plugin for Firebase Authentication by @swensonne.


## How it works

The Dynamic.xyz Auth plugin works by authenticating users using Dynamic.xyz API Authentication. Once a user is authenticated, the
plugin creates a Strapi user account for them if it doesn't already exist. The plugin also syncs the user's Dynamic.xyz
data for a specific environment with their Strapi account.

## Table of Contents

1. [Installation](#installation)
2. [Configuration](#configuration)
3. [Usage](#usage)

## Installation

To get started, you need to install the Dynamic.xyz Auth plugin for Strapi. We recommend using yarn for this installation.

### Via Command Line

```bash
yarn add @pagedao/strapi-plugin-dynamicxyz-auth
```

Once the installation is complete, you must rebuild your Strapi instance. You can do this with the following commands:

```bash
yarn build
yarn develop
```

Alternatively, you can run Strapi in development mode with the `--watch-admin` option:

```bash
yarn develop --watch-admin
```

The **Dynamic Auth** plugin should now be available in the **Plugins** section of your Strapi sidebar.

## Configuration

In order to configure the Firebase Auth plugin, follow these steps:

### Step 1 - Enable the Plugin

In your Strapi project, edit the `config/plugins.js` or `config/<env>/plugins.js` file to enable the Firebase Auth
plugin. If the file doesn't exist, create it manually. If you already have configurations for other plugins, add
the `firebase-auth` section to your existing `plugins` configuration.

To ensure the security of sensitive information, we have implemented a robust encryption process for the Firebase config JSON file in this project. The encrypted data is then stored as a hash in the database. Follow the steps below to set up and integrate Firebase with Strapi securely.

```js
module.exports = () => ({
    // ...

    "firebase-auth": {
        enabled: true,
        config:{ FIREBASE_JSON_ENCRYPTION_KEY:"encryptMe" }
    },

    // ...
});
```
Replace `"encryptMe"` with a strong and secure key. This key will be used in the encryption and decryption process.

### Step 2: Firebase Configuration Encryption and Integration with Strapi

To ensure the security of sensitive information, we have implemented a robust encryption process for the Firebase config JSON file in this project. The encrypted data is then stored as a hash in the database. Follow the steps below to set up and integrate Firebase with Strapi securely.

#### Step 1: Obtain Firebase Service Account Key

Navigate to the [Firebase Console](https://console.firebase.google.com/) and access your project. In the settings, locate the service account section and download the JSON key file. This file contains sensitive information required for Firebase Authentication.

#### Step 2: Submit Service Account Key in Strapi Settings

Access the Strapi admin panel and navigate to the settings page. Look for the section related to Firebase integration. Here, you will find an option to submit the `.json` service account key file. Upload the file you obtained in Step 2.

This service account key is essential for proper authentication with Firebase. It contains the necessary credentials for your Firebase project.

#### Step 3: Save Changes

After submitting the service account key, make sure to save the changes in the Strapi settings. This ensures that the encrypted configuration is stored securely in the database.
the `.json` [service account key file](https://firebase.google.com/docs/app-distribution/authenticate-service-account).
This key is essential for Firebase Authentication to work properly.

### Step 4 - Rebuild Admin Panel

After configuring the plugin, rebuild the Strapi admin panel:

```bash
npm run build
```

Alternatively, you can simply delete the existing `./build` directory.

### Step 5 - Grant Public Permissions

From the Strapi admin panel, navigate to "Users-permissions" and grant public role
to `{Your domain or localhost}/admin/settings/users-permissions/roles/2`. Make sure to enable public access to the
Firebase Authentication endpoint.

That's it! You're ready to use Firebase Authentication in your Strapi project. Enjoy! 🎉

## Usage


### Handling User Information

To ensure proper handling of user information, make sure to include the following fields in the user object:

- `firstName`
- `lastName`
- `phoneNumber`
- `email`

These fields can be populated during the creation of the user object if `profileMetaData` is provided.

#### Using `firebase-auth` Endpoint

When interacting with the `firebase-auth` endpoint, use the following JSON structure in the request body:

```json
{
    "idToken": "{{idToken}}",
    "profileMetaData": {
        "firstName": "name",
        "lastName": "name",
        "email": "email@gmail.com",
        "phoneNumber" : "+100000000"
    }
}
```

These values will be utilized only when the user does not exist and will be ignored in other cases.
