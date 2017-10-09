<span data-ttu-id="be418-101">tootag prostředků během nasazení, přidejte hello `tags` element toohello prostředků, kterou nasazujete.</span><span class="sxs-lookup"><span data-stu-id="be418-101">tootag a resource during deployment, add hello `tags` element toohello resource you are deploying.</span></span> <span data-ttu-id="be418-102">Zadejte název značky hello a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="be418-102">Provide hello tag name and value.</span></span>

### <a name="apply-a-literal-value-toohello-tag-name"></a><span data-ttu-id="be418-103">Použít název značky toohello hodnota literálu</span><span class="sxs-lookup"><span data-stu-id="be418-103">Apply a literal value toohello tag name</span></span>
<span data-ttu-id="be418-104">Hello následující příklad ukazuje, účet úložiště se dvěma značkami (`Dept` a `Environment`) nastavených tooliteral hodnoty:</span><span class="sxs-lookup"><span data-stu-id="be418-104">hello following example shows a storage account with two tags (`Dept` and `Environment`) that are set tooliteral values:</span></span>

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

### <a name="apply-an-object-toohello-tag-element"></a><span data-ttu-id="be418-105">Použít element objekt toohello značky</span><span class="sxs-lookup"><span data-stu-id="be418-105">Apply an object toohello tag element</span></span>
<span data-ttu-id="be418-106">Můžete definovat parametr objekt, který ukládá několik značek a použít tento element toohello značky object.</span><span class="sxs-lookup"><span data-stu-id="be418-106">You can define an object parameter that stores several tags, and apply that object toohello tag element.</span></span> <span data-ttu-id="be418-107">Každou vlastnost v objektu hello se změní na samostatné značky pro prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="be418-107">Each property in hello object becomes a separate tag for hello resource.</span></span> <span data-ttu-id="be418-108">Hello následující příklad obsahuje parametr s názvem `tagValues` tedy použité toohello značky elementu.</span><span class="sxs-lookup"><span data-stu-id="be418-108">hello following example has a parameter named `tagValues` that is applied toohello tag element.</span></span>

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

### <a name="apply-a-json-string-toohello-tag-name"></a><span data-ttu-id="be418-109">Použít název značky toohello řetězec formátu JSON</span><span class="sxs-lookup"><span data-stu-id="be418-109">Apply a JSON string toohello tag name</span></span>

<span data-ttu-id="be418-110">toostore mnoho hodnot v jedné značky, použít řetězec formátu JSON, který reprezentuje hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="be418-110">toostore many values in a single tag, apply a JSON string that represents hello values.</span></span> <span data-ttu-id="be418-111">řetězce formátu JSON celý Hello je uložený jako jeden značky, který nesmí překročit 256 znaků.</span><span class="sxs-lookup"><span data-stu-id="be418-111">hello entire JSON string is stored as one tag that cannot exceed 256 characters.</span></span> <span data-ttu-id="be418-112">Hello následující příklad obsahuje jedinou značku s názvem `CostCenter` obsahující několik hodnot z řetězce formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="be418-112">hello following example has a single tag named `CostCenter` that contains several values from a JSON string:</span></span>  

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