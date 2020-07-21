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
<br>
![problem]({{site.url}}/images/Hack1_problem_1.png){: class="center"}
파일을 열어보면 다음과 같습니다.(누가봐도 비트 관련 문제)

<br>
![problem]({{site.url}}/images/Hack2_hxd.png){: class="center"}
<figcaption class="caption">HxD</figcaption>
<br>
파일의 16진수 값을 보면 비트맵 파일 헤더, DIB 헤더 값에 특이점이 없습니다.(54Byte)
<br>
[bitmap 파일 구조]{https://dojang.io/mod/page/view.php?id=702}

<br>
![problem]({{site.url}}/images/Hack3_zsteg.png){: class="center"}
<br>
다른 방법으로 최하위 비트(lsb)에 숨겨져있는 텍스트를 확인하기 위해서 [zsteg]{https://github.com/zed-0xff/zsteg}를 사용하면 Flag 관련 문자열이 있음을 확인할 수 있습니다.
<br>

<br>
![problem]({{site.url}}/images/Hack4_verbos.png){: class="center"}
<figcaption class="caption">b1,lsb,bY</figcaption>
<br>
offset 기준으로 ASCII값을 확인하기 위해 -v(verbos)옵션을 사용하여 출력하면 전체 값을 확인할 수 있습니다.

<pre><code class = "language-c">zsteg file.bmp b,lsb,bY -v
</code></pre>

<br>
![problem]({{site.url}}/images/Hack5_search.png){: class="center"}
<figcaption class="caption">Unicode "충"</figcaption>
<br>

0x가 붙어있는 16진수 값이 보이므로 구글에 검색해보니 Unicode 값임을 확인할 수 있습니다.

<br>
![problem]({{site.url}}/images/Hack6_unicode.png){: class="center"}
<figcaption class="caption">Unicode "충"</figcaption>
<br>
2byte씩 끊어서 정상적으로 Convert되는 값들을 확인해 보면 플래그를 획득할 수 있습니다.

<br>
![problem]({{site.url}}/images/Hack7_exploit.png){: class="center"}
<figcaption class="caption">exploit</figcaption>
<br>

<pre><code class = "language-python">
'''
																|HackCTF{0U].B10C|
    00000010: 30 78 43 44 41 39 30 78  42 44 38 34 30 78 44 37  |0xCDA90xBD840xD7|
    00000020: 38 38 30 64 9a 38 41 43  30 30 30 48 da 38 43 45  |880d.8AC000H.8CE|
    00000030: 35 38 30 78 43 37 38 38  30 78 42 32 39 34 30 78  |580xC7880xB2940x|
    00000040: 43 38 37 34 30 78 43 37  41 43 30 76 e9 38 43 35  |C8740xC7AC0v.8C5|
    00000050: 37 43 7d 63 1f 9b 45 27  61 23 9b 6d b6 25 ca bd  |7C}
'''

data = 'B10C CDA9 BD84 D788 AC00 CE58 C788 B294 C874 C7AC C57C'

data = data.split()
for i in data:
    print (chr(int(i,16)),end='')

</code></pre>
수동 작업이 귀찮으므로 위와 같은 exploit 코드를 작성하면 한글로 된 플래그를 획득할 수 있습니다.

<br>
![problem]({{site.url}}/images/Hack8_result.png){: class="center"}
<figcaption class="caption">Result</figcaption>
<br>

