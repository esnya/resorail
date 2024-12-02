# ResoRail軌道スロット標準 1.0.0-rc.4

この文書における各キーワード「しなければならない（MUST）」、「してはならない（MUST NOT）」、「要求されている（REQUIRED）」、「することになる（SHALL）」、「することはない（SHALL NOT）」、「する必要がある（SHOULD）」、「しないほうがよい（SHOULD NOT）」、「推奨される（RECOMMENDED）」、「してもよい（MAY）」、「選択できる（OPTIONAL）」は、RFC 2119に記載されている内容に従い解釈してください。

## RosoRail軌道スロット仕様書

### 軌道スロット
車両が走行するためのスロットであり、本標準仕様の基本構成要素である。

#### Name
任意（OPTIONAL）。

#### Tag
"Rail"でなければならない（MUST）。

#### Origin
- X: 軌道中心にしなければならない（MUST）。
- Y: レール面にしなければならない（MUST）。
- Z: レール端または中心が望ましい（RECOMMENDED）。

#### Axis
- X: 枕木方向でなければならない（MUST）。
- Y: 上方向でなければならない（RECOMMENDED）。
- Z: 前後方向でなければならない（MUST）。特殊なレイアウトを除いて、連続する軌道で進行方向と同じにすることが要求される（REQUIRED）。

#### Scale
現実スケールにおいて (1, 1, 1) にすることが要求されている（REQUIRED）。

#### Components
Box Colliderをひとつ付与しなければならない（MUST）。

#### Box Collider
- Type: RaycastがHitするものでなければならない（MUST）。Staticにすることになる（SHALL）。
- Character Collider: 任意に選択できる（OPTIONAL）。
- Ignore Raycast: Falseでなければならない（MUST）。
- Offset:
    - X: 0にすることになる（SHALL）。
    - Y: コライダー上面をレール面から±200mm以内にしなければならない（MUST）。
    - Z: コライダー範囲がレール長と一致するようにしなければならない（MUST）。
- Size:
    - X: 800mm以上にする必要がある（SHOULD）。隣接する軌道の建築限界を支障してはならない（MUST NOT）。
    - Y: レール上面からコライダー下面まで200mm以上となるようにすることを推奨する（RECOMMENDED）。
    - Z: レール長と一致しなければならない（MUST）。

#### Notice
車両限界内からBox Colliderの間にRaycastを妨げるColliderが存在することはない（SHOULD NOT）。
LOD等を活用し軽量化に務める必要がある（SHOULD）。
分岐器などについては開通方向についてこの仕様を満たすように実装しなければならない（MUST）。

### 継ぎ目スロット
車両が継ぎ目通過音を発する位置を指定するために、継ぎ目スロットを設置してもよい（MAY）。

#### Name
音の種類を記述する（MUST）。通常の継ぎ目では"Normal Joint"、一般的な橋梁では"Bridge Joint"、一般的なトンネルでは"Tunnel Joint"としてもよい（MAY）。
特殊な音の種類を指定する場合、一般的な名称 + " " + 特殊な名前 + " " + "Joint" とすることを推奨する（RECOMMENDED）。

#### Tag
"Joint"でなければならない（MUST）。

#### Origin
- X: 軌道中心にしなければならない（MUST）。
- Y: レール面にしなければならない（MUST）。
- Z: 継ぎ目位置でなければならない（MUST）。

#### Axis
- X: 枕木方向でなければならない（MUST）。
- Y: 上方向でなければならない（RECOMMENDED）。
- Z: 前後方向でなければならない（MUST）。 設置する軌道スロットと同一であることが推奨される（RECOMMENDED）。

#### Scale
設置する軌道スロットと同一であることが要求される（REQUIRED）。

#### Components
Sphere Colliderをひとつ付与しなければならない（MUST）。

#### Shpere Collider
- Type: RaycastがHitするものでなければならない（MUST）。Staticにすることになる（SHALL）。
- Character Collider: 任意に選択できる（OPTIONAL）。
- Ignore Raycast: Falseでなければならない（MUST）。
- Offset:
    - X: 0である必要がある（SHOULD）。
    - Y: Sphere中心を軌道スロットのコライダー上面に合わせる必要がある（SHOULD）。
    - Z: 0である必要がある（SHOULD）。
- Radius: 0.15にすることになる（SHOULD）。

### Snap Points
Module Snapping Toolで配置できるようにするためにSnap Pointsスロットを追加しても良い（MAY）。
レール同士を接続するSnap Pointは、以下から適したものを任意の数だけ設定することが推奨される（RECOMMENDED）。

#### レールのZ+端に設置し、他のレールのZ-端と相互に接続できるSnapPoint
- Name: "Rail.F"でなければならない（MUST）。
- Tag: "Rail.B"でなければならない（MUST）。
- Position:
    - X: 0でなければならない（MUST）。
    - Y: 0でなければならない（MUST）。
    - Z: レールのZ+端でなければならない（MUST）。
- Rotation:
    - X: 0でなければならない（MUST）。
    - Y: 180でなければならない（MUST）。
    - Z: 0でなければならない（MUST）。
- Scale: (1, 1, 1)でなければならない（MUST）。

#### レールのZ-端に設置する、他のレールのZ+端と相互に接続できるSnapPoint
- Name: "Rail.B"でなければならない（MUST）。
- Tag: "Rail.F"でなければならない（MUST）。
- Position:
    - X: 0でなければならない（MUST）。
    - Y: 0でなければならない（MUST）。
    - Z: レールのZ-端でなければならない（MUST）。
- Rotation:
    - X: 0でなければならない（MUST）。
    - Y: 0でなければならない（MUST）。
    - Z: 0でなければならない（MUST）。
- Scale: (1, 1, 1)でなければならない（MUST）。

## 変更履歴
### 1.0.0 UNRELEASED
- 初版策定

### 1.0.0-rc.4 2023-11-27
- コライダー上面の規定を変更。

### 1.0.0-rc.3 2023-11-26
- 軌道Slotの方向について（REQUIRED）に変更。

### 1.0.0-rc.2  2023-11-26
- Snap Pointsの仕様を修正

### 1.0.0-rc.1 2023-11-26
- 継ぎ目スロットのName要件を変更
- 分岐器等に関する注釈を追加
