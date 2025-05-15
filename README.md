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
### Commonly used symbols in web.config file:

| Symbol | Name           | Meaning / Usage                                              | Context                                                           |
| ------ | -------------- | ------------------------------------------------------------ | ----------------------------------------------------------------- |
| `/`    | Forward Slash  | Path separator in **URLs** and **Unix-based file systems**   | URLs: `http://example.com/page`<br>Linux paths: `/var/www`        |
| `\`    | Backslash      | Path separator in **Windows file systems**                   | File path: `C:\MyFolder\MyFile.txt`                               |
| `~`    | Tilde          | Represents **application root** in ASP.NET                   | `~/Images/logo.png` refers to the `Images` folder at the app root |
| `:`    | Colon          | Used to separate **drive letters**, ports, or namespaces     | `C:\path`, `http://localhost:80`, `System.Web:`                   |
| `.`    | Dot / Period   | Refers to current directory or used in extensions/namespaces | File: `index.html`, Namespace: `System.IO`                        |
| `..`   | Double Dot     | Refers to **parent directory**                               | `../folder/file.txt` means "go up one level"                      |
| `*`    | Asterisk       | Wildcard to match any string                                 | IIS handler paths like `path="*"`                                 |
| `?`    | Question Mark  | Starts **query string** in a URL                             | `page.aspx?id=5`                                                  |
| `&`    | Ampersand      | Separates query string parameters                            | `?id=5&name=John`                                                 |
| `%`    | Percent        | Used in **URL encoding**                                     | `%20` = space, `%3A` = `:`                                        |
| `#`    | Hash           | Refers to a fragment on the page                             | `page.html#section1`                                              |
| `< >`  | Angle Brackets | Used to define **XML or HTML tags**                          | `<add ... />` in `web.config`                                     |
| `"`    | Double Quotes  | Used to wrap attribute values                                | `key="value"`                                                     |
| `'`    | Single Quote   | Also wraps strings (less common in XML)                      | `'value'`                                                         |
| `=`    | Equals         | Assigns values to attributes                                 | `path="*.axd"`                                                    |
| `;`    | Semicolon      | Separates key-value pairs in connection strings              | `Data Source=...;Initial Catalog=...`                             |

