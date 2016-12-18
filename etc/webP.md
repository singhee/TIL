# 구글에서 개발한 이미지 포맷 webP

webP라는 포맷은 구글에서 이미지 손실을 최소화 하면서 사이즈를 줄여 다운로드의 시간을 아끼기 위해 만든 이미지 포맷이다. webP는 이미지의 무손실과 손실 압축(lossless and lossy compression)을 제공하고 있다. 무손실 이미지의 경우 같은 화질의 png포멧과 비교했을 때 26% 사이즈가 감소되고 손실 이미지의 경우는 동일한 SSIM index(두 이미지의 비슷한 정도를 비교하는 방법)의 JEPG이미지와 비교했을 때 25%-34%가 감소된다고 한다.

## 장점
- 손실/무손실 압축 지원 (lossless and lossy compression)
- 투명 지원 (alpha channel)
- 색상정보, 메타데이터
- 무료 / 오픈소스
- 애니메이션 지원

## 단점
- 브라우저 지원률 미비
> PC : 크롬, 오페라
> 모바일 : 안드로이드 기본 브라우저, 크롬
> 크롬프레임 플러그인 이용 시 ie7 이후 버전에서 확인 가능
- 브라우저 지원현황 - http://caniuse.com/webp
- 파일 변환 시 JPEG 대비 8배 시간 소요
- 포토샵등 주요 디자인툴에서 지원도 미비함


### webP - Sierra