1. <span data-ttu-id="5aa09-101">Zkopírujte hello instalační program tooa místní složku (třeba C:\Temp) na serveru hello, které chcete tooprotect.</span><span class="sxs-lookup"><span data-stu-id="5aa09-101">Copy hello installer tooa local folder (for example, C:\Temp) on hello server that you want tooprotect.</span></span> <span data-ttu-id="5aa09-102">Spusťte hello jako správce na příkazovém řádku následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5aa09-102">Run hello following commands as an administrator at a command prompt:</span></span>

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. <span data-ttu-id="5aa09-103">tooinstall služby Mobility, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5aa09-103">tooinstall Mobility Service, run hello following command:</span></span>

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. <span data-ttu-id="5aa09-104">Teď je potřeba agenta hello toobe zaregistrována hello konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="5aa09-104">Now hello agent needs toobe registered with hello Configuration Server.</span></span>

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a><span data-ttu-id="5aa09-105">Argumenty příkazového řádku Instalační program služby mobility</span><span class="sxs-lookup"><span data-stu-id="5aa09-105">Mobility Service installer command-line arguments</span></span>

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| <span data-ttu-id="5aa09-106">Parametr</span><span class="sxs-lookup"><span data-stu-id="5aa09-106">Parameter</span></span>|<span data-ttu-id="5aa09-107">Typ</span><span class="sxs-lookup"><span data-stu-id="5aa09-107">Type</span></span>|<span data-ttu-id="5aa09-108">Popis</span><span class="sxs-lookup"><span data-stu-id="5aa09-108">Description</span></span>|<span data-ttu-id="5aa09-109">Možné hodnoty</span><span class="sxs-lookup"><span data-stu-id="5aa09-109">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="5aa09-110">/ Role</span><span class="sxs-lookup"><span data-stu-id="5aa09-110">/Role</span></span>|<span data-ttu-id="5aa09-111">Povinné</span><span class="sxs-lookup"><span data-stu-id="5aa09-111">Mandatory</span></span>|<span data-ttu-id="5aa09-112">Určuje, zda by měly být nainstalovány služby Mobility (MS) nebo MasterTarget(MT) by měly být nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="5aa09-112">Specifies whether Mobility Service (MS) should be installed or MasterTarget(MT) should be installed</span></span>|<span data-ttu-id="5aa09-113">MS</span><span class="sxs-lookup"><span data-stu-id="5aa09-113">MS</span></span> </br> <span data-ttu-id="5aa09-114">MT –</span><span class="sxs-lookup"><span data-stu-id="5aa09-114">MT</span></span>|
|<span data-ttu-id="5aa09-115">/InstallLocation</span><span class="sxs-lookup"><span data-stu-id="5aa09-115">/InstallLocation</span></span>|<span data-ttu-id="5aa09-116">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="5aa09-116">Optional</span></span>|<span data-ttu-id="5aa09-117">Umístění, kde je nainstalovaná služba Mobility</span><span class="sxs-lookup"><span data-stu-id="5aa09-117">Location where Mobility Service is installed</span></span>|<span data-ttu-id="5aa09-118">Libovolné složky v počítači hello</span><span class="sxs-lookup"><span data-stu-id="5aa09-118">Any folder on hello computer</span></span>|
|<span data-ttu-id="5aa09-119">/ Platform</span><span class="sxs-lookup"><span data-stu-id="5aa09-119">/Platform</span></span>|<span data-ttu-id="5aa09-120">Povinné</span><span class="sxs-lookup"><span data-stu-id="5aa09-120">Mandatory</span></span>|<span data-ttu-id="5aa09-121">Určuje hello platformy, na které hello služba Mobility je získávání nainstalovaná</span><span class="sxs-lookup"><span data-stu-id="5aa09-121">Specifies hello platform on which hello Mobility Service is getting installed</span></span> </br> </br><span data-ttu-id="5aa09-122">- **VMware** : tuto hodnotu použijte, pokud k instalaci služby mobility na virtuálním počítači systémem *VMware vSphere hostitelích ESXi*, *hostitelů Hyper-V* a *Phsyical servery*</span><span class="sxs-lookup"><span data-stu-id="5aa09-122">- **VMware** : use this value if you are installing mobility service on a VM running on *VMware vSphere ESXi Hosts*, *Hyper-V Hosts* and *Phsyical Servers*</span></span> </br> <span data-ttu-id="5aa09-123">- **Azure** : tuto hodnotu použijte, pokud instalujete agenta na virtuálním počítači Azure IaaS</span><span class="sxs-lookup"><span data-stu-id="5aa09-123">- **Azure** : use this value if you are installing agent on a Azure IaaS VM</span></span>| <span data-ttu-id="5aa09-124">VMware</span><span class="sxs-lookup"><span data-stu-id="5aa09-124">VMware</span></span> </br> <span data-ttu-id="5aa09-125">Azure</span><span class="sxs-lookup"><span data-stu-id="5aa09-125">Azure</span></span>|
|<span data-ttu-id="5aa09-126">/ Tichou</span><span class="sxs-lookup"><span data-stu-id="5aa09-126">/Silent</span></span>|<span data-ttu-id="5aa09-127">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="5aa09-127">Optional</span></span>|<span data-ttu-id="5aa09-128">Určuje toorun hello instalačního programu v bezobslužném režimu</span><span class="sxs-lookup"><span data-stu-id="5aa09-128">Specifies toorun hello installer in silent mode</span></span>| <span data-ttu-id="5aa09-129">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5aa09-129">NA</span></span>|

