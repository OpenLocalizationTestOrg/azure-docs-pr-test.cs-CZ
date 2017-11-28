<span data-ttu-id="b9625-101">Nejčastější dotazy týkající se propojení VNet-to-VNet se vztahují k připojení služby VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="b9625-101">The VNet-to-VNet FAQ applies to VPN Gateway connections.</span></span> <span data-ttu-id="b9625-102">Pokud hledáte informace o VNet Peering, přečtěte si téma [Partnerské vztahy virtuálních sítí](../articles/virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b9625-102">If you are looking for VNet Peering, see [Virtual Network Peering](../articles/virtual-network/virtual-network-peering-overview.md)</span></span>

### <a name="does-azure-charge-for-traffic-between-vnets"></a><span data-ttu-id="b9625-103">Účtuje se v Azure provoz mezi virtuálními sítěmi?</span><span class="sxs-lookup"><span data-stu-id="b9625-103">Does Azure charge for traffic between VNets?</span></span>

<span data-ttu-id="b9625-104">Při použití připojení brány VPN je provoz mezi virtuálními sítěmi v rámci stejné oblasti bezplatný v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="b9625-104">VNet-to-VNet traffic within the same region is free for both directions when using a VPN gateway connection.</span></span> <span data-ttu-id="b9625-105">V případě různých oblastí je výchozí přenos VNet-to-VNet zpoplatněn jako odchozí přenos dat mezi virtuálními sítěmi podle zdrojových oblastí.</span><span class="sxs-lookup"><span data-stu-id="b9625-105">Cross region VNet-to-VNet egress traffic is charged with the outbound inter-VNet data transfer rates based on the source regions.</span></span> <span data-ttu-id="b9625-106">Podrobnosti najdete na [stránce s cenami služby VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway/).</span><span class="sxs-lookup"><span data-stu-id="b9625-106">Refer to the [VPN Gateway pricing page](https://azure.microsoft.com/pricing/details/vpn-gateway/) for details.</span></span> <span data-ttu-id="b9625-107">Pokud virtuální sítě propojujete pomocí partnerského vztahu virtuálních sítí místo služby VPN Gateway, podívejte se na [stránku s cenami služby Virtual Network](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="b9625-107">If you are connecting your VNets using VNet Peering, rather than VPN Gateway, see the [Virtual Network pricing page](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>

### <a name="does-vnet-to-vnet-traffic-travel-across-the-internet"></a><span data-ttu-id="b9625-108">Prochází provoz VNet-to-VNet přes internet?</span><span class="sxs-lookup"><span data-stu-id="b9625-108">Does VNet-to-VNet traffic travel across the Internet?</span></span>

<span data-ttu-id="b9625-109">Ne.</span><span class="sxs-lookup"><span data-stu-id="b9625-109">No.</span></span> <span data-ttu-id="b9625-110">Provoz VNet-to-VNet se přenáší prostřednictvím páteřní struktury systému Microsoft Azure, nikoli po internetu.</span><span class="sxs-lookup"><span data-stu-id="b9625-110">VNet-to-VNet traffic travels across the Microsoft Azure backbone, not the Internet.</span></span>

### <a name="is-vnet-to-vnet-traffic-secure"></a><span data-ttu-id="b9625-111">Je provoz VNet-to-VNet bezpečný?</span><span class="sxs-lookup"><span data-stu-id="b9625-111">Is VNet-to-VNet traffic secure?</span></span>

<span data-ttu-id="b9625-112">Ano, je chráněn šifrováním s použitím protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="b9625-112">Yes, it is protected by IPsec/IKE encryption.</span></span>

### <a name="do-i-need-a-vpn-device-to-connect-vnets-together"></a><span data-ttu-id="b9625-113">Je k propojení virtuálních sítí potřeba zařízení VPN?</span><span class="sxs-lookup"><span data-stu-id="b9625-113">Do I need a VPN device to connect VNets together?</span></span>

<span data-ttu-id="b9625-114">Ne.</span><span class="sxs-lookup"><span data-stu-id="b9625-114">No.</span></span> <span data-ttu-id="b9625-115">Propojení více virtuálních sítí Azure nevyžaduje žádná zařízení VPN, pokud není vyžadována možnost připojení mezi různými místy.</span><span class="sxs-lookup"><span data-stu-id="b9625-115">Connecting multiple Azure virtual networks together doesn't require a VPN device unless cross-premises connectivity is required.</span></span>

### <a name="do-my-vnets-need-to-be-in-the-same-region"></a><span data-ttu-id="b9625-116">Je nutné, aby virtuální sítě byly ve stejné oblasti?</span><span class="sxs-lookup"><span data-stu-id="b9625-116">Do my VNets need to be in the same region?</span></span>

<span data-ttu-id="b9625-117">Ne.</span><span class="sxs-lookup"><span data-stu-id="b9625-117">No.</span></span> <span data-ttu-id="b9625-118">Virtuální sítě se můžou nacházet ve stejné oblasti (umístění) Azure nebo v různých oblastech.</span><span class="sxs-lookup"><span data-stu-id="b9625-118">The virtual networks can be in the same or different Azure regions (locations).</span></span>

### <a name="if-the-vnets-are-not-in-the-same-subscription-do-the-subscriptions-need-to-be-associated-with-the-same-ad-tenant"></a><span data-ttu-id="b9625-119">Pokud virtuální sítě nejsou ve stejném předplatném, musí být přidružené ke stejnému tenantovi AD?</span><span class="sxs-lookup"><span data-stu-id="b9625-119">If the VNets are not in the same subscription, do the subscriptions need to be associated with the same AD tenant?</span></span>

<span data-ttu-id="b9625-120">Ne.</span><span class="sxs-lookup"><span data-stu-id="b9625-120">No.</span></span>

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a><span data-ttu-id="b9625-121">Je možné použít VNet-to-VNet společně s připojením propojujícím víc serverů?</span><span class="sxs-lookup"><span data-stu-id="b9625-121">Can I use VNet-to-VNet along with multi-site connections?</span></span>

<span data-ttu-id="b9625-122">Ano.</span><span class="sxs-lookup"><span data-stu-id="b9625-122">Yes.</span></span> <span data-ttu-id="b9625-123">Možnost připojení k virtuální síti je možné využívat současně se sítěmi VPN s více servery.</span><span class="sxs-lookup"><span data-stu-id="b9625-123">Virtual network connectivity can be used simultaneously with multi-site VPNs.</span></span>

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a><span data-ttu-id="b9625-124">Ke kolika místních serverům a virtuálním sítím se může připojit jedna virtuální síť?</span><span class="sxs-lookup"><span data-stu-id="b9625-124">How many on-premises sites and virtual networks can one virtual network connect to?</span></span>

<span data-ttu-id="b9625-125">Viz tabulka [Požadavky na bránu](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements).</span><span class="sxs-lookup"><span data-stu-id="b9625-125">See [Gateway requirements](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements) table.</span></span>

### <a name="can-i-use-vnet-to-vnet-to-connect-vms-or-cloud-services-outside-of-a-vnet"></a><span data-ttu-id="b9625-126">Je možné použít VNet-to-VNet k propojení virtuálních počítačů nebo cloudových služeb mimo virtuální síť?</span><span class="sxs-lookup"><span data-stu-id="b9625-126">Can I use VNet-to-VNet to connect VMs or cloud services outside of a VNet?</span></span>

<span data-ttu-id="b9625-127">Ne.</span><span class="sxs-lookup"><span data-stu-id="b9625-127">No.</span></span> <span data-ttu-id="b9625-128">Propojení VNet-to-VNet podporují propojování virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="b9625-128">VNet-to-VNet supports connecting virtual networks.</span></span> <span data-ttu-id="b9625-129">Nepodporuje propojování virtuálních počítačů ani cloudových služeb mimo virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="b9625-129">It does not support connecting virtual machines or cloud services that are not in a virtual network.</span></span>

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a><span data-ttu-id="b9625-130">Může cloudová služba nebo koncový bod vyrovnávání zatížení pracovat nad více virtuálními sítěmi?</span><span class="sxs-lookup"><span data-stu-id="b9625-130">Can a cloud service or a load balancing endpoint span VNets?</span></span>

<span data-ttu-id="b9625-131">Ne.</span><span class="sxs-lookup"><span data-stu-id="b9625-131">No.</span></span> <span data-ttu-id="b9625-132">Cloudová služba ani koncový bod vyrovnávání zatížení nemůžou pracovat nad více virtuálními sítěmi ani v případě, že jsou propojeny.</span><span class="sxs-lookup"><span data-stu-id="b9625-132">A cloud service or a load balancing endpoint can't span across virtual networks, even if they are connected together.</span></span>

### <a name="can-i-used-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a><span data-ttu-id="b9625-133">Je možné použít typ sítě VPN PolicyBased pro připojení VNet-to-VNet nebo Multi-Site?</span><span class="sxs-lookup"><span data-stu-id="b9625-133">Can I used a PolicyBased VPN type for VNet-to-VNet or Multi-Site connections?</span></span>

<span data-ttu-id="b9625-134">Ne.</span><span class="sxs-lookup"><span data-stu-id="b9625-134">No.</span></span> <span data-ttu-id="b9625-135">Připojení VNet-to-VNet a Multi-Site vyžadují brány VPN Azure s typy sítě VPN RouteBased (dříve nazývané dynamické směrování).</span><span class="sxs-lookup"><span data-stu-id="b9625-135">VNet-to-VNet and Multi-Site connections require Azure VPN gateways with RouteBased (previously called Dynamic Routing) VPN types.</span></span>

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a><span data-ttu-id="b9625-136">Je možné připojit virtuální síť typu RouteBased k jiné virtuální síti typu PolicyBased?</span><span class="sxs-lookup"><span data-stu-id="b9625-136">Can I connect a VNet with a RouteBased VPN Type to another VNet with a PolicyBased VPN type?</span></span>

<span data-ttu-id="b9625-137">Ne, obě virtuální sítě MUSÍ používat sítě VPN založené na směrování (dříve nazývané dynamické směrování).</span><span class="sxs-lookup"><span data-stu-id="b9625-137">No, both virtual networks MUST be using route-based (previously called Dynamic Routing) VPNs.</span></span>

### <a name="do-vpn-tunnels-share-bandwidth"></a><span data-ttu-id="b9625-138">Sdílejí tunely VPN šířku pásma?</span><span class="sxs-lookup"><span data-stu-id="b9625-138">Do VPN tunnels share bandwidth?</span></span>

<span data-ttu-id="b9625-139">Ano.</span><span class="sxs-lookup"><span data-stu-id="b9625-139">Yes.</span></span> <span data-ttu-id="b9625-140">Všechna tunelová propojení sítí VPN v rámci virtuální sítě sdílejí v bráně VPN Azure šířku pásma, která je k dispozici, a stejnou smlouvu SLA pro dostupnost brány VPN v Azure.</span><span class="sxs-lookup"><span data-stu-id="b9625-140">All VPN tunnels of the virtual network share the available bandwidth on the Azure VPN gateway and the same VPN gateway uptime SLA in Azure.</span></span>

### <a name="are-redundant-tunnels-supported"></a><span data-ttu-id="b9625-141">Jsou podporovány redundantní tunely?</span><span class="sxs-lookup"><span data-stu-id="b9625-141">Are redundant tunnels supported?</span></span>

<span data-ttu-id="b9625-142">Redundantní tunely mezi párem virtuálních sítí jsou podporovány, pokud je jedna brána virtuální sítě nakonfigurována jako aktivní-aktivní.</span><span class="sxs-lookup"><span data-stu-id="b9625-142">Redundant tunnels between a pair of virtual networks are supported when one virtual network gateway is configured as active-active.</span></span>

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a><span data-ttu-id="b9625-143">Můžou se překrývat adresní prostory pro konfigurace VNet-to-VNet?</span><span class="sxs-lookup"><span data-stu-id="b9625-143">Can I have overlapping address spaces for VNet-to-VNet configurations?</span></span>

<span data-ttu-id="b9625-144">Ne.</span><span class="sxs-lookup"><span data-stu-id="b9625-144">No.</span></span> <span data-ttu-id="b9625-145">Není možné, aby se rozsahy IP adres překrývaly.</span><span class="sxs-lookup"><span data-stu-id="b9625-145">You can't have overlapping IP address ranges.</span></span>

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a><span data-ttu-id="b9625-146">Můžou se překrývat adresní prostory mezi propojenými virtuálními sítěmi a místními servery?</span><span class="sxs-lookup"><span data-stu-id="b9625-146">Can there be overlapping address spaces among connected virtual networks and on-premises local sites?</span></span>

<span data-ttu-id="b9625-147">Ne.</span><span class="sxs-lookup"><span data-stu-id="b9625-147">No.</span></span> <span data-ttu-id="b9625-148">Není možné, aby se rozsahy IP adres překrývaly.</span><span class="sxs-lookup"><span data-stu-id="b9625-148">You can't have overlapping IP address ranges.</span></span>



