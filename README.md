# Pysqlizer

Pysqlizer is a Python library for dynamically generating SQL queries based on tuple length and optimizing query generation efficiency.

## Installation

Install via pip:
```bash
pip install pysqlizer
```

## Usage

### `sqlify`
#### Function Signature:
```python
sqlify(keyColumn: str, data: list|tuple|int|str|dict|set|empty_data, raiseException: bool = False, how: str = "IN", condition: str = "OR") -> str
```

#### Parameters:
- **keyColumn (str):** The column name on which the condition will be applied.
- **data (list):** List of values to be used in the query.
- **raiseException (bool, optional):** If `True`, raises an error for an empty data list. Default is `False`.
- **how (str, optional):** SQL condition type (`IN`, `NOT IN`, `LIKE`, `NOT LIKE`). Default is `IN`.
- **condition (str, optional):** Logical condition between multiple statements (`AND`, `OR`). Default is `OR`.

#### Examples:
```python
from pysqlizer import sqlify

# Case 1: Basic IN condition
print(sqlify("product_id", [101, 102, 103]))
# Output: "product_id IN (101, 102, 103)"

# Case 2: NOT IN condition
print(sqlify("category_id", [5, 6], how="NOT IN"))
# Output: "category_id NOT IN (5, 6)"

# Case 3: LIKE condition
print(sqlify("product_name", ["phone", "laptop"], how="LIKE", condition="AND"))
# Output: "(product_name LIKE '%phone%' AND product_name LIKE '%laptop%')"

# Case 4: NOT LIKE condition
print(sqlify("product_name", ["tablet", "watch"], how="NOT LIKE", condition="OR"))
# Output: "(product_name NOT LIKE '%tablet%' OR product_name NOT LIKE '%watch%')"

# Case 5: Empty list without exception
print(sqlify("user_id", [], how="IN"))
# Output: "(1=0)" (safe fallback for empty lists)

# Case 6: Empty list with exception
print(sqlify("user_id", [], raiseException=True))
# Raises ValueError: "data list cannot be empty when raiseException=True"
```

### `sqlifylike`
#### Function Signature:
```python
sqlifylike(keyColumn: str, data: list|tuple|int|str|dict|set|empty_data, how: str = "LIKE", condition: str = "OR") -> str
```

#### Parameters:
- **keyColumn (str):** The column name on which the condition will be applied.
- **data (list):** List of values to be used in the LIKE conditions.
- **how (str, optional):** SQL condition type (`LIKE` or `NOT LIKE`). Default is `LIKE`.
- **condition (str, optional):** Logical condition between multiple statements (`AND`, `OR`). Default is `OR`.

#### Examples:
```python
from pysqlizer import sqlifylike

# Case 1: LIKE condition with OR
print(sqlifylike("product_name", ["phone", "laptop"]))
# Output: "(product_name LIKE '%phone%' OR product_name LIKE '%laptop%')"

# Case 2: LIKE condition with AND
print(sqlifylike("product_name", ["phone", "laptop"], condition="AND"))
# Output: "(product_name LIKE '%phone%' AND product_name LIKE '%laptop%')"

# Case 3: NOT LIKE condition with OR
print(sqlifylike("product_name", ["tablet", "watch"], how="NOT LIKE"))
# Output: "(product_name NOT LIKE '%tablet%' OR product_name NOT LIKE '%watch%')"

# Case 4: NOT LIKE condition with AND
print(sqlifylike("product_name", ["tablet", "watch"], how="NOT LIKE", condition="AND"))
# Output: "(product_name NOT LIKE '%tablet%' AND product_name NOT LIKE '%watch%')"
```

## License

```
MIT License

Copyright (c) 2025 Akash Kushwaha

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

(Full MIT license text here)
