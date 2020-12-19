# Camera App

[<img src="https://github.com/angular/angular/blob/master/aio/src/assets/images/logos/angular/angular.png" width="31" height="31"></img>](https://play.nativescript.org/?template=play-ng&id=VD8YSd&v=228) Using the Camera to Take a Picture

![image](https://raw.githubusercontent.com/NativeScript/code-samples/master/screens/basic-camera-ios.gif)

Example taken from [:bookmark:`nativescript.rocks`](https://plugins.nativescript.rocks/samples) and can be used as a template since it has already been converted to [NativeScript 7](https://nativescript.org/blog/nativescript-7-announcement)

## :o: Create a project by using this template

```
$ ns create nsCameraApp --template https://github.com/CraveFM/nsCameraApp
```


## :m: From Scratch

* Create a blank NativeScript/Angular/sass project

```
% ns create nsMusicPlayerUI --template @nativescript/template-blank-ng
```

## :gear: Configure

##### :bangbang: Installing Dependencies 

```
$ ns plugin add @nativescript/camera
```

## :a: Home Component

:round_pushpin: in the HomeComponent `Class`

- [ ] Add some instance variables that will be used later on

```typescript
    public saveToGallery: boolean = true;
    public cameraImage: ImageAsset;

```

- [ ] Add the main `capture()` method that takes pictures.  `takePicture()`

```typescript
    capture() {
        takePicture({ width: 250, height: 300, keepAspectRatio: true, saveToGallery: this.saveToGallery })
            .then((imageAsset: any) => {
                this.cameraImage = imageAsset;
                imageAsset.getImageAsync(function (nativeImage) {
                    let scale = 1;
                    let height = 0;
                    let width = 0;
                    if (imageAsset.android) {
                        // get the current density of the screen (dpi) and divide it by the default one to get the scale
                        scale = nativeImage.getDensity() / imageAsset.android.util.DisplayMetrics.DENSITY_DEFAULT;
                        height = imageAsset.options.height;
                        width = imageAsset.options.width;
                    } else {
                        scale = nativeImage.scale;
                        width = nativeImage.size.width * scale;
                        height = nativeImage.size.height * scale;
                    }
                    console.log(`Displayed Size: ${width}x${height} with scale ${scale}`);
                    console.log(`Image Size: ${width / scale}x${height / scale}`);
                });
            }, (error) => {
                console.log("Error: " + error);
            });
    }
```

- [ ] Let's react when the `Take Picture` button is pressed by asking `Permissions` first

```typescript
    onTakePictureTap(args: EventData) {
        requestPermissions().then(
            () => this.capture(),
            () => alert('permissions rejected')
        );
    }

```

:round_pushpin: Styles

- [ ] Add the `styleUrls` operator to the `@Component` decorator

```typescript
@Component({
    selector: "Home",
    templateUrl: "./home.component.html",
    styleUrls: ["./home.component.css"]
})
```

- [ ] In the `HomeComponent` stylesheet add the following `Image` body

```css
Image {
    border-width: 10;
    border-color: red;
}
```

:round_pushpin: Template

- [ ] Let finish with the XML template

* replace the current `<ActionBar>`

```xml
<ActionBar class="action-bar">
    <Label text="Camera"></Label>
</ActionBar>
```

* replace the current `<GridLayout>` with a more sophisticated one

```xml
<GridLayout rows="auto, *, auto">
	<StackLayout orientation="horizontal" row="0" padding="10">
		<Label text="saveToGallery"></Label>
		<Switch [(ngModel)]="saveToGallery"></Switch>
	</StackLayout>
	<Image row="1" [src]="cameraImage" stretch="fill" margin="10"></Image>
	<Button text="Take Picture" (tap)="onTakePictureTap($event)" row="2" padding="10"></Button>
</GridLayout>
```

## :o: Customization

:iphone: Android

Open up the manifest file `AndroidManifest.xml` (in `App_Resource/Android/src/main`) and add the following to the `<application>` parameter tag:

```xml
	<application
		...
		android:requestLegacyExternalStorage="true">
```

# References

https://stackoverflow.com/questions/37819550/java-io-filenotfoundexception-storage-emulated-0-new-file-txt-open-failed-ea
