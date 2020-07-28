---
title: "CYBRICS CTF Krevedka (Forensic, Easy, 50 pts)"
layout: post
date: 2020-07-27
image: /assets/images/mark-img01.PNG
headerImage: false
tag:
- wargame
- cybrics
- Digital forensics
category: blog
author: pil9

---
 
![problem]({{site.url}}/images/cybrics/1.PNG){: class="center"}
<figcaption class="caption">Fig1.문제</figcaption>  
<br>
문제 오픈당시 Forensics 250점 짜리 문제고 pcapng 확장자의 패킷 파일이 주어집니다.  
문제를 해석해보면 'caleches' 유저를 해킹한 사람의 Login 정보를 찾아야 합니다 .

---
<br>
![problem]({{site.url}}/images/cybrics/2.PNG){: class="center"}
<figcaption class="caption">Fig2.selected_packets.pcapng</figcaption>  
<br>
첨부된 약 300MB 크기의 패킷 파일을 열어보면 다음과 같고 약 110만개의 패킷이 저장되어 있습니다.

---
<br>
![problem]({{site.url}}/images/cybrics/3.PNG){: class="center"}
<figcaption class="caption">Fig3.caleches</figcaption>
<br>
문제에서 주어진 caleches 유저명을 details로 검색해보면 약 3개정도의 문자열이 검색되고 모두 로그인을 위한 POST 요청값으로 확인할 수 있습니다.

---
<br>
![problem]({{site.url}}/images/cybrics/4.PNG){: class="center"}
<figcaption class="caption">Fig4.Packet repeat</figcaption>
<br>
다른 패킷들도 확인해보면 TCP 연결을 위한 3way handshake 요청과 서버 response 값 그리고 위에서 확인한 로그인 요청을 위한 POST 데이터가 반복되는것을 확인할 수 있습니다.

---
<br>
![problem]({{site.url}}/images/cybrics/5.PNG){: class="center"}
<figcaption class="caption">Fig5.SQL Injection</figcaption>
<br>
'caleches' 문자열 검색을 통해 나머지 패킷들도 확인해보면 HTML URL Encode 필드의 password 값에 <b>"" or 1=1 =="</b> 값이 들어가있는 것을 보아 SQL injection 공격이 이루어진 것을 확인할 수 있습니다.

해당 패킷의 TCP Stream 데이터를 통해 공격자의 요청임을 유추할 수 있습니다.

---
<br>
<pre><code class = "language-python">POST /login HTTP/1.1
Host: kr3vedko.com
User-Agent: UCWEB/2.0 (Linux; U; Opera Mini/7.1.32052/30.3697; www1.smart.com.ph/; GT-S5360) U2/1.0.0 UCBrowser/9.8.0.534 Mobile
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
Cookie: session=b75d53bb-1326-4d78-aedf-9bd92e237fbf
Content-Length: 39
Content-Type: application/x-www-form-urlencoded
</code></pre>
<br>

---
하지만 패킷 내 POST 요청이 너무 많으므로 공격자가 사용한 User 로그인 정보를 찾기 위해서 Fig5 패킷 요청과 동일한 세션 또는 사용자 에이전트값이 일치하는 패킷을 찾아야 합니다.

---
<br>
![problem]({{site.url}}/images/cybrics/6.PNG){: class="center"}
<figcaption class="caption">Fig6.User-Agent</figcaption>
<br>
사이트 로그인을 위한 POST 요청마다 TCP 연결과정이 이루어지므로 세션정보를 제외하고 사용자 에이전트 값이 일치하는 패킷을 검색해보면 'caleches' 로그인 패킷 이외의 POST 요청 결과값이 1개 나오는 것을 확인할 수 있습니다. 

<br>

---
<br>
![problem]({{site.url}}/images/cybrics/7.PNG){: class="center"}
<figcaption class="caption">Fig7.Result</figcaption>
<br>
검색된 패킷의 Form item: "login" = "micropetalous" 값이 플래그로 인증되는 것을 확인할 수 있습니다.

---
<pre><code class = "language-html">Flag : cybrics{micropetalous}
</code></pre>


