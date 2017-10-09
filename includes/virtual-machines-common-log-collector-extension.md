
<span data-ttu-id="b6a70-101">Diagnostika problémů s cloudové služby Microsoft Azure vyžaduje shromažďování souborů protokolů služby hello na virtuální počítače jsou prováděny hello problémy.</span><span class="sxs-lookup"><span data-stu-id="b6a70-101">Diagnosing issues with an Microsoft Azure cloud service requires collecting hello service’s log files on virtual machines as hello issues occur.</span></span> <span data-ttu-id="b6a70-102">Můžete použít hello AzureLogCollector rozšíření na vyžádání tooperfom jednorázové shromažďování protokolů z jedné nebo více virtuálních počítačů služby cloudu (z webových rolí a rolí pracovního procesu) a přenos hello shromážděné soubory tooan účtu úložiště Azure – všechny bez vzdálené přihlášení tooany hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b6a70-102">You can use hello AzureLogCollector extension on-demand tooperfom one-time collection of logs from one or more Cloud Service VMs (from both web roles and worker roles) and transfer hello collected files tooan Azure storage account – all without remotely logging on tooany of hello VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="b6a70-103">Popisy pro většinu hello protokolovat informace lze najít na http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.</span><span class="sxs-lookup"><span data-stu-id="b6a70-103">Descriptions for most of hello logged information can be found at http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.</span></span>
> 
> 

<span data-ttu-id="b6a70-104">Existují dva režimy kolekce závisí na typech hello toobe soubory shromážděny.</span><span class="sxs-lookup"><span data-stu-id="b6a70-104">There are two modes of collection dependent on hello types of files toobe collected.</span></span>

* <span data-ttu-id="b6a70-105">Azure hostovaného agenta protokoly pouze (GA).</span><span class="sxs-lookup"><span data-stu-id="b6a70-105">Azure Guest Agent Logs only (GA).</span></span> <span data-ttu-id="b6a70-106">Tento režim kolekce zahrnuje všechny agenty hostů související tooAzure hello protokoly a dalšími součástmi Azure.</span><span class="sxs-lookup"><span data-stu-id="b6a70-106">This collection mode includes all hello logs related tooAzure guest agents and other Azure components.</span></span>
* <span data-ttu-id="b6a70-107">Všechny protokoly (úplná záloha).</span><span class="sxs-lookup"><span data-stu-id="b6a70-107">All Logs (Full).</span></span> <span data-ttu-id="b6a70-108">Tento režim kolekce bude shromažďovat všechny soubory v režimu GA plus:</span><span class="sxs-lookup"><span data-stu-id="b6a70-108">This collection mode will collect all files in GA mode plus:</span></span>
  
  * <span data-ttu-id="b6a70-109">protokoly událostí systému a aplikací</span><span class="sxs-lookup"><span data-stu-id="b6a70-109">system and application event logs</span></span>
  * <span data-ttu-id="b6a70-110">Protokoly chyb HTTP</span><span class="sxs-lookup"><span data-stu-id="b6a70-110">HTTP error logs</span></span>
  * <span data-ttu-id="b6a70-111">Protokoly služby IIS</span><span class="sxs-lookup"><span data-stu-id="b6a70-111">IIS Logs</span></span>
  * <span data-ttu-id="b6a70-112">Protokoly instalace</span><span class="sxs-lookup"><span data-stu-id="b6a70-112">Setup logs</span></span>
  * <span data-ttu-id="b6a70-113">Další systémové protokoly</span><span class="sxs-lookup"><span data-stu-id="b6a70-113">other system logs</span></span>

<span data-ttu-id="b6a70-114">V obou režimech kolekce složky kolekcí další data lze pomocí kolekce hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="b6a70-114">In both collection modes, additional data collection folders can be specified by using a collection of hello following structure:</span></span>

