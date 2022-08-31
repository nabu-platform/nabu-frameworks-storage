Querying a string as json in postgres:

```sql
select a.blob::json  -> 'menu' ->> 'id'
from
	(select '{"menu": {
  "id": "file",
  "value": "File",
  "popup": {
    "menuitem": [
      {"value": "New", "onclick": "CreateNewDoc()"},
      {"value": "Open", "onclick": "OpenDoc()"},
      {"value": "Close", "onclick": "CloseDoc()"}
    ]
  }
}}' as blob) a
where a.blob::json -> 'menu' ->> 'id' like '%il%'
``` 

Querying bytes as json in postgres:

```sql
select convert_from(a.blob, 'UTF8')::json  -> 'menu' ->> 'id'
from
	(select '{"menu": {
  "id": "file",
  "value": "File",
  "popup": {
    "menuitem": [
      {"value": "New", "onclick": "CreateNewDoc()"},
      {"value": "Open", "onclick": "OpenDoc()"},
      {"value": "Close", "onclick": "CloseDoc()"}
    ]
  }
}}'::bytea as blob) a
where convert_from(a.blob, 'UTF8')::json  -> 'menu' ->> 'id' like '%il%'
``` 



```sql
select convert_from(se.content, case when encoding is null then 'UTF-8' else encoding end)::json  -> 'menu' ->> 'id'
from
storage_entries se
where convert_from(se.content, case when encoding is null then 'UTF-8' else encoding end)::json -> 'menu' ->> 'id' like '%il%'
and se.content_type = 'application/json'
``` 