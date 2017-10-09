### <span data-ttu-id="de864-101"><a name="noconnection"></a>toomodify místní síťové brány IP předpon adres – žádné připojení brány</span><span class="sxs-lookup"><span data-stu-id="de864-101"><a name="noconnection"></a>toomodify local network gateway IP address prefixes - no gateway connection</span></span>

#### <a name="tooadd-additional-address-prefixes"></a><span data-ttu-id="de864-102">tooadd Další předpony adresy:</span><span class="sxs-lookup"><span data-stu-id="de864-102">tooadd additional address prefixes:</span></span>

1. <span data-ttu-id="de864-103">Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="de864-103">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="de864-104">Přidání hello adresní prostor IP adres v hello *přidat další rozsah adres* pole.</span><span class="sxs-lookup"><span data-stu-id="de864-104">Add hello IP address space in hello *Add additional address range* box.</span></span>
3. <span data-ttu-id="de864-105">Klikněte na tlačítko **Uložit** toosave nastavení.</span><span class="sxs-lookup"><span data-stu-id="de864-105">Click **Save** toosave your settings.</span></span>

#### <a name="tooremove-address-prefixes"></a><span data-ttu-id="de864-106">předpony adres tooremove:</span><span class="sxs-lookup"><span data-stu-id="de864-106">tooremove address prefixes:</span></span>

1. <span data-ttu-id="de864-107">Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="de864-107">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="de864-108">Klikněte na tlačítko hello **"..."** na hello řádek obsahující hello předponu chcete tooremove.</span><span class="sxs-lookup"><span data-stu-id="de864-108">Click hello **'...'** on hello line containing hello prefix you want tooremove.</span></span>
3. <span data-ttu-id="de864-109">Klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="de864-109">Click **Remove**.</span></span>
4. <span data-ttu-id="de864-110">Klikněte na tlačítko **Uložit** toosave nastavení.</span><span class="sxs-lookup"><span data-stu-id="de864-110">Click **Save** toosave your settings.</span></span>

### <span data-ttu-id="de864-111"><a name="withconnection"></a>toomodify předpon brány místní sítě IP adresy - existující připojení brány</span><span class="sxs-lookup"><span data-stu-id="de864-111"><a name="withconnection"></a>toomodify local network gateway IP address prefixes - existing gateway connection</span></span>

<span data-ttu-id="de864-112">Chcete-li mít připojení brány a tooadd nebo odebrat předpony IP adres hello obsažené v bránu místní sítě, je nutné toodo hello následující kroky v pořadí.</span><span class="sxs-lookup"><span data-stu-id="de864-112">If you have a gateway connection and want tooadd or remove hello IP address prefixes contained in your local network gateway, you need toodo hello following steps, in order.</span></span> <span data-ttu-id="de864-113">Způsobí to určitý výpadek připojení VPN.</span><span class="sxs-lookup"><span data-stu-id="de864-113">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="de864-114">Úpravy předpon adres IP, nepotřebujete toodelete hello VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="de864-114">When modifying IP address prefixes, you don't need toodelete hello VPN gateway.</span></span> <span data-ttu-id="de864-115">Potřebujete jenom tooremove hello připojení.</span><span class="sxs-lookup"><span data-stu-id="de864-115">You only need tooremove hello connection.</span></span>

#### <a name="1-remove-hello-connection"></a><span data-ttu-id="de864-116">1. Odeberte připojení hello.</span><span class="sxs-lookup"><span data-stu-id="de864-116">1. Remove hello connection.</span></span>

1. <span data-ttu-id="de864-117">Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **připojení**.</span><span class="sxs-lookup"><span data-stu-id="de864-117">On hello Local Network Gateway resource, in hello **Settings** section, click **Connections**.</span></span>
2. <span data-ttu-id="de864-118">Klikněte na tlačítko hello **...**  na hello řádek pro každé připojení, potom klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="de864-118">Click hello **...** on hello line for each connection, then click **Delete**.</span></span>
3. <span data-ttu-id="de864-119">Klikněte na tlačítko **Uložit** toosave nastavení.</span><span class="sxs-lookup"><span data-stu-id="de864-119">Click **Save** toosave your settings.</span></span>

#### <a name="2-modify-hello-address-prefixes"></a><span data-ttu-id="de864-120">2. Úprava předpon adres hello.</span><span class="sxs-lookup"><span data-stu-id="de864-120">2. Modify hello address prefixes.</span></span>

<span data-ttu-id="de864-121">tooadd Další předpony adresy:</span><span class="sxs-lookup"><span data-stu-id="de864-121">tooadd additional address prefixes:</span></span>

1. <span data-ttu-id="de864-122">Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="de864-122">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="de864-123">Přidejte hello adresní prostor IP adres.</span><span class="sxs-lookup"><span data-stu-id="de864-123">Add hello IP address space.</span></span>
3. <span data-ttu-id="de864-124">Klikněte na tlačítko **Uložit** toosave nastavení.</span><span class="sxs-lookup"><span data-stu-id="de864-124">Click **Save** toosave your settings.</span></span>

<span data-ttu-id="de864-125">předpony adres tooremove:</span><span class="sxs-lookup"><span data-stu-id="de864-125">tooremove address prefixes:</span></span>

1. <span data-ttu-id="de864-126">Na hello prostředku bránu místní sítě, v hello **nastavení** klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="de864-126">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="de864-127">Klikněte na tlačítko hello **...**  na hello řádek obsahující hello předponu chcete tooremove.</span><span class="sxs-lookup"><span data-stu-id="de864-127">Click hello **...** on hello line containing hello prefix you want tooremove.</span></span>
3. <span data-ttu-id="de864-128">Klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="de864-128">Click **Remove**.</span></span>
4. <span data-ttu-id="de864-129">Klikněte na tlačítko **Uložit** toosave nastavení.</span><span class="sxs-lookup"><span data-stu-id="de864-129">Click **Save** toosave your settings.</span></span>

#### <a name="3-recreate-hello-connection"></a><span data-ttu-id="de864-130">3. Znovu vytvořte připojení hello.</span><span class="sxs-lookup"><span data-stu-id="de864-130">3. Recreate hello connection.</span></span>

1. <span data-ttu-id="de864-131">Přejděte toohello brány virtuální sítě pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="de864-131">Navigate toohello Virtual Network Gateway for your VNet.</span></span> <span data-ttu-id="de864-132">(Není hello bránu místní sítě.)</span><span class="sxs-lookup"><span data-stu-id="de864-132">(Not hello Local Network Gateway.)</span></span>
2. <span data-ttu-id="de864-133">Na hello brány virtuální sítě, v hello **nastavení** klikněte na tlačítko **připojení**.</span><span class="sxs-lookup"><span data-stu-id="de864-133">On hello Virtual Network Gateway, in hello **Settings** section, click **Connections**.</span></span>
3. <span data-ttu-id="de864-134">Klikněte na tlačítko hello **+ přidat** tooopen hello **přidat připojení** okno.</span><span class="sxs-lookup"><span data-stu-id="de864-134">Click hello **+ Add** tooopen hello **Add connection** blade.</span></span>
4. <span data-ttu-id="de864-135">Znovu vytvořte připojení.</span><span class="sxs-lookup"><span data-stu-id="de864-135">Recreate your connection.</span></span>
5. <span data-ttu-id="de864-136">Klikněte na tlačítko **OK** toocreate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="de864-136">Click **OK** toocreate hello connection.</span></span>
