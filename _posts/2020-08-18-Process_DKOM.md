---
title: "Direct Kernel Object Manipulation"
layout: post
date: 2020-08-16
image: /assets/images/mark-img01.PNG
headerImage: false
tag:
- Memory
- Forensics
- Rootkit
category: blog
author: pil9

---
 

본 포스팅에서는 루트킷이 자신 프로세스를 은닉할 때 사용되는 기법인 DKOM(Direct Kernel Object Manipulation) 기법에 대해 알아보겠습니다.

---
<br>
DKOM 기법은 커널모드(Ring0)에서 실행되는 대표적인 은닉 기법입니다. 커널 레벨에서는 Kernel-Mode Object Manager를 통해 각각의 Object 객체(Device, Driver, EProcess, EThread 등)를 관리하는데 DKOM은 Object Manager를 거치지 않고 직접 커널 Object에 접근합니다. 따라서 권한 체킹이 발생하지 않습니다.


---
<br>
![problem]({{site.url}}/images/200818/DKOM_non.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
커널에서는 [EPROCESS](https://docs.microsoft.com/en-us/windows-hardware/drivers/kernel/eprocess) 구조체를 생성하여 프로세스에 대한 모든 정보를 관리합니다.
EPROCESS 구조체 중에는 ActiveProcessLinks라는 구조체가 있는데 해당 구조체는 Flink(Forward-link)와 Blink(Back-link)로 구성되어져 있습니다. 위 두가지 링크는 다른 프로세스들의 ActiveProcessLinks 구조체의 Flink, Blink를 가리킵니다.


---
<br>
![problem]({{site.url}}/images/200818/DKOM.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
DKOM 기법은 커널 디버깅을 통해 악성 프로세스가 리스트에서 제외될 수 있도록 Flink 와 Blink 값을 수정합니다.

---
<br>
![problem]({{site.url}}/images/200818/dbg.png){: class="center"}
<figcaption class="caption"></figcaption>  
<br>
실습내용

---
