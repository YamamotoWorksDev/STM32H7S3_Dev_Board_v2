# rev.D 保存履歴のGit移行・コミット計画

作成日: 2026-11-06。未コミットの実行前計画。

移行元: `C:\Users\shun\STM32H7S3_Dev_Board\STM32H7S3_Dev_Board_revD`
移行先: `C:\Users\shun\STM32H7S3_Dev_Board_v2`
作業ブランチ: `codex/recover-revd-history`
既存のREADME・LICENSEと既存2コミットを維持する。

## 復元範囲と限界

- 2026-10-29～2026-11-06の88段階を復元する。初期保存状態1件、採用済み結果50件、保存済み差分37件。管理設定と本計画のコミット1件を加え、予定コミットは89件。
- 10月27～28日の配置・ファンアウト・XSPI初期配線の、独立した編集可能なPCB原本は今回の保存範囲から復元しない。最初のPCBにはそれらの結果が含まれる。
- 初期段階はPCB・設計書・設定だけの状態を含む。各回路図やライブラリは原本が確認できる段階から追加し、後の版を過去の状態へ流用しない。すべてのコミットが回路図を含む完全なプロジェクトとは限らない。
- レビュー文書の旧版（SHA-256: `2771267da87971aa02618ac3864902222bf093deab4720c176bc8150e702be1b`）は原本未発見。推測で復元せず、最初に確認できる版から取り込む。
- 保存状態間の変更者・正確な時刻を特定できない差分は「保存済み調整」として区別する。これにはユーザーの手修正や保存形式・ベタ充填・文書の更新を含む場合がある。
- コミット日時と作者を過去へ偽装しない。以下に記載した日本語メッセージ全文を使用する。
- 未採用案、途中のルータ候補、検討だけの作業は取り込まない。同じ内容のファイルはハッシュで同一性を確認する。
- `.work`一式、PNG・ログ・一時ファイル・自動生成物は移行しない。復元用データは一時フォルダーに保管し、元の保存履歴は削除しない。

## 検証結果

- 採用した履歴に必要な原本144個をSHA-256で照合し、一時保管した。
- 各段階の対象ファイルだけを順番に適用する再現試験を行い、全段階の状態ハッシュと一致した。
- 最後の段階は現在の移行元の設計ファイル11個とバイト単位で一致する。元フォルダーへの書き込みは行っていない。
- PCBの保存形式差・ベタ充填だけの変化は、可能な範囲で次の変更へまとめた。比較には配線・ビア・部品・パッド・フィールド・ゾーン定義の情報を用いた。
- 過去の未配線途中の状態も含むため、全履歴のERC/DRC合格を意味しない。履歴移行のために過去の設計内容を修正していない。
- `.gitattributes`でKiCad原本と既存の設計・レビュー文書の改行自動変換を無効にし、Gitへの登録・取り出しでも保存原本のバイト列を維持する。
- 現在の設計の検証状態は移行前と同じ。最後のFLASH修正時はDRC175件（シルク等の既存指摘）、未配線0件。

## コミット対象ファイル一覧

- `.gitattributes`
- `.gitignore`
- `Docs/history/recovery-manifest.json`
- `Docs/history/recovery-plan.md`
- `JUMBLEQ_mainboard_revD_design.md`
- `JUMBLEQ_mainboard_revD_review.md`
- `STM32H7S3_Dev_Board.kicad_dru`
- `STM32H7S3_Dev_Board.kicad_pcb`
- `STM32H7S3_Dev_Board.kicad_pro`
- `STM32H7S3_Dev_Board.kicad_sch`
- `dsp.kicad_sch`
- `mcu.kicad_sch`
- `power.kicad_sch`
- `revD_new_symbols.kicad_sym`
- `usb.kicad_sch`

## コミット順序・対象・メッセージ全文

各項目のコードブロックが予定コミットメッセージの全文。実行にはこの一覧に対するユーザー承認が必要。GitHubへのpushは別承認とする。

### 001 rev.Dの履歴復元方針と管理対象を定義

対象: `.gitattributes`, `.gitignore`, `Docs/history/recovery-plan.md`, `Docs/history/recovery-manifest.json`

```text
rev.Dの履歴復元方針と管理対象を定義

保存記録とハッシュから再構成する履歴の対象・順序・制約を記録する。
作業用データとKiCadの一時ファイル・生成物をGit管理から除外する。
KiCad原本と既存設計文書の改行を保存し、Gitの自動変換による原本差異を防ぐ。
```

