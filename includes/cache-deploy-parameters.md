
### <a name="cacheskuname"></a>cacheSKUName
cenová úroveň Hello hello nové Azure Redis v mezipaměti.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "hello pricing tier of hello new Azure Redis Cache."
      }
    },

Šablona Hello definuje hello hodnoty, které jsou povoleny pro tento parametr (Basic nebo Standard) a přiřadí výchozí hodnotu (Basic), pokud není zadaná žádná hodnota. Basic poskytuje jeden uzel s více velikostí, které jsou k dispozici až too53 GB.
Standard poskytuje dva uzly primární/replika s více velikostí až too53 GB a 99,9 % SLA k dispozici.

### <a name="cacheskufamily"></a>cacheSKUFamily
Hello rodiny pro hello sku.

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "hello family for hello sku."
      }
    },


### <a name="cacheskucapacity"></a>cacheSKUCapacity
velikost Hello hello novou instanci Azure Redis Cache. 

    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "hello size of hello new Azure Redis Cache instance. "
      }
    }


Šablona Hello definuje hello hodnoty, které jsou povoleny pro tento parametr (0, 1, 2, 3, 4, 5 nebo 6) a přiřadí výchozí hodnotu (1) Pokud není zadaná žádná hodnota. Tato čísla odpovídají velikosti mezipaměti toofollowing: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB

