S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello. Šablona Hello obsahuje oddíl s názvem parametry obsahující všechny hodnoty parametru hello.
Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete. Parametry nedefinuje pro hodnoty, které vždy zůstanou hello stejné. Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny. 

Při definování parametrů, použijte hello **allowedValues** toospecify pole, které hodnoty uživatele můžete během nasazení zadat. Použití hello **defaultValue** pole tooassign toohello parametru hodnoty, pokud je během nasazení zadána žádná hodnota.

Nemůžeme se popisují jednotlivé parametry v šabloně hello.

### <a name="sitename"></a>Název webu
Název webové aplikace hello chcete toocreate Hello.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
Název Hello hello služby App Service naplánujte toouse pro hostování hello webové aplikace.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU
Hello cenovou úroveň pro hostování plán hello.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "hello pricing tier for hello hosting plan."
      }
    }

Šablona Hello definuje hello hodnoty, které jsou povoleny pro tento parametr a přiřadí výchozí hodnotu (S1), pokud není zadaná žádná hodnota.

### <a name="workersize"></a>workerSize
velikost instance Hello hello hostování plán (malé, střední a velké).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

Šablona Hello definuje hello hodnoty, které jsou povoleny pro tento parametr (0, 1 nebo 2) a přiřadí výchozí hodnota (0), pokud není zadaná žádná hodnota. Hello hodnoty odpovídají toosmall, střední a velké.

