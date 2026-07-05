---
name: japan-gradschool-research
description: |
  일본 대학원（생물·생명과학·농학 계열）입시 정보를 다국어로 조사·정리하고,
  한일 이중언어 HTML 가이드 또는 Markdown 메모를 생성·갱신한다.
  触发词（トリガー）：
    - 일본 대학원 조사 / 日本大学院调研 / 日本院校情报
    - XXX 대학원 알아봐 줘 / 查一下XXX大学院
    - 일본 원서 가이드 업데이트 / 更新日本院校指南
    - 일본 생물 대학원 정보 / 日本生物大学院信息
    - 일본 원서 대학 추가 / 添加日本院校
version: 1.0.0
allowed-tools: WebSearch, WebFetch, Write, Read, Edit, Bash
---

# japan-gradschool-research Skill

## 0. 사전 준비 (Step 0 — Setup)

### 0-1. 설정 파일 로드

반드시 `skills/_shared/user-config.json` 을 읽고, `user-config.local.json` 이 있으면 덮어쓴다.
사용할 경로 변수:

| 변수 | JSON 키 | 예시 |
|------|---------|------|
| 출력 디렉터리 | `japan_gradschool.output_dir` | `~/JapanGradReports` |
| 메인 가이드 파일 | `japan_gradschool.guide_file` | `japan_bio_grad_guide/index.html` |
| 조사 대상 전공 | `japan_gradschool.target_fields` | `["生物工学", "分子生物学", ...]` |

### 0-2. 인텐트 감지 (Mode Detection)

사용자 메시지에서 작업 모드를 판단한다:

| 모드 | 트리거 예시 | 설명 |
|------|-----------|------|
| **A — 단일 대학 심층 조사** | "도쿄대 농학연구과 알아봐 줘" | 특정 1개 대학·연구과 상세 정보 수집 + 메모 생성 |
| **B — 기존 가이드에 대학 추가** | "오사카대 추가해 줘" | 가이드 파일에 새 uni-card 삽입 + 링크표·비용표 갱신 |
| **C — 기존 가이드 일부 갱신** | "대학원 가이드 업데이트" | 변경된 정보(마감일·어학기준 등) 패치 적용 |
| **D — 신규 종합 가이드 생성** | "일본 생물 대학원 가이드 만들어 줘" | report-template.html 기반 전체 HTML 생성 |

모드 불명확 시, 사용자에게 한 번만 확인한다.

---

## 1. 정보 수집 (Step 1 — Research)

### 1-1. 공식 소스 우선 순위

```
Tier 1 (공식):  각 대학 研究科 공식 웹사이트 모집요강(募集要項) PDF
Tier 2 (공식):  JASSO (https://www.jasso.go.jp/), 文部科学省 (https://www.mext.go.jp/)
Tier 3 (참고):  大学院入試ドットコム, 研究室訪問記録ブログ, 受験体験談（나무위키 제외）
```

### 1-2. 병렬 검색 전략

같은 메시지 내에서 WebSearch 를 여러 번 동시 발화하여 병렬로 수집한다.
대학별 기본 쿼리 패턴:

```
"{大学名} {研究科名} 入試 {年度} 募集要項"
"{大学名} 国際 外国人 特別選抜 生物"
"{大学名} 大学院 TOEFL JLPT 英語 要件 {年度}"
"MEXT 外国人留学生奨学金 {大学名} {研究科名}"
```

`{年度}` = 현재 연도 + 1 (2026년이면 2027년 입시 기준)

### 1-3. WebFetch 대상 선정

WebSearch 결과에서 **공식 URL** (`ac.jp` 도메인) + **PDF 링크 포함 페이지** 를 우선 선택한다.
1개 대학당 최대 3회 WebFetch (모집요항 페이지 → 학비·생활비 페이지 → 장학금 페이지).

### 1-4. 3단계 폴백 전략

```
① 1차: 정확한 쿼리로 WebSearch
② 2차: 단순화 쿼리 ("도쿄대 농학 입시") + 시드 URL 직접 WebFetch
③ 3차: 공식 URL 없을 시 JASSO 포털에서 대학별 링크 추출
```

