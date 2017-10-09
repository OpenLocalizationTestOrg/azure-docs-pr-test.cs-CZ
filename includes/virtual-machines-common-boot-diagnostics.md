<span data-ttu-id="a659f-101">V Azure je teď dostupná podpora dvou funkcí ladění: Podpora výstupu konzoly a snímku obrazovky pro model nasazení virtuálních počítačů Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a659f-101">Support for two debugging features is now available in Azure: Console Output and Screenshot support for Azure Virtual Machines Resource Manager deployment model.</span></span> 

<span data-ttu-id="a659f-102">Při zpětném přepnutí vlastní image tooAzure nebo i spouštění jeden z Image platformy hello, může být mnoha důvodů, proč se virtuální počítač získá do jiných spouštěcího stavu.</span><span class="sxs-lookup"><span data-stu-id="a659f-102">When bringing your own image tooAzure or even booting one of hello platform images, there can be many reasons why a Virtual Machine gets into a non-bootable state.</span></span> <span data-ttu-id="a659f-103">Tyto funkce povolit tooeasily jste diagnostiku a obnovení virtuálních počítačů v případě selhání spuštění.</span><span class="sxs-lookup"><span data-stu-id="a659f-103">These features enable you tooeasily diagnose and recover your Virtual Machines from boot failures.</span></span>

<span data-ttu-id="a659f-104">Pro virtuální počítače s Linuxem, můžete snadno zobrazit výstup hello protokolu konzoly z hello portálu:</span><span class="sxs-lookup"><span data-stu-id="a659f-104">For Linux Virtual Machines, you can easily view hello output of your console log from hello Portal:</span></span>

