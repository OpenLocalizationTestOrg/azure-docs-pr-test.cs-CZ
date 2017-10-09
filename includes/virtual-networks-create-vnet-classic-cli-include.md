## <a name="how-toocreate-a-classic-vnet-using-azure-cli"></a><span data-ttu-id="bece9-101">Jak toocreate klasické virtuální sítě pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="bece9-101">How toocreate a classic VNet using Azure CLI</span></span>
<span data-ttu-id="bece9-102">Můžete použít rozhraní příkazového řádku Azure toomanage hello vašich prostředků Azure z příkazového řádku hello z libovolného počítače se systémem Windows, Linux nebo OSX.</span><span class="sxs-lookup"><span data-stu-id="bece9-102">You can use hello Azure CLI toomanage your Azure resources from hello command prompt from any computer running Windows, Linux, or OSX.</span></span> <span data-ttu-id="bece9-103">toocreate virtuální sítě pomocí hello rozhraní příkazového řádku Azure, postupujte podle kroků hello níže.</span><span class="sxs-lookup"><span data-stu-id="bece9-103">toocreate a VNet by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="bece9-104">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../articles/cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="bece9-104">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../articles/cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="bece9-105">Spustit hello **vytvořit virtuální síť azure sítě** příkaz toocreate virtuální síť a podsíť, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="bece9-105">Run hello **azure network vnet create** command toocreate a VNet and a subnet, as shown below.</span></span> <span data-ttu-id="bece9-106">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="bece9-106">hello list shown after hello output explains hello parameters used.</span></span>
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    <span data-ttu-id="bece9-107">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="bece9-107">Expected output:</span></span>
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * <span data-ttu-id="bece9-108">**--vnet**.</span><span class="sxs-lookup"><span data-stu-id="bece9-108">**--vnet**.</span></span> <span data-ttu-id="bece9-109">Název toobe hello virtuální síť vytvořili.</span><span class="sxs-lookup"><span data-stu-id="bece9-109">Name of hello VNet toobe created.</span></span> <span data-ttu-id="bece9-110">V našem scénáři je to *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="bece9-110">For our scenario, *TestVNet*</span></span>
   * <span data-ttu-id="bece9-111">**-e (nebo--adresní prostor)**.</span><span class="sxs-lookup"><span data-stu-id="bece9-111">**-e (or --address-space)**.</span></span> <span data-ttu-id="bece9-112">Adresní prostor sítě VNet.</span><span class="sxs-lookup"><span data-stu-id="bece9-112">VNet address space.</span></span> <span data-ttu-id="bece9-113">Pro náš scénář *192.168.0.0*</span><span class="sxs-lookup"><span data-stu-id="bece9-113">For our scenario, *192.168.0.0*</span></span>
   * <span data-ttu-id="bece9-114">**-i (nebo - cidr)**.</span><span class="sxs-lookup"><span data-stu-id="bece9-114">**-i (or -cidr)**.</span></span> <span data-ttu-id="bece9-115">Maska sítě ve formátu CIDR.</span><span class="sxs-lookup"><span data-stu-id="bece9-115">Network mask in CIDR format.</span></span> <span data-ttu-id="bece9-116">Pro náš scénář *16*.</span><span class="sxs-lookup"><span data-stu-id="bece9-116">For our scenario, *16*.</span></span>
   * <span data-ttu-id="bece9-117">**-n (nebo--název podsítě**).</span><span class="sxs-lookup"><span data-stu-id="bece9-117">**-n (or --subnet-name**).</span></span> <span data-ttu-id="bece9-118">Název první podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="bece9-118">Name of hello first subnet.</span></span> <span data-ttu-id="bece9-119">V našem scénáři je to *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="bece9-119">For our scenario, *FrontEnd*.</span></span>
   * <span data-ttu-id="bece9-120">**-p (nebo--IP adresu podsítě start)**.</span><span class="sxs-lookup"><span data-stu-id="bece9-120">**-p (or --subnet-start-ip)**.</span></span> <span data-ttu-id="bece9-121">Počáteční IP adresa pro podsíť nebo adresního prostoru podsítě.</span><span class="sxs-lookup"><span data-stu-id="bece9-121">Starting IP address for subnet, or subnet address space.</span></span> <span data-ttu-id="bece9-122">Pro náš scénář *192.168.1.0*.</span><span class="sxs-lookup"><span data-stu-id="bece9-122">For our scenario, *192.168.1.0*.</span></span>
   * <span data-ttu-id="bece9-123">**-r (nebo--podsítě cidr)**.</span><span class="sxs-lookup"><span data-stu-id="bece9-123">**-r (or --subnet-cidr)**.</span></span> <span data-ttu-id="bece9-124">Maska sítě ve formátu CIDR podsítě.</span><span class="sxs-lookup"><span data-stu-id="bece9-124">Network mask in CIDR format for subnet.</span></span> <span data-ttu-id="bece9-125">Pro náš scénář *24*.</span><span class="sxs-lookup"><span data-stu-id="bece9-125">For our scenario, *24*.</span></span>
   * <span data-ttu-id="bece9-126">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="bece9-126">**-l (or --location)**.</span></span> <span data-ttu-id="bece9-127">Oblast Azure, kde bude vytvořen hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="bece9-127">Azure region where hello VNet will be created.</span></span> <span data-ttu-id="bece9-128">Pro náš scénář *střed USA*.</span><span class="sxs-lookup"><span data-stu-id="bece9-128">For our scenario, *Central US*.</span></span>
