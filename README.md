# ERPNext-15 GoogleFilePicker with Shared Folders

In ERPNext v15, I changed the following file to enable showing Shared Drives on the Google Picker Dialog:

	bench/apps/frappe/frappe/public/js/integrations/google_drive_picker.js

I added the "shared drives" and "shared with me" views to the dialog builder as follows:

        createPicker(access_token) {
                const sharedDrivesView = new google.picker.DocsView(google.picker.ViewId.DOCS)
                        .setMode(window.google.picker.DocsViewMode.LIST)
                        .setEnableDrives(true)
                        .setIncludeFolders(true); // creates just the shared drives view

                const sharedWithMeView = new google.picker.DocsView(google.picker.ViewId.DOCS)
                        .setOwnedByMe(false); // creates just the shared with me view

                this.view = new google.picker.View(google.picker.ViewId.DOCS);
                this.picker = new google.picker.PickerBuilder()
                        .setDeveloperKey(this.developerKey)
                        .setAppId(this.appId)
                        .setOAuthToken(access_token)
                        .addView(this.view)
                        .addView(sharedDrivesView)
                        .addView(sharedWithMeView)
                        .addView(new google.picker.DocsUploadView())
                        .setLocale(frappe.boot.lang)
                        .setCallback(this.pickerCallback)
                        .build();
                this.picker.setVisible(true);
                this.setupHide();
        }


Then I ran the following commands to make the changes effective:

	bench build
	bench clear-cache

I adapted the solution from the below page:

	https://stackoverflow.com/questions/75268380/how-to-show-a-list-of-shared-with-me-files-in-google-picker

# ERPNext-12-GoogleFilePicker

This project provides Google File Picker integration for ERPNext v12.1.

<img src="https://raw.githubusercontent.com/osmansen/ERPNext-12-GoogleFilePicker/master/googleFilePicker.png">

To get the integration on your ERPNext installation, you will need to download the two files in this project and edit the following lines in FileUploader.vue with your own settings:

	developerKey: "xxxxxxxxxxxxxxxxxxx",
	appId: "123456789",
	apiUrl: "https://apis.google.com/js/api.js",
	scope: "https://www.googleapis.com/auth/drive.file",
	clientId: "xxxxxxxxxx.apps.googleusercontent.com",
	loginDomain: null, // "yoursite.com",
<p>
Once you complete editing the settings above, copy the two files into their folders in your ERPNext installation:
</p><p><code>
  frappe-bench/apps/frappe/frappe/public/js/frappe/file_uploader/FileUploader.vue</code><br/>
  <code>frappe-bench/apps/frappe/frappe/handler.py
  </code></p>
<p>
Then build your installation with the following command. This is necessary to build the js packs with the changes in FileUploader.vue:
</p>
<p><code>bench build</code><p>
<p><code>bench clear-cache</p></code>
 <p>
And finally restart your ERPNext. Alternatively, you can reboot your server. This is necessary to reflect the changes in handler.py:
</p>
<p>
  <code>
bench restart
</code></p>
<p>I added the Google Picker integration js into the FileUploader.vue file; I know I should have done the integration as a Vue component but I chose the path that was easier for me.</p>
<p>The integration works without deploying the handler.py file but then the name of the file you pick from the drive is not saved in ERPNext. It was disturbing for me to see every file I attached showing up with the name "view".</p> 
<p>Enjoy!</p>
