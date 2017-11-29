---
title: Suffusion-添加新浪微博插件
tags:
  - Suffusion
  - 微博
id: 597
categories:
  - 开源项目
  - 技术专栏
date: 2012-08-29 16:00:25
---

最近在改版blog，想把weibo放在侧栏，网上荡了一些，都不是令我很满意，决定自己动手……

1\. 首先在目录wp-content\themes\suffusion\widgets下建一个插件文件：suffusion-sina-weibo.php

代码如下：<!--more-->

[php]
&lt;?php
/**
 * This replaces the default Sina Weibo widget of WordPress
 *
 * @package mask.wang.cn@gmail.com 2012.08.29
 * 
 */
/*新浪微博*/

class sina extends WP_Widget //新浪微博
{
    function sina(){
		$widget_ops = array('classname'=&gt;'set_contact','description'=&gt;'Mask:sina微博边栏窗口添加');
		$this-&gt;WP_Widget(false,'新浪微博',$widget_ops);
    }
    function form($instance){
		$instance = wp_parse_args((array)$instance,array('title'=&gt;'新浪微博','sina'=&gt;'2077531403'));
		$title = htmlspecialchars($instance['title']);
		$sina = htmlspecialchars($instance['sina']);

		echo '&lt;p style=&quot;text-align:left;&quot;&gt;&lt;label for=&quot;'.$this-&gt;get_field_name('title').'&quot;&gt;标题:&lt;input style=&quot;width:220px;&quot; id=&quot;'.$this-&gt;get_field_id('title').'&quot; name=&quot;'.$this-&gt;get_field_name('title').'&quot; type=&quot;text&quot; value=&quot;'.$title.'&quot; /&gt;&lt;/label&gt;&lt;/p&gt;';
		echo '&lt;p style=&quot;text-align:left;&quot;&gt;&lt;label for=&quot;'.$this-&gt;get_field_name('sina').'&quot;&gt;新浪微博id:&lt;input style=&quot;width:220px;&quot; id=&quot;'.$this-&gt;get_field_id('sina').'&quot; name=&quot;'.$this-&gt;get_field_name('sina').'&quot; type=&quot;text&quot; value=&quot;'.$sina.'&quot; /&gt;&lt;/label&gt;&lt;/p&gt;';
		echo '&lt;p style=&quot;text-align:left;&quot;&gt;登录&lt;a href=&quot;http://weibo.com/&quot; target=&quot;_blank&quot;&gt;新浪&lt;/a&gt;查看列如下面的蓝色代码&lt;/p&gt;';
    	echo '注意必须是数字,点击个人中心出现 ';
		echo '&lt;p style=&quot;text-align:left;&quot;&gt;&lt;div class=&quot;wp_syntax&quot; style=&quot;overflow: auto;height: 37px; width: 220px;border: 1px solid #C0C0C0;&quot;&gt;&lt;pre  style=&quot;font-family:monospace;&quot;&gt;   http://weibo.com/&lt;span style=&quot;color: #0000ff;&quot;&gt;2077531403&lt;/span&gt;/profile/    &lt;/pre&gt;&lt;/div&gt;&lt;/p&gt;';
	}
	function update($new_instance,$old_instance){
		$instance = $old_instance;
		$instance['title'] = strip_tags(stripslashes($new_instance['title']));
		$instance['sina'] = strip_tags(stripslashes($new_instance['sina']));
		return $instance;
    }
   function widget($args,$instance){
	  extract($args);
	  $title = apply_filters('widget_title',empty($instance['title']) ? '&amp;nbsp;' : $instance['title']);
	  $sina = empty($instance['sina']) ? '#' : $instance['sina'];

	  echo $before_widget;
	  echo $before_title . $title . $after_title;
	  ?&gt;

	&lt;div class='fix'&gt;

	&lt;iframe width=&quot;238&quot; height=&quot;400&quot; class=&quot;share_self&quot;  frameborder=&quot;0&quot; scrolling=&quot;no&quot; src=&quot;http://widget.weibo.com/weiboshow/index.php?width=238&amp;height=400&amp;fansRow=1&amp;ptype=1&amp;speed=0&amp;skin=1&amp;isTitle=0&amp;noborder=0&amp;isWeibo=1&amp;isFans=0&amp;uid=&lt;?php echo $sina ?&gt;&amp;verifier=ea6a7882&quot;&gt;&lt;/iframe&gt;
	 &lt;/div&gt;

	  &lt;?php 
	  echo $after_widget;
   }
}
add_action('widgets_init',create_function('', 'return register_widget(&quot;sina&quot;);'));
?&gt;
[/php]

2.然后在wp-content\themes\suffusion\widgets的suffusion-widgets.php下添加
include_once ($template_path . '/widgets/suffusion-sina-weibo.php');
register_widget("sina");
3\. 在后台小工具添加微博，填上自己的weiboid， 如该blog侧边栏的微博，大功告成！