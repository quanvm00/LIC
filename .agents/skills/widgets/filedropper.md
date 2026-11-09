# File Dropper

- **Widget ID:** `com.mendix.widget.custom.filedropper.FileDropper`
- **Type:** CUSTOMWIDGET
- **Version:** 1.4.0

## MDL Example

```sql
CUSTOMWIDGET 'com.mendix.widget.custom.filedropper.FileDropper' widget1
```

## Properties

| Property | Type | Required | Default | Values / notes | Group | Description |
|----------|------|----------|---------|----------------|-------|-------------|
| `dataFileEntity` | entity | Yes |  |  | Data::Entity | File entity |
| `dataNameAttr` | attribute | Yes |  |  | Data::Attributes | Attribute for name of the file, usually 'Name' |
| `dataTypeAttr` | attribute |  |  |  | Data::Attributes | Attribute for the filetype, it is not required, it can be used to filter data. Values are like 'image/jpeg' etc. |
| `dataExtAttr` | attribute |  |  |  | Data::Attributes | Attribute for the extension. Is is not required, but can be used to filter data. Values are like 'jpeg', 'png' etc. The widget will determine based on the fileType what the expected extension is. If unknown, it will set it to 'unknown'. |
| `dataContextAssociation` | entity |  |  |  | Data::Context | If provided used to set the association of new files to the context. Should be a path to the context. |
| `dataAutoSave` | boolean | Yes | true |  | Data::Save | Automatically save to Mendix. If you set this to 'false' you will have to press a manual button to save the file to Mendix. |
| `dataSaveMethod` | enumeration | Yes | postRequest | `postRequest` \| `saveDocument` | Data::Save | Determine how the data is uploaded. Using the saveDocument method is the easiest, where it will use the standard Mendix methods. This is the most stable. Downside: no progress, so the progress bar will be shown as 50% while uploading. You can also use the POST method, which would provide a progress bar. |
| `restrictionMaxFileSize` | integer | Yes | 5 |  | Restrictions | Maximum file size in MB |
| `restrictionMaxFileCount` | integer | Yes | 0 |  | Restrictions | Maximum number of files to upload, if > 0 |
| `restrictionMimeType` | string |  | image/*,audio/*,video/* |  | Restrictions | Restrict the files to a certain type using Mime types. You can use a string like 'image/*,audio/*,video/*'. Note 1: This might not always be accurate. There can be differences between platforms & browsers. To be sure it is better to use the 'onAccept Microflow' Note 2: When using an Entity that is/inherits 'System.Image', Mime types will always be ignored and set to 'image/*'. You can further restrict this using the 'onAccept Microflow' |
| `verificationOnAcceptMicroflow` | microflow |  |  |  | Verification after upload::On File object | Microflow that will be called to determine if a file is accepted. If not, it should return a false. The Dropzone will remove the file by itself. This microflow will use the entity defined in 'Data'. Note: This microflow is executed AFTER the file is uploaded. When doing big uploads, it is better to maybe also add a verification before upload. |
| `verificationEntity` | entity |  |  |  | Verification before upload::Verification Entity | This entity is used to verify the file BEFORE it is uploaded to the server. The widget will create an object and add the defined attributes. After the 'beforeAccept Microflow/Nanoflow' returns an empty string value (this is the error message, empty when no error occurred), the file itself will be committed and uploaded. Note 1: Only non-persistent entities are accepted, as the widget will create/delete the object itself. Note 2: For this non-persistent entity, when applying Entity Access, give everyone permissions to create/delete this object, and read/write access on any attributes. Note 3: To avoid any other problem, make sure this helper object is detached from any other objects. It should be standalone and have zero references to others. |
| `verificationNameAttr` | attribute |  |  |  | Verification before upload::Verification Attributes | Attribute for the name of the file, usually 'Name'. |
| `verificationTypeAttr` | attribute |  |  |  | Verification before upload::Verification Attributes | Attribute for the filetype, it is not required, it can be used to filter data in the on accept microflow. Values are like 'image/jpeg'. |
| `verificationExtAttr` | attribute |  |  |  | Verification before upload::Verification Attributes | Attribute for the extension. Is is not required, but can be used to filter data. Values are like 'jpeg', 'png' etc. The widget will determine based on the fileType what the expected extension is. If unknown, it will set it to 'unknown'. |
| `verificationSizeAttr` | attribute |  |  |  | Verification before upload::Verification Attributes | Size of the file in bytes. |
| `verificationBeforeAcceptMicroflow` | microflow |  |  |  | Verification before upload::Verification Actions | Microflow that will be called BEFORE the file is uploaded. It will pass an object defined as Verification entity. The microflow should return a String which serves as a reason NOT to accept the file. If the microflow returns an empty string (either empty or ''), it considers the file to be accepted. |
| `verificationBeforeAcceptNanoflow` | nanoflow |  |  |  | Verification before upload::Verification Actions | Nanoflow that will be called BEFORE the file is uploaded. It will pass an object defined as Verification entity. The microflow should return a String which serves as a reason NOT to accept the file. If the microflow returns an empty string (either empty or ''), it considers the file to be accepted. |
| `eventsBeforeCommitMf` | microflow |  |  |  | Events::Before commit | Before committing any file, run this microflow. This can be used to verify the context object (needs to be committed before you can commit any file you associate with). It should return a boolean with True before it commits the files |
| `eventsBeforeCommitNf` | nanoflow |  |  |  | Events::Before commit | Before committing any file, run this nanoflow. This can be used to verify the context object (needs to be committed before you can commit any file you associate with). It should return a boolean with True before it commits the files |
| `eventsAfterCommitMf` | microflow |  |  |  | Events::After commit | After the file is succesfully committed, this microflow can be executed |
| `eventsAfterCommitNf` | nanoflow |  |  |  | Events::After commit | After the file is succesfully committed, this nanoflow can be executed |
| `uiDeleteButtonStyle` | enumeration | Yes | glyphicon | `glyphicon` \| `builtin` | UI::Delete Button | Choose the glyphicon (see next) or the built-in icon |
| `uiDeleteButtonGlyph` | string |  | remove-circle |  | UI::Delete Button | This is a standard glyphicon class. The prefix for glyphicons ("glyphicon glyphicon-") is automatically added to this. |
| `uiSaveButtonStyle` | enumeration | Yes | glyphicon | `glyphicon` \| `builtin` | UI::Save Button | Choose the glyphicon (see next) or the built-in icon |
| `uiSaveButtonGlyph` | string |  | upload |  | UI::Save Button | This is a standard glyphicon class. The prefix for glyphicons ("glyphicon glyphicon-") is automatically added to this. |
| `uiErrorButtonStyle` | enumeration | Yes | glyphicon | `glyphicon` \| `builtin` | UI::Error Button | Choose the glyphicon (see next) or the built-in icon |
| `uiErrorButtonGlyph` | string |  | warning-sign |  | UI::Error Button | This is a standard glyphicon class. The prefix for glyphicons ("glyphicon glyphicon-") is automatically added to this. |
| `uiShowPreview` | boolean | Yes | true |  | UI::Other | Show preview block on left side. |
| `uiShowPreviewLabel` | boolean | Yes | false |  | UI::Other | Show the type label in the previews |
| `uiShowImagePreviews` | boolean | Yes | true |  | UI::Other | Show previews of images. The widget will determine by itself, based on the Mime type, whether or not it can show a preview. It does this by analyzing the file and if it determines it is a 'image/*' file, show a base64 encoded version of the image. |
| `uiShowFileSize` | boolean | Yes | true |  | UI::Other | Show file size label |
| `uiHideProgressOnComplete` | boolean | Yes | true |  | UI::Progress Bar | When uploading to Mendix is complete, hide the progress bar |
| `uiPbColorStrokeNormal` | string | Yes | #2387AA |  | UI::Progress Bar | Default is '#2387AA' |
| `uiPbColorTrail` | string | Yes | #C8F5FF |  | UI::Progress Bar | Default is '#C8F5FF' |
| `uiPbColorError` | string | Yes | #FF0000 |  | UI::Progress Bar | Default is '#FF0000' |
| `uiPbColorSuccess` | string | Yes | #0A9B00 |  | UI::Progress Bar | Default is '#0A9B00' |
| `textDropZone` | translatableString |  |  |  | Texts | Description in the dropzone |
| `textDropZoneMaximum` | translatableString |  |  |  | Texts | Message when maximum is reached |
| `textDeleteFileConfirm` | translatableString |  |  |  | Texts | Confirmation message when deleting a file. If you leave this empty, confirmation is disabled. |
| `textFilesRejectedByServer` | translatableString |  |  |  | Texts | The %%FILENAME%% and %%ERROR%% are replaced by the widget |
| `textFilesRejected` | translatableString |  |  |  | Texts | Rejected files string |
| `textFileRejectedSize` | translatableString |  |  |  | Texts | String for when a file is rejected because of filesize. The %%FILENAME%% and %%MAXSIZE%% are replaced by the widget |

---

Regenerated by `mxcli widget docs` and by `refresh catalog`. For the same data live from the `.mpk` — including anything added by a widget upgrade since this file was written — run `mxcli widget describe filedropper -p <app.mpr>`.
