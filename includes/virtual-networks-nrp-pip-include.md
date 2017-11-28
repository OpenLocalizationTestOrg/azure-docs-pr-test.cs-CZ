## <a name="public-ip-address"></a><span data-ttu-id="130ba-101">Veřejná IP adresa</span><span class="sxs-lookup"><span data-stu-id="130ba-101">Public IP address</span></span>
<span data-ttu-id="130ba-102">Prostředek veřejné IP adresy obsahuje buď vyhrazené nebo dynamické internetové IP adresu.</span><span class="sxs-lookup"><span data-stu-id="130ba-102">A public IP address resource provides either a reserved or dynamic Internet facing IP address.</span></span> <span data-ttu-id="130ba-103">I když můžete vytvořit veřejnou IP adresu jako samostatný objekt, je třeba ji přidružit k jinému objektu skutečně použít adresu.</span><span class="sxs-lookup"><span data-stu-id="130ba-103">Although you can create a public IP address as a stand alone object, you need to associate it to another object to actually use the address.</span></span> <span data-ttu-id="130ba-104">Můžete přidružit veřejnou IP adresu, která nástroj pro vyrovnávání zatížení, aplikační bránu nebo síťový adaptér zajistit přístup k Internetu na tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="130ba-104">You can associate a public IP address to a load balancer, application  gateway, or a NIC to provide Internet access to those resources.</span></span>  

| <span data-ttu-id="130ba-105">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="130ba-105">Property</span></span> | <span data-ttu-id="130ba-106">Popis</span><span class="sxs-lookup"><span data-stu-id="130ba-106">Description</span></span> | <span data-ttu-id="130ba-107">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="130ba-107">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="130ba-108">**publicIPAllocationMethod**</span><span class="sxs-lookup"><span data-stu-id="130ba-108">**publicIPAllocationMethod**</span></span> |<span data-ttu-id="130ba-109">Určuje, zda je adresa IP *statické* nebo *dynamické*.</span><span class="sxs-lookup"><span data-stu-id="130ba-109">Defines if the IP address is *static* or *dynamic*.</span></span> |<span data-ttu-id="130ba-110">static, dynamické</span><span class="sxs-lookup"><span data-stu-id="130ba-110">static, dynamic</span></span> |
| <span data-ttu-id="130ba-111">**idleTimeoutInMinutes**</span><span class="sxs-lookup"><span data-stu-id="130ba-111">**idleTimeoutInMinutes**</span></span> |<span data-ttu-id="130ba-112">Definuje časový limit nečinnosti, s výchozí hodnotou 4 minuty.</span><span class="sxs-lookup"><span data-stu-id="130ba-112">Defines the idle time out, with a default value of 4 minutes.</span></span> <span data-ttu-id="130ba-113">Pokud v tuto chvíli je přijatá žádné další pakety pro dané relace, relace je ukončena.</span><span class="sxs-lookup"><span data-stu-id="130ba-113">If no more packets for a given session is received within this time, the session is terminated.</span></span> |<span data-ttu-id="130ba-114">Libovolná hodnota od 4 do 30.</span><span class="sxs-lookup"><span data-stu-id="130ba-114">any value between 4 and 30</span></span> |
| <span data-ttu-id="130ba-115">**IP adresa**</span><span class="sxs-lookup"><span data-stu-id="130ba-115">**ipAddress**</span></span> |<span data-ttu-id="130ba-116">Přiřazené objektu IP adresy.</span><span class="sxs-lookup"><span data-stu-id="130ba-116">IP address assigned to object.</span></span> <span data-ttu-id="130ba-117">Toto je vlastnost jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="130ba-117">This is a read-only property.</span></span> |<span data-ttu-id="130ba-118">104.42.233.77</span><span class="sxs-lookup"><span data-stu-id="130ba-118">104.42.233.77</span></span> |

### <a name="dns-settings"></a><span data-ttu-id="130ba-119">Nastavení DNS</span><span class="sxs-lookup"><span data-stu-id="130ba-119">DNS settings</span></span>
<span data-ttu-id="130ba-120">Veřejné IP adresy mít podřízený objekt s názvem **dnsSettings** obsahující následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="130ba-120">Public IP addresses have a child object named **dnsSettings** containing the following properties:</span></span>

| <span data-ttu-id="130ba-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="130ba-121">Property</span></span> | <span data-ttu-id="130ba-122">Popis</span><span class="sxs-lookup"><span data-stu-id="130ba-122">Description</span></span> | <span data-ttu-id="130ba-123">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="130ba-123">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="130ba-124">**domainNameLabel**</span><span class="sxs-lookup"><span data-stu-id="130ba-124">**domainNameLabel**</span></span> |<span data-ttu-id="130ba-125">Hostitel s názvem používají pro překlad.</span><span class="sxs-lookup"><span data-stu-id="130ba-125">Host named used for name resolution.</span></span> |<span data-ttu-id="130ba-126">WWW, ftp, vm1</span><span class="sxs-lookup"><span data-stu-id="130ba-126">www, ftp, vm1</span></span> |
| <span data-ttu-id="130ba-127">**plně kvalifikovaný název domény**</span><span class="sxs-lookup"><span data-stu-id="130ba-127">**fqdn**</span></span> |<span data-ttu-id="130ba-128">Plně kvalifikovaný název pro veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="130ba-128">Fully qualified name for the public IP.</span></span> |<span data-ttu-id="130ba-129">www.westus.cloudapp.Azure.com</span><span class="sxs-lookup"><span data-stu-id="130ba-129">www.westus.cloudapp.azure.com</span></span> |
| <span data-ttu-id="130ba-130">**reverseFqdn**</span><span class="sxs-lookup"><span data-stu-id="130ba-130">**reverseFqdn**</span></span> |<span data-ttu-id="130ba-131">Plně kvalifikovaný název domény na IP adresu a je zaregistrován ve službě DNS, jako záznam PTR.</span><span class="sxs-lookup"><span data-stu-id="130ba-131">Fully qualified domain name that resolves to the IP address and is registered in DNS as a PTR record.</span></span> |<span data-ttu-id="130ba-132">www.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="130ba-132">www.contoso.com.</span></span> |

<span data-ttu-id="130ba-133">Ukázka veřejnou IP adresu ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="130ba-133">Sample public IP address in JSON format:</span></span>

    {
       "name": "PIP01",
       "location": "North US",
       "tags": { "key": "value" },
       "properties": {
          "publicIPAllocationMethod": "Static",
          "idleTimeoutInMinutes": 4,
          "ipAddress": "104.42.233.77",
          "dnsSettings": {
             "domainNameLabel": "mylabel",
             "fqdn": "mylabel.westus.cloudapp.azure.com",
             "reverseFqdn": "contoso.com."
          }
       }
    } 

### <a name="additional-resources"></a><span data-ttu-id="130ba-134">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="130ba-134">Additional resources</span></span>
* <span data-ttu-id="130ba-135">Přečtěte si další informace o [veřejné IP adresy](../articles/virtual-network/virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="130ba-135">Get more information about [public IP addresses](../articles/virtual-network/virtual-networks-reserved-public-ip.md).</span></span>
* <span data-ttu-id="130ba-136">Další informace o [instance úrovně veřejné IP adresy](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="130ba-136">Learn about [instance level public IP addresses](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).</span></span>
* <span data-ttu-id="130ba-137">Pro čtení [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163638.aspx) pro veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="130ba-137">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163638.aspx) for public IP addresses.</span></span>