3. <span data-ttu-id="bece9-129">Spustit hello **sítě azure vnet podsíť vytváření** příkaz toocreate podsíť, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="bece9-129">Run hello **azure network vnet subnet create** command toocreate a subnet as shown below.</span></span> <span data-ttu-id="bece9-130">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="bece9-130">hello list shown after hello output explains hello parameters used.</span></span>
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    <span data-ttu-id="bece9-131">Tady je hello očekávaný výstup výše hello příkazu:</span><span class="sxs-lookup"><span data-stu-id="bece9-131">Here is hello expected output for hello command above:</span></span>
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * <span data-ttu-id="bece9-132">**-t (nebo--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="bece9-132">**-t (or --vnet-name**.</span></span> <span data-ttu-id="bece9-133">Název hello kde hello se vytvoří podsíť virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="bece9-133">Name of hello VNet where hello subnet will be created.</span></span> <span data-ttu-id="bece9-134">V našem scénáři je to *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="bece9-134">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="bece9-135">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="bece9-135">**-n (or --name)**.</span></span> <span data-ttu-id="bece9-136">Název nové podsítě hello.</span><span class="sxs-lookup"><span data-stu-id="bece9-136">Name of hello new subnet.</span></span> <span data-ttu-id="bece9-137">Pro náš scénář *back-end*.</span><span class="sxs-lookup"><span data-stu-id="bece9-137">For our scenario, *BackEnd*.</span></span>
   * <span data-ttu-id="bece9-138">**-a (nebo --address-prefixes)**.</span><span class="sxs-lookup"><span data-stu-id="bece9-138">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="bece9-139">Blok CIDR podsítě.</span><span class="sxs-lookup"><span data-stu-id="bece9-139">Subnet CIDR block.</span></span> <span data-ttu-id="bece9-140">Čtyři našem scénáři *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="bece9-140">Four our scenario, *192.168.2.0/24*.</span></span>
4. <span data-ttu-id="bece9-141">Spustit hello **sítě azure vnet show** příkaz tooview hello vlastnosti hello nové sítě vnet, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="bece9-141">Run hello **azure network vnet show** command tooview hello properties of hello new vnet, as shown below.</span></span>
   
            azure network vnet show
   
    <span data-ttu-id="bece9-142">Tady je hello očekávaný výstup výše hello příkazu:</span><span class="sxs-lookup"><span data-stu-id="bece9-142">Here is hello expected output for hello command above:</span></span>
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up hello virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

