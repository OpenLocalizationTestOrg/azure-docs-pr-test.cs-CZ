## <a name="public-ip-address"></a><span data-ttu-id="b68bc-101">Veřejná IP adresa</span><span class="sxs-lookup"><span data-stu-id="b68bc-101">Public IP address</span></span>
<span data-ttu-id="b68bc-102">Prostředek veřejné IP adresy obsahuje buď vyhrazené nebo dynamické internetové IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b68bc-102">A public IP address resource provides either a reserved or dynamic Internet facing IP address.</span></span> <span data-ttu-id="b68bc-103">I když můžete vytvořit veřejnou IP adresu jako samostatný objekt, je nutné tooassociate ho tooanother objekt tooactually použijte adresu hello.</span><span class="sxs-lookup"><span data-stu-id="b68bc-103">Although you can create a public IP address as a stand alone object, you need tooassociate it tooanother object tooactually use hello address.</span></span> <span data-ttu-id="b68bc-104">Můžete přidružit veřejnou IP adresu tooa služby Vyrovnávání zatížení, aplikační bránu nebo síťový adaptér tooprovide Internetu přístup k toothose prostředkům.</span><span class="sxs-lookup"><span data-stu-id="b68bc-104">You can associate a public IP address tooa load balancer, application  gateway, or a NIC tooprovide Internet access toothose resources.</span></span>  

| <span data-ttu-id="b68bc-105">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b68bc-105">Property</span></span> | <span data-ttu-id="b68bc-106">Popis</span><span class="sxs-lookup"><span data-stu-id="b68bc-106">Description</span></span> | <span data-ttu-id="b68bc-107">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="b68bc-107">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b68bc-108">**publicIPAllocationMethod**</span><span class="sxs-lookup"><span data-stu-id="b68bc-108">**publicIPAllocationMethod**</span></span> |<span data-ttu-id="b68bc-109">Určuje, zda text hello IP adresa je *statické* nebo *dynamické*.</span><span class="sxs-lookup"><span data-stu-id="b68bc-109">Defines if hello IP address is *static* or *dynamic*.</span></span> |<span data-ttu-id="b68bc-110">static, dynamické</span><span class="sxs-lookup"><span data-stu-id="b68bc-110">static, dynamic</span></span> |
| <span data-ttu-id="b68bc-111">**idleTimeoutInMinutes**</span><span class="sxs-lookup"><span data-stu-id="b68bc-111">**idleTimeoutInMinutes**</span></span> |<span data-ttu-id="b68bc-112">Definuje hello nečinnosti, po vypršení časového limitu, s výchozí hodnotou 4 minuty.</span><span class="sxs-lookup"><span data-stu-id="b68bc-112">Defines hello idle time out, with a default value of 4 minutes.</span></span> <span data-ttu-id="b68bc-113">Pokud v tuto chvíli je přijatá žádné další pakety pro dané relace, hello relace je ukončena.</span><span class="sxs-lookup"><span data-stu-id="b68bc-113">If no more packets for a given session is received within this time, hello session is terminated.</span></span> |<span data-ttu-id="b68bc-114">Libovolná hodnota od 4 do 30.</span><span class="sxs-lookup"><span data-stu-id="b68bc-114">any value between 4 and 30</span></span> |
| <span data-ttu-id="b68bc-115">**IP adresa**</span><span class="sxs-lookup"><span data-stu-id="b68bc-115">**ipAddress**</span></span> |<span data-ttu-id="b68bc-116">Tooobject přidělit IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b68bc-116">IP address assigned tooobject.</span></span> <span data-ttu-id="b68bc-117">Toto je vlastnost jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="b68bc-117">This is a read-only property.</span></span> |<span data-ttu-id="b68bc-118">104.42.233.77</span><span class="sxs-lookup"><span data-stu-id="b68bc-118">104.42.233.77</span></span> |

### <a name="dns-settings"></a><span data-ttu-id="b68bc-119">Nastavení DNS</span><span class="sxs-lookup"><span data-stu-id="b68bc-119">DNS settings</span></span>
<span data-ttu-id="b68bc-120">Veřejné IP adresy mít podřízený objekt s názvem **dnsSettings** obsahující hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="b68bc-120">Public IP addresses have a child object named **dnsSettings** containing hello following properties:</span></span>

| <span data-ttu-id="b68bc-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b68bc-121">Property</span></span> | <span data-ttu-id="b68bc-122">Popis</span><span class="sxs-lookup"><span data-stu-id="b68bc-122">Description</span></span> | <span data-ttu-id="b68bc-123">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="b68bc-123">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b68bc-124">**domainNameLabel**</span><span class="sxs-lookup"><span data-stu-id="b68bc-124">**domainNameLabel**</span></span> |<span data-ttu-id="b68bc-125">Hostitel s názvem používají pro překlad.</span><span class="sxs-lookup"><span data-stu-id="b68bc-125">Host named used for name resolution.</span></span> |<span data-ttu-id="b68bc-126">WWW, ftp, vm1</span><span class="sxs-lookup"><span data-stu-id="b68bc-126">www, ftp, vm1</span></span> |
| <span data-ttu-id="b68bc-127">**plně kvalifikovaný název domény**</span><span class="sxs-lookup"><span data-stu-id="b68bc-127">**fqdn**</span></span> |<span data-ttu-id="b68bc-128">Plně kvalifikovaný název pro hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b68bc-128">Fully qualified name for hello public IP.</span></span> |<span data-ttu-id="b68bc-129">www.westus.cloudapp.Azure.com</span><span class="sxs-lookup"><span data-stu-id="b68bc-129">www.westus.cloudapp.azure.com</span></span> |
| <span data-ttu-id="b68bc-130">**reverseFqdn**</span><span class="sxs-lookup"><span data-stu-id="b68bc-130">**reverseFqdn**</span></span> |<span data-ttu-id="b68bc-131">Název plně kvalifikované domény, který přeloží toohello IP adresy a je zaregistrován ve službě DNS, jako záznam PTR.</span><span class="sxs-lookup"><span data-stu-id="b68bc-131">Fully qualified domain name that resolves toohello IP address and is registered in DNS as a PTR record.</span></span> |<span data-ttu-id="b68bc-132">www.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="b68bc-132">www.contoso.com.</span></span> |

<span data-ttu-id="b68bc-133">Ukázka veřejnou IP adresu ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="b68bc-133">Sample public IP address in JSON format:</span></span>

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

### <a name="additional-resources"></a><span data-ttu-id="b68bc-134">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b68bc-134">Additional resources</span></span>
* <span data-ttu-id="b68bc-135">Přečtěte si další informace o [veřejné IP adresy](../articles/virtual-network/virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="b68bc-135">Get more information about [public IP addresses](../articles/virtual-network/virtual-networks-reserved-public-ip.md).</span></span>
* <span data-ttu-id="b68bc-136">Další informace o [instance úrovně veřejné IP adresy](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="b68bc-136">Learn about [instance level public IP addresses](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).</span></span>
* <span data-ttu-id="b68bc-137">Čtení hello [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163638.aspx) pro veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b68bc-137">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163638.aspx) for public IP addresses.</span></span>

