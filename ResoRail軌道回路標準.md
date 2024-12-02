# ResoRail軌道回路標準 1.0.0

この文書における各キーワード「しなければならない（MUST）」、「してはならない（MUST NOT）」、「要求されている（REQUIRED）」、「することになる（SHALL）」、「することはない（SHALL NOT）」、「する必要がある（SHOULD）」、「しないほうがよい（SHOULD NOT）」、「推奨される（RECOMMENDED）」、「してもよい（MAY）」、「選択できる（OPTIONAL）」は、RFC 2119に記載されている内容に従い解釈してください。

## ResoRail軌道回路仕様書

軌道回路は線路上の特定区間に車両が存在することを検知するための仕組みである。本仕様書は、ResoRail軌道スロット標準と合わせて使用されることで、実装によらない軌道回路の実現を目指す。

### 軌道回路スロット

同一の軌道回路を構成する軌道スロットは、すべて同一の軌道回路スロットの子または子孫でなければならない（MUST）。

#### Name

軌道回路を識別する名前であることを推奨する（RECOMMENDED）。

#### Tag

"TrackCircuit"であることを推奨する（RECOMMENDED）。

#### Components

以下のコンポーネントを持たなければならない（MUST）。

##### DynamicVariableSpace

###### SpaceName

"TrackCircuit"でなければならない（MUST）。

###### OnlyDirectBinding

Trueにすることが推奨される（RECOMMENDED）。

##### DynamicValueVariable

`float`型でなければならない（MUST）。

###### VariableName

"TrackCircuit/T"でなければならない（MUST）。

### 車両の実装

車両は走行する軌道スロットに対して`WriteDynamicValueVariable<float>`を呼び出さなければならない（MUST）。引数は以下の通りである。

- VariableName: "TrackCircuit/T"でなければならない（MUST）。
- Value: `WorldTimeFloat`の値でなければならない（MUST）。

値の書き込みは1秒に1回以上行うことが要求される（REQUIRED）。ただし、車両及び軌道回路を利用する装置をすべて管理できる場合、応答性や負荷の調整のために増減しても良い（MAY）。

### 軌道回路の利用

軌道回路を利用する装置は、軌道回路に車両が存在することを以下の手順で判断しなければならない（MUST）。

- 軌道スロットからDynamicValueVariable "TrackCircuit/T"を取得する。
- `WorldTimeFloat`の値を取得する。
- それぞれの値を比較し、差の絶対値が2秒以下である場合は車両が存在すると判断する。
  - 複数の装置が同じ軌道回路を監視する場合、`bool` のDynamicValueVariableとして保存し、他の装置と共有することが推奨される（RECOMMENDED）。
  - 車両側の書き込み間隔を変更した場合、比較する値の差を変更することになる（SHOULD）。

### "TrackCircuit/T"の初期化

保存したセッションを再開したとき、特定の瞬間に列車が存在せずとも車両が存在することを誤検知する可能性がある。推奨の数値を使用している場合、これは4秒間のみ発生する。
この誤検知を回避するために、セッション開始時に"TrackCircuit/T"の値を初期化してもよい（MAY）。初期値としては0以下の値を設定することが推奨される（RECOMMENDED）。
