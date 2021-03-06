---
layout: post
title:  "MariaDB Hierarchy Query"
description: Maria DB 계층적 쿼리
date:   2020-12-13 14:31:44 +0530
categories: MariaDB
---

Maria DB 계층적 쿼리

```sql
WITH RECURSIVE CTE AS
(
  SELECT
    AREA_CAT_NO,
    AREA_CAT_NM,
    AREA_CAT_NM AS AREA_CAT_NM_PATH,
    PROT_RNK,
    1 AS LEVEL,
    USE_YN
  FROM TCP_AREA_CAT
  WHERE UPPR_AREA_CAT_NO = 0
  UNION ALL
  SELECT
    A.AREA_CAT_NO,
    A.AREA_CAT_NM,
    A.UPPR_AREA_CAT_NO,
    CONCAT(B.AREA_CAT_NM_PATH, '>', A.AREA_CAT_NM) AS AREA_CAT_NM_PATH,
    A.PROT_RNK,
    1 + B.LEVEL AS LEVEL,
    A.USE_YN
  FROM TCP_AREA_CAT A INNER JOIN CTE B
  ON (A.UPPR_AREA_CAT_NO = B.AREA_CAT_NO)
)
SELECT
  AREA_CAT_NO,
  UPPR_AREA_CAT_NO,
  CONCAT(REPEAT(' ', LEVEL * 4), AREA_CAT_NM) AS HIRC_AREA_CAT_NM,
  AREA_CAT_NM,
  AREA_CAT_NM_PATH,
  PROT_RNK,
  LEVEL,
  USE_YN
FROM CTE
ORDER BY AREA_CAT_NM_PATH
```

아직은 Oracle Connect By에 비하면 많이 비약하다;; 
