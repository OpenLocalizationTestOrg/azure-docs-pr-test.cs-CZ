### <span data-ttu-id="2fec5-101"><a name="noconnection"></a>toomodify místní síťové brány IP předpon adres – žádné připojení brány</span><span class="sxs-lookup"><span data-stu-id="2fec5-101"><a name="noconnection"></a>toomodify local network gateway IP address prefixes - no gateway connection</span></span>

<span data-ttu-id="2fec5-102">tooadd Další předpony adresy:</span><span class="sxs-lookup"><span data-stu-id="2fec5-102">tooadd additional address prefixes:</span></span>

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
```

<span data-ttu-id="2fec5-103">předpony adres tooremove:</span><span class="sxs-lookup"><span data-stu-id="2fec5-103">tooremove address prefixes:</span></span><br>
<span data-ttu-id="2fec5-104">Vynechte předpony hello, které už nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="2fec5-104">Leave out hello prefixes that you no longer need.</span></span> <span data-ttu-id="2fec5-105">V tomto příkladu jsme už nepotřebujeme předponu 20.0.0.0/24 (z hello předchozího příkladu), takže jsme aktualizovat bránu místní sítě hello, s výjimkou tuto předponu.</span><span class="sxs-lookup"><span data-stu-id="2fec5-105">In this example, we no longer need prefix 20.0.0.0/24 (from hello previous example), so we update hello local network gateway, excluding that prefix.</span></span>

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
```

### <span data-ttu-id="2fec5-106"><a name="withconnection"></a>toomodify předpon brány místní sítě IP adresy - existující připojení brány</span><span class="sxs-lookup"><span data-stu-id="2fec5-106"><a name="withconnection"></a>toomodify local network gateway IP address prefixes - existing gateway connection</span></span>

<span data-ttu-id="2fec5-107">Chcete-li mít připojení brány a tooadd nebo odebrat předpony IP adres hello obsažené v bránu místní sítě, je nutné toodo hello následující kroky v pořadí.</span><span class="sxs-lookup"><span data-stu-id="2fec5-107">If you have a gateway connection and want tooadd or remove hello IP address prefixes contained in your local network gateway, you need toodo hello following steps, in order.</span></span> <span data-ttu-id="2fec5-108">Způsobí to určitý výpadek připojení VPN.</span><span class="sxs-lookup"><span data-stu-id="2fec5-108">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="2fec5-109">Úpravy předpon adres IP, nepotřebujete toodelete hello VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="2fec5-109">When modifying IP address prefixes, you don't need toodelete hello VPN gateway.</span></span> <span data-ttu-id="2fec5-110">Potřebujete jenom tooremove hello připojení.</span><span class="sxs-lookup"><span data-stu-id="2fec5-110">You only need tooremove hello connection.</span></span>


1. <span data-ttu-id="2fec5-111">Odeberte připojení hello.</span><span class="sxs-lookup"><span data-stu-id="2fec5-111">Remove hello connection.</span></span>

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. <span data-ttu-id="2fec5-112">Úprava předpon adres hello pro bránu místní sítě.</span><span class="sxs-lookup"><span data-stu-id="2fec5-112">Modify hello address prefixes for your local network gateway.</span></span>
   
  <span data-ttu-id="2fec5-113">Hello proměnnou pro hello LocalNetworkGateway nastavit.</span><span class="sxs-lookup"><span data-stu-id="2fec5-113">Set hello variable for hello LocalNetworkGateway.</span></span>

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  <span data-ttu-id="2fec5-114">Úprava předpon hello.</span><span class="sxs-lookup"><span data-stu-id="2fec5-114">Modify hello prefixes.</span></span>
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. <span data-ttu-id="2fec5-115">Vytvořte připojení hello.</span><span class="sxs-lookup"><span data-stu-id="2fec5-115">Create hello connection.</span></span> <span data-ttu-id="2fec5-116">V tomto příkladu konfigurujeme typ připojení IPsec.</span><span class="sxs-lookup"><span data-stu-id="2fec5-116">In this example, we configure an IPsec connection type.</span></span> <span data-ttu-id="2fec5-117">Při opětovném vytvoření připojení, použijte typ hello připojení, který je zadán pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2fec5-117">When you recreate your connection, use hello connection type that is specified for your configuration.</span></span> <span data-ttu-id="2fec5-118">Typy další připojení, najdete v části hello [rutiny prostředí PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="2fec5-118">For additional connection types, see hello [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) page.</span></span>
   
  <span data-ttu-id="2fec5-119">Hello proměnnou pro hello VirtualNetworkGateway nastavit.</span><span class="sxs-lookup"><span data-stu-id="2fec5-119">Set hello variable for hello VirtualNetworkGateway.</span></span>

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  <span data-ttu-id="2fec5-120">Vytvořte připojení hello.</span><span class="sxs-lookup"><span data-stu-id="2fec5-120">Create hello connection.</span></span> <span data-ttu-id="2fec5-121">Tento příklad používá proměnnou hello $local nastavený v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="2fec5-121">This example uses hello variable $local that you set in step 2.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```