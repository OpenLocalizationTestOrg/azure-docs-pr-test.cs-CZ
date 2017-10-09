## <a name="overview"></a><span data-ttu-id="3e360-101">Přehled</span><span class="sxs-lookup"><span data-stu-id="3e360-101">Overview</span></span>
<span data-ttu-id="3e360-102">Při vytváření nového virtuálního počítače (VM) ve skupině prostředků nasazením bitové kopie z [Azure Marketplace](https://azure.microsoft.com/marketplace/), jednotka operačního systému výchozí hello je 127 GB.</span><span class="sxs-lookup"><span data-stu-id="3e360-102">When you create a new virtual machine (VM) in a Resource Group by deploying an image from [Azure Marketplace](https://azure.microsoft.com/marketplace/), hello default OS drive is 127 GB.</span></span> <span data-ttu-id="3e360-103">Přestože je možné tooadd datové disky toohello virtuálních počítačů (kolik v závislosti na hello SKU jste zvolili) a kromě toho je doporučené tooinstall aplikací a zatížení s intenzivním procesoru v těchto discích dodatek, často mají zákazníci tooexpand hello operačního systému jednotka toosupport určité scénáře, jako je následující:</span><span class="sxs-lookup"><span data-stu-id="3e360-103">Even though it’s possible tooadd data disks toohello VM (how many depending upon hello SKU you’ve chosen) and moreover it’s recommended tooinstall applications and CPU intensive workloads on these addendum disks, oftentimes customers need tooexpand hello OS drive toosupport certain scenarios such as following:</span></span>

1. <span data-ttu-id="3e360-104">Podpora starších verzí aplikací, které instalují komponenty na jednotku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3e360-104">Support legacy applications that install components on OS drive.</span></span>
2. <span data-ttu-id="3e360-105">Migrace fyzického nebo virtuálního počítače s větší jednotkou operačního systému z místního prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e360-105">Migrate a physical PC or virtual machine from on-premises with a larger OS drive.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3e360-106">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="3e360-106">Azure has two different deployment models for creating and working with resources: Resource Manager and Classic.</span></span> <span data-ttu-id="3e360-107">Tento článek se zabývá pomocí modelu Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="3e360-107">This article covers using hello Resource Manager model.</span></span> <span data-ttu-id="3e360-108">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="3e360-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> 
> 

## <a name="resize-hello-os-drive"></a><span data-ttu-id="3e360-109">Změna velikosti disku hello operačního systému</span><span class="sxs-lookup"><span data-stu-id="3e360-109">Resize hello OS drive</span></span>
<span data-ttu-id="3e360-110">V tomto článku jsme vám dá dosáhnout hello o změně velikosti disku hello operačního systému pomocí resource manager modul [prostředí Azure Powershell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="3e360-110">In this article we’ll accomplish hello task of resizing hello OS drive using resource manager modules of [Azure Powershell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="3e360-111">Otevřete prostředí Powershell ISE nebo okno prostředí Powershell v režimu správce a postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="3e360-111">Open your Powershell ISE or Powershell window in administrative mode and follow hello steps below:</span></span>

1. <span data-ttu-id="3e360-112">Přihlášení tooyour Microsoft Azure účet v režimu správy prostředků a vybrat své předplatné následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3e360-112">Sign-in tooyour Microsoft Azure account in resource management mode and select your subscription as follows:</span></span>
   
   ```Powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. <span data-ttu-id="3e360-113">Nastavte název skupiny prostředků a název virtuálního počítače následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3e360-113">Set your resource group name and VM name as follows:</span></span>
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. <span data-ttu-id="3e360-114">Získejte odkaz tooyour virtuální počítač následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3e360-114">Obtain a reference tooyour VM as follows:</span></span>
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. <span data-ttu-id="3e360-115">Zastavte hello virtuální počítač před změnou velikosti disku hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3e360-115">Stop hello VM before resizing hello disk as follows:</span></span>
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. <span data-ttu-id="3e360-116">A tady se dodává hello okamžiku, kdy jsme jste čekali pro!</span><span class="sxs-lookup"><span data-stu-id="3e360-116">And here comes hello moment we’ve been waiting for!</span></span> <span data-ttu-id="3e360-117">Nastavení velikosti hello hodnota toohello potřeby disku hello operačního systému a aktualizace hello virtuální počítač následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3e360-117">Set hello size of hello OS disk toohello desired value and update hello VM as follows:</span></span>
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > <span data-ttu-id="3e360-118">Nová velikost Hello musí být větší než velikost disku existující hello.</span><span class="sxs-lookup"><span data-stu-id="3e360-118">hello new size should be greater than hello existing disk size.</span></span> <span data-ttu-id="3e360-119">maximální povolené Hello je 1023 GB.</span><span class="sxs-lookup"><span data-stu-id="3e360-119">hello maximum allowed is 1023 GB.</span></span>
   > 
   > 
6. <span data-ttu-id="3e360-120">Aktualizace hello virtuálních počítačů může trvat několik sekund.</span><span class="sxs-lookup"><span data-stu-id="3e360-120">Updating hello VM may take a few seconds.</span></span> <span data-ttu-id="3e360-121">Po dokončení provádění příkazu hello restartujte hello virtuální počítač následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3e360-121">Once hello command finishes executing, restart hello VM as follows:</span></span>
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

<span data-ttu-id="3e360-122">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="3e360-122">And that’s it!</span></span> <span data-ttu-id="3e360-123">Nyní RDP do hello virtuální počítač, otevřete správu počítače (nebo Správa disků) a rozbalte pomocí hello nově přidělené místo na disku hello.</span><span class="sxs-lookup"><span data-stu-id="3e360-123">Now RDP into hello VM, open Computer Management (or Disk Management) and expand hello drive using hello newly allocated space.</span></span>

## <a name="summary"></a><span data-ttu-id="3e360-124">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3e360-124">Summary</span></span>
<span data-ttu-id="3e360-125">V tomto článku jsme použili moduly Azure Resource Manageru z prostředí Powershell tooexpand hello jednotky operačního systému virtuálního počítače IaaS.</span><span class="sxs-lookup"><span data-stu-id="3e360-125">In this article, we used Azure Resource Manager modules of Powershell tooexpand hello OS drive of an IaaS virtual machine.</span></span> <span data-ttu-id="3e360-126">Hello celý skript pro vaši informaci reprodukovat níže je:</span><span class="sxs-lookup"><span data-stu-id="3e360-126">Reproduced below is hello complete script for your reference:</span></span>

```Powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="next-steps"></a><span data-ttu-id="3e360-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e360-127">Next Steps</span></span>
<span data-ttu-id="3e360-128">Když v tomto článku jsme zaměřuje především na rozšiřování hello operačního systému disku hello virtuálních počítačů, hello vyvinuté skript může být rovněž pro rozšíření hello datové disky připojené toohello virtuálních počítačů tak, že změníte jeden řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="3e360-128">Though in this article, we focused primarily on expanding hello OS disk of hello VM, hello developed script may also be used for expanding hello data disks attached toohello VM by changing a single line of code.</span></span> <span data-ttu-id="3e360-129">Například tooexpand hello první data připojené toohello virtuálních počítačů na disku, nahraďte hello ```OSDisk``` objekt ```StorageProfile``` s ```DataDisks``` pole a používat číselný index tooobtain odkaz toofirst připojené datový disk, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="3e360-129">For example, tooexpand hello first data disk attached toohello VM, replace hello ```OSDisk``` object of ```StorageProfile``` with ```DataDisks``` array and use a numeric index tooobtain a reference toofirst attached data disk, as shown below:</span></span>

```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
<span data-ttu-id="3e360-130">Podobně můžete odkazovat na jiné datové disky připojené toohello virtuálních počítačů, buď pomocí indexu jako v příkladu nahoře, nebo hello ```Name``` vlastnost hello disku, jak je uvedeno dále:</span><span class="sxs-lookup"><span data-stu-id="3e360-130">Similarly you may reference other data disks attached toohello VM, either by using an index as shown above or hello ```Name``` property of hello disk as illustrated below:</span></span>

```Powershell
($vm.StorageProfile.DataDisks | Where {$_.Name -eq 'my-second-data-disk'})[0].DiskSizeGB = 1023
```

<span data-ttu-id="3e360-131">Pokud chcete toofind na tom, jak tooattach disky tooan virtuálního počítače Azure Resource Manager, zaškrtněte toto políčko [článku](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3e360-131">If you want toofind out how tooattach disks tooan Azure Resource Manager VM, check this [article](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

