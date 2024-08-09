# Deploying cables.gl to Webflow

The following describes how to successfully embed cables.gl patch that can take inputs from outside canvas (like touch) on Webflow.

## Edit settings in cables.gl

Set image compose node to manual 1000x1000 for example. This will prevent strange resizing when embedded in Webflow div.

## Export patch.js file

From cables.gl export patch.js file. Ignore the rest.

## Put the file on Github

Add file to a GitHub repo, this folder.
https://github.com/notprolands/Cable-Patches-Jsdelivr/tree/main/js

## Open the file on GitHub

Select the file and copy the url, e.g. 
https://github.com/notprolands/Cable-Patches-Jsdelivr/blob/main/js/patch.js

## Convert to Jsdelivr link

 - Go to https://www.jsdelivr.com/github
 - Paste GitHub link
 - Obtain Jsdelivr link

## Change Webflow page settings

 - Go to Webflow editor
 - Select the page to edit
 - Click settings cog wheel
 - Scroll down to `before </body> tag`
 - Add the following line:

```js
<script src="https://cdn.jsdelivr.net/gh/notprolands/Cable-Testing@main/js/patch.js"></script>
```

## Create a code embed element on page

 - Select code embed element and put it into some sort of div
 - Past the following code into the element
 
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

## Set the code embed to relative

In position set it to `relative`. This will ensure that content is bound by the div and not overflowing.

## Set code embed overflow hidden

Code embed should have hidden overflow, not visible.

## Publish the site

Changes wont be visible without publish.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU4NTEzNTE4OCwxMzg2MzQ2MDYsODM5ND
I4NDcyLDE5NTc1ODg3NF19
-->