![portál Azure](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
<span data-ttu-id="a659f-106">Pro systém Windows a virtuální počítače s Linuxem, je však Azure také umožňuje toosee snímek hello virtuálních počítačů ze hypervisoru hello:</span><span class="sxs-lookup"><span data-stu-id="a659f-106">However, for both Windows and Linux Virtual Machines, Azure also enables you toosee a screenshot of hello VM from hello hypervisor:</span></span>

![Chyba](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

<span data-ttu-id="a659f-108">Obě tyto funkce jsou podporované pro virtuální počítače Azure ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="a659f-108">Both of these features are supported for Azure Virtual Machines in all regions.</span></span> <span data-ttu-id="a659f-109">Všimněte si, snímky a výstup může trvat až minut tooappear too10 ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a659f-109">Note, screenshots, and output can take up too10 minutes tooappear in your storage account.</span></span>

## <a name="common-boot-errors"></a><span data-ttu-id="a659f-110">Běžné chyby spuštění</span><span class="sxs-lookup"><span data-stu-id="a659f-110">Common boot errors</span></span>

- [<span data-ttu-id="a659f-111">0xC000000E</span><span class="sxs-lookup"><span data-stu-id="a659f-111">0xC000000E</span></span>](https://support.microsoft.com/help/4010129)
- [<span data-ttu-id="a659f-112">0xC000000F</span><span class="sxs-lookup"><span data-stu-id="a659f-112">0xC000000F</span></span>](https://support.microsoft.com/help/4010130)
- [<span data-ttu-id="a659f-113">0xC0000011</span><span class="sxs-lookup"><span data-stu-id="a659f-113">0xC0000011</span></span>](https://support.microsoft.com/help/4010134)
- [<span data-ttu-id="a659f-114">0xC0000034</span><span class="sxs-lookup"><span data-stu-id="a659f-114">0xC0000034</span></span>](https://support.microsoft.com/help/4010140)
- [<span data-ttu-id="a659f-115">0xC0000098</span><span class="sxs-lookup"><span data-stu-id="a659f-115">0xC0000098</span></span>](https://support.microsoft.com/help/4010137)
- [<span data-ttu-id="a659f-116">0xC00000BA</span><span class="sxs-lookup"><span data-stu-id="a659f-116">0xC00000BA</span></span>](https://support.microsoft.com/help/4010136)
- [<span data-ttu-id="a659f-117">0xC000014C</span><span class="sxs-lookup"><span data-stu-id="a659f-117">0xC000014C</span></span>](https://support.microsoft.com/help/4010141)
- [<span data-ttu-id="a659f-118">0xC0000221</span><span class="sxs-lookup"><span data-stu-id="a659f-118">0xC0000221</span></span>](https://support.microsoft.com/help/4010132)
- [<span data-ttu-id="a659f-119">0xC0000225</span><span class="sxs-lookup"><span data-stu-id="a659f-119">0xC0000225</span></span>](https://support.microsoft.com/help/4010138)
- [<span data-ttu-id="a659f-120">0xC0000359</span><span class="sxs-lookup"><span data-stu-id="a659f-120">0xC0000359</span></span>](https://support.microsoft.com/help/4010135)
- [<span data-ttu-id="a659f-121">0xC0000605</span><span class="sxs-lookup"><span data-stu-id="a659f-121">0xC0000605</span></span>](https://support.microsoft.com/help/4010131)
- [<span data-ttu-id="a659f-122">Operační systém nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="a659f-122">An operating system wasn't found</span></span>](https://support.microsoft.com/help/4010142)
- [<span data-ttu-id="a659f-123">Chyba při spuštění nebo NEDOSTUPNÉ_SPOUŠTĚCÍ_ZAŘÍZENÍ</span><span class="sxs-lookup"><span data-stu-id="a659f-123">Boot failure or INACCESSIBLE_BOOT_DEVICE</span></span>](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a><span data-ttu-id="a659f-124">Povolení diagnostiky na novém virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="a659f-124">Enable diagnostics on a new virtual machine</span></span>
1. <span data-ttu-id="a659f-125">Při vytváření nového virtuálního počítače z hello portál Preview, vyberte hello **Azure Resource Manager** z rozevíracího seznamu model nasazení hello:</span><span class="sxs-lookup"><span data-stu-id="a659f-125">When creating a new Virtual Machine from hello Preview Portal, select hello **Azure Resource Manager** from hello deployment model dropdown:</span></span>
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. <span data-ttu-id="a659f-127">Nakonfigurujte hello monitorování možnost tooselect hello účet úložiště kam chcete tooplace tyto diagnostické soubory.</span><span class="sxs-lookup"><span data-stu-id="a659f-127">Configure hello Monitoring option tooselect hello storage account where you would like tooplace these diagnostic files.</span></span>
 
    ![Vytvoření virtuálního počítače](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. <span data-ttu-id="a659f-129">Pokud nasazujete z šablony Azure Resource Manager, přejděte tooyour prostředek virtuálního počítače a připojte oddíl profilu hello diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="a659f-129">If you are deploying from an Azure Resource Manager template, navigate tooyour Virtual Machine resource and append hello diagnostics profile section.</span></span> <span data-ttu-id="a659f-130">Mějte na paměti, hlavičkou verze rozhraní API "2015-06-15" hello toouse.</span><span class="sxs-lookup"><span data-stu-id="a659f-130">Remember toouse hello “2015-06-15” API version header.</span></span>

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. <span data-ttu-id="a659f-131">Hello diagnostického profilu můžete účet úložiště hello tooselect místo tooput tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="a659f-131">hello diagnostics profile enables you tooselect hello storage account where you want tooput these logs.</span></span>

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

<span data-ttu-id="a659f-132">toodeploy ukázku virtuální počítač s Diagnostika spouštění povoleno, podívejte se na naše úložišti sem.</span><span class="sxs-lookup"><span data-stu-id="a659f-132">toodeploy a sample Virtual Machine with boot diagnostics enabled, check out our repo here.</span></span>

## <a name="update-an-existing-virtual-machine"></a><span data-ttu-id="a659f-133">Aktualizace stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a659f-133">Update an existing virtual machine</span></span> ##

<span data-ttu-id="a659f-134">Diagnostika spouštění tooenable prostřednictvím hello portál, můžete také aktualizovat existující virtuální počítač prostřednictvím hello portálu.</span><span class="sxs-lookup"><span data-stu-id="a659f-134">tooenable boot diagnostics through hello Portal, you can also update an existing Virtual Machine through hello Portal.</span></span> <span data-ttu-id="a659f-135">Diagnostika spouštění vyberte hello možnost a uložte.</span><span class="sxs-lookup"><span data-stu-id="a659f-135">Select hello Boot Diagnostics option and Save.</span></span> <span data-ttu-id="a659f-136">Restartujte vliv tootake hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a659f-136">Restart hello VM tootake effect.</span></span>

![Aktualizace stávajícího virtuálního počítače](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