### 002 rev.Dの最初に確認できる保存状態を取り込み

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_dru`, `STM32H7S3_Dev_Board.kicad_pcb`, `STM32H7S3_Dev_Board.kicad_pro`

```text
rev.Dの最初に確認できる保存状態を取り込み

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/decoupling-rework
最初に原本を確認できるPCB・設計書・設定を取り込む。回路図は原本を確認できる時点から追加する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/decoupling-rework/before.kicad_pcb`, `STM32H7S3_Dev_Board.kicad_pro` ← `.work/decoupling-rework/after.kicad_pro`, `STM32H7S3_Dev_Board.kicad_dru` ← `.work/decoupling-rework/after.kicad_dru`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/decoupling-rework/before-design.md`

### 003 MCUデカップリング部品の配置と配線を整理

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
MCUデカップリング部品の配置と配線を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/decoupling-rework
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/decoupling-rework/after.kicad_pcb`

### 004 保存済み調整を反映（u11-decoupling-20261029開始前）

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（u11-decoupling-20261029開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u11-decoupling-20261029
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u11-decoupling-20261029/before.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/u11-decoupling-20261029/before-design.md`

### 005 PSRAMのデカップリング配置を改善

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
PSRAMのデカップリング配置を改善

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u11-decoupling-20261029
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u11-decoupling-20261029/after.kicad_pcb`

### 006 保存済み調整を反映（u1-c24-c25-20261029開始前）

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（u1-c24-c25-20261029開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u1-c24-c25-20261029
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u1-c24-c25-20261029/before.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/u1-c24-c25-20261029/before-design.md`

### 007 FLASHのC24・C25周辺配線を整理

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
FLASHのC24・C25周辺配線を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u1-c24-c25-20261029
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u1-c24-c25-20261029/after.kicad_pcb`

### 008 保存済み調整を反映（u1-c3-20261029開始前）

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（u1-c3-20261029開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u1-c3-20261029
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u1-c3-20261029/before.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/u1-c3-20261029/before-design.md`

### 009 FLASHのC3を再配置

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
FLASHのC3を再配置

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u1-c3-20261029
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u1-c3-20261029/after.kicad_pcb`

### 010 保存済み調整を反映（u2-c18-c30-20261029開始前）

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（u2-c18-c30-20261029開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u2-c18-c30-20261029
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u2-c18-c30-20261029/before.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/u2-c18-c30-20261029/before-design.md`

### 011 MCUのC18・C30を再配置

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
MCUのC18・C30を再配置

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u2-c18-c30-20261029
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u2-c18-c30-20261029/after.kicad_pcb`

### 012 保存済み調整を反映（u2-five-power-20261029開始前）

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（u2-five-power-20261029開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u2-five-power-20261029
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u2-five-power-20261029/before.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/u2-five-power-20261029/before-design.md`

### 013 MCU電源ピン周辺を整理しC88を追加

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`, `mcu.kicad_sch`

```text
MCU電源ピン周辺を整理しC88を追加

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u2-five-power-20261029
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u2-five-power-20261029/stage/STM32H7S3_Dev_Board.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/u2-five-power-20261029/after-design.md`, `mcu.kicad_sch` ← `.work/u2-five-power-20261029/stage/mcu.kicad_sch`

### 014 保存済み調整を反映（power-routing-20261029開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（power-routing-20261029開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/power-routing-20261029
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/power-routing-20261029/before.kicad_pcb`

### 015 電源経路とデカップリング配線を整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
電源経路とデカップリング配線を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/power-routing-20261029
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/power-routing-20261029/polished.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/power-routing-20261029/after-design.md`

### 016 保存済み調整を反映（input-vbus-20261029開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`, `STM32H7S3_Dev_Board.kicad_sch`, `dsp.kicad_sch`, `power.kicad_sch`, `usb.kicad_sch`

```text
保存済み調整を反映（input-vbus-20261029開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/input-vbus-20261029
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/input-vbus-20261029/before.kicad_pcb`, `dsp.kicad_sch` ← `.work/input-vbus-20261029/dsp.kicad_sch`, `power.kicad_sch` ← `.work/input-vbus-20261029/power.kicad_sch`, `STM32H7S3_Dev_Board.kicad_sch` ← `.work/input-vbus-20261029/STM32H7S3_Dev_Board.kicad_sch`, `usb.kicad_sch` ← `.work/input-vbus-20261029/usb.kicad_sch`

