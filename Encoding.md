인코딩
======

코딩 작업시 사용하는 표준 인코딩은 `UTF-8 without BOM` 인코딩을 사용하되 `UTF-8 with BOM` 인코딩도 부분적으로 허용합니다. `UTF-8` 인코딩은 국가간 상호 운용성이 뛰어난 유니코드의 8비트 구현으로, `ASCII` 인코딩에 대한 하위 호환성을 제공합니다.

그러나 일부 개발 환경에서 `UTF-8 without BOM`을 정상적으로 인식하지 못하는 문제가 있을 수 있습니다.
이 경우 해당 개발 환경의 설정을 바꾸어주어야할 수 있습니다.

## Visual Studio

> Visual Studio 2022 한국어 버전을 기준으로 설명되어있습니다.

비주얼 스튜디오는 기본적으로 `UTF-8 without BOM`에 대한 지원이 꺼져있을 수 있습니다. 해당 설정을 수정하려면 아래의 절차를 따릅니다.

1. `도구` → `옵션` → `텍스트 편집기` → `일반` 순으로 옵션에 접근합니다.
2. `시그니처 없는 UTF-8 인코딩 자동 검색`을 활성화합니다.
![image](https://user-images.githubusercontent.com/6805899/202097246-26f22df5-42f5-4510-b42a-633a783afa31.png)
3. 확인 버튼을 클릭하여 저장합니다.

## 새로운 프로젝트의 인코딩 설정
새로운 프로젝트의 인코딩 설정은 소스파일을 생성할 때 인코딩을 결정할 중요한 요소입니다.<br>
이 설정은 각 언어별로 정의된 `.editorconfig`에 설정되어있습니다.

만약 이 레포지토리의 `.editorconfig`를 사용하지 못한다면 따로 해당 파일을 생성해서 다음과 같이 작성해주시면 됩니다.
```
root = true

[*]
charset = utf-8
```
이제 생성하는 소스파일은 모두 `UTF-8`로 인식합니다.

## 기존 프로젝트에 대한 인코딩 변경
기존 프로젝트의 일부 인코딩이 이미 `UTF-8`이 아닌 인코딩으로 저장되어 있다면 변환 과정이 필요합니다.

해당 과정은 일반 텍스트에서 인코딩을 감지하는 것이 매우 어렵기 떄문에 자동화 도구에 100% 의존해서는 안되며 오류가 있을 수 있습니다.

해당 도구는 MIT 라이센스로 공개된 [EncodeToUTF8](https://github.com/steamb23/EncodeToUTF8)을 사용할 수 있습니다. 2022년 11월 16일 기준으로 해당 도구의 배포 설정 및 설명 작성이 안되어 있기 때문에 관련 내용은 팀 내에 문의해주세요.
