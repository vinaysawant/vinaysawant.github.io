---
layout: post
title: "Call Rest API : Using PHP Curl"
date: 2015-01-30 14:52:51 +0530
comments: true
categories: 
---

Recently I was working on a rest API module, requirement was to get data from
one server and send it to another server who accepts only http/https calls.

To do this PHP provides `file_get_contets` and `Curl` option. Curl has more 
control on such calls so I chose it.

Now we are ready to fire get, post, put and delete calls using curl.
`https://your-server-url` is server url . 
######GET
```php
$call = new Curl('https://your-server-url/users');
$call->get()
``` 
######POST
```php
$data = array(
	//Data to send
);
$call = new Curl('https://your-server-url/users',$data);
$call->post()
```
######PUT
```php
$data = array(
	//Data to send
);
$call = new Curl('https://your-server-url/users/1',$data);
$call->put()
```
######DELETE
```php
$call = new Curl('https://your-server-url/users/1');
$call->delete()
``` 
```php
Class Curl(){

	private $url;
	private $data;
	private $headers;
	

	function __construct($url,$data = array()){
		$this->url = $url,
		$this->data = $data,
		$this->headers = array('Content-Type:application/json')
	}

	public function get(){
		$ch = curl_init();
		curl_setopt($ch,CURLOPT_URL, $this->url);
		curl_setopt($ch,CURLOPT_TIMEOUT, 4);
		curl_setopt($ch,CURLOPT_ENCODING, '');
		curl_setopt($ch,CURLOPT_MAXREDIRS, 3);
		curl_setopt($ch,CURLOPT_FOLLOWLOCATION, true);
		curl_setopt($ch,CURLOPT_RETURNTRANSFER, true);
		curl_setopt($ch,CURLOPT_HEADER, false);
		$response = trim(curl_exec($ch),"\r\n")
		$responseCode = curl_getinfo($ch,CURLINFO_HTTP_CODE);

		if(responseCode == 200){
		    //Do your Operations
		}
	}

	public function post(){
		$ch = curl_init();
		curl_setopt($ch,CURLOPT_URL, $this->url);
		curl_setopt($ch,CURLOPT_POST, true);
		curl_setopt($ch,CURLOPT_HTTPHEADER, $this->headers);
		curl_setopt($ch,CURLOPT_RETURNTRANSFER, true);
		curl_setopt($ch,CURLOPT_POSTFIELDS, $this->data);
		curl_exec($ch);
		$responseCode = curl_getinfo($ch,CURLINFO_HTTP_CODE);

		if(responseCode == 200){
		    //Do your Operations
		}
	}

	public function put(){
		$ch = curl_init();
		curl_setopt($ch,CURLOPT_URL, $this->url);
		curl_setopt($ch,CURLOPT_CUSTOMREQUEST, 'PUT');
		curl_setopt($ch,CURLOPT_HTTPHEADER, $this->headers);
		curl_setopt($ch,CURLOPT_RETURNTRANSFER, true);
		curl_setopt($ch,CURLOPT_POSTFIELDS, $this->data);
		curl_exec($ch);
		$responseCode = curl_getinfo($ch,CURLINFO_HTTP_CODE);

		if(responseCode == 200){
		    //Do your Operations
		}
	}

	public function delete(){
		$ch = curl_init();
		curl_setopt($ch,CURLOPT_URL, $this->url);
		curl_setopt($ch,CURLOPT_CUSTOMREQUEST, 'DELETE');
		curl_setopt($ch,CURLOPT_RETURNTRANSFER, true);
		curl_exec($ch);
		$responseCode = curl_getinfo($ch,CURLINFO_HTTP_CODE);

		if(responseCode == 200){
		    //Do your Operations
		}
	}
}
```
