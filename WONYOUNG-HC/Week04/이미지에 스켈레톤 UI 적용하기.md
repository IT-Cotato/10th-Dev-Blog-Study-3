## 스켈레톤(Skeleton) UI란?

스켈레톤 UI라 하면 **데이터를 가져오고 콘텐츠가 화면에 렌더링 되기 이전까지 로딩 중임을 나타내는 컴포넌트**이다.
Skeleton을 사전에서 찾아보면 '뼈대'라고 나오는데, 여기서 뼈대의 의미는 콘텐츠는 아직 보여주지 않지만 콘텐츠가 어느 위치에서 어떤 크기로 보이는지 윤곽을 나타내는 의미이다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc7RGnZ%2FbtsKPgQWpNZ%2FVs7T5UnwYz0GmV6hHiPI30%2Fimg.png">

위의 이미지는 유튜브에서 동영상 정보와 썸네일 이미지를 불러오기 이전까지 스켈레톤 UI를 보여주고 있는 화면이다.
물론 일반적인 환경에서 유튜브에 접속하면 스켈레톤이 너무 빨리 지나가서 눈치를 못 채고 지나갈 수도 있다. (본인도 스켈레톤에 대해서 알아보기 전 까지는 유튜브에서 이를 적용하고 있는지다 몰랐다.....)

**유튜브 외에도 많은 사이트에서 스켈레톤 UI를 적용하고 있는데, 왜 스켈레톤 UI를 적용해야 할 까?** 화면에 표시될 데이터를 불러오고, 데이터를 화면에 렌더링 하기까지 과정이 마치고 나서 사용자는 비로소 콘텐츠를 확인할 수 있다.
이 과정에서 화면에 표시되는 정보가 없이 사용자는 빈 화면을 보게 된다.
이때 스켈레톤 UI를 적용하면 콘텐츠가 표시되기 이전에 사용자는 화면이 어떻게 구성되어 있는지를 미리 확인할 수 있고, 페이지가 로딩되고 있다는 시각적인 피드백을 받을 수 있다.

하지만 브라우저에서 사용되는 대부분의 컴포넌트들은 거의 즉각적으로 렌더링이 된다.
그럼에도 불구하고 위와 같이 많은 사이트에서 스켈레톤 UI를 적용하는 이유는 무엇일까? 바로 **이미지의 경우**이다.

## 이미지에서 스켈레톤 UI를 적용하지 않은 경우

<img src="https://blog.kakaocdn.net/dn/XP5wP/btsKPbh1Sh3/m1YDrVK9WR9WtR8EipLmc0/img.gif">

