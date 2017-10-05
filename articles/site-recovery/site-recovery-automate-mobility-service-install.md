---
title: "Nasazení služby Mobility obnovení lokality pomocí Azure Automation DSC. | Microsoft Docs"
description: "Popisuje, jak automaticky nasadit služby Azure Site Recovery Mobility a Azure agent pro virtuální počítač VMware a fyzické serveru replikaci do Azure pomocí Azure Automation DSC."
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
ms.openlocfilehash: bcc5f11afbecac8fe63935f3401dd3e2d767e8aa
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-the-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a><span data-ttu-id="45d33-103">Nasazení služby Mobility pomocí Azure Automation DSC pro replikaci virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="45d33-103">Deploy the Mobility service with Azure Automation DSC for replication of VM</span></span>
<span data-ttu-id="45d33-104">V Operations Management Suite jsme poskytují komplexní zálohování a řešení pro zotavení po havárii, které můžete použít jako součást plánu pro kontinuitu podnikových.</span><span class="sxs-lookup"><span data-stu-id="45d33-104">In Operations Management Suite, we provide you with a comprehensive backup and disaster recovery solution that you can use as part of your business continuity plan.</span></span>

<span data-ttu-id="45d33-105">Spuštění této cesty společně s technologií Hyper-V pomocí repliky technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="45d33-105">We started this journey together with Hyper-V by using Hyper-V Replica.</span></span> <span data-ttu-id="45d33-106">Ale jsme mít rozšířenou o podporu heterogenní instalační program, protože zákazníci mají více hypervisorů a platformy v jejich cloudy.</span><span class="sxs-lookup"><span data-stu-id="45d33-106">But we have expanded to support a heterogeneous setup because customers have multiple hypervisors and platforms in their clouds.</span></span>

<span data-ttu-id="45d33-107">Pokud používáte úlohy VMware nebo fyzických serverů dnes, server pro správu všech součástí Azure Site Recovery běží v prostředí pro zpracování komunikace a data replikace s Azure, když Azure je cíl.</span><span class="sxs-lookup"><span data-stu-id="45d33-107">If you are running VMware workloads and/or physical servers today, a management server runs all of the Azure Site Recovery components in your environment to handle the communication and data replication with Azure, when Azure is your destination.</span></span>

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a><span data-ttu-id="45d33-108">Nasazení služby Mobility obnovení lokality pomocí Automation DSC</span><span class="sxs-lookup"><span data-stu-id="45d33-108">Deploy the Site Recovery Mobility service by using Automation DSC</span></span>
<span data-ttu-id="45d33-109">Začněme tím, provádění rychlé rozpis jaké tento server pro správu.</span><span class="sxs-lookup"><span data-stu-id="45d33-109">Let's start by doing a quick breakdown of what this management server does.</span></span>

<span data-ttu-id="45d33-110">Server pro správu spouští několik rolí serveru.</span><span class="sxs-lookup"><span data-stu-id="45d33-110">The management server runs several server roles.</span></span> <span data-ttu-id="45d33-111">Jedna z těchto rolí se *konfigurace*, který koordinuje komunikaci a spravuje procesy data replikace a obnovení.</span><span class="sxs-lookup"><span data-stu-id="45d33-111">One of these roles is *configuration*, which coordinates communication and manages data replication and recovery processes.</span></span>

<span data-ttu-id="45d33-112">Kromě toho *proces* role funguje jako replikační brána.</span><span class="sxs-lookup"><span data-stu-id="45d33-112">In addition, the *process* role acts as a replication gateway.</span></span> <span data-ttu-id="45d33-113">Tato role přijímá data replikace z chráněných zdrojových počítačů, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odešle ji do účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="45d33-113">This role receives replication data from protected source machines, optimizes it with caching, compression, and encryption, and then sends it to an Azure storage account.</span></span> <span data-ttu-id="45d33-114">Jednou z funkcí pro roli proces je také nabízená instalace služby Mobility na chráněné počítače a provádět automatického zjišťování virtuálních počítačů VMware.</span><span class="sxs-lookup"><span data-stu-id="45d33-114">One of the functions for the process role is also to push installation of the Mobility service to protected machines and perform automatic discovery of VMware VMs.</span></span>

<span data-ttu-id="45d33-115">Pokud dojde navrácení služeb po obnovení z Azure, *hlavní cíl* role bude zpracovávat data replikace v rámci této operace.</span><span class="sxs-lookup"><span data-stu-id="45d33-115">If there's a failback from Azure, the *master target* role will handle the replication data as part of this operation.</span></span>

<span data-ttu-id="45d33-116">Pro chráněné počítače spoléháme na *služba Mobility*.</span><span class="sxs-lookup"><span data-stu-id="45d33-116">For the protected machines, we rely on the *Mobility service*.</span></span> <span data-ttu-id="45d33-117">Tato součást je nasazený na každý počítač (virtuálního počítače VMware nebo fyzických serverů), který chcete replikovat do Azure.</span><span class="sxs-lookup"><span data-stu-id="45d33-117">This component is deployed to every machine (VMware VM or physical server) that you want to replicate to Azure.</span></span> <span data-ttu-id="45d33-118">Se zaznamenává datové zápisy na počítači a předává je na serveru pro správu (proces role).</span><span class="sxs-lookup"><span data-stu-id="45d33-118">It captures data writes on the machine and forwards them to the management server (process role).</span></span>

