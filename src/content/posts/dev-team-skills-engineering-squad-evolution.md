---
title: "15개 역할을 6개 책임 오너로: engineering-squad 스킬 개선기와 실측 검증"
author: Dongjin Nam
pubDatetime: 2026-11-20T09:00:00+09:00
featured: true
draft: false
tags:
  - dev-team-skills
  - ai-agent
  - engineering-squad
  - harness
description: "dev-team-skills의 핵심 스킬 engineering-squad를 15개 역할에서 6개 책임 오너(Lean Squad)로 통합하고, 4-arm 파일럿과 결정론적 하네스로 검증한 과정을 정리합니다."
---

AI 코딩 에이전트에 역할 기반 워크플로를 얹을 때 흔히 빠지는 함정이 있습니다. 역할을 세분화할수록 품질이 올라갈 것 같지만, 실제로는 **책임 경계가 흩어지고 문서 유지비가 커집니다.**

[`dev-team-skills`](https://github.com/DongJinNam/dev-team-skills)의 핵심 스킬인 `engineering-squad`를 **15개 역할명에서 6개 책임 오너(Lean Squad)**로 재설계하고([#18](https://github.com/DongJinNam/dev-team-skills/pull/18)), 4개 조건 파일럿과 결정론적 검증 하네스로 실측한 과정을 정리합니다.

## 1. 문제: 15개 역할 체계에서 드러난 병목

초기 `engineering-squad`는 전문 역할 14개와 Lead 1개(총 15개 역할명, Tier 1/2 결정 책임자 8개)로 구성되어 있었습니다. 선택 호출 구조였지만 실제 운영을 점검하면서(기준 커밋 `8fd545f`) 다음과 같은 구조적 문제가 확인되었습니다.

| 기존 영역 | 확인된 문제 |
| --- | --- |
| QA / Test | 기준 설계와 실행·회귀 인계가 두 역할로 나뉘어 AC-테스트-증거 추적이 끊김 |
| DevOps / SRE | 배포와 복구·관측 소유권이 분산되어 기존 서비스 설정 변경 경로가 불명확함 |
| Planner / Marketing / Rookie | 제품 판단, 메시지, 반론이 별도 역할로 쪼개져 작은 작업에서도 인계 비용 발생 |
| Finance / Humanizer | 비용 점검과 문장 정리가 별도 역할처럼 보여 사소한 수정에도 오버헤드로 해석됨 |
| 구 경로 사본 | `skills/SKILL*`, `skills/references/roles*`에 본문이 중복되어 정본 불일치 발생 |

핵심은 "역할이 많다"는 숫자 자체가 아니었습니다. **하나의 산출물에 대한 단일 책임자(Single Accountable Owner)가 흐려졌다**는 점이 문제였습니다.

## 2. 설계 토론과 가장 강했던 반론

대안은 세 가지였습니다.

1. 기존 14개 전문 역할 유지
2. **6개 책임 오너(Lean Squad)로 통합**
3. Lead / 개발 / 검수 3개로 극단적 축소

독립 검토(`/root/design_review`) 단계에서 나온 가장 강한 반론은 이것이었습니다.

> "이미 선택 호출 구조인데, 직함 수만 줄인다고 실제로 토큰이나 호출이 줄어든다고 볼 수 있는가?"

타당한 지적이었습니다. 과거 전체 실행 로그를 확보하지 못한 상태에서 "역할을 줄였으니 토큰을 아꼈다"고 주장하는 것은 근거 없는 추측이 됩니다. 그래서 개선 목표를 **추정 절감**이 아니라 **검증 가능한 구조 변화**로 재정의했습니다.

- **책임 단일화**: QA+Test → `Quality Lead`, DevOps+SRE → `Delivery Lead`, 기획·반론 → `Product Lead`
- **작업 크기별 경로 명시**: 문서 수정은 Lead + 독립 Security만 활성화, 기능 개발은 Engineering/Quality 추가
- **비협상 게이트 유지**: 역할을 합쳐도 **Lead의 자기 보안 승인 금지**, **독립 Security Review PASS**, **컴플라이언스 위반 차단** 규칙은 그대로 유지

### 최종 6-Owner 구조

| Owner | 통합 책임 |
| --- | --- |
| **Tech Lead** | 도메인 간 의사결정, FinOps, 핸드오프, 최종 평문 정리 |
| **Product Lead** | 기획, 메시징, 증거 수집, 반론 실험 설계 |
| **Engineering Lead** | Frontend, Backend, Data, AI/하네스 구현 |
| **Quality Lead** | 테스트 기준 설계, QA 실행, AC-테스트-실제 결과 증거 단일 소유 |
| **Delivery Lead** | DevOps, SRE, 배포·복구 및 기존 서비스 운영 변경 |
| **Security Review Engineer** | 독립 보안 검토 + 컴플라이언스 적용성·위반 차단 |

상세 의사결정 기록은 [`docs/decisions/2026-11-06-lean-squad.md`](https://github.com/DongJinNam/dev-team-skills/blob/main/docs/decisions/2026-11-06-lean-squad.md)에 공개되어 있습니다.

## 3. 실측 검증: 4-Arm 파일럿과 예상치 못한 버그 발견

구조를 바꾼 뒤 "깔끔해졌다"에서 끝내지 않고, 동일 모델(`gpt-6-astra/high`)에서 4개 조건을 비교하는 12회 파일럿(조건당 3개 과제)을 수행했습니다([`2026-11-07-four-arm-results.md`](https://github.com/DongJinNam/dev-team-skills/blob/main/docs/decisions/2026-11-07-four-arm-results.md)).

| 조건 | 검토 완료 | `dev-team` 대비 비교 (표본 내 관찰값) |
| --- | --- | --- |
| Default (스킬 없음) | 2/3 | 시간·토큰은 가장 적으나 결함 발생 |
| Superpowers | 3/3 | `dev-team`이 시간 **−7.35%**, 처리 토큰 **−15.61%** |
| ECC | 3/3 | `dev-team`이 시간 **−14.33%**, 처리 토큰 **−20.91%** |
| **dev-team-skills** | **3/3** | 추가 스킬 중 동일 완료율에서 가장 낮은 시간·토큰 |

### 한계를 숨기지 않고 기록한 이유

이 표본만으로 "항상 더 싸고 우수하다"고 일반화하지 않았습니다.

- 과제 3개 × 각 1회로 표본이 작습니다.
- 처리 토큰에는 캐시된 입력이 포함되며, `dev-team`의 비캐시 입력은 비교군보다 많아 **금전적 비용 절감 주장으로 쓰지 않았습니다.**
- 일부 PowerShell `Get-Content` 호출에서 인코딩 미지정으로 비ASCII 문자가 깨지는 통제 한계도 관찰되었습니다.

### 파일럿이 잡아낸 진짜 버그: UTF-8 BOM

실측을 돌린 가장 큰 수확은 의외의 곳에서 나왔습니다. Codex 실행 로그에서 `humanizer/SKILL.md: missing YAML frontmatter delimited by ---` 오류가 발견된 것입니다.

원인은 파일 앞 3바이트의 **UTF-8 BOM**이었습니다. 기존 하네스가 `utf-8-sig`로 BOM을 조용히 벗겨내고 통과시키고 있어서, 정작 런타임(Codex)에서는 스킬이 로드되지 않고 있었습니다.

1. `humanizer/SKILL.md`와 `SKILL_en.md`의 BOM 3바이트 제거
2. 검증 하네스가 BOM 포함 스킬 파일을 **실패로 거부**하도록 규칙 강화
3. 회귀 테스트 추가 후 Codex `app-server`의 `skills/list`로 재확인 → 고유 스킬 발견 **7/8 → 8/8**, 로드 오류 **1 → 0**
4. 전체 단위 테스트 **79/79 PASS** 확인

## 4. `v1.2.0`까지의 후속 개선

6-Owner 전환 이후에도 실사용 피드백을 반영해 하네스와 운영 흐름을 계속 다듬었습니다.

- **코드화된 불변식 추가 ([#26](https://github.com/DongJinNam/dev-team-skills/pull/26), [#27](https://github.com/DongJinNam/dev-team-skills/pull/27))**: `SKL-001~004`(경로·frontmatter·상대 링크·앵커), `CAT-001~002`(카탈로그 드리프트), `MIR-001`(미러 바이트 일치) 검사 자동화
- **루프 정책 수정 및 Eval 회귀 ([#41](https://github.com/DongJinNam/dev-team-skills/pull/41))**: `record_workflow_owners`·`request_delta_confirmation` 문장을 보정하고 19/19 결정 평가 캡처 재생 통과
- **PR-gated 릴리즈 플로우 (`v1.2.0`, [#42](https://github.com/DongJinNam/dev-team-skills/pull/42), [#43](https://github.com/DongJinNam/dev-team-skills/pull/43))**: `.changelog/` 프래그먼트 취합과 릴리즈 승격을 PR 게이트로 통합하고, 포크 PR에서 보안 패킷 워크플로가 잘못 실행되지 않도록 제한

## 마무리: 스킬 개선에서 얻은 원칙

이번 `engineering-squad` 개선에서 얻은 교훈은 단순합니다.

1. **역할 수가 아니라 책임 단일화가 품질을 결정한다.**
2. **가정이 아니라 재현 가능한 실패(BOM 버그, 19/19 eval)가 스킬을 실제로 단단하게 만든다.**
3. **한계를 명시한 작은 실측이 과장된 벤치마크보다 오래 신뢰받는다.**

다음 글에서는 `harness-engineering`의 결정론적 검증기 설계와 멀티 플랫폼(`Claude Code` · `Codex` · `Antigravity` · `Cursor`) 패키징 구조를 다룰 예정입니다.
