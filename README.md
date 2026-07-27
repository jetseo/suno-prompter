# 🎵 Suno AI Prompter

Suno 음악 생성 AI를 위한 **선택형 프롬프트 빌더**

## 기능

- 🌍 **7단계 위저드** — 스타일 → 스토리 → 분위기 → 템포 → 악기 → 보컬 → 기타
- 🎸 **지역/국가/장르 계층 선택** — 6개 지역, 30개 국가, 60+ 장르
- 🥁 **BPM 슬라이더** — 직접 입력 또는 8가지 프리셋 선택
- ✨ **변형 프롬프트** — More emotional / energetic / cinematic / minimal
- 🧩 **곡 구조 템플릿** — K-Pop 댄스 / 힙합·R&B / 시티팝 / 발라드 / 모던 록 5종의 `[Intro]→[Verse]→[Chorus]...` 구조 태그를 Suno Lyrics 창에 바로 붙여넣기용으로 복사
- 📋 **클립보드 복사** — 생성된 프롬프트 원클릭 복사
- 📊 **CSV 내보내기** — 선택 내역 엑셀로 저장

## 사용법

1. 7개 탭을 순서대로 선택
2. 우측 패널에서 생성된 프롬프트 확인
3. "클립보드 저장" 버튼으로 복사, 원하면 곡 구조 템플릿도 복사
4. [Suno](https://suno.com) Advanced 모드에서 Lyrics(구조 태그)와 Styles(프롬프트)에 각각 붙여넣기

## 마스터링 툴 (`mastering.html`)

Suno로 뽑은 음원을 브라우저 안에서 바로 다듬는 별도 도구입니다. 서버 업로드 없이 로컬에서만 처리됩니다.

- 🎚 **러프니스(LUFS) 미터** — 원본/처리 후 음압을 VU 미터로 실시간 표시
- 🧹 **고역 노이즈 억제 & AI 아티팩트 제거** — 수노 특유의 메탈릭 노이즈, 보코더/스펙트럴 스파이크 감쇠
- 🎛 **7밴드 EQ + 음색 프리셋** — Clean / Punchy / Warm / Airy / V-Shape
- ↔️ **스테레오 폭 조절**, 🎯 **원클릭 자동 마스터링**, 🔊 **트루피크 리미터**
- 💾 **WAV로 저장** — A/B 비교 후 바로 내보내기

`index.html`과 `mastering.html` 상단에서 서로 링크로 이동할 수 있습니다.

## 로컬 실행

```bash
# 그냥 index.html(프롬프트 빌더) 또는 mastering.html(마스터링 툴)을 브라우저로 열면 됩니다
open index.html
open mastering.html
```

## 배포

Vercel 또는 GitHub Pages로 `index.html`, `mastering.html` 정적 파일 배포

---

Made with ♪ by [jetseo](https://github.com/jetseo)
