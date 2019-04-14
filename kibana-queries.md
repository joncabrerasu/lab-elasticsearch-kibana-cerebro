#Cluster Health
GET _cat/health

# Indices info
GET _cat/indices


# Index name: 'lab'

#Add document ad to index lab ('default' indicates type)
# By default ES creates 5 shards and 1 replica (be carefull with ES nodes created in your cluster)
POST /lab/default/1 
{
  "title": "First ad in lab ES index"
} 

# Get document
GET /lab/default/1

#Search documents from lab index
POST lab/_search
