https://blossomwhale.tistory.com/53


BlossomWhale
메뉴
header과 cpp파일의 include 관계
2020. 10. 15. 20:04ㆍ컴퓨터 수업/C++

header과 cpp파일의 include 관계.
빨간 화살표는 포함(include) 관계를 의미한다. 머리 부분이 꼬리 부분을 포함(include) 한 것이다.

(ex. main.cpp 에는 #include "SortedType.h"가 있다.

 

 


실제로 그 파일이 기입되는것이라 생각하면 된다.
다음 두 코드는 다르다.

 

#include "SortedType.h"
int main()
{
SortedType stack;
}
int main()
{
SortedType stack;
}
#include "SortedType.h"
 

위의 것은 SortedType.h가 전방선언 되었고, 밑의 것은 후방선언 되었기에, 밑의 main함수에서는 "물리적으로" SortedType.h의 존재여부를 알 방법이 없다.

Common rule
포함(include)는 cpp가 header를 include 하는식으로 진행된다.

 

즉,

 

main.cpp 에서 include를 사용할 때에는

 

#include "xxx.h" 만 포함해야지

#include "xxx.cpp"를 포함하지는 않아야된다. 

Cpp파일은 어느 header까지 접근 가능한가?
우선 명시적으로 포함(include)한 header들은 당연히 접근 가능하다.

따라서, 위의 경우에 main.cpp는 SortedType.h에 접근 가능하다.

(SortedType.cpp에 접근 가능한지 여부는 밑에서 서술하겠다.)

또한, header끼리의 포함관계에 따라서 접근 가능한 header가 달라진다.

예를들어, 위의 그림에서 SortedType.h는 ItemType.h를 포함(include)하고 있으므로,

물리적으로 SortedType.h에는 ItemType.h가 공존한다. 따라서, 그런 SortedType.h를 include한 main.cpp는 당연히 ItemType.h에도 접근이 가능하다.

Cpp파일은 어느 cpp까지 접근 가능한가? ★
답부터 말하자면, 같은 header를 공유한다면 접근 가능하다.

같은 header를 공유한다면 source code로는 접근 못하지만 obj파일은 접근 가능하다.

다른 포스팅에서도 밝힌 바 있지만, 빌드라는 행위는

전처리기 -> 컴파일러 -> 링커의 도움을 받는데, 여기서 #include "xxx.h"는 전처리기가 처리해준다.

하지만 같은 header를 공유하는지의 여부는 링커가 도와주는데, 컴파일 이후 발생된 object파일을

바이너리 파일로 바꿔 주기 위해서는 같은 header를 공유하는 cpp파일들을 링킹 해야한다.

 

위의 그림을 보면, main.cpp는 SortedType.h라는 헤더파일을 갖고 있고,

SortedType.cpp또한 SortedType.h라는 헤더파일을 갖고있다. 두 cpp파일이 하나의 header파일을

공유하니, 두 cpp파일들은 서로 access가 가능하다.

두 cpp파일은 서로 각 cpp파일의 사용자 지정 함수들에 접근할 수 없다. 엄밀히 말하자면 해당 식별자를 각 cpp파일에서 서로 찾을 수 없을 것이다.

(예를들어, main.cpp파일에

void Cigarette()
{
    return;
}
를 정의한다해도, SortedType.cpp에서는 Cigarette이란 식별자를 찾을 수 없다고 한다.

(역도 마찬가지이다.))

 

또한, ItemType을 갖고 있는 cpp파일은 main , SortedType, ItemType / cpp 파일들이지만

ItemType에 사용자지정함수 rain()을 정의한다고 해도 ItemType.cpp 외에는 그 누구도 이 사용자 지정 함수를 사용 할 수 없다.

 그렇다면 어떻게 main.cpp에서 SortedType stack을 선언 후 사용가능한가?
엄밀히 말하자면 Compile Time엔 main.cpp에서 SortedType의 생성자를 알 방법이 없다.

하지만, 각 cpp파일들이 compile되고, 이 obj 파일들이 링킹될때가 되서야 main.cpp의 생성자를 알 수 있는 것이다.

 

이부분은 잘 모르지만, 아마 main함수에서는 StackType의 생성자가 존재할것이라 생각하고 미리 인터페이스처럼 구현될 것 같다.

 

이런 문제에 의해 template class의 경우에는 header와 cpp를 구분하면 에러가 발생하는 경우가 존재하는 것 같다.

#Pragma once
cpp파일에서 header만 include 시킨다면, 결국 나 (User)가 source code를 작성할 때, header를 포함할때, 다음과 같이 생각한다면 어떤 cpp에 결국 접근할 수 있을지 알 수 있다.

 

main함수를 작성하다가, SortedType stack을 사용하고 싶다,

#include "SortedType.h"

UnsortedType stack을 사용하고 싶다.

#include "UnsortedType.h"

ItemType temp_item 을 사용하고 싶다.

#include "ItemType.h"을 사용하면 된다.

 

그런데, SortedType.h에 ItemType.h가 포함되어 있으므로, 이에 대해서 ItemType.h 재정의 문제가 발생하게 된다.

 

위의 include를 풀자면 ,

#include "SortedType.h"

#include "ItemType.h"

#include "UnsortedType.h"

#include "ItemType.h" 라는 문제가 발생하는 것이다.

 

따라서 이 문제를 처리하기위해, #ifndef 나 #pragma once를 사용하여 한번만 처리하도록 하는것이다..

 

그래서 include는 뭐하면 되는데?
그냥 본인이 원하는 헤더 싹다 포함시키고 #pragma once를 사용하는것이 가장 현실적이다.

 

그리고 #pragma once는 header에만 기입한다.

 


좋아요1
공유하기게시글 관리구독하기
TAG
pragma once #include #header #전처리기 #링커 #링킹 #컴파일 #빌드
관련글
대입연산자(=)의 오버로딩 시, referenece를 반환하는 이유
2020.10.16
lambda function
2020.10.15
지역 포인터
2020.10.14
Function Pointer
2020.10.02
댓글
이름
비밀번호
댓글을 입력해주세요.
 비공개 댓글 남기기
 1 ··· 32 33 34 35 36 37 38 39 40 ··· 78 
티스토리
© 2018 TISTORY. All rights reserved.
