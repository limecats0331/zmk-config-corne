# Corne Keymap 설계 노트

기존 Lily58 (58키)에서 Corne (42키)로 마이그레이션하면서 결정한 layout 설계와
그 이유를 기록한 문서.

## 컨텍스트

- **기존**: Lily58 — 5행 6열 + thumb cluster, 외곽 pinky 칼럼 풍부, 숫자행 dedicated
- **새 키보드**: Corne — 3행 6열 + 3 thumb cluster, 숫자행 없음
- **잃는 키 수**: 약 16개 (숫자행 12 + 행4 내부 키 2 + 기타)
- **사용자 환경**: Mac, 영문 위주(한글은 가끔), 코딩 중심 사용

## 설계 철학: 보수적 (현방파)

세 가지 방향 중 선택:

1. **보수적** — Home row mods (HRM) 안 씀, 임시키는 thumb/layer로, Lily58 멘탈모델 최대 유지
2. **적극적** — HRM 활용, thumb 단순화, 학습 필요
3. **극단적** — combos 위주 (miryoku 스타일), 학습량 큼

→ **1번 선택.** 이유:
- 이미 익숙한 Lily58 layout과 mental model 유지가 가장 가치 있음
- 새 키보드 받자마자 생산성 손실 최소화

## Base Layer

```
┌────┬────┬────┬────┬────┬────┐         ┌────┬────┬────┬────┬────┬────┐
│TAB │ Q  │ W  │ E  │ R  │ T  │         │ Y  │ U  │ I  │ O  │ P  │BSPC│
├────┼────┼────┼────┼────┼────┤         ├────┼────┼────┼────┼────┼────┤
│CTL │ A  │ S  │ D  │ F  │ G  │         │ H  │ J  │ K  │ L  │ ;  │ '  │
│/ESC│    │    │    │    │    │         │    │    │    │    │    │    │
├────┼────┼────┼────┼────┼────┤         ├────┼────┼────┼────┼────┼────┤
│LSFT│ Z  │ X  │ C  │ V  │ B  │         │ N  │ M  │ ,  │ .  │ /  │RSFT│
└────┴────┴────┼────┼────┼────┤         ├────┼────┼────┼────┴────┴────┘
               │LALT│LGUI│SPC │         │ENT │mo 2│mo 1│
               └────┴────┴────┘         └────┴────┴────┘
```

### Thumb cluster는 Lily58 그대로

`LALT LGUI SPACE | ENTER mo 2 mo 1` — Lily58 thumb 배치 동일. 손가락 근육기억 그대로 사용.

### `&mt LCTRL ESC` (Row 2 outer-left)

hold=Ctrl / tap=Esc. Caps Lock 위치를 Ctrl-Esc로 쓰는 전통 Mac/vim 패턴.

**왜 ESC고 TAB이 아닌가?**
- `&mt LCTRL TAB`이라고 가정하면 **Ctrl+TAB이 불가능**해짐
- hold-tap은 같은 키를 동시에 hold와 tap 할 수 없음 (자기참조 문제)
- Ctrl+TAB은 브라우저 탭 전환 등으로 자주 쓰지만 Ctrl+ESC는 거의 안 씀 → ESC가 안전한 짝
- TAB은 base layer 그대로 두어 Cmd+TAB 같은 빈번 단축키 손가락 부담 없음

### SPACE/ENTER에 layer-tap을 두지 않은 이유

초기 제안: `&lt 1 SPACE` (= Lily58의 `spc_ctrl_morph` 동작 — SPACE hold가 layer 1)

기각:
- SPACE는 자주 누름 + 꾹 누름 (게임/느린 타이핑 시 의도치 않은 hold)
- Layer 의도와 SPACE 입력 의도가 자주 충돌
- 그래서 SPACE/ENTER는 순수 `&kp`로 유지, layer는 dedicated `&mo` 키로만

### `&lt 1 LALT`를 안 쓴 이유

대안 검토: LALT 위치에 layer-tap을 둬서 hold=mo 1, tap=LALT 동시 사용.

기각 이유:
- Mac에서 LALT 단독 keypress는 modifier로 동작하지 않음 (다른 키와 chord 안 되면 무의미)
- tap 자리는 사실상 낭비 → 그냥 `&mo 1`과 거의 동일
- 단순성 우선 → 그냥 dedicated `&mo`

## Lower Layer (mo 1, 우측 thumb outer)

```
┌────┬────┬────┬────┬────┬────┐         ┌────┬────┬────┬────┬────┬────┐
│ ___│ 1  │ 2  │ 3  │ 4  │ 5  │         │ 6  │ 7  │ 8  │ 9  │ 0  │ `  │
├────┼────┼────┼────┼────┼────┤         ├────┼────┼────┼────┼────┼────┤
│ ___│ !  │ @  │ #  │ $  │ %  │         │ ^  │ &  │ *  │ (  │ )  │ ~  │
├────┼────┼────┼────┼────┼────┤         ├────┼────┼────┼────┼────┼────┤
│ ___│ `  │ ~  │ |  │ =  │ +  │         │ [  │ ]  │ {  │ }  │ \  │ ___│
└────┴────┴────┼────┼────┼────┤         ├────┼────┼────┼────┴────┴────┘
               │ ___│ ___│ ___│         │ ___│ ___│ ___│
               └────┴────┴────┘         └────┴────┴────┘
```

