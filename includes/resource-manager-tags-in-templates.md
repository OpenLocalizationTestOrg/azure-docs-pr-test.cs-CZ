<span data-ttu-id="ab6c0-101">Pokud chcete přidat značku k prostředku během nasazení, přidejte k nasazovanému prostředku element `tags`.</span><span class="sxs-lookup"><span data-stu-id="ab6c0-101">To tag a resource during deployment, add the `tags` element to the resource you are deploying.</span></span> <span data-ttu-id="ab6c0-102">Zadejte název a hodnotu značky.</span><span class="sxs-lookup"><span data-stu-id="ab6c0-102">Provide the tag name and value.</span></span>

### <a name="apply-a-literal-value-to-the-tag-name"></a><span data-ttu-id="ab6c0-103">Použití literálové hodnoty na název značky</span><span class="sxs-lookup"><span data-stu-id="ab6c0-103">Apply a literal value to the tag name</span></span>
<span data-ttu-id="ab6c0-104">Následující příklad ukazuje účet úložiště se dvěma značkami (`Dept` a `Environment`) nastavenými na literálové hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ab6c0-104">The following example shows a storage account with two tags (`Dept` and `Environment`) that are set to literal values:</span></span>

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

### <a name="apply-an-object-to-the-tag-element"></a><span data-ttu-id="ab6c0-105">Použití objektu na element značky</span><span class="sxs-lookup"><span data-stu-id="ab6c0-105">Apply an object to the tag element</span></span>
<span data-ttu-id="ab6c0-106">Můžete definovat parametr objektu, ve kterém je uloženo několik značek, a použít tento objekt na element značky.</span><span class="sxs-lookup"><span data-stu-id="ab6c0-106">You can define an object parameter that stores several tags, and apply that object to the tag element.</span></span> <span data-ttu-id="ab6c0-107">Každá vlastnost v objektu se stane samostatnou značkou pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="ab6c0-107">Each property in the object becomes a separate tag for the resource.</span></span> <span data-ttu-id="ab6c0-108">V následujícím příkladu je na element značky použitý parametr `tagValues`.</span><span class="sxs-lookup"><span data-stu-id="ab6c0-108">The following example has a parameter named `tagValues` that is applied to the tag element.</span></span>

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

### <a name="apply-a-json-string-to-the-tag-name"></a><span data-ttu-id="ab6c0-109">Použití řetězce JSON na název značky</span><span class="sxs-lookup"><span data-stu-id="ab6c0-109">Apply a JSON string to the tag name</span></span>

<span data-ttu-id="ab6c0-110">Pokud chcete uložit mnoho hodnot v jedné značce, použijte řetězec JSON, který představuje hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ab6c0-110">To store many values in a single tag, apply a JSON string that represents the values.</span></span> <span data-ttu-id="ab6c0-111">Celý řetězec JSON je uložený jako jedna značka, která nesmí překročit 256 znaků.</span><span class="sxs-lookup"><span data-stu-id="ab6c0-111">The entire JSON string is stored as one tag that cannot exceed 256 characters.</span></span> <span data-ttu-id="ab6c0-112">V následujícím příkladu je jedna značka `CostCenter`, která obsahuje několik hodnot z řetězce JSON:</span><span class="sxs-lookup"><span data-stu-id="ab6c0-112">The following example has a single tag named `CostCenter` that contains several values from a JSON string:</span></span>  

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