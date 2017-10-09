<span data-ttu-id="0a88f-101">Je nutné vytvořit virtuální síť a podsíť brány nejprve, před prací na hello následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="0a88f-101">You must create a VNet and a gateway subnet first, before working on hello following tasks.</span></span> <span data-ttu-id="0a88f-102">Najdete v článku hello [konfigurace virtuální sítě pomocí portálu classic hello](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="0a88f-102">See hello article [Configure a Virtual Network using hello classic portal](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) for more information.</span></span>   

## <a name="add-a-gateway"></a><span data-ttu-id="0a88f-103">Přidat bránu</span><span class="sxs-lookup"><span data-stu-id="0a88f-103">Add a gateway</span></span>
<span data-ttu-id="0a88f-104">Použijte příkaz hello níže toocreate bránu.</span><span class="sxs-lookup"><span data-stu-id="0a88f-104">Use hello command below toocreate a gateway.</span></span> <span data-ttu-id="0a88f-105">Být jisti toosubstitute žádné hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="0a88f-105">Be sure toosubstitute any values for your own.</span></span>

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-hello-gateway-was-created"></a><span data-ttu-id="0a88f-106">Ověřte, zda byl vytvořen hello brány</span><span class="sxs-lookup"><span data-stu-id="0a88f-106">Verify hello gateway was created</span></span>
<span data-ttu-id="0a88f-107">Byla vytvořena pomocí příkazu hello níže tooverify, který hello brány.</span><span class="sxs-lookup"><span data-stu-id="0a88f-107">Use hello command below tooverify that hello gateway has been created.</span></span> <span data-ttu-id="0a88f-108">Tento příkaz načte také hello brány ID, které potřebujete pro jiné operace.</span><span class="sxs-lookup"><span data-stu-id="0a88f-108">This command also retrieves hello gateway ID, which you need for other operations.</span></span>

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a><span data-ttu-id="0a88f-109">Změnit velikost brány</span><span class="sxs-lookup"><span data-stu-id="0a88f-109">Resize a gateway</span></span>
<span data-ttu-id="0a88f-110">Existuje řada [SKU brány](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="0a88f-110">There are a number of [Gateway SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span></span> <span data-ttu-id="0a88f-111">Můžete použít následující příkaz toochange hello skladová položka brány kdykoli hello.</span><span class="sxs-lookup"><span data-stu-id="0a88f-111">You can use hello following command toochange hello Gateway SKU at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0a88f-112">Tento příkaz nefunguje pro UltraPerformance bránu.</span><span class="sxs-lookup"><span data-stu-id="0a88f-112">This command doesn't work for UltraPerformance gateway.</span></span> <span data-ttu-id="0a88f-113">toochange vaší brány tooan UltraPerformance bránu, nejprve odeberte hello existující bránu ExpressRoute a poté vytvořit novou bránu UltraPerformance.</span><span class="sxs-lookup"><span data-stu-id="0a88f-113">toochange your gateway tooan UltraPerformance gateway, first remove hello existing ExpressRoute gateway, and then create a new UltraPerformance gateway.</span></span> <span data-ttu-id="0a88f-114">toodowngrade bránu z bránu UltraPerformance nejprve odeberte hello UltraPerformance brány a poté vytvořit novou bránu.</span><span class="sxs-lookup"><span data-stu-id="0a88f-114">toodowngrade your gateway from an UltraPerformance gateway, first remove hello UltraPerformance gateway, and then create a new gateway.</span></span> 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a><span data-ttu-id="0a88f-115">Odebrat bránu</span><span class="sxs-lookup"><span data-stu-id="0a88f-115">Remove a gateway</span></span>
<span data-ttu-id="0a88f-116">Použijte příkaz hello níže tooremove brány</span><span class="sxs-lookup"><span data-stu-id="0a88f-116">Use hello command below tooremove a gateway</span></span>

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>