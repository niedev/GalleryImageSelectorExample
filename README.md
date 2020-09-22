# UserImageExample
This repository contains an open source sample application that implements a user image selector from an extern gallery, the sample is based on the <a href="https://github.com/niedev/RTranslator/blob/master/app/src/main/java/nie/translator/rtranslatordevedition/settings/UserImageContainer.java" target="_blank" rel="noopener noreferrer">UserImageContainer</a> class of <a href="https://github.com/niedev/RTranslator" target="_blank" rel="noopener noreferrer">RTranslator</a>.
The UserImageContainer is highly optimized (fixes a lot of bugs including the rotation bug) and work with a variety of different galleries, even when combined
(you can use a gallery for pick and another for crop without problems)
if you have problem with a specific gallery app please report it in Issues (I cannot guarantee that I will be able to fix the problem but I can try if I have time).

The passages for make this example (and for use UserImageContainer in other project) from a new project with an empty activity are:

- add an xml icon (user_icon.xml) to drawable
- add the following permission to the manifest: 
  ```
  <uses-permissionandroid:name="android.permission.READ_EXTERNAL_STORAGE"/>
  ```
- override onActivityResult in the activity that was passed or in the fragment if it is not null and here call userImageContainer.onActivityResult passing the arguments
of the onActivityResult overwritten and true or false respectively if you want to save the selected image or not (you can save it later with saveContent())
- add this to the manifest:
  ```
  <provider
	  android:name="androidx.core.content.FileProvider"
	  android:authorities="nie.translator.userimage.fileprovider"
	  android:exported="false"
	  android:grantUriPermissions="true">
	  <meta-data
		  android:name="android.support.FILE_PROVIDER_PATHS"
		  android:resource="@xml/filepaths"/>
  </provider>
  ```
- create the "filepaths.xml" file in the "xml" folder with the following content:
  ```
  <paths>
	  <cache-pathpath="temporary_images/"name="temporaryUserImage"/>
  </paths>
  ```


For saving the image you can mark the last parameter of userImageContainer.onActivityResult with true, in this case the image will be saved automatically at the end of the crop, 
if you want instead to save the image in another moment (maybe after an error check for other data) you can mark the save parameter of onActivityResult false and use
userImageContainer.saveContent() and UserImageContainer will save the last cropped image.
The saved image can be accessed with "new File(context.getFilesDir(), "user_image");" in this case context must be an Activity or an Application or a Service.

In this case I have inserted the imageView in an activity but to make UserImageContainer work with an imageView contained in a fragment just insert the fragment 
between the arguments and instead of overriding onActivityResult in the activity we will have to do it in the fragment, for an example of this see directlty the
implementation in RTranslator 

Notes: onActivityResult overwritten will be called two times, one after selection and one after the crop, if the save parameter is true the image will be saved only after
the crop, not the first time.
