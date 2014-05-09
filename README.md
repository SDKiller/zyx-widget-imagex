ImageX
=======

[![Latest Stable Version](https://poser.pugx.org/zyx/widget-imagex/v/stable.png)](https://packagist.org/packages/zyx/widget-imagex)
[![Latest Unstable Version](https://poser.pugx.org/zyx/widget-imagex/v/unstable.png)](https://packagist.org/packages/zyx/widget-imagex)
[![Total Downloads](https://poser.pugx.org/zyx/widget-imagex/downloads.png)](https://packagist.org/packages/zyx/widget-imagex)
[![License](https://poser.pugx.org/zyx/widget-imagex/license.png)](https://packagist.org/packages/zyx/widget-imagex)



Widget for simple generating OpenGraph meta tags and Schema.org markdown for images ('X' stands for 'eXtended').

You may do absolutely nothing - OpenGraph meta tags and Schema.org markdown will be generated 'on-the-fly' from image properties (which you can still define like you do in ```Html::img()```).


REQUIREMENTS
------------

You should generally follow [Yii 2 requirements](https://github.com/yiisoft/yii2/blob/master/README.md).
The minimum is that your Web server supports PHP 5.4.0.


INSTALLATION
------------

### Install via Composer

If you do not have [Composer](http://getcomposer.org/), you may install it by following the instructions
at [getcomposer.org](http://getcomposer.org/doc/00-intro.md#installation-nix).

Either run

```
php composer.phar require --prefer-dist zyx/widget-imagex "*"
```

or add

```
"zyx/widget-imagex": "*"
```

to the require section of your composer.json.


USAGE
-----


Just add following string to your layout

```
use zyx\widgets\ImageX;
```

And call widget where the image is supposed to be instaed of usual ```Html::img()```:

```
echo ImageX::widget([
                  'src' => 'http://static.yiiframework.com/css/img/logo.png',
                  'options' => ['width' => 280, 'height' => 60],
                  'og' => [],
                  'md' => ['div_class' => 'image_wrap']
                ]);

```

**Note:** only 'src' parameter is mandatory. Array of 'options' is the same you use for image options in ```Html::img()```. If both 'og' (OpenGraph) and 'md' (schema.org) configuration arrays are empty - you may not declare them at all. You can to disable one of them (they are both enabled by default) - e.g. ```'md' => ['enable' => false]```.


So, the result of example above should be something like this:
--------------------------------------------------------------

In the HEAD:

```
...
  <meta property="og:image" content="http://static.yiiframework.com/css/img/logo.png">
  <meta property="og:image:type" content="image/png">
  <meta property="og:image:width" content="280">
  <meta property="og:image:height" content="60">
...

```

In the BODY ('img' is wrapped in 'div' tag):

```
...
<div itemscope itemtype="http://schema.org/ImageObject" class="image_wrap">
  <img src="http://static.yiiframework.com/css/img/logo.png" width="280" height="60" alt="" itemprop="contentUrl" />
	<meta itemprop="width" content="280" />
	<meta itemprop="height" content="60" />
</div>
...

```

Properties like 'width' and 'height' set explicitly in 'og' and 'md' configuration arrays, passed to widget, have priority.
If no such options were set, widget attempts to extract properties from image 'options' array.

So you can just call:

```
echo ImageX::widget([
                  'src' => 'http://static.yiiframework.com/css/img/logo.png',
                  'options' => ['width' => 280, 'height' => 60]
                ]);

```

like you called ```Html::img()``` with no additional options.
