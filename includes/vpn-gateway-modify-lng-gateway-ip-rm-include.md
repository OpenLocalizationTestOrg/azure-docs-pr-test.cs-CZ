### <span data-ttu-id="e785a-101"><a name="gwipnoconnection"></a>Úprava IP adresy brány místní sítě (GatewayIpAddress) – žádné připojení brány</span><span class="sxs-lookup"><span data-stu-id="e785a-101"><a name="gwipnoconnection"></a> To modify the local network gateway 'GatewayIpAddress' - no gateway connection</span></span>

<span data-ttu-id="e785a-102">Pokud zařízení VPN, ke kterému se chcete připojit, změnilo svou veřejnou IP adresu, musíte upravit bránu místní sítě, aby odrážela tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="e785a-102">If the VPN device that you want to connect to has changed its public IP address, you need to modify the local network gateway to reflect that change.</span></span> <span data-ttu-id="e785a-103">Pomocí tohoto příkladu upravte bránu místní sítě, která nemá připojení brány.</span><span class="sxs-lookup"><span data-stu-id="e785a-103">Use the example to modify a local network gateway that does not have a gateway connection.</span></span>

<span data-ttu-id="e785a-104">Při upravování této hodnoty můžete také zároveň upravit předpony adres.</span><span class="sxs-lookup"><span data-stu-id="e785a-104">When modifying this value, you can also modify the address prefixes at the same time.</span></span> <span data-ttu-id="e785a-105">Ujistěte se, že k přepsání aktuálního nastavení používáte existující název brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="e785a-105">Be sure to use the existing name of your local network gateway in order to overwrite the current settings.</span></span> <span data-ttu-id="e785a-106">Pokud použijete jiný název, vytvoříte novou bránu místní sítě, místo abyste přepsali tu stávající.</span><span class="sxs-lookup"><span data-stu-id="e785a-106">If you use a different name, you create a new local network gateway, instead of overwriting the existing one.</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <span data-ttu-id="e785a-107"><a name="gwipwithconnection"></a>Úprava IP adresy brány místní sítě (GatewayIpAddress) – existující připojení brány</span><span class="sxs-lookup"><span data-stu-id="e785a-107"><a name="gwipwithconnection"></a>To modify the local network gateway 'GatewayIpAddress' - existing gateway connection</span></span>

<span data-ttu-id="e785a-108">Pokud zařízení VPN, ke kterému se chcete připojit, změnilo svou veřejnou IP adresu, musíte upravit bránu místní sítě, aby odrážela tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="e785a-108">If the VPN device that you want to connect to has changed its public IP address, you need to modify the local network gateway to reflect that change.</span></span> <span data-ttu-id="e785a-109">Pokud už nějaké připojení brány existuje, musíte ho nejdřív odebrat.</span><span class="sxs-lookup"><span data-stu-id="e785a-109">If a gateway connection already exists, you first need to remove the connection.</span></span> <span data-ttu-id="e785a-110">Po odebrání připojení můžete upravit IP adresu brány a vytvořit nové připojení.</span><span class="sxs-lookup"><span data-stu-id="e785a-110">After the connection is removed, you can modify the gateway IP address and recreate a new connection.</span></span> <span data-ttu-id="e785a-111">Můžete také zároveň upravit předpony adresy.</span><span class="sxs-lookup"><span data-stu-id="e785a-111">You can also modify the address prefixes at the same time.</span></span> <span data-ttu-id="e785a-112">Způsobí to určitý výpadek připojení VPN.</span><span class="sxs-lookup"><span data-stu-id="e785a-112">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="e785a-113">Při upravování IP adresy brány není potřeba odstraňovat bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="e785a-113">When modifying the gateway IP address, you don't need to delete the VPN gateway.</span></span> <span data-ttu-id="e785a-114">Stačí jenom odebrat připojení.</span><span class="sxs-lookup"><span data-stu-id="e785a-114">You only need to remove the connection.</span></span>
 

1. <span data-ttu-id="e785a-115">Odeberte připojení.</span><span class="sxs-lookup"><span data-stu-id="e785a-115">Remove the connection.</span></span> <span data-ttu-id="e785a-116">Název vašeho připojení můžete najít pomocí rutiny Get-AzureRmVirtualNetworkGatewayConnection.</span><span class="sxs-lookup"><span data-stu-id="e785a-116">You can find the name of your connection by using the 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet.</span></span>

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. <span data-ttu-id="e785a-117">Upravte hodnotu GatewayIpAddress.</span><span class="sxs-lookup"><span data-stu-id="e785a-117">Modify the 'GatewayIpAddress' value.</span></span> <span data-ttu-id="e785a-118">Můžete také zároveň upravit předpony adresy.</span><span class="sxs-lookup"><span data-stu-id="e785a-118">You can also modify the address prefixes at the same time.</span></span> <span data-ttu-id="e785a-119">Ujistěte se, že k přepsání aktuálního nastavení používáte existující název brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="e785a-119">Be sure to use the existing name of your local network gateway to overwrite the current settings.</span></span> <span data-ttu-id="e785a-120">Pokud to neuděláte, vytvoříte novou bránu místní sítě, místo abyste přepsali tu stávající.</span><span class="sxs-lookup"><span data-stu-id="e785a-120">If you don't, you create a new local network gateway, instead of overwriting the existing one.</span></span>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. <span data-ttu-id="e785a-121">Vytvořte připojení.</span><span class="sxs-lookup"><span data-stu-id="e785a-121">Create the connection.</span></span> <span data-ttu-id="e785a-122">V tomto příkladu konfigurujeme typ připojení IPsec.</span><span class="sxs-lookup"><span data-stu-id="e785a-122">In this example, we configure an IPsec connection type.</span></span> <span data-ttu-id="e785a-123">Až budete připojení znovu vytvářet, použijte typ připojení stanovený pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e785a-123">When you recreate your connection, use the connection type that is specified for your configuration.</span></span> <span data-ttu-id="e785a-124">Informace o dalších typech připojení najdete na stránce [Rutina prostředí PowerShell](https://msdn.microsoft.com/library/mt603611.aspx).</span><span class="sxs-lookup"><span data-stu-id="e785a-124">For additional connection types, see the [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) page.</span></span>  <span data-ttu-id="e785a-125">Pokud chcete získat název brány VirtualNetworkGateway, můžete spustit rutinu Get-AzureRmVirtualNetworkGateway.</span><span class="sxs-lookup"><span data-stu-id="e785a-125">To obtain the VirtualNetworkGateway name, you can run the 'Get-AzureRmVirtualNetworkGateway' cmdlet.</span></span>
   
    <span data-ttu-id="e785a-126">Nastavte proměnné.</span><span class="sxs-lookup"><span data-stu-id="e785a-126">Set the variables.</span></span>

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    <span data-ttu-id="e785a-127">Vytvořte připojení.</span><span class="sxs-lookup"><span data-stu-id="e785a-127">Create the connection.</span></span>

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```