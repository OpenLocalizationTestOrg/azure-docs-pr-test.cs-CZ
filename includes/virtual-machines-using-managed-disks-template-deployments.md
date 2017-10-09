# <a name="using-managed-disks-in-azure-resource-manager-templates"></a><span data-ttu-id="6c40f-101">Pomocí spravovaného disky v šablonách Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="6c40f-101">Using Managed Disks in Azure Resource Manager Templates</span></span>

<span data-ttu-id="6c40f-102">Tento dokument vás provede hello rozdíly mezi spravovanými a nespravovanými disky pomocí Azure Resource Manager šablony tooprovision virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6c40f-102">This document walks through hello differences between managed and unmanaged disks when using Azure Resource Manager templates tooprovision virtual machines.</span></span> <span data-ttu-id="6c40f-103">To vám pomůže tooupdate existujících šablon, které používají nespravovaná disky toomanaged disky.</span><span class="sxs-lookup"><span data-stu-id="6c40f-103">This will help you tooupdate existing templates that are using unmanaged Disks toomanaged disks.</span></span> <span data-ttu-id="6c40f-104">Pro referenci používáme hello [101-vm jednoduché – windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) šablony jako vodítko.</span><span class="sxs-lookup"><span data-stu-id="6c40f-104">For reference, we are using hello [101-vm-simple-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) template as a guide.</span></span> <span data-ttu-id="6c40f-105">Uvidíte hello šablony pomocí obou [discích spravovaných](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) a předchozí verze pomocí [nespravované disky](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) budete-li toodirectly porovnat.</span><span class="sxs-lookup"><span data-stu-id="6c40f-105">You can see hello template using both [managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) and a prior version using [unmanaged disks](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) if you'd like toodirectly compare them.</span></span>

## <a name="unmanaged-disks-template-formatting"></a><span data-ttu-id="6c40f-106">Nespravované disky šablony, formátování</span><span class="sxs-lookup"><span data-stu-id="6c40f-106">Unmanaged Disks template formatting</span></span>

<span data-ttu-id="6c40f-107">toobegin, jsme se podívejte na tom, jak nespravované disky jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="6c40f-107">toobegin, we take a look at how unmanaged disks are deployed.</span></span> <span data-ttu-id="6c40f-108">Při vytváření nespravované disků, je třeba souborů virtuálního pevného disku hello toohold účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c40f-108">When creating unmanaged disks, you need a storage account toohold hello VHD files.</span></span> <span data-ttu-id="6c40f-109">Můžete vytvořit nový účet úložiště nebo použít jednu, která již existuje.</span><span class="sxs-lookup"><span data-stu-id="6c40f-109">You can create a new storage account or use one that already exists.</span></span> <span data-ttu-id="6c40f-110">Tento článek vám ukáže, jak toocreate nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c40f-110">This article will show you how toocreate a new storage account.</span></span> <span data-ttu-id="6c40f-111">tooaccomplish, budete potřebovat prostředek účet úložiště v bloku hello prostředky, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6c40f-111">tooaccomplish this, you need a storage account resource in hello resources block as shown below.</span></span>

