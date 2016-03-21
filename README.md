# Step 3: resizing an Ajaxed Image

Some desktop websites bring in images via Ajax after the page has already loaded, these can be a challenge to optimize for mobile, but we have a solution!

## Preparation

If you haven't done so already, check out the [Hijax](https://github.com/mobify/workshop--hijax) tutorial to learn how we use our **Hijax** tool to intercept Ajax requests. 

## Task

### Add Hijax and resizeImages to UI

The UI files don't automatically have access to the plugins that the view files do. We need to take a look at the paths declared in `app/config/`ui.js`. YOu'll notice that Hijax should already have a path listed, after the Hijax path lets add a comma and insert two new paths: 

``` javascript
    ...,
    'mobifyjs/utils': '../node_modules/adaptivejs/node_modules/mobifyjs-utils/utils',
    'resizeImages': 'bower_components/imageresize/resizeImages'
```

We also need to add a path to `'mobifyjs/utils'` as resizeImages uses this path.

### Prepare the Template

Inside of `app/pages/home/template.dust` we'll add a div `class="c-ad-space"` that we can append the image to. Add it between the `c-shipping-banner` and the `t-home__promo` divs.

``` html
    <div class="c-shipping-banner">...</div>

    <div class="c-ad-space">
    </div>

    <div class="t-home__promo">
    ...
```

### Change to Script Order

In order for Hijax to be successful, the Hijax script needs to run before the desktop Ajax call that it is listening for so we need to make a small change inside of `app/global/base.dust`. We will cut `{desktopScripts}` from it's position at the top of the `{+scripts}` block, and move it to the bottom. We'll move it so that desktop scripts run last inside of that `{+scripts} block.

``` javascript
        ...
        {desktopScripts}
    {/scripts}
``` 

Now we're ready to start using Hijax!

### Add Hijax and ResizeImages to UI

Inside of `app/pages/home/ui.js` we'll add Hijax and ResizeImages to the require block, and pass it to the opening function.

``` javascript
    define([
        '$',
        'hijax',
        'resizeImages'
    ],
    function($, Hijax, ResizeImages) { ... });
```

### Hijax the /add-image ajax call

On the desktop site the site makes a call to `/add-image.html` and returns the ad that is displayed. We'll make a new Hijax and have it listen for that call. Still in `app/pages/home/ui.js` inside of the `homeUI` function write the following:

``` javascript 
    var hijax = new Hijax();
    hijax.set('add-image', '/add-image.html', {
        receive: function(data, xhr) {
        console.log(data);
        }
    }
```  

If you preview your project at this point and open the crome web inspector you will see `<img class="diagon-alley" src="/img/promos/diagon-alley.png" />` displayed in the console. So we have successfully captured the result of the ajax call, now we just need to resize the image and append it to the div we created earlier.

### Resize the Image

Inside of the Hijax `receive` function let's delete the `console.log(data)` and create a new options variable. We'll allow ResizeImages to decide the ideal size this image should be, by not setting a default `maxHeight` or `maxWidth`. Since this image was not on the page when adaptive first ran, it still has an `src` attribute, not a `x-src` attribute. 

By default ResizeImages selects on `x-src` so we need to change the `sourceAttribute` option.  

```
    var options = {
        sourceAttribute: 'src'
    };
```

Now we can now create a `$image` variable and set it equal to `$(data)`. Then We'll pass `$image` to our image resizer and append it to our available div. still inside of the `receive` function append:

```
    var $image = $(data);
    ResizeImages.resize($image, options);
    $('.c-ad-space').append($image);
```

Our additions to the `homeUI` function should look something like this: 

```
    var homeUI = function() {
        var hijax = new Hijax();
        hijax.set('add-image', '/add-image.html', {
            receive: function(data, xhr) {
                var options = {
                    sourceAttribute: 'src'
                };
                var $image = $(data);
                ResizeImages.resize($image, options);
                $('.c-ad-space').append($image);
            }
        });
        ...
    };
```

### Take a Look

In the terminal run `grunt preview -auto` and click the preview button in the browser. 

You'll notice that the ad for Diagon Alley now appears via ajax after the page has loaded. You can inspect it and check that the image src now begins with "//ir0.mobify.com..."


## Continue to Completed Workshop

When you're ready to take a look at the completed code, run the following command:

```
git reset --hard HEAD && git clean -df && git checkout completed-workshop
```

Continue to view the [Completed Workshop](https://github.com/mobify/workshop--mobile-assets/blob/completed-workshop).