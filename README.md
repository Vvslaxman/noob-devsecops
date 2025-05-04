# noob-devsecops

| Application Name | Observed Layers                      | Tier Type              | Enterprise-Grade? |
| ---------------- | ------------------------------------ | ---------------------- | ----------------- |
| **UPSI**         | Angular + ASP.NET Zero API + SQL DB  | 3-tier                 | Yes               |
| **MyInsider**    | ASP.NET MVC + Controllers + SQL DB   | 2-tier or light 3-tier | Moderate          |
| **MyEsops**      | Angular + WebAPI (.NET 4.8) + SQL DB | 3-tier                 | Yes               |

```scss
[Frontend UI]
     ↓ (API Call)
[API Controller]
     ↓
[Application Service]
     ↓
[Domain Service / Business Logic]
     ↓
[Repository]
     ↓
[SQL Server DB Table]
```


| App Name  | Frontend      | Backend         | Auth System   | DBs Used                   | Key Tables                   | New Client Flow         | Login Flow             |
| --------- | ------------- | --------------- | ------------- | -------------------------- | ---------------------------- | ----------------------- | ---------------------- |
| UPSI      | Angular       | ASP.NET Zero    | JWT (AbpAuth) | UPSI\_QA                   | AbpUsers, AbpTenants, Grants | Add to AbpTenants       | TokenAuth.Authenticate |
| MyEsops   | Angular + MVC | .NET 4.8 WebAPI | JWT or Cookie | ESOPSMain, ESOPSLog, etc.  | Users, Clients, Vesting      | Add to Clients table    | Token endpoint         |
| MyInsider | ASP.NET MVC   | Controllers     | Cookie-based  | Insider\_DB1, Insider\_DB2 | Users, Roles                 | Add to Companies table? | Home/Login             |

### DB queries for finding out most important tables
```sql
SELECT 
    referenced_entity_name, COUNT(*) AS ReferenceCount
FROM 
    sys.sql_expression_dependencies
GROUP BY 
    referenced_entity_name
ORDER BY 
    ReferenceCount DESC;
```

```sql
SELECT 
    OBJECT_NAME(i.object_id) AS TableName, 
    i.user_seeks + i.user_scans + i.user_lookups + i.user_updates AS AccessCount
FROM sys.dm_db_index_usage_stats i
WHERE database_id = DB_ID()
ORDER BY AccessCount DESC;
```
