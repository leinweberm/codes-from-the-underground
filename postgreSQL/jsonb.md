# JSONB 
The jsonb datatype is an advanced binary storage format with full processing, indexing and searching capabilities, and as such pre-processes the JSON data to an internal format, which does include a single value per key; and also isn't sensible to extra whitespace or indentation.

## Property accessors:
>- ->
vracího hodnotu z JSONB na základě předanéhé klíče nebo indexu</br>
```
_hodnota = muj_objekt->'jeho_property';
```
>- ->>
</br>

## Methods:
>- **JSONB_BUILD_OBJECT()**
```sql
_myObject = JSONB_BUILD_OBJECT('firstname', 'Martin', 'lastname', 'Leinweber');
-- vytvoří jsonb objekt '{"firtsname": "Martin", "lastname": "Leinweber"}'
```

>- **JSONB_BUILD_ARRAY()**
>- **JSONB_OBJECT_KEYS()**
```json

```

>- **JSONB_SET()**
</br>
Metoda slouží pro update vnořených hodnot v JSONB poli nebo objektu.
</br>
```sql
	-- ukázka dat ze sloupce data
	-- '{ "adress": {"PSČ": "736 01", "city": "Ostrava"}, "contacts": {} }'
	UPDATE schema.table
	SET "data" = JSONB_SET(
			"data",
			'{adress, city}',
			'"Havířov"',
			true
	)
```

>- **JSONB_INSERT()**
</br>
```sql
```