<span data-ttu-id="45d33-119">V případě, že pracujete s kontinuity podnikových procesů, je důležité si uvědomit, úlohy, infrastruktury a související součásti.</span><span class="sxs-lookup"><span data-stu-id="45d33-119">When you're dealing with business continuity, it's important to understand your workloads, your infrastructure, and the components involved.</span></span> <span data-ttu-id="45d33-120">Potom můžete splňovat požadavky na cíli času obnovení (RTO) a cíl bodu obnovení (RPO).</span><span class="sxs-lookup"><span data-stu-id="45d33-120">You can then meet the requirements for your recovery time objective (RTO) and recovery point objective (RPO).</span></span> <span data-ttu-id="45d33-121">V tomto kontextu služba Mobility je klíčem k zajištění ochrany vašich úloh podle předpokladů.</span><span class="sxs-lookup"><span data-stu-id="45d33-121">In this context, the Mobility service is key to ensuring that your workloads are protected as you would expect.</span></span>

<span data-ttu-id="45d33-122">Jak tedy, optimalizované způsobem, zajistíte, že máte spolehlivé chráněné instalace pomoci z některé součásti služby Operations Management Suite?</span><span class="sxs-lookup"><span data-stu-id="45d33-122">So how can you, in an optimized way, ensure that you have a reliable protected setup with help from some Operations Management Suite components?</span></span>

<span data-ttu-id="45d33-123">Tento článek obsahuje příklad použití Azure Automation požadovaného stavu konfigurace (DSC), společně s Site Recovery, abyste ověřili, že:</span><span class="sxs-lookup"><span data-stu-id="45d33-123">This article provides an example of how you can use Azure Automation Desired State Configuration (DSC), together with Site Recovery, to ensure that:</span></span>

* <span data-ttu-id="45d33-124">Služba Mobility a agenta virtuálního počítače Azure se nasadí do počítače systému Windows, které chcete chránit.</span><span class="sxs-lookup"><span data-stu-id="45d33-124">The Mobility service and Azure VM agent are deployed to the Windows machines that you want to protect.</span></span>
* <span data-ttu-id="45d33-125">Služba Mobility a agenta virtuálního počítače Azure jsou vždy spuštěny Azure je cílem replikace.</span><span class="sxs-lookup"><span data-stu-id="45d33-125">The Mobility service and Azure VM agent are always running when Azure is the replication target.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45d33-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="45d33-126">Prerequisites</span></span>
* <span data-ttu-id="45d33-127">Úložiště pro ukládání požadované instalace</span><span class="sxs-lookup"><span data-stu-id="45d33-127">A repository to store the required setup</span></span>
* <span data-ttu-id="45d33-128">Úložiště pro ukládání vyžaduje heslo pro registraci se serverem pro správu</span><span class="sxs-lookup"><span data-stu-id="45d33-128">A repository to store the required passphrase to register with the management server</span></span>

  > [!NOTE]
  > <span data-ttu-id="45d33-129">Jedinečné heslo se vygeneruje pro každý server pro správu.</span><span class="sxs-lookup"><span data-stu-id="45d33-129">A unique passphrase is generated for each management server.</span></span> <span data-ttu-id="45d33-130">Pokud se chystáte nasadit více serverů pro správu, musíte zajistit, že správné heslo je uloženo v souboru passphrase.txt.</span><span class="sxs-lookup"><span data-stu-id="45d33-130">If you are going to deploy multiple management servers, you have to ensure that the correct passphrase is stored in the passphrase.txt file.</span></span>
  >
  >