```
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2016-01-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

<span data-ttu-id="6c40f-112">V rámci hello objektu virtuálního počítače potřebujeme závislost na hello tooensure účet úložiště, vytvořené před hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6c40f-112">Within hello virtual machine object, we need a dependency on hello storage account tooensure that it's created before hello virtual machine.</span></span> <span data-ttu-id="6c40f-113">V rámci hello `storageProfile` tématu, potom zadejte hello úplný identifikátor URI hello umístění virtuálního pevného disku, který odkazuje na účet úložiště hello a je potřebná pro disk hello operačního systému a všech datových disků.</span><span class="sxs-lookup"><span data-stu-id="6c40f-113">Within hello `storageProfile` section, we then specify hello full URI of hello VHD location, which references hello storage account and is needed for hello OS disk and any data disks.</span></span> 

```
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "name": "osdisk",
                "vhd": {
                    "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "name": "datadisk1",
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "vhd": {
                        "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/datadisk1.vhd')]"
                    },
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

## <a name="managed-disks-template-formatting"></a><span data-ttu-id="6c40f-114">Spravované disky šablony formátování</span><span class="sxs-lookup"><span data-stu-id="6c40f-114">Managed disks template formatting</span></span>

<span data-ttu-id="6c40f-115">S Azure spravované disky hello disku se změní na nejvyšší úrovni prostředků a už vyžaduje toobe účet úložiště vytvořené uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="6c40f-115">With Azure Managed Disks, hello disk becomes a top-level resource and no longer requires a storage account toobe created by hello user.</span></span> <span data-ttu-id="6c40f-116">Spravované disky nejprve byly vystaveny v hello `2016-04-30-preview` verze rozhraní API, jsou k dispozici ve všech dalších verzích rozhraní API a se teď typ disku výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="6c40f-116">Managed disks were first exposed in hello `2016-04-30-preview` API version, they are available in all subsequent API versions and are now hello default disk type.</span></span> <span data-ttu-id="6c40f-117">Hello následující části provede hello výchozí nastavení a podrobností, jak přizpůsobit toofurther vaše disky.</span><span class="sxs-lookup"><span data-stu-id="6c40f-117">hello following sections walk through hello default settings and detail how toofurther customize your disks.</span></span>

> [!NOTE]
> <span data-ttu-id="6c40f-118">Je doporučeno toouse rozhraní API verze pozdější než `2016-04-30-preview` jako došlo k narušující změny mezi `2016-04-30-preview` a `2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="6c40f-118">It is recommended toouse an API version later than `2016-04-30-preview` as there were breaking changes between `2016-04-30-preview` and `2017-03-30`.</span></span>
>
>

### <a name="default-managed-disk-settings"></a><span data-ttu-id="6c40f-119">Výchozí nastavení spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="6c40f-119">Default managed disk settings</span></span>

<span data-ttu-id="6c40f-120">toocreate virtuální počítač s spravovaných disků, je už potřebovat toocreate hello úložiště účet prostředků a následujícím způsobem můžete aktualizovat prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6c40f-120">toocreate a VM with managed disks, you no longer need toocreate hello storage account resource and can update your virtual machine resource as follows.</span></span> <span data-ttu-id="6c40f-121">Specificky Upozorňujeme, že hello `apiVersion` odráží `2017-03-30` a hello `osDisk` a `dataDisks` už odkazovat tooa konkrétní identifikátor URI pro hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="6c40f-121">Specifically note that hello `apiVersion` reflects `2017-03-30` and hello `osDisk` and `dataDisks` no longer refer tooa specific URI for hello VHD.</span></span> <span data-ttu-id="6c40f-122">Pokud nasazujete bez zadání dalších vlastností, bude používat hello disku [úložiště LRS standardní](../articles/storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="6c40f-122">When deploying without specifying additional properties, hello disk will use [Standard LRS storage](../articles/storage/common/storage-redundancy.md).</span></span> <span data-ttu-id="6c40f-123">Pokud není zadán žádný název, jak dlouho trvá hello formát `<VMName>_OsDisk_1_<randomstring>` pro disk hello operačního systému a `<VMName>_disk<#>_<randomstring>` pro každý datový disk.</span><span class="sxs-lookup"><span data-stu-id="6c40f-123">If no name is specified, it takes hello format of `<VMName>_OsDisk_1_<randomstring>` for hello OS disk and `<VMName>_disk<#>_<randomstring>` for each data disk.</span></span> <span data-ttu-id="6c40f-124">Ve výchozím nastavení je Azure disk encryption zakázaný. ukládání do mezipaměti je pro čtení a zápis pro disk hello operačního systému a jeden pro datové disky.</span><span class="sxs-lookup"><span data-stu-id="6c40f-124">By default, Azure disk encryption is disabled; caching is Read/Write for hello OS disk and None for data disks.</span></span> <span data-ttu-id="6c40f-125">Můžete si povšimnout v následujícím příkladu hello, že je stále závislost účtu úložiště, i když toto je pouze pro úložiště diagnostiky a není potřeba ukládání na disk.</span><span class="sxs-lookup"><span data-stu-id="6c40f-125">You may notice in hello example below there is still a storage account dependency, though this is only for storage of diagnostics and is not needed for disk storage.</span></span>

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="using-a-top-level-managed-disk-resource"></a><span data-ttu-id="6c40f-126">Použití spravovaných disků na nejvyšší úrovni prostředků</span><span class="sxs-lookup"><span data-stu-id="6c40f-126">Using a top-level managed disk resource</span></span>

<span data-ttu-id="6c40f-127">Jako alternativní toospecifying hello konfiguraci disku v objektu hello virtuálního počítače můžete vytvořit nejvyšší úrovně diskový prostředek a připojte ji jako součást hello vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6c40f-127">As an alternative toospecifying hello disk configuration in hello virtual machine object, you can create a top-level disk resource and attach it as part of hello virtual machine creation.</span></span> <span data-ttu-id="6c40f-128">Například můžeme vytvořit prostředek disku takto toouse jako datový disk.</span><span class="sxs-lookup"><span data-stu-id="6c40f-128">For example, we can create a disk resource as follows toouse as a data disk.</span></span>

```
{
    "type": "Microsoft.Compute/disks",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "apiVersion": "2017-03-30",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "properties": {
        "creationData": {
            "createOption": "Empty"
        },
        "diskSizeGB": 1023
    }
}
```

<span data-ttu-id="6c40f-129">V rámci hello objekt virtuálního počítače jsme pak můžete odkazovat toobe objekt tento disk připojit.</span><span class="sxs-lookup"><span data-stu-id="6c40f-129">Within hello VM object, we can then reference this disk object toobe attached.</span></span> <span data-ttu-id="6c40f-130">Zadání ID prostředku hello hello spravované disk, který jsme vytvořili v hello `managedDisk` vlastnost umožňuje hello připojení disku hello jako hello vytvoří virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6c40f-130">Specifying hello resource ID of hello managed disk we created in hello `managedDisk` property allows hello attachment of hello disk as hello VM is created.</span></span> <span data-ttu-id="6c40f-131">Všimněte si, že hello `apiVersion` hello prostředků virtuálního počítače je příliš nastavený`2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="6c40f-131">Note that hello `apiVersion` for hello VM resource is set too`2017-03-30`.</span></span> <span data-ttu-id="6c40f-132">Všimněte si také, že vytvořili jsme závislost na hello disku prostředků tooensure, které je úspěšně vytvořen před vytvořením virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6c40f-132">Also note that we've created a dependency on hello disk resource tooensure it's successfully created before VM creation.</span></span> 

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]",
        "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "lun": 0,
                    "name": "[concat(variables('vmName'),'-datadisk1')]",
                    "createOption": "attach",
                    "managedDisk": {
                        "id": "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
                    }
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a><span data-ttu-id="6c40f-133">Vytvoření skupiny dostupnosti spravované s virtuálními počítači pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="6c40f-133">Create managed availability sets with VMs using managed disks</span></span>