### 017 入力電源とVBUS経路を整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
入力電源とVBUS経路を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/input-vbus-20261029
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/input-vbus-20261029/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/input-vbus-20261029/after-design.md`

### 018 保存済み調整を反映（c68-solid-20261029開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`, `STM32H7S3_Dev_Board.kicad_pro`

```text
保存済み調整を反映（c68-solid-20261029開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/c68-solid-20261029
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/c68-solid-20261029/before.kicad_pcb`, `STM32H7S3_Dev_Board.kicad_pro` ← `.work/c68-solid-20261029/after.kicad_pro`

### 019 C68周辺の電源接続を改善

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
C68周辺の電源接続を改善

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/c68-solid-20261029
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/c68-solid-20261029/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/c68-solid-20261029/after-design.md`

### 020 保存済み調整を反映（power-zone-cleanup-20261029開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（power-zone-cleanup-20261029開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/power-zone-cleanup-20261029
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/power-zone-cleanup-20261029/latest.kicad_pcb`

### 021 電源ゾーンと未使用銅箔を整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
電源ゾーンと未使用銅箔を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/power-zone-cleanup-20261029
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/power-zone-cleanup-20261029/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/power-zone-cleanup-20261029/after-design.md`

### 022 保存済み調整を反映（ground-axis-20261029開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（ground-axis-20261029開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/ground-axis-20261029
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/ground-axis-20261029/before.kicad_pcb`

### 023 GND接続と引き出し形状を整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
GND接続と引き出し形状を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/ground-axis-20261029
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/ground-axis-20261029/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/ground-axis-20261029/after-design.md`

### 024 保存済み調整を反映（u5-output-20261030開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（u5-output-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u5-output-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u5-output-20261030/before.kicad_pcb`

### 025 U5の出力コンデンサ周辺を整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
U5の出力コンデンサ周辺を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u5-output-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u5-output-20261030/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/u5-output-20261030/after-design.md`

### 026 5V_SYSを内層の電源ゾーンへ移設

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
5V_SYSを内層の電源ゾーンへ移設

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/sys-inner-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/sys-inner-20261030/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/sys-inner-20261030/after-design.md`

### 027 保存済み調整を反映（c67-axis-20261030開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（c67-axis-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/c67-axis-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/c67-axis-20261030/before.kicad_pcb`

### 028 C67の接続形状を整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
C67の接続形状を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/c67-axis-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/c67-axis-20261030/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/c67-axis-20261030/after-design.md`

### 029 保存済み調整を反映（ucpd-width-review-20261030開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（ucpd-width-review-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/ucpd-width-review-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/ucpd-width-review-20261030/baseline.kicad_pcb`

### 030 保存済み調整を反映（usb-fs-20261030開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（usb-fs-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usb-fs-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/usb-fs-20261030/before.kicad_pcb`

### 031 U14周辺のUSB FS配線を引き直し

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
U14周辺のUSB FS配線を引き直し

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usb-fs-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/usb-fs-20261030/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/usb-fs-20261030/after-design.md`

### 032 保存済み調整を反映（usb-fs-u14-tight-20261030開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（usb-fs-u14-tight-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usb-fs-u14-tight-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/usb-fs-u14-tight-20261030/before.kicad_pcb`

### 033 U14のUSB FS差動配線を改善

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
U14のUSB FS差動配線を改善

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usb-fs-u14-tight-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/usb-fs-u14-tight-20261030/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/usb-fs-u14-tight-20261030/after-design.md`

### 034 保存済み調整を反映（5vin-review-20261030開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（5vin-review-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/5vin-review-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/5vin-review-20261030/current.kicad_pcb`

### 035 保存済み調整を反映（vbus-zones-review-20261030開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（vbus-zones-review-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/vbus-zones-review-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/vbus-zones-review-20261030/current.kicad_pcb`

### 036 保存済み調整を反映（power-cleanup-review-20261030開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（power-cleanup-review-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/power-cleanup-review-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/power-cleanup-review-20261030/current.kicad_pcb`

### 037 保存済み調整を反映（oscillators-20261030開始前）

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pro`, `mcu.kicad_sch`, `power.kicad_sch`

```text
保存済み調整を反映（oscillators-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/oscillators-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pro` ← `.work/oscillators-20261030/after.kicad_pro`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/oscillators-20261030/before-design.md`, `mcu.kicad_sch` ← `.work/adc-j1-routing-20261103/source/mcu.kicad_sch`, `power.kicad_sch` ← `power.kicad_sch`

