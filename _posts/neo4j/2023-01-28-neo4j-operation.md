---
layout: post
title: neo4j base operation
subtitle: 
categories: programming-language
tags: [neo4j]
---

## start neo4j

```
Enter the command line : neo4j.bat console 
```

## create node

```
Enter a statement on $
user **create**
• creat a statement by CREATE.
• () means a node.
• a node include identifier and label.
• {} include attribute.
e.g. Create (r1:Rapper{name:"Pharaoh",Team:"Walkingdead"}) return r1;
e.g. Create (r2:Rapper:Producer:Composer{name:"Cloud"})
```

## creat contact

```
• –> means point to sth.
• [] include identifier, attribute etc.
• the realationship also have tag and attribute.
• use MATCH to connect realationship 
e.g. ## no attribute
    match(r2:Rapper{name:"Pharaoh"}),(r3:Rapper{name:"KeyNG"})
    create(r2)-[:队友]->(r3)
e.g. ## attribute
    match(r2:Rapper{name:"Pharaoh"}),(r3:Rapper{name:"KeyNG"})
    create(r2)-[:队友{时间:"2018年夏",共属的团队:"Walkingdead"}]->(r3)
```

## Search

```
MATCH 
(
   <node-name>:<label-name>
)
e.g. ## search Pharaoh
    MATCH (r2:Rapper{name:"Pharaoh"}) RETURN r2
e.g. ## search Pharaoh as Identifier
    MATCH (r2:Rapper{name:"Pharaoh"}) RETURN r2.Team
e.g. ## search Pharaoh
    MATCH p = (:Rapper{name:'Pharaoh'})-[]->() RETURN p
```

## Delete

```
## Create(r:Rapper{name:"6ix9ine",Team:"Blood"})

## delete by name
MATCH(r:Rapper{name:"6ix9ine"})
delete r

## delete by id
MATCH (r)
WHERE id(r) = 84
DELETE r
```

```
## delete node and contact
MATCH (r)
WHERE id(r) = 83
DETACH DELETE r

## delte all the node and realationship under a label
MATCH (r:Loc)
DETACH DELETE r

## deldte all
MATCH (r)
DETACH DELETE r
```

## Screen —— where

```
## 1
match (r)
where n.name="GAI"
return n

## 2
match (r:Rapper)
where r.name="GAI"
return r
```

1 is screen name="GAI“ before ergodic all the node, but when you have too much node that may screen slowly and use more memory space, use 2 can deal this problem ,which have 'label' to reduce the scope.

```
## find multiple nodes
## use or no and
match (r:Rapper)
where r.name='GAI' or r.name='Pharaoh'
return r
```

## Remove and Set

SET：add new attribute to existing node or contact
REMOVE：delete attribute to existiong node or contact

difference between DELETE  and REMOVE：

- DELETE use to delete node and contact

- REMOVE use to delete tags and acctribute

similarity of DELETE and REMOVE:

- they should't be used separately.

- they should be used with MATCH.

### Remove

delete attribute

```
## Create (:Basketballplayer{name:"George",Team:"Clipper",score:"89"})

match(b:Basketballplayer{name:"George"})
remove b.Team
return b
```

delete contact's attribute

```
## Create (:Singer{name:"Jay Chou"})-[:`合作`{`合作作品`:"歌曲 天地一斗"}]->(:Basketballplayer:{name:"Kobe",Team:"Lakers"})

match p= (:Singer)-[r]->(:Basketballplayer)
remove r.`合作作品`
return p
```

delete poiint's tags

```
## Create (m:Movie:Cinema:Picture)

MATCH (m:Movie) 
REMOVE m:Picture
return m
```

### Set

add new attribute

```
## Create (:Basketballplayer{name:"LeBron",Team:"Cavaliers",AKA:"King"})

match(b:Basketballplayer{name:"LeBron"})
set b.Team="Heat"
return b

match(b:Basketballplayer{name:"LeBron"})
set b.award="3 chmpions"
return b
```

add new attribute to existing concact

```
## match(r1:Rapper{name:"Buzzy"}),(r2:Rapper{name:"KeyNG"})
## create(r1) -[:`队友`{`共属的团队`:"Walkingdead"}]->(r2)

match p=(:Rapper{name:"Buzzy"})-[r]->(:Rapper{name:"KeyNG"})
set r.`时间`="2018年夏"
return p
```

## Sorting

```
match(b:Basketballplayer)
return b.name,b.Team,b.score
order by b.score
```

## Limit and skip

limit : Filter or limit the number of rows returned by the query.

skip : Ignore the first few lines.

**be used after return**

```
limit num.
skip num.
```

## merge

merge : Create point, realationship and attributes to retrieve data from the database

```
MERGE (<节点或关系的名称>:<节点或关系的标签名称>
{
   <节点或关系的属性名称>:<节点或关系的属性值>
   .....
     <节点或关系的属性名称>:<节点或关系的属性值>
})


e.g. merge(:Basketballplayer{name:"Love"})
```
