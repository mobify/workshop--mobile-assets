# Step 1: Resize The Banner Image

### Install NPM Modules

1. In your `workshop--mobile-assets` project folder, enter the following command to install NPM modules:

    ```
    npm install
    ```
## Task

### Include the Image Resizer in the Home View 

The imageresize component is initially included as default in an adaptivejs project's  bower components, and you may notice is automatically included in the `app/global/baseView.js` file. However, we need to get access to it in our `app/pages/home/view.js` file. 

app/pages/home/view.js:
```javascript
    define([
        '$',
        'global/baseView',
        'dust!pages/home/template',
        'resizeImages'
    ],
    function($, baseView, template, ResizeImages) { ... });
``` 
### Resize Banner Image

Inside of the context find the `hero` key. We are going to resize this banner image, and set its maximum height and width to 400px before the context returns it.
Your `hero` key should now look like this:

```javascript
    hero: function() {
        var $hero = $('.hero');
        ResizeImages.resize($hero, {
            maxWidth: 400,
            maxHeight: 400
        });
        return $hero;
    },
```

We are calling ResizeImages.resize, and sending it the image we want resized, along with an options hash.

### Take a Look

In the terminal run `grunt preview -auto` and click the preview button in the browser. 

Inspect the src attribute of the hero image, it should now start with "//ir0.mobify.com"

## Note

We have declared the maxWidth and maxHeight, however, when used without specifying these values our image resizer will default maxWidth to be the screen width of the device. So you can choose to allow Image Resizer to calculate these numbers for you to optimize for smartphones and tablets of various sizes. 


## Continue to Step 2

When you're ready to continue to Step 2, run the following command:

```
git reset --hard HEAD && git clean -df && git checkout step-2-alternate-image
```

Then, follow the directions in the [README](https://github.com/mobify/workshop--mobile-assets/blob/step-2-alternate-image/README.md) for the Step 2 branch.