# Workshop instruction

## 1. Interior 3D modeling

### 1.1 Open up
Open Sweet Home 3D and following the steps as follows.

### 1.2 Create a Room
From the toolbar on the top, click the “Create room” button to create the floor of the room you want to create. 
Then, click the “Create wall” button and build walls around the floor.

### 1.3 Set textures for the walls, floor, and ceilings
Double click on the floor -> check "Display Ceilings" -> choose "Texture" for the radio buttons of floor, ceilings, and walls. Click the square box for choosing the texture you want.

### 1.4 Design your room
Choose different furnitures, doors, and windows you want from the left menu and drag it to your room. Double click the items and choose the texture or color you want. 

### 1.5 Extra sources
For searching texture online, you can use the ones suggested by Sweet Home 3D (http://www.sweethome3d.com/importTextures.jsp). Alternatively, you could search the type of texture on Google with the word “seamless”. For example, if you want to search for leather, you could input “leather seamless” in the search bar. Note that you should look for texture images in square (1:1 aspect ratio).

For extra furniture models, Sweet Home 3D also suggested some sources in http://www.sweethome3d.com/zh-tw/importModels.jsp. Besides, you could search free 3D interior models which is in the fomat of .OBJ/.KMZ/.3DS/.DAE. Download the models and import them to Sweet Home 3D by clicking Furnitures -> Choose Models.

### 1.6 Export models
"3D view" -> "Export to OBJ format", which will export everything to a folder.

## 2. WebVR application development

### 2.1 Create a GitHub repository

We will use GitHub for hosting the web page we're going to create, such we will have a link for viewing that page and sharing it to friends.

Click the **+** button at the top-right corner to create a new repository. Give it a name (e.g. `vr-room`), check the box labeled "Initialize this repository with a README", click "Create repository".

In the repository page, select the "settings" tab. Scroll down and find the "GitHub Pages" section. In the "Source" dropdown menu, select "master branch" and click "Save". The "GitHub Pages" section should now display something like `Your site is ready to be published at https://andyli.github.io/vr-room/.`.

### 2.2 Create a Cloud9 workspace

[Create a new Cloud9 workspace](https://c9.io/new). Make sure:

 * In the "Team" dropdown menu, select "Don't set a team for this workspace". It is because if you choose our "VR Workshop" team, the workspace could be automatically removed when we terminate our education plan. You should keep your work :)

 * In "Clone from Git or Mercurial URL", enter the GitHub repository url you've just created (e.g. `https://github.com/andyli/vr-room`).
 
 * Choose the HTML5 template.

### 2.3 Create a WebVR web page

Open the workspace you have just created. In the top menu, select "File" -> "New From Template" -> "HTML file". Save it as "index.html" by selecting "File" -> "Save".

Within `<head>...</head>`, add `<script src="https://aframe.io/releases/0.5.0/aframe.min.js"></script>`.

Within `<body>...</body>`, add the code as follows:
```html
<a-scene>
    <a-sphere position="0 1.25 -5" radius="1.25" color="#EF2D5E"></a-sphere>
    <a-box position="-1 0.5 -3" rotation="0 45 0" width="1" height="1" depth="1" color="#4CC3D9"></a-box>
    <a-cylinder position="1 0.75 -3" radius="0.5" height="1.5" color="#FFC65D"></a-cylinder>
    <a-plane position="0 0 -4" rotation="-90 0 0" width="4" height="4" color="#7BC8A4"></a-plane>
    <a-sky color="#ECECEC"></a-sky>
</a-scene>
```

You can now preview the web page by selecting "Preview" -> "Live Preview File (index.html)". You may use the [A-Frame Inspector](https://aframe.io/docs/0.5.0/guides/using-the-aframe-inspector.html) (<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>i</kbd>) to navigate and play with different settings.

### 2.4 Convert the interior model

First of all, convert the .obj file exported from Sweet Home 3D to another format (.dae), because A-Frame does not fully support the .obj file exported by Sweet Home 3D. To do so, use a command line program, [assimp](http://www.assimp.org/).

**Windows users:**

Download our precompiled .exe file at [assimp_3.3.1+d402825_win32.zip](https://github.com/TCLResearchHK/VRWorkshopCityU/raw/master/assimp_3.3.1%2Bd402825_win32.zip), extract it next to the .obj file. Open a command prompt window by entering `cmd` in the file explorer location bar. Input the command as follows:
``` cmd
assimp.exe export interior.obj interior.dae --config=full
```
Remember to replace `interior.obj` with the file name of your model.

**Mac users:**

Open "Terminal" (in "Applications" -> "Utilities"). Install [homebrew](https://brew.sh/), then `brew install assimp --HEAD` (assimp 3.3.1 is buggy, so we need `--HEAD`). Drag the folder containing your files to the Terminal Dock icon at the bottom. Input the command as follows:
```bash
assimp export interior.obj interior.dae --config=full
```
Remember to replace `interior.obj` with the file name of your model.

**Linux users:**

Build the latest assimp since assimp 3.3.1 is buggy:

```bash
git clone https://github.com/assimp/assimp.git
cd assimp
mkdir build
cd build
cmake .. -DASSIMP_BUILD_IFC_IMPORTER=OFF # turn off the IFC format, which takes lots of resource to build
make
sudo make install
```
Convert the model file:
```bash
assimp export interior.obj interior.dae --config=full
```
Remember to replace `interior.obj` with the file name of your model.

### 2.5 Import the interior model

Upload the .dae file and the texture images (*.jpeg) by "File" -> "Upload Local Files...", or simply drag-and-drop the files from file explorer to the Cloud9 workspace file panel.

Finally, replace the primitive shapes to our room model by using the [`<a-collada-model>`](https://aframe.io/docs/0.5.0/primitives/a-collada-model.html) entity. You probably want to set `scale="0.01 0.01 0.01"`.

### 2.6 Publish the web page

In the bottom "bash" termainal panel, input the commands as follows:
``` bash
git add --all
git commit -m "update web site"
git push
```

The files in the Cloud9 workspace will be synchronized to your GitHub repository. You can now use the GitHub Pages link created in the earlier step to view the web page.
