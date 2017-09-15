
<span data-ttu-id="70774-101">Diagnostika problémů s cloudové služby Microsoft Azure vyžaduje shromažďování souborů protokolů služby u virtuálních počítačů jako dojít k problémům.</span><span class="sxs-lookup"><span data-stu-id="70774-101">Diagnosing issues with an Microsoft Azure cloud service requires collecting the service’s log files on virtual machines as the issues occur.</span></span> <span data-ttu-id="70774-102">Můžete použít AzureLogCollector rozšíření na vyžádání k perfom jednorázové shromažďování protokolů z jedné nebo více cloudové služby virtuálních počítačů (z webové role i role pracovního procesu) a přenos shromážděných souborů do účtu úložiště Azure – všechny bez vzdálené přihlášení k jakémukoli z virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="70774-102">You can use the AzureLogCollector extension on-demand to perfom one-time collection of logs from one or more Cloud Service VMs (from both web roles and worker roles) and transfer the collected files to an Azure storage account – all without remotely logging on to any of the VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="70774-103">Popisy pro většinu zaznamenané informace lze najít na http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.</span><span class="sxs-lookup"><span data-stu-id="70774-103">Descriptions for most of the logged information can be found at http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.</span></span>
> 
> 

<span data-ttu-id="70774-104">Existují dva režimy kolekce závisí na typy souborů, které se mají shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="70774-104">There are two modes of collection dependent on the types of files to be collected.</span></span>

* <span data-ttu-id="70774-105">Azure hostovaného agenta protokoly pouze (GA).</span><span class="sxs-lookup"><span data-stu-id="70774-105">Azure Guest Agent Logs only (GA).</span></span> <span data-ttu-id="70774-106">Tento režim kolekce zahrnuje všechny protokoly související s agenty Azure hostů a další součásti Azure.</span><span class="sxs-lookup"><span data-stu-id="70774-106">This collection mode includes all the logs related to Azure guest agents and other Azure components.</span></span>
* <span data-ttu-id="70774-107">Všechny protokoly (úplná záloha).</span><span class="sxs-lookup"><span data-stu-id="70774-107">All Logs (Full).</span></span> <span data-ttu-id="70774-108">Tento režim kolekce bude shromažďovat všechny soubory v režimu GA plus:</span><span class="sxs-lookup"><span data-stu-id="70774-108">This collection mode will collect all files in GA mode plus:</span></span>
  
  * <span data-ttu-id="70774-109">protokoly událostí systému a aplikací</span><span class="sxs-lookup"><span data-stu-id="70774-109">system and application event logs</span></span>
  * <span data-ttu-id="70774-110">Protokoly chyb HTTP</span><span class="sxs-lookup"><span data-stu-id="70774-110">HTTP error logs</span></span>
  * <span data-ttu-id="70774-111">Protokoly služby IIS</span><span class="sxs-lookup"><span data-stu-id="70774-111">IIS Logs</span></span>
  * <span data-ttu-id="70774-112">Protokoly instalace</span><span class="sxs-lookup"><span data-stu-id="70774-112">Setup logs</span></span>
  * <span data-ttu-id="70774-113">Další systémové protokoly</span><span class="sxs-lookup"><span data-stu-id="70774-113">other system logs</span></span>

<span data-ttu-id="70774-114">V obou režimech kolekce složky kolekcí další data lze pomocí kolekce následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="70774-114">In both collection modes, additional data collection folders can be specified by using a collection of the following structure:</span></span>

