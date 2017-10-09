Je nutné vytvořit virtuální síť a podsíť brány nejprve, před prací na hello následující úlohy. Najdete v článku hello [konfigurace virtuální sítě pomocí portálu classic hello](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) Další informace.   

## <a name="add-a-gateway"></a>Přidat bránu
Použijte příkaz hello níže toocreate bránu. Být jisti toosubstitute žádné hodnoty vlastními.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-hello-gateway-was-created"></a>Ověřte, zda byl vytvořen hello brány
Byla vytvořena pomocí příkazu hello níže tooverify, který hello brány. Tento příkaz načte také hello brány ID, které potřebujete pro jiné operace.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Změnit velikost brány
Existuje řada [SKU brány](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Můžete použít následující příkaz toochange hello skladová položka brány kdykoli hello.

> [!IMPORTANT]
> Tento příkaz nefunguje pro UltraPerformance bránu. toochange vaší brány tooan UltraPerformance bránu, nejprve odeberte hello existující bránu ExpressRoute a poté vytvořit novou bránu UltraPerformance. toodowngrade bránu z bránu UltraPerformance nejprve odeberte hello UltraPerformance brány a poté vytvořit novou bránu. 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Odebrat bránu
Použijte příkaz hello níže tooremove brány

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>