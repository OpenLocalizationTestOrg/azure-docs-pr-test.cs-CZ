1. <span data-ttu-id="defa0-101">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="defa0-101">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="defa0-102">Spuštění v levé horní části hello, klikněte na tlačítko **nový > výpočetní > Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="defa0-102">Starting in hello upper left, click **New > Compute > Windows Server 2016 Datacenter**.</span></span>

    ![Přejděte Image virtuálního počítače Azure toohello hello portálu](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. <span data-ttu-id="defa0-104">Na Windows Server 2016 Datacenter hello vyberte model nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="defa0-104">On hello Windows Server 2016 Datacenter, select hello Classic deployment model.</span></span> <span data-ttu-id="defa0-105">Klikněte na Vytvořit.</span><span class="sxs-lookup"><span data-stu-id="defa0-105">Click Create.</span></span>

    ![Snímek obrazovky zobrazující Image virtuálního počítače Azure hello v hello portálu k dispozici](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a><span data-ttu-id="defa0-107">1. Okno Základy</span><span class="sxs-lookup"><span data-stu-id="defa0-107">1. Basics blade</span></span>

<span data-ttu-id="defa0-108">okno základy Hello požadavků pro správu informace o virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="defa0-108">hello Basics blade requests administrative information for hello virtual machine.</span></span>

1. <span data-ttu-id="defa0-109">Zadejte **název** hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="defa0-109">Enter a **Name** for hello virtual machine.</span></span> <span data-ttu-id="defa0-110">V příkladu hello _HeroVM_ je název hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="defa0-110">In hello example, _HeroVM_ is hello name of hello virtual machine.</span></span> <span data-ttu-id="defa0-111">Hello název musí mít 1 až 15 znaků a nesmí obsahovat speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="defa0-111">hello name must be 1-15 characters long and it cannot contain special characters.</span></span>

2. <span data-ttu-id="defa0-112">Zadejte **uživatelské jméno** a silné **heslo** , které jsou používané toocreate místní účet hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="defa0-112">Enter a **User name** and a strong **Password** that are used toocreate a local account on hello VM.</span></span> <span data-ttu-id="defa0-113">Hello místní účet se používá toosign v tooand spravovat hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="defa0-113">hello local account is used toosign in tooand manage hello VM.</span></span> <span data-ttu-id="defa0-114">V příkladu hello _azureuser_ je hello uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="defa0-114">In hello example, _azureuser_ is hello user name.</span></span>

 <span data-ttu-id="defa0-115">Hello heslo musí mít 8-123 znaků a musí splňovat tři mimo hello čtyři následující požadavky na složitost: jedno malé písmeno, jedno velké písmeno, jedna číslice a jeden speciální znak.</span><span class="sxs-lookup"><span data-stu-id="defa0-115">hello password must be 8-123 characters long and meet three out of hello four following complexity requirements: one lower case character, one upper case character, one number, and one special character.</span></span> <span data-ttu-id="defa0-116">Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](../articles/virtual-machines/windows/faq.md).</span><span class="sxs-lookup"><span data-stu-id="defa0-116">See more about [username and password requirements](../articles/virtual-machines/windows/faq.md).</span></span>

3. <span data-ttu-id="defa0-117">Hello **předplatné** je volitelný.</span><span class="sxs-lookup"><span data-stu-id="defa0-117">hello **Subscription** is optional.</span></span> <span data-ttu-id="defa0-118">Jedním z běžných nastavení jsou průběžné platby.</span><span class="sxs-lookup"><span data-stu-id="defa0-118">One common setting is "Pay-As-You-Go".</span></span>

4. <span data-ttu-id="defa0-119">Vyberte existující **skupiny prostředků** nebo název typu hello nové.</span><span class="sxs-lookup"><span data-stu-id="defa0-119">Select an existing **Resource group** or type hello name for a new one.</span></span> <span data-ttu-id="defa0-120">V příkladu hello _HeroVMRG_ je hello název skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="defa0-120">In hello example, _HeroVMRG_ is hello name of hello resource group.</span></span>

5. <span data-ttu-id="defa0-121">Vyberte datové centrum Azure **umístění** místo toorun hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="defa0-121">Select an Azure datacenter **Location** where you want hello VM toorun.</span></span> <span data-ttu-id="defa0-122">V příkladu hello **východní USA** hello umístění.</span><span class="sxs-lookup"><span data-stu-id="defa0-122">In hello example, **East US** is hello location.</span></span>

6. <span data-ttu-id="defa0-123">Až budete hotovi, klikněte na tlačítko **Další** toocontinue toohello další okno.</span><span class="sxs-lookup"><span data-stu-id="defa0-123">When you are done, click **Next** toocontinue toohello next blade.</span></span>

    ![Snímek obrazovky, který zobrazuje nastavení hello na hello okno základy pro konfiguraci virtuálního počítače Azure](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a><span data-ttu-id="defa0-125">2. Okno Velikost</span><span class="sxs-lookup"><span data-stu-id="defa0-125">2. Size blade</span></span>

<span data-ttu-id="defa0-126">Velikost okna Hello identifikuje podrobnosti konfigurace hello hello virtuálních počítačů a uvádí různé volby, které zahrnují operačního systému, počet procesorů, typ disku úložiště a odhadované měsíční náklady na využití.</span><span class="sxs-lookup"><span data-stu-id="defa0-126">hello Size blade identifies hello configuration details of hello VM, and lists various choices that include OS, number of processors, disk storage type, and estimated monthly usage costs.</span></span>  

<span data-ttu-id="defa0-127">Zvolte velikost virtuálního počítače a pak klikněte na tlačítko **vyberte** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="defa0-127">Choose a VM size, and then click **Select** toocontinue.</span></span> <span data-ttu-id="defa0-128">V tomto příkladu _DS1_\__V2 standardní_ je hello velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="defa0-128">In this example, _DS1_\__V2 Standard_ is hello VM size.</span></span>

  ![Snímek obrazovky okna velikost hello, který ukazuje hello velikosti virtuálního počítače Azure, které můžete vybrat](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a><span data-ttu-id="defa0-130">3. Okno Nastavení</span><span class="sxs-lookup"><span data-stu-id="defa0-130">3. Settings blade</span></span>

<span data-ttu-id="defa0-131">okno nastavení Hello požadavků možností úložiště a síť.</span><span class="sxs-lookup"><span data-stu-id="defa0-131">hello Settings blade requests storage and network options.</span></span> <span data-ttu-id="defa0-132">Můžete přijmout výchozí nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="defa0-132">You can accept hello default settings.</span></span> <span data-ttu-id="defa0-133">Azure v případě potřeby vytvoří odpovídající položky.</span><span class="sxs-lookup"><span data-stu-id="defa0-133">Azure creates appropriate entries where necessary.</span></span>

<span data-ttu-id="defa0-134">Pokud jste vybrali velikost virtuálního počítače, která to podporuje, a jako typ disku vyberete Premium (SSD), můžete vyzkoušet službu Azure Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="defa0-134">If you selected a virtual machine size that supports it, you can try Azure Premium Storage by selecting Premium (SSD) in Disk type.</span></span>

<span data-ttu-id="defa0-135">Jakmile budete se změnami hotovi, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="defa0-135">When you're done making changes, click **OK**.</span></span>

## <a name="4-summary-blade"></a><span data-ttu-id="defa0-136">4. Okno Souhrn</span><span class="sxs-lookup"><span data-stu-id="defa0-136">4. Summary blade</span></span>

<span data-ttu-id="defa0-137">Souhrn okno Hello uvádí hello nastavení zadané v předchozích oken hello.</span><span class="sxs-lookup"><span data-stu-id="defa0-137">hello Summary blade lists hello settings specified in hello previous blades.</span></span> <span data-ttu-id="defa0-138">Klikněte na tlačítko **OK** až budete připravené toomake hello image.</span><span class="sxs-lookup"><span data-stu-id="defa0-138">Click **OK** when you're ready toomake hello image.</span></span>

 ![Sestava souhrnu okna poskytnutí zadaná nastavení hello virtuálního počítače](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

<span data-ttu-id="defa0-140">Po vytvoření virtuálního počítače hello hello portál uvádí hello nového virtuálního počítače v části **všechny prostředky**a zobrazí dlaždice hello virtuálního počítače na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="defa0-140">After hello virtual machine is created, hello portal lists hello new virtual machine under **All resources**, and displays a tile of hello virtual machine on hello dashboard.</span></span> <span data-ttu-id="defa0-141">Hello odpovídající cloudové služby a účet úložiště se taky vytvořit a uvedené.</span><span class="sxs-lookup"><span data-stu-id="defa0-141">hello corresponding cloud service and storage account also are created and listed.</span></span> <span data-ttu-id="defa0-142">Automaticky spustit hello virtuálního počítače a cloudové služby a jejich stav je uveden jako **systémem**.</span><span class="sxs-lookup"><span data-stu-id="defa0-142">Both hello virtual machine and cloud service are started automatically and their status is listed as **Running**.</span></span>

 ![Konfigurace agenta virtuálního počítače a hello koncových bodů hello virtuálního počítače](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