>[!TIP]
> <span data-ttu-id="5aa09-130">Hello instalační protokoly naleznete v části %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log</span><span class="sxs-lookup"><span data-stu-id="5aa09-130">hello setup logs can be found under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log</span></span>

#### <a name="mobility-service-registration-command-line-arguments"></a><span data-ttu-id="5aa09-131">Argumenty příkazového řádku registrace služby mobility</span><span class="sxs-lookup"><span data-stu-id="5aa09-131">Mobility Service registration command-line arguments</span></span>

```
Usage :
UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | <span data-ttu-id="5aa09-132">Parametr</span><span class="sxs-lookup"><span data-stu-id="5aa09-132">Parameter</span></span>|<span data-ttu-id="5aa09-133">Typ</span><span class="sxs-lookup"><span data-stu-id="5aa09-133">Type</span></span>|<span data-ttu-id="5aa09-134">Popis</span><span class="sxs-lookup"><span data-stu-id="5aa09-134">Description</span></span>|<span data-ttu-id="5aa09-135">Možné hodnoty</span><span class="sxs-lookup"><span data-stu-id="5aa09-135">Possible values</span></span>|
  |-|-|-|-|
  |<span data-ttu-id="5aa09-136">/ CSEndPoint</span><span class="sxs-lookup"><span data-stu-id="5aa09-136">/CSEndPoint</span></span> |<span data-ttu-id="5aa09-137">Povinné</span><span class="sxs-lookup"><span data-stu-id="5aa09-137">Mandatory</span></span>|<span data-ttu-id="5aa09-138">IP adresa serveru konfigurace hello</span><span class="sxs-lookup"><span data-stu-id="5aa09-138">IP address of hello configuration server</span></span>| <span data-ttu-id="5aa09-139">Všechny platnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="5aa09-139">Any valid IP address</span></span>|
  |<span data-ttu-id="5aa09-140">/PassphraseFilePath</span><span class="sxs-lookup"><span data-stu-id="5aa09-140">/PassphraseFilePath</span></span>|<span data-ttu-id="5aa09-141">Povinné</span><span class="sxs-lookup"><span data-stu-id="5aa09-141">Mandatory</span></span>|<span data-ttu-id="5aa09-142">Umístění hello heslo</span><span class="sxs-lookup"><span data-stu-id="5aa09-142">Location of hello passphrase</span></span> |<span data-ttu-id="5aa09-143">Všechny platné UNC nebo místní cesta</span><span class="sxs-lookup"><span data-stu-id="5aa09-143">Any valid UNC or local file path</span></span>|


>[!TIP]
> <span data-ttu-id="5aa09-144">Hello AgentConfiguration protokoly najdete v části %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log</span><span class="sxs-lookup"><span data-stu-id="5aa09-144">hello AgentConfiguration logs can be found under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log</span></span>
