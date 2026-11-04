\---

title: Process and Results of Execution

date: 2026-11-04

draft: false

build:

&#x20; list: never

\---

#### 수행 과정

1. 텍사스 홀덤 규칙을 정리하고 구현할 기능(카드 분배, 베팅, 족보 판정)을 정했습니다.
2. 카드 덱 생성, 셔플과 족보 판정 로직을 먼저 구현하고 콘솔 출력으로 결과가 맞는지 확인했습니다.
3. 카드와 버튼을 보여주는 화면을 만들고 로직과 연결했습니다.
4. 직접 여러 판을 플레이 하며 오류를 찾고 디자인적인 부분(테이블 화면,  베팅 칩, 버튼 등)을 함께 수정했습니다.



이 게임을 웹으로 만든 이유는 앱으로 만들게 되면 개발 환경 설정과 설치, 배포 과정이 필요하지만 웹으로 만들게 되면 브라우저만 있으면 바로 실행해 결과를 확인할 수 있어 학업을 병행하면서 작은 앱이나 게임을 만들자는 저의 목표와 더 부합하다고 생각했기 때문입니다.



#### **결과물**



**```javascript**

**function compareEval(a, b) {**

&#x20; **if (a.rank !== b.rank) return a.rank - b.rank;**

&#x20; **for (let i = 0; i < Math.max(a.tiebreak.length, b.tiebreak.length); i++) {**

&#x20;   **const x = a.tiebreak\[i] || 0, y = b.tiebreak\[i] || 0;**

&#x20;   **if (x !== y) return x - y;**

&#x20; **}**

&#x20; **return 0;**

**}**



**function evaluateBest(cards) {**

&#x20; **if (cards.length < 5) return { rank: 0, tiebreak: cards.map(c => c.rank).sort((a, b) => b - a) };**

&#x20; **const combos = combinations(cards, 5);**

&#x20; **let best = null;**

&#x20; **for (const combo of combos) {**

&#x20;   **const ev = evaluate5(combo);**

&#x20;   **if (!best || compareEval(ev, best) > 0) best = ev;**

&#x20; **}**

&#x20; **return best;**

**}**

**```**



위 코드는 족보 판정 호직의 핵심 부분입니다. 플레이어의 카드 2장과 공용 카드 5장, 총 7장에서 만들 수 있는 5장 조합 21개를 모두 평가해 가장 강한 족보를 고릅니다.

`compareEval`은 족보 등급을 먼저 비교하고, 같은 족보일 때는 높은 카드를 순서대로 비교해 승자를 정합니다. 이 로직으로 같은 원페어끼리도 페어 숫자와 

나머지 카드를 비교해 정확히 승패가 갈리는 것을 확인했습니다.

