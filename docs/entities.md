```mermaid
---
title: すべてのエンティティ
---
erDiagram
    StationLink
    Station
    Node
    Train
    Rail
    RailwayNetwork
    TrainSchedule
```

```mermaid
---
title: 鉄道網-関連
---
erDiagram
    StationLink_s ||--|o Station_s : look
    RailwayNetwork |o--|| StationLink_s : has
```

```mermaid
---
title: 運行計画
---
erDiagram
    StationLink_s ||--|o Station_s : look
    RailwayNetwork |o--|| StationLink_s : has
```