### 038 X1・X2の配置とクロック配線を整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
X1・X2の配置とクロック配線を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/oscillators-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/oscillators-20261030/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/oscillators-20261030/after-design.md`

### 039 保存済み調整を反映（usb-bga-review-20261030開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（usb-bga-review-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usb-bga-review-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/usb-bga-review-20261030/before.kicad_pcb`

### 040 USBのBGA引き出し線幅を見直し

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
USBのBGA引き出し線幅を見直し

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usb-bga-review-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/usb-bga-review-20261030/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/usb-bga-review-20261030/after-design.md`

### 041 PSRAM配線の線幅と経路を見直し

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
PSRAM配線の線幅と経路を見直し

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/psram-width-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/psram-width-20261030/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/psram-width-20261030/after-design.md`

### 042 FLASH配線の線幅と経路を見直し

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
FLASH配線の線幅と経路を見直し

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/flash-width-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/flash-width-20261030/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/flash-width-20261030/after-design.md`

### 043 保存済み調整を反映（u2-manual-review-20261030開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（u2-manual-review-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u2-manual-review-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u2-manual-review-20261030/filled.kicad_pcb`

### 044 保存済み調整を反映（dvdd-r27-recheck-20261030開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（dvdd-r27-recheck-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/dvdd-r27-recheck-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/dvdd-r27-recheck-20261030/filled.kicad_pcb`

### 045 DVDD周辺とR27付近の不要配線を整理

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
DVDD周辺とR27付近の不要配線を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/dvdd-r27-cleanup-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/dvdd-r27-cleanup-20261030/after.kicad_pcb`

### 046 SWD・SAI・SPIとDSP周辺を配線

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
SWD・SAI・SPIとDSP周辺を配線

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/digital-routing-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/digital-routing-20261030/final.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/digital-routing-20261030/after-design.md`

### 047 配線の幅変更と不要な曲げを整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
配線の幅変更と不要な曲げを整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/routing-style-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/routing-style-20261030/phase4.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/routing-style-20261030/after-design.md`

### 048 保存済み調整を反映（usb-control-routing-20261030開始前）

対象: `revD_new_symbols.kicad_sym`

```text
保存済み調整を反映（usb-control-routing-20261030開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usb-control-routing-20261030
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `revD_new_symbols.kicad_sym` ← `revD_new_symbols.kicad_sym`

### 049 J9・J10・U7の残配線を接続

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
J9・J10・U7の残配線を接続

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usb-control-routing-20261030
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/usb-control-routing-20261030/wide.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/usb-control-routing-20261030/after-design.md`

### 050 ビア近傍の配線形状を整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
ビア近傍の配線形状を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usb-via-cleanup-20261031
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/usb-via-cleanup-20261031/base.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/usb-via-cleanup-20261031/after-design.md`

### 051 保存済み調整を反映（hostcc2-width-20261031開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（hostcc2-width-20261031開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/hostcc2-width-20261031
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/hostcc2-width-20261031/before.kicad_pcb`

### 052 HOST_CC2_DETの線幅と経路を改善

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
HOST_CC2_DETの線幅と経路を改善

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/hostcc2-width-20261031
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/hostcc2-width-20261031/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/hostcc2-width-20261031/after-design.md`

### 053 保存済み調整を反映（hostcc2-restore-20261031開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（hostcc2-restore-20261031開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/hostcc2-restore-20261031
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/hostcc2-restore-20261031/before.kicad_pcb`

### 054 手動保存後のHOST_CC2_DET修正を再反映

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
手動保存後のHOST_CC2_DET修正を再反映

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/hostcc2-restore-20261031
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/hostcc2-restore-20261031/after.kicad_pcb`

### 055 保存済み調整を反映（ucpd-isense-route-20261102開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（ucpd-isense-route-20261102開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/ucpd-isense-route-20261102
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/ucpd-isense-route-20261102/before.kicad_pcb`

### 056 UCPD_ISENSEの経路を整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
UCPD_ISENSEの経路を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/ucpd-isense-route-20261102
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/ucpd-isense-route-20261102/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/ucpd-isense-route-20261102/after-design.md`

### 057 保存済み調整を反映（dvdd-inner-20261102開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（dvdd-inner-20261102開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/dvdd-inner-20261102
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/dvdd-inner-20261102/before.kicad_pcb`

### 058 DVDDを内層へ移設しDSP周辺信号を整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
DVDDを内層へ移設しDSP周辺信号を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/dvdd-inner-20261102
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/dvdd-inner-20261102/after.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/dvdd-inner-20261102/after-design.md`

