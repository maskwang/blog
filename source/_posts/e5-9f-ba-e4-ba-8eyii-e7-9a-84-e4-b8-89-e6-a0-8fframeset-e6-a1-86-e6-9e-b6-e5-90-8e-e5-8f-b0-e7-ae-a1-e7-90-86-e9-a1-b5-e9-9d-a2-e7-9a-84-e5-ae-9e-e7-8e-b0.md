---
title: 基于YIi的三栏frameset框架后台管理页面的实现
id: 35
categories:
  - Yii
  - 技术专栏
date: 2011-08-21 05:47:17
tags:
---

前段时间和大家讨论过 yii后台管理页面结构实现方法的问题，现在我的项目接近收尾，向大家分享一下我的后台管理页面实现，
就是那种常见的 frameset三栏布局，主要代码如下：

SiteController.php

&lt;?php

classSiteControllerextendsCController
{
/**
* Declares class-based actions.
*/
publicfunction actions()
{
return array(
// captcha action renders the CAPTCHA image
// this is used by the contact page
'captcha'=&gt;array(
'class'=&gt;'CCaptchaAction',
'backColor'=&gt;0xEBF4FB,
),
);
}

/**
* This is the default 'index' action that is invoked
* when an action is not explicitly requested by users.
*/
publicfunction actionIndex()
{
// renders the view file 'protected/views/site/index.php'
// using the default layout 'protected/views/layouts/main.php'

//注意运行yiic shell前需要改回$this-&gt;render('index'); 否则无法进入shell
$this-&gt;render('index');
}

/**
* Displays the contact page
*/
publicfunction actionContact()
{
$contact=newContactForm;
if(isset($_POST['ContactForm']))
{
$contact-&gt;attributes=$_POST['ContactForm'];
if($contact-&gt;validate())
{
$headers="From: {$contact-&gt;email}\r\nReply-To: {$contact-&gt;email}";
mail(Yii::app()-&gt;params['adminEmail'],$contact-&gt;subject,$contact-&gt;body,$headers);
Yii::app()-&gt;user-&gt;setFlash('contact','Thank you for contacting us. We will respond to you as soon as possible.');
$this-&gt;refresh();
}
}
$this-&gt;render('contact',array('contact'=&gt;$contact));
}

/**
* Displays the login page
*/
publicfunction actionLogin()
{
$form=newLoginForm;
// collect user input data
if(isset($_POST['LoginForm']))
{
$form-&gt;attributes=$_POST['LoginForm'];
// validate user input and redirect to previous page if valid
if($form-&gt;validate())
$this-&gt;redirect(Yii::app()-&gt;user-&gt;returnUrl);
}
// display the login form
$this-&gt;layout='login';
$this-&gt;render('login',array('form'=&gt;$form));
}

/**
* Logout the current user and redirect to homepage.
*/
publicfunction actionLogout()
{
Yii::app()-&gt;user-&gt;logout();
$this-&gt;redirect(Yii::app()-&gt;homeUrl);
}
/**
* 管理框架页
*/
publicfunction actionDefault()
{
if(Yii::app()-&gt;user-&gt;isGuest){
$this-&gt;redirect(array('site/login'));
}
else{
$this-&gt;renderPartial('default');
}
}
/**
* 管理框架页 Head
*/
publicfunction actionHead()
{
if(Yii::app()-&gt;user-&gt;isGuest){
$this-&gt;redirect(array('site/login'));
}
else{
$this-&gt;renderPartial('head');
}
}
/**
* 管理框架页 left
*/
publicfunction actionLeft()
{
if(Yii::app()-&gt;user-&gt;isGuest){
$this-&gt;redirect(array('site/login'));
}
else{
Yii::app()-&gt;getClientScript()-&gt;registerCoreScript('jquery');
$this-&gt;layout='left';
$this-&gt;render('left');
}
}
}

views/site/default.php

&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd"&gt;
&lt;htmlxmlns="http://www.w3.org/1999/xhtml"&gt;
&lt;head&gt;
&lt;metahttp-equiv="Content-Type"content="text/html; charset=utf-8"/&gt;
&lt;title&gt;&lt;/title&gt;
&lt;/head&gt;

&lt;framesetrows="92,*"cols="*"frameborder="no"border="0"framespacing="0"&gt;
&lt;frame src="&lt;?php echo Yii::app()-&gt;request-&gt;baseUrl;?&gt;/index.php/site/head" name="topFrame" scrolling="no" noresize="noresize" id="topFrame" /&gt;
&lt;framesetcols="215,*"frameborder="no"border="0"framespacing="0"&gt;
&lt;frame src="&lt;?php echo Yii::app()-&gt;request-&gt;baseUrl;?&gt;/index.php/site/left" scrolling="no" noresize="noresize" id="leftFrame" /&gt;
&lt;framesrc=""name="mainFrame"id="mainFrame"/&gt;
&lt;/frameset&gt;
&lt;/frameset&gt;
&lt;noframes&gt;&lt;body&gt;
&lt;/body&gt;
&lt;/noframes&gt;&lt;/html&gt;

其它相关的layout和view文件就不提供了，就是简单的html