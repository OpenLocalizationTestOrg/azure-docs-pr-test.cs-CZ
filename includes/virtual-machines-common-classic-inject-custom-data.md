


<span data-ttu-id="324ca-101">Toto téma popisuje postup:</span><span class="sxs-lookup"><span data-stu-id="324ca-101">This topic describes how to:</span></span>

* <span data-ttu-id="324ca-102">Vložení dat do Azure virtuální počítač (VM) při jeho zřizování.</span><span class="sxs-lookup"><span data-stu-id="324ca-102">Inject data into an Azure virtual machine (VM) when it is being provisioned.</span></span>
* <span data-ttu-id="324ca-103">Načíst ho pro systém Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="324ca-103">Retrieve it for both Windows and Linux.</span></span>
* <span data-ttu-id="324ca-104">Použít speciální nástroje, které jsou k dispozici na některé systémy toodetect a automaticky zpracovávat vlastní data.</span><span class="sxs-lookup"><span data-stu-id="324ca-104">Use special tools available on some systems toodetect and handle custom data automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="324ca-105">Tento článek popisuje, jak vlastních dat může pomocí virtuální počítač vytvořen s hello Azure Service Management API vložit.</span><span class="sxs-lookup"><span data-stu-id="324ca-105">This article describes how custom data can be injected by using a VM created with hello Azure Service Management API.</span></span> <span data-ttu-id="324ca-106">jak zjistit, toouse hello rozhraní API správy prostředků Azure, toosee [hello příklad šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).</span><span class="sxs-lookup"><span data-stu-id="324ca-106">toosee how toouse hello Azure Resource Management API, see [hello example template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).</span></span>
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a><span data-ttu-id="324ca-107">Vložení vlastní data do virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="324ca-107">Injecting custom data into your Azure virtual machine</span></span>
<span data-ttu-id="324ca-108">Tato funkce v současné době podporuje pouze v hello [rozhraní příkazového řádku Azure](https://github.com/Azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="324ca-108">This feature is currently supported only in hello [Azure Command-Line Interface](https://github.com/Azure/azure-xplat-cli).</span></span> <span data-ttu-id="324ca-109">Tady vytváříme `custom-data.txt` soubor, který obsahuje naše data pak vložit, v toohello virtuálních počítačů během zřizování.</span><span class="sxs-lookup"><span data-stu-id="324ca-109">Here we create a `custom-data.txt` file that contains our data, then inject that in toohello VM during provisioning.</span></span> <span data-ttu-id="324ca-110">I když některé z možností hello může použít pro hello `azure vm create` příkaz hello následující ukazuje jeden ze způsobů velmi základní:</span><span class="sxs-lookup"><span data-stu-id="324ca-110">Although you may use any of hello options for hello `azure vm create` command, hello following demonstrates one very basic approach:</span></span>

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-hello-virtual-machine"></a><span data-ttu-id="324ca-111">Pomocí vlastních dat ve virtuálním počítači hello</span><span class="sxs-lookup"><span data-stu-id="324ca-111">Using custom data in hello virtual machine</span></span>
* <span data-ttu-id="324ca-112">Pokud je virtuální počítač Azure virtuálních počítačů na bázi Windows, pak hello vlastní datový soubor je uložen příliš`%SYSTEMDRIVE%\AzureData\CustomData.bin`.</span><span class="sxs-lookup"><span data-stu-id="324ca-112">If your Azure VM is a Windows-based VM, then hello custom data file is saved too`%SYSTEMDRIVE%\AzureData\CustomData.bin`.</span></span> <span data-ttu-id="324ca-113">I když byl tootransfer kódováním base64 z místního počítače toohello hello nového virtuálního počítače, je automaticky dekódovat a dá otevřít nebo použít okamžitě.</span><span class="sxs-lookup"><span data-stu-id="324ca-113">Although it was base64-encoded tootransfer from hello local computer toohello new VM, it is automatically decoded and can be opened or used immediately.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="324ca-114">Pokud existuje soubor hello, se přepíše.</span><span class="sxs-lookup"><span data-stu-id="324ca-114">If hello file exists, it is overwritten.</span></span> <span data-ttu-id="324ca-115">Hello zabezpečení v adresáři hello je nastaven příliš**systému: Úplné řízení** a **Administrators: Úplné řízení**.</span><span class="sxs-lookup"><span data-stu-id="324ca-115">hello security on hello directory is set too**System:Full Control** and **Administrators:Full Control**.</span></span>
  > 
  > 
* <span data-ttu-id="324ca-116">Pokud virtuální počítač Azure je virtuální počítač založený na Linuxu, hello vlastní datový soubor bude umístěn v jedna z následujících hello umístí v závislosti na vaší distro.</span><span class="sxs-lookup"><span data-stu-id="324ca-116">If your Azure VM is a Linux-based VM, then hello custom data file will be located in one of hello following places depending on your distro.</span></span> <span data-ttu-id="324ca-117">Hello data mohou být kódováním base64, proto může třeba toodecode hello data nejdřív:</span><span class="sxs-lookup"><span data-stu-id="324ca-117">hello data may be base64-encoded, so you may need toodecode hello data first:</span></span>
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a><span data-ttu-id="324ca-118">Init cloudu v Azure</span><span class="sxs-lookup"><span data-stu-id="324ca-118">Cloud-init on Azure</span></span>
<span data-ttu-id="324ca-119">Pokud virtuální počítač Azure z image Ubuntu nebo CoreOS, můžete použít CustomData toosend cloudu config toocloud-init.</span><span class="sxs-lookup"><span data-stu-id="324ca-119">If your Azure VM is from an Ubuntu or CoreOS image, then you can use CustomData toosend a cloud-config toocloud-init.</span></span> <span data-ttu-id="324ca-120">Nebo pokud vlastní datový soubor skriptu, pak cloudu init jednoduše ho provést.</span><span class="sxs-lookup"><span data-stu-id="324ca-120">Or if your custom data file is a script, then cloud-init can simply execute it.</span></span>

### <a name="ubuntu-cloud-images"></a><span data-ttu-id="324ca-121">Ubuntu cloudu obrázků</span><span class="sxs-lookup"><span data-stu-id="324ca-121">Ubuntu Cloud Images</span></span>
<span data-ttu-id="324ca-122">Ve většině Image Azure Linux, by upravit "/ etc/waagent.conf" tooconfigure hello dočasných prostředků disku a odkládací soubor.</span><span class="sxs-lookup"><span data-stu-id="324ca-122">In most Azure Linux images, you would edit "/etc/waagent.conf" tooconfigure hello temporary resource disk and swap file.</span></span> <span data-ttu-id="324ca-123">V tématu [Azure Linux Agent uživatelská příručka](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace.</span><span class="sxs-lookup"><span data-stu-id="324ca-123">See [Azure Linux Agent user guide](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information.</span></span>

<span data-ttu-id="324ca-124">Ale na Image cloudu Ubuntu hello, musíte použít cloudové init tooconfigure hello prostředků disku (tj. disk hello "dočasné") a velikost odkládacího souboru oddílu.</span><span class="sxs-lookup"><span data-stu-id="324ca-124">However, on hello Ubuntu Cloud Images, you must use cloud-init tooconfigure hello resource disk (that is, hello "ephemeral" disk) and swap partition.</span></span> <span data-ttu-id="324ca-125">Hello následující stránky na stránkách wiki Ubuntu hello další podrobnosti najdete v části: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).</span><span class="sxs-lookup"><span data-stu-id="324ca-125">See hello following page on hello Ubuntu wiki for more details: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps-using-cloud-init"></a><span data-ttu-id="324ca-126">Další kroky: pomocí init cloudu</span><span class="sxs-lookup"><span data-stu-id="324ca-126">Next steps: Using cloud-init</span></span>
<span data-ttu-id="324ca-127">Další informace najdete v tématu hello [cloudu init dokumentaci Ubuntu](https://help.ubuntu.com/community/CloudInit).</span><span class="sxs-lookup"><span data-stu-id="324ca-127">For further information, see hello [cloud-init documentation for Ubuntu](https://help.ubuntu.com/community/CloudInit).</span></span>

<!--Link references-->
[<span data-ttu-id="324ca-128">Přidání Role referenční dokumentace rozhraní API REST služby správy</span><span class="sxs-lookup"><span data-stu-id="324ca-128">Add Role Service Management REST API Reference</span></span>](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[<span data-ttu-id="324ca-129">Rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="324ca-129">Azure Command-line Interface</span></span>](https://github.com/Azure/azure-xplat-cli)

