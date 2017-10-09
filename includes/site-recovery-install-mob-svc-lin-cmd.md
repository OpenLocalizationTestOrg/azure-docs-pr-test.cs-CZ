1. <span data-ttu-id="436ec-101">Zkopírujte hello instalační program tooa místní složky (například TMP) na serveru hello, které chcete tooprotect.</span><span class="sxs-lookup"><span data-stu-id="436ec-101">Copy hello installer tooa local folder (for example, /tmp) on hello server that you want tooprotect.</span></span> <span data-ttu-id="436ec-102">V terminálu spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="436ec-102">In a terminal, run hello following commands:</span></span>
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. <span data-ttu-id="436ec-103">tooinstall služby Mobility, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="436ec-103">tooinstall Mobility Service, run hello following command:</span></span>

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. <span data-ttu-id="436ec-104">Po dokončení instalace musí hello služba Mobility tooget registrované toohello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="436ec-104">Once installation is complete, hello Mobility Service needs tooget registered toohello configuration server.</span></span> <span data-ttu-id="436ec-105">Spusťte následující příkaz tooregister hello služba Mobility se konfigurační server hello.</span><span class="sxs-lookup"><span data-stu-id="436ec-105">Run hello following command tooregister hello Mobility Service with Configuration server.</span></span>

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a><span data-ttu-id="436ec-106">Instalátoru služby mobility příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="436ec-106">Mobility Service installer command-line</span></span>

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|<span data-ttu-id="436ec-107">Parametr</span><span class="sxs-lookup"><span data-stu-id="436ec-107">Parameter</span></span>|<span data-ttu-id="436ec-108">Typ</span><span class="sxs-lookup"><span data-stu-id="436ec-108">Type</span></span>|<span data-ttu-id="436ec-109">Popis</span><span class="sxs-lookup"><span data-stu-id="436ec-109">Description</span></span>|<span data-ttu-id="436ec-110">Možné hodnoty</span><span class="sxs-lookup"><span data-stu-id="436ec-110">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="436ec-111">-r</span><span class="sxs-lookup"><span data-stu-id="436ec-111">-r</span></span> |<span data-ttu-id="436ec-112">Povinné</span><span class="sxs-lookup"><span data-stu-id="436ec-112">Mandatory</span></span>|<span data-ttu-id="436ec-113">Určuje, zda by měly být nainstalovány služby Mobility (MS) nebo MasterTarget(MT) by měly být nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="436ec-113">Specifies whether Mobility Service (MS) should be installed or MasterTarget(MT) should be installed</span></span>|<span data-ttu-id="436ec-114">MS</span><span class="sxs-lookup"><span data-stu-id="436ec-114">MS</span></span> </br> <span data-ttu-id="436ec-115">MT –</span><span class="sxs-lookup"><span data-stu-id="436ec-115">MT</span></span>|
|<span data-ttu-id="436ec-116">-d</span><span class="sxs-lookup"><span data-stu-id="436ec-116">-d</span></span> |<span data-ttu-id="436ec-117">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="436ec-117">Optional</span></span>|<span data-ttu-id="436ec-118">Umístění, kde bude nainstalován služba Mobility</span><span class="sxs-lookup"><span data-stu-id="436ec-118">Location where Mobility Service will be installed</span></span>|<span data-ttu-id="436ec-119">/usr/local/ASR</span><span class="sxs-lookup"><span data-stu-id="436ec-119">/usr/local/ASR</span></span>|
|<span data-ttu-id="436ec-120">-v</span><span class="sxs-lookup"><span data-stu-id="436ec-120">-v</span></span>|<span data-ttu-id="436ec-121">Povinné</span><span class="sxs-lookup"><span data-stu-id="436ec-121">Mandatory</span></span>|<span data-ttu-id="436ec-122">Určuje hello platformy, na které hello služba Mobility je získávání nainstalovaná</span><span class="sxs-lookup"><span data-stu-id="436ec-122">Specifies hello platform on which hello Mobility Service is getting installed</span></span> </br> </br><span data-ttu-id="436ec-123">- **VMware** : tuto hodnotu použijte, pokud k instalaci služby mobility na virtuálním počítači systémem *VMware vSphere hostitelích ESXi*, *hostitelů Hyper-V* a *Phsyical servery*</span><span class="sxs-lookup"><span data-stu-id="436ec-123">- **VMware** : use this value if you are installing mobility service on a VM running on *VMware vSphere ESXi Hosts*, *Hyper-V Hosts* and *Phsyical Servers*</span></span> </br> <span data-ttu-id="436ec-124">- **Azure** : tuto hodnotu použijte, pokud instalujete agenta na virtuálním počítači Azure IaaS</span><span class="sxs-lookup"><span data-stu-id="436ec-124">- **Azure** : use this value if you are installing agent on a Azure IaaS VM</span></span>| <span data-ttu-id="436ec-125">VMware</span><span class="sxs-lookup"><span data-stu-id="436ec-125">VMware</span></span> </br> <span data-ttu-id="436ec-126">Azure</span><span class="sxs-lookup"><span data-stu-id="436ec-126">Azure</span></span>|
|<span data-ttu-id="436ec-127">-q</span><span class="sxs-lookup"><span data-stu-id="436ec-127">-q</span></span>|<span data-ttu-id="436ec-128">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="436ec-128">Optional</span></span>|<span data-ttu-id="436ec-129">Určuje toorun instalačního programu v bezobslužném režimu</span><span class="sxs-lookup"><span data-stu-id="436ec-129">Specifies toorun installer in silent mode</span></span>| <span data-ttu-id="436ec-130">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="436ec-130">N/A</span></span>|


