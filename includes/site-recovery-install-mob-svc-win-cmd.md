1. <span data-ttu-id="8cdca-101">Zkopírujte instalační službu do místní složky (například C:\Temp) na serveru, který chcete chránit.</span><span class="sxs-lookup"><span data-stu-id="8cdca-101">Copy the installer to a local folder (for example, C:\Temp) on the server that you want to protect.</span></span> <span data-ttu-id="8cdca-102">Jako správce na příkazovém řádku spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="8cdca-102">Run the following commands as an administrator at a command prompt:</span></span>

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. <span data-ttu-id="8cdca-103">Pro instalaci služby Mobility, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8cdca-103">To install Mobility Service, run the following command:</span></span>

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. <span data-ttu-id="8cdca-104">Nyní agent musí být registrováno s konfiguračním serverem.</span><span class="sxs-lookup"><span data-stu-id="8cdca-104">Now the agent needs to be registered with the Configuration Server.</span></span>

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a><span data-ttu-id="8cdca-105">Argumenty příkazového řádku Instalační program služby mobility</span><span class="sxs-lookup"><span data-stu-id="8cdca-105">Mobility Service installer command-line arguments</span></span>

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| <span data-ttu-id="8cdca-106">Parametr</span><span class="sxs-lookup"><span data-stu-id="8cdca-106">Parameter</span></span>|<span data-ttu-id="8cdca-107">Typ</span><span class="sxs-lookup"><span data-stu-id="8cdca-107">Type</span></span>|<span data-ttu-id="8cdca-108">Popis</span><span class="sxs-lookup"><span data-stu-id="8cdca-108">Description</span></span>|<span data-ttu-id="8cdca-109">Možné hodnoty</span><span class="sxs-lookup"><span data-stu-id="8cdca-109">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="8cdca-110">/ Role</span><span class="sxs-lookup"><span data-stu-id="8cdca-110">/Role</span></span>|<span data-ttu-id="8cdca-111">Povinné</span><span class="sxs-lookup"><span data-stu-id="8cdca-111">Mandatory</span></span>|<span data-ttu-id="8cdca-112">Určuje, zda by měly být nainstalovány služby Mobility (MS) nebo MasterTarget(MT) by měly být nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="8cdca-112">Specifies whether Mobility Service (MS) should be installed or MasterTarget(MT) should be installed</span></span>|<span data-ttu-id="8cdca-113">MS</span><span class="sxs-lookup"><span data-stu-id="8cdca-113">MS</span></span> </br> <span data-ttu-id="8cdca-114">MT –</span><span class="sxs-lookup"><span data-stu-id="8cdca-114">MT</span></span>|
|<span data-ttu-id="8cdca-115">/InstallLocation</span><span class="sxs-lookup"><span data-stu-id="8cdca-115">/InstallLocation</span></span>|<span data-ttu-id="8cdca-116">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="8cdca-116">Optional</span></span>|<span data-ttu-id="8cdca-117">Umístění, kde je nainstalovaná služba Mobility</span><span class="sxs-lookup"><span data-stu-id="8cdca-117">Location where Mobility Service is installed</span></span>|<span data-ttu-id="8cdca-118">Libovolná složka v počítači</span><span class="sxs-lookup"><span data-stu-id="8cdca-118">Any folder on the computer</span></span>|
|<span data-ttu-id="8cdca-119">/ Platform</span><span class="sxs-lookup"><span data-stu-id="8cdca-119">/Platform</span></span>|<span data-ttu-id="8cdca-120">Povinné</span><span class="sxs-lookup"><span data-stu-id="8cdca-120">Mandatory</span></span>|<span data-ttu-id="8cdca-121">Určuje platformu, na kterém služba Mobility je získávání nainstalovaná</span><span class="sxs-lookup"><span data-stu-id="8cdca-121">Specifies the platform on which the Mobility Service is getting installed</span></span> </br> </br><span data-ttu-id="8cdca-122">- **VMware** : tuto hodnotu použijte, pokud k instalaci služby mobility na virtuálním počítači systémem *VMware vSphere hostitelích ESXi*, *hostitelů Hyper-V* a *Phsyical servery*</span><span class="sxs-lookup"><span data-stu-id="8cdca-122">- **VMware** : use this value if you are installing mobility service on a VM running on *VMware vSphere ESXi Hosts*, *Hyper-V Hosts* and *Phsyical Servers*</span></span> </br> <span data-ttu-id="8cdca-123">- **Azure** : tuto hodnotu použijte, pokud instalujete agenta na virtuálním počítači Azure IaaS</span><span class="sxs-lookup"><span data-stu-id="8cdca-123">- **Azure** : use this value if you are installing agent on a Azure IaaS VM</span></span>| <span data-ttu-id="8cdca-124">VMware</span><span class="sxs-lookup"><span data-stu-id="8cdca-124">VMware</span></span> </br> <span data-ttu-id="8cdca-125">Azure</span><span class="sxs-lookup"><span data-stu-id="8cdca-125">Azure</span></span>|
|<span data-ttu-id="8cdca-126">/ Tichou</span><span class="sxs-lookup"><span data-stu-id="8cdca-126">/Silent</span></span>|<span data-ttu-id="8cdca-127">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="8cdca-127">Optional</span></span>|<span data-ttu-id="8cdca-128">Určuje, spusťte instalační program v bezobslužném režimu</span><span class="sxs-lookup"><span data-stu-id="8cdca-128">Specifies to run the installer in silent mode</span></span>| <span data-ttu-id="8cdca-129">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8cdca-129">NA</span></span>|

