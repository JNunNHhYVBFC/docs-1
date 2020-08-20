---
editable: false
---

# ENDSWITH

_String functions_

#### Syntax {#syntax}


```
ENDSWITH( string, substring )
```

#### Description {#description}
Returns `TRUE` if `string` ends in `substring`. For case-insensitive searches, see [IENDSWITH](IENDSWITH.md).

**Argument types:**
- `string` — `String`
- `substring` — `String`


**Return type**: `Boolean`

#### Examples {#examples}

```
ENDSWITH("Petrov Ivan", "Ivan") = TRUE
```

```
ENDSWITH("Lorem ipsum", "sum") = TRUE
```

```
ENDSWITH("Lorem ipsum", "abc") = FALSE
```


#### Data source support {#data-source-support}

`Materialized Dataset`, `ClickHouse 1.1`, `Yandex.Metrica`, `Microsoft SQL Server 2017 (14.0)`, `MySQL 5.6`, `PostgreSQL 9.3`.