* <span data-ttu-id="45d33-131">Windows Management Framework (WMF) 5.0 nainstalovaný na počítačích, které chcete povolit pro ochranu (požadavek pro Automation DSC)</span><span class="sxs-lookup"><span data-stu-id="45d33-131">Windows Management Framework (WMF) 5.0 installed on the machines that you want to enable for protection (a requirement for Automation DSC)</span></span>

  > [!NOTE]
  > <span data-ttu-id="45d33-132">Pokud chcete používat počítače DSC pro systém Windows, které mají nainstalovaný WMF 4.0, najdete v části [pomocí DSC v odpojených prostředích](## Use DSC in disconnected environments).</span><span class="sxs-lookup"><span data-stu-id="45d33-132">If you want to use DSC for Windows machines that have WMF 4.0 installed, see the section [Use DSC in disconnected environments](## Use DSC in disconnected environments).</span></span>
  

<span data-ttu-id="45d33-133">Služba Mobility lze nainstalovat pomocí příkazového řádku a několika argumenty.</span><span class="sxs-lookup"><span data-stu-id="45d33-133">The Mobility service can be installed through the command line and accepts several arguments.</span></span> <span data-ttu-id="45d33-134">Proto je potřeba mít binárních souborů (po jejich extrahování z vašeho nastavení) a ukládat je na místě, kde je můžete obnovit pomocí konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="45d33-134">That’s why you need to have the binaries (after extracting them from your setup) and store them in a place where you can retrieve them by using a DSC configuration.</span></span>

## <a name="step-1-extract-binaries"></a><span data-ttu-id="45d33-135">Krok 1: Extrahování binárních souborů</span><span class="sxs-lookup"><span data-stu-id="45d33-135">Step 1: Extract binaries</span></span>
1. <span data-ttu-id="45d33-136">Pokud chcete extrahovat soubory, které potřebujete pro tento instalační program, přejděte na následující adresář na vašem serveru pro správu:</span><span class="sxs-lookup"><span data-stu-id="45d33-136">To extract the files that you need for this setup, browse to the following directory on your management server:</span></span>

    <span data-ttu-id="45d33-137">**Recovery\home\svsystems\pushinstallsvc\repository \Microsoft azure lokality**</span><span class="sxs-lookup"><span data-stu-id="45d33-137">**\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**</span></span>

    <span data-ttu-id="45d33-138">V této složce měli byste vidět soubor MSI s názvem:</span><span class="sxs-lookup"><span data-stu-id="45d33-138">In this folder, you should see an MSI file named:</span></span>

    <span data-ttu-id="45d33-139">**Microsoft ASR_UA_version_Windows_GA_date_Release.exe**</span><span class="sxs-lookup"><span data-stu-id="45d33-139">**Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**</span></span>

    <span data-ttu-id="45d33-140">Použijte následující příkaz k extrakci instalační program:</span><span class="sxs-lookup"><span data-stu-id="45d33-140">Use the following command to extract the installer:</span></span>

    <span data-ttu-id="45d33-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span><span class="sxs-lookup"><span data-stu-id="45d33-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span></span>
2. <span data-ttu-id="45d33-142">Vyberte všechny soubory a odeslat je do složky komprimované (ZIP).</span><span class="sxs-lookup"><span data-stu-id="45d33-142">Select all files and send them to a compressed (zipped) folder.</span></span>

<span data-ttu-id="45d33-143">Nyní máte binární soubory, které potřebujete k automatizaci instalace služby Mobility pomocí Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="45d33-143">You now have the binaries that you need to automate the setup of the Mobility service by using Automation DSC.</span></span>

### <a name="passphrase"></a><span data-ttu-id="45d33-144">Přístupové heslo</span><span class="sxs-lookup"><span data-stu-id="45d33-144">Passphrase</span></span>
<span data-ttu-id="45d33-145">Dále musíte určit, kam chcete umístit tento komprimované složce.</span><span class="sxs-lookup"><span data-stu-id="45d33-145">Next, you need to determine where you want to place this zipped folder.</span></span> <span data-ttu-id="45d33-146">Účet úložiště Azure můžete použít, jak je znázorněno novější ukládat heslo, které potřebujete k instalaci.</span><span class="sxs-lookup"><span data-stu-id="45d33-146">You can use an Azure storage account, as shown later, to store the passphrase that you need for the setup.</span></span> <span data-ttu-id="45d33-147">Agent bude potom proveďte registraci se serverem pro správu jako součást procesu.</span><span class="sxs-lookup"><span data-stu-id="45d33-147">The agent will then register with the management server as part of the process.</span></span>

<span data-ttu-id="45d33-148">Do textového souboru jako passphrase.txt lze uložit heslo, které jste získali při nasazení serveru pro správu.</span><span class="sxs-lookup"><span data-stu-id="45d33-148">The passphrase that you got when you deployed the management server can be saved to a text file as passphrase.txt.</span></span>

<span data-ttu-id="45d33-149">Umístěte komprimované složce a heslo v vyhrazené kontejneru v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="45d33-149">Place both the zipped folder and the passphrase in a dedicated container in the Azure storage account.</span></span>

![Umístění složky](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

<span data-ttu-id="45d33-151">Pokud chcete zachovat tyto soubory ve sdílené složce v síti, můžete tak učinit.</span><span class="sxs-lookup"><span data-stu-id="45d33-151">If you prefer to keep these files on a share on your network, you can do so.</span></span> <span data-ttu-id="45d33-152">Potřebujete zkontrolujte, zda prostředek DSC, který budete používat později přístup a můžete získat nastavení a heslo.</span><span class="sxs-lookup"><span data-stu-id="45d33-152">You just need to ensure that the DSC resource that you will be using later has access and can get the setup and passphrase.</span></span>

## <a name="step-2-create-the-dsc-configuration"></a><span data-ttu-id="45d33-153">Krok 2: Vytvoření konfigurace DSC</span><span class="sxs-lookup"><span data-stu-id="45d33-153">Step 2: Create the DSC configuration</span></span>
<span data-ttu-id="45d33-154">Instalace závisí na WMF 5.0.</span><span class="sxs-lookup"><span data-stu-id="45d33-154">The setup depends on WMF 5.0.</span></span> <span data-ttu-id="45d33-155">Pro počítač úspěšně použít konfiguraci prostřednictvím Automation DSC musí být k dispozici WMF 5.0.</span><span class="sxs-lookup"><span data-stu-id="45d33-155">For the machine to successfully apply the configuration through Automation DSC, WMF 5.0 needs to be present.</span></span>

<span data-ttu-id="45d33-156">Prostředí používá následující ukázková konfigurace DSC:</span><span class="sxs-lookup"><span data-stu-id="45d33-156">The environment uses the following example DSC configuration:</span></span>

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
<span data-ttu-id="45d33-157">Konfigurace bude postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="45d33-157">The configuration will do the following:</span></span>

* <span data-ttu-id="45d33-158">Proměnné se v konfiguraci umožňují získat binární soubory pro tuto službu Mobility a agenta virtuálního počítače Azure, kde lze získat heslo a kam se mají ukládat výstup.</span><span class="sxs-lookup"><span data-stu-id="45d33-158">The variables will tell the configuration where to get the binaries for the Mobility service and the Azure VM agent, where to get the passphrase, and where to store the output.</span></span>
* <span data-ttu-id="45d33-159">Konfigurace naimportuje xPSDesiredStateConfiguration DSC prostředků, tak, aby můžete použít `xRemoteFile` ke stažení souborů z úložiště.</span><span class="sxs-lookup"><span data-stu-id="45d33-159">The configuration will import the xPSDesiredStateConfiguration DSC resource, so that you can use `xRemoteFile` to download the files from the repository.</span></span>
* <span data-ttu-id="45d33-160">Konfigurace vytvoří adresář, kam chcete uložit binární soubory.</span><span class="sxs-lookup"><span data-stu-id="45d33-160">The configuration will create a directory where you want to store the binaries.</span></span>
* <span data-ttu-id="45d33-161">Archiv prostředků bude extrahujte soubory z komprimované složky.</span><span class="sxs-lookup"><span data-stu-id="45d33-161">The archive resource will extract the files from the zipped folder.</span></span>
* <span data-ttu-id="45d33-162">Z UNIFIEDAGENT nainstaluje balíček prostředků instalace služby Mobility. Instalační program EXE s konkrétní argumenty.</span><span class="sxs-lookup"><span data-stu-id="45d33-162">The package Install resource will install the Mobility service from the UNIFIEDAGENT.EXE installer with the specific arguments.</span></span> <span data-ttu-id="45d33-163">(Proměnné, které vytvořit argumenty, které je nutné změnit tak, aby se zohlednilo vaše prostředí.)</span><span class="sxs-lookup"><span data-stu-id="45d33-163">(The variables that construct the arguments need to be changed to reflect your environment.)</span></span>
* <span data-ttu-id="45d33-164">Balíček prostředků AzureAgent nainstaluje agenta virtuálního počítače Azure, který se doporučuje pro každý virtuální počítač, který běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="45d33-164">The package AzureAgent resource will install the Azure VM agent, which is recommended on every VM that runs in Azure.</span></span> <span data-ttu-id="45d33-165">Agent virtuálního počítače Azure také umožňuje přidat rozšíření do virtuálního počítače po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="45d33-165">The Azure VM agent also makes it possible to add extensions to the VM after failover.</span></span>
* <span data-ttu-id="45d33-166">Prostředek služby nebo prostředky zajistí vždy spuštěn související služby Mobility a službami Azure.</span><span class="sxs-lookup"><span data-stu-id="45d33-166">The service resource or resources will ensure that the related Mobility services and the Azure services are always running.</span></span>

<span data-ttu-id="45d33-167">Uložte konfiguraci jako **ASRMobilityService**.</span><span class="sxs-lookup"><span data-stu-id="45d33-167">Save the configuration as **ASRMobilityService**.</span></span>

> [!NOTE]
> <span data-ttu-id="45d33-168">Nezapomeňte nahradit CSIP ve vaší konfiguraci tak, aby odrážela samotnou správu serveru, tak, aby agent správně připojí a bude používat správné heslo.</span><span class="sxs-lookup"><span data-stu-id="45d33-168">Remember to replace the CSIP in your configuration to reflect the actual management server, so that the agent will be connected correctly and will use the correct passphrase.</span></span>
>
>

## <a name="step-3-upload-to-automation-dsc"></a><span data-ttu-id="45d33-169">Krok 3: Nahrát do Automation DSC</span><span class="sxs-lookup"><span data-stu-id="45d33-169">Step 3: Upload to Automation DSC</span></span>
<span data-ttu-id="45d33-170">Vzhledem k tomu, že konfigurace DSC, které jste provedli importuje modul požadovaný prostředek DSC (xPSDesiredStateConfiguration), je třeba importovat Tenhle modul ve službě Automation před nahráním konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="45d33-170">Because the DSC configuration that you made will import a required DSC resource module (xPSDesiredStateConfiguration), you need to import that module in Automation before you upload the DSC configuration.</span></span>

<span data-ttu-id="45d33-171">Přihlaste se k účtu Automation, přejděte na **prostředky** > **moduly**a klikněte na tlačítko **procházet galerii**.</span><span class="sxs-lookup"><span data-stu-id="45d33-171">Sign in to your Automation account, browse to **Assets** > **Modules**, and click **Browse Gallery**.</span></span>

<span data-ttu-id="45d33-172">Zde můžete vyhledat modul a naimportujte ho do vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="45d33-172">Here you can search for the module and import it to your account.</span></span>

![Import modulu](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

<span data-ttu-id="45d33-174">Když toto dokončíte, přejděte k počítači, kde máte nainstalované moduly Azure Resource Manager a přejít k importu nově vytvořený konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="45d33-174">When you finish this, go to your machine where you have the Azure Resource Manager modules installed and proceed to import the newly created DSC configuration.</span></span>

### <a name="import-cmdlets"></a><span data-ttu-id="45d33-175">Rutiny importu</span><span class="sxs-lookup"><span data-stu-id="45d33-175">Import cmdlets</span></span>
<span data-ttu-id="45d33-176">V prostředí PowerShell Přihlaste se k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="45d33-176">In PowerShell, sign in to your Azure subscription.</span></span> <span data-ttu-id="45d33-177">Upravte rutiny zohlednilo vaše prostředí a zaznamenat informace o účtu Automation v proměnné:</span><span class="sxs-lookup"><span data-stu-id="45d33-177">Modify the cmdlets to reflect your environment and capture your Automation account information in a variable:</span></span>

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

<span data-ttu-id="45d33-178">Odešlete konfigurace do Automation DSC pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="45d33-178">Upload the configuration to Automation DSC by using the following cmdlet:</span></span>

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a><span data-ttu-id="45d33-179">Zkompilování konfigurace v Automation DSC</span><span class="sxs-lookup"><span data-stu-id="45d33-179">Compile the configuration in Automation DSC</span></span>
<span data-ttu-id="45d33-180">Dále musíte o zkompilování konfigurace v Automation DSC, takže můžete začít registrovat uzly k němu.</span><span class="sxs-lookup"><span data-stu-id="45d33-180">Next, you need to compile the configuration in Automation DSC, so that you can start to register nodes to it.</span></span> <span data-ttu-id="45d33-181">Můžete toho dosáhnout spuštěním následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="45d33-181">You achieve that by running the following cmdlet:</span></span>

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

<span data-ttu-id="45d33-182">Může to trvat několik minut, protože v podstatě nasazujete konfigurace hostované službě vyžádání DSC.</span><span class="sxs-lookup"><span data-stu-id="45d33-182">This can take a few minutes, because you're basically deploying the configuration to the hosted DSC pull service.</span></span>

<span data-ttu-id="45d33-183">Po konfiguraci zkompilujete, můžete načíst informace o úlohách pomocí prostředí PowerShell (Get-AzureRmAutomationDscCompilationJob) nebo pomocí [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="45d33-183">After you compile the configuration, you can retrieve the job information by using PowerShell (Get-AzureRmAutomationDscCompilationJob) or by using the [Azure portal](https://portal.azure.com/).</span></span>

![Načtení úlohy](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

<span data-ttu-id="45d33-185">Teď úspěšně publikována a konfiguraci DSC nahrán do Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="45d33-185">You have now successfully published and uploaded your DSC configuration to Automation DSC.</span></span>

## <a name="step-4-onboard-machines-to-automation-dsc"></a><span data-ttu-id="45d33-186">Krok 4: Připojit počítače k Automation DSC</span><span class="sxs-lookup"><span data-stu-id="45d33-186">Step 4: Onboard machines to Automation DSC</span></span>
> [!NOTE]
> <span data-ttu-id="45d33-187">Jeden z požadovaných součástí pro dokončení tohoto scénáře je, že vaše počítače Windows jsou aktualizovány na nejnovější verzi WMF.</span><span class="sxs-lookup"><span data-stu-id="45d33-187">One of the prerequisites for completing this scenario is that your Windows machines are updated with the latest version of WMF.</span></span> <span data-ttu-id="45d33-188">Můžete stáhnout a nainstalovat správnou verzi pro vaši platformu z [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).</span><span class="sxs-lookup"><span data-stu-id="45d33-188">You can download and install the correct version for your platform from the [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).</span></span>
>
>

<span data-ttu-id="45d33-189">Nyní vytvoříte metaconfig pro DSC, která budou platit pro uzly.</span><span class="sxs-lookup"><span data-stu-id="45d33-189">You will now create a metaconfig for DSC that you will apply to your nodes.</span></span> <span data-ttu-id="45d33-190">Uspět s tím, budete muset získat adresu URL koncového bodu a primární klíč pro vybrané účtu Automation v Azure.</span><span class="sxs-lookup"><span data-stu-id="45d33-190">To succeed with this, you need to retrieve the endpoint URL and the primary key for your selected Automation account in Azure.</span></span> <span data-ttu-id="45d33-191">Můžete najít tyto hodnoty v části **klíče** na **všechna nastavení** okno pro účet služby Automation.</span><span class="sxs-lookup"><span data-stu-id="45d33-191">You can find these values under **Keys** on the **All settings** blade for the Automation account.</span></span>

![Hodnoty klíče](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

<span data-ttu-id="45d33-193">V tomto příkladu máte fyzického serveru Windows Server 2012 R2, který chcete chránit pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="45d33-193">In this example, you have a Windows Server 2012 R2 physical server that you want to protect by using Site Recovery.</span></span>

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a><span data-ttu-id="45d33-194">Zkontrolujte všechny čekající operace přejmenování souboru v registru</span><span class="sxs-lookup"><span data-stu-id="45d33-194">Check for any pending file rename operations in the registry</span></span>
<span data-ttu-id="45d33-195">Než začnete přidružení serveru s koncovým bodem Automation DSC, doporučujeme zkontrolovat pro všechny čekající operace přejmenování souboru v registru.</span><span class="sxs-lookup"><span data-stu-id="45d33-195">Before you start to associate the server with the Automation DSC endpoint, we recommend that you check for any pending file rename operations in the registry.</span></span> <span data-ttu-id="45d33-196">Instalace se může mají zakázáno dokončení kvůli čeká na restartování.</span><span class="sxs-lookup"><span data-stu-id="45d33-196">They might prohibit the setup from finishing due to a pending reboot.</span></span>

<span data-ttu-id="45d33-197">Spusťte následující rutiny ověřte, zda je žádné čeká na restartování na serveru:</span><span class="sxs-lookup"><span data-stu-id="45d33-197">Run the following cmdlet to verify that there’s no pending reboot on the server:</span></span>

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
<span data-ttu-id="45d33-198">Pokud se zobrazí prázdné, jste na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="45d33-198">If this shows empty, you are OK to proceed.</span></span> <span data-ttu-id="45d33-199">V opačném případě by to vyřešit tak, že restartování serveru během časového období údržby.</span><span class="sxs-lookup"><span data-stu-id="45d33-199">If not, you should address this by rebooting the server during a maintenance window.</span></span>

<span data-ttu-id="45d33-200">Použít konfiguraci na serveru, spusťte prostředí PowerShell integrovaném skriptovacím prostředí (ISE) a spusťte následující skript.</span><span class="sxs-lookup"><span data-stu-id="45d33-200">To apply the configuration on the server, start the PowerShell Integrated Scripting Environment (ISE) and run the following script.</span></span> <span data-ttu-id="45d33-201">Toto je v podstatě DSC místní konfiguraci, která bude určit, aby modul správce místní konfigurace zaregistrovat službu Automation DSC a načíst konkrétní konfiguraci (ASRMobilityService.localhost).</span><span class="sxs-lookup"><span data-stu-id="45d33-201">This is essentially a DSC local configuration that will instruct the Local Configuration Manager engine to register with the Automation DSC service and retrieve the specific configuration (ASRMobilityService.localhost).</span></span>

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

<span data-ttu-id="45d33-202">Tato konfigurace způsobí, že modul správce místní konfigurace k registraci v Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="45d33-202">This configuration will cause the Local Configuration Manager engine to register itself with Automation DSC.</span></span> <span data-ttu-id="45d33-203">Také se určí, jak pracovat modul, jak ho postupovat, pokud je konfigurace odlišily (ApplyAndAutoCorrect) a jak by měla postupovat s konfigurací Pokud je vyžadován restart.</span><span class="sxs-lookup"><span data-stu-id="45d33-203">It will also determine how the engine should operate, what it should do if there's a configuration drift (ApplyAndAutoCorrect), and how it should proceed with the configuration if a reboot is required.</span></span>

<span data-ttu-id="45d33-204">Po spuštění tohoto skriptu uzlu začněte registrace u služby Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="45d33-204">After you run this script, the node should start to register with Automation DSC.</span></span>

![Registrace uzlu v průběhu](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

<span data-ttu-id="45d33-206">Pokud přejdete zpět na portál Azure, uvidíte, že nově zaregistrovaný uzel má nyní zobrazovaly v portálu.</span><span class="sxs-lookup"><span data-stu-id="45d33-206">If you go back to the Azure portal, you can see that the newly registered node has now appeared in the portal.</span></span>

![Registrovaný uzlu na portálu](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

<span data-ttu-id="45d33-208">Na serveru můžete spustit následující rutiny prostředí PowerShell k ověření, že správně zaregistrovány uzlu:</span><span class="sxs-lookup"><span data-stu-id="45d33-208">On the server, you can run the following PowerShell cmdlet to verify that the node has been registered correctly:</span></span>

```powershell
Get-DscLocalConfigurationManager
```

<span data-ttu-id="45d33-209">Po konfiguraci byl vyžádat a použít na server, můžete to ověřit spuštěním následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="45d33-209">After the configuration has been pulled and applied to the server, you can verify this by running the following cmdlet:</span></span>

```powershell
Get-DscConfigurationStatus
```

<span data-ttu-id="45d33-210">Výstup ukazuje, že server úspěšně vyžádat jeho konfigurace:</span><span class="sxs-lookup"><span data-stu-id="45d33-210">The output shows that the server has successfully pulled its configuration:</span></span>

![Výstup](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

<span data-ttu-id="45d33-212">Kromě toho instalace služby Mobility má svou vlastní protokolu, který se nachází v *SystemDrive*\ProgramData\ASRSetupLogs.</span><span class="sxs-lookup"><span data-stu-id="45d33-212">In addition, the Mobility service setup has its own log that can be found at *SystemDrive*\ProgramData\ASRSetupLogs.</span></span>

<span data-ttu-id="45d33-213">Je to.</span><span class="sxs-lookup"><span data-stu-id="45d33-213">That’s it.</span></span> <span data-ttu-id="45d33-214">Teď úspěšně nasazen a registrované služby Mobility na počítači, který chcete chránit pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="45d33-214">You have now successfully deployed and registered the Mobility service on the machine that you want to protect by using Site Recovery.</span></span> <span data-ttu-id="45d33-215">DSC se ujistěte, že jsou vždy neběží požadované služby.</span><span class="sxs-lookup"><span data-stu-id="45d33-215">DSC will make sure that the required services are always running.</span></span>

![Úspěšné nasazení](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

<span data-ttu-id="45d33-217">Po úspěšné nasazení zjistí, server pro správu, musíte nakonfigurovat ochranu a zapnout replikaci na počítači pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="45d33-217">After the management server detects the successful deployment, you can configure protection and enable replication on the machine by using Site Recovery.</span></span>

## <a name="use-dsc-in-disconnected-environments"></a><span data-ttu-id="45d33-218">Použití DSC v odpojených prostředích</span><span class="sxs-lookup"><span data-stu-id="45d33-218">Use DSC in disconnected environments</span></span>
<span data-ttu-id="45d33-219">Pokud vaše počítače nejsou připojené k Internetu, můžete stále spolehnout na DSC k nasazení a konfiguraci služby Mobility na jiné úlohy, které chcete chránit.</span><span class="sxs-lookup"><span data-stu-id="45d33-219">If your machines aren’t connected to the Internet, you can still rely on DSC to deploy and configure the Mobility service on the workloads that you want to protect.</span></span>

<span data-ttu-id="45d33-220">Můžete vytvořit vlastní server DSC za instanci ve vašem prostředí a v podstatě poskytovat stejné funkce, které jste získali z Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="45d33-220">You can instantiate your own DSC pull server in your environment to essentially provide the same capabilities that you get from Automation DSC.</span></span> <span data-ttu-id="45d33-221">To znamená klienti načte konfiguraci (po dokončení registrace) ke koncovému bodu DSC.</span><span class="sxs-lookup"><span data-stu-id="45d33-221">That is, the clients will pull the configuration (after it's registered) to the DSC endpoint.</span></span> <span data-ttu-id="45d33-222">Další možností je ručně push konfigurace DSC na počítače, místně nebo vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="45d33-222">However, another option is to manually push the DSC configuration to your machines, either locally or remotely.</span></span>

<span data-ttu-id="45d33-223">Všimněte si, že v tomto příkladu je přidaný parametr pro název počítače.</span><span class="sxs-lookup"><span data-stu-id="45d33-223">Note that in this example, there's an added parameter for the computer name.</span></span> <span data-ttu-id="45d33-224">Vzdálené soubory, které jsou umístěné ve vzdálené sdílené složce, která musí být přístupný pomocí počítače, které chcete chránit.</span><span class="sxs-lookup"><span data-stu-id="45d33-224">The remote files are now located on a remote share that should be accessible by the machines that you want to protect.</span></span> <span data-ttu-id="45d33-225">Konec skript představuje konfigurace a pak spustí použít konfigurace DSC k cílovému počítači.</span><span class="sxs-lookup"><span data-stu-id="45d33-225">The end of the script enacts the configuration and then starts to apply the DSC configuration to the target computer.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="45d33-226">Požadavky</span><span class="sxs-lookup"><span data-stu-id="45d33-226">Prerequisites</span></span>
<span data-ttu-id="45d33-227">Ujistěte se, že je nainstalován modul PowerShell xPSDesiredStateConfiguration.</span><span class="sxs-lookup"><span data-stu-id="45d33-227">Make sure that the xPSDesiredStateConfiguration PowerShell module is installed.</span></span> <span data-ttu-id="45d33-228">Pro počítače s Windows, kde je nainstalován WMF 5.0 můžete nainstalovat modul xPSDesiredStateConfiguration spuštěním následující rutiny na cílových počítačích:</span><span class="sxs-lookup"><span data-stu-id="45d33-228">For Windows machines where WMF 5.0 is installed, you can install the xPSDesiredStateConfiguration module by running the following cmdlet on the target machines:</span></span>

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

<span data-ttu-id="45d33-229">Můžete také stáhnout a uložit v modulu v případě, že budete muset distribuovat do počítače s Windows, které mají WMF 4.0.</span><span class="sxs-lookup"><span data-stu-id="45d33-229">You can also download and save the module in case you need to distribute it to Windows machines that have WMF 4.0.</span></span> <span data-ttu-id="45d33-230">Spusťte tuto rutinu na počítači, kde se nachází PowerShellGet (WMF 5.0):</span><span class="sxs-lookup"><span data-stu-id="45d33-230">Run this cmdlet on a machine where PowerShellGet (WMF 5.0) is present:</span></span>

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

<span data-ttu-id="45d33-231">Také pro WMF 4.0, zkontrolujte, zda [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) je nainstalován na počítačích.</span><span class="sxs-lookup"><span data-stu-id="45d33-231">Also for WMF 4.0, ensure that the [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) is installed on the machines.</span></span>

<span data-ttu-id="45d33-232">Následující konfigurace můžete vloží do počítače s Windows, které mají WMF 5.0 a WMF 4.0:</span><span class="sxs-lookup"><span data-stu-id="45d33-232">The following configuration can be pushed to Windows machines that have WMF 5.0 and WMF 4.0:</span></span>

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

<span data-ttu-id="45d33-233">Pokud chcete vytvořit vlastní server DSC za ve vaší podnikové síti tak, aby napodoboval možností, které můžete získat z Automation DSC, přečtěte si téma [nastavení webového serveru vyžádané replikace DSC](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="45d33-233">If you want to instantiate your own DSC pull server on your corporate network to mimic the capabilities that you can get from Automation DSC, see [Setting up a DSC web pull server](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span></span>

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a><span data-ttu-id="45d33-234">Volitelné: Konfigurace DSC nasazení pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="45d33-234">Optional: Deploy a DSC configuration by using an Azure Resource Manager template</span></span>
<span data-ttu-id="45d33-235">Tento článek se zaměřuje na tom, jak můžete vytvořit vlastní konfigurace DSC pro automatické nasazení služby Mobility a Agent virtuálního počítače Azure – a ujistěte se, která běží na počítačích, které chcete chránit.</span><span class="sxs-lookup"><span data-stu-id="45d33-235">This article has focused on how you can create your own DSC configuration to automatically deploy the Mobility service and the Azure VM Agent--and ensure that they are running on the machines that you want to protect.</span></span> <span data-ttu-id="45d33-236">Máme také šablonu Azure Resource Manager, nasadíte tuto konfiguraci DSC na nový nebo existující účet automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="45d33-236">We also have an Azure Resource Manager template that will deploy this DSC configuration to a new or existing Azure Automation account.</span></span> <span data-ttu-id="45d33-237">Šablonu bude používat vstupních parametrů k vytvoření automatizace prostředky, které budou obsahovat proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="45d33-237">The template will use input parameters to create Automation assets that will contain the variables for your environment.</span></span>

<span data-ttu-id="45d33-238">Po nasazení šablony, jednoduše najdete ke kroku 4 v tomto průvodci zařadit do vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="45d33-238">After you deploy the template, you can simply refer to step 4 in this guide to onboard your machines.</span></span>

<span data-ttu-id="45d33-239">Šablonu bude postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="45d33-239">The template will do the following:</span></span>

1. <span data-ttu-id="45d33-240">Použít existující účet Automation nebo vytvořit novou</span><span class="sxs-lookup"><span data-stu-id="45d33-240">Use an existing Automation account or create a new one</span></span>
2. <span data-ttu-id="45d33-241">Trvat vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="45d33-241">Take input parameters for:</span></span>
   * <span data-ttu-id="45d33-242">ASRRemoteFile – umístění pro uložení instalace služby Mobility</span><span class="sxs-lookup"><span data-stu-id="45d33-242">ASRRemoteFile--the location where you have stored the Mobility service setup</span></span>
   * <span data-ttu-id="45d33-243">ASRPassphrase – umístění, kam jste uložili soubor passphrase.txt</span><span class="sxs-lookup"><span data-stu-id="45d33-243">ASRPassphrase--the location where you have stored the passphrase.txt file</span></span>
   * <span data-ttu-id="45d33-244">ASRCSEndpoint – IP adresa serveru pro správu</span><span class="sxs-lookup"><span data-stu-id="45d33-244">ASRCSEndpoint--the IP address of your management server</span></span>
3. <span data-ttu-id="45d33-245">Naimportujte modul Powershellu xPSDesiredStateConfiguration</span><span class="sxs-lookup"><span data-stu-id="45d33-245">Import the xPSDesiredStateConfiguration PowerShell module</span></span>
4. <span data-ttu-id="45d33-246">Vytvoření a kompilaci konfigurace DSC</span><span class="sxs-lookup"><span data-stu-id="45d33-246">Create and compile the DSC configuration</span></span>

<span data-ttu-id="45d33-247">Aby mohl začít registrace vašich počítačů pro ochranu, stane se všechny předchozí kroky ve správném pořadí.</span><span class="sxs-lookup"><span data-stu-id="45d33-247">All the preceding steps will happen in the right order, so that you can start onboarding your machines for protection.</span></span>

<span data-ttu-id="45d33-248">Šablona, s pokyny pro nasazení, se nachází na [Githubu](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span><span class="sxs-lookup"><span data-stu-id="45d33-248">The template, with instructions for deployment, is located on [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span></span>

<span data-ttu-id="45d33-249">Nasazení šablony pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="45d33-249">Deploy the template by using PowerShell:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="45d33-250">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45d33-250">Next steps</span></span>
<span data-ttu-id="45d33-251">Poté, co nasadíte agenty služby Mobility, můžete [povolit replikaci](site-recovery-vmware-to-azure.md) pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="45d33-251">After you deploy the Mobility service agents, you can [enable replication](site-recovery-vmware-to-azure.md) for the virtual machines.</span></span>
