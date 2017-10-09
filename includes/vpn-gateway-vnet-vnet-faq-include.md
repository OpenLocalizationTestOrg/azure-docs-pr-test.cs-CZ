<span data-ttu-id="2970b-101">Nejčastější dotazy týkající se propojení VNet-to-VNet Hello platí tooVPN připojení brány.</span><span class="sxs-lookup"><span data-stu-id="2970b-101">hello VNet-to-VNet FAQ applies tooVPN Gateway connections.</span></span> <span data-ttu-id="2970b-102">Pokud hledáte informace o VNet Peering, přečtěte si téma [Partnerské vztahy virtuálních sítí](../articles/virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2970b-102">If you are looking for VNet Peering, see [Virtual Network Peering](../articles/virtual-network/virtual-network-peering-overview.md)</span></span>

### <a name="does-azure-charge-for-traffic-between-vnets"></a><span data-ttu-id="2970b-103">Účtuje se v Azure provoz mezi virtuálními sítěmi?</span><span class="sxs-lookup"><span data-stu-id="2970b-103">Does Azure charge for traffic between VNets?</span></span>

<span data-ttu-id="2970b-104">Provoz VNet-to-VNet v rámci hello stejné oblasti je obou směrech zdarma při použití připojení k bráně VPN.</span><span class="sxs-lookup"><span data-stu-id="2970b-104">VNet-to-VNet traffic within hello same region is free for both directions when using a VPN gateway connection.</span></span> <span data-ttu-id="2970b-105">Pro různé oblasti VNet-to-VNet odchozí provoz je pověřen hello odchozí mezi virtuálními sazby za přenos dat podle zdrojových oblastí hello.</span><span class="sxs-lookup"><span data-stu-id="2970b-105">Cross region VNet-to-VNet egress traffic is charged with hello outbound inter-VNet data transfer rates based on hello source regions.</span></span> <span data-ttu-id="2970b-106">Odkazovat toohello [brány VPN stránce s cenami](https://azure.microsoft.com/pricing/details/vpn-gateway/) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2970b-106">Refer toohello [VPN Gateway pricing page](https://azure.microsoft.com/pricing/details/vpn-gateway/) for details.</span></span> <span data-ttu-id="2970b-107">Pokud se připojujete vaší virtuální sítě pomocí virtuální sítě partnerský vztah, nikoli brány sítě VPN, přečtěte si téma hello [virtuální sítě stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="2970b-107">If you are connecting your VNets using VNet Peering, rather than VPN Gateway, see hello [Virtual Network pricing page](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>

### <a name="does-vnet-to-vnet-traffic-travel-across-hello-internet"></a><span data-ttu-id="2970b-108">Cestují provoz VNet-to-VNet přes hello Internetu?</span><span class="sxs-lookup"><span data-stu-id="2970b-108">Does VNet-to-VNet traffic travel across hello Internet?</span></span>

<span data-ttu-id="2970b-109">Ne.</span><span class="sxs-lookup"><span data-stu-id="2970b-109">No.</span></span> <span data-ttu-id="2970b-110">Provoz VNet-to-VNet se přenáší po hello páteřní Microsoft Azure, hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="2970b-110">VNet-to-VNet traffic travels across hello Microsoft Azure backbone, not hello Internet.</span></span>

### <a name="is-vnet-to-vnet-traffic-secure"></a><span data-ttu-id="2970b-111">Je provoz VNet-to-VNet bezpečný?</span><span class="sxs-lookup"><span data-stu-id="2970b-111">Is VNet-to-VNet traffic secure?</span></span>

<span data-ttu-id="2970b-112">Ano, je chráněn šifrováním s použitím protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="2970b-112">Yes, it is protected by IPsec/IKE encryption.</span></span>

### <a name="do-i-need-a-vpn-device-tooconnect-vnets-together"></a><span data-ttu-id="2970b-113">Společně tooconnect zařízení VPN virtuální sítě potřeba?</span><span class="sxs-lookup"><span data-stu-id="2970b-113">Do I need a VPN device tooconnect VNets together?</span></span>

<span data-ttu-id="2970b-114">Ne.</span><span class="sxs-lookup"><span data-stu-id="2970b-114">No.</span></span> <span data-ttu-id="2970b-115">Propojení více virtuálních sítí Azure nevyžaduje žádná zařízení VPN, pokud není vyžadována možnost připojení mezi různými místy.</span><span class="sxs-lookup"><span data-stu-id="2970b-115">Connecting multiple Azure virtual networks together doesn't require a VPN device unless cross-premises connectivity is required.</span></span>

### <a name="do-my-vnets-need-toobe-in-hello-same-region"></a><span data-ttu-id="2970b-116">Můj virtuální sítě nutná toobe v hello stejné oblasti?</span><span class="sxs-lookup"><span data-stu-id="2970b-116">Do my VNets need toobe in hello same region?</span></span>

<span data-ttu-id="2970b-117">Ne.</span><span class="sxs-lookup"><span data-stu-id="2970b-117">No.</span></span> <span data-ttu-id="2970b-118">Hello virtuální sítě může být v hello oblastí Azure stejný nebo jiný (umístění).</span><span class="sxs-lookup"><span data-stu-id="2970b-118">hello virtual networks can be in hello same or different Azure regions (locations).</span></span>

### <a name="if-hello-vnets-are-not-in-hello-same-subscription-do-hello-subscriptions-need-toobe-associated-with-hello-same-ad-tenant"></a><span data-ttu-id="2970b-119">Pokud hello virtuálních sítí nejsou v hello stejné předplatné, odběry hello potřebují toobe přidružený klientovi hello stejnou AD?</span><span class="sxs-lookup"><span data-stu-id="2970b-119">If hello VNets are not in hello same subscription, do hello subscriptions need toobe associated with hello same AD tenant?</span></span>

<span data-ttu-id="2970b-120">Ne.</span><span class="sxs-lookup"><span data-stu-id="2970b-120">No.</span></span>

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a><span data-ttu-id="2970b-121">Je možné použít VNet-to-VNet společně s připojením propojujícím víc serverů?</span><span class="sxs-lookup"><span data-stu-id="2970b-121">Can I use VNet-to-VNet along with multi-site connections?</span></span>

<span data-ttu-id="2970b-122">Ano.</span><span class="sxs-lookup"><span data-stu-id="2970b-122">Yes.</span></span> <span data-ttu-id="2970b-123">Možnost připojení k virtuální síti je možné využívat současně se sítěmi VPN s více servery.</span><span class="sxs-lookup"><span data-stu-id="2970b-123">Virtual network connectivity can be used simultaneously with multi-site VPNs.</span></span>

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a><span data-ttu-id="2970b-124">Ke kolika místních serverům a virtuálním sítím se může připojit jedna virtuální síť?</span><span class="sxs-lookup"><span data-stu-id="2970b-124">How many on-premises sites and virtual networks can one virtual network connect to?</span></span>

<span data-ttu-id="2970b-125">Viz tabulka [Požadavky na bránu](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements).</span><span class="sxs-lookup"><span data-stu-id="2970b-125">See [Gateway requirements](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements) table.</span></span>

### <a name="can-i-use-vnet-to-vnet-tooconnect-vms-or-cloud-services-outside-of-a-vnet"></a><span data-ttu-id="2970b-126">Můžete použít VNet-to-VNet tooconnect virtuálních počítačů nebo cloudových služeb mimo virtuální síť?</span><span class="sxs-lookup"><span data-stu-id="2970b-126">Can I use VNet-to-VNet tooconnect VMs or cloud services outside of a VNet?</span></span>

<span data-ttu-id="2970b-127">Ne.</span><span class="sxs-lookup"><span data-stu-id="2970b-127">No.</span></span> <span data-ttu-id="2970b-128">Propojení VNet-to-VNet podporují propojování virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="2970b-128">VNet-to-VNet supports connecting virtual networks.</span></span> <span data-ttu-id="2970b-129">Nepodporuje propojování virtuálních počítačů ani cloudových služeb mimo virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="2970b-129">It does not support connecting virtual machines or cloud services that are not in a virtual network.</span></span>

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a><span data-ttu-id="2970b-130">Může cloudová služba nebo koncový bod vyrovnávání zatížení pracovat nad více virtuálními sítěmi?</span><span class="sxs-lookup"><span data-stu-id="2970b-130">Can a cloud service or a load balancing endpoint span VNets?</span></span>

<span data-ttu-id="2970b-131">Ne.</span><span class="sxs-lookup"><span data-stu-id="2970b-131">No.</span></span> <span data-ttu-id="2970b-132">Cloudová služba ani koncový bod vyrovnávání zatížení nemůžou pracovat nad více virtuálními sítěmi ani v případě, že jsou propojeny.</span><span class="sxs-lookup"><span data-stu-id="2970b-132">A cloud service or a load balancing endpoint can't span across virtual networks, even if they are connected together.</span></span>

### <a name="can-i-used-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a><span data-ttu-id="2970b-133">Je možné použít typ sítě VPN PolicyBased pro připojení VNet-to-VNet nebo Multi-Site?</span><span class="sxs-lookup"><span data-stu-id="2970b-133">Can I used a PolicyBased VPN type for VNet-to-VNet or Multi-Site connections?</span></span>

<span data-ttu-id="2970b-134">Ne.</span><span class="sxs-lookup"><span data-stu-id="2970b-134">No.</span></span> <span data-ttu-id="2970b-135">Připojení VNet-to-VNet a Multi-Site vyžadují brány VPN Azure s typy sítě VPN RouteBased (dříve nazývané dynamické směrování).</span><span class="sxs-lookup"><span data-stu-id="2970b-135">VNet-to-VNet and Multi-Site connections require Azure VPN gateways with RouteBased (previously called Dynamic Routing) VPN types.</span></span>

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-tooanother-vnet-with-a-policybased-vpn-type"></a><span data-ttu-id="2970b-136">Je možné připojit virtuální síť s typ sítě VPN RouteBased tooanother virtuální síť s typu Policybased?</span><span class="sxs-lookup"><span data-stu-id="2970b-136">Can I connect a VNet with a RouteBased VPN Type tooanother VNet with a PolicyBased VPN type?</span></span>

<span data-ttu-id="2970b-137">Ne, obě virtuální sítě MUSÍ používat sítě VPN založené na směrování (dříve nazývané dynamické směrování).</span><span class="sxs-lookup"><span data-stu-id="2970b-137">No, both virtual networks MUST be using route-based (previously called Dynamic Routing) VPNs.</span></span>

### <a name="do-vpn-tunnels-share-bandwidth"></a><span data-ttu-id="2970b-138">Sdílejí tunely VPN šířku pásma?</span><span class="sxs-lookup"><span data-stu-id="2970b-138">Do VPN tunnels share bandwidth?</span></span>

<span data-ttu-id="2970b-139">Ano.</span><span class="sxs-lookup"><span data-stu-id="2970b-139">Yes.</span></span> <span data-ttu-id="2970b-140">Všechna tunelová propojení VPN virtuální sítě hello sdílet hello dostupnou šířku pásma hello Azure VPN gateway a hello stejné SLA provozu v Azure VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="2970b-140">All VPN tunnels of hello virtual network share hello available bandwidth on hello Azure VPN gateway and hello same VPN gateway uptime SLA in Azure.</span></span>

### <a name="are-redundant-tunnels-supported"></a><span data-ttu-id="2970b-141">Jsou podporovány redundantní tunely?</span><span class="sxs-lookup"><span data-stu-id="2970b-141">Are redundant tunnels supported?</span></span>

<span data-ttu-id="2970b-142">Redundantní tunely mezi párem virtuálních sítí jsou podporovány, pokud je jedna brána virtuální sítě nakonfigurována jako aktivní-aktivní.</span><span class="sxs-lookup"><span data-stu-id="2970b-142">Redundant tunnels between a pair of virtual networks are supported when one virtual network gateway is configured as active-active.</span></span>

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a><span data-ttu-id="2970b-143">Můžou se překrývat adresní prostory pro konfigurace VNet-to-VNet?</span><span class="sxs-lookup"><span data-stu-id="2970b-143">Can I have overlapping address spaces for VNet-to-VNet configurations?</span></span>

<span data-ttu-id="2970b-144">Ne.</span><span class="sxs-lookup"><span data-stu-id="2970b-144">No.</span></span> <span data-ttu-id="2970b-145">Není možné, aby se rozsahy IP adres překrývaly.</span><span class="sxs-lookup"><span data-stu-id="2970b-145">You can't have overlapping IP address ranges.</span></span>

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a><span data-ttu-id="2970b-146">Můžou se překrývat adresní prostory mezi propojenými virtuálními sítěmi a místními servery?</span><span class="sxs-lookup"><span data-stu-id="2970b-146">Can there be overlapping address spaces among connected virtual networks and on-premises local sites?</span></span>

<span data-ttu-id="2970b-147">Ne.</span><span class="sxs-lookup"><span data-stu-id="2970b-147">No.</span></span> <span data-ttu-id="2970b-148">Není možné, aby se rozsahy IP adres překrývaly.</span><span class="sxs-lookup"><span data-stu-id="2970b-148">You can't have overlapping IP address ranges.</span></span>