### 059 保存済み調整を反映（usbi-ss-review-20261102開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（usbi-ss-review-20261102開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usbi-ss-review-20261102
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/usbi-ss-review-20261102/before.kicad_pcb`

### 060 保存済み調整を反映（usbi-sclk-review-20261102開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（usbi-sclk-review-20261102開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usbi-sclk-review-20261102
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/usbi-sclk-review-20261102/before.kicad_pcb`

### 061 保存済み調整を反映（status-control-20261102開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（status-control-20261102開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/status-control-20261102
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/status-control-20261102/before.kicad_pcb`

### 062 電源状態とホスト電源制御とLEDを配線

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
電源状態とホスト電源制御とLEDを配線

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/status-control-20261102
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/status-control-20261102/final.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/status-control-20261102/after-design.md`

### 063 保存済み調整を反映（reset-switch-swap-20261102開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（reset-switch-swap-20261102開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/reset-switch-swap-20261102
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/reset-switch-swap-20261102/before.kicad_pcb`

### 064 SW4とSW1の配置と配線を変更

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
SW4とSW1の配置と配線を変更

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/reset-switch-relocation-20261102
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/reset-switch-relocation-20261102/final.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/reset-switch-relocation-20261102/after-design.md`

### 065 MCUとJ3のRMII・SDMMC配線を接続

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
MCUとJ3のRMII・SDMMC配線を接続

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/u2-j3-routing-20261102
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/u2-j3-routing-20261102/final.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/u2-j3-routing-20261102/final-design.md`

### 066 保存済み調整を反映（d2-width-review-20261103開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（d2-width-review-20261103開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/d2-width-review-20261103
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/d2-width-review-20261103/before.kicad_pcb`

### 067 SDMMC周辺の細線区間と迂回を見直し

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
SDMMC周辺の細線区間と迂回を見直し

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/d2-width-review-20261103
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/d2-width-review-20261103/final.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/d2-width-review-20261103/final-design.md`

### 068 SDMMCのD0からD3の信号ビア数を統一

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
SDMMCのD0からD3の信号ビア数を統一

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/sdmmc-four-vias-20261103
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/sdmmc-four-vias-20261103/final.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/sdmmc-four-vias-20261103/final-design.md`

### 069 保存済み調整を反映（adc-j1-routing-20261103開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（adc-j1-routing-20261103開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/adc-j1-routing-20261103
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/adc-j1-routing-20261103/source/STM32H7S3_Dev_Board.kicad_pcb`

### 070 J1を14ピンへ拡張しADC入力を配線

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`, `mcu.kicad_sch`

```text
J1を14ピンへ拡張しADC入力を配線

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/adc-j1-routing-20261103
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/adc-j1-routing-20261103/final.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/adc-j1-routing-20261103/JUMBLEQ_mainboard_revD_design.md`, `mcu.kicad_sch` ← `.work/adc-j1-routing-20261103/mcu.kicad_sch`

### 071 J1・J6を移動しJ1のGNDピンを変更

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`, `mcu.kicad_sch`

```text
J1・J6を移動しJ1のGNDピンを変更

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/j1-j6-shift-20261104
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/j1-j6-shift-20261104/final.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/j1-j6-shift-20261104/JUMBLEQ_mainboard_revD_design.md`, `mcu.kicad_sch` ← `.work/j1-j6-shift-20261104/mcu.kicad_sch`

### 072 ADC1_IN10の線幅を統一

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
ADC1_IN10の線幅を統一

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/adc-in10-width-20261104
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/adc-in10-width-20261104/routed.kicad_pcb`

### 073 ADC1_IN5の曲がりと線幅を整理

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
ADC1_IN5の曲がりと線幅を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/adc-in5-style-20261104
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/adc-in5-style-20261104/routed.kicad_pcb`

### 074 USBi_SSの線幅と経路を整理

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
USBi_SSの線幅と経路を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/usbi-ss-style-20261104
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/usbi-ss-style-20261104/routed.kicad_pcb`

### 075 ADC1_IN3の線幅変更を整理

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
ADC1_IN3の線幅変更を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/adc-in3-style-20261104
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/adc-in3-style-20261104/routed.kicad_pcb`

### 076 ETH_MDIOの線幅と経路を整理

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
ETH_MDIOの線幅と経路を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/eth-mdio-style-20261104
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/eth-mdio-style-20261104/routed.kicad_pcb`

