1. laravel-5-and-the-front-end - Introduction
--------------------------------------

In `gulpfile.js` you can add:
```
elixir(function(mix) {
    mix.less('app.less') //possible ; here
        .coffee()  //this triggers coffee script
        .phpUnit() //this will trigger unit tests
        .scripts([ //this concatenates and minifies the following script in this order
            "js.jquery.js",
            "js/underscore.js",
            "js/one.js"
        ])
        .stryles([ //sames as scripts but for style sheets
            "js.jquery.js",
            "js/underscore.js",
            "js/one.js"
        ])


        .publish( //publish moves these files from vendor to public
            'jquery/dist/jquery.min.js', //we find the file vendor/bowerComponents/jquery/dist/jquery.min.js
            'public/js/vendor/jquery.js' // and move it to public/js/vendor
        )
        .publish(
            'bootstrap-sass-official/assets/javascripts/bootstrap.js',
            'public/js/vendor/bootstrap.js'
        )
        .publish(
            'font-awesome/fonts',
            'public/css/fonts'
        )
});
```