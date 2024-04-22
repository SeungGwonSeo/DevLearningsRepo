# Error [ERR_REQUIRE_ESM]: require() of ES Module 관련 이슈


대시보드/홈 화면 구성중에 그래프형태로 UI 를 구성할 일이 있어서 React Chart.js 대신 nivo 를 이용하여 그래프를 그려보려고했다. (공식문서가 좋아보였음.)

router를 이용하여 이동하거나 했을때는 문제가 생기지 않았지만 새로고침을 했을 때 관련 버그가 발생.

찾아보니,
Next.js의 동작 방식과 관련이 있습니다. 그러나 이것만이 원인은 아닙니다. 여러 가지 이유가 있을 수 있지만, 일반적으로 다음과 같은 몇 가지 이유로 서버 측에서 렌더링할 때 문제가 발생할 수 있습니다

브라우저 전용 API 사용: 일부 라이브러리나 컴포넌트는 window, document 같은 브라우저 전용 객체를 사용할 수 있습니다. 서버 측에서는 이러한 객체들이 존재하지 않기 때문에, 해당 객체들을 사용하는 코드를 서버에서 실행하려고 하면 오류가 발생합니다.

CSS 및 애니메이션: 일부 시각적인 라이브러리는 초기 로딩 시 CSS나 애니메이션과 관련된 동작을 수행합니다. 서버에서는 이러한 동작을 수행할 수 없기 때문에 문제가 발생할 수 있습니다.

Canvas나 WebGL 사용: 몇몇 라이브러리는 그래픽을 그리기 위해 Canvas나 WebGL 같은 기술을 사용합니다. 이러한 기술은 서버 측에서는 동작하지 않습니다.

 

그래프를 svg로 그리다 보니 d3.js 에서 문제가 생기는것 같다 (확실치 않음)

[v0.81.0 can lead to import issues · Issue #2310 · plouc/nivo](https://github.com/plouc/nivo/issues/2310#issuecomment-1552663752)  에서 같은 이슈를 




```import { ResponsiveLine } from "@nivo/line";``` 에서

```import dynamic from "next/dynamic";
const ResponsiveLine = dynamic(() => import("@nivo/line").then(m => m.ResponsiveLine), { ssr: false });
```

해서 처리완료.