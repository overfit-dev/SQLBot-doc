Our SQLBot API translates questions in natural language to SQL instructions. 

**How it works:**
User passes table name, number of its columns, column names, column decriptions and the question. 
The API returns structred SQL and the raw SQL that can be used directly to query user's databases.

```console
$curl http://35.246.38.47:5000/predict -H "Content-Type: application/x-www-form-urlencoded" \
    -d 'json={ 
	"data": { 
		"tensor": {
			"values": [
				TABLE_NAME, 
				NUMBER_OF_COLUMNS, 
				COLUMN_NAMES..., 
				COLUMN_DESCRIPTIONS..., 
				QUESTION
			]
		}
	}
}'
```
```json
{
"data":{
	"tensor":{
		"values":[
			{
				"query":{
					"agg":"AGGREGATION_OP_ID",
					"conds":[
						["COLUMN_ID","OP_ID","VALUE"]
					],
					"sel":"COLUMN_ID"
				},
				"sql":"SQL_INSTRUCTION"
			}
		]
	}
},
}
```

**Example:**

```console
$curl http://35.246.38.47:5000/predict -H "Content-Type: application/x-www-form-urlencoded" \
    -d 'json={
	"data": 
		{"tensor": {
			"values": [
				"football_player", 
				"6", 
				"name", "date", "age", "position", "years_in_toronto", "team", "player name", 
				"Date", "Age", "Position", "Years in Toronto", "School/Club Team", 
				"Average age of players in defense"
			]
		}
	}
}'
```

```json
{
"data":{
	"tensor":{
		"values":[
			{
				"query":{
					"agg":5,
					"conds":[[3,0,"defense"]],
					"sel":2
				},
				"sql":"SELECT avg(age) FROM football_player WHERE position = defense"
			}
		]
	}
},
"meta":{}
}
```
