---
layout: default-layout
title: Dynamsoft Camera Enhancer - Guide on Android
description: This is the documentation - Guide on Android page of Dynamsoft Camera Enhancer.
keywords:  Camera Enhancer, Guide on Android
needAutoGenerateSidebar: true
noTitleIndex: true
needGenerateH3Content: true
breadcrumbText: Android Guide
---

# User Guide on Android (Java)

- System Requirements:
  - Supported OS: Android 5 or higher (Android 7 or higher recommended).
  - Supported ABI: arm64-v8a/armeabi-v7a/x86/x86_64.

- Environment: Android Studio 3.4+.

## Installation

If you don’t have SDK yet, please download the Dynamsoft Camera Enhancer(DCE) SDK from the <a href="https://www.dynamsoft.com/camera-enhancer/downloads/1000021-confirmation/?utm_source=docs" target="_blank">Dynamsoft website</a> and unzip the package. After decompression, the root directory of the DCE installation package is `DynamsoftCameraEnhancer`, which is represented by `[INSTALLATION FOLDER]`.

## Build Your First Application

The following sample will demonstrate how to acquire a frame from video streaming by DCE.
>Note:
>
>- The following steps are completed in Android Studio 4.2.
>- You can download the similar complete source code from [Here](https://github.com/Dynamsoft/camera-enhancer-mobile-samples/tree/main/android/HelloWorld).
>- For more samples on using Dynamsoft Camera Enhancer supporting Barcode Reader please [click here](https://github.com/Dynamsoft/barcode-reader-mobile-samples/tree/main/android/).

### Create a New Project

1. Open Android Studio and select New Project… in the File > New > New Project… menu to create a new project.

2. Choose the correct template for your project. In this sample, we’ll use `Empty Activity`.

3. When prompted, choose your app name (`HelloWorld`) and set the Save location, Language, and Minimum SDK (21)
    >Note: With minSdkVersion set to 21, your app is available on more than 94.1% of devices on the Google Play Store (last update: March 2021).

### Include the library

There are two ways to include the Dynamsoft Camera Enhancer SDK into your project：

#### Local Binary Dependency

1. Go to the **Libs** folder in the installation package, copy the file `DynamsoftCameraEnhancerAndroid.aar` and `DynamsoftCoreAndroid.aar` to the target directory `HelloWorld\app\libs`

2. Open the file `HelloWorld\app\build.gradle`, and add reference in the dependencies:

    ```groovy
    dependencies {
        implementation fileTree(dir: 'libs', include: ['*.aar'])
    }
    ```

3. Click `Sync Now`. After the synchronization completes, the SDK is added to the project.

4. import the package int the file `MainActivity.java`

    ```java
    import com.dynamsoft.dce.*;
    import com.dynamsoft.core.*;
    ```

#### Remote Binary Dependency

1. Open the file `HelloWorld\app\build.gradle`, and add the remote repository:

    ```groovy
    repositories {
        maven {
            url "https://download2.dynamsoft.com/maven/aar"
        }
    }
    ```

2. Add reference in the dependencies:

    ```groovy
    dependencies {
        implementation 'com.dynamsoft:dynamsoftcameraenhancer:{version-number}@aar'
        implementation 'com.dynamsoft:dynamsoftcore:{version-number}@aar'
    }
    ```

    >Note:Please replace {version-number} with the correct version number.

3. Click `Sync Now`. After the synchronization completes, the SDK is added to the project.

4. import the package in the file `MainActivity.java`

    ```java
    import com.dynamsoft.dce.*;
    ```

### Initialize Dynamsoft Camera Enhancer

1. Initialize the license

    ```java
    CameraEnhancer.initLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9", new DCELicenseVerificationListener() {
        @Override
        public void DCELicenseVerificationCallback(boolean isSuccess, Exception error) {
            if(!isSuccess){
                error.printStackTrace();
            }
        }
    });
    ```

    >Note:
    >
    >- Network connection is required for the license to work.
    >- "DLS2***" is a time-limited public trial license used in the sample.
    >- If the license has expired, please request a trial license through the <a href="https://www.dynamsoft.com/customer/license/trialLicense?utm_source=docs" target="_blank">customer portal</a>.

2. Create an instance of Dynamsoft Camera Enhancer

    ```java
    CameraEnhancer mCameraEnhancer;
    mCameraEnhancer = new CameraEnhancer(MainActivity.this);
    ```

### Create Camera View And Control Camera

1. In the Project window, open app > res > layout > `activity_main.xml`, create a DCE camera view section under the root node.

    ```xml
    <com.dynamsoft.dce.DCECameraView
        android:id="@+id/cameraView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:layout_editor_absoluteX="25dp"
        tools:layout_editor_absoluteY="0dp" />
    ```

2. Initialize the camera view, and bind to the Camera Enhancer object.

    ```java
    DCECameraView mCameraView;

    mCameraView = findViewById(R.id.cameraView);
    mCameraEnhancer.setCameraView(cameraView);
    ```

3. Override the MainActivity.onResume and MainActivity.onPause function to open and close camera.

    ```java
    @Override
    protected void onResume() {
        super.onResume();
        needCapture = false;
        try {
            mCameraEnhancer.open();
        } catch (CameraEnhancerException e) {
            e.printStackTrace();
        }
    }

    @Override
    protected void onPause() {
        super.onPause();
        try {
            mCameraEnhancer.close();
        } catch (CameraEnhancerException e) {
            e.printStackTrace();
        }
    }
    ```

### Capture Frame

1. In the Project window, open app > res > layout > `activity_main.xml`, and add a `Button` to capture frame.

    ```xml
    <Button
        android:id="@+id/btn_capture"
        android:layout_width="70dp"
        android:layout_height="70dp"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="25dp"
        android:background="@drawable/icon_capture"/>
    ```

2. Add UI variables and event response codes

    ```java
    // Click to capture a frame
    private Button btnCapture;

    btnCapture =(Button)findViewById(R.id.btnCapture);

    btnCapture.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            // Here we just set a flag, the actual capture action will be executed in the `frameOutputCallback`
            needCapture = true;
        }
    });

    ```

3. Add a frame listener to acquire the latest frame from video streaming. Here we capture a frame and show it on the other activity(`ShowPictureActivity`).

    ```java
    static DCEFrame mFrame;

    mCameraEnhancer.addListener(new DCEFrameListener() {
        @Override
        public void frameOutputCallback(DCEFrame frame, long nowTime) {
            if(needCapture){
                needCapture = false;
                mFrame = frame;
                Intent intent = new Intent(MainActivity.this,ShowPictureActivity.class);
                startActivity(intent);
            }
        }
    });
    ```

### Additional Steps

1. In the Project window, open app > res > layout > `activity_show_picture.xml`, and add a `ImageView`.

    ```xml
    <ImageView
        android:id="@+id/iv_picture"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        />
    ```

2. Display the captured frame in ImageView.

    ```java
    public class ShowPictureActivity extends AppCompatActivity {
        ImageView ivPicture;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_show_picture);
            ivPicture = findViewById(R.id.iv_picture);

            DCEFrame frame = MainActivity.mFrame;

            // Convert to Bitmap from the captured frame.
            Bitmap bitmap = frame.toBitmap();

            // Rotate it to the nature device orientation.
            bitmap = rotateBitmap(bitmap, frame.getOrientation());

            // Display it in ImageView
            ivPicture.setImageBitmap(bitmap);
        }

        private Bitmap rotateBitmap(Bitmap origin, float degree) {
            if (origin == null) {
                return null;
            }
            int width = origin.getWidth();
            int height = origin.getHeight();
            Matrix matrix = new Matrix();
            matrix.setRotate(degree);
            Bitmap newBM = Bitmap.createBitmap(origin, 0, 0, width, height, matrix, false);
            origin.recycle();
            return newBM;
        }
    }
    ```

### Build and Run the Project

1. Select the device that you want to run your app on from the target device drop-down menu in the toolbar.

2. Click `Run app` button, then Android Studio installs your app on your connected device and starts it.

>Note:
>- You can download the similar complete source code from [Here](https://github.com/Dynamsoft/camera-enhancer-mobile-samples/tree/main/android/HelloWorld).
>- For more samples on using Dynamsoft Camera Enhancer supporting Barcode Reader please [click here](https://github.com/Dynamsoft/barcode-reader-mobile-samples/tree/main/android/).

## What's next?

### How to integration with barcode reader

<a href="https://www.dynamsoft.com/barcode-reader/programming/android/user-guide.html?utm_source=docs" target="_blank">This article</a> guides you to integrate barcode reader function into your app.
