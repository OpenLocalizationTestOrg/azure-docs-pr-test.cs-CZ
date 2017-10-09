<span data-ttu-id="0c2e0-101">Hello kroky pro tuto úlohu použijte virtuální sítě na základě hello hodnot v hello následující referenční seznam konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-101">hello steps for this task use a VNet based on hello values in hello following configuration reference list.</span></span> <span data-ttu-id="0c2e0-102">Názvy a další nastavení jsou také popsány v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-102">Additional settings and names are also outlined in this list.</span></span> <span data-ttu-id="0c2e0-103">Tento seznam nepoužívá přímo v žádném z kroků hello, i když přidáme proměnné založené na hodnotách hello v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-103">We don't use this list directly in any of hello steps, although we do add variables based on hello values in this list.</span></span> <span data-ttu-id="0c2e0-104">Toouse hello seznamu můžete kopírovat jako odkaz, nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-104">You can copy hello list toouse as a reference, replacing hello values with your own.</span></span>

<span data-ttu-id="0c2e0-105">**Seznam odkazů konfigurace**</span><span class="sxs-lookup"><span data-stu-id="0c2e0-105">**Configuration reference list**</span></span>

* <span data-ttu-id="0c2e0-106">Název virtuální sítě = "TestVNet"</span><span class="sxs-lookup"><span data-stu-id="0c2e0-106">Virtual Network Name = "TestVNet"</span></span>
* <span data-ttu-id="0c2e0-107">Virtuální adresní prostor sítě = 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="0c2e0-107">Virtual Network address space = 192.168.0.0/16</span></span>
* <span data-ttu-id="0c2e0-108">Skupina prostředků = "TestRG"</span><span class="sxs-lookup"><span data-stu-id="0c2e0-108">Resource Group = "TestRG"</span></span>
* <span data-ttu-id="0c2e0-109">Subnet1 Name = "FrontEnd"</span><span class="sxs-lookup"><span data-stu-id="0c2e0-109">Subnet1 Name = "FrontEnd"</span></span> 
* <span data-ttu-id="0c2e0-110">Adresní prostor Subnet1 = "192.168.1.0/24"</span><span class="sxs-lookup"><span data-stu-id="0c2e0-110">Subnet1 address space = "192.168.1.0/24"</span></span>
* <span data-ttu-id="0c2e0-111">Název podsítě brány: "GatewaySubnet" název musí být vždy podsíť brány *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-111">Gateway Subnet name: "GatewaySubnet" You must always name a gateway subnet *GatewaySubnet*.</span></span>
* <span data-ttu-id="0c2e0-112">Adresního prostoru podsítě brány = "192.168.200.0/26"</span><span class="sxs-lookup"><span data-stu-id="0c2e0-112">Gateway Subnet address space = "192.168.200.0/26"</span></span>
* <span data-ttu-id="0c2e0-113">Oblast = "East US"</span><span class="sxs-lookup"><span data-stu-id="0c2e0-113">Region = "East US"</span></span>
* <span data-ttu-id="0c2e0-114">Název brány = "GW"</span><span class="sxs-lookup"><span data-stu-id="0c2e0-114">Gateway Name = "GW"</span></span>
* <span data-ttu-id="0c2e0-115">Název brány IP = "GWIP"</span><span class="sxs-lookup"><span data-stu-id="0c2e0-115">Gateway IP Name = "GWIP"</span></span>
* <span data-ttu-id="0c2e0-116">Konfigurace IP brány Name = "gwipconf"</span><span class="sxs-lookup"><span data-stu-id="0c2e0-116">Gateway IP configuration Name = "gwipconf"</span></span>
* <span data-ttu-id="0c2e0-117">Typ = "ExpressRoute" Tento typ je požadovaná konfigurace ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-117">Type = "ExpressRoute" This type is required for an ExpressRoute configuration.</span></span>
* <span data-ttu-id="0c2e0-118">Název veřejné IP adresy brány = "gwpip"</span><span class="sxs-lookup"><span data-stu-id="0c2e0-118">Gateway Public IP Name = "gwpip"</span></span>

## <a name="add-a-gateway"></a><span data-ttu-id="0c2e0-119">Přidat bránu</span><span class="sxs-lookup"><span data-stu-id="0c2e0-119">Add a gateway</span></span>
1. <span data-ttu-id="0c2e0-120">Připojte tooyour předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-120">Connect tooyour Azure Subscription.</span></span>

  ```powershell 
  Login-AzureRmAccount
  Get-AzureRmSubscription 
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. <span data-ttu-id="0c2e0-121">Deklarace proměnných pro toto cvičení.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-121">Declare your variables for this exercise.</span></span> <span data-ttu-id="0c2e0-122">Mít jistotu tooedit hello ukázka tooreflect hello nastavení, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-122">Be sure tooedit hello sample tooreflect hello settings that you want toouse.</span></span>

  ```powershell 
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "GW"
  $GWIPName = "GWIP"
  $GWIPconfName = "gwipconf"
  $VNetName = "TestVNet"
  ```
3. <span data-ttu-id="0c2e0-123">Uložit jako proměnnou hello objekt virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-123">Store hello virtual network object as a variable.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  ```
4. <span data-ttu-id="0c2e0-124">Přidáte tooyour podsíť brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-124">Add a gateway subnet tooyour Virtual Network.</span></span> <span data-ttu-id="0c2e0-125">Hello podsíť brány musí mít název "GatewaySubnet".</span><span class="sxs-lookup"><span data-stu-id="0c2e0-125">hello gateway subnet must be named "GatewaySubnet".</span></span> <span data-ttu-id="0c2e0-126">Měli byste vytvořit podsíť brány, který je/27 nebo větší (/ 26, / 25 atd.).</span><span class="sxs-lookup"><span data-stu-id="0c2e0-126">You should create a gateway subnet that is /27 or larger (/26, /25, etc.).</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
  ```
5. <span data-ttu-id="0c2e0-127">Nastavte konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-127">Set hello configuration.</span></span>

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. <span data-ttu-id="0c2e0-128">Uložit jako proměnnou a podsíť brány hello.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-128">Store hello gateway subnet as a variable.</span></span>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
  ```
