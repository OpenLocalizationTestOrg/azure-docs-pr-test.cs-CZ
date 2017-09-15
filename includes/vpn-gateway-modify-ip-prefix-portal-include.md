### <span data-ttu-id="91f00-101"><a name="noconnection"></a>Úprava předpon IP adres místní síťové brány – žádné připojení brány</span><span class="sxs-lookup"><span data-stu-id="91f00-101"><a name="noconnection"></a>To modify local network gateway IP address prefixes - no gateway connection</span></span>

#### <a name="to-add-additional-address-prefixes"></a><span data-ttu-id="91f00-102">Přidání dalších předpon adres:</span><span class="sxs-lookup"><span data-stu-id="91f00-102">To add additional address prefixes:</span></span>

1. <span data-ttu-id="91f00-103">Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="91f00-103">On the Local Network Gateway resource, in the **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="91f00-104">Přidejte adresní prostor IP adres v *přidat další rozsah adres* pole.</span><span class="sxs-lookup"><span data-stu-id="91f00-104">Add the IP address space in the *Add additional address range* box.</span></span>
3. <span data-ttu-id="91f00-105">Klikněte na tlačítko **Uložit** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="91f00-105">Click **Save** to save your settings.</span></span>

#### <a name="to-remove-address-prefixes"></a><span data-ttu-id="91f00-106">Odebrání předpon adres:</span><span class="sxs-lookup"><span data-stu-id="91f00-106">To remove address prefixes:</span></span>

1. <span data-ttu-id="91f00-107">Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="91f00-107">On the Local Network Gateway resource, in the **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="91f00-108">Klikněte **'...**</span><span class="sxs-lookup"><span data-stu-id="91f00-108">Click the **'...'**</span></span> <span data-ttu-id="91f00-109">na řádek obsahující předponu, která chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="91f00-109">on the line containing the prefix you want to remove.</span></span>
3. <span data-ttu-id="91f00-110">Klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="91f00-110">Click **Remove**.</span></span>
4. <span data-ttu-id="91f00-111">Klikněte na tlačítko **Uložit** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="91f00-111">Click **Save** to save your settings.</span></span>

### <span data-ttu-id="91f00-112"><a name="withconnection"></a>Úprava předpon IP adres místní síťové brány – existující připojení brány</span><span class="sxs-lookup"><span data-stu-id="91f00-112"><a name="withconnection"></a>To modify local network gateway IP address prefixes - existing gateway connection</span></span>

<span data-ttu-id="91f00-113">Pokud máte připojení k bráně a chcete přidat nebo odebrat předpony IP adres obsažené v bráně místní sítě, musíte v uvedeném pořadí provést následující kroky.</span><span class="sxs-lookup"><span data-stu-id="91f00-113">If you have a gateway connection and want to add or remove the IP address prefixes contained in your local network gateway, you need to do the following steps, in order.</span></span> <span data-ttu-id="91f00-114">Způsobí to určitý výpadek připojení VPN.</span><span class="sxs-lookup"><span data-stu-id="91f00-114">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="91f00-115">Při upravování předpon IP adres není potřeba odstraňovat bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="91f00-115">When modifying IP address prefixes, you don't need to delete the VPN gateway.</span></span> <span data-ttu-id="91f00-116">Stačí jenom odebrat připojení.</span><span class="sxs-lookup"><span data-stu-id="91f00-116">You only need to remove the connection.</span></span>

#### <a name="1-remove-the-connection"></a><span data-ttu-id="91f00-117">1. Odeberte připojení.</span><span class="sxs-lookup"><span data-stu-id="91f00-117">1. Remove the connection.</span></span>

1. <span data-ttu-id="91f00-118">Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **připojení**.</span><span class="sxs-lookup"><span data-stu-id="91f00-118">On the Local Network Gateway resource, in the **Settings** section, click **Connections**.</span></span>
2. <span data-ttu-id="91f00-119">Klikněte **...**  na řádek pro každé připojení klikněte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="91f00-119">Click the **...** on the line for each connection, then click **Delete**.</span></span>
3. <span data-ttu-id="91f00-120">Klikněte na tlačítko **Uložit** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="91f00-120">Click **Save** to save your settings.</span></span>

#### <a name="2-modify-the-address-prefixes"></a><span data-ttu-id="91f00-121">2. Úprava předpon adres.</span><span class="sxs-lookup"><span data-stu-id="91f00-121">2. Modify the address prefixes.</span></span>

<span data-ttu-id="91f00-122">Přidání dalších předpon adres:</span><span class="sxs-lookup"><span data-stu-id="91f00-122">To add additional address prefixes:</span></span>

1. <span data-ttu-id="91f00-123">Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="91f00-123">On the Local Network Gateway resource, in the **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="91f00-124">Přidejte adresní prostor IP adres.</span><span class="sxs-lookup"><span data-stu-id="91f00-124">Add the IP address space.</span></span>
3. <span data-ttu-id="91f00-125">Klikněte na tlačítko **Uložit** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="91f00-125">Click **Save** to save your settings.</span></span>

<span data-ttu-id="91f00-126">Odebrání předpon adres:</span><span class="sxs-lookup"><span data-stu-id="91f00-126">To remove address prefixes:</span></span>

1. <span data-ttu-id="91f00-127">Na bráně místní sítě prostředku v **nastavení** klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="91f00-127">On the Local Network Gateway resource, in the **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="91f00-128">Klikněte **...**  na řádek obsahující předpona, kterou chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="91f00-128">Click the **...** on the line containing the prefix you want to remove.</span></span>
3. <span data-ttu-id="91f00-129">Klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="91f00-129">Click **Remove**.</span></span>
4. <span data-ttu-id="91f00-130">Klikněte na tlačítko **Uložit** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="91f00-130">Click **Save** to save your settings.</span></span>

#### <a name="3-recreate-the-connection"></a><span data-ttu-id="91f00-131">3. Znovu vytvořte připojení.</span><span class="sxs-lookup"><span data-stu-id="91f00-131">3. Recreate the connection.</span></span>

1. <span data-ttu-id="91f00-132">Přejděte k bráně virtuální sítě pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="91f00-132">Navigate to the Virtual Network Gateway for your VNet.</span></span> <span data-ttu-id="91f00-133">(Nikoli brány místní sítě.)</span><span class="sxs-lookup"><span data-stu-id="91f00-133">(Not the Local Network Gateway.)</span></span>
2. <span data-ttu-id="91f00-134">Na bráně virtuální sítě v **nastavení** klikněte na tlačítko **připojení**.</span><span class="sxs-lookup"><span data-stu-id="91f00-134">On the Virtual Network Gateway, in the **Settings** section, click **Connections**.</span></span>
3. <span data-ttu-id="91f00-135">Klikněte **+ přidat** otevřete **přidat připojení** okno.</span><span class="sxs-lookup"><span data-stu-id="91f00-135">Click the **+ Add** to open the **Add connection** blade.</span></span>
4. <span data-ttu-id="91f00-136">Znovu vytvořte připojení.</span><span class="sxs-lookup"><span data-stu-id="91f00-136">Recreate your connection.</span></span>
5. <span data-ttu-id="91f00-137">Klikněte na tlačítko **OK** k vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="91f00-137">Click **OK** to create the connection.</span></span>