---

## 2. 데이터 추출 및 정규화 (Step 2 — Extract & Normalize)

수집 정보를 아래 JSON 스키마로 정규화한다. 확인되지 않은 필드는 `"unknown"` 처리.

```json
{
  "univ_id": "todai-agri",
  "name_ja": "東京大学 大学院 農学生命科学研究科",
  "name_ko": "도쿄대학교 대학원 농학생명과학연구과",
  "emoji": "🌸",
  "location_ja": "東京都 文京区",
  "location_ko": "도쿄도 분쿄구",
  "rank_tier": "S",
  "departments": [
    {
      "name_ja": "農学国際専攻",
      "name_ko": "농학국제전공",
      "degree": "修士課程",
      "quota_intl": "若干名",
      "language_medium": "英語・日本語"
    }
  ],
  "requirements": {
    "toefl_ibt_min": 79,
    "toeic_min": null,
    "jlpt_min": null,
    "jlpt_note_ko": "JLPT 불필요 (영어 전형 존재)",
    "jlpt_note_ja": "JLPT不要（英語入試あり）",
    "gpa_note_ko": "GPA 기준 명시 없음, 출신교 성적증명 제출",
    "gpa_note_ja": "GPA基準明示なし、成績証明書提出必要"
  },
  "schedule": {
    "exam_season": "夏入試",
    "application_window_ko": "2026년 5~6월경",
    "application_window_ja": "2026年5〜6月頃",
    "exam_date_ko": "2026년 7월경",
    "exam_date_ja": "2026年7月頃",
    "result_date_ko": "2026년 8월경",
    "result_date_ja": "2026年8月頃",
    "winter_exam": true,
    "winter_note_ko": "겨울 입시 있음 (11~12월 원서)",
    "winter_note_ja": "冬入試あり（11〜12月出願）"
  },
  "kenkyusei": {
    "available": true,
    "note_ko": "연구생 입학 가능 → 수사과정 진학 루트",
    "note_ja": "研究生入学可能 → 修士課程進学ルート"
  },
  "mext": {
    "available": true,
    "type": "国費外国人留学生 / 학교 추천",
    "amount_jpy": 148000,
    "note_ko": "월 14.8만 엔, 수업료·입학금 면제",
    "note_ja": "月14.8万円、授業料・入学金免除"
  },
  "lab_contact": {
    "required_before_apply": true,
    "note_ko": "내낙(内諾) 필수. 출원 전 지도교수 연락 必",
    "note_ja": "内諾必須。出願前に指導教官へ連絡要"
  },
  "official_links": [
    {
      "label_ko": "모집요강",
      "label_ja": "募集要項",
      "url": "https://www.a.u-tokyo.ac.jp/graduate/admission/"
    }
  ],
  "living_cost": {
    "city": "Tokyo",
    "monthly_min_jpy": 100000,
    "monthly_max_jpy": 130000,
    "note_ko": "기숙사 미입주 시 도쿄 기준"
  },
  "warn_boxes": [
    {
      "text_ko": "2026년~: 외국인 특별선발 폐지 → 일반입시 통일",
      "text_ja": "2026年〜：外国人特別選抜廃止→一般入試に統一"
    }
  ],
  "tips": [
    {
      "text_ko": "연구실 홈페이지에서 교수 연구 분야 반드시 확인",
      "text_ja": "研究室HPで教員の研究分野を必ず確認"
    }
  ],
  "data_confirmed_date": "2026-05",
  "source_urls": []
}
```

**Tier 판정 기준:**

| Tier | 기준 |
|------|------|
| S | 세계 상위 100위 이내 (QS/THE) + 일본 최상위 |
| A | 旧帝 + NAIST/一橋 수준 |
| B | 上位国立・私立 (神戸・広島 등) |

---

## 3. 갭 감지 (Step 3 — Gap Detection)

모드 B/C 에서만 실행. 기존 가이드 파일을 Read 한 뒤:

