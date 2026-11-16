# fpe-study — 소방설비기사(전기분야) 기출 학습 시스템

## 산출물
- `index.html` — 단일 파일 웹앱(데이터·이미지 경로 포함, 4.5MB). `figs/` 폴더에 그림 283개.
- 사본: `downloads/소방설비기사_전기_기출학습.html`

## 수록 범위
### 필기(객관식 4지선다, 회차당 80문항 = 4과목 × 20문항)
| 연도 | 회차 수 | 출처 | 해설 |
|---|---|---|---|
| 2003~2015 | 39회차 (3,120문항) | 모두CBT(comcbt.com) 교사용 PDF 원문 파싱 | 없음(정답만) |
| 2016~2026 | 31회차 (2,480문항) | 모두CBT(moducbt.com) 구조화 데이터 | 있음 |
| 합계 | **70회차 5,600문항** | | |

- 2003~2015년 정답은 100% 교사용 PDF 뒤쪽 정답표와 대조 검증(3,120/3,120 일치).
- 2022년 3회차부터 필기는 CBT 전환으로 공식 기출 비공개 → 2022-3회~2026-1회는 복원·재구성 문항(모두CBT 표기). 보기 순서는 원본 시험지와 다를 수 있음(모두CBT가 보기를 섞어 제공).

### 실기(필답형)
| 구분 | 회차 | 문항 |
|---|---|---|
| 다산에듀 복원 기출 PDF (0ham.tistory.com 경유) | 2010~2023년 44회차 | 752 |
| 실기 단답·서술 100제 (L.N.R, 2019~2022 기출) | 1세트 | 100 |
| 모두CBT 실기 서술형 세트(모음 88 + #1~#4 + 틀린문제 26) | 6세트 | 194 |
| 합계 | 49개 필터 | **1,046문항** |

- 실기 기출은 Q-Net이 공개하지 않아 전부 복원 자료 기준.
- 2019~2020년 및 일부 계산문항은 PDF의 수식이 한글 수식 폰트(Private Use Area)로 들어 있어 텍스트 변환 시 숫자가 일부 누락됨. 해당 문항 답안 끝에 안내 문구를 자동 부착.
- 2024년 실기 PDF 3회차는 스캔 이미지 형태라 텍스트 추출 불가 → 제외.

## 기능
- 🔄 새로고침(캐시 무시, localStorage 보존), localStorage 키 `fpe_pro_v1` / `fpe_exam_v1`
- 📘 필기 기출풀이: 회차·과목 필터, 20문항 페이지네이션, 📖 해설 토글(통계 미반영), ★ 헷갈림
- ⏱️ 필기 모의고사: 과목별 20 × 4과목 = 80문항, 120분, 과락 40점·평균 60점 판정, OMR 그리드, 최근 결과 저장
- 📌 오답·북마크: 오답/★ 모아보기 + 해설
- ✍️ 실기 대비: 4대 유형 접이식 가이드, 회차/유형/★ 필터, 🧭 풀이 틀, 📖 모범답안, 🔲 빈칸 암기 모드, ✅ 암기완료, 3문항·90분 실기 모의고사(답안 메모 + 일괄 대조)
- ⚡ 암기카드 18장(공식·수치·법령), 📅 학습 로드맵 15단계

## 배포 (2026-11-16 완료)
- 라이브: https://chataeyun.github.io/fpe-study/ (HTTP 200, 4.5MB index.html + figs 283개 정상)
- 저장소: https://github.com/chataeyun/fpe-study (public), 커밋 a7d5607
- **배포 소스 브랜치는 `gh-pages`** (main 아님 → GitHub Pages 설정에서 `gh-pages` / `/` 로 잡혀 있음).
  - Pages API(`gh api -X POST repos/chataeyun/fpe-study/pages`)는 GitHub App에 Pages 권한이 없어 403이었고,
    공개 저장소에 `gh-pages` 브랜치를 만들자 Pages가 자동 활성화되며 그 브랜치를 소스로 잡았다.
  - 워크플로(.github/workflows/pages.yml) 우회도 App에 `workflows` 권한이 없어 푸시가 거부된다.
- **내용을 고칠 때**: main 커밋 후 `git push origin main:gh-pages`까지 해야 사이트가 갱신된다.
- 주의: Jekyll이 밑줄(`_`)로 시작하는 파일을 제외하므로 확인용 임시 페이지는 `m375.html`처럼 밑줄 없이 올린다.

```bash
cd /workspace/fpe-study
git add -A && git commit -m "..."
git push origin main && git push origin main:gh-pages   # gh-pages = 배포 브랜치
```

## 재생성 파이프라인 (fpe-work/)
`convert_data.py`(모두CBT GraphQL) → `build_old.py`(comcbt PDF 2003~2015) → `merge.py` → `data/questions.json`
`build_essay.py` + `build_essay_pdf.py` → `data/essay.json`
`build_app.py`(template.html + 데이터 주입) → `fpe-study/index.html`
`test_app.py`(quickjs DOM 스텁으로 렌더·채점·필터 로직 검증)
