tootag prostředků během nasazení, přidejte hello `tags` element toohello prostředků, kterou nasazujete. Zadejte název značky hello a hodnotu.

### <a name="apply-a-literal-value-toohello-tag-name"></a>Použít název značky toohello hodnota literálu
Hello následující příklad ukazuje, účet úložiště se dvěma značkami (`Dept` a `Environment`) nastavených tooliteral hodnoty:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

### <a name="apply-an-object-toohello-tag-element"></a>Použít element objekt toohello značky
Můžete definovat parametr objekt, který ukládá několik značek a použít tento element toohello značky object. Každou vlastnost v objektu hello se změní na samostatné značky pro prostředek hello. Hello následující příklad obsahuje parametr s názvem `tagValues` tedy použité toohello značky elementu.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "tagValues": {
      "type": "object",
      "defaultValue": {
        "Dept": "Finance",
        "Environment": "Production"
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": "[parameters('tagValues')]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {}
    }
  ]
}
```

### <a name="apply-a-json-string-toohello-tag-name"></a>Použít název značky toohello řetězec formátu JSON

toostore mnoho hodnot v jedné značky, použít řetězec formátu JSON, který reprezentuje hello hodnoty. řetězce formátu JSON celý Hello je uložený jako jeden značky, který nesmí překročit 256 znaků. Hello následující příklad obsahuje jedinou značku s názvem `CostCenter` obsahující několik hodnot z řetězce formátu JSON:  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```