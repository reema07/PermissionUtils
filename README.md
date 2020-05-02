# FileProvider
File Provider Error 
i.e. FileUriExposedException: file:///storage/emulated/0/image01.jpg exposed beyond app through ClipData.Item.getUri()


If targetSdkVersion is higher than 24, then FileProvider is used to grant access.

Create an xml file(Path: res\xml) provider_paths.xml

    <?xml version="1.0" encoding="utf-8"?>
    <paths xmlns:android="http://schemas.android.com/apk/res/android">
    <external-path name="external_files" path="."/>
    </paths>


Add a Provider in AndroidManifest.xml

    <provider
        android:name="android.support.v4.content.FileProvider"
        android:authorities="${applicationId}.provider"
        android:exported="false"
        android:grantUriPermissions="true">
        <meta-data
            android:name="android.support.FILE_PROVIDER_PATHS"
            android:resource="@xml/provider_paths"/>
    </provider>
If you are using androidx, the FileProvider path should be:

    android:name="androidx.core.content.FileProvider"
and replace

    Uri uri = Uri.fromFile(fileImagePath);
to

    Uri uri = FileProvider.getUriForFile(MainActivity.this, BuildConfig.APPLICATION_ID + ".provider",fileImagePath);
    Edit: While you're including the URI with an Intent make sure to add below line:

    intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
and you are good to go. Hope it helps.
