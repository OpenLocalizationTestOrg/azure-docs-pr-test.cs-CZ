<!--author=alkohli last changed: 9/16/15-->


#### <a name="to-cable-your-device-for-power"></a><span data-ttu-id="171ff-101">Chcete-li zapojení kabeláže zařízení pro napájení</span><span class="sxs-lookup"><span data-stu-id="171ff-101">To cable your device for power</span></span>
> [!NOTE]
> <span data-ttu-id="171ff-102">Obě skříně zařízení StorSimple zahrnují redundantní PCMs.</span><span class="sxs-lookup"><span data-stu-id="171ff-102">Both enclosures on your StorSimple device include redundant PCMs.</span></span> <span data-ttu-id="171ff-103">Pro každou skříň musí být nainstalované PCMs a připojené k jiné power zdroje k zajištění vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="171ff-103">For each enclosure, the PCMs must be installed and connected to different power sources to ensure high availability.</span></span>
> 
> 

1. <span data-ttu-id="171ff-104">Zajistěte, aby napájení přepínače na všechny PCMs byla v pozici OFF.</span><span class="sxs-lookup"><span data-stu-id="171ff-104">Make sure that the power switches on all the PCMs are in the OFF position.</span></span>
2. <span data-ttu-id="171ff-105">Na primární skříň připojte k obou PCMs napájecích kabelů.</span><span class="sxs-lookup"><span data-stu-id="171ff-105">On the primary enclosure, connect the power cords to both PCMs.</span></span> <span data-ttu-id="171ff-106">Napájecích kabelů jsou identifikovány červeně v diagramu kabeláže napájení.</span><span class="sxs-lookup"><span data-stu-id="171ff-106">The power cords are identified in red in the power cabling diagram, below.</span></span>
3. <span data-ttu-id="171ff-107">Ujistěte se, že dvě PCMs na primární skříni používají zdrojů energie samostatné.</span><span class="sxs-lookup"><span data-stu-id="171ff-107">Make sure that the two PCMs on the primary enclosure use separate power sources.</span></span>
4. <span data-ttu-id="171ff-108">K zapnutí jednotek pro distribuci rack připojte napájecích kabelů, jak je znázorněno v power kabelů diagram.</span><span class="sxs-lookup"><span data-stu-id="171ff-108">Attach the power cords to the power on the rack distribution units as shown in the power cabling diagram.</span></span>
5. <span data-ttu-id="171ff-109">Opakujte kroky 2 až 4 pro EBOD skříň.</span><span class="sxs-lookup"><span data-stu-id="171ff-109">Repeat steps 2 through 4 for the EBOD enclosure.</span></span>
6. <span data-ttu-id="171ff-110">Zapněte skříni EBOD překlopení vypínač na každý PCM na pozici ON.</span><span class="sxs-lookup"><span data-stu-id="171ff-110">Turn on the EBOD enclosure by flipping the power switch on each PCM to the ON position.</span></span>
7. <span data-ttu-id="171ff-111">Ověřte, jestli je zapnutá skříni EBOD kontrolou, že jsou zelená LED na zadní straně řadičem EBOD povolena ON.</span><span class="sxs-lookup"><span data-stu-id="171ff-111">Verify that the EBOD enclosure is turned on by checking that the green LEDs on the back of the EBOD controller are turned ON.</span></span>
8. <span data-ttu-id="171ff-112">Zapněte primární skříň překlopení každý přepínač PCM na pozici ON.</span><span class="sxs-lookup"><span data-stu-id="171ff-112">Turn on the primary enclosure by flipping each PCM switch to the ON position.</span></span>
9. <span data-ttu-id="171ff-113">Ověřte, že systém je na zajištěním řadiče zařízení, které jste zapnuli LED.</span><span class="sxs-lookup"><span data-stu-id="171ff-113">Verify that the system is on by ensuring the device controller LEDs have turned ON.</span></span>
10. <span data-ttu-id="171ff-114">Ujistěte se, že je spojení mezi řadičem EBOD a řadiče zařízení aktivní pomocí ověření, že jsou čtyři LED vedle SAS portů na řadiči EBOD zelená.</span><span class="sxs-lookup"><span data-stu-id="171ff-114">Make sure that the connection between the EBOD controller and the device controller is active by verifying that the four LEDs next to the SAS port on the EBOD controller are green.</span></span>
    
    > [!IMPORTANT]
    > <span data-ttu-id="171ff-115">K zajištění vysoké dostupnosti pro váš systém, doporučujeme, budete striktně dodržovat power kabelů schéma vidět v následujícím diagramu.</span><span class="sxs-lookup"><span data-stu-id="171ff-115">To ensure high availability for your system, we recommend that you strictly adhere to the power cabling scheme shown in the following diagram.</span></span>
    > 
    > 
    
    ![Zapojení kabeláže zařízení 4U pro napájení](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    <span data-ttu-id="171ff-117">**Kabeláž napájení**</span><span class="sxs-lookup"><span data-stu-id="171ff-117">**Power cabling**</span></span>
    
    | <span data-ttu-id="171ff-118">Štítek</span><span class="sxs-lookup"><span data-stu-id="171ff-118">Label</span></span> | <span data-ttu-id="171ff-119">Popis</span><span class="sxs-lookup"><span data-stu-id="171ff-119">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="171ff-120">1</span><span class="sxs-lookup"><span data-stu-id="171ff-120">1</span></span> |<span data-ttu-id="171ff-121">Primární skříň</span><span class="sxs-lookup"><span data-stu-id="171ff-121">Primary enclosure</span></span> |
    | <span data-ttu-id="171ff-122">2</span><span class="sxs-lookup"><span data-stu-id="171ff-122">2</span></span> |<span data-ttu-id="171ff-123">PCM 0</span><span class="sxs-lookup"><span data-stu-id="171ff-123">PCM 0</span></span> |
    | <span data-ttu-id="171ff-124">3</span><span class="sxs-lookup"><span data-stu-id="171ff-124">3</span></span> |<span data-ttu-id="171ff-125">PCM 1</span><span class="sxs-lookup"><span data-stu-id="171ff-125">PCM 1</span></span> |
    | <span data-ttu-id="171ff-126">4</span><span class="sxs-lookup"><span data-stu-id="171ff-126">4</span></span> |<span data-ttu-id="171ff-127">Řadič 0</span><span class="sxs-lookup"><span data-stu-id="171ff-127">Controller 0</span></span> |
    | <span data-ttu-id="171ff-128">5</span><span class="sxs-lookup"><span data-stu-id="171ff-128">5</span></span> |<span data-ttu-id="171ff-129">Řadič 1</span><span class="sxs-lookup"><span data-stu-id="171ff-129">Controller 1</span></span> |
    | <span data-ttu-id="171ff-130">6</span><span class="sxs-lookup"><span data-stu-id="171ff-130">6</span></span> |<span data-ttu-id="171ff-131">EBOD řadič 0</span><span class="sxs-lookup"><span data-stu-id="171ff-131">EBOD controller 0</span></span> |
    | <span data-ttu-id="171ff-132">7</span><span class="sxs-lookup"><span data-stu-id="171ff-132">7</span></span> |<span data-ttu-id="171ff-133">EBOD řadiči 1</span><span class="sxs-lookup"><span data-stu-id="171ff-133">EBOD controller 1</span></span> |
    | <span data-ttu-id="171ff-134">8</span><span class="sxs-lookup"><span data-stu-id="171ff-134">8</span></span> |<span data-ttu-id="171ff-135">EBOD skříň</span><span class="sxs-lookup"><span data-stu-id="171ff-135">EBOD enclosure</span></span> |
    | <span data-ttu-id="171ff-136">9</span><span class="sxs-lookup"><span data-stu-id="171ff-136">9</span></span> |<span data-ttu-id="171ff-137">Jednotky PDU</span><span class="sxs-lookup"><span data-stu-id="171ff-137">PDUs</span></span> |

