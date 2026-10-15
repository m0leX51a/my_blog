# Diving into android penetration testing Talk (Anas Eladly)

# Android pentesting methods

- **Static analysis**
    
    involves examining the source code of an app without executing it. This method looks for security vulnerabilities that can be identified by inspecting code.
    

- **Dynamic Analysis**
    
    by running the application and observing its behavior to uncover security vulnerabilities that appear during its operation.
    

# Android Application Structure

## Android package kit (APK)

 It  is the encapsulation of the app's code,
resources, assets, and manifest file.

![image.png](Diving%20into%20android%20penetration%20testing%20Talk%20(Anas/image.png)

lip : have the libraries to deal with hardware 

res : Stores structured app resources like images, layouts, strings, and styles.

assets : Stores raw, unstructured files (e.g., fonts, JSON, HTML, videos).

class.dex : have the source code  of the application at `dex` format

androidManifest: mapping all application components 

META_INF : have all trusted ssl 

resources: have chunks of source code 

### Getting apk files

we can use “apktool”

```bash
apktool d apk_name
```

### Getting source code

decode apk and attempt to get source code through the smali code 

![image.png](Diving%20into%20android%20penetration%20testing%20Talk%20(Anas/image%201.png)

### Analyze Manifest.xml

![image.png](Diving%20into%20android%20penetration%20testing%20Talk%20(Anas/image%202.png)

by analyze the manifest code we can observe and collect information about application such 

- `compileSdkVersion` when we get this we can search for some attacks can done specific on this version
- uses-permisssions the permissions the application need from android
- `debuggable="true"`