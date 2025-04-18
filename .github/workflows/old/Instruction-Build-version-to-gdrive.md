# Using GitHub Actions for browser build

These instructions allow you to build AndroidAPS with a browser.


## Setup Github AndroidAPS repository

1. Fork https://github.com/nightscout/AndroidAPS into your account. If you already have a fork of AndroidAPS in GitHub, you can't make another one. You can continue to work with your existing fork, or delete that from GitHub and then fork https://github.com/nightscout/AndroidAPS.


## Prepare your existing or new Android Studio signing keystore

1. Base64 encoding of the keystore file (on a Windows PC):\
   certutil -encode keystore.jks SIGNING_KEY.txt
2. Base64 encoding of the keystore file (on other plattforms with openssl):\
   openssl base64 < keystore.jks | tr -d '\n' | tee SIGNING_KEY.txt
3. Base64 encoding of the keystore file (on macOS):\
   base64 keystore.jks > SIGNING_KEY.txt


## Setup Github secrets for the keystore and the Google Drive API

1. In the forked AndroidAPS repository, go to Settings -> Secrets and variables -> Actions.
1. For each of the following secrets, tap on "New repository secret", then add the name of the secret, along with the value you defined during keystore creation time. As value for the secret SIGNING_KEY use the text out of the file SIGNING_KEY.txt, which is stored between the two lines --BEGIN CERTIFICATE-- and --END CERTIFICATE--.  
    * `KEY_STORE_PASSWORD`
    * `KEY_ALIAS`
    * `KEY_PASSWORD`
    * `SIGNING_KEY`
1. For the Google Drive upload of the build AndroidAPS app, the following secrets have to be defined:
    * `GDRIVE_CREDENTIALS`
    * `GDRIVE_FOLDER_ID`

### Google Service Account (GSA)
First of all, you will need a **Google Service Account** for your project. Service accounts are specific Google account types used by services instead of people. To create one, go to [*Service Accounts*](https://console.cloud.google.com/apis/credentials) in the *IAM and administration* section of the **Google Cloud Platform** dashboard. Create a new project or choose an existing one. Click on create new service account and continue with the process. At the end, you will get the option to generate a key. **Store this key safely**. It's a JSON file.

## Prepare your new Google API key

1. Base64 encoding of the Google API JSON key file (on a Windows PC):\
   certutil -encode google-api-key.json GDRIVE_CREDENTIALS.txt
2. Base64 encoding of the Google API JSON key file (on other plattforms with openssl):\
   openssl base64 < google-api-key.json | tr -d '\n' | tee GDRIVE_CREDENTIALS.txt
3. Base64 encoding of the Google API JSON key file (on macOS):\
   base64 google-api-key.json > GDRIVE_CREDENTIALS.txt

## Setup Github secrets for the Google Drive API

1. In the forked AndroidAPS repository, go to Settings -> Secrets and variables -> Actions.
1. Add the new repository secret GDRIVE_CREDENTIALS.\
   As value for the secret GDRIVE_CREDENTIALS use the text out of the file GDRIVE_CREDENTIALS.txt,\
   which is stored between the two lines --BEGIN CERTIFICATE-- and --END CERTIFICATE--.  

### Share Drive folder with the Google Service Account (GSA)

Go to your **Google Drive** and find the folder you want your files to be uploaded to and share it with the Google Service Account (GSA). You can find your service account email address in the `client_email` property of your Google Service Account (GSA) credentials.

### Setup Github secret for the upload folder on Google Drive

Go to your **Google Drive** and find the folder you want your files to be uploaded to and take note of **the folder's ID**, the long set of characters after the last `/` in your browsers address bar. Store it as new Github secret named GDRIVE_FOLDER_ID.

## Build AndroidAPS
1. On your forked AndroidAPS repository, go to Actions.
2. If not already done, activate workflow on your repository.
3. On the left side, select the workflow "Build app version to gdrive".
4. On the right side, click on the drop down menue "Run workflow" and select "Branch: master" which is the default value.
5. Then click on "Run workflow".


## Upload the build and signed app to Google Drive (direct through the workflow)
1. When the workflow (build, sign, upload to Google Drive) is completed,
   look into your Google Drive App and see the build and signed AAPS apk file.