<span data-ttu-id="6c40f-134">toocreate spravované dostupnost sady s virtuálními počítači pomocí spravovaných disků, přidejte hello `sku` objekt toohello dostupnosti prostředků a nastavte a hello `name` vlastnost příliš`Aligned`.</span><span class="sxs-lookup"><span data-stu-id="6c40f-134">toocreate managed availability sets with VMs using managed disks, add hello `sku` object toohello availability set resource and set hello `name` property too`Aligned`.</span></span> <span data-ttu-id="6c40f-135">Tím se zajistí hello disky pro jednotlivé virtuální počítače dostatečně izolované od sebe navzájem tooavoid jediný bod selhání.</span><span class="sxs-lookup"><span data-stu-id="6c40f-135">This ensures that hello disks for each VM are sufficiently isolated from each other tooavoid single points of failure.</span></span> <span data-ttu-id="6c40f-136">Všimněte si, že hello `apiVersion` pro prostředek skupiny dostupnosti hello je nastaven příliš`2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="6c40f-136">Also note that hello `apiVersion` for hello availability set resource is set too`2017-03-30`.</span></span>

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/availabilitySets",
    "location": "[resourceGroup().location]",
    "name": "[variables('avSetName')]",
    "properties": {
        "PlatformUpdateDomainCount": 3,
        "PlatformFaultDomainCount": 2
    },
    "sku": {
        "name": "Aligned"
    }
}
```