* <span data-ttu-id="b6a70-115">**Název**: název hello hello kolekce, která se použije jako hello název podsložky uvnitř toobe soubor zip hello shromažďují.</span><span class="sxs-lookup"><span data-stu-id="b6a70-115">**Name**: hello name of hello collection, which will be used as hello name of subfolder inside hello zip file toobe collected.</span></span>
* <span data-ttu-id="b6a70-116">**Umístění**: hello cesta toohello složky na virtuálním počítači hello kde budou shromažďovány souboru.</span><span class="sxs-lookup"><span data-stu-id="b6a70-116">**Location**: hello path toohello folder on hello virtual machine where file will be collected.</span></span>
* <span data-ttu-id="b6a70-117">**SearchPattern**: hello vzor názvů hello toobe soubory shromážděny.</span><span class="sxs-lookup"><span data-stu-id="b6a70-117">**SearchPattern**: hello pattern of hello names of files toobe collected.</span></span> <span data-ttu-id="b6a70-118">Výchozí hodnota je "*"</span><span class="sxs-lookup"><span data-stu-id="b6a70-118">Default is “*”</span></span>
* <span data-ttu-id="b6a70-119">**Rekurzivní**: Pokud budou soubory hello shromážděných rekurzivně složce hello.</span><span class="sxs-lookup"><span data-stu-id="b6a70-119">**Recursive**: if hello files will be collected recursively under hello folder.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6a70-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b6a70-120">Prerequisites</span></span>
* <span data-ttu-id="b6a70-121">Potřebujete toohave účet úložiště pro soubory zip toosave generované rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b6a70-121">You need toohave a storage account for extension toosave generated zip files.</span></span>
* <span data-ttu-id="b6a70-122">Ujistěte se, že používáte V0.8.0 rutiny Azure PowerShell nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="b6a70-122">You must make sure that you are using Azure PowerShell Cmdlets V0.8.0 or above.</span></span> <span data-ttu-id="b6a70-123">Další informace najdete v tématu [Azure stáhne](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="b6a70-123">For more information, see [Azure Downloads](https://azure.microsoft.com/downloads/).</span></span>

## <a name="add-hello-extension"></a><span data-ttu-id="b6a70-124">Přidat rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="b6a70-124">Add hello extension</span></span>
<span data-ttu-id="b6a70-125">Můžete použít [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) rutiny nebo [rozhraní API REST pro správu služby](https://msdn.microsoft.com/library/ee460799.aspx) tooadd hello AzureLogCollector rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b6a70-125">You can use [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) cmdlets or [Service Management REST APIs](https://msdn.microsoft.com/library/ee460799.aspx) tooadd hello AzureLogCollector extension.</span></span>

<span data-ttu-id="b6a70-126">Pro cloudové služby, hello existující rutiny Azure Powershellu, **Set-AzureServiceExtension**, může být použité tooenable hello rozšíření u instancí role cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b6a70-126">For Cloud Services, hello existing Azure Powershell cmdlet, **Set-AzureServiceExtension**, can be used tooenable hello extension on Cloud Service role instances.</span></span> <span data-ttu-id="b6a70-127">Pokaždé, když je toto rozšíření povolené prostřednictvím této rutiny, aktivuje se u instancí role hello vybrané vybraných rolí shromáždění protokolů.</span><span class="sxs-lookup"><span data-stu-id="b6a70-127">Every time this extension is enabled through this cmdlet, log collection is triggered on hello selected role instances of selected roles.</span></span>

<span data-ttu-id="b6a70-128">Pro virtuální počítače, hello existující rutiny Azure Powershellu, **Set-AzureVMExtension**, může být rozšíření hello tooenable používané virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="b6a70-128">For Virtual Machines, hello existing Azure Powershell cmdlet, **Set-AzureVMExtension**, can be used tooenable hello extension on Virtual Machines.</span></span> <span data-ttu-id="b6a70-129">Pokaždé, když je toto rozšíření povolené pomocí rutin hello, aktivuje se na každou instanci shromáždění protokolů.</span><span class="sxs-lookup"><span data-stu-id="b6a70-129">Every time this extension is enabled through hello cmdlets, log collection is triggered on each instance.</span></span>

<span data-ttu-id="b6a70-130">Toto rozšíření interně používá hello na základě JSON PublicConfiguration a PrivateConfiguration.</span><span class="sxs-lookup"><span data-stu-id="b6a70-130">Internally, this extension uses hello JSON-based PublicConfiguration and PrivateConfiguration.</span></span> <span data-ttu-id="b6a70-131">Hello následuje hello rozložení ukázkové JSON pro veřejné a privátní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b6a70-131">hello following is hello layout of a sample JSON for public and private configuration.</span></span>

### <a name="publicconfiguration"></a><span data-ttu-id="b6a70-132">PublicConfiguration</span><span class="sxs-lookup"><span data-stu-id="b6a70-132">PublicConfiguration</span></span>
    {
        "Instances":  "*",
        "Mode":  "Full",
        "SasUri":  "SasUri tooyour storage account with sp=wl",
        "AdditionalData":
        [
          {
                  "Name":  "StorageData",
                  "Location":  "%roleroot%storage",
                  "SearchPattern":  "*.*",
                  "Recursive":  "true"
          },
          {
                "Name":  "CustomDataFolder2",
                "Location":  "c:\customFolder",
                "SearchPattern":  "*.log",
                "Recursive":  "false"
          },
        ]
    }

### <a name="privateconfiguration"></a><span data-ttu-id="b6a70-133">PrivateConfiguration</span><span class="sxs-lookup"><span data-stu-id="b6a70-133">PrivateConfiguration</span></span>
    {

    }

> [!NOTE]
> <span data-ttu-id="b6a70-134">Toto rozšíření nevyžaduje **privateConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="b6a70-134">This extension doesn’t need **privateConfiguration**.</span></span> <span data-ttu-id="b6a70-135">Můžete jenom zadat prázdnou strukturou hello **– PrivateConfiguration** argument.</span><span class="sxs-lookup"><span data-stu-id="b6a70-135">You can just provide an empty structure for hello **–PrivateConfiguration** argument.</span></span>
> 
> 

<span data-ttu-id="b6a70-136">Proveďte jeden z hello dvě následující kroky tooadd hello AzureLogCollector tooone nebo více instancí služeb Cloud nebo virtuální počítač z vybrané role, které aktivuje hello kolekce na každý počítač toorun a odesílat hello shromážděné soubory tooAzure účtu zadat.</span><span class="sxs-lookup"><span data-stu-id="b6a70-136">You can follow one of hello two following steps tooadd hello AzureLogCollector tooone or more instances of a Cloud Service or Virtual Machine of selected roles, which triggers hello collections on each VM toorun and send hello collected files tooAzure account specified.</span></span>

## <a name="adding-as-a-service-extension"></a><span data-ttu-id="b6a70-137">Přidání jako rozšíření služby</span><span class="sxs-lookup"><span data-stu-id="b6a70-137">Adding as a Service Extension</span></span>
1. <span data-ttu-id="b6a70-138">Postupujte podle hello pokyny tooconnect prostředí Azure PowerShell tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="b6a70-138">Follow hello instructions tooconnect Azure PowerShell tooyour subscription.</span></span>
2. <span data-ttu-id="b6a70-139">Zadejte název služby hello, slotu, role a role instance toowhich chcete tooadd a povolit rozšíření AzureLogCollector hello.</span><span class="sxs-lookup"><span data-stu-id="b6a70-139">Specify hello service name, slot, roles, and role instances toowhich you want tooadd and enable hello AzureLogCollector extension.</span></span>
   
        #Specify your cloud service name
        $ServiceName = 'extensiontest2'
   
        #Specify hello slot. 'Production' or 'Staging'
        $slot = 'Production'
   
        #Specified hello roles on which hello extension will be installed and enabled
        $roles = @("WorkerRole1","WebRole1")
   
        #Specify hello instances on which extension will be installed and enabled.  Use wildcard * for all instances
        $instances = @("*")
   
        #Specify hello collection mode, "Full" or "GA"
        $mode = "GA"
3. <span data-ttu-id="b6a70-140">Zadejte složku hello další data, pro které se budou shromažďovat soubory (Tento krok je volitelný).</span><span class="sxs-lookup"><span data-stu-id="b6a70-140">Specify hello additional data folder for which files will be collected (this step is optional).</span></span>
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
   
   > [!NOTE]
   > <span data-ttu-id="b6a70-141">Můžete použít token `%roleroot%` toospecify hello role kořenové jednotce vzhledem k tomu nepoužívá pevnou jednotku.</span><span class="sxs-lookup"><span data-stu-id="b6a70-141">You can use token `%roleroot%` toospecify hello role root drive since it doesn’t use a fixed drive.</span></span>
   > 
   > 
4. <span data-ttu-id="b6a70-142">Zadejte název účtu úložiště Azure hello a klíče toowhich shromážděné soubory budou odeslány.</span><span class="sxs-lookup"><span data-stu-id="b6a70-142">Provide hello Azure storage account name and key toowhich collected files will be uploaded.</span></span>
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
5. <span data-ttu-id="b6a70-143">Volání hello SetAzureServiceLogCollector.ps1 (zahrnutá na konci hello tohoto článku hello) jako způsobem tooenable hello AzureLogCollector rozšíření pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b6a70-143">Call hello SetAzureServiceLogCollector.ps1 (included at hello end of hello article) as follows tooenable hello AzureLogCollector extension for a Cloud Service.</span></span> <span data-ttu-id="b6a70-144">Po dokončení provádění hello hello nahrát soubor naleznete v části`https://YouareStorageAccountName.blob.core.windows.net/vmlogs`</span><span class="sxs-lookup"><span data-stu-id="b6a70-144">Once hello execution is completed, you can find hello uploaded file under `https://YouareStorageAccountName.blob.core.windows.net/vmlogs`</span></span>
   
        .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList

<span data-ttu-id="b6a70-145">Hello následuje hello Definice hello parametry předané toohello skriptu.</span><span class="sxs-lookup"><span data-stu-id="b6a70-145">hello following is hello definition of hello parameters passed toohello script.</span></span> <span data-ttu-id="b6a70-146">(To je zkopírovat níže také.)</span><span class="sxs-lookup"><span data-stu-id="b6a70-146">(This is copied below as well.)</span></span>

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
      [Parameter(Mandatory=$true)]
    [string]   $ServiceName,

    [Parameter(Mandatory=$false)]
    [string[]] $Roles ,

    [Parameter(Mandatory=$false)]
    [string[]] $Instances,

    [Parameter(Mandatory=$false)]
    [string]   $Slot = 'Production',

    [Parameter(Mandatory=$false)]
    [string]   $Mode = 'Full',

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountName,

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountKey,

    [Parameter(Mandatory=$false)]
    [PSObject[]] $AdditionDataLocationList = $null
    )

* <span data-ttu-id="b6a70-147">*ServiceName*: název vaší cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b6a70-147">*ServiceName*: Your cloud service name.</span></span>
* <span data-ttu-id="b6a70-148">*Role*: seznam rolí, například "WebRole1" nebo "WorkerRole1".</span><span class="sxs-lookup"><span data-stu-id="b6a70-148">*Roles*: A list of roles, such as “WebRole1” or ”WorkerRole1”.</span></span>
* <span data-ttu-id="b6a70-149">*Instance*: seznam názvů hello instancí role oddělené čárkou – použijte hello zástupný řetězec ("*") pro všechny instance role.</span><span class="sxs-lookup"><span data-stu-id="b6a70-149">*Instances*: A list of hello names of role instances separated by comma -- use hello wildcard string (“*”) for all role instances.</span></span>
* <span data-ttu-id="b6a70-150">*Slot*: název slotu.</span><span class="sxs-lookup"><span data-stu-id="b6a70-150">*Slot*: Slot name.</span></span> <span data-ttu-id="b6a70-151">"Výroba" nebo "Přípravy".</span><span class="sxs-lookup"><span data-stu-id="b6a70-151">“Production” or “Staging”.</span></span>
* <span data-ttu-id="b6a70-152">*Režim*: režim kolekce.</span><span class="sxs-lookup"><span data-stu-id="b6a70-152">*Mode*: Collection mode.</span></span> <span data-ttu-id="b6a70-153">"Úplná" nebo "GA".</span><span class="sxs-lookup"><span data-stu-id="b6a70-153">“Full” or “GA”.</span></span>
* <span data-ttu-id="b6a70-154">*StorageAccountName*: název Azure účet úložiště pro ukládání shromážděných dat.</span><span class="sxs-lookup"><span data-stu-id="b6a70-154">*StorageAccountName*: Name of Azure storage account for storing collected data.</span></span>
* <span data-ttu-id="b6a70-155">*StorageAccountKey*: název Azure klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b6a70-155">*StorageAccountKey*: Name of Azure storage account key.</span></span>
* <span data-ttu-id="b6a70-156">*AdditionalDataLocationList*: seznam hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="b6a70-156">*AdditionalDataLocationList*: A list of hello following structure:</span></span>
  
      {
      String Name,
      String Location,
      String SearchPattern,
      Bool   Recursive
      }

## <a name="adding-as-a-vm-extension"></a><span data-ttu-id="b6a70-157">Přidání jako rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b6a70-157">Adding as a VM Extension</span></span>
<span data-ttu-id="b6a70-158">Postupujte podle hello pokyny tooconnect prostředí Azure PowerShell tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="b6a70-158">Follow hello instructions tooconnect Azure PowerShell tooyour subscription.</span></span>

1. <span data-ttu-id="b6a70-159">Zadejte název služby hello, virtuálních počítačů a režim kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="b6a70-159">Specify hello service name, VM, and hello collection mode.</span></span>
   
        #Specify your cloud service name
        $ServiceName = 'YourCloudServiceName'
   
        #Specify hello VM name
        $VMName = "'YourVMName'"
   
        #Specify hello collection mode, "Full" or "GA"
        $mode = "GA"
   
        Specify hello additional data folder for which files will be collected (this step is optional).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
2. <span data-ttu-id="b6a70-160">Zadejte název účtu úložiště Azure hello a klíče toowhich shromážděné soubory budou odeslány.</span><span class="sxs-lookup"><span data-stu-id="b6a70-160">Provide hello Azure storage account name and key toowhich collected files will be uploaded.</span></span>
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
3. <span data-ttu-id="b6a70-161">Volání hello SetAzureVMLogCollector.ps1 (zahrnutá na konci hello tohoto článku hello) jako způsobem tooenable hello AzureLogCollector rozšíření pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b6a70-161">Call hello SetAzureVMLogCollector.ps1 (included at hello end of hello article) as follows tooenable hello AzureLogCollector extension for a Cloud Service.</span></span> <span data-ttu-id="b6a70-162">Po dokončení provádění hello můžete najít v souboru hello nahrán pod https://YouareStorageAccountName.blob.core.windows.net/vmlogs</span><span class="sxs-lookup"><span data-stu-id="b6a70-162">Once hello execution is completed, you can find hello uploaded file under https://YouareStorageAccountName.blob.core.windows.net/vmlogs</span></span>

<span data-ttu-id="b6a70-163">Hello následuje hello Definice hello parametry předané toohello skriptu.</span><span class="sxs-lookup"><span data-stu-id="b6a70-163">hello following is hello definition of hello parameters passed toohello script.</span></span> <span data-ttu-id="b6a70-164">(To je zkopírovat níže také.)</span><span class="sxs-lookup"><span data-stu-id="b6a70-164">(This is copied below as well.)</span></span>

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
        [Parameter(Mandatory=$true)]
      [string]   $ServiceName,

      [Parameter(Mandatory=$false)]
      [string] $VMName ,

        [Parameter(Mandatory=$false)]
      [string]   $Mode = 'Full',

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountName,

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountKey,

      [Parameter(Mandatory=$false)]
      [PSObject[]] $AdditionDataLocationList = $null
      )

* <span data-ttu-id="b6a70-165">Název služby: Váš název cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b6a70-165">ServiceName: Your cloud service name.</span></span>
* <span data-ttu-id="b6a70-166">Název hello VMName hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b6a70-166">VMName hello name of hello VM.</span></span>
* <span data-ttu-id="b6a70-167">Režim: Režim kolekce.</span><span class="sxs-lookup"><span data-stu-id="b6a70-167">Mode: Collection mode.</span></span> <span data-ttu-id="b6a70-168">"Úplná" nebo "GA".</span><span class="sxs-lookup"><span data-stu-id="b6a70-168">“Full” or “GA”.</span></span>
* <span data-ttu-id="b6a70-169">StorageAccountName: Název účtu úložiště Azure pro ukládání shromážděna data.</span><span class="sxs-lookup"><span data-stu-id="b6a70-169">StorageAccountName: Name of Azure storage account for storing collected data.</span></span>
* <span data-ttu-id="b6a70-170">StorageAccountKey: Název klíč účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b6a70-170">StorageAccountKey: Name of Azure storage account key.</span></span>
* <span data-ttu-id="b6a70-171">AdditionalDataLocationList: Seznam hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="b6a70-171">AdditionalDataLocationList: A list of hello following structure:</span></span>

```
      {
        String Name,
        String Location,
        String SearchPattern,
        Bool   Recursive
      }
```

## <a name="extention-powershell-script-files"></a><span data-ttu-id="b6a70-172">Soubory skriptu prostředí PowerShell vytvořit vícejazyčný</span><span class="sxs-lookup"><span data-stu-id="b6a70-172">Extention PowerShell Script files</span></span>
<span data-ttu-id="b6a70-173">SetAzureServiceLogCollector.ps1</span><span class="sxs-lookup"><span data-stu-id="b6a70-173">SetAzureServiceLogCollector.ps1</span></span>

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Roles ,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Instances = '*',

                  [Parameter(Mandatory=$false)]
                  [string]   $Slot = 'Production',

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject

    if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be seperated by ,
    {
        $instanceText = $Instances[0]
        for ($i = 1;$i -lt $Instances.Count;$i++)
        {
              $instanceText = $instanceText+ "," + $Instances[$i]
          }
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value $instanceText
    }
    else  #For all instances if not specified.  hello value should be a space or *
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value " "
    }

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need tooget hello Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData toocollect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it tooJSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json
    "publicConfig is:  $publicConfigJSON"

    #we just provide a empty privateConfig object
    $privateconfig = "{
    }"

    if ($Roles -ne $null)
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot -Role $Roles -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }
    else
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot  -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }

    #
    #This is an optional step: generate a sasUri toohello container so it can be shared with other people if nened
    #
    $SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
    $SasUri = $SasUri + "&restype=container&comp=list"
    Write-Output "hello container for uploaded file can be accessed using this link:`r`n$sasuri"