위의 이미지는 현재 코테이토 동아리 페이지에서 세션 기록을 보여주는 페이지이다.
각 카드마다 대표 이미지를 한 장씩 가지고 있다.
(사이트 링크 : [https://www.cotato.kr/session?generationId=6](https://www.cotato.kr/session?generationId=6))

이미지가 있는 페이지에서 스켈레톤을 적용하지 않으면 위와 같이 **이미지가 렌더링 되는 과정이 브라우저에 쌩으로 표시된다.**
극단적인 예시를 위해 네트워크 상태가 좋지 않음을 가정하고 테스트한 결과이지만, 정상적인 경우에서 테스트를 해도 이미지가 렌더링 되는 과정을 볼 수 있다.

개발 과정에서 카드의 개수가 많지 않은 상태에서 테스트를 했을 때는 이미지가 빠르게 렌더링 되어 문제점을 발견하지 못했지만, 실제 운영 환경에서 카드의 개수가 많아져 렌더링해야 하는 이미지의 개수가 많아지니깐 이미지 렌더링의 문제점이 발생하게 되었다.

이를 해결하기 위해서 이미지 지연 로딩이나 여러 가지 이미지 최적화 방법을 적용해 봤지만, 근본적으로 이미지가 브라우저에서 렌더링 되는 시간은 필연적으로 발생하기에 위와 같이 이미지가 버벅거리면서 화면에 표출되는 현상을 해결하지는 못했다.
그래서 선택한 해결책은 **스켈레톤을 적용해서 이미지가 렌더링 되는 과정을 화면에 표시하지 않는 것이었다.**

## 스켈레톤을 적용하는 과정 

해당 사이트는 리액트를 통해 개발하고 있고, 본문에서 사용되는 라이브러리는 styled-components와 MUI이다. 코드의 경우 실제 코드에서 많은 부분을 생략해서 설명을 하겠지만, 동작하는 로직은 동일하게 작성했다.

### 스켈레톤을 적용하지 않은 코드

```tsx
import React from 'react';
import styled from 'styled-components';

//
//
//

interface CardProps {
  image: string;
}

//
//
//

const Card = ({ image }: CardProps) => {
  return (
    <Container>
      ...
      {
        isImageLoaded ? <CardImage src={image} alt="session" /> : <Sceleton />;
      }
      ...
    </Container>
  );
};

//
//
//

const Container = styled.div`
...
`;

const CardImage = styled.img`
...
`;

export default Card;
```

카드에서 이미지 외에도 다양한 정보들이 있지만, 코드의 설명을 간략하게 하기 위해서 이미지 외의 코드는 포함시키지 않았다. Card의 상위 컴포넌트에서 map 함수를 통해서 렌더링 한다.

```tsx
return images.map((image) => <Card image={image} />);
```

현재 상태로 코드를 실행시키면 아까와 같이 이미지가 렌더링 되는 과정이 브라우저에서 그대로 표출된다.

### 이미지가 렌더링이 완료되기 전까지 스켈레톤을 보여주기

스켈레톤을 적용하기 위해서는 이미지가 렌더링이 완료되는 시점을 알아야 한다. 그럼 코드에서 이미지가 완전히 렌더링이 된 시기는 어떻게 알 수 있을까?

https://ko.react.dev/reference/react-dom/components/common#common

리액트 공식문서를 확인해 보면 아래와 같은 설명을 찾을 수 있다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbpZfsD%2FbtsKPcgVwLL%2FQJb0K9rRCFKWJOk8Kaw651%2Fimg.png">

img 태그의 경우 **onLoad 이벤트**가 발생하는데, 이는 img 태그에서 이미지가 렌더링이 완료된 경우 호출된다. 그러면 이미지 렌더링이 완료된 이후 코드를 제어하는 것이 가능해졌다.

그러면 코드가 처음 실행될 때는 스켈레톤을 이미지 자리에 먼저 보여주고, 이미지가 렌더링이 완료되면 이미지를 화면에 보여주면 스켈레톤을 적용할 수 있겠다!

```tsx
import React, { useState } from 'react';
import styled from 'styled-components';
import Skeleton from '@mui/material/Skeleton';

//
//
//

interface CardProps {
  image: string;
}

//
//
//

const Card = ({ image }: CardProps) => {
  /**
   * 이미지가 렌더링이 되었는지를 저장하는 state
   * 코드가 최초로 실행되는 시기는 이미지가 렌더링을 시작한 시기이니 true로 초기화
   */
  const [isImageLoaded, setIsImageLoaded] = useState(true);

  return (
    <Container>
      ...
      {
        /* 이미지가 로딩중인 경우에는 스켈레톤을 표출 */
        /* 이미지가 로딩이 완료된 시점부터는 이미지를 표출 */
        isImageLoaded ? (
          <Sceleton />
        ) : (
       	  /* 이미지가 렌더링이 완료되면 onLoad 이벤트를 통해 setIsImageLoaded state를 false로 설정 */
          <CardImage src={image} alt="session" onLoad={() => setIsImageLoaded(false)} />
        );
      }
      ...
    </Container>
  );
};

//
//
//

const Container = styled.div`
...
`;

const CardImage = styled.img`
...
`;

export default Card;
```

스켈레톤 컴포넌트는 직접 구현하지 않고, MUI 라이브러리를 사용했다.

https://mui.com/material-ui/react-skeleton/

1\. 코드가 처음 실행되는 경우 이미지는 당연히 로딩 중이기에 isImageLoaded를 true로 설정

2\. isImageLoaded가 true인 경우에는 이미지가 렌더링이 완료되지 않았기에 스켈레톤을 보여줌

3\. 이미지가 렌더링이 완료되면 onLoad 이벤트가 호출되어 isImageLoaded가 false로 변경

4\. isImageLoaded가 false인 경우에는 이미지 렌더링이 완료되었기에 이미지를 보여줌

이런 순서로 코드가 실행되면 페이지에 처음 접속했을 때는 스켈레톤이 보이고, 어느 정도 시간이 지나면 이미지가 보이겠다고 생각했지만, 실제로 페이지에 접속하면 스켈레톤만 계속해서 보인다.

<img src="https://blog.kakaocdn.net/dn/bI8PGb/btsKQeERpSb/N2KN29RbUgyXASW0rKAO21/img.gif">

이런 현상이 발생하는 원인은 **onLoad 이벤트 자체가 발생하고 있지 않기 때문이다.**

코드를 분석하면 3항 연산자를 통해서 <Skeleton /> 또는 <CardImage /> 둘 중 하나의 컴포넌트만 실행시키고 있다.
처음에는 <Skeleton /> 컴포넌트만 실행을 시키기 때문에 **<CardImage /> 컴포넌트는 실행이 되지 않아 이미지 렌더링 자체가 발생하고 있지 않은 것이다.** 
정확하게는 <CardImage />가 DOM에 mount 되지 않기 때문에 이미지 렌더링 과정이 발생하지 않는 것이다.

### 스켈레톤과 이미지 컴포넌트를 동시에 실행시키기

이미지에 스켈레톤을 적용하기 위해서는 **스켈레톤 또는 이미지 컴포넌트 둘 중 하나만 실행시키는 것이 아니라 둘 다 실행을 시켜야 한다.**
즉 두 컴포넌트 모두 DOM에 mount를 해야 한다.

```tsx
import React, { useState } from 'react';
import styled from 'styled-components';
import Skeleton from '@mui/material/Skeleton';

//
//
//

interface CardProps {
  image: string;
}

interface CardImageProps {
  $display: string;
}

//
//
//

const Card = ({ image }: CardProps) => {
  /**
   * 이미지가 렌더링이 되었는지를 저장하는 state
   * 코드가 최초로 실행되는 시기는 이미지가 렌더링을 시작한 시기이니 true로 초기화
   */
  const [isImageLoaded, setIsImageLoaded] = useState(true);

  return (
    <Container>
      ...
      {
        /* 이미지가 렌더링이 완료되지 않으면 스켈레톤 컴포넌트를 실행 */
        isImageLoaded && <Skeleton />;
      }
      {/* 이미지 컴포넌트는 항상 실행 */
       /* 단 이미지가 렌더링이 완료되지 않으면 display 속성을 none으로 설정하여 화면에 표시되지 않도록 함 */
      }
      <CardImage
        src={image}
        alt="session"
        $display={isImageLoaded ? 'block' : 'none'}
        onLoad={() => setIsImageLoaded(false)}
      />
      ...
    </Container>
  );
};

//
//
//

const Container = styled.div`
...
`;

const CardImage = styled.img<CardImageProps>`
  display: ${({ $display }) => $display};
...
`;

export default Card;
```

스켈레톤과 이미지 컴포넌트를 동시에 실행을 시키되, 이미지 컴포넌트의 경우 **CSS의 display 속성을 이용해서 화면에 표출을 할 것인지 여부를 결정했다.**
이렇게 하면 두 컴포넌트를 모두 실행시키면서 이미지 컴포넌트의 경우 이미지 렌더링이 완료된 시점에만 화면에 표출되게 할 수 있다.
그리고 당연히 이미지 렌더링이 완료되면 더 이상 스켈레톤 컴포넌트는 실행되지 않는다.

<img src="https://blog.kakaocdn.net/dn/nQgr7/btsKNYjIfBR/KpGdqYVHtf8ZErYEKMrNHK/img.gif">

**이렇게 해서 이미지가 렌더링이 되는 과정을 스켈레톤으로 먼저 보여주고, 이미지 렌더링이 완료된 경우에만 화면에 보이도록 구현했다!!**

### 데이터를 받지 않은 상태에서 스켈레톤 적용하기

현재 코드에서는 브라우저가 서버에서 세션에 대한 정보(이미지)를 받으면 그때부터 스켈레톤을 보여주기 시작한다.
그렇기 때문에 페이지에 처음 접속해서 이미지가 렌더링를 시작하기도 이전, 즉 서버로부터 이미지 정보를 받기 이전에는 화면에 아무것도 표출되지 않는다.
그래서 서버에서 세션에 대한 정보를 받기 이전에도 카드 전체에 대해서도 스켈레톤을 적용하면 페이지에 처음 접근해서 서버로부터 응답이 오기 이전에도 카드에 대한 윤곽을 나타낼 수 있을 것이다.

```tsx
import React, { useState } from 'react';
import styled from 'styled-components';
import Skeleton from '@mui/material/Skeleton';

//
//
//

interface CardProps {
  image?: string;
}

interface CardImageProps {
  $display: string;
}

//
//
//

const Card = ({ image }: CardProps) => {
  const [isImageLoaded, setIsImageLoaded] = useState(true);

  return (
    <Container>
      ...
      {
        isImageLoaded && <Skeleton />;
      }
      {
        // image에 대한 정보가 없는 경우 카드 컴포넌트를 실행하지 않음
        image && (
          <CardImage
            src={image}
            alt="session"
            $display={isImageLoaded ? 'block' : 'none'}
            onLoad={() => setIsImageLoaded(false)}
          />
        );
      }
      ...
    </Container>
  );
};

//
//
//

const Container = styled.div`
...
`;

const CardImage = styled.img<CardImageProps>`
  display: ${({ $display }) => $display};
...
`;

export default Card;
```

카드 컴포넌트의 props의 image를 optional로 변경을 해 주어 image에 대한 값이 존재하지 않는 경우 스켈레톤 컴포넌트만 실행되도록 한다.

```tsx
return images
  ? images.map((image) => <Card image={image} />)
  : new Array(12).fill(null).map((_) => <Card />);
```

상위 컴포넌트에서는 images에 대한 정보가 있는 경우(서버로부터 세션 정보를 불러온 경우) 카드 컴포넌트에 이미지 정보를 전달해서 실행시키고, 아직 images에 대한 정보가 없는 경우(서버로부터 세션 정보를 받지 않은 경우) 빈 배열을 생성해서 이미지를 전달받지 못해 스켈레톤만 보여주는 카드 컴포넌트를 실행시킨다.

빈 배열의 크기를 12개로 선정한 이유는 화면 전체가 카드로 채워질 수 있는 개수이다. (한 줄에 6개의 카드가 보이는 화면에서 카드가 2줄이 보여질 수 있도록)

<img src="https://blog.kakaocdn.net/dn/PQmSR/btsKOSXjLfn/l1LevTkUbo1yxmQBFGNJQK/img.gif">

카드에서 이미지 말고 헤더와 푸터에 있는 콘텐츠도 이렇게 세션에 대한 정보가 없는 경우 스켈레톤이 적용되는 모습을 볼 수 있다.
당장 위의 드롭박스에는 서버로부터 정보를 받아오기까지 중간에 아무런 처리가 없기 때문에 렌더링 되는 과정이 보여지는 모습과 상반된다.

## 아직 개선해야 하는 부분

현재 카드 부분만 보면 개선해야 하는 부분이 두 가지 정도가 보인다.

<img src="https://blog.kakaocdn.net/dn/nQgr7/btsKNYjIfBR/KpGdqYVHtf8ZErYEKMrNHK/img.gif">

### 이미지가 렌더링 완료되는 시점이 카드별로 다르다

카드별로 이미지를 각각 렌더링을 하고 있기에 렌더링이 완료되는 시점이 달라 카드별로 이미지가 화면에 보이는 시점이 다르다.
이미지가 렌더링 되는 과정이 보이지 않아 처음 이미지가 버벅거리는 화면보다는 괜찮지만, 이미지가 제각각 화면에 보이기 시작하는 게 어색해 보인다.

이 문제를 해결하기 위해서는 모든 카드 컴포넌트의 isImageLoaded state가 true가 된 시점에 스켈레톤 컴포넌트 실행을 중지시키고 이미지 컴포넌트를 화면에 보여주기 시작하는 제어가 필요하다.

### 타이포그래피에 스켈레톤이 적용되지 않았다

세션에 대한 정보가 존재하지 않는 경우에는 카드 전체에 대해서 스켈레톤이 적용되고, 세션 정보가 존재하는 순간부터 타이포그래피(헤더)는 스켈레톤이 없어 바로 텍스트가 보인다.
아주 잠깐이지만 타이포그래피가 표시되고 폰트가 적용되는 모습을 볼 수 있다.

이 문제는 브라우저에서 폰트를 서버로부터 받아오고 적용이 되기 이전까지는 헤더에는 계속 스켈레톤을 적용하고, 폰트 적용이 된 이후 발생하는 이벤트에서 헤더 부분에 스켈레톤을 제거하고 타이포그래피를 표출하는 로직이 필요해 보인다.

## 스켈레톤을 통해 UX 향상하기

스켈레톤을 적용해서 콘텐츠가 화면에 렌더링 되기 이전에 윤곽을 먼저 보여주는 UI를 적용해 보았다. 그러면 모든 컴포넌트에 스켈레톤을 적용하는 것이 UX를 향상하는데 도움이 될까?

[https://tech.kakaopay.com/post/skeleton-ui-idea/](https://tech.kakaopay.com/post/skeleton-ui-idea/)

위의 카카오페이 기술 블로그에서 소개하는 Progress Indicator 지침을 보면 1초 미만이 소요되는 항목에 애니메이션을 사용하면 주의가 산만해진다고 설명한다.
**콘텐츠 로드가 빠른 시간 안에 이루어지면 오히려 스켈레톤에서 콘텐츠로 전환되는 과정이 UX를 해칠 수 있다.**

실제로 해당 글에서 첨부한 모든 이미지들은 네트워크 성능에 제한을 설정하고 캡쳐를 한 이미지들이다.
바로 위의 문단에서 두 문제점의 경우에 실제 배포 환경에서는 저런 문제점이 있는지 눈치채지 못할 정도이다.
물론 위의 개선점들은 UX를 하락시키는 스켈레톤 적용은 아니지만, 개발 코스트를 고려하면 반드시 적용을 할 필요가 있는가에 대한 생각을 해 볼 필요가 있다.

결론적으로 API 응답 시간과 테스트를 통해 직접 사용성을 확인하고 스켈레톤 적용 여부를 결정하는 게 UX를 향상하는 방법이 될 수 있겠다.
