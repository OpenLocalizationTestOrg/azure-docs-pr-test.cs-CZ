<span data-ttu-id="66e9c-101">S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="66e9c-101">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="66e9c-102">Šablona Hello obsahuje oddíl s názvem parametry obsahující všechny hodnoty parametru hello.</span><span class="sxs-lookup"><span data-stu-id="66e9c-102">hello template includes a section called Parameters that contains all of hello parameter values.</span></span>
<span data-ttu-id="66e9c-103">Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="66e9c-103">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="66e9c-104">Parametry nedefinuje pro hodnoty, které vždy zůstanou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="66e9c-104">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="66e9c-105">Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="66e9c-105">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span> 

<span data-ttu-id="66e9c-106">Při definování parametrů, použijte hello **allowedValues** toospecify pole, které hodnoty uživatele můžete během nasazení zadat.</span><span class="sxs-lookup"><span data-stu-id="66e9c-106">When defining parameters, use hello **allowedValues** field toospecify which values a user can provide during deployment.</span></span> <span data-ttu-id="66e9c-107">Použití hello **defaultValue** pole tooassign toohello parametru hodnoty, pokud je během nasazení zadána žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="66e9c-107">Use hello **defaultValue** field tooassign a value toohello parameter, if no value is provided during deployment.</span></span>

<span data-ttu-id="66e9c-108">Nemůžeme se popisují jednotlivé parametry v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="66e9c-108">We will describe each parameter in hello template.</span></span>

### <a name="sitename"></a><span data-ttu-id="66e9c-109">Název webu</span><span class="sxs-lookup"><span data-stu-id="66e9c-109">siteName</span></span>
<span data-ttu-id="66e9c-110">Název webové aplikace hello chcete toocreate Hello.</span><span class="sxs-lookup"><span data-stu-id="66e9c-110">hello name of hello web app that you wish toocreate.</span></span>

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a><span data-ttu-id="66e9c-111">hostingPlanName</span><span class="sxs-lookup"><span data-stu-id="66e9c-111">hostingPlanName</span></span>
<span data-ttu-id="66e9c-112">Název Hello hello služby App Service naplánujte toouse pro hostování hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="66e9c-112">hello name of hello App Service plan toouse for hosting hello web app.</span></span>

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a><span data-ttu-id="66e9c-113">SKU</span><span class="sxs-lookup"><span data-stu-id="66e9c-113">sku</span></span>
<span data-ttu-id="66e9c-114">Hello cenovou úroveň pro hostování plán hello.</span><span class="sxs-lookup"><span data-stu-id="66e9c-114">hello pricing tier for hello hosting plan.</span></span>

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

<span data-ttu-id="66e9c-115">Šablona Hello definuje hello hodnoty, které jsou povoleny pro tento parametr a přiřadí výchozí hodnotu (S1), pokud není zadaná žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="66e9c-115">hello template defines hello values that are permitted for this parameter, and assigns a default value (S1) if no value is specified.</span></span>

### <a name="workersize"></a><span data-ttu-id="66e9c-116">workerSize</span><span class="sxs-lookup"><span data-stu-id="66e9c-116">workerSize</span></span>
<span data-ttu-id="66e9c-117">velikost instance Hello hello hostování plán (malé, střední a velké).</span><span class="sxs-lookup"><span data-stu-id="66e9c-117">hello instance size of hello hosting plan (small, medium, or large).</span></span>

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

<span data-ttu-id="66e9c-118">Šablona Hello definuje hello hodnoty, které jsou povoleny pro tento parametr (0, 1 nebo 2) a přiřadí výchozí hodnota (0), pokud není zadaná žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="66e9c-118">hello template defines hello values that are permitted for this parameter (0, 1, or 2), and assigns a default value (0) if no value is specified.</span></span> <span data-ttu-id="66e9c-119">Hello hodnoty odpovídají toosmall, střední a velké.</span><span class="sxs-lookup"><span data-stu-id="66e9c-119">hello values correspond toosmall, medium and large.</span></span>