<span data-ttu-id="b6a70-174">SetAzureVMLogCollector.ps1</span><span class="sxs-lookup"><span data-stu-id="b6a70-174">SetAzureVMLogCollector.ps1</span></span>

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string] $VMName ,

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value "*"

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need tooget hello Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData toocollect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it tooJSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json

    Write-Output "PublicConfigurtion is: \r\n$publicConfigJSON"

    #
    #we just provide a empty privateConfig object
    #
    $privateconfig = "{
    }"

    if ($VMName -ne $null )
    {
          $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
          $VM.VM.OSVirtualHardDisk.OS

          if ($VM.VM.OSVirtualHardDisk.OS -like '*Windows*')
          {
                Set-AzureVMExtension -VM $VM -ExtensionName "AzureLogCollector" -Publisher Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.* | Update-AzureVM -Verbose

                #
                #We will check hello VM status toofind if operation by extension has been completed or not. hello completion of hello operation,either succeed or fail, can be indicated by
                #hello presence of SubstatusList field.
                #
                $Completed = $false
                while ($Completed -ne $true)
                {
                        $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
                        $status = $VM.ResourceExtensionStatusList | Where-Object {$_.HandlerName -eq "Microsoft.WindowsAzure.Compute.AzureLogCollector"}

                        if ( ($status.Code -ne 0) -and ($status.Status -like '*error*'))
                        {
                            Write-Output "Error status is returned: $($Status.ExtensionSettingStatus.FormattedMessage.Message)."
                              $Completed = $true
                        }
                        elseif (($status.ExtensionSettingStatus.SubstatusList -eq $null -or $status.ExtensionSettingStatus.SubstatusList.Count -lt 1))
                        {
                              $Completed = $false
                              Write-Output "Waiting for operation toocomplete..."
                        }
                        else
                        {
                              $Completed = $true
                              Write-Output "Operation completed."

                        $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                              $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                      #
                            # This is an optional step:  For easier access toohello file, we can generate a read-only SasUri directly toohello file
                              #
                              $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                              $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                            Write-Output "hello uploaded file can be accessed using this link: $ReadSasUri"

                              #
                              #This is an optional step:  Remove hello extension after we are done
                              #
                              Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                        }
                        Start-Sleep -s 5
                }
          }
          else
          {
              Write-Output "VM OS Type is not Windows, hello extension cannot be enabled"
          }

    }
    else
    {
      Write-Output "VM name is not specified, hello extension cannot be enabled"
    }

## <a name="next-steps"></a><span data-ttu-id="b6a70-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6a70-175">Next Steps</span></span>
<span data-ttu-id="b6a70-176">Nyní můžete prozkoumat nebo kopírování protokolů z jednoho umístění velmi jednoduché.</span><span class="sxs-lookup"><span data-stu-id="b6a70-176">Now you can examine or copy your logs from one very simple location.</span></span>

