# Deploying cables.gl to Webflow

The following describes how to successfully embed cables.gl patch that can take inputs from outside canvas (like touch) on Webflow.

## Edit settings in cables.gl

Set image compose node size setting to to `manual` and `1000x1000` for example. This will prevent strange resizing when embedded in Webflow container.

Set mouse or touch input are to `document`, to react to outputs outside of canvas.

## Export patch.js file

From cables.gl export patch.js file. Ignore the rest.

## Put the file on Github

Add file to a GitHub repo, this folder.
https://github.com/notprolands/Cable-Patches-Jsdelivr/tree/main/js

## Open the file on GitHub

Select the file and copy the url, e.g. 
https://github.com/notprolands/Cable-Patches-Jsdelivr/blob/main/js/patch.js

## Convert to Jsdelivr link

 Go to https://www.jsdelivr.com/github. Paste GitHub link and obtain Jsdelivr link

## Change Webflow page settings

Go to Webflow editor, select the page to edit. Click settings cog wheel and scroll down to `before </body> tag`. Add the following line:

```js
<script src="https://cdn.jsdelivr.net/gh/notprolands/Cable-Testing@main/js/patch.js"></script>
```

## Create a code embed element on page

Select code embed element and put it into some sort of div. Past the following code into the element.
 
```html
<html>
<head>
		<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1">
    <title>touch_controls</title>
    <style>
    body, html {
        height: 100px;
        margin: 0;
        padding: 0;
    }

    canvas {
        display: block;
        position: absolute;
        width: 1000px;
        height: 300px;
		}
    </style>
</head>
<body>
    <canvas id="glcanvas"></canvas>
    <script type="text/javascript" src="js/patch.js" async></script>
    <script type="text/javascript">
        function showError(errId, errMsg) {
            // Handle critical errors here if needed
        }

        function patchInitialized(patch) {
            // Access the patch object (patch), register variable watchers, etc.
        }

        function patchFinishedLoading(patch) {
            // The patch is ready, all assets have been loaded
        }

        document.addEventListener("CABLES.jsLoaded", function (event) {
            CABLES.patch = new CABLES.Patch({
                patch: CABLES.exportedPatch,
                "prefixAssetPath": "",
                "assetPath": "assets/",
                "jsPath": "js/",
                "glCanvasId": "glcanvas",
                "glCanvasResizeToWindow": false,  // Set to false to prevent strange resizing when pulling bottom border
                "onError": showError,
                "onPatchLoaded": patchInitialized,
                "onFinishedLoading": patchFinishedLoading
            });
        });

        // Prevent default touch behaviors like scrolling
        document.getElementById('glcanvas').addEventListener('touchmove', function(e) {
            e.preventDefault();
        }, false);
    </script>
</body>
</html>
```
Pay attention to the following.

Set this to false otherwise you may get strange resizing when pulling bottom border of window.

`glCanvasResizeToWindow": false`

Set canvas to fixed width and height that fits into desired container.

```html
canvas {
        display: block;
        position: absolute;
        width: 1000px;
        height: 300px;
		}
```

## Set Webflow code embed overflow hidden

In Webflow UI set code embed container size width `100%`, height `100%` and overflow `hidden` (crossed out eye).

## Set the code embed to relative

In Webflow UI, set position of the code embed container to `relative`. This will ensure that content is bound by the div and not overflowing. Position set to `static` will make it overflow everywhere.

## Set events to none

In Webflow UI, select code embed container 

## Publish the site

Changes wont be visible without publish.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc2MDA4NTE1MCwxMzg2MzQ2MDYsODM5ND
I4NDcyLDE5NTc1ODg3NF19
-->