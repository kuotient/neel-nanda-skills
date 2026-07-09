# neel-nanda-skills

에이전트가 Neel Nanda가 논문을 판단하듯 판단하게 만드는 Claude 스킬입니다.
문체가 아니라 추론 구조를요.

> Neel Nanda와 무관하며 그의 보증을 받지 않았습니다. 그가 Anthropic 논문에
> 남긴 [공개 논평](https://transformer-circuits.pub/2026/workspace/index.html)의
> 추론 구조를 옮겨 담은 것이며, 이름은 "파인만 기법"처럼 기술적으로만 씁니다.
> · [English](README.md)

## 만든 이유

Anthropic의 "Global Workspace" 논문에 Neel Nanda의 초청 리뷰가 함께 실렸는데,
그 논평이 계속 머릿속에 남았습니다. 그냥 맞는 말이어서가 아니라 *지어져
있어서*였습니다. 그는 논문을 별개의 claim으로 쪼개고, 각각을 얼마나 믿는지
말하고, 증거를 보기 전에 자기 prior부터 세우고, 결과마다 지루한 설명을
사냥하고, 자기가 판단할 자격이 없는 부분을 명시했습니다.

아무리 자세한 프롬프트를 줘도 제 클로드는 그렇게 잘 짜인 논평을 낸 적이
없었습니다. 유창하고 그럴듯한 글을 쓰고는, 방금 읽은 것에 슬그머니
동의하곤 했죠. 그래서 그의 논평이 왜 작동하는지 뜯어보고, 여덟 개의
무브로 적어, 스킬로 만들었습니다.

## 설치

**Claude Code (플러그인)**

```
/plugin marketplace add kuotient/neel-nanda-skills
/plugin install neel-nanda-skills@neel-nanda-skills
```

**Claude Code (수동)**

```
git clone https://github.com/kuotient/neel-nanda-skills.git
cp -r neel-nanda-skills/skills/neel-nanda-analysis ~/.claude/skills/
```

**claude.ai / Desktop**: 설정 → Capabilities → Skills →
`skills/neel-nanda-analysis` 폴더(zip) 업로드.

어떤 언어로든 판단 요청("이거 어떻게 생각해", "review this", "poke holes in
this")에 트리거됩니다.

## 여덟 가지 무브

| # | 무브 | 막아주는 것 |
|---|------|------------|
| 1 | 독립 claim으로 분해; 헤드라인 claim은 값싼 버전과 야심찬 버전으로 가르기 | 전체에 판정 하나 |
| 2 | 중요도 / 확신도 / 판단자격을 따로 평가 | "좋았다" |
| 3 | 증거 보기 전에 first-principles prior부터 | "논문이 그렇다니까 믿는다" |
| 4 | worked example로 직관 만들기 | 미끄러운 추상화 |
| 5 | 유리한 결과마다 boring hypothesis 사냥 | motivated reasoning |
| 6 | 증거를 스펙트럼으로 등급화; 뭐가 cruxy한지 명시 | 균일한 칭찬 |
| 7 | 전문성과 커버리지의 경계 표시 | 가짜 전지성 |
| 8 | 실용성 한정: 어디에 / 어디서 실패하나 | "이건 게임체인저" |

모든 판단의 공통 형태: **commit, then bound.** 믿는 것을 말하고, 곧바로
어디까지인지 말한다.

## 차이

같은 모델, 같은 메모("코드리뷰를 선택제로 하자"는 그럴듯한 제안, 함정 있는
통계 4개)를 스킬 없이/있이 리뷰시켰습니다. 둘 다 통계 함정은 잡습니다. 글을
*여는 방식*을 보세요:

> **스킬 없음:** "This memo diagnoses a real problem and then reaches for the
> wrong cure with data that doesn't support it."
>
> **스킬 있음:** "My prior, before the numbers: review's value was never in
> the average PR. It lives in the tail [...] Same data, opposite conclusion,
> which means the data is not discriminating between us. That alone should stop
> the rollout."

하나는 판정을 건네고, 하나는 세계의 모델을 건넨 뒤 메모의 증거로는 그걸 못
가른다는 걸 보여줍니다. 메모 전문과 두 출력: [examples/](examples/).

## 어떻게 검증했나

어렵게 검증했습니다. 블라인드 에이전트에게 이 스킬과 같은 논문만 주고(Neel의
리뷰는 컨텍스트에 전혀 없음) 비교했더니, 그가 도달한 곳에 독립적으로
도달했습니다. 최종 판정까지요:

> **Neel:** "I expect it to largely be useful as a hypothesis generation tool
> [...] I expect it to have many false positives."
>
> **블라인드 에이전트:** "use it to *find*, never to *clear*."

무스킬 베이스라인도 돌렸습니다. 정직한 질문은 "스킬이 한 건가, 모델이 원래
잘한 건가"니까요. 원래 잘했습니다. 그래서 블라인드 심판이 둘을 8개 구조
체크로 채점했더니 **스킬 없음 6/8, 스킬 7/8.** 갈린 두 개는 "증거보다 먼저
prior 세우기"와 "자격 부족 시 판정 유보"입니다.

실격 런 2개 포함 전체 런, 감사, 재현 절차: [validation/](validation/). 정직한
한계: 어떤 것도 재현 못 한 건 Neel이 논문을 다른 모델에서 재현한 것과 십 년치
직관입니다. 구조는 이식되지만 전문성은 이식되지 않습니다.

## 출처와 귀속

[Neel Nanda](https://www.neelnanda.io/)의
[*Verbalizable Representations Form a Global Workspace in Language Models*](https://transformer-circuits.pub/2026/workspace/index.html)
(Anthropic, Transformer Circuits, 2026) 초청 외부 논평을 공부해 그 판단 구조를
스킬로 옮긴 것입니다. 그는 이 레포와 무관하며 보증하지 않았고, 원문 논평이
어떤 재구성본보다 낫습니다. 위 인용은 비교 목적의 짧은 발췌이며 원문으로
링크합니다.

[andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills)
에서 정신적 영감을 받았습니다. 특정 연구자의 작업 규율을 스킬로 패키징할 수
있다는 아이디어요. 내용은 독립적입니다.

## 라이선스

MIT. [LICENSE](LICENSE) 참조.
