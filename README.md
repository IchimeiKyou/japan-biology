# 🎓 Japan Bio Grad School Guide / 일본 대학원 생물계열 수험 가이드

> 日本語 · 中文 · 한국어 · English

---

## 🇯🇵 日本語

### プロジェクト概要

韓国の生物系学部卒業予定者を主な対象とした、**日本大学院（生物・生命科学・農学系）受験ガイド**です。  
東京大学・京都大学・大阪大学など 13 校以上の入試情報を韓日バイリンガルで収録しています。

### ファイル構成

```
japan_bio_grad_guide/
├── index.html                          # メインガイド（オフライン単体閲覧可）
└── japan-gradschool-research/
    ├── SKILL.md                        # AgentConstruction スキル定義
    └── templates/
        ├── report-template.html        # 新規ガイド生成用 HTML テンプレート
        └── uni-card-template.html      # 大学カード断片テンプレート
```

### 閲覧方法

`index.html` をブラウザで直接開くだけで閲覧できます。外部 CDN への依存はありません。

### スキルによる更新方法（AgentConstruction 環境が必要）

[AgentConstruction](https://github.com/your-org/AgentConstruction) の `japan-gradschool-research` スキルを使用することで、
大学情報の追加・更新・新規ガイド生成が自動化できます。

| モード | トリガー例 | 説明 |
|--------|-----------|------|
| A — 単一大学調査 | 「東大農学研究科 調べて」 | 1 校の詳細情報収集 + Markdown メモ生成 |
| B — 大学カード追加 | 「大阪大学 追加して」 | `index.html` にカードを挿入 |
| C — 既存情報更新 | 「ガイドをアップデート」 | 変更された入試日程・語学基準のパッチ |
| D — 新規ガイド生成 | 「生物大学院ガイドを作って」 | テンプレートから完全 HTML を生成 |

### 情報の時効性

収録情報は収集時点の公式 HP・募集要項に基づきます。  
**入試情報は毎年変更されます。必ず各大学公式 HP にて最新情報をご確認ください。**

---

## 🇨🇳 中文

### 项目简介

本项目是面向**韩国生物系本科毕业生申请日本研究生院**的双语（韩日）备考指南。  
收录东京大学、京都大学、大阪大学等 13 所以上高校的入学考试信息，涵盖语言要求、申请流程、奖学金及生活费用等。

### 文件结构

```
japan_bio_grad_guide/
├── index.html                          # 主指南（离线单文件，可直接浏览器打开）
└── japan-gradschool-research/
    ├── SKILL.md                        # AgentConstruction Skill 定义文件
    └── templates/
        ├── report-template.html        # 新建指南 HTML 模板
        └── uni-card-template.html      # 单所大学卡片 HTML 片段模板
```

### 查看方式

直接用浏览器打开 `index.html` 即可，**无需网络连接，无外部 CDN 依赖**。

### 使用 Skill 更新（需要 AgentConstruction 环境）

在 [AgentConstruction](https://github.com/your-org/AgentConstruction) 框架中，通过以下触发词激活 `japan-gradschool-research` Skill：

```
日本大学院调研 / 查一下 XXX 大学院 / 更新日本院校指南 / 添加日本院校 / 日本生物大学院信息
```

Skill 支持四种工作模式：

| 模式 | 触发示例 | 说明 |
|------|---------|------|
| A — 单校深度调查 | 「查一下东大农学研究科」 | 收集单所大学详细信息，生成 Markdown 备忘录 |
| B — 添加大学卡片 | 「把大阪大学加进去」 | 在 `index.html` 中插入新卡片，并更新链接表 |
| C — 更新现有信息 | 「更新院校指南」 | 仅 patch 变更的入试日程/语言要求 |
| D — 生成新指南 | 「生成生物大学院指南」 | 从模板生成完整 HTML 文件 |

### 数据时效性说明

收录信息来源于各大学官方网站（`ac.jp` 域名）及 JASSO 等官方渠道。  
**入学考试信息每年更新，请在正式申请前务必查阅各大学最新募集要项。**

### 技术说明

- 纯 HTML/CSS，零 JavaScript 依赖，无障碍离线使用
- 韩语在上、日语在下的双语布局，地名一律使用日文官方表记
- CSS 设计系统：深海军蓝 `#1a3f6f` + 绿色日语副文字 `#1b4d3e`

---

## 🇰🇷 한국어

### 프로젝트 개요

**한국 생물계열 학부 졸업 예정자**를 위한 일본 대학원(생물·생명과학·농학 계열) 한일 이중언어 수험 가이드입니다.  
도쿄대학·교토대학·오사카대학 등 13개교 이상의 입시 정보를 수록하며, 어학 요건·출원 일정·장학금·생활비·교수 컨택 방법까지 포함합니다.

### 파일 구조

```
japan_bio_grad_guide/
├── index.html                          # 메인 가이드 (오프라인 단독 파일)
└── japan-gradschool-research/
    ├── SKILL.md                        # AgentConstruction 스킬 정의
    └── templates/
        ├── report-template.html        # 신규 가이드 HTML 템플릿
        └── uni-card-template.html      # 대학 카드 HTML 조각 템플릿
```

### 열람 방법

`index.html` 을 브라우저로 직접 열면 됩니다. **인터넷 연결 불필요, 외부 CDN 의존 없음.**

### Skill로 업데이트 (AgentConstruction 환경 필요)

[AgentConstruction](https://github.com/your-org/AgentConstruction) 프레임워크에서 `japan-gradschool-research` 스킬을 아래 트리거로 활성화합니다:

```
일본 대학원 조사 / XXX 대학원 알아봐 줘 / 일본 원서 가이드 업데이트 / 일본 원서 대학 추가
```

스킬은 4가지 모드를 지원합니다:

| 모드 | 트리거 예시 | 설명 |
|------|-----------|------|
| A — 단일 대학 심층 조사 | 「도쿄대 농학연구과 알아봐 줘」 | 1개교 상세 정보 수집 + Markdown 메모 생성 |
| B — 대학 카드 추가 | 「오사카대 추가해 줘」 | `index.html` 에 새 카드 삽입 + 링크표 갱신 |
| C — 기존 정보 갱신 | 「가이드 업데이트 해 줘」 | 변경된 일정·어학 기준만 패치 |
| D — 신규 가이드 생성 | 「생물 대학원 가이드 만들어 줘」 | 템플릿 기반 전체 HTML 생성 |

### 정보 시효성 주의

수록 정보는 수집 시점의 공식 홈페이지 및 모집요강(募集要項)에 근거합니다.  
**입시 정보는 매년 변경됩니다. 정식 출원 전 반드시 각 대학 공식 홈페이지에서 최신 정보를 재확인하십시오.**

### 기술 사양

- 순수 HTML/CSS, JavaScript 미사용, 오프라인 완전 동작
- 한국어 상단 + 일본어 하단(소폰트) 이중언어 레이아웃
- 지명은 일본어 공식 표기(한자·가나) 사용, 간체자 대체 금지

---

## 🇬🇧 English

### Project Overview

A **bilingual (Korean/Japanese) graduate school application guide** for Korean undergraduate students in biology-related fields seeking admission to Japanese graduate programs.  
Covers 13+ universities including the University of Tokyo, Kyoto University, and Osaka University, with detailed information on language requirements, application timelines, scholarships, and professor contact strategies.

### File Structure

```
japan_bio_grad_guide/
├── index.html                          # Main guide (self-contained, offline-ready)
└── japan-gradschool-research/
    ├── SKILL.md                        # AgentConstruction Skill definition
    └── templates/
        ├── report-template.html        # HTML template for generating new guides
        └── uni-card-template.html      # HTML fragment template for single university cards
```

### How to View

Simply open `index.html` in any browser. **No internet connection required — zero external CDN dependencies.**

### Updating via Skill (requires AgentConstruction)

Activate the `japan-gradschool-research` skill in [AgentConstruction](https://github.com/your-org/AgentConstruction) with any of these triggers:

```
일본 대학원 조사 / 日本大学院调研 / XXX대학원 알아봐 줘 / 更新日本院校指南 / 添加日本院校
```

The skill supports four operating modes:

| Mode | Example trigger | Description |
|------|----------------|-------------|
| A — Deep single-university research | "Look into Tokyo Univ. Agri." | Full info collection for one school + Markdown memo |
| B — Add university card | "Add Osaka University" | Insert new card into `index.html`, update link/cost tables |
| C — Patch existing guide | "Update the grad school guide" | Apply targeted edits to changed deadlines or score thresholds |
| D — Generate new full guide | "Create a bio grad school guide" | Render a complete HTML file from `report-template.html` |

### Data Currency

All information is sourced from official university websites (`ac.jp` domains), JASSO, and MEXT at the time of collection.  
**Admissions requirements change every year. Always verify the latest 募集要項 (application guidelines) on each university's official website before applying.**

### Technical Notes

- Pure HTML/CSS, no JavaScript, fully offline-capable single-file output
- Bilingual layout: Korean text on top, Japanese subtitle in smaller font below (`color: #1b4d3e`)
- Japanese place names and department names always use official Japanese script (kanji/kana)
- CSS design system: Navy `#1a3f6f` + Teal `#1b4d3e` + Amber `#e07b2a`

---

*Last updated: 2026-05 · Maintained by [AgentConstruction](https://github.com/your-org/AgentConstruction) `japan-gradschool-research` skill*
