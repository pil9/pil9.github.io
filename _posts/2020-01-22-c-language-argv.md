---
title: "C언어 argc 그리고 argv"
layout: post
date: 2020-01-22
image: /assets/images/mark-img01.png
headerImage: false
tag:
- C
- argc
- argv
- argument
category: blog
author: pil9
---
 

 

argc = arguments(인수) count

argv = arguments vector

```c
int main(int argc, char *argv[])
```

C코드 예제들을 보면 메인 함수(int main) 파라미터 값으로 int argc, char\* argv\[\] 를 많이 볼 수 있습니다.

이는 **실행 파일 옵션**을 받기 위해 쓰이며 GUI 환경보다는 CLI 환경의 프로그램에서 자주 쓰입니다.

즉 파일에 옵션을 부여하여 실행한다는 개념인데 명령어로 예시를 들면 **/와 같이 오는 문자열**로 볼 수 있습니다.

```c
ipconfig /all
shutdown /help
```

**int argc** = 프로그램 실행 시 입력한 실행 파일 옵션의 **개수**가 저장됩니다.

**char \*argv** = 프로그램 실행 시 입력한 옵션의 가변적인 개수의 **문자열**들이 저장됩니다.

> char\* : C에는 배열 파라미터 같은 개념이 없기 때문에 포인터로 정의됨

<br>
![argc]({{site.url}}/images/argv.png){: class="center"}
<figcaption class="caption">""으로 묶어주면 띄어쓰기를 포함한다</figcaption>
<br>


위와 같이 실행 파일 옵션을 입력해준 경우 argc에는 파일 이름(Test.exe)까지 카운트된 정수 4가 저장되며.

argv 포인터가 가리키는 배열에는 {"Test.exe", "Hello", "123", "Hello world"} 형식으로 저장됩니다.


<table class="tg center" style="undefined;table-layout: fixed; width: 210px;">
<colgroup>
<col style="width: 94px">
<col style="width: 116px">
</colgroup>
  <tr>
    <th class="tg-5k8v" colspan="2">argc : 4</th>
  </tr>
  <tr>
    <td class="tg-5k8v">argv[0]</td>
    <td class="tg-5k8v">"Test.exe"</td>
  </tr>
  <tr>
    <td class="tg-jzjz">argv[1]</td>
    <td class="tg-jzjz">"Hello"</td>
  </tr>
  <tr>
    <td class="tg-jzjz">argv[2]</td>
    <td class="tg-jzjz">"123"</td>
  </tr>
  <tr>
    <td class="tg-jzjz">argv[3]</td>
    <td class="tg-jzjz">"Hello world"</td>
  </tr>
</table>
  
<br>
입력받은 옵션의 개수값이나 문자열들은 아래 코드와 같이 활용이 가능합니다.

<!--{% highlight c %}
{% endhighlight %}-->

```c
#include <stdio.h>

int main(int argc, char* argv[]) {

	if (argc == 1) {
		printf("실행 파일 옵션을 입력해 주세요");
		//프로세스 종료
	}
	else {
		printf("입력된 옵션은 %d개입니다\n", argc - 1);
               //프로그램명(상대경로) 또는 절대경로까지 카운트 되므로 -1

		printf("입력한 옵션 목록\n");
		for (int i = 1; i < argc; i++) {
			printf("%s\n", argv[i]);
		}
	}
}
```

<br>
![argc]({{site.url}}/images/argv_result.png){: class="center"}
<figcaption class="caption">실행 결과</figcaption>
<br>

주로 C코드에서만 사용되나 파이썬에서도 비슷한 형식으로 옵션값을 받습니다.