---
title: "Apple 사칭 피싱메일 - iPad라는 iPad mini에서 iCloud에 로그인하는 데 엑세스했습니다"
layout: post
date: 2020-02-15
image: /assets/images/mark-img01.png
headerImage: false
tag:
- phishing
- hacking
- mail
category: blog
author: pil9
---

Apple을 사칭한 해킹메일이 유포되고 있어 각별한 주의가 필요합니다.

금일 실제로 네이버 메일로 온 **“iPad”라는 iPad mini에서 iCloud에 로그인하는 데 액세스했습니다** 의 메일을 분석하였습니다.

[##_Image|kage@dqg9Ta/btqB1nSM430/xiyhilz8fj30PokLsX2eFk/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|전형적인 피싱사기 메일||_##]

메일주소 별명을 Apple社 도메인으로 사용하고 있으며 Apple社의 iCloud 계정 보안 위협을 문제로 본문에 링크를 삽입하여 클릭을 유도하고 있습니다.

실제 애플사의 보안 경고 메일 형식을 그대로 따라하고 있어 언어 번역 간 맞춤법 등의 문제가 없기 떄문에

iCloud를 자주 사용하거나 해킹메일에 대한 경각심이 없는분들은 쉽게 인지하지 못 할 것이라고 판단됩니다.

해당 **_https://www.apple.com_**는 단순 텍스트 값 이고 실제로 걸려있는 하이퍼링크 값은

**_http://ow.ly/J55E50yna3E?idtrack=MzZkAcf0&system=buildint_**으로 되어있습니다.

또한 해당 하이퍼 링크값은 URL단축(ow.ly)이 이루어져 있습니다.

[##_Image|kage@dHT6Ty/btqBZr9L5fH/fVRlckQYAgiXAuQDGUOrN1/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|디코딩된 URL||_##]

해당 **_http://ow.ly/J55E50yna3E_** 주소를 통해 _**_https://secureloginco-kr.servequake.com/_** 주소로 리다이렉트 되며 애플 계정 관리페이지(**https://appleid.apple.com/**_)를 모방한 피싱 사이트로 접속됩니다.

[##_Image|kage@b0iqIM/btqB1HjdHiz/yvWK6cmmnyUbktsXBmAAeK/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|APPLE 계정관리 페이지와 100%일치하는 피싱사이트||_##]

중앙의 ID/PW 입력 폼 이외의 배너들은 작동하지 않으며 ID/PW값 입력 시 ajax 메소드를 통해 입력한 계정 정보가 truelogin.php로 값이 전송됩니다.

[##_Image|kage@bVlEGU/btqBZgHmwh2/slpsLpFC4kRoln2gAJdqVK/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|피싱사이트 js 코드||_##]

또한 정상적으로 값이 전송되었을 경우 **_https://secureloginco-kr.servequake.com/account/files/locked.php?country=KR_**으로 리다이렉트 됩니다.

[##_Image|kage@Og26m/btqBZqCXG3o/j2cpkSZKcLHmXXz4dqyEJ0/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|locked.php||_##]

계정 확인버튼 클릭 시 추가 계정 정보를 입력하는 폼이 출력됩니다.

[##_Image|kage@Fq2Qk/btqBZfIvcJU/zyQu6slCwmPQFSuGBRwTtK/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|추가 계정 정보 입력||_##]

이름, 생일, 전화번호, 주소, 신용카드 정보등을 입력받고 있으며 실제 결제 모듈에서 사용되는 Payment 관련 스크립트를 사용하고 있어 카드번호 일치여부등을 확인하게 구성되어 있습니다.(공식 사이트와 매우 유사함)

[##_Image|kage@CdMmJ/btqBZrhyERo/RJQX51JvxuH86RxTvCVr9K/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|올바른 정보만 전송되도록 작성된 일부 코드||_##]

생일 형식과 카드번호 자릿수 및 해외 결제 카드 종류(VISA,MasterCard, JCB, AMEX 등)를 검증하며 올바른 값이 입력되면 Continue 버튼이 활성화 됩니다.

Continue 버튼 클릭 시 _**https://secureloginco-kr.servequake.com/account/submit.php**_ 로 리다이렉트 되며 입력된 정보에 기반된 카드의 비밀번호를 묻는 폼이 출력됩니다.

[##_Image|kage@bnszLF/btqB1GxQBPt/XH2ckgS75QWwS5qfKxhU51/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|비밀번호 탈취 폼||_##]

비밀번호 제출 시 계정을 복구한다는 말과 함께 애플 공식 계정관리 사이트  
(_**https://appleid.apple.com/#!&page=signin**_)로 리다이렉트 됩니다.

[##_Image|kage@bl5XOc/btqB0e90GI6/mIRlKplZJwwKXAMHVgWPo0/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|계정 활성을 알리는 가짜 메세지||_##]

마지막으로 해당 피싱사이트 주소로 재 접속 시 서버에 접속할 수 있는 권한이 없다고 표시됩니다.

[##_Image|kage@cWKnJ6/btqB1cRxHgE/KBP2qs7uKzYKXb7YqKTLgk/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|접근차단||_##]

**위와같은 피싱사기를 예방하기 위해서는..**

1\. **크롬 브라우저**를 통한 구글 세이프 기능을 사용하여 사이트 접속 전 경고 메세지를 표시하도록 해야합니다.

[##_Image|kage@b3affU/btqB0falGNr/uLG3FJTP9ZWKZ9AlevYoqk/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|구글 세이프 브라우징 기능||_##]

2\. **발신자의 메일 주소**를 확인하는 습관을 가져야 하며 특정 모바일 앱에서는 메일 주소 별명만 표기되고 전체 주소는 표시되지 않는 경우가 있으니 유의해서 체크해야 합니다.

[##_Image|kage@oKosp/btqB1mGIg2J/AyheoYNG0nsK31NguyLkfk/img.png|alignCenter|data-origin-width="0" data-origin-height="0"|예시)메일 주소 변경된 스팸 메일에 대한 네이버 메일 주의 문구||_##]

3\. 메일에 파일이 첨부되어있을 경우 **확장자**를 반드시 확인해야 합니다. 악성코드를 다이렉트로 실행시킬 수 있는 확장자는 _**exe, hwp, xls, bat, jar, lnk, scr**_ 등이 있습니다.

4\. 본문 포스팅과 같이 **링크**가 첨부되었을 경우엔 실제 이동되는 주소가 **공식 사이트 주소**인지 반드시 확인해야 합니다.

