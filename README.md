# Upload Button for Web Interface
Simple button that will open a file explorer window on your phone or computer and allow you to upload a file, for the ReMarkable Tablet's Web Interface. Simple work around to drag and drop. This mod requires adding 4 lines to the index.html file. Remeber to change `app-<your-css-version>.css` and `app-<your-js-version>.js` to the correct file names on your device, Ex: app-90b703c8a29751ae81d5.css and app-9fb33d24a178758a2437.js. Picture at bottom.


## Instructions

All the following commands will be performed after ssh-ing into your ReMarkable Tablet.

Navigate to the webui directory

`cd /usr/share/remarkable/webui`

The index.html file will look something like this

```
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport"
            content="width=device-width, initial-scale=1.0, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
        <title>reMarkable &bull; Web Interface</title>
        <link href="/app-<your-css-version>.css" rel="stylesheet">
    </head>
    <body>
        <div id="root"></div>
        <script type="text/javascript" src="/app-<your-js-version>.js"></script>
    </body>
</html>
```

First, back the file up. 

`cp index.html index.html.bak`


Then, add the following lines of code underneath the `<body>` line and above the `<div id="root"></div>` line in index.html. Full code at bottom.

```
<form method=post enctype=multipart/form-data action=upload>
    <input type=file name=file>
    <input type=submit value=Upload>
</form>
```

After saving, you should be able to reload the web page and see the upload button. If not, restore the backup:

`cp index.html.bak index.html`


## Optinal Desktop Formatting

The web interface uses css to lock the file list in a fixed postion. The addition of the button at the top will cause the list to start too high on the page. 

The offending css is `.list{position:fixed;width:70%;top:70px;bottom:0;overflow-y:auto;max-width:d700px}` in `app-<your-css-version>.css`

We want to change top:70px to top:94:px.

### Bash Commands to do so

First, back the file up. 

`cp app-<your-css-version>.css app-<your-css-version>.css.bak`

Then check if this css style has the same format in your css file. Run this command

`grep -oh '.list{position:fixed;width:70%;top:70px' app-<your-css-version>.css && echo 'Match found' || echo 'No match found'`

If so, the following command will change top to 94px.

`sed -i 's/.list{position:fixed;width:70%;top:70px/.list{position:fixed;width:70%;top:94px/g' app-<your-css-version>.css`

If not, you will need to manually change this.

## Limitations

- Without more advanced file upload handeling, you will be redirected to the response status of the file upload.

- Only one file can be uploaded at a time.

## Example

![mobile_web_ui](https://user-images.githubusercontent.com/122753594/213054617-a4f68efe-08a5-4c45-a866-6103e3e144fd.jpg)

## Full index.html 

```
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport"
            content="width=device-width, initial-scale=1.0, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
        <title>reMarkable &bull; Web Interface</title>
        <link href="/app-<your-css-version>.css" rel="stylesheet">
    </head>
    <body>
        <form method=post enctype=multipart/form-data action=upload>
            <input type=file name=file>
            <input type=submit value=Upload>
        </form>
        <div id="root"></div>
        <script type="text/javascript" src="/app-<your-js-version>.js"></script>
    </body>
</html>
```