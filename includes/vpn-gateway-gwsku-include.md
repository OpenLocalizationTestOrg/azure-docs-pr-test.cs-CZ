<span data-ttu-id="088e0-101">Při vytváření brány virtuální sítě musíte určit SKU brány, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="088e0-101">When you create a virtual network gateway, you need to specify the gateway SKU that you want to use.</span></span> <span data-ttu-id="088e0-102">Vyberte SKU, které splňují vaše požadavky na základě typů úloh, propustnosti, funkcí a SLA.</span><span class="sxs-lookup"><span data-stu-id="088e0-102">Select the SKUs that satisfy your requirements based on the types of workloads, throughputs, features, and SLAs.</span></span>

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <span data-ttu-id="088e0-103"><a name="workloads"></a>Produkční *vs.* vývojářské a testovací úlohy</span><span class="sxs-lookup"><span data-stu-id="088e0-103"><a name="workloads"></a>Production *vs.* Dev-Test Workloads</span></span>

<span data-ttu-id="088e0-104">Vzhledem k rozdílům ve SLA a sadách funkcí doporučujeme pro produkci *vs.* vývoj a testování následující SKU:</span><span class="sxs-lookup"><span data-stu-id="088e0-104">Due to the differences in SLAs and feature sets, we recommend the following SKUs for production *vs.* dev-test:</span></span>

| <span data-ttu-id="088e0-105">**Úloha**</span><span class="sxs-lookup"><span data-stu-id="088e0-105">**Workload**</span></span>                       | <span data-ttu-id="088e0-106">**SKU**</span><span class="sxs-lookup"><span data-stu-id="088e0-106">**SKUs**</span></span>               |
| ---                                | ---                    |
| <span data-ttu-id="088e0-107">**Produkce, kritické úlohy**</span><span class="sxs-lookup"><span data-stu-id="088e0-107">**Production, critical workloads**</span></span> | <span data-ttu-id="088e0-108">VpnGw1, VpnGw2, VpnGw3</span><span class="sxs-lookup"><span data-stu-id="088e0-108">VpnGw1, VpnGw2, VpnGw3</span></span> |
| <span data-ttu-id="088e0-109">**Vývoj a testování nebo testování konceptu**</span><span class="sxs-lookup"><span data-stu-id="088e0-109">**Dev-test or proof of concept**</span></span>   | <span data-ttu-id="088e0-110">Basic</span><span class="sxs-lookup"><span data-stu-id="088e0-110">Basic</span></span>                  |
|                                    |                        |

