### <span data-ttu-id="ada94-101"><a name="noconnection"></a>Úprava předpon IP adres místní síťové brány – žádné připojení brány</span><span class="sxs-lookup"><span data-stu-id="ada94-101"><a name="noconnection"></a>To modify local network gateway IP address prefixes - no gateway connection</span></span>

<span data-ttu-id="ada94-102">Přidání dalších předpon adres:</span><span class="sxs-lookup"><span data-stu-id="ada94-102">To add additional address prefixes:</span></span>

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
```

<span data-ttu-id="ada94-103">Odebrání předpon adres:</span><span class="sxs-lookup"><span data-stu-id="ada94-103">To remove address prefixes:</span></span><br>
<span data-ttu-id="ada94-104">Vynechte předpony, které už nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="ada94-104">Leave out the prefixes that you no longer need.</span></span> <span data-ttu-id="ada94-105">V tomto příkladu už nepotřebujeme předponu 20.0.0.0/24 (z předchozího příkladu), takže bránu místní sítě aktualizujeme a tuto předponu vyloučíme.</span><span class="sxs-lookup"><span data-stu-id="ada94-105">In this example, we no longer need prefix 20.0.0.0/24 (from the previous example), so we update the local network gateway, excluding that prefix.</span></span>

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
```

### <span data-ttu-id="ada94-106"><a name="withconnection"></a>Úprava předpon IP adres místní síťové brány – existující připojení brány</span><span class="sxs-lookup"><span data-stu-id="ada94-106"><a name="withconnection"></a>To modify local network gateway IP address prefixes - existing gateway connection</span></span>

<span data-ttu-id="ada94-107">Pokud máte připojení k bráně a chcete přidat nebo odebrat předpony IP adres obsažené v bráně místní sítě, musíte v uvedeném pořadí provést následující kroky.</span><span class="sxs-lookup"><span data-stu-id="ada94-107">If you have a gateway connection and want to add or remove the IP address prefixes contained in your local network gateway, you need to do the following steps, in order.</span></span> <span data-ttu-id="ada94-108">Způsobí to určitý výpadek připojení VPN.</span><span class="sxs-lookup"><span data-stu-id="ada94-108">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="ada94-109">Při upravování předpon IP adres není potřeba odstraňovat bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="ada94-109">When modifying IP address prefixes, you don't need to delete the VPN gateway.</span></span> <span data-ttu-id="ada94-110">Stačí jenom odebrat připojení.</span><span class="sxs-lookup"><span data-stu-id="ada94-110">You only need to remove the connection.</span></span>


1. <span data-ttu-id="ada94-111">Odeberte připojení.</span><span class="sxs-lookup"><span data-stu-id="ada94-111">Remove the connection.</span></span>

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. <span data-ttu-id="ada94-112">Upravte předpony IP adresy pro bránu místní sítě.</span><span class="sxs-lookup"><span data-stu-id="ada94-112">Modify the address prefixes for your local network gateway.</span></span>
   
  <span data-ttu-id="ada94-113">Nastavte proměnnou pro LocalNetworkGateway.</span><span class="sxs-lookup"><span data-stu-id="ada94-113">Set the variable for the LocalNetworkGateway.</span></span>

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  <span data-ttu-id="ada94-114">Upravte předpony.</span><span class="sxs-lookup"><span data-stu-id="ada94-114">Modify the prefixes.</span></span>
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. <span data-ttu-id="ada94-115">Vytvořte připojení.</span><span class="sxs-lookup"><span data-stu-id="ada94-115">Create the connection.</span></span> <span data-ttu-id="ada94-116">V tomto příkladu konfigurujeme typ připojení IPsec.</span><span class="sxs-lookup"><span data-stu-id="ada94-116">In this example, we configure an IPsec connection type.</span></span> <span data-ttu-id="ada94-117">Až budete připojení znovu vytvářet, použijte typ připojení stanovený pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ada94-117">When you recreate your connection, use the connection type that is specified for your configuration.</span></span> <span data-ttu-id="ada94-118">Informace o dalších typech připojení najdete na stránce [Rutina prostředí PowerShell](https://msdn.microsoft.com/library/mt603611.aspx).</span><span class="sxs-lookup"><span data-stu-id="ada94-118">For additional connection types, see the [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) page.</span></span>
   
  <span data-ttu-id="ada94-119">Nastavte proměnnou pro VirtualNetworkGateway.</span><span class="sxs-lookup"><span data-stu-id="ada94-119">Set the variable for the VirtualNetworkGateway.</span></span>

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  <span data-ttu-id="ada94-120">Vytvořte připojení.</span><span class="sxs-lookup"><span data-stu-id="ada94-120">Create the connection.</span></span> <span data-ttu-id="ada94-121">Tento příklad používá proměnnou $local, kterou jste nastavili v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="ada94-121">This example uses the variable $local that you set in step 2.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```