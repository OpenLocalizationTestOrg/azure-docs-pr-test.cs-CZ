---
title: "aaaDeploy hello služba Mobility obnovení lokality pomocí Azure Automation DSC. | Microsoft Docs"
description: "Popisuje, jak nasazovat toouse Azure Automation DSC tooautomatically hello Azure Site Recovery Mobility service a Azure agenta pro virtuální počítač VMware a tooAzure replikace fyzického serveru"
services: site-recovery
documentationcenter: 
author: krnese
manager: lorenr
editor: 
ms.assetid: 1f8cd3ac-0522-48eb-a5f0-679ee9192ddb
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: krnese
ms.openlocfilehash: 52cdd13ceb61718a21137180c55db86919af5929
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a><span data-ttu-id="2a549-103">Nasazení služby Mobility hello s Azure Automation DSC pro replikaci virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2a549-103">Deploy hello Mobility service with Azure Automation DSC for replication of VM</span></span>
<span data-ttu-id="2a549-104">V Operations Management Suite jsme poskytují komplexní zálohování a řešení pro zotavení po havárii, které můžete použít jako součást plánu pro kontinuitu podnikových.</span><span class="sxs-lookup"><span data-stu-id="2a549-104">In Operations Management Suite, we provide you with a comprehensive backup and disaster recovery solution that you can use as part of your business continuity plan.</span></span>

<span data-ttu-id="2a549-105">Spuštění této cesty společně s technologií Hyper-V pomocí repliky technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="2a549-105">We started this journey together with Hyper-V by using Hyper-V Replica.</span></span> <span data-ttu-id="2a549-106">Ale rozšířené toosupport heterogenní instalace vzhledem k tomu, že zákazníci, mají více hypervisorů a platformy v jejich cloudy.</span><span class="sxs-lookup"><span data-stu-id="2a549-106">But we have expanded toosupport a heterogeneous setup because customers have multiple hypervisors and platforms in their clouds.</span></span>

<span data-ttu-id="2a549-107">Pokud používáte úlohy VMware nebo fyzických serverů dnes, server pro správu se spustí všechny hello součásti Azure Site Recovery ve vaší prostředí toohandle hello komunikace a data replikace s Azure, případě Azure je cíl.</span><span class="sxs-lookup"><span data-stu-id="2a549-107">If you are running VMware workloads and/or physical servers today, a management server runs all of hello Azure Site Recovery components in your environment toohandle hello communication and data replication with Azure, when Azure is your destination.</span></span>

## <a name="deploy-hello-site-recovery-mobility-service-by-using-automation-dsc"></a><span data-ttu-id="2a549-108">Nasazení služby Site Recovery Mobility hello pomocí Automation DSC</span><span class="sxs-lookup"><span data-stu-id="2a549-108">Deploy hello Site Recovery Mobility service by using Automation DSC</span></span>
<span data-ttu-id="2a549-109">Začněme tím, provádění rychlé rozpis jaké tento server pro správu.</span><span class="sxs-lookup"><span data-stu-id="2a549-109">Let's start by doing a quick breakdown of what this management server does.</span></span>

<span data-ttu-id="2a549-110">server pro správu Hello spouští několik rolí serveru.</span><span class="sxs-lookup"><span data-stu-id="2a549-110">hello management server runs several server roles.</span></span> <span data-ttu-id="2a549-111">Jedna z těchto rolí se *konfigurace*, který koordinuje komunikaci a spravuje procesy data replikace a obnovení.</span><span class="sxs-lookup"><span data-stu-id="2a549-111">One of these roles is *configuration*, which coordinates communication and manages data replication and recovery processes.</span></span>

<span data-ttu-id="2a549-112">Kromě toho hello *proces* role funguje jako replikační brána.</span><span class="sxs-lookup"><span data-stu-id="2a549-112">In addition, hello *process* role acts as a replication gateway.</span></span> <span data-ttu-id="2a549-113">Tato role přijímá data replikace z chráněných zdrojových počítačů, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odešle ji tooan účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="2a549-113">This role receives replication data from protected source machines, optimizes it with caching, compression, and encryption, and then sends it tooan Azure storage account.</span></span> <span data-ttu-id="2a549-114">Jeden z hello funkce pro roli hello procesu se taky toopush instalaci počítače tooprotected služby Mobility hello a provádět automatického zjišťování virtuálních počítačů VMware.</span><span class="sxs-lookup"><span data-stu-id="2a549-114">One of hello functions for hello process role is also toopush installation of hello Mobility service tooprotected machines and perform automatic discovery of VMware VMs.</span></span>

<span data-ttu-id="2a549-115">Pokud dojde navrácení služeb po obnovení z Azure, hello *hlavní cíl* role, bude zpracovávat data replikace hello jako součást této operace.</span><span class="sxs-lookup"><span data-stu-id="2a549-115">If there's a failback from Azure, hello *master target* role will handle hello replication data as part of this operation.</span></span>