### 한 layer에 숫자 + 모든 특수문자 몰아넣기

코딩에 필수인 brackets/braces/pipe/backslash까지 모두 Lower 한 곳에 배치.
Raise layer를 nav 전용으로 비울 수 있음.

### 알려진 빈틈

`-` (minus) 키가 base에 없음. Shift+`=`는 `_`을 줌. 만약 `-`을 base나 Lower
어딘가에 두고 싶으면 빈 자리에 `&kp MINUS` 추가.

## Raise Layer (mo 2, 우측 thumb mid)

```
┌────┬────┬────┬────┬────┬────┐         ┌────┬────┬────┬────┬────┬────┐
│ F1 │ F2 │ F3 │ F4 │ F5 │ F6 │         │ F7 │ F8 │ F9 │F10 │F11 │F12 │
├────┼────┼────┼────┼────┼────┤         ├────┼────┼────┼────┼────┼────┤
│ ___│ ___│ ___│ ___│ ___│ ___│         │ ___│ ___│ ___│ ___│ ↑  │ ___│
├────┼────┼────┼────┼────┼────┤         ├────┼────┼────┼────┼────┼────┤
│ ___│ ___│ ___│ ___│ ___│ ___│         │ ___│ ___│ ___│ ←  │ ↓  │ →  │
└────┴────┴────┼────┼────┼────┤         ├────┼────┼────┼────┴────┴────┘
               │ ___│ ___│ ___│         │ ___│ ___│ ___│
               └────┴────┴────┘         └────┴────┴────┘
```

- **F1~F12** 최상단 12자리 전체 (외곽 pinky까지)
- **화살표**는 Lily58 inverted-T 위치 그대로: UP을 P 칼럼, LEFT/DOWN/RIGHT을 L/;/' 칼럼
- 양손 chord 없이 single thumb hold로 빈번한 nav 처리 가능

## Adjust Layer (conditional mo 1 + mo 2)

```
┌────┬────┬────┬────┬────┬────┐         ┌────┬────┬────┬────┬────┬────┐
│BCLR│BT0 │BT1 │BT2 │BT3 │BT4 │         │BTD0│BTD1│BTD2│BTD3│BTD4│ ___│
├────┼────┼────┼────┼────┼────┤         ├────┼────┼────┼────┼────┼────┤
│ ___│ ___│ ___│ ___│ ___│ ___│         │ ___│ ___│ ___│ ___│ ___│ ___│
├────┼────┼────┼────┼────┼────┤         ├────┼────┼────┼────┼────┼────┤
│ ___│ ___│ ___│ ___│ ___│ ___│         │ ___│ ___│ ___│ ___│ ___│ ___│
└────┴────┴────┼────┼────┼────┤         ├────┼────┼────┼────┴────┴────┘
               │ ___│ ___│ ___│         │ ___│ ___│ ___│
               └────┴────┴────┘         └────┴────┴────┘
```

ZMK `conditional_layers`로 mo 1과 mo 2를 동시에 hold하면 자동 활성화:

```c
conditional_layers {
    adjust {
        if-layers = <1 2>;
        then-layer = <3>;
    };
};
```

가끔만 쓰는 BT 페어링/연결/해제 전용. 두 thumb 동시 hold가 부담스럽지 않을
정도로 빈도가 낮은 기능들만 모아둠. 필요시 시스템 리셋, 매크로 등 추가 가능.

## Behavior 튜닝

```c
&mt {
    tapping-term-ms = <200>;
    flavor = "tap-preferred";
    require-prior-idle-ms = <125>;
};
```

- `tap-preferred` flavor: pinky가 느리므로 quick tap이 hold로 오인되는 사고를
  방지. ESC 의도가 Ctrl로 인식되면 곤란해서 tap 우선.
- `require-prior-idle-ms = 125`: 직전 키 입력 후 125ms 안에 누른 mod-tap은
  무조건 tap으로 해석. 빠른 타이핑 중 사고 방지.
- 실제 써보고 ESC 의도가 Ctrl로 잘못 발화되면 `tapping-term-ms`를 줄이거나
  `require-prior-idle-ms`를 늘릴 것.

## 파일 구성

- `config/corne.keymap` — 메인 keymap
- `config/corne.keymap.bak` — 원래 reference keymap 백업 (`*.bak`은 gitignore)
- `config/corne.conf` — board 설정 (RGB underglow는 현재 비활성)
- `build.yaml` — Zephyr 4.1 board variant 문법 사용: `nice_nano//zmk`

## 빌드 인프라 메모

ZMK가 2025-12-09에 Zephyr 4.1로 마이그레이션. 영향:

- Board 이름 체계 변경: `nice_nano_v2` → `nice_nano//zmk` (revision 2.0.0이
  기본값 + `//zmk` variant suffix 필수)
- `west.yml`이 `revision: main`이므로 항상 최신 ZMK를 받아옴 → 자동으로 새
  규칙 적용
- 안정성이 필요하면 `revision: v0.3` 같은 태그로 pin 가능 (대신 새 기능/수정
  못 받음)
- 참고: https://zmk.dev/blog/2025/12/09/zephyr-4-1#zmk-board-variant