### 077 基板全体の線幅と経路を整理

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
基板全体の線幅と経路を整理

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/board-style-review-20261104
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/board-style-review-20261104/final.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/board-style-review-20261104/after-design.md`

### 078 FLASHのECSとRESET周辺配線を完了

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
FLASHのECSとRESET周辺配線を完了

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/flash-control-completion-20261104
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/flash-control-completion-20261104/final.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/flash-control-completion-20261104/final-design.md`

### 079 I2C2_SDAの重複配線を除去

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
I2C2_SDAの重複配線を除去

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/i2c-sda-overlap-20261104
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/i2c-sda-overlap-20261104/final.kicad_pcb`

### 080 保存済み調整を反映（remaining-routing-20261104開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（remaining-routing-20261104開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/remaining-routing-20261104
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/remaining-routing-20261104/before.kicad_pcb`

### 081 J2・スイッチ・LEDの残配線を完了

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
J2・スイッチ・LEDの残配線を完了

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/remaining-routing-20261104
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/remaining-routing-20261104/final.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/remaining-routing-20261104/after-design.md`

### 082 保存済み調整を反映（sai-pad-clearance-20261105開始前）

対象: `JUMBLEQ_mainboard_revD_review.md`

```text
保存済み調整を反映（sai-pad-clearance-20261105開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/sai-pad-clearance-20261105
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `JUMBLEQ_mainboard_revD_review.md` ← `.work/sai-pad-clearance-20261105/before-JUMBLEQ_mainboard_revD_review.md`

### 083 SAI経路とC28・R3周辺を改善

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
SAI経路とC28・R3周辺を改善

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/sai-pad-clearance-20261105
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/sai-pad-clearance-20261105/review-after/STM32H7S3_Dev_Board.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/sai-pad-clearance-20261105/review-after/JUMBLEQ_mainboard_revD_design.md`

### 084 LED1・LED2のMCUピン割当てを変更

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`, `mcu.kicad_sch`

```text
LED1・LED2のMCUピン割当てを変更

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/led-pin-relocation-20261105
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/led-pin-relocation-20261105/review-after/STM32H7S3_Dev_Board.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/led-pin-relocation-20261105/review-after/JUMBLEQ_mainboard_revD_design.md`, `mcu.kicad_sch` ← `.work/led-pin-relocation-20261105/mcu.kicad_sch`

### 085 保存済み調整を反映（sai2b-schematic-20261105開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（sai2b-schematic-20261105開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/sai2b-schematic-20261105
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/sai2b-schematic-20261105/source/STM32H7S3_Dev_Board.kicad_pcb`

### 086 SAI2BのE11・C12・B12へ回路図を変更

対象: `STM32H7S3_Dev_Board.kicad_sch`, `mcu.kicad_sch`

```text
SAI2BのE11・C12・B12へ回路図を変更

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/sai2b-schematic-20261105
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `mcu.kicad_sch` ← `.work/sai2b-schematic-20261105/mcu.kicad_sch`, `STM32H7S3_Dev_Board.kicad_sch` ← `.work/sai2b-schematic-20261105/STM32H7S3_Dev_Board.kicad_sch`

### 087 保存済み調整を反映（sai2b-routing-20261105開始前）

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
保存済み調整を反映（sai2b-routing-20261105開始前）

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/sai2b-routing-20261105
前回の採用結果と次の確認済み保存状態の差分を反映する。変更者・詳細時刻は断定しない。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/sai2b-routing-20261105/source/STM32H7S3_Dev_Board.kicad_pcb`

### 088 旧SAI配線を撤去しSAI2Bを配線

対象: `JUMBLEQ_mainboard_revD_design.md`, `STM32H7S3_Dev_Board.kicad_pcb`

```text
旧SAI配線を撤去しSAI2Bを配線

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/sai2b-routing-20261105
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/sai2b-routing-20261105/review-after/STM32H7S3_Dev_Board.kicad_pcb`, `JUMBLEQ_mainboard_revD_design.md` ← `.work/sai2b-routing-20261105/review-after/JUMBLEQ_mainboard_revD_design.md`

### 089 FLASHのクリアランス不足と重複配線端を修正

対象: `STM32H7S3_Dev_Board.kicad_pcb`

```text
FLASHのクリアランス不足と重複配線端を修正

過去の保存記録から再構成した履歴。元の作業日時・作者を再現するものではない。
参照記録: .work/flash-two-fixes-20261106
保存記録または検証済み採用ファイルに対応する結果を反映する。
```

原本: `STM32H7S3_Dev_Board.kicad_pcb` ← `.work/flash-two-fixes-20261106/final.kicad_pcb`