### <a name="additional-scenarios-and-customizations"></a><span data-ttu-id="6c40f-137">Další scénáře a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="6c40f-137">Additional scenarios and customizations</span></span>

<span data-ttu-id="6c40f-138">úplné informace toofind na specifikacích hello REST API, přečtěte si hello [vytvoření spravovaného disku dokumentace k REST API](/rest/api/manageddisks/disks/disks-create-or-update).</span><span class="sxs-lookup"><span data-stu-id="6c40f-138">toofind full information on hello REST API specifications, please review hello [create a managed disk REST API documentation](/rest/api/manageddisks/disks/disks-create-or-update).</span></span> <span data-ttu-id="6c40f-139">Zjistíte další scénáře, a také výchozí a přijatelných hodnot, které mohou být odeslaná toohello rozhraní API prostřednictvím šablony nasazení.</span><span class="sxs-lookup"><span data-stu-id="6c40f-139">You will find additional scenarios, as well as default and acceptable values that can be submitted toohello API through template deployments.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="6c40f-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c40f-140">Next steps</span></span>

* <span data-ttu-id="6c40f-141">Úplné šablony, které používají spravovaných disků na adrese hello následující odkazy úložišti Azure rychlý start.</span><span class="sxs-lookup"><span data-stu-id="6c40f-141">For full templates that use managed disks visit hello following Azure Quickstart Repo links.</span></span>
    * [<span data-ttu-id="6c40f-142">Virtuální počítač s Windows spravované disku</span><span class="sxs-lookup"><span data-stu-id="6c40f-142">Windows VM with managed disk</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [<span data-ttu-id="6c40f-143">Virtuální počítač s Linuxem pomocí spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="6c40f-143">Linux VM with managed disk</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [<span data-ttu-id="6c40f-144">Úplný seznam spravovaných disků na šablony</span><span class="sxs-lookup"><span data-stu-id="6c40f-144">Full list of managed disk templates</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="6c40f-145">Navštivte hello [přehled disky spravované Azure](../articles/virtual-machines/windows/managed-disks-overview.md) dokumentu toolearn Další informace o discích spravovaných.</span><span class="sxs-lookup"><span data-stu-id="6c40f-145">Visit hello [Azure Managed Disks Overview](../articles/virtual-machines/windows/managed-disks-overview.md) document toolearn more about managed disks.</span></span>
* <span data-ttu-id="6c40f-146">Zkontrolujte hello šablony referenční dokumentaci pro prostředky virtuálního počítače návštěvou hello [odkaz na šablonu Microsoft.Compute/virtualMachines](/templates/microsoft.compute/virtualmachines) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="6c40f-146">Review hello template reference documentation for virtual machine resources by visiting hello [Microsoft.Compute/virtualMachines template reference](/templates/microsoft.compute/virtualmachines) document.</span></span>
* <span data-ttu-id="6c40f-147">Zkontrolujte hello šablony referenční dokumentaci pro prostředky disku návštěvou hello [odkaz na šablonu Microsoft.Compute/disks](/templates/microsoft.compute/disks) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="6c40f-147">Review hello template reference documentation for disk resources by visiting hello [Microsoft.Compute/disks template reference](/templates/microsoft.compute/disks) document.</span></span>
 