<span data-ttu-id="088e0-111">Pokud používáte staré SKU, pro produkci se doporučují Standard a HighPerformance SKU.</span><span class="sxs-lookup"><span data-stu-id="088e0-111">If you are using the old SKUs, the production SKU recommendations are Standard and HighPerformance SKUs.</span></span> <span data-ttu-id="088e0-112">Informace o starých skladových položkách najdete v tématu [Skladové položky brány (starší skladové položky)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="088e0-112">For information on the old SKUs, see [Gateway SKUs (legacy SKUs)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).</span></span>

###  <span data-ttu-id="088e0-113"><a name="feature"></a>Sady funkcí SKU brány</span><span class="sxs-lookup"><span data-stu-id="088e0-113"><a name="feature"></a>Gateway SKU feature sets</span></span>

<span data-ttu-id="088e0-114">Nové SKU brány zjednodušují sady funkcí, které se na branách nabízejí:</span><span class="sxs-lookup"><span data-stu-id="088e0-114">The new gateway SKUs streamline the feature sets offered on the gateways:</span></span>

| <span data-ttu-id="088e0-115">**SKU**</span><span class="sxs-lookup"><span data-stu-id="088e0-115">**SKU**</span></span>| <span data-ttu-id="088e0-116">**Funkce**</span><span class="sxs-lookup"><span data-stu-id="088e0-116">**Features**</span></span>|
| ---    | ---         |
|<span data-ttu-id="088e0-117">**Basic**</span><span class="sxs-lookup"><span data-stu-id="088e0-117">**Basic**</span></span>   | <span data-ttu-id="088e0-118">**Síť VPN založená na směrování:** 10 tunelů s P2S</span><span class="sxs-lookup"><span data-stu-id="088e0-118">**Route-based VPN**: 10 tunnels with P2S</span></span><br><br><span data-ttu-id="088e0-119">**Síť VPN založená na zásadách (IKEv1):** 1 tunel, bez P2S</span><span class="sxs-lookup"><span data-stu-id="088e0-119">**Policy-based VPN**: (IKEv1): 1 tunnel; no P2S</span></span>|
| <span data-ttu-id="088e0-120">**VpnGw1, VpnGw2 a VpnGw3**</span><span class="sxs-lookup"><span data-stu-id="088e0-120">**VpnGw1, VpnGw2, and VpnGw3**</span></span> | <span data-ttu-id="088e0-121">**Síť VPN založená na směrování:** až 30 tunelů (*), P2S, BGP, aktivní-aktivní, vlastní zásady IPsec/IKE, koexistence ExpressRoute/VPN</span><span class="sxs-lookup"><span data-stu-id="088e0-121">**Route-based VPN**: up to 30 tunnels (*), P2S, BGP, active-active, custom IPsec/IKE policy, ExpressRoute/VPN co-existence</span></span> |
|        |             |

<span data-ttu-id="088e0-122">(*) Můžete nakonfigurovat PolicyBasedTrafficSelectors pro připojení brány sítě VPN založené na směrování (VpnGw1, VpnGw2, VpnGw3) k několika místním zařízením brány firewall založeným na zásadách.</span><span class="sxs-lookup"><span data-stu-id="088e0-122">(*) You can configure "PolicyBasedTrafficSelectors" to connect a route-based VPN gateway (VpnGw1, VpnGw2, VpnGw3) to multiple on-premises policy-based firewall devices.</span></span> <span data-ttu-id="088e0-123">Podrobnosti najdete v tématu věnovaném [připojení bran VPN k několika místním zařízením VPN založeným na zásadách s využitím PowerShellu](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="088e0-123">Refer to [Connect VPN gateways to multiple on-premises policy-based VPN devices using PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) for details.</span></span>

###  <span data-ttu-id="088e0-124"><a name="resize"></a>Změna velikosti SKU brány</span><span class="sxs-lookup"><span data-stu-id="088e0-124"><a name="resize"></a>Resizing gateway SKUs</span></span>

1. <span data-ttu-id="088e0-125">Můžete měnit velikost mezi VpnGw1, VpnGw2 a VpnGw3 SKU.</span><span class="sxs-lookup"><span data-stu-id="088e0-125">You can resize between VpnGw1, VpnGw2, and VpnGw3 SKUs.</span></span>
2. <span data-ttu-id="088e0-126">Pokud používáte staré SKU brány, můžete měnit velikost mezi Basic, Standard a HighPerformance SKU.</span><span class="sxs-lookup"><span data-stu-id="088e0-126">When working with the old gateway SKUs, you can resize between Basic, Standard, and HighPerformance SKUs.</span></span>
2. <span data-ttu-id="088e0-127">**Není možné** změnit velikost z Basic/Standard/HighPerformance SKU na nové VpnGw1/VpnGw2/VpnGw3 SKU.</span><span class="sxs-lookup"><span data-stu-id="088e0-127">You **cannot** resize from Basic/Standard/HighPerformance SKUs to the new VpnGw1/VpnGw2/VpnGw3 SKUs.</span></span> <span data-ttu-id="088e0-128">Místo toho je nutné na nové SKU [migrovat](#migrate).</span><span class="sxs-lookup"><span data-stu-id="088e0-128">You must, instead, [migrate](#migrate) to the new SKUs.</span></span>

###  <span data-ttu-id="088e0-129"><a name="migrate"></a>Migrace ze starých SKU na nové SKU</span><span class="sxs-lookup"><span data-stu-id="088e0-129"><a name="migrate"></a>Migrating from old SKUs to the new SKUs</span></span>

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]
