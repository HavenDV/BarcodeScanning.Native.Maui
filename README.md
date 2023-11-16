# BarcodeScanning.Native.Maui
Barcode scanning library based on native platform APIs for barcode detection: 
1. [Google ML Kit](https://developers.google.com/ml-kit)
2. [Apple Vision framework](https://developer.apple.com/documentation/vision)

This library was inspired by existing MAUI barcode scanning libraries: [BarcodeScanner.Mobile](https://github.com/JimmyPun610/BarcodeScanner.Mobile) & [Zxing.Net.MAUI](https://github.com/Redth/ZXing.Net.Maui), but comes with many code improvements and uses native ML APIs on both Android and iOS.

## Key features
1. Uses native APIs for maximal performance and barcode readability,
2. Optimized for continuous scanning,
3. Ability to scan multiple barcodes in one frame,
4. Ability to pool multiple scans for better scanning consistency,
5. Transformed barcode bounding box for on-screen positioning,
6. Code-behind and MVVM compatibility,
7. Android ony - Abillity to invert source image to scan natively unsupported inverted barcodes, but at a performance cost.

## Usage
1. Install [Nuget package](https://www.nuget.org/packages/BarcodeScanning.Native.Maui),
2. Initialize the plugin in your `MauiProgram.cs`:
    ```csharp
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            ...
            .UseBarcodeScanning();
            ...

        return builder.Build();
    }
    ```
3. Add required permissions:
    #### Android
    Edit `AndroidManifest.xml` file (under the Platforms\Android folder) and add the following permissions inside of the `manifest` node:
    ```xml
    <uses-permission android:name="android.permission.CAMERA" />
    ```
    #### iOS
    Edit `info.plist` file (under the Platforms\iOS folder) and add the following permissions inside of the `dict` node:
    ```xml
    <key>NSCameraUsageDescription</key>
    <string>Enable camera for barcode scanning.</string>
    ```
4. In XAML, add correct namespace, for example:
    ```xaml
    xmlns:scanner="clr-namespace:BarcodeScanning;assembly=BarcodeScanning.Native.Maui"
    ```
5. Set the `CameraEnabled` property to `true` in XAML, code behind or ViewModel to start the camera.
6. Listen to `OnDetectionFinished` event in Code-behind or bind `OnDetectionFinishedCommand` property to a Command in your ViewModel.
7. As a best practice set the `CameraEnabled` property to `false` in `OnDisappearing()` method override in your Page.

An example can be found in [BarcodeScanning.Test](https://github.com/afriscic/BarcodeScanning.Native.Maui/tree/master/BarcodeScanning.Test) project.

## Supported barcode symbologies
#### Android
1D: Codabar, Code 39, Code 93, Code 128, EAN-8, EAN-13, ITF, UPC-A, UPC-E; 2D: Aztec, Data Matrix, PDF417, QR Code
#### iOS
1D: Codabar, Code 39, Code 93, Code 128, EAN-8, EAN-13, GS1 DataBar, ITF, UPC-E; 2D: Aztec, Data Matrix, MicroPDF417, MicroQR, PDF417, QR Code

## Bindable properties
A list of bindable properties with descriptions can be found in [ICameraView.cs](https://github.com/afriscic/BarcodeScanning.Native.Maui/blob/master/BarcodeScanning.Native.Maui/ICameraView.cs) interface source file.

## TODO Windows and macOS support
Windows and macOS are currently unsupported, but support can be added in the future. Vision framework is compatible with macOS so this implementation wouldn't be difficult. For Windows, barcode detection could be supported through [Zxing.Net](https://github.com/micjahn/ZXing.Net) project.