>[!TIP]
> <span data-ttu-id="8cdca-130">Instalační protokoly naleznete v části %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log</span><span class="sxs-lookup"><span data-stu-id="8cdca-130">The setup logs can be found under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log</span></span>

#### <a name="mobility-service-registration-command-line-arguments"></a><span data-ttu-id="8cdca-131">Argumenty příkazového řádku registrace služby mobility</span><span class="sxs-lookup"><span data-stu-id="8cdca-131">Mobility Service registration command-line arguments</span></span>

```
Usage :
UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | <span data-ttu-id="8cdca-132">Parametr</span><span class="sxs-lookup"><span data-stu-id="8cdca-132">Parameter</span></span>|<span data-ttu-id="8cdca-133">Typ</span><span class="sxs-lookup"><span data-stu-id="8cdca-133">Type</span></span>|<span data-ttu-id="8cdca-134">Popis</span><span class="sxs-lookup"><span data-stu-id="8cdca-134">Description</span></span>|<span data-ttu-id="8cdca-135">Možné hodnoty</span><span class="sxs-lookup"><span data-stu-id="8cdca-135">Possible values</span></span>|
  |-|-|-|-|
  |<span data-ttu-id="8cdca-136">/ CSEndPoint</span><span class="sxs-lookup"><span data-stu-id="8cdca-136">/CSEndPoint</span></span> |<span data-ttu-id="8cdca-137">Povinné</span><span class="sxs-lookup"><span data-stu-id="8cdca-137">Mandatory</span></span>|<span data-ttu-id="8cdca-138">IP adresa konfiguračního serveru</span><span class="sxs-lookup"><span data-stu-id="8cdca-138">IP address of the configuration server</span></span>| <span data-ttu-id="8cdca-139">Všechny platnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="8cdca-139">Any valid IP address</span></span>|
  |<span data-ttu-id="8cdca-140">/PassphraseFilePath</span><span class="sxs-lookup"><span data-stu-id="8cdca-140">/PassphraseFilePath</span></span>|<span data-ttu-id="8cdca-141">Povinné</span><span class="sxs-lookup"><span data-stu-id="8cdca-141">Mandatory</span></span>|<span data-ttu-id="8cdca-142">Umístění heslo</span><span class="sxs-lookup"><span data-stu-id="8cdca-142">Location of the passphrase</span></span> |<span data-ttu-id="8cdca-143">Všechny platné UNC nebo místní cesta</span><span class="sxs-lookup"><span data-stu-id="8cdca-143">Any valid UNC or local file path</span></span>|


>[!TIP]
> <span data-ttu-id="8cdca-144">Protokoly AgentConfiguration naleznete v části %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log</span><span class="sxs-lookup"><span data-stu-id="8cdca-144">The AgentConfiguration logs can be found under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log</span></span>
