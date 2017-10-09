<span data-ttu-id="62c43-101">Když vytvoříte bránu virtuální sítě, je nutné toospecify hello brány, které chcete toouse SKU.</span><span class="sxs-lookup"><span data-stu-id="62c43-101">When you create a virtual network gateway, you need toospecify hello gateway SKU that you want toouse.</span></span> <span data-ttu-id="62c43-102">Vyberte hello SKU, které splňují vaše požadavky na základě typů hello zatížení, propustnost, funkce a SLA.</span><span class="sxs-lookup"><span data-stu-id="62c43-102">Select hello SKUs that satisfy your requirements based on hello types of workloads, throughputs, features, and SLAs.</span></span>

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <span data-ttu-id="62c43-103"><a name="workloads"></a>Produkční *vs.* vývojářské a testovací úlohy</span><span class="sxs-lookup"><span data-stu-id="62c43-103"><a name="workloads"></a>Production *vs.* Dev-Test Workloads</span></span>

<span data-ttu-id="62c43-104">Z důvodu toohello rozdíly v SLA a sady funkcí, doporučujeme následující SKU pro produkční prostředí hello *oproti* vývoj testování:</span><span class="sxs-lookup"><span data-stu-id="62c43-104">Due toohello differences in SLAs and feature sets, we recommend hello following SKUs for production *vs.* dev-test:</span></span>

| <span data-ttu-id="62c43-105">**Úloha**</span><span class="sxs-lookup"><span data-stu-id="62c43-105">**Workload**</span></span>                       | <span data-ttu-id="62c43-106">**SKU**</span><span class="sxs-lookup"><span data-stu-id="62c43-106">**SKUs**</span></span>               |
| ---                                | ---                    |
| <span data-ttu-id="62c43-107">**Produkce, kritické úlohy**</span><span class="sxs-lookup"><span data-stu-id="62c43-107">**Production, critical workloads**</span></span> | <span data-ttu-id="62c43-108">VpnGw1, VpnGw2, VpnGw3</span><span class="sxs-lookup"><span data-stu-id="62c43-108">VpnGw1, VpnGw2, VpnGw3</span></span> |
| <span data-ttu-id="62c43-109">**Vývoj a testování nebo testování konceptu**</span><span class="sxs-lookup"><span data-stu-id="62c43-109">**Dev-test or proof of concept**</span></span>   | <span data-ttu-id="62c43-110">Basic</span><span class="sxs-lookup"><span data-stu-id="62c43-110">Basic</span></span>                  |
|                                    |                        |

