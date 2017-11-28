1. <span data-ttu-id="1ccc2-101">Přihlaste se na web [Azure Portal ](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1ccc2-101">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="1ccc2-102">Začněte v levém horním rohu a klikněte na **Nový > Compute > Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-102">Starting in the upper left, click **New > Compute > Windows Server 2016 Datacenter**.</span></span>

    ![Přechod k imagím virtuálních počítačů Azure na portálu](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. <span data-ttu-id="1ccc2-104">Ve Windows Serveru 2016 Datacenter vyberte model nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-104">On the Windows Server 2016 Datacenter, select the Classic deployment model.</span></span> <span data-ttu-id="1ccc2-105">Klikněte na Vytvořit.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-105">Click Create.</span></span>

    ![Snímek obrazovky zobrazující image virtuálního počítače Azure dostupné na Portálu](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a><span data-ttu-id="1ccc2-107">1. Okno Základy</span><span class="sxs-lookup"><span data-stu-id="1ccc2-107">1. Basics blade</span></span>

<span data-ttu-id="1ccc2-108">Okno Základy požaduje informace pro správu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-108">The Basics blade requests administrative information for the virtual machine.</span></span>

1. <span data-ttu-id="1ccc2-109">Zadejte **název** pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-109">Enter a **Name** for the virtual machine.</span></span> <span data-ttu-id="1ccc2-110">Virtuální počítač v tomto příkladu má název _HeroVM_.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-110">In the example, _HeroVM_ is the name of the virtual machine.</span></span> <span data-ttu-id="1ccc2-111">Název musí mít 1 až 15 znaků a nesmí obsahovat speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-111">The name must be 1-15 characters long and it cannot contain special characters.</span></span>

2. <span data-ttu-id="1ccc2-112">Zadejte **uživatelské jméno** a silné **heslo**, které se použijí k vytvoření místního účtu ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-112">Enter a **User name** and a strong **Password** that are used to create a local account on the VM.</span></span> <span data-ttu-id="1ccc2-113">Místní účet slouží k přihlášení k virtuálnímu počítači a jeho správě.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-113">The local account is used to sign in to and manage the VM.</span></span> <span data-ttu-id="1ccc2-114">Uživatelské jméno v tomto příkladu je _azureuser_.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-114">In the example, _azureuser_ is the user name.</span></span>

 <span data-ttu-id="1ccc2-115">Heslo musí mít 8 až 123 znaků a musí splňovat tři ze čtyř bezpečnostních požadavků: jedno malé písmeno, jedno velké písmeno, jedna číslice a jeden speciální znak.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-115">The password must be 8-123 characters long and meet three out of the four following complexity requirements: one lower case character, one upper case character, one number, and one special character.</span></span> <span data-ttu-id="1ccc2-116">Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](../articles/virtual-machines/windows/faq.md).</span><span class="sxs-lookup"><span data-stu-id="1ccc2-116">See more about [username and password requirements](../articles/virtual-machines/windows/faq.md).</span></span>

3. <span data-ttu-id="1ccc2-117">**Předplatné** je volitelné.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-117">The **Subscription** is optional.</span></span> <span data-ttu-id="1ccc2-118">Jedním z běžných nastavení jsou průběžné platby.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-118">One common setting is "Pay-As-You-Go".</span></span>

4. <span data-ttu-id="1ccc2-119">Vyberte existující **skupinu prostředků** nebo zadejte název nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-119">Select an existing **Resource group** or type the name for a new one.</span></span> <span data-ttu-id="1ccc2-120">Skupina prostředků v tomto příkladu má název _HeroVMRG_.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-120">In the example, _HeroVMRG_ is the name of the resource group.</span></span>

5. <span data-ttu-id="1ccc2-121">Vyberte **umístění** datového centra Azure, kde chcete virtuální počítač spustit.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-121">Select an Azure datacenter **Location** where you want the VM to run.</span></span> <span data-ttu-id="1ccc2-122">V tomto příkladu je použité umístění **Východní USA**.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-122">In the example, **East US** is the location.</span></span>

6. <span data-ttu-id="1ccc2-123">Až to budete mít, přejděte kliknutím na **OK** k dalšímu oknu.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-123">When you are done, click **Next** to continue to the next blade.</span></span>

    ![Snímek obrazovky, který zobrazuje nastavení v okně Základy pro konfiguraci virtuálního počítače Azure](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a><span data-ttu-id="1ccc2-125">2. Okno Velikost</span><span class="sxs-lookup"><span data-stu-id="1ccc2-125">2. Size blade</span></span>

<span data-ttu-id="1ccc2-126">Okno Velikost určuje konfigurační detaily virtuálního počítače a zobrazuje různé možnosti, jako je operační systém, počet procesorů, typ diskového úložiště a odhadované měsíční náklady na využití.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-126">The Size blade identifies the configuration details of the VM, and lists various choices that include OS, number of processors, disk storage type, and estimated monthly usage costs.</span></span>  

<span data-ttu-id="1ccc2-127">Zvolte velikost virtuálního počítače a poté pokračujte kliknutím na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-127">Choose a VM size, and then click **Select** to continue.</span></span> <span data-ttu-id="1ccc2-128">V tomto příkladu je použitý virtuální počítač s velikostí _DS1_\__V2 Standard_.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-128">In this example, _DS1_\__V2 Standard_ is the VM size.</span></span>

  ![Snímek obrazovky okna Velikost zobrazující velikosti virtuálního počítače Azure, ze kterých můžete vybírat](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a><span data-ttu-id="1ccc2-130">3. Okno Nastavení</span><span class="sxs-lookup"><span data-stu-id="1ccc2-130">3. Settings blade</span></span>

<span data-ttu-id="1ccc2-131">Okno Nastavení vyžaduje možnosti úložiště a sítě.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-131">The Settings blade requests storage and network options.</span></span> <span data-ttu-id="1ccc2-132">Můžete přijmout výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-132">You can accept the default settings.</span></span> <span data-ttu-id="1ccc2-133">Azure v případě potřeby vytvoří odpovídající položky.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-133">Azure creates appropriate entries where necessary.</span></span>

<span data-ttu-id="1ccc2-134">Pokud jste vybrali velikost virtuálního počítače, která to podporuje, a jako typ disku vyberete Premium (SSD), můžete vyzkoušet službu Azure Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-134">If you selected a virtual machine size that supports it, you can try Azure Premium Storage by selecting Premium (SSD) in Disk type.</span></span>

<span data-ttu-id="1ccc2-135">Jakmile budete se změnami hotovi, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-135">When you're done making changes, click **OK**.</span></span>

## <a name="4-summary-blade"></a><span data-ttu-id="1ccc2-136">4. Okno Souhrn</span><span class="sxs-lookup"><span data-stu-id="1ccc2-136">4. Summary blade</span></span>

<span data-ttu-id="1ccc2-137">Okno Souhrn zobrazuje nastavení zadaná v předchozích oknech.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-137">The Summary blade lists the settings specified in the previous blades.</span></span> <span data-ttu-id="1ccc2-138">Až budete připravení vytvořit image, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-138">Click **OK** when you're ready to make the image.</span></span>

 ![Sestava okna Souhrn s konkrétními nastaveními virtuálního počítače](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

<span data-ttu-id="1ccc2-140">Po vytvoření se nový virtuální počítač zobrazí na portálu v části **Všechny prostředky** a na řídicím panelu se zobrazí dlaždice tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-140">After the virtual machine is created, the portal lists the new virtual machine under **All resources**, and displays a tile of the virtual machine on the dashboard.</span></span> <span data-ttu-id="1ccc2-141">Současně se vytvoří a zobrazí odpovídající účet služby a cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-141">The corresponding cloud service and storage account also are created and listed.</span></span> <span data-ttu-id="1ccc2-142">Virtuální počítač i cloudová služba se automaticky spustí a jejich stav bude uvedený jako **spuštěný**.</span><span class="sxs-lookup"><span data-stu-id="1ccc2-142">Both the virtual machine and cloud service are started automatically and their status is listed as **Running**.</span></span>

 ![Konfigurace agenta virtuálního počítače a koncových bodů virtuálního počítače](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
