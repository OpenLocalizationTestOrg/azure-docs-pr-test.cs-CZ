<span data-ttu-id="6f5b2-101">použít tooadd skupinu prostředků tooa značka **sadu azure skupin**.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-101">tooadd a tag tooa resource group, use **azure group set**.</span></span> <span data-ttu-id="6f5b2-102">Pokud skupina prostředků hello nemá všechny existující značky, předejte hello značky.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-102">If hello resource group does not have any existing tags, pass in hello tag.</span></span>

```azurecli
azure group set -n tag-demo-group -t Dept=Finance
```

<span data-ttu-id="6f5b2-103">Značky jsou aktualizovány jako celek.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-103">Tags are updated as a whole.</span></span> <span data-ttu-id="6f5b2-104">Pokud chcete tooadd skupinu prostředků tooa značku, která má existující značky, předejte všechny značky hello.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-104">If you want tooadd a tag tooa resource group that has existing tags, pass all hello tags.</span></span> 

```azurecli
azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
```

<span data-ttu-id="6f5b2-105">Značky nejsou zdědí prostředky ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-105">Tags are not inherited by resources in a resource group.</span></span> <span data-ttu-id="6f5b2-106">použít tooadd prostředek tooa značka **sadu prostředků azure**.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-106">tooadd a tag tooa resource, use **azure resource set**.</span></span> <span data-ttu-id="6f5b2-107">Předejte hello číslo verze rozhraní API pro hello typ prostředku, který chcete přidat značku hello.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-107">Pass hello API version number for hello resource type that you are adding hello tag to.</span></span> <span data-ttu-id="6f5b2-108">Pokud potřebujete tooretrieve hello rozhraní API verze, použijte následující příkaz s hello poskytovatel prostředků pro typ text hello, že jsou nastavení hello:</span><span class="sxs-lookup"><span data-stu-id="6f5b2-108">If you need tooretrieve hello API version, use hello following command with hello resource provider for hello type you are setting:</span></span>

```azurecli
azure provider show -n Microsoft.Storage --json
```

<span data-ttu-id="6f5b2-109">V hello výsledky vyhledejte požadovaný typ prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-109">In hello results, look for hello resource type you want.</span></span>

```azurecli
"resourceTypes": [
{
  "resourceType": "storageAccounts",
  ...
  "apiVersions": [
    "2016-01-01",
    "2015-06-15",
    "2015-05-01-preview"
  ]
}
...
```

<span data-ttu-id="6f5b2-110">Nyní, zadejte tuto verzi rozhraní API, název skupiny prostředků, prostředků název, typ prostředku a hodnota značky jako parametry.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-110">Now, provide that API version, resource group name, resource name, resource type, and tag value as parameters.</span></span>

```azurecli
azure resource set -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -t Dept=Finance -o 2016-01-01
```

<span data-ttu-id="6f5b2-111">Značky existují přímo na skupiny prostředků a prostředky.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-111">Tags exist directly on resources and resource groups.</span></span> <span data-ttu-id="6f5b2-112">toosee hello existující značky, získat skupinu prostředků a její prostředky s **zobrazit skupiny azure**.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-112">toosee hello existing tags, get a resource group and its resources with **azure group show**.</span></span>

```azurecli
azure group show -n tag-demo-group --json
```

<span data-ttu-id="6f5b2-113">Která vrací metadata o hello skupinu prostředků, včetně tooit všechny značky, které jsou použity.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-113">Which returns metadata about hello resource group, including any tags applied tooit.</span></span>

```azurecli
{
  "id": "/subscriptions/4705409c-9372-42f0-914c-64a504530837/resourceGroups/tag-demo-group",
  "name": "tag-demo-group",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "location": "southcentralus",
  "tags": {
    "Dept": "Finance",
    "Environment": "Production",
    "Project": "Upgrade"
  },
  ...
}
```

<span data-ttu-id="6f5b2-114">Zobrazit hello značky pro určitý prostředek pomocí **prostředků azure zobrazit**.</span><span class="sxs-lookup"><span data-stu-id="6f5b2-114">You view hello tags for a particular resource by using **azure resource show**.</span></span>

```azurecli
azure resource show -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -o 2016-01-01 --json
```

<span data-ttu-id="6f5b2-115">tooretrieve všechny prostředky hello s hodnotou značku, použijte:</span><span class="sxs-lookup"><span data-stu-id="6f5b2-115">tooretrieve all hello resources with a tag value, use:</span></span>

```azurecli
azure resource list -t Dept=Finance --json
```

<span data-ttu-id="6f5b2-116">tooretrieve všechny skupiny prostředků hello s hodnota značky, použijte:</span><span class="sxs-lookup"><span data-stu-id="6f5b2-116">tooretrieve all hello resource groups with a tag value, use:</span></span>

```azurecli
azure group list -t Dept=Finance
```

<span data-ttu-id="6f5b2-117">Můžete zobrazit stávající značky hello ve vašem předplatném s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6f5b2-117">You can view hello existing tags in your subscription with hello following command:</span></span>

```azurecli
azure tag list
```
