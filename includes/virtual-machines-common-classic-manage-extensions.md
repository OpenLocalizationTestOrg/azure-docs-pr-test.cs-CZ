


## <a name="using-vm-extensions"></a><span data-ttu-id="2f7eb-101">Pomocí rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2f7eb-101">Using VM Extensions</span></span>
<span data-ttu-id="2f7eb-102">Rozšíření virtuálního počítače Azure implementovat chování nebo funkce, které buď další programy fungovat na virtuálních počítačích Azure (například hello **WebDeployForVSDevTest** rozšíření umožňuje sadě Visual Studio tooWeb nasadit řešení na vašem virtuálním počítači Azure) nebo zadejte Hello schopnost toointeract s hello virtuálních počítačů toosupport některé jiné chování (například vy můžete používat rozšíření hello přístup virtuálních počítačů z prostředí PowerShell, hello rozhraní příkazového řádku Azure a tooreset klienti REST nebo upravte hodnoty vzdáleného přístupu na vašem virtuálním počítači Azure).</span><span class="sxs-lookup"><span data-stu-id="2f7eb-102">Azure VM Extensions implement behaviors or features that either help other programs work on Azure VMs (for example, hello **WebDeployForVSDevTest** extension allows Visual Studio tooWeb Deploy solutions on your Azure VM) or provide hello ability for you toointeract with hello VM toosupport some other behavior (for example, you can use hello VM Access extensions from PowerShell, hello Azure CLI, and REST clients tooreset or modify remote access values on your Azure VM).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2f7eb-103">Úplný seznam rozšíření hello funkce podporují, najdete v části [rozšíření virtuálního počítače Azure a funkce](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2f7eb-103">For a complete list of extensions by hello features they support, see [Azure VM Extensions and Features](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="2f7eb-104">Protože každý rozšíření virtuálního počítače podporuje konkrétní funkci, přesně co můžete a nemůžete dělat s příponou závisí na hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-104">Because each VM extension supports a specific feature, exactly what you can and cannot do with an extension depends on hello extension.</span></span> <span data-ttu-id="2f7eb-105">Proto před změnou virtuálního počítače, ujistěte se, že jste četli hello dokumentaci hello chcete toouse rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-105">Therefore, before modifying your VM, make sure you have read hello documentation for hello VM Extension you want toouse.</span></span> <span data-ttu-id="2f7eb-106">Odebírání některé rozšíření virtuálního počítače není podporováno; ostatní uživatelé mají vlastnosti, které lze nastavit, které výrazně mění chování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-106">Removing some VM Extensions is not supported; others have properties that can be set that change VM behavior radically.</span></span>
> 
> 

<span data-ttu-id="2f7eb-107">Hello běžné úkoly, jsou:</span><span class="sxs-lookup"><span data-stu-id="2f7eb-107">hello most common tasks are:</span></span>

1. <span data-ttu-id="2f7eb-108">Hledání dostupných rozšíření</span><span class="sxs-lookup"><span data-stu-id="2f7eb-108">Finding Available Extensions</span></span>
2. <span data-ttu-id="2f7eb-109">Aktualizace načíst rozšíření</span><span class="sxs-lookup"><span data-stu-id="2f7eb-109">Updating Loaded Extensions</span></span>
3. <span data-ttu-id="2f7eb-110">Přidání rozšíření</span><span class="sxs-lookup"><span data-stu-id="2f7eb-110">Adding Extensions</span></span>
4. <span data-ttu-id="2f7eb-111">Odebrání rozšíření</span><span class="sxs-lookup"><span data-stu-id="2f7eb-111">Removing Extensions</span></span>

## <a name="find-available-extensions"></a><span data-ttu-id="2f7eb-112">Najít dostupných rozšíření</span><span class="sxs-lookup"><span data-stu-id="2f7eb-112">Find Available Extensions</span></span>
<span data-ttu-id="2f7eb-113">Můžete vyhledat rozšíření a použití rozšířené informace:</span><span class="sxs-lookup"><span data-stu-id="2f7eb-113">You can locate extension and extended information using:</span></span>

* <span data-ttu-id="2f7eb-114">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f7eb-114">PowerShell</span></span>
* <span data-ttu-id="2f7eb-115">Rozhraní Azure napříč platformami příkazového řádku (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="2f7eb-115">Azure Cross-Platform Command Line Interface (Azure CLI)</span></span>
* <span data-ttu-id="2f7eb-116">Rozhraní REST API pro správu služeb</span><span class="sxs-lookup"><span data-stu-id="2f7eb-116">Service Management REST API</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="2f7eb-117">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f7eb-117">Azure PowerShell</span></span>
<span data-ttu-id="2f7eb-118">Některá rozšíření mít rutiny prostředí PowerShell, které jsou specifické toothem, což může zjednodušit jejich konfigurace z prostředí PowerShell; ale hello následující rutiny fungovat pro všechny rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-118">Some extensions have PowerShell cmdlets that are specific toothem, which may make their configuration from PowerShell easier; but hello following cmdlets work for all VM extensions.</span></span>

<span data-ttu-id="2f7eb-119">Můžete použít následující rutiny tooobtain informace o dostupných rozšíření hello:</span><span class="sxs-lookup"><span data-stu-id="2f7eb-119">You can use hello following cmdlets tooobtain information about available extensions:</span></span>

* <span data-ttu-id="2f7eb-120">Pro instance webové role nebo rolí pracovního procesu, můžete použít hello [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-120">For instances of web roles or worker roles, you can use hello [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet.</span></span>
* <span data-ttu-id="2f7eb-121">Pro instance virtuálních počítačů, můžete použít hello [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-121">For instances of Virtual Machines, you can use hello [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet.</span></span>
  
   <span data-ttu-id="2f7eb-122">Například hello následující příklad ukazuje kód jak toolist informace pro hello **IaaSDiagnostics** rozšíření pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-122">For example, hello following code example shows how toolist the information for hello **IaaSDiagnostics** extension using PowerShell.</span></span>
  
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

### <a name="azure-command-line-interface-azure-cli"></a><span data-ttu-id="2f7eb-123">Rozhraní příkazového řádku Azure (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="2f7eb-123">Azure Command Line Interface (Azure CLI)</span></span>
<span data-ttu-id="2f7eb-124">Některá rozšíření mají příkazy rozhraní příkazového řádku Azure, které jsou konkrétní toothem (hello Docker rozšíření virtuálního počítače je jedním z příkladů), který může zjednodušit jejich konfigurace; ale hello následující příkazy práci pro všechna rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-124">Some extensions have Azure CLI commands that are specific toothem (hello Docker VM Extension is one example), which may make their configuration easier; but hello following commands work for all VM extensions.</span></span>

<span data-ttu-id="2f7eb-125">Můžete použít hello **seznamu rozšíření virtuálních počítačů azure** příkaz tooobtain informace o dostupných rozšíření a použijte hello **–-json** možnost toodisplay všechny dostupné informace o jeden nebo více rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-125">You can use hello **azure vm extension list** command tooobtain information about available extensions, and use hello **–-json** option toodisplay all available information about one or more extensions.</span></span> <span data-ttu-id="2f7eb-126">Pokud použijete název rozšíření, vrátí příkaz hello JSON popis všech dostupných rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-126">If you do not use an extension name, hello command returns a JSON description of all available extensions.</span></span>

<span data-ttu-id="2f7eb-127">Například hello následující příklad kódu ukazuje, jak toolist hello informace pro hello **IaaSDiagnostics** rozšíření pomocí rozhraní příkazového řádku Azure hello **seznamu rozšíření virtuálních počítačů azure** příkazu a použití hello **–-json** možnost tooreturn úplné informace.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-127">For example, hello following code example shows how toolist hello information for hello **IaaSDiagnostics** extension using hello Azure CLI **azure vm extension list** command and uses hello **–-json** option tooreturn complete information.</span></span>

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



### <a name="service-management-rest-apis"></a><span data-ttu-id="2f7eb-128">Rozhraní REST API pro správu služeb</span><span class="sxs-lookup"><span data-stu-id="2f7eb-128">Service Management REST APIs</span></span>
<span data-ttu-id="2f7eb-129">Můžete použít následující rozhraní REST API tooobtain informace o dostupných rozšíření hello:</span><span class="sxs-lookup"><span data-stu-id="2f7eb-129">You can use hello following REST APIs tooobtain information about available extensions:</span></span>

* <span data-ttu-id="2f7eb-130">Pro instance webové role nebo rolí pracovního procesu, můžete použít hello [seznam dostupných rozšíření](https://msdn.microsoft.com/library/dn169559.aspx) operaci.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-130">For instances of web roles or worker roles, you can use hello [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span> <span data-ttu-id="2f7eb-131">verze hello toolist rozšíření k dispozici, můžete použít [verze rozšíření seznamu](https://msdn.microsoft.com/library/dn495437.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f7eb-131">toolist hello versions of available extensions, you can use [List Extension Versions](https://msdn.microsoft.com/library/dn495437.aspx).</span></span>
* <span data-ttu-id="2f7eb-132">Pro instance virtuálních počítačů, můžete použít hello [rozšíření prostředků seznamu](https://msdn.microsoft.com/library/dn495441.aspx) operaci.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-132">For instances of Virtual Machines, you can use hello [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span> <span data-ttu-id="2f7eb-133">verze hello toolist rozšíření k dispozici, můžete použít [verze rozšíření prostředků seznamu](https://msdn.microsoft.com/library/dn495440.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f7eb-133">toolist hello versions of available extensions, you can use [List Resource Extension Versions](https://msdn.microsoft.com/library/dn495440.aspx).</span></span>

## <a name="add-update-or-disable-extensions"></a><span data-ttu-id="2f7eb-134">Přidání, aktualizace nebo zakázat rozšíření</span><span class="sxs-lookup"><span data-stu-id="2f7eb-134">Add, Update, or Disable Extensions</span></span>
<span data-ttu-id="2f7eb-135">Rozšíření můžete přidat, pokud je vytvořena instance nebo mohou být přidány tooa spuštěna instance.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-135">Extensions can be added when an instance is created or they can be added tooa running instance.</span></span> <span data-ttu-id="2f7eb-136">Rozšíření můžete aktualizovat, zakázat nebo odebrat.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-136">Extensions can be updated, disabled, or removed.</span></span> <span data-ttu-id="2f7eb-137">Tyto akce můžete provést pomocí rutin prostředí Azure PowerShell nebo pomocí operace REST API pro správu služby hello.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-137">You can perform these actions by using Azure PowerShell cmdlets or by using hello Service Management REST API operations.</span></span> <span data-ttu-id="2f7eb-138">Parametry jsou požadované tooinstall a nastavit některá rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-138">Parameters are required tooinstall and set up some extensions.</span></span> <span data-ttu-id="2f7eb-139">Veřejné a privátní parametry jsou podporovány pro rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-139">Public and private parameters are supported for extensions.</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="2f7eb-140">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f7eb-140">Azure PowerShell</span></span>
<span data-ttu-id="2f7eb-141">Pomocí rutin prostředí Azure PowerShell je hello nejjednodušší způsob, jak tooadd a aktualizace rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-141">Using Azure PowerShell cmdlets is hello easiest way tooadd and update extensions.</span></span> <span data-ttu-id="2f7eb-142">Při použití rutiny rozšíření hello většinu hello konfigurace rozšíření hello se provádí za vás.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-142">When you use hello extension cmdlets, most of hello configuration of hello extension is done for you.</span></span> <span data-ttu-id="2f7eb-143">V některých případech může být nutné tooprogrammatically přidat rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-143">At times, you may need tooprogrammatically add an extension.</span></span> <span data-ttu-id="2f7eb-144">Pokud tuto funkci potřebujete toodo, je nutné zadat hello konfigurace rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-144">When you need toodo this, you must provide hello configuration of hello extension.</span></span>

<span data-ttu-id="2f7eb-145">Můžete použít následující rutiny tooknow, zda rozšíření vyžaduje konfiguraci parametrů veřejné a privátní hello:</span><span class="sxs-lookup"><span data-stu-id="2f7eb-145">You can use hello following cmdlets tooknow whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="2f7eb-146">Pro instance webové role nebo rolí pracovního procesu, můžete použít hello **Get-AzureServiceAvailableExtension** rutiny.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-146">For instances of web roles or worker roles, you can use hello **Get-AzureServiceAvailableExtension** cmdlet.</span></span>
* <span data-ttu-id="2f7eb-147">Pro instance virtuálních počítačů, můžete použít hello **Get-AzureVMAvailableExtension** rutiny.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-147">For instances of Virtual Machines, you can use hello **Get-AzureVMAvailableExtension** cmdlet.</span></span>

### <a name="service-management-rest-apis"></a><span data-ttu-id="2f7eb-148">Rozhraní REST API pro správu služeb</span><span class="sxs-lookup"><span data-stu-id="2f7eb-148">Service Management REST APIs</span></span>
<span data-ttu-id="2f7eb-149">Když načtete seznam dostupných rozšíření pomocí hello rozhraní REST API, zobrazí se informace o tom, jak rozšíření hello toobe nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-149">When you retrieve a listing of available extensions by using hello REST APIs, you receive information about how hello extension is toobe configured.</span></span> <span data-ttu-id="2f7eb-150">vrácené informace Hello může zobrazit informace o parametrech reprezentována na schéma veřejné a privátní schématu.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-150">hello information that is returned might show parameter information represented by a public schema and private schema.</span></span> <span data-ttu-id="2f7eb-151">Veřejné parametr hodnoty jsou vráceny v dotazech o instancích hello.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-151">Public parameter values are returned in queries about hello instances.</span></span> <span data-ttu-id="2f7eb-152">Hodnoty parametru privátní nebudou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-152">Private parameter values are not returned.</span></span>

<span data-ttu-id="2f7eb-153">Můžete použít následující tooknow rozhraní REST API, zda rozšíření vyžaduje konfiguraci parametrů veřejné a privátní hello:</span><span class="sxs-lookup"><span data-stu-id="2f7eb-153">You can use hello following REST APIs tooknow whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="2f7eb-154">Pro instance webové role nebo rolí pracovního procesu, hello **PublicConfigurationSchema** a **PrivateConfigurationSchema** elementy obsahují informace hello hello odezvy z hello [seznamu Dostupná rozšíření](https://msdn.microsoft.com/library/dn169559.aspx) operaci.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-154">For instances of web roles or worker roles, hello **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain hello information in hello response from hello [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span>
* <span data-ttu-id="2f7eb-155">Pro instance virtuálních počítačů, hello **PublicConfigurationSchema** a **PrivateConfigurationSchema** elementy obsahují informace hello hello odezvy z hello [seznamu Rozšíření prostředků](https://msdn.microsoft.com/library/dn495441.aspx) operaci.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-155">For instances of Virtual Machines, hello **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain hello information in hello response from hello [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span>

> [!NOTE]
> <span data-ttu-id="2f7eb-156">Rozšíření můžete také použít konfigurace, které jsou definovány s JSON.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-156">Extensions can also use configurations that are defined with JSON.</span></span> <span data-ttu-id="2f7eb-157">Pokud tyto typy rozšíření používají, jenom hello **SampleConfig** element se používá.</span><span class="sxs-lookup"><span data-stu-id="2f7eb-157">When these types of extensions are used, only hello **SampleConfig** element is used.</span></span>
> 
> 

