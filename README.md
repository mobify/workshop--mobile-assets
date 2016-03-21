# Step 2: Setting an Alternate Image for Mobile

## Task

### Extract Options into a variable

 We are going to resize the shipping image and set its height and width to also be 400px, so we are going to extract and reuse the options used in the hero key. Just inside of the function, above the `return` statement add the following:

```javascript
    var defaultOptions = {
        maxWidth: 400,
        maxHeight: 400
    };
```
Inside the `hero` key we should now be passing the `defaultOptions` variable to `ResizeImages.resize()`. 

``` javascript
    ...
    ResizeImages.resize($hero, defaultOptions);
    ...
```

### Resize the Shipping Image

Inside of the context find the `shipping` key. Let's make a `$shipping` variable with `var $shipping = $('.free-shipping');`. Now we can send that variable along with the options hash to the `ResizeImages.resize()` function. `ResizeImages.resize()` is a destructive function so the `$shipping` variable gets changed, and we can now return it (in this case we are returning its `x-src` attribute).

``` javascript
    shipping: function() {
        var $shipping = $('.free-shipping');
        ResizeImages.resize($shipping, defaultOptions);
        return $shipping.attr('x-src');
    },
```

### Use an Alternate Shipping Image

The shipping image has an attribute called `data-mobile-src`, someone decided that they want this image to show up on mobile instead of the original. We can set this by changing the `sourceAttribute` key in our options.

Inside of the `shipping` function let's define a new variable:

```
    var options = {
        sourceAttribute: 'data-mobile-src'
    };
```

### Change the Image Quality

while we're at it, let's change the image quality. Image quality can only be changed on JPGs, it is a value from 1 to 100. (If your image is not a JPG, you can change it by adding `format: 'jpg'` to your options hash.) This image is already a JPG so we'll leave it as it is, but let's add a `quality` key to our new options hash.

```
    var options = {
        sourceAttribute: 'data-mobile-src',
        quality: 50
    };
```

### Combine the Options Hashes

we want to reuse the default options, but also use our new options, so we'll use JQuery's `$.extend` function to combine the two.

``` javascript
   $.extend(options, defaultOptions);
```

This is also a destructive function, with the first hash being changed, this will change our `options` variable.

we can now pass our `options` variable as the options hash for `ResizeImages.resize()`

``` javascript
    ResizeImages.resize($freeShipping, options);
``` 

Your `app/pages/home/view.js` file should now look something like this:

``` javascript
    define([
        '$',
        'global/baseView',
        'dust!pages/home/template',
        'resizeImages'
    ],
    function($, baseView, template, ResizeImages) {
        var defaultOptions = {
            maxWidth: 400,
            maxHeight: 400
        };
        return {
            template: template,
            extend: baseView,
            context: {
                templateName: 'home',
                hero: function() {
                    var $hero = $('.hero');
                    ResizeImages.resize($hero, defaultOptions);
                    return $hero;
                },
                shipping: function() {
                    var $freeShipping = $('.free-shipping');
                    var options = {
                        sourceAttribute: 'data-mobile-src',
                        quality: 50
                    };
                    $.extend(options, defaultOptions);
                    ResizeImages.resize($freeShipping, options);
                    return $('.free-shipping').attr('x-src');
                },
                ...
            }
        };
    });
```

### Take a Look

In the terminal run `grunt preview -auto` and click the preview button in the browser. 

You'll notice that the free shipping image has now changed, and if you inspect it, it also has an src that begins with "//ir0.mobify.com..."


## Continue to Step 3

When you're ready to continue to Step 3, run the following command:

```
git reset --hard HEAD && git clean -df && git checkout step-3-images-via-ajax
```

Then, follow the directions in the [README](https://github.com/mobify/workshop--mobile-assets/blob/step-3-images-via-ajax/README.md) for the Step 2 branch.