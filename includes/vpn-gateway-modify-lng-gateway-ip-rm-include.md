### <span data-ttu-id="33207-101"><a name="gwipnoconnection"></a>Brána místní sítě hello toomodify GatewayIpAddress - žádné připojení brány</span><span class="sxs-lookup"><span data-stu-id="33207-101"><a name="gwipnoconnection"></a> toomodify hello local network gateway 'GatewayIpAddress' - no gateway connection</span></span>

<span data-ttu-id="33207-102">Pokud hello zařízení VPN, které chcete tooconnect toohas změnit jeho veřejnou IP adresu, musíte toomodify hello místní síťové brány tooreflect, který změnit.</span><span class="sxs-lookup"><span data-stu-id="33207-102">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="33207-103">Použijte hello příklad toomodify bránu místní sítě, která nemá připojení brány.</span><span class="sxs-lookup"><span data-stu-id="33207-103">Use hello example toomodify a local network gateway that does not have a gateway connection.</span></span>

<span data-ttu-id="33207-104">Při úpravě tuto hodnotu, můžete také upravit předpony adres hello v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="33207-104">When modifying this value, you can also modify hello address prefixes at hello same time.</span></span> <span data-ttu-id="33207-105">Zda toouse hello stávající název brány místní sítě se v pořadí toooverwrite hello aktuální nastavení.</span><span class="sxs-lookup"><span data-stu-id="33207-105">Be sure toouse hello existing name of your local network gateway in order toooverwrite hello current settings.</span></span> <span data-ttu-id="33207-106">Pokud použijete jiný název, můžete vytvořit novou bránu místní sítě, místo přepsání hello existující.</span><span class="sxs-lookup"><span data-stu-id="33207-106">If you use a different name, you create a new local network gateway, instead of overwriting hello existing one.</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <span data-ttu-id="33207-107"><a name="gwipwithconnection"></a>Brána místní sítě hello toomodify GatewayIpAddress - existující připojení brány</span><span class="sxs-lookup"><span data-stu-id="33207-107"><a name="gwipwithconnection"></a>toomodify hello local network gateway 'GatewayIpAddress' - existing gateway connection</span></span>

<span data-ttu-id="33207-108">Pokud hello zařízení VPN, které chcete tooconnect toohas změnit jeho veřejnou IP adresu, musíte toomodify hello místní síťové brány tooreflect, který změnit.</span><span class="sxs-lookup"><span data-stu-id="33207-108">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="33207-109">Pokud připojení brány již existuje, je nutné nejprve tooremove hello připojení.</span><span class="sxs-lookup"><span data-stu-id="33207-109">If a gateway connection already exists, you first need tooremove hello connection.</span></span> <span data-ttu-id="33207-110">Po odebrání hello připojení můžete upravit IP adresu brány hello a znovu vytvořte nové připojení.</span><span class="sxs-lookup"><span data-stu-id="33207-110">After hello connection is removed, you can modify hello gateway IP address and recreate a new connection.</span></span> <span data-ttu-id="33207-111">Můžete také upravit předpony adres hello v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="33207-111">You can also modify hello address prefixes at hello same time.</span></span> <span data-ttu-id="33207-112">Způsobí to určitý výpadek připojení VPN.</span><span class="sxs-lookup"><span data-stu-id="33207-112">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="33207-113">Při úpravě IP adresu brány hello, nepotřebujete toodelete hello VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="33207-113">When modifying hello gateway IP address, you don't need toodelete hello VPN gateway.</span></span> <span data-ttu-id="33207-114">Potřebujete jenom tooremove hello připojení.</span><span class="sxs-lookup"><span data-stu-id="33207-114">You only need tooremove hello connection.</span></span>
 

1. <span data-ttu-id="33207-115">Odeberte připojení hello.</span><span class="sxs-lookup"><span data-stu-id="33207-115">Remove hello connection.</span></span> <span data-ttu-id="33207-116">Název hello připojení můžete najít pomocí rutiny hello 'Get-AzureRmVirtualNetworkGatewayConnection'.</span><span class="sxs-lookup"><span data-stu-id="33207-116">You can find hello name of your connection by using hello 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet.</span></span>

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. <span data-ttu-id="33207-117">Změníte hodnotu 'GatewayIpAddress' hello.</span><span class="sxs-lookup"><span data-stu-id="33207-117">Modify hello 'GatewayIpAddress' value.</span></span> <span data-ttu-id="33207-118">Můžete také upravit předpony adres hello v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="33207-118">You can also modify hello address prefixes at hello same time.</span></span> <span data-ttu-id="33207-119">Být jisti toouse hello stávající název vaší místní brány toooverwrite hello aktuální nastavení sítě.</span><span class="sxs-lookup"><span data-stu-id="33207-119">Be sure toouse hello existing name of your local network gateway toooverwrite hello current settings.</span></span> <span data-ttu-id="33207-120">Pokud to neuděláte, můžete vytvořit novou bránu místní sítě, místo přepsání hello existující.</span><span class="sxs-lookup"><span data-stu-id="33207-120">If you don't, you create a new local network gateway, instead of overwriting hello existing one.</span></span>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. <span data-ttu-id="33207-121">Vytvořte připojení hello.</span><span class="sxs-lookup"><span data-stu-id="33207-121">Create hello connection.</span></span> <span data-ttu-id="33207-122">V tomto příkladu konfigurujeme typ připojení IPsec.</span><span class="sxs-lookup"><span data-stu-id="33207-122">In this example, we configure an IPsec connection type.</span></span> <span data-ttu-id="33207-123">Při opětovném vytvoření připojení, použijte typ hello připojení, který je zadán pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="33207-123">When you recreate your connection, use hello connection type that is specified for your configuration.</span></span> <span data-ttu-id="33207-124">Typy další připojení, najdete v části hello [rutiny prostředí PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="33207-124">For additional connection types, see hello [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) page.</span></span>  <span data-ttu-id="33207-125">Název VirtualNetworkGateway hello tooobtain, můžete spustit rutinu "Get-AzureRmVirtualNetworkGateway" hello.</span><span class="sxs-lookup"><span data-stu-id="33207-125">tooobtain hello VirtualNetworkGateway name, you can run hello 'Get-AzureRmVirtualNetworkGateway' cmdlet.</span></span>
   
    <span data-ttu-id="33207-126">Nastavení proměnných hello.</span><span class="sxs-lookup"><span data-stu-id="33207-126">Set hello variables.</span></span>

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    <span data-ttu-id="33207-127">Vytvořte připojení hello.</span><span class="sxs-lookup"><span data-stu-id="33207-127">Create hello connection.</span></span>

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```