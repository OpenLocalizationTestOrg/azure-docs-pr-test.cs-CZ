


## <a name="using-vm-extensions"></a><span data-ttu-id="e855f-101">Pomocí rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e855f-101">Using VM Extensions</span></span>
<span data-ttu-id="e855f-102">Rozšíření virtuálního počítače Azure implementovat chování nebo funkce, které buď další programy fungovat na virtuálních počítačích Azure (například **WebDeployForVSDevTest** rozšíření umožňuje sadě Visual Studio pro řešení pro nasazení webu na vašem virtuálním počítači Azure) nebo zadejte možnost pro vás k interakci s virtuálním Počítačem pro podporu některé jiné chování (například můžete použít rozšíření přístupu virtuálních počítačů z prostředí PowerShell, rozhraní příkazového řádku Azure a REST klientů resetovat nebo upravte hodnoty vzdáleného přístupu na vašem virtuálním počítači Azure).</span><span class="sxs-lookup"><span data-stu-id="e855f-102">Azure VM Extensions implement behaviors or features that either help other programs work on Azure VMs (for example, the **WebDeployForVSDevTest** extension allows Visual Studio to Web Deploy solutions on your Azure VM) or provide the ability for you to interact with the VM to support some other behavior (for example, you can use the VM Access extensions from PowerShell, the Azure CLI, and REST clients to reset or modify remote access values on your Azure VM).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e855f-103">Úplný seznam rozšíření funkce podporují, najdete v části [rozšíření virtuálního počítače Azure a funkce](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e855f-103">For a complete list of extensions by the features they support, see [Azure VM Extensions and Features](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="e855f-104">Protože každý rozšíření virtuálního počítače podporuje konkrétní funkci, přesně co můžete a nemůžete dělat s příponou závisí na rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e855f-104">Because each VM extension supports a specific feature, exactly what you can and cannot do with an extension depends on the extension.</span></span> <span data-ttu-id="e855f-105">Proto před změnou virtuálního počítače, zkontrolujte, zda že máte ke čtení v dokumentaci pro rozšíření virtuálního počítače, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="e855f-105">Therefore, before modifying your VM, make sure you have read the documentation for the VM Extension you want to use.</span></span> <span data-ttu-id="e855f-106">Odebírání některé rozšíření virtuálního počítače není podporováno; ostatní uživatelé mají vlastnosti, které lze nastavit, které výrazně mění chování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e855f-106">Removing some VM Extensions is not supported; others have properties that can be set that change VM behavior radically.</span></span>
> 
> 

<span data-ttu-id="e855f-107">Běžné úkoly, jsou:</span><span class="sxs-lookup"><span data-stu-id="e855f-107">The most common tasks are:</span></span>

1. <span data-ttu-id="e855f-108">Hledání dostupných rozšíření</span><span class="sxs-lookup"><span data-stu-id="e855f-108">Finding Available Extensions</span></span>
2. <span data-ttu-id="e855f-109">Aktualizace načíst rozšíření</span><span class="sxs-lookup"><span data-stu-id="e855f-109">Updating Loaded Extensions</span></span>
3. <span data-ttu-id="e855f-110">Přidání rozšíření</span><span class="sxs-lookup"><span data-stu-id="e855f-110">Adding Extensions</span></span>
4. <span data-ttu-id="e855f-111">Odebrání rozšíření</span><span class="sxs-lookup"><span data-stu-id="e855f-111">Removing Extensions</span></span>

## <a name="find-available-extensions"></a><span data-ttu-id="e855f-112">Najít dostupných rozšíření</span><span class="sxs-lookup"><span data-stu-id="e855f-112">Find Available Extensions</span></span>
<span data-ttu-id="e855f-113">Můžete vyhledat rozšíření a použití rozšířené informace:</span><span class="sxs-lookup"><span data-stu-id="e855f-113">You can locate extension and extended information using:</span></span>

* <span data-ttu-id="e855f-114">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e855f-114">PowerShell</span></span>
* <span data-ttu-id="e855f-115">Rozhraní Azure napříč platformami příkazového řádku (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="e855f-115">Azure Cross-Platform Command Line Interface (Azure CLI)</span></span>
* <span data-ttu-id="e855f-116">Rozhraní REST API pro správu služeb</span><span class="sxs-lookup"><span data-stu-id="e855f-116">Service Management REST API</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="e855f-117">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e855f-117">Azure PowerShell</span></span>
<span data-ttu-id="e855f-118">Některá rozšíření mít rutiny prostředí PowerShell, které jsou specifické pro, který může zjednodušit jejich konfigurace z prostředí PowerShell; ale následující rutiny fungovat pro všechny rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e855f-118">Some extensions have PowerShell cmdlets that are specific to them, which may make their configuration from PowerShell easier; but the following cmdlets work for all VM extensions.</span></span>

<span data-ttu-id="e855f-119">Následující rutiny můžete použít k získání informací o dostupných rozšíření:</span><span class="sxs-lookup"><span data-stu-id="e855f-119">You can use the following cmdlets to obtain information about available extensions:</span></span>

* <span data-ttu-id="e855f-120">Pro instance webové role nebo rolí pracovního procesu, můžete použít [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="e855f-120">For instances of web roles or worker roles, you can use the [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet.</span></span>
* <span data-ttu-id="e855f-121">Pro instance virtuálních počítačů, můžete použít [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="e855f-121">For instances of Virtual Machines, you can use the [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet.</span></span>
  
   <span data-ttu-id="e855f-122">Například následující příklad kódu ukazuje, jak uvádí informace k **IaaSDiagnostics** rozšíření pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e855f-122">For example, the following code example shows how to list the information for the **IaaSDiagnostics** extension using PowerShell.</span></span>
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a><span data-ttu-id="e855f-123">Rozhraní příkazového řádku Azure (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="e855f-123">Azure Command Line Interface (Azure CLI)</span></span>
<span data-ttu-id="e855f-124">Některá rozšíření mají příkazy rozhraní příkazového řádku Azure, které jsou specifické pro jejich (Docker rozšíření virtuálního počítače je jedním z příkladů), který může zjednodušit jejich konfigurace; ale pro následující příkazy pro všechna rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e855f-124">Some extensions have Azure CLI commands that are specific to them (the Docker VM Extension is one example), which may make their configuration easier; but the following commands work for all VM extensions.</span></span>

<span data-ttu-id="e855f-125">Můžete použít **seznamu rozšíření virtuálních počítačů azure** příkaz získat informace o dostupných rozšíření a používat **–-json** možnosti se zobrazí všechny dostupné informace o jeden nebo více rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e855f-125">You can use the **azure vm extension list** command to obtain information about available extensions, and use the **–-json** option to display all available information about one or more extensions.</span></span> <span data-ttu-id="e855f-126">Pokud použijete název rozšíření, příkaz vrátí JSON popis všech dostupných rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e855f-126">If you do not use an extension name, the command returns a JSON description of all available extensions.</span></span>

<span data-ttu-id="e855f-127">Například následující příklad kódu ukazuje, jak uvádí informace k **IaaSDiagnostics** rozšíření pomocí rozhraní příkazového řádku Azure **seznamu rozšíření virtuálních počítačů azure** příkaz a používá **–-json**  možnost vrátit úplné informace.</span><span class="sxs-lookup"><span data-stu-id="e855f-127">For example, the following code example shows how to list the information for the **IaaSDiagnostics** extension using the Azure CLI **azure vm extension list** command and uses the **–-json** option to return complete information.</span></span>

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a><span data-ttu-id="e855f-128">Rozhraní REST API pro správu služeb</span><span class="sxs-lookup"><span data-stu-id="e855f-128">Service Management REST APIs</span></span>
<span data-ttu-id="e855f-129">Následující rozhraní REST API můžete použít k získání informací o dostupných rozšíření:</span><span class="sxs-lookup"><span data-stu-id="e855f-129">You can use the following REST APIs to obtain information about available extensions:</span></span>

* <span data-ttu-id="e855f-130">Pro instance webové role nebo rolí pracovního procesu, můžete použít [seznam dostupných rozšíření](https://msdn.microsoft.com/library/dn169559.aspx) operaci.</span><span class="sxs-lookup"><span data-stu-id="e855f-130">For instances of web roles or worker roles, you can use the [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span> <span data-ttu-id="e855f-131">Seznam verzí rozšíření k dispozici, můžete použít [verze rozšíření seznamu](https://msdn.microsoft.com/library/dn495437.aspx).</span><span class="sxs-lookup"><span data-stu-id="e855f-131">To list the versions of available extensions, you can use [List Extension Versions](https://msdn.microsoft.com/library/dn495437.aspx).</span></span>
* <span data-ttu-id="e855f-132">Pro instance virtuálních počítačů, můžete použít [rozšíření prostředků seznamu](https://msdn.microsoft.com/library/dn495441.aspx) operaci.</span><span class="sxs-lookup"><span data-stu-id="e855f-132">For instances of Virtual Machines, you can use the [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span> <span data-ttu-id="e855f-133">Seznam verzí rozšíření k dispozici, můžete použít [verze rozšíření prostředků seznamu](https://msdn.microsoft.com/library/dn495440.aspx).</span><span class="sxs-lookup"><span data-stu-id="e855f-133">To list the versions of available extensions, you can use [List Resource Extension Versions](https://msdn.microsoft.com/library/dn495440.aspx).</span></span>

## <a name="add-update-or-disable-extensions"></a><span data-ttu-id="e855f-134">Přidání, aktualizace nebo zakázat rozšíření</span><span class="sxs-lookup"><span data-stu-id="e855f-134">Add, Update, or Disable Extensions</span></span>
<span data-ttu-id="e855f-135">Rozšíření můžete přidat, pokud je vytvořena instance nebo mohou být přidány do spuštěné instance.</span><span class="sxs-lookup"><span data-stu-id="e855f-135">Extensions can be added when an instance is created or they can be added to a running instance.</span></span> <span data-ttu-id="e855f-136">Rozšíření můžete aktualizovat, zakázat nebo odebrat.</span><span class="sxs-lookup"><span data-stu-id="e855f-136">Extensions can be updated, disabled, or removed.</span></span> <span data-ttu-id="e855f-137">Pomocí rutin prostředí Azure PowerShell nebo pomocí rozhraní API REST služby Management operace můžete provádět tyto akce.</span><span class="sxs-lookup"><span data-stu-id="e855f-137">You can perform these actions by using Azure PowerShell cmdlets or by using the Service Management REST API operations.</span></span> <span data-ttu-id="e855f-138">Parametry jsou potřebné k instalaci a nastavení některá rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e855f-138">Parameters are required to install and set up some extensions.</span></span> <span data-ttu-id="e855f-139">Veřejné a privátní parametry jsou podporovány pro rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e855f-139">Public and private parameters are supported for extensions.</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="e855f-140">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e855f-140">Azure PowerShell</span></span>
<span data-ttu-id="e855f-141">Pomocí rutin prostředí Azure PowerShell je nejjednodušší způsob, jak přidat a aktualizovat rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e855f-141">Using Azure PowerShell cmdlets is the easiest way to add and update extensions.</span></span> <span data-ttu-id="e855f-142">Při použití rutiny rozšíření velká část konfigurace rozšíření se provádí za vás.</span><span class="sxs-lookup"><span data-stu-id="e855f-142">When you use the extension cmdlets, most of the configuration of the extension is done for you.</span></span> <span data-ttu-id="e855f-143">Potřebujete v některých případech prostřednictvím kódu programu přidat rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e855f-143">At times, you may need to programmatically add an extension.</span></span> <span data-ttu-id="e855f-144">Pokud budete potřebovat k tomu, je nutné zadat konfiguraci rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e855f-144">When you need to do this, you must provide the configuration of the extension.</span></span>

<span data-ttu-id="e855f-145">Potřebujete vědět, zda rozšíření vyžaduje konfiguraci veřejné a privátní parametrů můžete použít následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="e855f-145">You can use the following cmdlets to know whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="e855f-146">Pro instance webové role nebo rolí pracovního procesu, můžete použít **Get-AzureServiceAvailableExtension** rutiny.</span><span class="sxs-lookup"><span data-stu-id="e855f-146">For instances of web roles or worker roles, you can use the **Get-AzureServiceAvailableExtension** cmdlet.</span></span>
* <span data-ttu-id="e855f-147">Pro instance virtuálních počítačů, můžete použít **Get-AzureVMAvailableExtension** rutiny.</span><span class="sxs-lookup"><span data-stu-id="e855f-147">For instances of Virtual Machines, you can use the **Get-AzureVMAvailableExtension** cmdlet.</span></span>

### <a name="service-management-rest-apis"></a><span data-ttu-id="e855f-148">Rozhraní REST API pro správu služeb</span><span class="sxs-lookup"><span data-stu-id="e855f-148">Service Management REST APIs</span></span>
<span data-ttu-id="e855f-149">Když je načíst seznam dostupných rozšíření pomocí rozhraní REST API, zobrazí se informace o tom, jak rozšíření nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="e855f-149">When you retrieve a listing of available extensions by using the REST APIs, you receive information about how the extension is to be configured.</span></span> <span data-ttu-id="e855f-150">Informace, které se vrátí může zobrazit informace o parametrech reprezentována na schéma veřejné a privátní schématu.</span><span class="sxs-lookup"><span data-stu-id="e855f-150">The information that is returned might show parameter information represented by a public schema and private schema.</span></span> <span data-ttu-id="e855f-151">Veřejné parametr hodnoty jsou vráceny v dotazech o instance.</span><span class="sxs-lookup"><span data-stu-id="e855f-151">Public parameter values are returned in queries about the instances.</span></span> <span data-ttu-id="e855f-152">Hodnoty parametru privátní nebudou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="e855f-152">Private parameter values are not returned.</span></span>

<span data-ttu-id="e855f-153">Potřebujete vědět, zda rozšíření vyžaduje konfiguraci veřejné a privátní parametrů můžete použít následující rozhraní REST API:</span><span class="sxs-lookup"><span data-stu-id="e855f-153">You can use the following REST APIs to know whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="e855f-154">Pro instance webové role nebo rolí pracovního procesu **PublicConfigurationSchema** a **PrivateConfigurationSchema** elementy obsahovat informace v odpovědi z [seznamu dostupných Rozšíření](https://msdn.microsoft.com/library/dn169559.aspx) operaci.</span><span class="sxs-lookup"><span data-stu-id="e855f-154">For instances of web roles or worker roles, the **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain the information in the response from the [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span>
* <span data-ttu-id="e855f-155">Pro instance virtuálních počítačů, **PublicConfigurationSchema** a **PrivateConfigurationSchema** elementy obsahovat informace v odpověď [seznamu prostředků Rozšíření](https://msdn.microsoft.com/library/dn495441.aspx) operaci.</span><span class="sxs-lookup"><span data-stu-id="e855f-155">For instances of Virtual Machines, the **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain the information in the response from the [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span>

> [!NOTE]
> <span data-ttu-id="e855f-156">Rozšíření můžete také použít konfigurace, které jsou definovány s JSON.</span><span class="sxs-lookup"><span data-stu-id="e855f-156">Extensions can also use configurations that are defined with JSON.</span></span> <span data-ttu-id="e855f-157">Pokud tyto typy rozšíření se používají, jenom **SampleConfig** element se používá.</span><span class="sxs-lookup"><span data-stu-id="e855f-157">When these types of extensions are used, only the **SampleConfig** element is used.</span></span>
> 
> 

