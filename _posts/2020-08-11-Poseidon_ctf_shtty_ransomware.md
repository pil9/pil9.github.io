---
title: "Poseidon CTF sh*tty_Ransomware"
layout: post
date: 2020-08-11
image: /assets/images/mark-img01.PNG
headerImage: false
tag:
- ctf
- Digital forensics
category: blog
author: pil9

---
 
![problem]({{site.url}}/images/poseidon/0.PNG){: class="center"}
<figcaption class="caption">문제</figcaption>  
<br>
발번역 : 멍청한 내동생은 아주 작은 크기의 게임을 찾다가 랜섬웨어를 다운로드 했고 파일이 암호화 되었다.
랜섬웨어 C&C 서버 주소를 찾아라..

포세이돈 CTF 포렌식 3번 문제입니다.

---
 
![problem]({{site.url}}/images/poseidon/1.png){: class="center"}
<figcaption class="caption">Volatility imageinfo</figcaption>  
<br>
문제에 첨부된 파일(ShittyRansom.raw)은 메모리 덤프 이미지 파일이므로 Volatility 2.6을 사용해서 분석을 진행했습니다. 이미지 프로파일 확인을 위해 imageinfo 명령어를 사용해줍니다.

---
<br>
![problem]({{site.url}}/images/poseidon/2.png){: class="center"}
<figcaption class="caption">Process,Network info</figcaption>  
<br>
랜섬웨어 관련 IP&PORT (C&C) 정보를 찾아야 하므로 현재 랜섬웨어 프로세스가 활성화 되어있는지, 있다면 네트워크 활성 정보가 있는지 확인하였으나 특이점을 발견하지 못 했습니다.(pstree, netscan)

---
<br>
![problem]({{site.url}}/images/poseidon/3.png){: class="center"}
<figcaption class="caption">Process command line</figcaption>  
<br>
또한 프로세스 별 커맨드라인 정보를 확인하였으나 악성 커맨드가 포함된 프로세스는 존재하지 않았습니다.(cmdline, cmdscan)

---
<table class="tg center" style="undefined;table-layout: fixed; width: 210px;">
<colgroup>
<col style="width: 94px">
<col style="width: 116px">
</colgroup>
<style type="text/css">
.tg  {border-collapse:collapse;border-color:#aaa;border-spacing:0;}
.tg td{background-color:#fff;border-color:#aaa;border-style:solid;border-width:1px;color:#333;
  font-family:Arial, sans-serif;font-size:14px;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{background-color:#f38630;border-color:#aaa;border-style:solid;border-width:1px;color:#fff;
  font-family:Arial, sans-serif;font-size:14px;font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
.tg .tg-7btt{border-color:inherit;font-weight:bold;text-align:center;vertical-align:top}
</style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky"></th>
    <th class="tg-7btt">History</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-7btt">IE(Win7)</td>
    <td class="tg-c3ow">%Profile%\AppData\Local\Microsoft\Windows\WebCache\WebCacheV01.dat</td>
  </tr>
  <tr>
    <td class="tg-7btt">Chrome</td>
    <td class="tg-c3ow">%Profile%\AppData\Local\Google\Chrome\User Data\Default\History</td>
  </tr>
</tbody>
</table>

<br>
게임으로 위장한 랜섬웨어를 인터넷에서 받았을 확률이 높으므로 제일먼저 IE History 와 Chrome History 정보를 확인하였습니다.

---
<br>
![problem]({{site.url}}/images/poseidon/4.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
History 파일 추출을 위해 Mft 엔트리 정보(mftparser)를 확인하고

---
<br>
![problem]({{site.url}}/images/poseidon/5.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
filescan을 통해 메모리 풀의 Datasection offset 주소를 확인한 뒤

---
<br>
![problem]({{site.url}}/images/poseidon/6.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
dumpfiles 플러그인으로 filescan을 통해 구한 물리 오프셋 주소를 기반으로 history 파일을 추출하였습니다.

---
<br>
![problem]({{site.url}}/images/poseidon/7.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
추출된 history 데이터를 [ChromehistoryView](https://www.nirsoft.net/utils/chrome_history_view.html)를 통해 확인하면 일부 접속 기록을 확인할 수 있습니다.


---
<br>
![problem]({{site.url}}/images/poseidon/8.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
접속 시간 순으로 분석해보면 pro evolution soccer 2010 gameplay 구글검색 -> PES 2010 PC Gameplay HD 유튜브 시청 -> pro evolution soccer 2010 light download 구글검색 -> pastebin 사이트 방문 -> 구글드라이브 접속 으로 확인할 수 있습니다.

---
<br>
![problem]({{site.url}}/images/poseidon/9.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
해당 pastebin 주소에 접속하면 구글드라이브 주소와 압축파일 비밀번호등이 적혀있습니다.
구글드라이브에 업로드된 파일이 랜섬웨어가 맞다면 랜섬웨어 최초 유포지로 유추할 수 있습니다.

---
<br>
![problem]({{site.url}}/images/poseidon/10.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
해당 구글드라이브에 접속하면 실행 파일과 DLL 파일이 업로드 되어있고 pastebin에 적혀있던 암호(pes10)로 압축 해제가 가능합니다.

---
<br>
![problem]({{site.url}}/images/poseidon/11.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
VirusTotal을 통해 Malware.exe을 검사한 결과 별다른 악성 행위는 하지 않는 것으로 유추할 수 있습니다.

---
<br>
![problem]({{site.url}}/images/poseidon/12.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
C&C 서버 접속 여부를 확인하기 위해 Wireshark 네트워크 패킷 캡처를 통한 동적 분석을 진행합니다.

---

<br>
![problem]({{site.url}}/images/poseidon/13.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
Malware.exe 실행 결과 23.97.198.147:32841 호스트 주소로 HTTP 통신을 하는것을 확인할 수 있습니다.

---
<br>
![problem]({{site.url}}/images/poseidon/14.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
최종적으로 해당 주소에 접속해보면 Flag가 출력됩니다.

---

<br>
![problem]({{site.url}}/images/poseidon/15.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
++추가
superponible의 [chromehistory](https://github.com/superponible/volatility-plugins) Vol 플러그인을 사용하면 보다 쉽게 크롬 접속 기록 추출이 가능합니다.

---