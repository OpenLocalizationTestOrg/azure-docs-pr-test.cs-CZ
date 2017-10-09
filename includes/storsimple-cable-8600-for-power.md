<!--author=alkohli last changed: 9/16/15-->


#### <a name="toocable-your-device-for-power"></a><span data-ttu-id="24685-101">toocable vašeho zařízení z hlediska výkonu</span><span class="sxs-lookup"><span data-stu-id="24685-101">toocable your device for power</span></span>
> [!NOTE]
> <span data-ttu-id="24685-102">Obě skříně zařízení StorSimple zahrnují redundantní PCMs.</span><span class="sxs-lookup"><span data-stu-id="24685-102">Both enclosures on your StorSimple device include redundant PCMs.</span></span> <span data-ttu-id="24685-103">Pro každou skříň hello PCMs musí být nainstalovaný a připojený toodifferent power zdroje tooensure vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="24685-103">For each enclosure, hello PCMs must be installed and connected toodifferent power sources tooensure high availability.</span></span>
> 
> 

1. <span data-ttu-id="24685-104">Zajistěte, aby hello napájení přepínače na všechny PCMs hello byla v pozici hello OFF.</span><span class="sxs-lookup"><span data-stu-id="24685-104">Make sure that hello power switches on all hello PCMs are in hello OFF position.</span></span>
2. <span data-ttu-id="24685-105">Na hello primární skříň připojte hello power kabelů tooboth PCMs.</span><span class="sxs-lookup"><span data-stu-id="24685-105">On hello primary enclosure, connect hello power cords tooboth PCMs.</span></span> <span data-ttu-id="24685-106">Hello napájecích kabelů jsou identifikovány červeně hello power kabelů diagramu níže.</span><span class="sxs-lookup"><span data-stu-id="24685-106">hello power cords are identified in red in hello power cabling diagram, below.</span></span>
3. <span data-ttu-id="24685-107">Ujistěte se, který hello dvě PCMs na hello primární skříň použití samostatných power zdroje.</span><span class="sxs-lookup"><span data-stu-id="24685-107">Make sure that hello two PCMs on hello primary enclosure use separate power sources.</span></span>
4. <span data-ttu-id="24685-108">Připojte hello power kabelů toohello zapnutí jednotek pro distribuci rack hello jak je znázorněno v hello power kabelů diagram.</span><span class="sxs-lookup"><span data-stu-id="24685-108">Attach hello power cords toohello power on hello rack distribution units as shown in hello power cabling diagram.</span></span>
5. <span data-ttu-id="24685-109">Opakujte kroky 2 až 4 pro hello EBOD skříň.</span><span class="sxs-lookup"><span data-stu-id="24685-109">Repeat steps 2 through 4 for hello EBOD enclosure.</span></span>
6. <span data-ttu-id="24685-110">Zapněte hello EBOD skříň překlopení hello vypínač na každý PCM toohello ON pozici.</span><span class="sxs-lookup"><span data-stu-id="24685-110">Turn on hello EBOD enclosure by flipping hello power switch on each PCM toohello ON position.</span></span>
7. <span data-ttu-id="24685-111">Ověřte, že tento hello EBOD skříň zapnutý kontrolou, že jsou hello zelená LED na hello zadní hello EBOD řadiče povolena ON.</span><span class="sxs-lookup"><span data-stu-id="24685-111">Verify that hello EBOD enclosure is turned on by checking that hello green LEDs on hello back of hello EBOD controller are turned ON.</span></span>
8. <span data-ttu-id="24685-112">Zapněte primární skříň hello překlopení každý PCM přepínač toohello ON pozici.</span><span class="sxs-lookup"><span data-stu-id="24685-112">Turn on hello primary enclosure by flipping each PCM switch toohello ON position.</span></span>
9. <span data-ttu-id="24685-113">Ověřte, že hello systému je na zajištěním řadiče zařízení hello, které jste zapnuli LED.</span><span class="sxs-lookup"><span data-stu-id="24685-113">Verify that hello system is on by ensuring hello device controller LEDs have turned ON.</span></span>
10. <span data-ttu-id="24685-114">Ujistěte se, že hello připojení mezi řadiči EBOD hello a řadiče zařízení hello je aktivní pomocí ověření, že budou zelené hello čtyři LED další toohello SAS portů na řadiči EBOD hello.</span><span class="sxs-lookup"><span data-stu-id="24685-114">Make sure that hello connection between hello EBOD controller and hello device controller is active by verifying that hello four LEDs next toohello SAS port on hello EBOD controller are green.</span></span>
    
    > [!IMPORTANT]
    > <span data-ttu-id="24685-115">tooensure vysoká dostupnost pro váš systém, doporučujeme striktně dodržovat toohello power kabelů hello následující diagram znázorňuje schéma.</span><span class="sxs-lookup"><span data-stu-id="24685-115">tooensure high availability for your system, we recommend that you strictly adhere toohello power cabling scheme shown in hello following diagram.</span></span>
    > 
    > 
    
    ![Zapojení kabeláže zařízení 4U pro napájení](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    <span data-ttu-id="24685-117">**Kabeláž napájení**</span><span class="sxs-lookup"><span data-stu-id="24685-117">**Power cabling**</span></span>
    
    | <span data-ttu-id="24685-118">Štítek</span><span class="sxs-lookup"><span data-stu-id="24685-118">Label</span></span> | <span data-ttu-id="24685-119">Popis</span><span class="sxs-lookup"><span data-stu-id="24685-119">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="24685-120">1</span><span class="sxs-lookup"><span data-stu-id="24685-120">1</span></span> |<span data-ttu-id="24685-121">Primární skříň</span><span class="sxs-lookup"><span data-stu-id="24685-121">Primary enclosure</span></span> |
    | <span data-ttu-id="24685-122">2</span><span class="sxs-lookup"><span data-stu-id="24685-122">2</span></span> |<span data-ttu-id="24685-123">PCM 0</span><span class="sxs-lookup"><span data-stu-id="24685-123">PCM 0</span></span> |
    | <span data-ttu-id="24685-124">3</span><span class="sxs-lookup"><span data-stu-id="24685-124">3</span></span> |<span data-ttu-id="24685-125">PCM 1</span><span class="sxs-lookup"><span data-stu-id="24685-125">PCM 1</span></span> |
    | <span data-ttu-id="24685-126">4</span><span class="sxs-lookup"><span data-stu-id="24685-126">4</span></span> |<span data-ttu-id="24685-127">Řadič 0</span><span class="sxs-lookup"><span data-stu-id="24685-127">Controller 0</span></span> |
    | <span data-ttu-id="24685-128">5</span><span class="sxs-lookup"><span data-stu-id="24685-128">5</span></span> |<span data-ttu-id="24685-129">Řadič 1</span><span class="sxs-lookup"><span data-stu-id="24685-129">Controller 1</span></span> |
    | <span data-ttu-id="24685-130">6</span><span class="sxs-lookup"><span data-stu-id="24685-130">6</span></span> |<span data-ttu-id="24685-131">EBOD řadič 0</span><span class="sxs-lookup"><span data-stu-id="24685-131">EBOD controller 0</span></span> |
    | <span data-ttu-id="24685-132">7</span><span class="sxs-lookup"><span data-stu-id="24685-132">7</span></span> |<span data-ttu-id="24685-133">EBOD řadiči 1</span><span class="sxs-lookup"><span data-stu-id="24685-133">EBOD controller 1</span></span> |
    | <span data-ttu-id="24685-134">8</span><span class="sxs-lookup"><span data-stu-id="24685-134">8</span></span> |<span data-ttu-id="24685-135">EBOD skříň</span><span class="sxs-lookup"><span data-stu-id="24685-135">EBOD enclosure</span></span> |
    | <span data-ttu-id="24685-136">9</span><span class="sxs-lookup"><span data-stu-id="24685-136">9</span></span> |<span data-ttu-id="24685-137">Jednotky PDU</span><span class="sxs-lookup"><span data-stu-id="24685-137">PDUs</span></span> |

