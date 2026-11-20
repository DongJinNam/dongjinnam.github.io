---
title: "dev-team-skills 개발 이력: v1.2.0까지의 아키텍처 변천사"
author: Dongjin Nam
pubDatetime: 2026-11-19T18:00:00+09:00
featured: false
draft: false
tags:
  - dev-team-skills
  - changelog
  - ai-agent
description: "dev-team-skills가 초기 멀티 에이전트 프롬프트 모음에서 6-Owner 스쿼드, 결정론적 검증 하네스, PR 기반 릴리즈 플로우(v1.2.0)로 발전한 마일스톤을 정리합니다."
---

[`dev-team-skills`](https://github.com/DongJinNam/dev-team-skills)는 코딩 에이전트(Claude Code, Codex, Antigravity, Cursor) 위에서 동작하는 **6-Owner 엔지니어링 스쿼드와 결정론적 검증 하네스** 프로젝트입니다.

프로젝트가 커지면서 단순히 스킬 문서를 늘리는 대신, **"에이전트가 실행하기 전에 패키지 무결성을 기계적으로 증명하는 구조"**로 방향을 잡았습니다. `v1.2.0`까지의 주요 마일스톤을 커밋·PR 이력 기준으로 정리합니다.

## 마일스톤 요약

| 시기 | 주요 PR | 핵심 변화 |
| --- | --- | --- |
| 2026-11-02 ~ 11-04 | `#13` ~ `#17` | Bash·PowerShell 패키지 검증 통일, 6개 판단 회귀시험 추가, 다수결에서 책임자 결정 체계로 전환, 토큰 사용량 비교 검증기 도입 |
| 2026-11-06 ~ 11-08 | `#18` ~ `#21` | **15개 역할 → 6-Owner Lean Squad 통합**, Windows 인코딩·UTF-8 BOM 결함 수정(스킬 발견 7/8→8/8), 4-arm 파일럿 실측 |
| 2026-11-08 | `#22` ~ `#28` | 독립 AI 보안 리뷰어 운영 모델 수립, 코드화된 하네스 불변식(`SKL-001~004`, `CAT-001~002`, `MIR-001`) 도입 |
| 2026-11-11 ~ 11-14 | `#30` ~ `#36` | `engineering-squad` 작업 시간·모델 라우팅 계약 추가, 보안 패킷 자동 초안(Phase 1) 및 캡처 갱신 런북(Phase 2) 구축 |
| 2026-11-15 ~ 11-16 | `#41` ~ `#43` | **`v1.2.0` 릴리즈**: PR 기반 릴리즈/체인지로그 플로우, 영문 정본 + `SKILL_ko.md` 단일 소스 미러링(`scripts/adapt.py`), 19/19 eval 캡처 갱신 |

## 이 블로그 개편에도 6-Owner 체계를 적용했습니다

이 블로그(`dongjinnam.github.io`) 역시 2022년 구형 Jekyll 템플릿에서 **AstroPaper**로 전면 개편하면서 `dev-team-skills`의 6-Owner 역할을 그대로 적용했습니다.

- **Tech Lead**: AstroPaper 스택 선정, 한국어 OG 폰트 제약 파악(`dynamicOgImage: false`), `humanizer-kr` 기준 본문 교정
- **Product Lead**: "AI 에이전트 엔지니어링 로그" 포지셔닝 및 `engineering-squad` 개선기 1호 시리즈 기획
- **Engineering Lead**: AstroPaper 설정, 페이지/포스트 마이그레이션, Pagefind 검색 및 RSS 구성
- **Quality Lead**: `astro check` + 프로덕션 빌드 + Pagefind 인덱싱 + 생성 결과물 검증
- **Delivery Lead**: 기존 Jekyll 이력을 `archive/legacy-jekyll` 브랜치로 보존하고 GitHub Actions 기반 Pages 배포 구축
- **Security Review Engineer**: 레거시 폴더에 남아 있던 2020년 `.ppk` 키 파일 노출 식별·제거 및 Actions 최소 권한(`contents: read`, `pages: write`, `id-token: write`) 검증

앞으로도 `dev-team-skills`의 주요 릴리즈와 개별 스킬 개선 사례를 이 블로그에 실측 근거와 함께 기록할 예정입니다.
