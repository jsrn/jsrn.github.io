---
layout: tcd
id: "0006"
title: Postgres casting gotcha
authored: "2025-10-31"
updated:  "XXXX-XX-XX"
---

Take the following table:

```plain
Table "public.slack_integrations"
 Column         | Type   | Nullable | Default
----------------+--------+----------+---------
 id             | bigint | not null | nextval
 integration_id | bigint |          |
 project_id     | bigint |          |
```

We may want to update this table using a CTE.

```sql
WITH integration_keys (integration_id, project_id) AS (
    VALUES (1, NULL)
)
UPDATE
  integrations
SET
  project_id = integration_keys.project_id
FROM
  integration_keys
WHERE
  integration_keys.integration_id = slack_integrations.integration_id AND
  slack_integrations.id IN (123)
```

This will result in an error like the following:

```plain
ActiveRecord::StatementInvalid:
  PG::DatatypeMismatch:
    ERROR: column "project_id" is of type bigint but expression is of type text
LINE 4: project_id = integration_keys.project_id
```

This is because Postgres will cast a lone NULL in a CTE to a string. It WILL infer the type of the column if you have multiple sets of values with valid data from which to infer type. Otherwise, you need to cast the value yourself, like `NULL::bigint`.
