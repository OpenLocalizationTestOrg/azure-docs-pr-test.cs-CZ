S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello. Šablona Hello obsahuje oddíl s názvem parametry obsahující všechny hodnoty parametru hello.
Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete. Parametry nedefinuje pro hodnoty, které vždy zůstanou hello stejné. Každá hodnota parametru se používá v hello šablony toodefine hello, které prostředky, které jsou nasadit. 

Při definování parametrů, použijte hello **allowedValues** toospecify pole, které hodnoty uživatele můžete během nasazení zadat. Použití hello **defaultValue** pole tooassign toohello parametru hodnoty, pokud je během nasazení zadána žádná hodnota.

Nemůžeme se popisují jednotlivé parametry v šabloně hello.

### <a name="logicappname"></a>logicAppName
Název Hello toocreate aplikace logiky hello.

    "logicAppName": {
        "type": "string"
    }