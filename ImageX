<?php
/**
 * @link https://github.com/SDKiller/zyx-widget-imagex
 * @copyright Copyright (c) 2014 Serge Postrash
 * @license BSD 3-Clause, see LICENSE.md
 */

namespace zyx\widgets;

use Yii;
use yii\base\Widget;
use yii\helpers\Url;
use yii\helpers\Html;
use yii\base\InvalidParamException;


/**
 * Class ImageX
 *
 * Widget for simple generating OpenGraph meta tags and Schema.org markdown for images ('X' stands for 'eXtended')
 *
 * @package zyx\widgets
 */
class ImageX extends Widget
{

    /**
     * @var null $src Image src attribute (the same you could used for [[Html::img()]]
     */
    public $src = null;
    /**
     * @var array $options Usual image options (the same you could used for [[Html::img()]]
     */
    public $options = [];
    /**
     * @var array $og OpenGraph options (not mandatory) - if not set, widget will try to get them from image options
     * You can disable rendering OpenGraph meta tags by setting 'enable' => false
     */
    public $og = ['enable' => true];
    /**
     * @var array $md Schema.org options (not mandatory) - if not set, widget will try to get them from image options
     * You can disable rendering Schema.org markdown by setting 'enable' => false
     */
    public $md = ['enable' => true];


    public function __construct($config = [])
    {
        //by default both opengraph and microdata are enabled, disable if ['enable' => false] was explicitly passed to widget
        $config['og'] = array_merge($this->og, (isset($config['og'])) ? $config['og'] : []);
        $config['md'] = array_merge($this->md, (isset($config['md'])) ? $config['md'] : []);

        parent::__construct($config);
    }

    public function init()
    {
        if (empty($this->src)) {
            throw new InvalidParamException('Image path should be defined!');
        }

        parent::init();
    }


    public function run()
    {
        $html = '';
        $html_end = '';
        if (isset($this->og['enable']) && $this->og['enable'] !== false) {
            $this->renderHead();
        }
        if (isset($this->md['enable']) && $this->md['enable'] !== false) {
            $html .= '<div itemscope itemtype="http://schema.org/ImageObject" '.(empty($this->md['div_class']) ? '' : 'class="'.$this->md['container_class'].'"').'>';
            $html_end .= $this->renderMarkdown();
        }

        $html .= Html::img($this->src, $this->options);
        $html .= $html_end;

        echo $html;
    }

    /**
     * This function renderes meta tags for image according to OpenGraph protocol.
     * @see http://ogp.me/
     * Properties set explicitly in 'og' configuration array, passed to widget, have priority.
     * If no 'og' options were set, widget attempts to extract properties from image 'options' array.
     */
    private function renderHead()
    {
        $this->view->registerMetaTag(['property' => 'og:image', 'content' => Url::to($this->src)]);

        if (!empty($this->og['secure_url'])) {
            $this->view->registerMetaTag(['property' => 'og:image:secure_url', 'content' => $this->og['secure_url']]);
        }

        $type = empty($this->og['type']) ? $this->guessMime($this->src) : $this->og['type'];
        if (!empty($type)) {
            $this->view->registerMetaTag(['property' => 'og:image:type', 'content' => $type]);
        }

        if (!empty($this->options['width']) || !empty($this->og['width'])) {
            $this->view->registerMetaTag(['property' => 'og:image:width', 'content' => (empty($this->og['width']) ? $this->options['width'] : $this->og['width'])]);
        }
        if (!empty($this->options['height']) || !empty($this->og['height'])) {
            $this->view->registerMetaTag(['property' => 'og:image:height', 'content' => (empty($this->og['height']) ? $this->options['height'] : $this->og['height'])]);
        }
    }


    private function guessMime($src)
    {
        $mime = '';

        $mimes = [
            'bmp'  => 'image/bmp',
            'gif'  => 'image/gif',
            'jpeg' => 'image/jpeg',
            'jpe'  => 'image/jpeg',
            'jpg'  => 'image/jpeg',
            'png'  => 'image/png',
            'tiff' => 'image/tiff',
            'tif'  => 'image/tiff',
            'svg'  => 'image/svg+xml'
        ];

        $dot = strrpos($src, '.') + 1;
        $ext = substr($src, $dot);

        if (array_key_exists($ext, $mimes)) {
            $mime = $mimes[$ext];
        }

        return $mime;
    }


    /**
     * Properties set explicitly in 'md' configuration array, passed to widget, have priority.
     * Schema.org gives no exaples for 'width' and 'height' properties, we take appropriate example
     * @see http://help.yandex.ru/webmaster/video/schema-org-markup.xml
     * @return string
     */
    private function renderMarkdown()
    {
        $this->options['itemprop'] = 'contentUrl';

        $html = '';
        if (!empty($this->options['width']) || !empty($this->md['width'])) {
            $html .= '<meta itemprop="width" content="'.(empty($this->md['width']) ? $this->options['width'] : $this->md['width']).'" />';
        }
        if (!empty($this->options['height']) || !empty($this->og['height'])) {
            $html .= '<meta itemprop="height" content="'.(empty($this->md['height']) ? $this->options['height'] : $this->md['height']).'" />';
        }
        $html .= '</div>';

        return $html;
    }

}
