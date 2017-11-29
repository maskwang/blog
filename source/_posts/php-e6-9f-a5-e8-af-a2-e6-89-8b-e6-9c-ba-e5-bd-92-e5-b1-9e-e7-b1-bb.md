---
title: php查询手机归属类
id: 544
categories:
  - Php
  - 技术专栏
  - 编程语言
date: 2012-05-11 12:00:28
tags:
---

最近工作需要， 写了个手机归属类， 欢迎拍砖
<pre lang="php">
<?php
/**
 * 查询 手机归属地 
 * edit on 2012-5-8 by mask.wang.cn@gmail.com
 * example: $data = new Guishu(136xxxx);
 */
header("Content-Type:text/html;charset=utf-8");

class Guishu
{
	//手机归属查询api
	const GUISHU_API = 'http://webservice.webxml.com.cn/WebServices/MobileCodeWS.asmx/getMobileCodeInfo';

	public $data = array();
	public $province = ''; //省
	public $city     = ''; //市
	public $type     = ''; //号码类型[移动|联通]
	public $card     = ''; //卡

	public function __construct($mobileCode, $userId = '')
	{

		$this->data = simplexml_load_string($this->getUrl(self::GUISHU_API, 'POST', http_build_query(array(
				'mobileCode' => $mobileCode, 
				'userId' => $userId)))); 

		if (strpos($this->data, 'http://')) { 
			return false;
		} else { 
			for ($i=0; $i < count($temp = explode(' ', $this->data)); $i++) {
				if ($i === 0) {
					$this->province = str_replace($mobileCode.'：', '', $temp[$i]); 
				}

				if ($i === 1) {
					$this->city = $temp[$i];
				}

				if ($i === 2) {
					$this->type = strstr($temp[$i], '移动') ? '移动' : '联通';
					$this->card = str_replace(array($this->city, $this->type), array('',''), $temp[$i]);
				}
			};
			return  array(
				'province'  => $this->province,
				'city'      => $this->city,
				'type'      => $this->type,
				'card'      => $this->card
			); 
		} 
	}

	/**
	* curl
	* @param string $url
	* @param string $method[post|get]
	* @param array $post_data
	* @return array|boole
	*/
	private function getUrl($url, $method = 'get', $post_data = '')
	{
		$ch = curl_init();
		curl_setopt($ch,CURLOPT_URL,$url);
		curl_setopt($ch,CURLOPT_RETURNTRANSFER,true);
		if ($method == 'post' || $method == 'POST') {
			//指定post数据
			curl_setopt($ch, CURLOPT_POST, 1);
			//添加变量
			curl_setopt($ch, CURLOPT_POSTFIELDS, $post_data);
		}
		curl_setopt($ch, CURLOPT_TIMEOUT, 10); 	// 超时时间
		$content = curl_exec($ch);
		curl_close($ch);
		$content = is_object($content) ? json_decode($content) : $content;

		return $content;
	}
}
?>
</pre>