* <span data-ttu-id="70774-115">**Název**: název kolekce, která se použije jako název podsložky v souboru zip, které se mají shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="70774-115">**Name**: The name of the collection, which will be used as the name of subfolder inside the zip file to be collected.</span></span>
* <span data-ttu-id="70774-116">**Umístění**: cesta ke složce na virtuálním počítači, kde budou shromažďovány souboru.</span><span class="sxs-lookup"><span data-stu-id="70774-116">**Location**: The path to the folder on the virtual machine where file will be collected.</span></span>
* <span data-ttu-id="70774-117">**SearchPattern**: vzor názvů souborů, které se mají shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="70774-117">**SearchPattern**: The pattern of the names of files to be collected.</span></span> <span data-ttu-id="70774-118">Výchozí hodnota je "*"</span><span class="sxs-lookup"><span data-stu-id="70774-118">Default is “*”</span></span>
* <span data-ttu-id="70774-119">**Rekurzivní**: Pokud soubory budou shromažďovat rekurzivně ve složce.</span><span class="sxs-lookup"><span data-stu-id="70774-119">**Recursive**: if the files will be collected recursively under the folder.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70774-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="70774-120">Prerequisites</span></span>
* <span data-ttu-id="70774-121">Potřebujete mít účet úložiště pro rozšíření, které chcete ukládat soubory generované zip.</span><span class="sxs-lookup"><span data-stu-id="70774-121">You need to have a storage account for extension to save generated zip files.</span></span>
* <span data-ttu-id="70774-122">Ujistěte se, že používáte V0.8.0 rutiny Azure PowerShell nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="70774-122">You must make sure that you are using Azure PowerShell Cmdlets V0.8.0 or above.</span></span> <span data-ttu-id="70774-123">Další informace najdete v tématu [Azure stáhne](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="70774-123">For more information, see [Azure Downloads](https://azure.microsoft.com/downloads/).</span></span>

## <a name="add-the-extension"></a><span data-ttu-id="70774-124">Přidejte rozšíření</span><span class="sxs-lookup"><span data-stu-id="70774-124">Add the extension</span></span>
<span data-ttu-id="70774-125">Můžete použít [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) rutiny nebo [rozhraní API REST pro správu služby](https://msdn.microsoft.com/library/ee460799.aspx) přidat AzureLogCollector rozšíření.</span><span class="sxs-lookup"><span data-stu-id="70774-125">You can use [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) cmdlets or [Service Management REST APIs](https://msdn.microsoft.com/library/ee460799.aspx) to add the AzureLogCollector extension.</span></span>

<span data-ttu-id="70774-126">Pro cloudové služby, existující rutiny Azure Powershellu **Set-AzureServiceExtension**, slouží k povolení rozšíření u instancí role cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="70774-126">For Cloud Services, the existing Azure Powershell cmdlet, **Set-AzureServiceExtension**, can be used to enable the extension on Cloud Service role instances.</span></span> <span data-ttu-id="70774-127">Pokaždé, když je toto rozšíření povolené prostřednictvím této rutiny, aktivuje se v instancích vybranou roli v systému vybrané role shromáždění protokolů.</span><span class="sxs-lookup"><span data-stu-id="70774-127">Every time this extension is enabled through this cmdlet, log collection is triggered on the selected role instances of selected roles.</span></span>

<span data-ttu-id="70774-128">Pro virtuální počítače, existující rutiny Azure Powershellu **Set-AzureVMExtension**, slouží k povolení rozšíření na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="70774-128">For Virtual Machines, the existing Azure Powershell cmdlet, **Set-AzureVMExtension**, can be used to enable the extension on Virtual Machines.</span></span> <span data-ttu-id="70774-129">Pokaždé, když je toto rozšíření povolené prostřednictvím rutin, aktivuje se na každou instanci shromáždění protokolů.</span><span class="sxs-lookup"><span data-stu-id="70774-129">Every time this extension is enabled through the cmdlets, log collection is triggered on each instance.</span></span>

<span data-ttu-id="70774-130">Toto rozšíření interně používá na základě JSON PublicConfiguration a PrivateConfiguration.</span><span class="sxs-lookup"><span data-stu-id="70774-130">Internally, this extension uses the JSON-based PublicConfiguration and PrivateConfiguration.</span></span> <span data-ttu-id="70774-131">Toto je rozložení ukázku JSON pro veřejné a privátní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="70774-131">The following is the layout of a sample JSON for public and private configuration.</span></span>

### <a name="publicconfiguration"></a><span data-ttu-id="70774-132">PublicConfiguration</span><span class="sxs-lookup"><span data-stu-id="70774-132">PublicConfiguration</span></span>
    {
        "Instances":  "*",
        "Mode":  "Full",
        "SasUri":  "SasUri to your storage account with sp=wl",
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

### <a name="privateconfiguration"></a><span data-ttu-id="70774-133">PrivateConfiguration</span><span class="sxs-lookup"><span data-stu-id="70774-133">PrivateConfiguration</span></span>
    {

    }

> [!NOTE]
> <span data-ttu-id="70774-134">Toto rozšíření nevyžaduje **privateConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="70774-134">This extension doesn’t need **privateConfiguration**.</span></span> <span data-ttu-id="70774-135">Zadáte na prázdnou strukturou pro **– PrivateConfiguration** argument.</span><span class="sxs-lookup"><span data-stu-id="70774-135">You can just provide an empty structure for the **–PrivateConfiguration** argument.</span></span>
> 
> 

<span data-ttu-id="70774-136">Můžete provést jednu z dvě následující kroky k přidání AzureLogCollector na jeden nebo více instancí cloudové služby nebo virtuálního počítače vybrané role, která aktivuje kolekce na každý virtuální počítač ke spuštění a odeslání shromážděné soubory pro zadaný účet Azure.</span><span class="sxs-lookup"><span data-stu-id="70774-136">You can follow one of the two following steps to add the AzureLogCollector to one or more instances of a Cloud Service or Virtual Machine of selected roles, which triggers the collections on each VM to run and send the collected files to Azure account specified.</span></span>

## <a name="adding-as-a-service-extension"></a><span data-ttu-id="70774-137">Přidání jako rozšíření služby</span><span class="sxs-lookup"><span data-stu-id="70774-137">Adding as a Service Extension</span></span>
1. <span data-ttu-id="70774-138">Postupujte podle pokynů k připojení k vašemu předplatnému Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="70774-138">Follow the instructions to connect Azure PowerShell to your subscription.</span></span>
2. <span data-ttu-id="70774-139">Zadejte název, slotu, role a role instance služby pro které chcete přidat a povolit AzureLogCollector rozšíření.</span><span class="sxs-lookup"><span data-stu-id="70774-139">Specify the service name, slot, roles, and role instances to which you want to add and enable the AzureLogCollector extension.</span></span>
   
        #Specify your cloud service name
        $ServiceName = 'extensiontest2'
   
        #Specify the slot. 'Production' or 'Staging'
        $slot = 'Production'
   
        #Specified the roles on which the extension will be installed and enabled
        $roles = @("WorkerRole1","WebRole1")
   
        #Specify the instances on which extension will be installed and enabled.  Use wildcard * for all instances
        $instances = @("*")
   
        #Specify the collection mode, "Full" or "GA"
        $mode = "GA"
3. <span data-ttu-id="70774-140">Zadejte složku další data, pro které se budou shromažďovat soubory (Tento krok je volitelný).</span><span class="sxs-lookup"><span data-stu-id="70774-140">Specify the additional data folder for which files will be collected (this step is optional).</span></span>
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
   
   > [!NOTE]
   > <span data-ttu-id="70774-141">Můžete použít token `%roleroot%` chcete určit kořenovou jednotku role, protože nepoužívá pevnou jednotku.</span><span class="sxs-lookup"><span data-stu-id="70774-141">You can use token `%roleroot%` to specify the role root drive since it doesn’t use a fixed drive.</span></span>
   > 
   > 
4. <span data-ttu-id="70774-142">Zadejte název účtu úložiště Azure a klíč, ke kterému se nahraje shromážděných souborů.</span><span class="sxs-lookup"><span data-stu-id="70774-142">Provide the Azure storage account name and key to which collected files will be uploaded.</span></span>
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
5. <span data-ttu-id="70774-143">Volání SetAzureServiceLogCollector.ps1 (zahrnutá na konci tohoto článku) následujícím způsobem povolit rozšíření AzureLogCollector pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="70774-143">Call the SetAzureServiceLogCollector.ps1 (included at the end of the article) as follows to enable the AzureLogCollector extension for a Cloud Service.</span></span> <span data-ttu-id="70774-144">Po dokončení provádění můžete najít nahrávaný soubor v části`https://YouareStorageAccountName.blob.core.windows.net/vmlogs`</span><span class="sxs-lookup"><span data-stu-id="70774-144">Once the execution is completed, you can find the uploaded file under `https://YouareStorageAccountName.blob.core.windows.net/vmlogs`</span></span>
   
        .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList

<span data-ttu-id="70774-145">Zde je definice parametry předané do skriptu.</span><span class="sxs-lookup"><span data-stu-id="70774-145">The following is the definition of the parameters passed to the script.</span></span> <span data-ttu-id="70774-146">(To je zkopírovat níže také.)</span><span class="sxs-lookup"><span data-stu-id="70774-146">(This is copied below as well.)</span></span>

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

* <span data-ttu-id="70774-147">*ServiceName*: název vaší cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="70774-147">*ServiceName*: Your cloud service name.</span></span>
* <span data-ttu-id="70774-148">*Role*: seznam rolí, například "WebRole1" nebo "WorkerRole1".</span><span class="sxs-lookup"><span data-stu-id="70774-148">*Roles*: A list of roles, such as “WebRole1” or ”WorkerRole1”.</span></span>
* <span data-ttu-id="70774-149">*Instance*: seznam názvů instancí role oddělené čárkou – použít zástupný znak řetězec ("*") pro všechny instance role.</span><span class="sxs-lookup"><span data-stu-id="70774-149">*Instances*: A list of the names of role instances separated by comma -- use the wildcard string (“*”) for all role instances.</span></span>
* <span data-ttu-id="70774-150">*Slot*: název slotu.</span><span class="sxs-lookup"><span data-stu-id="70774-150">*Slot*: Slot name.</span></span> <span data-ttu-id="70774-151">"Výroba" nebo "Přípravy".</span><span class="sxs-lookup"><span data-stu-id="70774-151">“Production” or “Staging”.</span></span>
* <span data-ttu-id="70774-152">*Režim*: režim kolekce.</span><span class="sxs-lookup"><span data-stu-id="70774-152">*Mode*: Collection mode.</span></span> <span data-ttu-id="70774-153">"Úplná" nebo "GA".</span><span class="sxs-lookup"><span data-stu-id="70774-153">“Full” or “GA”.</span></span>
* <span data-ttu-id="70774-154">*StorageAccountName*: název Azure účet úložiště pro ukládání shromážděných dat.</span><span class="sxs-lookup"><span data-stu-id="70774-154">*StorageAccountName*: Name of Azure storage account for storing collected data.</span></span>
* <span data-ttu-id="70774-155">*StorageAccountKey*: název Azure klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="70774-155">*StorageAccountKey*: Name of Azure storage account key.</span></span>
* <span data-ttu-id="70774-156">*AdditionalDataLocationList*: seznam následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="70774-156">*AdditionalDataLocationList*: A list of the following structure:</span></span>
  
      {
      String Name,
      String Location,
      String SearchPattern,
      Bool   Recursive
      }

## <a name="adding-as-a-vm-extension"></a><span data-ttu-id="70774-157">Přidání jako rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="70774-157">Adding as a VM Extension</span></span>
<span data-ttu-id="70774-158">Postupujte podle pokynů k připojení k vašemu předplatnému Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="70774-158">Follow the instructions to connect Azure PowerShell to your subscription.</span></span>

1. <span data-ttu-id="70774-159">Zadejte název služby, virtuálních počítačů a režim kolekce.</span><span class="sxs-lookup"><span data-stu-id="70774-159">Specify the service name, VM, and the collection mode.</span></span>
   
        #Specify your cloud service name
        $ServiceName = 'YourCloudServiceName'
   
        #Specify the VM name
        $VMName = "'YourVMName'"
   
        #Specify the collection mode, "Full" or "GA"
        $mode = "GA"
   
        Specify the additional data folder for which files will be collected (this step is optional).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
2. <span data-ttu-id="70774-160">Zadejte název účtu úložiště Azure a klíč, ke kterému se nahraje shromážděných souborů.</span><span class="sxs-lookup"><span data-stu-id="70774-160">Provide the Azure storage account name and key to which collected files will be uploaded.</span></span>
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
3. <span data-ttu-id="70774-161">Volání SetAzureVMLogCollector.ps1 (zahrnutá na konci tohoto článku) následujícím způsobem povolit rozšíření AzureLogCollector pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="70774-161">Call the SetAzureVMLogCollector.ps1 (included at the end of the article) as follows to enable the AzureLogCollector extension for a Cloud Service.</span></span> <span data-ttu-id="70774-162">Po dokončení provádění najdete v části https://YouareStorageAccountName.blob.core.windows.net/vmlogs nahrávaný soubor</span><span class="sxs-lookup"><span data-stu-id="70774-162">Once the execution is completed, you can find the uploaded file under https://YouareStorageAccountName.blob.core.windows.net/vmlogs</span></span>

<span data-ttu-id="70774-163">Zde je definice parametry předané do skriptu.</span><span class="sxs-lookup"><span data-stu-id="70774-163">The following is the definition of the parameters passed to the script.</span></span> <span data-ttu-id="70774-164">(To je zkopírovat níže také.)</span><span class="sxs-lookup"><span data-stu-id="70774-164">(This is copied below as well.)</span></span>

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

* <span data-ttu-id="70774-165">Název služby: Váš název cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="70774-165">ServiceName: Your cloud service name.</span></span>
* <span data-ttu-id="70774-166">VMName název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="70774-166">VMName The name of the VM.</span></span>
* <span data-ttu-id="70774-167">Režim: Režim kolekce.</span><span class="sxs-lookup"><span data-stu-id="70774-167">Mode: Collection mode.</span></span> <span data-ttu-id="70774-168">"Úplná" nebo "GA".</span><span class="sxs-lookup"><span data-stu-id="70774-168">“Full” or “GA”.</span></span>
* <span data-ttu-id="70774-169">StorageAccountName: Název účtu úložiště Azure pro ukládání shromážděna data.</span><span class="sxs-lookup"><span data-stu-id="70774-169">StorageAccountName: Name of Azure storage account for storing collected data.</span></span>
* <span data-ttu-id="70774-170">StorageAccountKey: Název klíč účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="70774-170">StorageAccountKey: Name of Azure storage account key.</span></span>
* <span data-ttu-id="70774-171">AdditionalDataLocationList: Seznam následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="70774-171">AdditionalDataLocationList: A list of the following structure:</span></span>

```
      {
        String Name,
        String Location,
        String SearchPattern,
        Bool   Recursive
      }
```

## <a name="extention-powershell-script-files"></a><span data-ttu-id="70774-172">Soubory skriptu prostředí PowerShell vytvořit vícejazyčný</span><span class="sxs-lookup"><span data-stu-id="70774-172">Extention PowerShell Script files</span></span>
<span data-ttu-id="70774-173">SetAzureServiceLogCollector.ps1</span><span class="sxs-lookup"><span data-stu-id="70774-173">SetAzureServiceLogCollector.ps1</span></span>

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
    else  #For all instances if not specified.  The value should be a space or *
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
    #we need to get the Sasuri from StorageAccount and containers
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
    #Add AdditionalData to collect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it to JSON format
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
    #This is an optional step: generate a sasUri to the container so it can be shared with other people if nened
    #
    $SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
    $SasUri = $SasUri + "&restype=container&comp=list"
    Write-Output "The container for uploaded file can be accessed using this link:`r`n$sasuri"


<span data-ttu-id="70774-174">SetAzureVMLogCollector.ps1</span><span class="sxs-lookup"><span data-stu-id="70774-174">SetAzureVMLogCollector.ps1</span></span>

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
    #we need to get the Sasuri from StorageAccount and containers
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
    #Add AdditionalData to collect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it to JSON format
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
                #We will check the VM status to find if operation by extension has been completed or not. The completion of the operation,either succeed or fail, can be indicated by
                #the presence of SubstatusList field.
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
                              Write-Output "Waiting for operation to complete..."
                        }
                        else
                        {
                              $Completed = $true
                              Write-Output "Operation completed."

                        $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                              $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                      #
                            # This is an optional step:  For easier access to the file, we can generate a read-only SasUri directly to the file
                              #
                              $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                              $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                            Write-Output "The uploaded file can be accessed using this link: $ReadSasUri"

                              #
                              #This is an optional step:  Remove the extension after we are done
                              #
                              Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                        }
                        Start-Sleep -s 5
                }
          }
          else
          {
              Write-Output "VM OS Type is not Windows, the extension cannot be enabled"
          }

    }
    else
    {
      Write-Output "VM name is not specified, the extension cannot be enabled"
    }

## <a name="next-steps"></a><span data-ttu-id="70774-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70774-175">Next Steps</span></span>
<span data-ttu-id="70774-176">Nyní můžete prozkoumat nebo kopírování protokolů z jednoho umístění velmi jednoduché.</span><span class="sxs-lookup"><span data-stu-id="70774-176">Now you can examine or copy your logs from one very simple location.</span></span>