<span data-ttu-id="62c43-111">Pokud používáte hello staré SKU, hello produkční SKU doporučení jsou standardní a HighPerformance SKU.</span><span class="sxs-lookup"><span data-stu-id="62c43-111">If you are using hello old SKUs, hello production SKU recommendations are Standard and HighPerformance SKUs.</span></span> <span data-ttu-id="62c43-112">Informace o hello staré SKU, najdete v části [SKU brány (starší verze SKU)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="62c43-112">For information on hello old SKUs, see [Gateway SKUs (legacy SKUs)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).</span></span>

###  <span data-ttu-id="62c43-113"><a name="feature"></a>Sady funkcí SKU brány</span><span class="sxs-lookup"><span data-stu-id="62c43-113"><a name="feature"></a>Gateway SKU feature sets</span></span>

<span data-ttu-id="62c43-114">nové SKU brány Hello zjednodušit sady funkcí hello nabízí na branách hello:</span><span class="sxs-lookup"><span data-stu-id="62c43-114">hello new gateway SKUs streamline hello feature sets offered on hello gateways:</span></span>

| <span data-ttu-id="62c43-115">**SKU**</span><span class="sxs-lookup"><span data-stu-id="62c43-115">**SKU**</span></span>| <span data-ttu-id="62c43-116">**Funkce**</span><span class="sxs-lookup"><span data-stu-id="62c43-116">**Features**</span></span>|
| ---    | ---         |
|<span data-ttu-id="62c43-117">**Basic**</span><span class="sxs-lookup"><span data-stu-id="62c43-117">**Basic**</span></span>   | <span data-ttu-id="62c43-118">**Síť VPN založená na směrování:** 10 tunelů s P2S</span><span class="sxs-lookup"><span data-stu-id="62c43-118">**Route-based VPN**: 10 tunnels with P2S</span></span><br><br><span data-ttu-id="62c43-119">**Síť VPN založená na zásadách (IKEv1):** 1 tunel, bez P2S</span><span class="sxs-lookup"><span data-stu-id="62c43-119">**Policy-based VPN**: (IKEv1): 1 tunnel; no P2S</span></span>|
| <span data-ttu-id="62c43-120">**VpnGw1, VpnGw2 a VpnGw3**</span><span class="sxs-lookup"><span data-stu-id="62c43-120">**VpnGw1, VpnGw2, and VpnGw3**</span></span> | <span data-ttu-id="62c43-121">**Sítě VPN založené na trasách**: až too30 tunely (*), P2S, protokol BGP, aktivní aktivní, vlastní zásady protokolu IPsec/IKE, ExpressRoute nebo VPN koexistenci</span><span class="sxs-lookup"><span data-stu-id="62c43-121">**Route-based VPN**: up too30 tunnels (*), P2S, BGP, active-active, custom IPsec/IKE policy, ExpressRoute/VPN co-existence</span></span> |
|        |             |

<span data-ttu-id="62c43-122">(*) Můžete nakonfigurovat tooconnect "PolicyBasedTrafficSelectors" založená na trasách zařízení brány VPN (VpnGw1, VpnGw2, VpnGw3) toomultiple místní brány firewall na základě zásad.</span><span class="sxs-lookup"><span data-stu-id="62c43-122">(*) You can configure "PolicyBasedTrafficSelectors" tooconnect a route-based VPN gateway (VpnGw1, VpnGw2, VpnGw3) toomultiple on-premises policy-based firewall devices.</span></span> <span data-ttu-id="62c43-123">Odkazovat příliš[toomultiple připojení VPN Gateway místně na základě zásad zařízení VPN pomocí prostředí PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="62c43-123">Refer too[Connect VPN gateways toomultiple on-premises policy-based VPN devices using PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) for details.</span></span>

###  <span data-ttu-id="62c43-124"><a name="resize"></a>Změna velikosti SKU brány</span><span class="sxs-lookup"><span data-stu-id="62c43-124"><a name="resize"></a>Resizing gateway SKUs</span></span>

1. <span data-ttu-id="62c43-125">Můžete měnit velikost mezi VpnGw1, VpnGw2 a VpnGw3 SKU.</span><span class="sxs-lookup"><span data-stu-id="62c43-125">You can resize between VpnGw1, VpnGw2, and VpnGw3 SKUs.</span></span>
2. <span data-ttu-id="62c43-126">Při práci s původním SKU brány hello, můžete změnit velikost mezi Basic, Standard a HighPerformance SKU.</span><span class="sxs-lookup"><span data-stu-id="62c43-126">When working with hello old gateway SKUs, you can resize between Basic, Standard, and HighPerformance SKUs.</span></span>
2. <span data-ttu-id="62c43-127">Můžete **nelze** změnit velikost z SKU Basic nebo Standard nebo HighPerformance toohello nové VpnGw1/VpnGw2/VpnGw3 SKU.</span><span class="sxs-lookup"><span data-stu-id="62c43-127">You **cannot** resize from Basic/Standard/HighPerformance SKUs toohello new VpnGw1/VpnGw2/VpnGw3 SKUs.</span></span> <span data-ttu-id="62c43-128">Místo toho musíte [migrovat](#migrate) toohello nové SKU.</span><span class="sxs-lookup"><span data-stu-id="62c43-128">You must, instead, [migrate](#migrate) toohello new SKUs.</span></span>

###  <span data-ttu-id="62c43-129"><a name="migrate"></a>Migrace z původního toohello SKU nové SKU</span><span class="sxs-lookup"><span data-stu-id="62c43-129"><a name="migrate"></a>Migrating from old SKUs toohello new SKUs</span></span>

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]
