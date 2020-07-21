---
title: "HackCTF Forensics Let'S get it ! Boo*4(350)"
layout: post
date: 2020-07-20
image: /assets/images/mark-img01.png
headerImage: false
tag:
- wargame
- HackCTF
- Digital forensics
category: blog
author: pil9

---
 
![problem]({{site.url}}/images/Hack1_problem.png){: class="center"}
<figcaption class="caption">문제</figcaption>  
<br>
Forensics 350점 짜리 문제고 비트맵 이미지 파일이 주어집니다.
문제에서 L,S,B 글자만 대문자인것을 보아 LSB 문제로 유추할 수 있습니다.

![problem]({{site.url}}/images/Hack1_problem_1.png){: class="center"}
파일을 열어보면 다음과 같습니다.(누가봐도 비트 관련 문제)

<br>
![problem]({{site.url}}/images/Hack2_hxd.png){: class="center"}
<figcaption class="caption">유효 pin값은 101부터</figcaption>
<br>
파일의 16진수 값을 보면 비트맵 파일 헤더, DIB 헤더 값에 특이점이 없습니다.(54Byte)

![problem]({{site.url}}/images/Hack3_zsteg.png){: class="center"}
최하위 비트(lsb)에 숨겨져있는 텍스트를 확인하기 위해서 [zsteg]{https://github.com/zed-0xff/zsteg}를 사용하면 Flag 관련 문자열이 있음을 확인할 수 있습니다.
<br>

<br>
![problem]({{site.url}}/images/Hack2_hxd.png){: class="center"}
<figcaption class="caption">zsteg .bmp lsb,bY -v</figcaption>
<br>
offset 기준으로 ASCII값을 확인하기 위해 -v(verbos)옵션을 사용하여 출력하면 전체 값을 확인할 수 있습니다.

<pre><code class = "language-c">zsteg file.bmp b,lsb,bY -v
</code></pre>