1. 해당 대학 카드가 이미 있는지 검색 (`univ_id` 또는 대학명 일치)
2. 있으면: 변경된 필드(어학 기준·일정·warn 박스) 목록 생성 → 패치만 적용
3. 없으면: uni-card-template.html 기반으로 새 카드 생성 → 삽입 위치 결정
   - 삽입 위치: Tier 내림차순, 같은 Tier이면 50음 순서

---

## 4. 출력 생성 (Step 4 — Output)

### 모드 A — Markdown 메모 저장

파일명: `{output_dir}/{univ_id}_memo_{YYYY-MM}.md`

```markdown
# {大学名} 受験メモ / 수험 메모
> 조사일: {YYYY-MM-DD}  출처: {source_urls}

## 요약 / 要約
...

## 어학 요건 / 語学要件
...

## 전형 일정 / 入試スケジュール
...

## 내낙 / 内諾
...

## 참고 링크 / 参考リンク
...
```

### 모드 B — 가이드에 uni-card 삽입

1. `skills/japan-gradschool-research/templates/uni-card-template.html` 을 Read
2. 모든 `{{PLACEHOLDER}}` 를 정규화된 JSON 데이터로 치환
3. 기존 가이드에서 `<!-- UNI-CARD-END -->` 주석 앞에 삽입
4. 공식 링크 표, 생활비 표도 해당 대학 행 추가

### 모드 C — 패치 적용

Edit 툴로 변경된 필드만 부분 수정. 전체 파일 재쓰기 금지.

### 모드 D — 신규 HTML 생성

1. `skills/japan-gradschool-research/templates/report-template.html` 을 Read
2. 모든 `{{PLACEHOLDER}}` 치환 + 대학 카드 삽입
3. 파일명: `{output_dir}/japan_bio_grad_guide_{YYYY-MM}.html`

---

## 5. 아카이브 및 인덱스 (Step 5 — Archive)

```
{output_dir}/
  index.md                  ← 전체 보고서/메모 목록 (갱신)
  {univ_id}_memo_{YYYY-MM}.md
  japan_bio_grad_guide_{YYYY-MM}.html
```

`index.md` 업데이트 규칙:
- 기존 파일 Read → 해당 항목 있으면 날짜·파일명 업데이트, 없으면 새 행 추가
- Edit 툴로 부분 수정

---

## 6. 품질 체크리스트 (Step 6 — QC)

출력 전 반드시 확인:

- [ ] 모든 지명·연구과명이 일본어 공식 표기 사용 (한자·가나)
- [ ] 한국어 설명 아래 반드시 일본어 `<small style="color:var(--ja-color);">` 동반
- [ ] 어학 점수는 공식 출처 확인, 불명확 시 `"unknown"` + 출처 주석
- [ ] 전형 일정은 과거 연도 패턴 + 공식 확인 여부 명시
- [ ] 내낙 필요 여부 표시
- [ ] warn-box는 날짜 포함 변경 이력 기재
- [ ] 생성된 HTML이 오프라인 단독 브라우징 가능 (외부 CDN 없음)
- [ ] 출력 파일 경로 사용자에게 보고

---

## 참고: 주요 공식 소스 URL

| 기관 | URL |
|------|-----|
| 東京大学 農学生命科学研究科 | https://www.a.u-tokyo.ac.jp/graduate/admission/ |
| 京都大学 生命科学研究科 | https://www.lif.kyoto-u.ac.jp/admissions/ |
| 大阪大学 生命機能研究科 | https://www.fbs.osaka-u.ac.jp/ja/admissions/ |
| 名古屋大学 生命農学研究科 | https://www.agr.nagoya-u.ac.jp/~graduate/ |
| 北海道大学 生命科学院 | https://www.lsi.hokudai.ac.jp/admission/ |
| 東北大学 生命科学研究科 | https://www.lifesci.tohoku.ac.jp/admission/ |
| 九州大学 農学研究院 | https://agr.kyushu-u.ac.jp/outline/graduate/ |
| NAIST 先端科学技術研究科 | https://www.naist.jp/admissions/ |
| JASSO 奨学金情報 | https://www.jasso.go.jp/study_j/scholarships/ |
| 文科省 国費外国人留学生 | https://www.mext.go.jp/a_menu/koutou/ryugaku/ |