<span data-ttu-id="2a549-116">Pro počítače chráněné hello spoléháme na hello *služba Mobility*.</span><span class="sxs-lookup"><span data-stu-id="2a549-116">For hello protected machines, we rely on hello *Mobility service*.</span></span> <span data-ttu-id="2a549-117">Tato součást je nasazené tooevery počítače (virtuální počítač VMware nebo fyzických serverů), které chcete tooreplicate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2a549-117">This component is deployed tooevery machine (VMware VM or physical server) that you want tooreplicate tooAzure.</span></span> <span data-ttu-id="2a549-118">Se zaznamenává datové zápisy na počítači hello a předává je na serveru pro správu toohello (proces role).</span><span class="sxs-lookup"><span data-stu-id="2a549-118">It captures data writes on hello machine and forwards them toohello management server (process role).</span></span>

<span data-ttu-id="2a549-119">V případě, že pracujete s kontinuity podnikových procesů, je důležité toounderstand úlohy, infrastruktury a součásti hello zahrnuta.</span><span class="sxs-lookup"><span data-stu-id="2a549-119">When you're dealing with business continuity, it's important toounderstand your workloads, your infrastructure, and hello components involved.</span></span> <span data-ttu-id="2a549-120">Potom můžete splňovat požadavky hello cíli času obnovení (RTO) a cíl bodu obnovení (RPO).</span><span class="sxs-lookup"><span data-stu-id="2a549-120">You can then meet hello requirements for your recovery time objective (RTO) and recovery point objective (RPO).</span></span> <span data-ttu-id="2a549-121">V tomto kontextu hello služba Mobility je klíče tooensuring která jsou chráněná úlohy, jako byste očekávali.</span><span class="sxs-lookup"><span data-stu-id="2a549-121">In this context, hello Mobility service is key tooensuring that your workloads are protected as you would expect.</span></span>

<span data-ttu-id="2a549-122">Jak tedy, optimalizované způsobem, zajistíte, že máte spolehlivé chráněné instalace pomoci z některé součásti služby Operations Management Suite?</span><span class="sxs-lookup"><span data-stu-id="2a549-122">So how can you, in an optimized way, ensure that you have a reliable protected setup with help from some Operations Management Suite components?</span></span>

<span data-ttu-id="2a549-123">Tento článek obsahuje příklad použití Azure Automation požadovaného stavu konfigurace (DSC), společně s Site Recovery, tooensure který:</span><span class="sxs-lookup"><span data-stu-id="2a549-123">This article provides an example of how you can use Azure Automation Desired State Configuration (DSC), together with Site Recovery, tooensure that:</span></span>

* <span data-ttu-id="2a549-124">Služba Mobility Hello a agenta virtuálního počítače Azure jsou nasazené toohello počítače s Windows, které chcete tooprotect.</span><span class="sxs-lookup"><span data-stu-id="2a549-124">hello Mobility service and Azure VM agent are deployed toohello Windows machines that you want tooprotect.</span></span>
* <span data-ttu-id="2a549-125">Služba Mobility Hello a agenta virtuálního počítače Azure jsou vždy spuštěny Azure je cílem replikace hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-125">hello Mobility service and Azure VM agent are always running when Azure is hello replication target.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a549-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2a549-126">Prerequisites</span></span>
* <span data-ttu-id="2a549-127">Instalace vyžaduje hello toostore s úložiště</span><span class="sxs-lookup"><span data-stu-id="2a549-127">A repository toostore hello required setup</span></span>
* <span data-ttu-id="2a549-128">Úložiště toostore hello vyžaduje heslo tooregister se serverem pro správu hello</span><span class="sxs-lookup"><span data-stu-id="2a549-128">A repository toostore hello required passphrase tooregister with hello management server</span></span>

  > [!NOTE]
  > <span data-ttu-id="2a549-129">Jedinečné heslo se vygeneruje pro každý server pro správu.</span><span class="sxs-lookup"><span data-stu-id="2a549-129">A unique passphrase is generated for each management server.</span></span> <span data-ttu-id="2a549-130">Chcete-li toodeploy více serverů pro správu, máte tooensure této hello správné heslo je uloženo v souboru passphrase.txt hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-130">If you are going toodeploy multiple management servers, you have tooensure that hello correct passphrase is stored in hello passphrase.txt file.</span></span>
  >
  >
