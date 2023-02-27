# opensearch snapshot restore

**절대!! path.repo 경로 지우면 안됨!!!!! (ex. /pnp_sys_log/opensearch/backup/)**

## Snapshot
기본적으로 모든 Authoriztion 설정 필요

* Username: 
* Password: 

### 1. Create snapshot repository
snapshot repository 생성

**`PUT`** > `http://{ip}:9200/_snapshot/backup_repo`
```json
{
  "type": "fs",
  "settings": {
    "location": "/pnp_sys_log/opensearch/backup"
  }
}
```

### 2. Backup
snapshot 생성 (백업)

**`PUT`** > `http://{ip}:9200/_snapshot/backup_repo/{원하는 index}?wait_for_completion=true`
```json
{
  "indices": "{원하는 index}",
  "ignore_unavailable": true,
  "include_global_state": false
}

// ex.
{
  "indices": "infolog_20230227",
  "ignore_unavailable": true,
  "include_global_state": false
}
```

### 3. Snapshot check
snapshot 확인

**`GET`** > `http://{ip}:9200/_snapshot/backup_repo/_all`

### 4. Restore
snapshot restore

**`POST`** > `http://{ip}:9200/_snapshot/backup_repo/{원하는 index}/_restore?wait_for_completion=true`

```json
{
  "indices": "{원하는 index}",
  "ignore_unavailable": false,
  "include_global_state": true
}

// ex.
{
  "indices": "infolog_20230227",
  "ignore_unavailable": false,
  "include_global_state": true
}
```
