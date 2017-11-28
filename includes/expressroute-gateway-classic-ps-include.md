<span data-ttu-id="d1312-101">Je nutné vytvořit virtuální síť a podsíť brány nejprve, před prací na následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="d1312-101">You must create a VNet and a gateway subnet first, before working on the following tasks.</span></span> <span data-ttu-id="d1312-102">Najdete v článku [konfigurace virtuální sítě pomocí portálu classic](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="d1312-102">See the article [Configure a Virtual Network using the classic portal](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) for more information.</span></span>   

## <a name="add-a-gateway"></a><span data-ttu-id="d1312-103">Přidat bránu</span><span class="sxs-lookup"><span data-stu-id="d1312-103">Add a gateway</span></span>
<span data-ttu-id="d1312-104">Použijte následující příkaz k vytvoření brány.</span><span class="sxs-lookup"><span data-stu-id="d1312-104">Use the command below to create a gateway.</span></span> <span data-ttu-id="d1312-105">Nezapomeňte nahradit všechny hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="d1312-105">Be sure to substitute any values for your own.</span></span>

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a><span data-ttu-id="d1312-106">Ověřte, že vytvoření brány</span><span class="sxs-lookup"><span data-stu-id="d1312-106">Verify the gateway was created</span></span>
<span data-ttu-id="d1312-107">Použijte následující příkaz k ověření, zda byl vytvořen brány.</span><span class="sxs-lookup"><span data-stu-id="d1312-107">Use the command below to verify that the gateway has been created.</span></span> <span data-ttu-id="d1312-108">Tento příkaz načte také ID brány, které potřebujete pro jiné operace.</span><span class="sxs-lookup"><span data-stu-id="d1312-108">This command also retrieves the gateway ID, which you need for other operations.</span></span>

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a><span data-ttu-id="d1312-109">Změnit velikost brány</span><span class="sxs-lookup"><span data-stu-id="d1312-109">Resize a gateway</span></span>
<span data-ttu-id="d1312-110">Existuje řada [SKU brány](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="d1312-110">There are a number of [Gateway SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span></span> <span data-ttu-id="d1312-111">Tento příkaz můžete kdykoli změnit skladová položka brány.</span><span class="sxs-lookup"><span data-stu-id="d1312-111">You can use the following command to change the Gateway SKU at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d1312-112">Tento příkaz nefunguje pro UltraPerformance bránu.</span><span class="sxs-lookup"><span data-stu-id="d1312-112">This command doesn't work for UltraPerformance gateway.</span></span> <span data-ttu-id="d1312-113">Chcete-li změnit bránu pro bránu UltraPerformance, nejprve odeberte existující bránu ExpressRoute a poté vytvořit novou bránu UltraPerformance.</span><span class="sxs-lookup"><span data-stu-id="d1312-113">To change your gateway to an UltraPerformance gateway, first remove the existing ExpressRoute gateway, and then create a new UltraPerformance gateway.</span></span> <span data-ttu-id="d1312-114">Přejít na starší verzi bránu z bránu UltraPerformance, nejprve odeberte UltraPerformance brány a poté vytvořit novou bránu.</span><span class="sxs-lookup"><span data-stu-id="d1312-114">To downgrade your gateway from an UltraPerformance gateway, first remove the UltraPerformance gateway, and then create a new gateway.</span></span> 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a><span data-ttu-id="d1312-115">Odebrat bránu</span><span class="sxs-lookup"><span data-stu-id="d1312-115">Remove a gateway</span></span>
<span data-ttu-id="d1312-116">Pomocí následujícího příkazu odeberte brány</span><span class="sxs-lookup"><span data-stu-id="d1312-116">Use the command below to remove a gateway</span></span>

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>