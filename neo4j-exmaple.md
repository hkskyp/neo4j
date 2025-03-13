# csv 파일 로드

## db imoprt 폴더가 root

## 명령어
```console
LOAD CSV WITH HEADERS FROM "file:///r.csv" AS row 
MERGE (:R {id:row.ID, caption:row.CAPTION, tool:row.TOOL})
LOAD CSV WITH HEADERS FROM "file:///d.csv" AS row 
MERGE (:D {id:row.ID, caption:row.CAPTION, tool:row.TOOL})
LOAD CSV WITH HEADERS FROM "file:///t.csv" AS row 
MERGE (:T {id:row.ID, caption:row.CAPTION, tool:row.TOOL})
LOAD CSV WITH HEADERS FROM "file:///ssrs.csv" AS row 
MERGE (:SSRS {id:row.ID, caption:row.CAPTION})
```

## 로드 및 관계 설정
```console
LOAD CSV WITH HEADERS FROM "file:///srs.csv" AS row 
MATCH (ssrs:SSRS {id:row.SSRS}), (r:R {id:row.R})
MERGE (ssrs)-[:srs]->(r)

LOAD CSV WITH HEADERS FROM "file:///sdd.csv" AS row 
MATCH (r:R {id:row.R}), (d:D {id:row.D})
MERGE (r)-[:sdd]->(d)

LOAD CSV WITH HEADERS FROM "file:///std.csv" AS row 
MATCH (d:D {id:row.D}), (t:T {id:row.T})
MERGE (d)-[:std]->(t)
```

# 삭제

## 노드 및 관계 삭제
```console
MATCH (n)
DETACH DELETE n;
```

## 노드 및 관계 삭제
```console
MATCH (a{name:'Jane'})
DELETE a
```

## 속성 삭제
```console
MATCH (n)
WHERE n.name='Charlie'
REMOVE n.gender
RETURN n
```

# 검색
```console
MATCH (n:SSRS{id: "SSRS-001"})-[:srs{tool:"SW"}]->(r:R) RETURN n, r;
MATCH (n:SSRS)-[:srs{tool:"SW"}]->(r:R) RETURN n, r;
MATCH (ssrs:SSRS)<-[:srs]-(r:R)-[:sdd]->(d:D)-[:std]->(t:T) RETURN r, ssrs, d, t ORDER BY r.id;
MATCH (ssrs:SSRS)-[:srs]->(r:R)-[:sdd]->(d:D)-[:std]->(t:T)
WHERE d.id = "D-001"
RETURN r, ssrs, d, t 
ORDER BY r.id;
```
