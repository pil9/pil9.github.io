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

```
int main(int argc, char *argv[])
```

C코드 예제들을 보면 메인 함수(int main) 파라미터 값으로 int argc, char\* argv\[\] 를 많이 볼 수 있습니다.

이는 **실행 파일 옵션**을 받기 위해 쓰이며 GUI 환경보다는 CLI 환경의 프로그램에서 자주 쓰입니다.

즉 파일에 옵션을 부여하여 실행한다는 개념인데 명령어로 예시를 들면 **/와 같이 오는 문자열**로 볼 수 있습니다.

```
ipconfig /all
shutdown /help
```

**int argc** = 프로그램 실행 시 입력한 실행 파일 옵션의 **개수**가 저장됩니다.

**char \*argv** = 프로그램 실행 시 입력한 옵션의 가변적인 개수의 **문자열**들이 저장됩니다.

> char\* : C에는 배열 파라미터 같은 개념이 없기 때문에 포인터로 정의됨



![argv](/_posts\img\argv.png){: class="bigger-image"}
<figcaption class="caption">""으로 묶어주면 띄어쓰기를 포함한다</figcaption>


[##_Image|kage@drUOWX/btqBpPbm5ku/RiL8WR23IyoO60rWnS8jHk/img.png|alignCenter|data-filename="제목 없음.png" data-origin-width="444" data-origin-height="144"|""으로 묶어주면 띄어쓰기를 포함한다||_##]

위와 같이 실행 파일 옵션을 입력해준 경우 argc에는 파일 이름(Test.exe)까지 카운트된 정수 4가 저장되며.

argv 배열에는 {"Test.exe", "Hello", "123", "Hello world"} 형식으로 저장됩니다.

<table style="letter-spacing: 0px; border-collapse: collapse; width: 30%; height: 100px; border-radius: 3px; font-family: Menlo, Consolas, Monaco, monospace;" border="1"><tbody><tr style="height: 20px;"><td style="width: 50%; text-align: center; height: 20px;" colspan="2">argc : 4</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px; text-align: center;">argv[0]</td><td style="width: 50%; height: 20px; text-align: center;">"Test.exe"</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px; text-align: center;">argv[1]</td><td style="width: 50%; height: 20px; text-align: center;">"Hello"</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px; text-align: center;">argv[2]</td><td style="width: 50%; height: 20px; text-align: center;">"123"</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px; text-align: center;">argv[3]</td><td style="width: 50%; height: 20px; text-align: center;">"Hello world"</td></tr></tbody></table>

입력받은 옵션의 개수값이나 문자열들은 아래 코드와 같이 활용이 가능합니다.

{% highlight c %}

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
{% endhighlight %}

[##_Image|kage@bUP2NM/btqBphF10BK/n3heQW1dkU2JTK3CjcYom0/img.png|alignCenter|data-filename="캡처1.PNG" data-origin-width="594" data-origin-height="165"|실행 결과||_##]

주로 C코드에서만 사용되나 파이썬에서도 비슷한 형식으로 옵션값을 받습니다.