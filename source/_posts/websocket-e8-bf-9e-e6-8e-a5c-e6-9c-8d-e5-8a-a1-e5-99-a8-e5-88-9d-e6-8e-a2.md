---
title: websocket连接c服务器初探
id: 583
categories:
  - C语言
  - 编程语言
date: 2012-08-07 12:43:41
tags:
---

client.html
<pre lang="html"><script type="mce-text/javascript">// <![CDATA[
		var socket;

		function connect()
		{
			try{
				socket = new WebSocket('ws://192.168.26.99:9880');
			}catch(e){
				alert('error');
				return;
			}
			socket.onopen = sOpen;
			socket.onerror = sError;
			socket.onmessage = sMessage;
			socket.onclose = sClose;
		}

		function sOpen()
		{
			alert('connect success!');
		}

		function sError()
		{
			alert('connect error');
		}

		function sMessage(msg)
		{
			alert('server says:'+msg);
		}

		function sClose()
		{
			alert('connect close!');
		}

		function send()
		{
			socket.send('hello,i am maskwang');
		}

		function close()
		{
			socket.close();
		}

// ]]></script><input id="msg" type="text" /> <button id="connect" onclick="connect();">Connect</button> <button id="send" onclick="send();">Send</button> <button id="close" onclick="close();">Close</button></pre>
<!--more-->
<pre lang="html"></pre>
socket.c
<pre lang="c">/*
 * Author: mask.wang.cn@gmail.com
 * Date: 2012/8/7
 */
 #include 
 #include 
 #include &lt;arpa/inet.h&gt;
 #include &lt;netinet/in.h&gt;
 #include &lt;sys/types.h&gt;
 #include &lt;sys/socket.h&gt;
 #include 
 #include &lt;openssl/sha.h&gt;
 #define PORT 9880
 #define MAXLENGTH 1024+1
 void parsestr(char *request, char *data)
 {
	int needle;
	strcat(request, "HTTP/1.1 101 WebSocket Protocal Handshake\r\n");
	strcat(request, "Upgrade:WebSocket\r\n");
	strcat(request, "Connection:Upgrase\r\n");
	//这个值等于 base64_encode(sha1(key+258EAFA5-E914-47DA-95CA-C5AB0DC85B11)) 由于我这里没有合适的base64算法和sha1算法，所以就不写了。
    strcat(request,"Sec-WebSocket-Accept:ZmQ5OWUxMjgwMTViNTEyM2FmZTRlOGViODZkNTk3OTBjMWRiYjBiYg==\r\n");
 }

 int main(int argc, char ** argv)
 {
	int sockfd, len, maxfd, ret, retval, newfd;
	int reuse = 1;
	fd_set rwfd;
	struct sockaddr_in l_addr;
	struct sockaddr_in c_addr;
	struct timeval tv;
	char buf[MAXLENGTH];
	char request[MAXLENGTH];
	sockfd = socket(AF_INET, SOCK_STREAM, 0);
	setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, &amp;reuse, sizeof(int));

	if (sockfd &lt; 0)
		perror("socket");
	l_addr.sin_port = htons(PORT);
	l_addr.sin_family = AF_INET;
	l_addr.sin_addr.s_addr = INADDR_ANY;

	if (bind(sockfd, (struct sockaddr*)&amp;l_addr, sizeof(struct sockaddr)) &lt; 0)
		perror("bind");

	listen(sockfd, 5);
	len = sizeof(struct sockaddr);
	tv.tv_sec = 5;
	tv.tv_usec = 0;
	bzero(request, MAXLENGTH);

	while(1)
	{
		newfd = accept(sockfd, (struct sockaddr*)&amp;c_addr, &amp;len);
		if (newfd == -1){
			perror("accept");
			exit(1);
		} else {
			printf("%s is comming\n", inet_ntoa(c_addr.sin_addr));
		}

		while(1)
		{
			FD_ZERO(&amp;rwfd);
			FD_SET(0, &amp;rwfd);
			FD_SET(newfd, &amp;rwfd);
			maxfd = 0;
			if (newfd &gt; maxfd)
				maxfd = newfd;
			retval = select(maxfd+1, &amp;rwfd, NULL, NULL, &amp;tv);
			if (retval == -1){
				perror("select");
				break;
			} else {
				if (FD_ISSET(0, &amp;rwfd)){
					bzero(buf, MAXLENGTH);
					fgets(buf, MAXLENGTH, stdin);
					parsestr(request, buf);
					len = send(newfd, request, MAXLENGTH, 0);
					if (len &gt; 0)
						printf("i sayed:%s\n", buf);
				}

				if (FD_ISSET(newfd, &amp;rwfd)){
					len = recv(newfd, buf, MAXLENGTH, 0);
					if (len &gt; 0){
						if (strncmp(buf, "quit", 4) == 0){
							close(newfd);
						} else {
							printf("client says:%s\n", buf);
						}
					}
				}
			}
		}
		return 0;
	}
 }</pre>