7. <span data-ttu-id="0c2e0-129">Vyžádejte si veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-129">Request a public IP address.</span></span> <span data-ttu-id="0c2e0-130">Před vytvořením brány hello je požadována Hello IP adresa.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-130">hello IP address is requested before creating hello gateway.</span></span> <span data-ttu-id="0c2e0-131">Nelze zadat hello IP adresu, které chcete toouse; se přidělí dynamicky.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-131">You cannot specify hello IP address that you want toouse; it’s dynamically allocated.</span></span> <span data-ttu-id="0c2e0-132">Tuto IP adresu budete používat v další části Konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-132">You'll use this IP address in hello next configuration section.</span></span> <span data-ttu-id="0c2e0-133">Hello AllocationMethod musí být dynamické.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-133">hello AllocationMethod must be Dynamic.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  ```
8. <span data-ttu-id="0c2e0-134">Vytvoření hello konfigurace pro bránu.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-134">Create hello configuration for your gateway.</span></span> <span data-ttu-id="0c2e0-135">Hello konfigurace brány definuje podsíť hello a toouse hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-135">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="0c2e0-136">V tomto kroku určíte hello konfigurace, který se použije při vytváření brány hello.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-136">In this step, you are specifying hello configuration that will be used when you create hello gateway.</span></span> <span data-ttu-id="0c2e0-137">Tento krok není ve skutečnosti vytvoření objektu gateway hello.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-137">This step does not actually create hello gateway object.</span></span> <span data-ttu-id="0c2e0-138">Ukázka hello níže toocreate používejte vlastní konfiguraci brány.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-138">Use hello sample below toocreate your gateway configuration.</span></span>

  ```powershell
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```
9. <span data-ttu-id="0c2e0-139">Vytvoření brány hello.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-139">Create hello gateway.</span></span> <span data-ttu-id="0c2e0-140">V tomto kroku hello **- GatewayType** je obzvláště důležité.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-140">In this step, hello **-GatewayType** is especially important.</span></span> <span data-ttu-id="0c2e0-141">Je nutné použít hodnotu hello **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-141">You must use hello value **ExpressRoute**.</span></span> <span data-ttu-id="0c2e0-142">Po spuštění těchto rutin, může trvat hello brány 45 minut nebo další toocreate.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-142">After running these cmdlets, hello gateway can take 45 minutes or more toocreate.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
  ```

## <a name="verify-hello-gateway-was-created"></a><span data-ttu-id="0c2e0-143">Ověřte, zda byl vytvořen hello brány</span><span class="sxs-lookup"><span data-stu-id="0c2e0-143">Verify hello gateway was created</span></span>
<span data-ttu-id="0c2e0-144">Použijte následující příkazy, které vytvořil tooverify, který hello brány hello:</span><span class="sxs-lookup"><span data-stu-id="0c2e0-144">Use hello following commands tooverify that hello gateway has been created:</span></span>

```powershell
Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a><span data-ttu-id="0c2e0-145">Změnit velikost brány</span><span class="sxs-lookup"><span data-stu-id="0c2e0-145">Resize a gateway</span></span>
<span data-ttu-id="0c2e0-146">Existuje řada [SKU brány](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="0c2e0-146">There are a number of [Gateway SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span></span> <span data-ttu-id="0c2e0-147">Můžete použít následující příkaz toochange hello skladová položka brány kdykoli hello.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-147">You can use hello following command toochange hello Gateway SKU at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0c2e0-148">Tento příkaz nefunguje pro UltraPerformance bránu.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-148">This command doesn't work for UltraPerformance gateway.</span></span> <span data-ttu-id="0c2e0-149">toochange vaší brány tooan UltraPerformance bránu, nejprve odeberte hello existující bránu ExpressRoute a poté vytvořit novou bránu UltraPerformance.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-149">toochange your gateway tooan UltraPerformance gateway, first remove hello existing ExpressRoute gateway, and then create a new UltraPerformance gateway.</span></span> <span data-ttu-id="0c2e0-150">toodowngrade bránu z bránu UltraPerformance nejprve odeberte hello UltraPerformance brány a poté vytvořit novou bránu.</span><span class="sxs-lookup"><span data-stu-id="0c2e0-150">toodowngrade your gateway from an UltraPerformance gateway, first remove hello UltraPerformance gateway, and then create a new gateway.</span></span>
> 
> 

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a><span data-ttu-id="0c2e0-151">Odebrat bránu</span><span class="sxs-lookup"><span data-stu-id="0c2e0-151">Remove a gateway</span></span>
<span data-ttu-id="0c2e0-152">Použijte následující příkaz tooremove bránu hello:</span><span class="sxs-lookup"><span data-stu-id="0c2e0-152">Use hello following command tooremove a gateway:</span></span>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```