* <span data-ttu-id="2a549-131">Windows Management Framework (WMF) 5.0 nainstalován v počítačích hello chcete tooenable pro ochranu (požadavek pro Automation DSC)</span><span class="sxs-lookup"><span data-stu-id="2a549-131">Windows Management Framework (WMF) 5.0 installed on hello machines that you want tooenable for protection (a requirement for Automation DSC)</span></span>

  > [!NOTE]
  > <span data-ttu-id="2a549-132">Pokud chcete počítače toouse DSC pro systém Windows, které mají WMF 4.0 nainstalovat, najdete v tématu hello [použití DSC v odpojených prostředích](## Use DSC in disconnected environments).</span><span class="sxs-lookup"><span data-stu-id="2a549-132">If you want toouse DSC for Windows machines that have WMF 4.0 installed, see hello section [Use DSC in disconnected environments](## Use DSC in disconnected environments).</span></span>
  

<span data-ttu-id="2a549-133">Služba Mobility Hello dají nainstalovat přes hello příkazového řádku a přijímá několik argumentů.</span><span class="sxs-lookup"><span data-stu-id="2a549-133">hello Mobility service can be installed through hello command line and accepts several arguments.</span></span> <span data-ttu-id="2a549-134">To je důvod, proč potřebujete toohave hello binárních souborů (po jejich extrahování z vašeho nastavení) a ukládat je na místě, kde je můžete obnovit pomocí konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="2a549-134">That’s why you need toohave hello binaries (after extracting them from your setup) and store them in a place where you can retrieve them by using a DSC configuration.</span></span>

## <a name="step-1-extract-binaries"></a><span data-ttu-id="2a549-135">Krok 1: Extrahování binárních souborů</span><span class="sxs-lookup"><span data-stu-id="2a549-135">Step 1: Extract binaries</span></span>
1. <span data-ttu-id="2a549-136">tooextract hello soubory, které potřebujete pro tento instalační program, procházet toohello následující directory na vašem serveru pro správu:</span><span class="sxs-lookup"><span data-stu-id="2a549-136">tooextract hello files that you need for this setup, browse toohello following directory on your management server:</span></span>

    <span data-ttu-id="2a549-137">**Recovery\home\svsystems\pushinstallsvc\repository \Microsoft azure lokality**</span><span class="sxs-lookup"><span data-stu-id="2a549-137">**\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**</span></span>

    <span data-ttu-id="2a549-138">V této složce měli byste vidět soubor MSI s názvem:</span><span class="sxs-lookup"><span data-stu-id="2a549-138">In this folder, you should see an MSI file named:</span></span>

    <span data-ttu-id="2a549-139">**Microsoft ASR_UA_version_Windows_GA_date_Release.exe**</span><span class="sxs-lookup"><span data-stu-id="2a549-139">**Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**</span></span>

    <span data-ttu-id="2a549-140">Použijte následující příkaz tooextract hello instalačního programu hello:</span><span class="sxs-lookup"><span data-stu-id="2a549-140">Use hello following command tooextract hello installer:</span></span>

    <span data-ttu-id="2a549-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span><span class="sxs-lookup"><span data-stu-id="2a549-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span></span>
2. <span data-ttu-id="2a549-142">Vyberte všechny soubory a jejich odeslání tooa komprimované složky (ZIP).</span><span class="sxs-lookup"><span data-stu-id="2a549-142">Select all files and send them tooa compressed (zipped) folder.</span></span>

<span data-ttu-id="2a549-143">Nyní máte hello binární soubory, je nutné tooautomate hello instalace služby Mobility hello pomocí Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="2a549-143">You now have hello binaries that you need tooautomate hello setup of hello Mobility service by using Automation DSC.</span></span>

### <a name="passphrase"></a><span data-ttu-id="2a549-144">Přístupové heslo</span><span class="sxs-lookup"><span data-stu-id="2a549-144">Passphrase</span></span>
<span data-ttu-id="2a549-145">Dále musíte toodetermine místo tooplace této komprimované složky.</span><span class="sxs-lookup"><span data-stu-id="2a549-145">Next, you need toodetermine where you want tooplace this zipped folder.</span></span> <span data-ttu-id="2a549-146">Účet úložiště Azure můžete použít jako uvedené později toostore hello heslo, které potřebujete k instalaci hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-146">You can use an Azure storage account, as shown later, toostore hello passphrase that you need for hello setup.</span></span> <span data-ttu-id="2a549-147">Hello agenta bude potom proveďte registraci se serverem pro správu hello jako součást procesu hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-147">hello agent will then register with hello management server as part of hello process.</span></span>

<span data-ttu-id="2a549-148">Hello heslo, které jste získali při nasazení serveru pro správu hello můžete uložit jako passphrase.txt tooa textového souboru.</span><span class="sxs-lookup"><span data-stu-id="2a549-148">hello passphrase that you got when you deployed hello management server can be saved tooa text file as passphrase.txt.</span></span>

<span data-ttu-id="2a549-149">Umístěte hello komprimované složce a hello heslo v vyhrazené kontejneru v hello účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="2a549-149">Place both hello zipped folder and hello passphrase in a dedicated container in hello Azure storage account.</span></span>

![Umístění složky](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

<span data-ttu-id="2a549-151">Pokud dáváte přednost tookeep tyto soubory ve sdílené složce v síti, můžete tak učinit.</span><span class="sxs-lookup"><span data-stu-id="2a549-151">If you prefer tookeep these files on a share on your network, you can do so.</span></span> <span data-ttu-id="2a549-152">Stačí tooensure, má přístup hello DSC prostředek, který budete používat později a můžete získat hello nastavení a heslo.</span><span class="sxs-lookup"><span data-stu-id="2a549-152">You just need tooensure that hello DSC resource that you will be using later has access and can get hello setup and passphrase.</span></span>

## <a name="step-2-create-hello-dsc-configuration"></a><span data-ttu-id="2a549-153">Krok 2: Vytvoření konfigurace hello DSC</span><span class="sxs-lookup"><span data-stu-id="2a549-153">Step 2: Create hello DSC configuration</span></span>
<span data-ttu-id="2a549-154">Instalační program Hello závisí na WMF 5.0.</span><span class="sxs-lookup"><span data-stu-id="2a549-154">hello setup depends on WMF 5.0.</span></span> <span data-ttu-id="2a549-155">Pro počítač toosuccessfully hello použít konfiguraci hello prostřednictvím Automation DSC, WMF 5.0 musí toobe přítomen.</span><span class="sxs-lookup"><span data-stu-id="2a549-155">For hello machine toosuccessfully apply hello configuration through Automation DSC, WMF 5.0 needs toobe present.</span></span>

<span data-ttu-id="2a549-156">prostředí Hello používá následující příklad konfigurace DSC hello:</span><span class="sxs-lookup"><span data-stu-id="2a549-156">hello environment uses hello following example DSC configuration:</span></span>

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
<span data-ttu-id="2a549-157">provede konfigurace Hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="2a549-157">hello configuration will do hello following:</span></span>

* <span data-ttu-id="2a549-158">proměnné Hello bude informovat hello konfigurace, kde tooget hello binární soubory pro služby Mobility hello a hello agenta virtuálního počítače Azure, kde tooget hello přístupové heslo, a kde toostore hello výstup.</span><span class="sxs-lookup"><span data-stu-id="2a549-158">hello variables will tell hello configuration where tooget hello binaries for hello Mobility service and hello Azure VM agent, where tooget hello passphrase, and where toostore hello output.</span></span>
* <span data-ttu-id="2a549-159">Hello konfigurace naimportuje hello xPSDesiredStateConfiguration DSC prostředků, tak, aby můžete použít `xRemoteFile` toodownload hello soubory z úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-159">hello configuration will import hello xPSDesiredStateConfiguration DSC resource, so that you can use `xRemoteFile` toodownload hello files from hello repository.</span></span>
* <span data-ttu-id="2a549-160">Konfigurace Hello vytvoří adresáře, kde chcete toostore hello binární soubory.</span><span class="sxs-lookup"><span data-stu-id="2a549-160">hello configuration will create a directory where you want toostore hello binaries.</span></span>
* <span data-ttu-id="2a549-161">Hello archivu prostředků bude extrahujte hello soubory z komprimované složky hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-161">hello archive resource will extract hello files from hello zipped folder.</span></span>
* <span data-ttu-id="2a549-162">Hello balíček instalace prostředků bude z hello UNIFIEDAGENT instalaci služby Mobility hello. Instalační program EXE s konkrétní argumenty hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-162">hello package Install resource will install hello Mobility service from hello UNIFIEDAGENT.EXE installer with hello specific arguments.</span></span> <span data-ttu-id="2a549-163">(hello proměnné, které vytvořit hello argumenty potřebovat změnit toobe tooreflect prostředí.)</span><span class="sxs-lookup"><span data-stu-id="2a549-163">(hello variables that construct hello arguments need toobe changed tooreflect your environment.)</span></span>
* <span data-ttu-id="2a549-164">Hello balíček prostředků AzureAgent nainstaluje agenta hello virtuálního počítače Azure, který se doporučuje pro každý virtuální počítač, který běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="2a549-164">hello package AzureAgent resource will install hello Azure VM agent, which is recommended on every VM that runs in Azure.</span></span> <span data-ttu-id="2a549-165">agent virtuálního počítače Azure Hello také je možné tooadd toohello rozšíření virtuálního počítače po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="2a549-165">hello Azure VM agent also makes it possible tooadd extensions toohello VM after failover.</span></span>
* <span data-ttu-id="2a549-166">Hello prostředek služby nebo prostředky zajišťují, že hello související služby Mobility a hello Azure vždy běží služby.</span><span class="sxs-lookup"><span data-stu-id="2a549-166">hello service resource or resources will ensure that hello related Mobility services and hello Azure services are always running.</span></span>

<span data-ttu-id="2a549-167">Uložte konfiguraci hello jako **ASRMobilityService**.</span><span class="sxs-lookup"><span data-stu-id="2a549-167">Save hello configuration as **ASRMobilityService**.</span></span>

> [!NOTE]
> <span data-ttu-id="2a549-168">Nezapomeňte tooreplace hello CSIP ve vaší konfiguraci tooreflect hello samotnou správu serveru, tak, aby hello agenta správně připojí a bude používat hello správné heslo.</span><span class="sxs-lookup"><span data-stu-id="2a549-168">Remember tooreplace hello CSIP in your configuration tooreflect hello actual management server, so that hello agent will be connected correctly and will use hello correct passphrase.</span></span>
>
>

## <a name="step-3-upload-tooautomation-dsc"></a><span data-ttu-id="2a549-169">Krok 3: Nahrát na server tooAutomation DSC</span><span class="sxs-lookup"><span data-stu-id="2a549-169">Step 3: Upload tooAutomation DSC</span></span>
<span data-ttu-id="2a549-170">Protože hello DSC konfigurace, které jste provedli importuje modul požadovaný prostředek DSC (xPSDesiredStateConfiguration), je třeba tooimport Tenhle modul ve službě Automation před nahráním hello DSC konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2a549-170">Because hello DSC configuration that you made will import a required DSC resource module (xPSDesiredStateConfiguration), you need tooimport that module in Automation before you upload hello DSC configuration.</span></span>

<span data-ttu-id="2a549-171">Přihlášení tooyour účet Automation, procházet příliš**prostředky** > **moduly**a klikněte na tlačítko **procházet galerii**.</span><span class="sxs-lookup"><span data-stu-id="2a549-171">Sign in tooyour Automation account, browse too**Assets** > **Modules**, and click **Browse Gallery**.</span></span>

<span data-ttu-id="2a549-172">Zde můžete vyhledat hello modulu a importujte ho tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="2a549-172">Here you can search for hello module and import it tooyour account.</span></span>

![Import modulu](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

<span data-ttu-id="2a549-174">Když toto dokončíte, přejděte tooyour počítače, ve které máte nainstalované moduly Azure Resource Manageru hello a pokračovat konfigurace DSC tooimport hello nově vytvořený.</span><span class="sxs-lookup"><span data-stu-id="2a549-174">When you finish this, go tooyour machine where you have hello Azure Resource Manager modules installed and proceed tooimport hello newly created DSC configuration.</span></span>

### <a name="import-cmdlets"></a><span data-ttu-id="2a549-175">Rutiny importu</span><span class="sxs-lookup"><span data-stu-id="2a549-175">Import cmdlets</span></span>
<span data-ttu-id="2a549-176">V prostředí PowerShell Přihlaste se tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="2a549-176">In PowerShell, sign in tooyour Azure subscription.</span></span> <span data-ttu-id="2a549-177">Upravte tooreflect hello rutin prostředí a zaznamenat informace o účtu Automation v proměnné:</span><span class="sxs-lookup"><span data-stu-id="2a549-177">Modify hello cmdlets tooreflect your environment and capture your Automation account information in a variable:</span></span>

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

<span data-ttu-id="2a549-178">Odešlete hello konfigurace tooAutomation DSC pomocí hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="2a549-178">Upload hello configuration tooAutomation DSC by using hello following cmdlet:</span></span>

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-hello-configuration-in-automation-dsc"></a><span data-ttu-id="2a549-179">Zkompilování konfigurace hello v Automation DSC</span><span class="sxs-lookup"><span data-stu-id="2a549-179">Compile hello configuration in Automation DSC</span></span>
<span data-ttu-id="2a549-180">Dále musíte toocompile hello konfigurace v Automation DSC, tak, aby bylo možné spustit tooit tooregister uzlů.</span><span class="sxs-lookup"><span data-stu-id="2a549-180">Next, you need toocompile hello configuration in Automation DSC, so that you can start tooregister nodes tooit.</span></span> <span data-ttu-id="2a549-181">Můžete toho dosáhnout spuštěním následující rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="2a549-181">You achieve that by running hello following cmdlet:</span></span>

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

<span data-ttu-id="2a549-182">Může to trvat několik minut, protože v podstatě nasazujete hello konfigurace toohello hostované DSC vyžádání služby.</span><span class="sxs-lookup"><span data-stu-id="2a549-182">This can take a few minutes, because you're basically deploying hello configuration toohello hosted DSC pull service.</span></span>

<span data-ttu-id="2a549-183">Po zkompilujete hello konfigurace, můžete načíst informace o úlohách hello pomocí prostředí PowerShell (Get-AzureRmAutomationDscCompilationJob) nebo pomocí hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2a549-183">After you compile hello configuration, you can retrieve hello job information by using PowerShell (Get-AzureRmAutomationDscCompilationJob) or by using hello [Azure portal](https://portal.azure.com/).</span></span>

![Načtení úlohy](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

<span data-ttu-id="2a549-185">Teď úspěšně publikována a nahrát váš tooAutomation DSC konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="2a549-185">You have now successfully published and uploaded your DSC configuration tooAutomation DSC.</span></span>

## <a name="step-4-onboard-machines-tooautomation-dsc"></a><span data-ttu-id="2a549-186">Krok 4: Zařadit počítače tooAutomation DSC</span><span class="sxs-lookup"><span data-stu-id="2a549-186">Step 4: Onboard machines tooAutomation DSC</span></span>
> [!NOTE]
> <span data-ttu-id="2a549-187">Jedním z hello předpoklady pro dokončení tohoto scénáře je, že vaše počítače Windows jsou aktualizovány s hello nejnovější verze WMF.</span><span class="sxs-lookup"><span data-stu-id="2a549-187">One of hello prerequisites for completing this scenario is that your Windows machines are updated with hello latest version of WMF.</span></span> <span data-ttu-id="2a549-188">Můžete stáhnout a nainstalovat správnou verzi hello pro vaši platformu z hello [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).</span><span class="sxs-lookup"><span data-stu-id="2a549-188">You can download and install hello correct version for your platform from hello [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).</span></span>
>
>

<span data-ttu-id="2a549-189">Nyní vytvoříte metaconfig pro DSC, že použijete tooyour uzlů.</span><span class="sxs-lookup"><span data-stu-id="2a549-189">You will now create a metaconfig for DSC that you will apply tooyour nodes.</span></span> <span data-ttu-id="2a549-190">toosucceed s tím, potřebujete tooretrieve hello koncový bod adresy URL a hello primární klíč pro vybrané účtu Automation v Azure.</span><span class="sxs-lookup"><span data-stu-id="2a549-190">toosucceed with this, you need tooretrieve hello endpoint URL and hello primary key for your selected Automation account in Azure.</span></span> <span data-ttu-id="2a549-191">Můžete najít tyto hodnoty v části **klíče** na hello **všechna nastavení** okně hello účet Automation.</span><span class="sxs-lookup"><span data-stu-id="2a549-191">You can find these values under **Keys** on hello **All settings** blade for hello Automation account.</span></span>

![Hodnoty klíče](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

<span data-ttu-id="2a549-193">V tomto příkladu budete mít fyzický server Windows Server 2012 R2 chcete tooprotect pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="2a549-193">In this example, you have a Windows Server 2012 R2 physical server that you want tooprotect by using Site Recovery.</span></span>

### <a name="check-for-any-pending-file-rename-operations-in-hello-registry"></a><span data-ttu-id="2a549-194">Zkontrolujte všechny čekající operace přejmenování souboru v registru hello</span><span class="sxs-lookup"><span data-stu-id="2a549-194">Check for any pending file rename operations in hello registry</span></span>
<span data-ttu-id="2a549-195">Než začnete tooassociate hello server s koncovým bodem hello Automation DSC, doporučujeme zkontrolovat pro všechny čekající operace přejmenování souboru v registru hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-195">Before you start tooassociate hello server with hello Automation DSC endpoint, we recommend that you check for any pending file rename operations in hello registry.</span></span> <span data-ttu-id="2a549-196">Může vylučují hello instalace z dokončení kvůli tooa čekat na restartování.</span><span class="sxs-lookup"><span data-stu-id="2a549-196">They might prohibit hello setup from finishing due tooa pending reboot.</span></span>

<span data-ttu-id="2a549-197">Spusťte následující rutinu tooverify se žádné čeká na restartování na serveru hello hello:</span><span class="sxs-lookup"><span data-stu-id="2a549-197">Run hello following cmdlet tooverify that there’s no pending reboot on hello server:</span></span>

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
<span data-ttu-id="2a549-198">Pokud se zobrazí prázdné, jste OK tooproceed.</span><span class="sxs-lookup"><span data-stu-id="2a549-198">If this shows empty, you are OK tooproceed.</span></span> <span data-ttu-id="2a549-199">V opačném případě by to vyřešit tak, že restartování serveru hello během časového období údržby.</span><span class="sxs-lookup"><span data-stu-id="2a549-199">If not, you should address this by rebooting hello server during a maintenance window.</span></span>

<span data-ttu-id="2a549-200">Konfigurace hello tooapply na serveru hello, otevřete hello prostředí PowerShell integrovaném skriptovacím prostředí (ISE) a spusťte následující skript hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-200">tooapply hello configuration on hello server, start hello PowerShell Integrated Scripting Environment (ISE) and run hello following script.</span></span> <span data-ttu-id="2a549-201">Toto je v podstatě místní konfigurace DSC, který bude pokyn hello správce místní konfigurace modulu tooregister s hello služby Automation DSC a načíst konkrétní konfiguraci hello (ASRMobilityService.localhost).</span><span class="sxs-lookup"><span data-stu-id="2a549-201">This is essentially a DSC local configuration that will instruct hello Local Configuration Manager engine tooregister with hello Automation DSC service and retrieve hello specific configuration (ASRMobilityService.localhost).</span></span>

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

<span data-ttu-id="2a549-202">Tato konfigurace způsobí hello správce místní konfigurace modulu tooregister samotné s Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="2a549-202">This configuration will cause hello Local Configuration Manager engine tooregister itself with Automation DSC.</span></span> <span data-ttu-id="2a549-203">Také se určí, jak pracovat hello modul, jak ho postupovat, pokud je konfigurace odlišily (ApplyAndAutoCorrect) a jak by měla postupovat s konfigurací hello Pokud je vyžadován restart.</span><span class="sxs-lookup"><span data-stu-id="2a549-203">It will also determine how hello engine should operate, what it should do if there's a configuration drift (ApplyAndAutoCorrect), and how it should proceed with hello configuration if a reboot is required.</span></span>

<span data-ttu-id="2a549-204">Po spuštění tohoto skriptu hello uzlu by měl začínat tooregister Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="2a549-204">After you run this script, hello node should start tooregister with Automation DSC.</span></span>

![Registrace uzlu v průběhu](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

<span data-ttu-id="2a549-206">Pokud přejdete zpět toohello portálu Azure, najdete v tomto uzlu nově zaregistrovaný hello má nyní zobrazovaly v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-206">If you go back toohello Azure portal, you can see that hello newly registered node has now appeared in hello portal.</span></span>

![Registrovaný uzlu hello portálu](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

<span data-ttu-id="2a549-208">Na serveru hello můžete spustit hello následující tooverify rutiny, která hello uzel má správně zaregistrovány prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="2a549-208">On hello server, you can run hello following PowerShell cmdlet tooverify that hello node has been registered correctly:</span></span>

```powershell
Get-DscLocalConfigurationManager
```

<span data-ttu-id="2a549-209">Po konfiguraci hello načtený a použitých toohello serveru, můžete to ověřit spuštěním následující rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="2a549-209">After hello configuration has been pulled and applied toohello server, you can verify this by running hello following cmdlet:</span></span>

```powershell
Get-DscConfigurationStatus
```

<span data-ttu-id="2a549-210">výstup Hello ukazuje, že tento server hello má úspěšně vyžádat jeho konfigurace:</span><span class="sxs-lookup"><span data-stu-id="2a549-210">hello output shows that hello server has successfully pulled its configuration:</span></span>

![Výstup](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

<span data-ttu-id="2a549-212">Kromě toho instalace služby Mobility hello má svou vlastní protokolu, který se nachází v *SystemDrive*\ProgramData\ASRSetupLogs.</span><span class="sxs-lookup"><span data-stu-id="2a549-212">In addition, hello Mobility service setup has its own log that can be found at *SystemDrive*\ProgramData\ASRSetupLogs.</span></span>

<span data-ttu-id="2a549-213">Je to.</span><span class="sxs-lookup"><span data-stu-id="2a549-213">That’s it.</span></span> <span data-ttu-id="2a549-214">Teď úspěšně nasazen a zaregistrován hello služba Mobility na počítači hello chcete tooprotect pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="2a549-214">You have now successfully deployed and registered hello Mobility service on hello machine that you want tooprotect by using Site Recovery.</span></span> <span data-ttu-id="2a549-215">DSC se přesvědčíte, že hello požadované vždy běží služby.</span><span class="sxs-lookup"><span data-stu-id="2a549-215">DSC will make sure that hello required services are always running.</span></span>

![Úspěšné nasazení](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

<span data-ttu-id="2a549-217">Po server pro správu hello zjistí hello úspěšné nasazení, můžete nakonfigurovat ochranu a zapnout replikaci na počítači hello pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="2a549-217">After hello management server detects hello successful deployment, you can configure protection and enable replication on hello machine by using Site Recovery.</span></span>

## <a name="use-dsc-in-disconnected-environments"></a><span data-ttu-id="2a549-218">Použití DSC v odpojených prostředích</span><span class="sxs-lookup"><span data-stu-id="2a549-218">Use DSC in disconnected environments</span></span>
<span data-ttu-id="2a549-219">Pokud vaše počítače nejsou připojené toohello Internetu, můžete stále závisí na DSC toodeploy a nakonfigurovat službu Mobility hello na hello úlohy, které chcete tooprotect.</span><span class="sxs-lookup"><span data-stu-id="2a549-219">If your machines aren’t connected toohello Internet, you can still rely on DSC toodeploy and configure hello Mobility service on hello workloads that you want tooprotect.</span></span>

<span data-ttu-id="2a549-220">Můžete vytvořit vlastní server DSC za instanci ve vašem prostředí tooessentially zadejte hello stejné funkce, které jste získali z Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="2a549-220">You can instantiate your own DSC pull server in your environment tooessentially provide hello same capabilities that you get from Automation DSC.</span></span> <span data-ttu-id="2a549-221">To znamená, bude hello klientům načítat konfigurace hello (po dokončení registrace) toohello DSC koncový bod.</span><span class="sxs-lookup"><span data-stu-id="2a549-221">That is, hello clients will pull hello configuration (after it's registered) toohello DSC endpoint.</span></span> <span data-ttu-id="2a549-222">Další možností je však toomanually nabízené hello DSC konfigurace tooyour počítače, místně nebo vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="2a549-222">However, another option is toomanually push hello DSC configuration tooyour machines, either locally or remotely.</span></span>

<span data-ttu-id="2a549-223">Všimněte si, že v tomto příkladu je přidaný parametr pro název počítače hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-223">Note that in this example, there's an added parameter for hello computer name.</span></span> <span data-ttu-id="2a549-224">Hello vzdálené soubory jsou umístěné ve vzdálené sdílené složce, která by měla být přístupné hello počítače, které chcete tooprotect.</span><span class="sxs-lookup"><span data-stu-id="2a549-224">hello remote files are now located on a remote share that should be accessible by hello machines that you want tooprotect.</span></span> <span data-ttu-id="2a549-225">Hello konec skriptu hello představuje hello konfigurace a pak spustí tooapply hello DSC konfigurace toohello cílový počítač.</span><span class="sxs-lookup"><span data-stu-id="2a549-225">hello end of hello script enacts hello configuration and then starts tooapply hello DSC configuration toohello target computer.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="2a549-226">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2a549-226">Prerequisites</span></span>
<span data-ttu-id="2a549-227">Zkontrolujte, zda že je nainstalován tento modul PowerShell xPSDesiredStateConfiguration hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-227">Make sure that hello xPSDesiredStateConfiguration PowerShell module is installed.</span></span> <span data-ttu-id="2a549-228">Pro počítače s Windows, kde je nainstalován WMF 5.0 můžete nainstalovat modul xPSDesiredStateConfiguration hello spuštěním následující rutiny na cílových počítačích hello hello:</span><span class="sxs-lookup"><span data-stu-id="2a549-228">For Windows machines where WMF 5.0 is installed, you can install hello xPSDesiredStateConfiguration module by running hello following cmdlet on hello target machines:</span></span>

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

<span data-ttu-id="2a549-229">Můžete také stáhnout a uložit hello modulu v případě, že potřebujete toodistribute ho tooWindows počítače, které mají WMF 4.0.</span><span class="sxs-lookup"><span data-stu-id="2a549-229">You can also download and save hello module in case you need toodistribute it tooWindows machines that have WMF 4.0.</span></span> <span data-ttu-id="2a549-230">Spusťte tuto rutinu na počítači, kde se nachází PowerShellGet (WMF 5.0):</span><span class="sxs-lookup"><span data-stu-id="2a549-230">Run this cmdlet on a machine where PowerShellGet (WMF 5.0) is present:</span></span>

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

<span data-ttu-id="2a549-231">Pro WMF 4.0, ujistěte se také, že hello [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) nainstalován v počítačích hello.</span><span class="sxs-lookup"><span data-stu-id="2a549-231">Also for WMF 4.0, ensure that hello [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) is installed on hello machines.</span></span>

<span data-ttu-id="2a549-232">Hello následující konfigurace mohou poslat tooWindows počítače, které mají WMF 5.0 a WMF 4.0:</span><span class="sxs-lookup"><span data-stu-id="2a549-232">hello following configuration can be pushed tooWindows machines that have WMF 5.0 and WMF 4.0:</span></span>

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

<span data-ttu-id="2a549-233">Pokud chcete tooinstantiate vlastní DSC vyžádání obsahu server na možnostech hello toomimic vaší podnikové síti, které můžete získat z Automation DSC, najdete v části [nastavení webového serveru vyžádané replikace DSC](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="2a549-233">If you want tooinstantiate your own DSC pull server on your corporate network toomimic hello capabilities that you can get from Automation DSC, see [Setting up a DSC web pull server](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span></span>

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a><span data-ttu-id="2a549-234">Volitelné: Konfigurace DSC nasazení pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2a549-234">Optional: Deploy a DSC configuration by using an Azure Resource Manager template</span></span>
<span data-ttu-id="2a549-235">Tento článek se zaměřuje na tom, jak můžete vytvořit vlastní tooautomatically konfigurace DSC nasadit službu Mobility hello a hello agenta virtuálního počítače Azure – a ujistěte se, že jsou spuštěny v hello počítače, které chcete tooprotect.</span><span class="sxs-lookup"><span data-stu-id="2a549-235">This article has focused on how you can create your own DSC configuration tooautomatically deploy hello Mobility service and hello Azure VM Agent--and ensure that they are running on hello machines that you want tooprotect.</span></span> <span data-ttu-id="2a549-236">Máme také šablonu Azure Resource Manager, nasadíte tuto DSC konfigurace tooa nový nebo existující účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="2a549-236">We also have an Azure Resource Manager template that will deploy this DSC configuration tooa new or existing Azure Automation account.</span></span> <span data-ttu-id="2a549-237">Šablona Hello použije vstupní parametry toocreate automatizace prostředky, které budou obsahovat hello proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="2a549-237">hello template will use input parameters toocreate Automation assets that will contain hello variables for your environment.</span></span>

<span data-ttu-id="2a549-238">Poté, co nasadíte hello šablony, jednoduše najdete toostep 4 v této příručce tooonboard vašich počítačů.</span><span class="sxs-lookup"><span data-stu-id="2a549-238">After you deploy hello template, you can simply refer toostep 4 in this guide tooonboard your machines.</span></span>

<span data-ttu-id="2a549-239">Šablona Hello provede hello následující:</span><span class="sxs-lookup"><span data-stu-id="2a549-239">hello template will do hello following:</span></span>

1. <span data-ttu-id="2a549-240">Použít existující účet Automation nebo vytvořit novou</span><span class="sxs-lookup"><span data-stu-id="2a549-240">Use an existing Automation account or create a new one</span></span>
2. <span data-ttu-id="2a549-241">Trvat vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="2a549-241">Take input parameters for:</span></span>
   * <span data-ttu-id="2a549-242">ASRRemoteFile – hello umístění pro uložení instalace služby Mobility hello</span><span class="sxs-lookup"><span data-stu-id="2a549-242">ASRRemoteFile--hello location where you have stored hello Mobility service setup</span></span>
   * <span data-ttu-id="2a549-243">ASRPassphrase – hello umístění pro uložení souboru passphrase.txt hello</span><span class="sxs-lookup"><span data-stu-id="2a549-243">ASRPassphrase--hello location where you have stored hello passphrase.txt file</span></span>
   * <span data-ttu-id="2a549-244">ASRCSEndpoint – hello IP adresa serveru pro správu</span><span class="sxs-lookup"><span data-stu-id="2a549-244">ASRCSEndpoint--hello IP address of your management server</span></span>
3. <span data-ttu-id="2a549-245">Importujte modul PowerShell xPSDesiredStateConfiguration hello</span><span class="sxs-lookup"><span data-stu-id="2a549-245">Import hello xPSDesiredStateConfiguration PowerShell module</span></span>
4. <span data-ttu-id="2a549-246">Vytvoření a kompilaci konfigurace DSC hello</span><span class="sxs-lookup"><span data-stu-id="2a549-246">Create and compile hello DSC configuration</span></span>

<span data-ttu-id="2a549-247">Všechny hello předchozích kroků proběhne ve správném pořadí hello, aby mohl začít registrace vašich počítačů pro ochranu.</span><span class="sxs-lookup"><span data-stu-id="2a549-247">All hello preceding steps will happen in hello right order, so that you can start onboarding your machines for protection.</span></span>

<span data-ttu-id="2a549-248">Šablona Hello s pokyny pro nasazení, se nachází na [Githubu](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span><span class="sxs-lookup"><span data-stu-id="2a549-248">hello template, with instructions for deployment, is located on [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span></span>

<span data-ttu-id="2a549-249">Nasazení šablony hello pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="2a549-249">Deploy hello template by using PowerShell:</span></span>

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a><span data-ttu-id="2a549-250">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2a549-250">Next steps</span></span>
<span data-ttu-id="2a549-251">Poté, co nasadíte agenty služby Mobility hello, můžete [povolit replikaci](site-recovery-vmware-to-azure.md) hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2a549-251">After you deploy hello Mobility service agents, you can [enable replication](site-recovery-vmware-to-azure.md) for hello virtual machines.</span></span>
