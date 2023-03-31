# 如何在 android 上从相机和图库中拍摄照片？

> 原文：<https://medium.com/analytics-vidhya/how-to-take-photos-from-the-camera-and-gallery-on-android-87afe11dfe41?source=collection_archive---------1----------------------->

对于一个 Android 新手来说，为了进行测试，你需要一个简单明了的指南。正如您所发现的，有许多关于这个主题的文章、网站和助手。然而，它们中的很大一部分并没有很好地工作，或者没有尝试过，或者太复杂以至于无法思考理解。让我带你通过一个最熟练的方法来执行这个没有任何问题的指南。

你可以从 [Github](https://github.com/ayushgemini/android-choose-photo) 下载完整的源代码。

![](img/ee6f4e12a3100cda0d391250d3e190f7.png)

首先，你必须在你的布局中实现一个`ImageView`来捕捉你通过相机或图库上传的图像。

下面是我为上述目的实现的`ImageView`。

```
<ImageView
        android:layout_width="match_parent"
        android:id="@+id/my_avatar_imageview"
        android:layout_height="wrap_content"
        android:adjustViewBounds="true"
         />
```

现在，这样做之后，您的 activity_main.xml 将如下所示。

activity_main.xml

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
   <ImageView
        android:layout_width="match_parent"
        android:id="@+id/my_avatar_imageview"
        android:layout_height="wrap_content"
        android:adjustViewBounds="true"
         />
</RelativeLayout>
```

现在在 MainActivity 类中，您必须首先声明 ImageView。

```
private ImageView imageView;
```

在 onCreate override 方法中，您必须通过添加以下内容来初始化 ImageView 元素，

```
imageView = findViewById(R.id.my_avatar_imageview);
```

# 相机和外部存储器的运行时许可

如你所知，如果你使用的是棉花糖或更高版本的设备，那么你需要获得首次运行许可。对于我们的应用程序，我们将需要两个运行时权限。

1.  相机许可
2.  外部存储权限

让我们来看看你将如何获得这个许可

首先，您需要在清单文件 ie 中定义这两个权限。AndroidManifest.xml

```
<uses-permission android:name="android.permission.CAMERA"/>
 <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```

现在，您的 AndroidManifest.xml 将如下所示:

AndroidManifest.xml

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.geminisoftservices.choosephoto"> <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/> <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" /> <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application></manifest>
```

好了，在清单文件上添加了这两个权限之后，您需要创建一个函数来检查 MainActivity 上的运行时权限。如果用户没有权限，此功能将显示一个弹出窗口，以便用户可以单击“允许”按钮授予您的应用程序权限。

```
// function to check permission public static boolean checkAndRequestPermissions(final Activity context) {
        int WExtstorePermission = ContextCompat.checkSelfPermission(context,
                Manifest.permission.WRITE_EXTERNAL_STORAGE);
        int cameraPermission = ContextCompat.checkSelfPermission(context,
                Manifest.permission.CAMERA);
        List<String> listPermissionsNeeded = new ArrayList<>();
        if (cameraPermission != PackageManager.PERMISSION_GRANTED) {
            listPermissionsNeeded.add(Manifest.permission.CAMERA);
        }
        if (WExtstorePermission != PackageManager.PERMISSION_GRANTED) {
            listPermissionsNeeded
                    .add(Manifest.permission.WRITE_EXTERNAL_STORAGE);
        }
        if (!listPermissionsNeeded.isEmpty()) {
            ActivityCompat.requestPermissions(context, listPermissionsNeeded
                            .toArray(new String[listPermissionsNeeded.size()]),
                    REQUEST_ID_MULTIPLE_PERMISSIONS);
            return false;
        }
        return true;
    }
```

现在您需要重写 onRequestPermissionsResult，在那里您将处理您的权限结果。如果许可被授予，那么您可以继续调用 chooseImage 函数。

```
// Handled permission Result
 @Override
    public void onRequestPermissionsResult(int requestCode,String[] permissions, int[] grantResults) {
        switch (requestCode) {
            case REQUEST_ID_MULTIPLE_PERMISSIONS:
                if (ContextCompat.checkSelfPermission(MainActivity.this,
                        Manifest.permission.CAMERA) != PackageManager.PERMISSION_GRANTED) {
                    Toast.makeText(getApplicationContext(),
                            "FlagUp Requires Access to Camara.", Toast.LENGTH_SHORT)
                            .show(); } else if (ContextCompat.checkSelfPermission(MainActivity.this,
                        Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
                    Toast.makeText(getApplicationContext(),
                            "FlagUp Requires Access to Your Storage.",
                            Toast.LENGTH_SHORT).show(); } else {
                    chooseImage(MainActivity.this);
                }
                break;
        }
    }
```

# 从相机和图库中选择图像

现在让我们看看 chooseImage 函数，它将显示一个包含三个选项对话框

1.  拍照
2.  从照片库中选择一张照片
3.  出口

```
// function to let's the user to choose image from camera or gallery private void chooseImage(Context context){ final CharSequence[] optionsMenu = {"Take Photo", "Choose from Gallery", "Exit" }; // create a menuOption Array // create a dialog for showing the optionsMenu AlertDialog.Builder builder = new AlertDialog.Builder(context); // set the items in builder builder.setItems(optionsMenu, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) { if(optionsMenu[i].equals("Take Photo")){ // Open the camera and get the photo Intent takePicture = new Intent(android.provider.MediaStore.ACTION_IMAGE_CAPTURE);
                    startActivityForResult(takePicture, 0);
                }
                else if(optionsMenu[i].equals("Choose from Gallery")){ // choose from  external storage Intent pickPhoto = new Intent(Intent.ACTION_PICK, android.provider.MediaStore.Images.Media.EXTERNAL_CONTENT_URI);
                    startActivityForResult(pickPhoto , 1); }
                else if (optionsMenu[i].equals("Exit")) {
                    dialogInterface.dismiss();
                } }
        });
        builder.show();
    }
```

现在，为了处理 startActivityForResult 的结果，您需要覆盖 onActivityResult

```
@Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode != RESULT_CANCELED) {
            switch (requestCode) {
                case 0:
                    if (resultCode == RESULT_OK && data != null) {
                        Bitmap selectedImage = (Bitmap) data.getExtras().get("data");
                        imageView.setImageBitmap(selectedImage);
                    }
                    break;
                case 1:
                    if (resultCode == RESULT_OK && data != null) {
                        Uri selectedImage = data.getData();
                        String[] filePathColumn = {MediaStore.Images.Media.DATA};
                        if (selectedImage != null) {
                            Cursor cursor = getContentResolver().query(selectedImage, filePathColumn, null, null, null);
                            if (cursor != null) {
                                cursor.moveToFirst(); int columnIndex = cursor.getColumnIndex(filePathColumn[0]);
                                String picturePath = cursor.getString(columnIndex);
                                imageView.setImageBitmap(BitmapFactory.decodeFile(picturePath));
                                cursor.close();
                            }
                        } }
                    break;
            }
        }
    }
```

完成所有步骤后，就可以调用 onCreate 方法中的权限检查函数。

```
if(checkAndRequestPermissions(MainActivity.this)){
            chooseImage(MainActivity.this);
        }
```

所以在做了所有的事情之后，你的 MainActivity.java 看起来会像这样

MainActivity.java

```
package com.geminisoftservices.choosephoto;import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;import android.Manifest;
import android.app.Activity;
import android.content.Context;
import android.content.DialogInterface;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.net.Uri;
import android.os.Bundle;
import android.provider.MediaStore;
import android.widget.ImageView;
import android.widget.Toast;import java.util.ArrayList;
import java.util.List;public class MainActivity extends AppCompatActivity { // Declare the ImageView first. private ImageView imageView; public static final int REQUEST_ID_MULTIPLE_PERMISSIONS = 101; @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main); // Initialize the imageView (Link the imageView with front-end component ImageView) imageView = findViewById(R.id.my_avatar_imageview); if(checkAndRequestPermissions(MainActivity.this)){
            chooseImage(MainActivity.this);
        } } // function to let's the user to choose image from camera or gallery private void chooseImage(Context context){ final CharSequence[] optionsMenu = {"Take Photo", "Choose from Gallery", "Exit" }; // create a menuOption Array // create a dialog for showing the optionsMenu AlertDialog.Builder builder = new AlertDialog.Builder(context); // set the items in builder builder.setItems(optionsMenu, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) { if(optionsMenu[i].equals("Take Photo")){ // Open the camera and get the photo Intent takePicture = new Intent(android.provider.MediaStore.ACTION_IMAGE_CAPTURE);
                    startActivityForResult(takePicture, 0);
                }
                else if(optionsMenu[i].equals("Choose from Gallery")){ // choose from  external storage Intent pickPhoto = new Intent(Intent.ACTION_PICK, android.provider.MediaStore.Images.Media.EXTERNAL_CONTENT_URI);
                    startActivityForResult(pickPhoto , 1); }
                else if (optionsMenu[i].equals("Exit")) {
                    dialogInterface.dismiss();
                } }
        });
        builder.show();
    } // function to check permission public static boolean checkAndRequestPermissions(final Activity context) {
        int WExtstorePermission = ContextCompat.checkSelfPermission(context,
                Manifest.permission.WRITE_EXTERNAL_STORAGE);
        int cameraPermission = ContextCompat.checkSelfPermission(context,
                Manifest.permission.CAMERA);
        List<String> listPermissionsNeeded = new ArrayList<>();
        if (cameraPermission != PackageManager.PERMISSION_GRANTED) {
            listPermissionsNeeded.add(Manifest.permission.CAMERA);
        }
        if (WExtstorePermission != PackageManager.PERMISSION_GRANTED) {
            listPermissionsNeeded
                    .add(Manifest.permission.WRITE_EXTERNAL_STORAGE);
        }
        if (!listPermissionsNeeded.isEmpty()) {
            ActivityCompat.requestPermissions(context, listPermissionsNeeded
                            .toArray(new String[listPermissionsNeeded.size()]),
                    REQUEST_ID_MULTIPLE_PERMISSIONS);
            return false;
        }
        return true;
    } // Handled permission Result @Override
    public void onRequestPermissionsResult(int requestCode,String[] permissions, int[] grantResults) {
        switch (requestCode) {
            case REQUEST_ID_MULTIPLE_PERMISSIONS:
                if (ContextCompat.checkSelfPermission(MainActivity.this,
                        Manifest.permission.CAMERA) != PackageManager.PERMISSION_GRANTED) {
                    Toast.makeText(getApplicationContext(),
                            "FlagUp Requires Access to Camara.", Toast.LENGTH_SHORT)
                            .show(); } else if (ContextCompat.checkSelfPermission(MainActivity.this,
                        Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
                    Toast.makeText(getApplicationContext(),
                            "FlagUp Requires Access to Your Storage.",
                            Toast.LENGTH_SHORT).show(); } else {
                    chooseImage(MainActivity.this);
                }
                break;
        }
    } @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode != RESULT_CANCELED) {
            switch (requestCode) {
                case 0:
                    if (resultCode == RESULT_OK && data != null) {
                        Bitmap selectedImage = (Bitmap) data.getExtras().get("data");
                        imageView.setImageBitmap(selectedImage);
                    }
                    break;
                case 1:
                    if (resultCode == RESULT_OK && data != null) {
                        Uri selectedImage = data.getData();
                        String[] filePathColumn = {MediaStore.Images.Media.DATA};
                        if (selectedImage != null) {
                            Cursor cursor = getContentResolver().query(selectedImage, filePathColumn, null, null, null);
                            if (cursor != null) {
                                cursor.moveToFirst(); int columnIndex = cursor.getColumnIndex(filePathColumn[0]);
                                String picturePath = cursor.getString(columnIndex);
                                imageView.setImageBitmap(BitmapFactory.decodeFile(picturePath));
                                cursor.close();
                            }
                        } }
                    break;
            }
        }
    } }
```

就这样，现在你可以运行你的应用程序，享受代码。

我希望你喜欢这个教程，并且你已经学会了如何从图库中获取图片或者在 Android 上从相机中捕捉图片。

快乐编码。

下一个故事再见。敬请期待！

**可以在**[**LinkedIn**](https://www.linkedin.com/in/ayush-gemini/)**&**[**Github**](https://github.com/ayushgemini)**上联系我。**