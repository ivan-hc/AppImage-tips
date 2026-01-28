## Intro
This is a guide for children aged 6 and up.

-----------------------------------------------------------

## Index
- [AppDir](#appdir)
  - [Structure of the AppDir](#structure-of-the-appdir)
 
- [What to use](#what-to-use)
  - [How to use appimagetool](#how-to-use-appimagetool)
    - [Simple use](#simple-use)
    - [How to add delta updates](#how-to-add-delta-updates)

- [How to convert existing AppImages](#how-to-convert-existing-AppImages)

-----------------------------------------------------------

## AppDir
An AppDir is the standard directory layout used to bundle an application and all its required files (binaries, libraries, resources) in one self-contained folder.

You can give it whatever name you want, but in our examples below here we will always use "AppDir".

## Structure of the AppDir
To be valid, a AppDir must contain at least these three essential components in the root directory:
- a valid .desktop file
- an icon (PNG or SVG)
- an AppRun

Depending on the complexity of the program you want to launch, you must include all the files and directories it needs to run.

For example, if you extract multiple .deb packages into a single AppDir, you need the three elements listed above to convert it into an AppImage.

In the screenshots below, some examples of AppDirs

| Program name | Content (you can use the option `--appimage-extract` on every type2+ AppImage)
| - | - 
| firefox | ![Istantanea_2026-01-28_19-08-03 png](https://github.com/user-attachments/assets/523fc78c-7429-465e-acee-b107450c63ed)
| bottles | ![Istantanea_2026-01-28_19-08-29 png](https://github.com/user-attachments/assets/ab6209b4-90fe-42aa-adc0-43ec9a3ec19b)
| converseen | ![Istantanea_2026-01-28_19-10-05 png](https://github.com/user-attachments/assets/385c4526-fdfb-40c3-8ede-b3ab56b356d2)
| kdenlive | ![Istantanea_2026-01-28_19-10-29 png](https://github.com/user-attachments/assets/2f910505-e0cb-430e-9e95-1d691f5b7b46)
| simple-scan (gtk3) | ![Istantanea_2026-01-28_19-11-15 png](https://github.com/user-attachments/assets/51a83081-352f-4cd0-83f0-93058bb389dd)

-----------------------------------------------------------

## What to use
Use `appimagetool`, the official one is https://github.com/AppImage/appimagetool

Alternatively, you can use this fork https://github.com/pkgforge-dev/appimagetool-uruntime 

## How to use appimagetool
Given the "AppDir", you need to set the architecture (ARCH).

## Simple use
Run the program like this
```
ARCH=x86_64 ./appimagetool AppDir
```

You can also specify the name you want to give to your AppImage package, like this
```
APPNAME="SAMPLE"
VERSION="1.2.3"
ARCH=x86_64 ./appimagetool AppDir "$APPNAME"_"$VERSION"-x86_64.AppImage
```

## Advanced use - How to add delta updates
You can also add delta updates info (option -u), to made updates quick (using `zsync` or even better `appimageupdatetool`).

Let suppose we want add update info for this repository, ivan-hc/AppImage-tips:
```
APPNAME="SAMPLE"
VERSION="1.2.3"
GITHUB_REPOSITORY_OWNER="ivan-hc"
REPO="AppImage-tips"
TAG="latest"
UPINFO="gh-releases-zsync|$GITHUB_REPOSITORY_OWNER|$REPO|$TAG|*x86_64.AppImage.zsync"
ARCH=x86_64 ./appimagetool -u "$UPINFO" .AppDir "$APPNAME"_"$VERSION"-x86_64.AppImage
```

NOTE, "$GITHUB_REPOSITORY_OWNER" is already set in Github Actions.

-----------------------------------------------------------

## How to convert existing AppImages
If your old AppImage supports the option `--appimage-extract`, run it with this option to obtain the extracted AppDir (usually its name is "squashfs-root").

Use `appimagetool` on that directory to obtain a new generation AppImage that runs without `libfuse2`.

-----------------------------------------------------------

| [Go back to Index](#Index) |
| - |
