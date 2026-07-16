# Database Access Analysis: [ClassName].[MethodName]

## 1. Executive Summary

Brief overview of what this method achieves and which core tables it interacts with.

## 2. CRUD Matrix

| Schema | Table Name | Operations (C/R/U/D) | Description of Impact |
| :--- | :--- | :--- | :--- |
| example_schema | example_table | Read, Update | ... |

## 3. Detailed Schema & Logic Mapping

### Table: `[TABLE_NAME]`

*Associated Entity: `[EntityName]`*

| Column Name | Java Field | Data Type | Why is it used? (Business/Technical Reason) |
| :--- | :--- | :--- | :--- |
| `column_name` | `fieldName` | String/Long/etc | Detailed explanation of its role in the logic. |

[Repeat for other tables if applicable]

## 4. Data Flow

1. `[Table1].[Column 1.1]` to obtain the `[resource1]` by parameter `[parameter1]`.
2. `[Table1].[Column 1.2]` to identify the `[resource2]`.
3. `[Table2].[Column 2.1]` to obtain the `[resource2]` by parameter `[Column 1.2]`.
4. `[Table2].[Column 2.2]` to filter by the `[value]`.
5. `[Table2].[Column 2.3]` to obtain the value to return `[value]`.

[Add steps as required]
