
### <a name="cacheskuname"></a><span data-ttu-id="56e92-101">cacheSKUName</span><span class="sxs-lookup"><span data-stu-id="56e92-101">cacheSKUName</span></span>
<span data-ttu-id="56e92-102">cenová úroveň Hello hello nové Azure Redis v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="56e92-102">hello pricing tier of hello new Azure Redis Cache.</span></span>

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

<span data-ttu-id="56e92-103">Šablona Hello definuje hello hodnoty, které jsou povoleny pro tento parametr (Basic nebo Standard) a přiřadí výchozí hodnotu (Basic), pokud není zadaná žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="56e92-103">hello template defines hello values that are permitted for this parameter (Basic or Standard), and assigns a default value (Basic) if no value is specified.</span></span> <span data-ttu-id="56e92-104">Basic poskytuje jeden uzel s více velikostí, které jsou k dispozici až too53 GB.</span><span class="sxs-lookup"><span data-stu-id="56e92-104">Basic provides a single node with multiple sizes available up too53 GB.</span></span>
<span data-ttu-id="56e92-105">Standard poskytuje dva uzly primární/replika s více velikostí až too53 GB a 99,9 % SLA k dispozici.</span><span class="sxs-lookup"><span data-stu-id="56e92-105">Standard provides two-node Primary/Replica with multiple sizes available up too53 GB and 99.9% SLA.</span></span>

### <a name="cacheskufamily"></a><span data-ttu-id="56e92-106">cacheSKUFamily</span><span class="sxs-lookup"><span data-stu-id="56e92-106">cacheSKUFamily</span></span>
<span data-ttu-id="56e92-107">Hello rodiny pro hello sku.</span><span class="sxs-lookup"><span data-stu-id="56e92-107">hello family for hello sku.</span></span>

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


### <a name="cacheskucapacity"></a><span data-ttu-id="56e92-108">cacheSKUCapacity</span><span class="sxs-lookup"><span data-stu-id="56e92-108">cacheSKUCapacity</span></span>
<span data-ttu-id="56e92-109">velikost Hello hello novou instanci Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="56e92-109">hello size of hello new Azure Redis Cache instance.</span></span> 

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


<span data-ttu-id="56e92-110">Šablona Hello definuje hello hodnoty, které jsou povoleny pro tento parametr (0, 1, 2, 3, 4, 5 nebo 6) a přiřadí výchozí hodnotu (1) Pokud není zadaná žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="56e92-110">hello template defines hello values that are permitted for this parameter (0, 1, 2, 3, 4, 5 or 6), and assigns a default value (1) if no value is specified.</span></span> <span data-ttu-id="56e92-111">Tato čísla odpovídají velikosti mezipaměti toofollowing: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB</span><span class="sxs-lookup"><span data-stu-id="56e92-111">Those numbers correspond toofollowing cache sizes: 0 = 250 MB, 1 = 1 GB, 2 = 2.5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB</span></span>