#### <a name="mobility-service-configuration-command-line"></a><span data-ttu-id="436ec-131">Konfigurace služby mobility příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="436ec-131">Mobility Service configuration command-line</span></span>

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|<span data-ttu-id="436ec-132">Parametr</span><span class="sxs-lookup"><span data-stu-id="436ec-132">Parameter</span></span>|<span data-ttu-id="436ec-133">Typ</span><span class="sxs-lookup"><span data-stu-id="436ec-133">Type</span></span>|<span data-ttu-id="436ec-134">Popis</span><span class="sxs-lookup"><span data-stu-id="436ec-134">Description</span></span>|<span data-ttu-id="436ec-135">Možné hodnoty</span><span class="sxs-lookup"><span data-stu-id="436ec-135">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="436ec-136">-i</span><span class="sxs-lookup"><span data-stu-id="436ec-136">-i</span></span> |<span data-ttu-id="436ec-137">Povinné</span><span class="sxs-lookup"><span data-stu-id="436ec-137">Mandatory</span></span>|<span data-ttu-id="436ec-138">IP hello konfigurační Server</span><span class="sxs-lookup"><span data-stu-id="436ec-138">IP of hello Configuration Server</span></span>|<span data-ttu-id="436ec-139">Libovolná platná IP adresa</span><span class="sxs-lookup"><span data-stu-id="436ec-139">Any valid IP Address</span></span>|
|<span data-ttu-id="436ec-140">-P</span><span class="sxs-lookup"><span data-stu-id="436ec-140">-P</span></span> |<span data-ttu-id="436ec-141">Povinné</span><span class="sxs-lookup"><span data-stu-id="436ec-141">Mandatory</span></span>|<span data-ttu-id="436ec-142">Uložení přístupové heslo pro připojení k hello celého souboru cestě hello souboru</span><span class="sxs-lookup"><span data-stu-id="436ec-142">Full file path hello file where hello connection passphrase is saved</span></span>|<span data-ttu-id="436ec-143">Všechny platné složce.</span><span class="sxs-lookup"><span data-stu-id="436ec-143">Any valid folder</span></span>|
