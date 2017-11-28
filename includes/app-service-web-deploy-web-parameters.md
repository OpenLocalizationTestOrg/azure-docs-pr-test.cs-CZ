<span data-ttu-id="4150e-101">Pomocí Azure Resource Manageru definujete parametry pro hodnoty, které chcete zadat při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="4150e-101">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="4150e-102">Šablona obsahuje oddíl s názvem parametry obsahující všechny hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="4150e-102">The template includes a section called Parameters that contains all of the parameter values.</span></span>
<span data-ttu-id="4150e-103">Měli byste parametr pro ty hodnoty, které se liší podle prostředí, ve kterém provádíte nasazení nebo na základě projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="4150e-103">You should define a parameter for those values that will vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="4150e-104">Nedefinují parametry pro hodnoty, které zůstanou vždy stejná.</span><span class="sxs-lookup"><span data-stu-id="4150e-104">Do not define parameters for values that will always stay the same.</span></span> <span data-ttu-id="4150e-105">Každá hodnota parametru se v šabloně použije k definování nasazovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="4150e-105">Each parameter value is used in the template to define the resources that are deployed.</span></span> 

<span data-ttu-id="4150e-106">Při definování parametrů, použijte **allowedValues** můžete během nasazení zadat pole, které chcete určit, které hodnoty uživatele.</span><span class="sxs-lookup"><span data-stu-id="4150e-106">When defining parameters, use the **allowedValues** field to specify which values a user can provide during deployment.</span></span> <span data-ttu-id="4150e-107">Použití **defaultValue** pole o přiřazení hodnoty na parametr, pokud je během nasazení zadána žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="4150e-107">Use the **defaultValue** field to assign a value to the parameter, if no value is provided during deployment.</span></span>

<span data-ttu-id="4150e-108">Nemůžeme se popisují jednotlivé parametry v šabloně.</span><span class="sxs-lookup"><span data-stu-id="4150e-108">We will describe each parameter in the template.</span></span>

### <a name="sitename"></a><span data-ttu-id="4150e-109">Název webu</span><span class="sxs-lookup"><span data-stu-id="4150e-109">siteName</span></span>
<span data-ttu-id="4150e-110">Název webové aplikace, kterou chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="4150e-110">The name of the web app that you wish to create.</span></span>

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a><span data-ttu-id="4150e-111">hostingPlanName</span><span class="sxs-lookup"><span data-stu-id="4150e-111">hostingPlanName</span></span>
<span data-ttu-id="4150e-112">Název plánu služby App Service pro hostování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4150e-112">The name of the App Service plan to use for hosting the web app.</span></span>

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a><span data-ttu-id="4150e-113">SKU</span><span class="sxs-lookup"><span data-stu-id="4150e-113">sku</span></span>
<span data-ttu-id="4150e-114">Cenovou úroveň pro hostování plánu.</span><span class="sxs-lookup"><span data-stu-id="4150e-114">The pricing tier for the hosting plan.</span></span>

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
        "description": "The pricing tier for the hosting plan."
      }
    }

<span data-ttu-id="4150e-115">Definuje hodnoty, které jsou povoleny pro tento parametr šablony a přiřadí výchozí hodnotu (S1), pokud není zadaná žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="4150e-115">The template defines the values that are permitted for this parameter, and assigns a default value (S1) if no value is specified.</span></span>

### <a name="workersize"></a><span data-ttu-id="4150e-116">workerSize</span><span class="sxs-lookup"><span data-stu-id="4150e-116">workerSize</span></span>
<span data-ttu-id="4150e-117">Velikost instance plánu hostingu (malé, střední a velké).</span><span class="sxs-lookup"><span data-stu-id="4150e-117">The instance size of the hosting plan (small, medium, or large).</span></span>

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

<span data-ttu-id="4150e-118">Šablona definuje hodnoty, které jsou povoleny pro tento parametr (0, 1 nebo 2) a přiřadí výchozí hodnota (0), pokud není zadaná žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="4150e-118">The template defines the values that are permitted for this parameter (0, 1, or 2), and assigns a default value (0) if no value is specified.</span></span> <span data-ttu-id="4150e-119">Hodnoty odpovídají malé, střední a velké.</span><span class="sxs-lookup"><span data-stu-id="4150e-119">The values correspond to small, medium and large.</span></span>

