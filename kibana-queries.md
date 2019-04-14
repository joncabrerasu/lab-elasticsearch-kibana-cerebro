### Cluster Health
```
GET _cat/health
```

### Indices info
```
GET _cat/indices
```

### Index name: 'lab'

### Add document ad to index lab ('default' indicates type)
### By default ES creates 5 shards and 1 replica (be carefull with ES nodes created in your cluster)
```
POST /lab/default/1 
{
  "title": "First ad in lab ES index"
} 
```

### Add complex document ad to index lab
```
POST /lab/default/ 
{
  "title": "Complex ad in lab ES index",
  "type": "PROFESSIONAL",
  "category": "Mobile",
  "price": 750.25,
  "created_date": "2019-04-12"
} 
```
### Add documents via bulk process:
### Bulk
```
POST _bulk
{ "index" : { "_index" : "lab", "_type" : "default" } }
{ "title": "Complex ad created via bulk process", "type": "PRIVATE", "category": "Car", "price": "1400.90", "created_date": "2019-04-12" }
{ "index" : { "_index" : "lab", "_type": "default" } }
{ "title" : "Another Complex ad created via bulk process", "type" : "PROFESSIONAL", "category": "Games", "price": "15.20", "created_date": "2019-04-12" }
```

### Get document
```
GET /lab/default/1
```

### Get index config
```
GET lab
```
### Get index field mappings
```
GET lab/_mapping
```

### Search documents from lab index
```
POST lab/_search
```
