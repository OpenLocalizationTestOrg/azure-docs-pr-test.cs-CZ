---
title: "Verze rozšíření schéma konfigurace Azure Diagnostics a historie | Microsoft Docs"
description: "Relevantní pro shromažďování čítače výkonu ve virtuálních počítačích Azure, škálovatelné sady virtuálních počítačů, Service Fabric a cloudové služby."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/16/2017
ms.author: robb
ms.openlocfilehash: 119e8a237f24cdc80a1ab8e376f2b308c9eada05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-diagnostics-extention-configuration-schema-versions-and-history"></a><span data-ttu-id="fca4a-103">Verze rozšíření schéma konfigurace Azure Diagnostics a historie</span><span class="sxs-lookup"><span data-stu-id="fca4a-103">Azure Diagnostics extention configuration schema versions and history</span></span>
<span data-ttu-id="fca4a-104">Tato stránka indexy verze schématu rozšíření Azure Diagnostics dodávána jako součást nástroje Microsoft Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="fca4a-104">This page indexes Azure Diagnostics extension schema versions shipped as part of the Microsoft Azure SDK.</span></span>  

> [!NOTE]
> <span data-ttu-id="fca4a-105">Rozšíření diagnostiky Azure je komponenta, kterou používá ke shromažďování čítačů výkonu a další statistiky z:</span><span class="sxs-lookup"><span data-stu-id="fca4a-105">The Azure Diagnostics extension is the component used to collect performance counters and other statistics from:</span></span>
> - <span data-ttu-id="fca4a-106">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="fca4a-106">Azure Virtual Machines</span></span> 
> - <span data-ttu-id="fca4a-107">Škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fca4a-107">Virtual Machine Scale Sets</span></span>
> - <span data-ttu-id="fca4a-108">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="fca4a-108">Service Fabric</span></span> 
> - <span data-ttu-id="fca4a-109">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="fca4a-109">Cloud Services</span></span> 
> - <span data-ttu-id="fca4a-110">Network Security Groups (Skupiny zabezpečení sítě)</span><span class="sxs-lookup"><span data-stu-id="fca4a-110">Network Security Groups</span></span>
> 
> <span data-ttu-id="fca4a-111">Tato stránka je pouze relevantní, pokud používáte některou z těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="fca4a-111">This page is only relevant if you are using one of these services.</span></span>

<span data-ttu-id="fca4a-112">Rozšíření diagnostiky Azure se používá s dalšími produkty Microsoftu diagnostiky jako monitorování Azure, Application Insights a analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="fca4a-112">The Azure Diagnostics extension is used with other Microsoft diagnostics products like Azure Monitor, Application Insights, and Log Analytics.</span></span> <span data-ttu-id="fca4a-113">Další informace najdete v části [přehled nástrojů pro monitorování Microsoft](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fca4a-113">For more information see [Microsoft Monitoring Tools Overview](monitoring-overview.md).</span></span>

## <a name="azure-sdk-and-diagnostics-versions-shipping-chart"></a><span data-ttu-id="fca4a-114">Azure SDK a Diagnostika verze přesouvání grafu</span><span class="sxs-lookup"><span data-stu-id="fca4a-114">Azure SDK and diagnostics versions shipping chart</span></span>  

|<span data-ttu-id="fca4a-115">Azure SDK verze</span><span class="sxs-lookup"><span data-stu-id="fca4a-115">Azure SDK version</span></span> | <span data-ttu-id="fca4a-116">Verze rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="fca4a-116">Diagnostics extension version</span></span> | <span data-ttu-id="fca4a-117">Model</span><span class="sxs-lookup"><span data-stu-id="fca4a-117">Model</span></span>|  
|------------------|-------------------------------|------|  
|<span data-ttu-id="fca4a-118">1.x</span><span class="sxs-lookup"><span data-stu-id="fca4a-118">1.x</span></span>               |<span data-ttu-id="fca4a-119">1.0</span><span class="sxs-lookup"><span data-stu-id="fca4a-119">1.0</span></span>                            |<span data-ttu-id="fca4a-120">modul plug-in</span><span class="sxs-lookup"><span data-stu-id="fca4a-120">plug-in</span></span>|  
|<span data-ttu-id="fca4a-121">2.0 - 2.4</span><span class="sxs-lookup"><span data-stu-id="fca4a-121">2.0 - 2.4</span></span>         |<span data-ttu-id="fca4a-122">1.0</span><span class="sxs-lookup"><span data-stu-id="fca4a-122">1.0</span></span>                            |<span data-ttu-id="fca4a-123">modul plug-in</span><span class="sxs-lookup"><span data-stu-id="fca4a-123">plug-in</span></span>|  
|<span data-ttu-id="fca4a-124">2.5</span><span class="sxs-lookup"><span data-stu-id="fca4a-124">2.5</span></span>               |<span data-ttu-id="fca4a-125">1.2</span><span class="sxs-lookup"><span data-stu-id="fca4a-125">1.2</span></span>                            |<span data-ttu-id="fca4a-126">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="fca4a-126">extension</span></span>|  
|<span data-ttu-id="fca4a-127">2.6</span><span class="sxs-lookup"><span data-stu-id="fca4a-127">2.6</span></span>               |<span data-ttu-id="fca4a-128">1.3</span><span class="sxs-lookup"><span data-stu-id="fca4a-128">1.3</span></span>                            |<span data-ttu-id="fca4a-129">"</span><span class="sxs-lookup"><span data-stu-id="fca4a-129">"</span></span>|  
|<span data-ttu-id="fca4a-130">2.7</span><span class="sxs-lookup"><span data-stu-id="fca4a-130">2.7</span></span>               |<span data-ttu-id="fca4a-131">1.4</span><span class="sxs-lookup"><span data-stu-id="fca4a-131">1.4</span></span>                            |<span data-ttu-id="fca4a-132">"</span><span class="sxs-lookup"><span data-stu-id="fca4a-132">"</span></span>|  
|<span data-ttu-id="fca4a-133">2.8</span><span class="sxs-lookup"><span data-stu-id="fca4a-133">2.8</span></span>               |<span data-ttu-id="fca4a-134">1.5</span><span class="sxs-lookup"><span data-stu-id="fca4a-134">1.5</span></span>                            |<span data-ttu-id="fca4a-135">"</span><span class="sxs-lookup"><span data-stu-id="fca4a-135">"</span></span>|  
|<span data-ttu-id="fca4a-136">2.9</span><span class="sxs-lookup"><span data-stu-id="fca4a-136">2.9</span></span>               |<span data-ttu-id="fca4a-137">1.6</span><span class="sxs-lookup"><span data-stu-id="fca4a-137">1.6</span></span>                            |<span data-ttu-id="fca4a-138">"</span><span class="sxs-lookup"><span data-stu-id="fca4a-138">"</span></span>|
|<span data-ttu-id="fca4a-139">2.96</span><span class="sxs-lookup"><span data-stu-id="fca4a-139">2.96</span></span>              |<span data-ttu-id="fca4a-140">1.7</span><span class="sxs-lookup"><span data-stu-id="fca4a-140">1.7</span></span>                            |<span data-ttu-id="fca4a-141">"</span><span class="sxs-lookup"><span data-stu-id="fca4a-141">"</span></span>|
|<span data-ttu-id="fca4a-142">2.96</span><span class="sxs-lookup"><span data-stu-id="fca4a-142">2.96</span></span>              |<span data-ttu-id="fca4a-143">1.8</span><span class="sxs-lookup"><span data-stu-id="fca4a-143">1.8</span></span>                            |<span data-ttu-id="fca4a-144">"</span><span class="sxs-lookup"><span data-stu-id="fca4a-144">"</span></span>|
|<span data-ttu-id="fca4a-145">2.96</span><span class="sxs-lookup"><span data-stu-id="fca4a-145">2.96</span></span>              |<span data-ttu-id="fca4a-146">1.8.1</span><span class="sxs-lookup"><span data-stu-id="fca4a-146">1.8.1</span></span>                          |<span data-ttu-id="fca4a-147">"</span><span class="sxs-lookup"><span data-stu-id="fca4a-147">"</span></span>|
|<span data-ttu-id="fca4a-148">2.96</span><span class="sxs-lookup"><span data-stu-id="fca4a-148">2.96</span></span>              |<span data-ttu-id="fca4a-149">1.9</span><span class="sxs-lookup"><span data-stu-id="fca4a-149">1.9</span></span>                            |<span data-ttu-id="fca4a-150">"</span><span class="sxs-lookup"><span data-stu-id="fca4a-150">"</span></span>|



 <span data-ttu-id="fca4a-151">Azure Diagnostics verze 1.0 nejprve poskytuje modul plug-in model – což znamená, že při instalaci sady Azure SDK vy máte verzi Azure diagnostics dodávané s ním.</span><span class="sxs-lookup"><span data-stu-id="fca4a-151">Azure Diagnostics version 1.0 first shipped in a plug-in model -- meaning that when you installed the Azure SDK, you got the version of Azure diagnostics shipped with it.</span></span>  

 <span data-ttu-id="fca4a-152">Od verze 2.5 SDK (diagnostics verze 1.2), Azure diagnostics byl přesunut do model rozšíření.</span><span class="sxs-lookup"><span data-stu-id="fca4a-152">Starting with SDK 2.5 (diagnostics version 1.2), Azure diagnostics went to an extension model.</span></span> <span data-ttu-id="fca4a-153">Abyste mohli využívat nové funkce nástroje byly pouze k dispozici v novější SDK služby Azure, ale všechny služby pomocí nástroje Azure diagnostics by vyzvedne, až na nejnovější verzi přesouvání přímo z Azure.</span><span class="sxs-lookup"><span data-stu-id="fca4a-153">The tools to utilize new features were only available in newer Azure SDKs, but any service using Azure diagnostics would pick up the latest shipping version directly from Azure.</span></span> <span data-ttu-id="fca4a-154">Například každý, kdo stále pomocí sady SDK, 2.5 by být načítání na nejnovější verzi vidět v předchozí tabulce, bez ohledu na to, když používají funkce novější.</span><span class="sxs-lookup"><span data-stu-id="fca4a-154">For example, anyone still using SDK 2.5 would be loading the latest version shown in the previous table, regardless if they are using the newer features.</span></span>  

## <a name="schemas-index"></a><span data-ttu-id="fca4a-155">Index schémat.</span><span class="sxs-lookup"><span data-stu-id="fca4a-155">Schemas index</span></span>  
<span data-ttu-id="fca4a-156">Různé verze diagnostiky Azure používají schémata jinou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="fca4a-156">Different versions of Azure diagnostics use different configuration schemas.</span></span> 

[<span data-ttu-id="fca4a-157">Schéma konfigurace diagnostiky 1.0</span><span class="sxs-lookup"><span data-stu-id="fca4a-157">Diagnostics 1.0 Configuration Schema</span></span>](azure-diagnostics-schema-1dot0.md)  

[<span data-ttu-id="fca4a-158">Schéma konfigurace diagnostiky 1.2</span><span class="sxs-lookup"><span data-stu-id="fca4a-158">Diagnostics 1.2 Configuration Schema</span></span>](azure-diagnostics-schema-1dot2.md)  

[<span data-ttu-id="fca4a-159">Diagnostika 1.3 a novější schéma konfigurace</span><span class="sxs-lookup"><span data-stu-id="fca4a-159">Diagnostics 1.3 and later Configuration Schema</span></span>](azure-diagnostics-schema-1dot3-and-later.md)  

## <a name="version-history"></a><span data-ttu-id="fca4a-160">Historie verzí</span><span class="sxs-lookup"><span data-stu-id="fca4a-160">Version history</span></span>


### <a name="diagnostics-extension-19"></a><span data-ttu-id="fca4a-161">Rozšíření diagnostiky 1.9</span><span class="sxs-lookup"><span data-stu-id="fca4a-161">Diagnostics extension 1.9</span></span> 
<span data-ttu-id="fca4a-162">Byla přidána podpora Docker.</span><span class="sxs-lookup"><span data-stu-id="fca4a-162">Added Docker support.</span></span>


### <a name="diagnostics-extension-181"></a><span data-ttu-id="fca4a-163">Rozšíření diagnostiky 1.8.1</span><span class="sxs-lookup"><span data-stu-id="fca4a-163">Diagnostics extension 1.8.1</span></span> 
<span data-ttu-id="fca4a-164">Můžete zadat token SAS místo klíč účtu úložiště v privátní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fca4a-164">Can specify a SAS token instead of a storage account key in the private config.</span></span> <span data-ttu-id="fca4a-165">Pokud je zadaný SAS token, klíče účtu úložiště se ignoruje.</span><span class="sxs-lookup"><span data-stu-id="fca4a-165">If a SAS token is provided, the storage account key is ignored.</span></span>


```json
{
    "storageAccountName": "diagstorageaccount",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    }
}
```

```xml
<PrivateConfig>
    <StorageAccount name="diagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    <SecondaryStorageAccounts>
        <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
</PrivateConfig>
```


### <a name="diagnostics-extension-18"></a><span data-ttu-id="fca4a-166">Rozšíření diagnostiky 1.8</span><span class="sxs-lookup"><span data-stu-id="fca4a-166">Diagnostics extension 1.8</span></span> 
<span data-ttu-id="fca4a-167">Typ úložiště přidané do PublicConfig.</span><span class="sxs-lookup"><span data-stu-id="fca4a-167">Added Storage Type to PublicConfig.</span></span> <span data-ttu-id="fca4a-168">Může být StorageType *tabulky*, *Blob*, *TableAndBlob*.</span><span class="sxs-lookup"><span data-stu-id="fca4a-168">StorageType can be *Table*, *Blob*, *TableAndBlob*.</span></span> <span data-ttu-id="fca4a-169">*Tabulka* je výchozí.</span><span class="sxs-lookup"><span data-stu-id="fca4a-169">*Table* is the default.</span></span>


```json
{
    "WadCfg": {
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```xml
<PublicConfig>
    <WadCfg />
    <StorageAccount>diagstorageaccount</StorageAccount>
    <StorageType>TableAndBlob</StorageType>
</PublicConfig>
```


### <a name="diagnostics-extension-17"></a><span data-ttu-id="fca4a-170">Rozšíření diagnostiky 1.7</span><span class="sxs-lookup"><span data-stu-id="fca4a-170">Diagnostics extension 1.7</span></span> 
<span data-ttu-id="fca4a-171">Přidání možnosti trasy k centru EventHub.</span><span class="sxs-lookup"><span data-stu-id="fca4a-171">Added the ability to route to EventHub.</span></span>

### <a name="diagnostics-extension-15"></a><span data-ttu-id="fca4a-172">Rozšíření diagnostiky 1.5</span><span class="sxs-lookup"><span data-stu-id="fca4a-172">Diagnostics extension 1.5</span></span>
<span data-ttu-id="fca4a-173">Přidat element jímky a možnost odesílat data diagnostiky [Application Insights](../application-insights/app-insights-cloudservices.md) snadněji diagnostikovat problémy v aplikaci, jakož i úroveň systému a infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="fca4a-173">Added the sinks element and the ability to send diagnostics data to [Application Insights](../application-insights/app-insights-cloudservices.md) making it easier to diagnose issues across your application as well as the system and infrastructure level.</span></span>

### <a name="azure-sdk-26-and-diagnostics-extension-13"></a><span data-ttu-id="fca4a-174">Azure SDK 2.6 a Diagnostika extension 1.3</span><span class="sxs-lookup"><span data-stu-id="fca4a-174">Azure SDK 2.6 and diagnostics extension 1.3</span></span> 
<span data-ttu-id="fca4a-175">Pro cloudové služby projekty v sadě Visual Studio byly provedeny následující změny.</span><span class="sxs-lookup"><span data-stu-id="fca4a-175">For Cloud Service projects in Visual Studio, the following changes were made.</span></span> <span data-ttu-id="fca4a-176">(Tyto změny také použita na novější verzi sady Azure SDK.)</span><span class="sxs-lookup"><span data-stu-id="fca4a-176">(These changes also apply to later versions of Azure SDK.)</span></span>

* <span data-ttu-id="fca4a-177">Místní emulátoru nyní podporuje diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="fca4a-177">The local emulator now supports diagnostics.</span></span> <span data-ttu-id="fca4a-178">To znamená, že můžete shromažďovat diagnostická data a ujistěte se, že aplikace je vytváření správné trasování, když jste vývoj a testování v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fca4a-178">This means you can collect diagnostics data and ensure your application is creating the right traces while you're developing and testing in Visual Studio.</span></span> <span data-ttu-id="fca4a-179">Připojovací řetězec `UseDevelopmentStorage=true` umožňuje shromažďování dat diagnostiky, když spouštíte projekt cloudové služby v sadě Visual Studio pomocí emulátoru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="fca4a-179">The connection string `UseDevelopmentStorage=true` enables diagnostics data collection while you're running your cloud service project in Visual Studio by using the Azure storage emulator.</span></span> <span data-ttu-id="fca4a-180">Všechny diagnostická data se shromažďují v účtu úložiště (vývoj pro úložiště).</span><span class="sxs-lookup"><span data-stu-id="fca4a-180">All diagnostics data is collected in the (Development Storage) storage account.</span></span>
* <span data-ttu-id="fca4a-181">Řetězec připojení účtu úložiště diagnostiky (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) je znovu uložené v souboru (.csdef) služby.</span><span class="sxs-lookup"><span data-stu-id="fca4a-181">The diagnostics storage account connection string (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) is stored once again in the service configuration (.cscfg) file.</span></span> <span data-ttu-id="fca4a-182">V souboru diagnostics.wadcfgx bylo zadáno v Azure SDK 2.5 účet úložiště diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="fca4a-182">In Azure SDK 2.5 the diagnostics storage account was specified in the diagnostics.wadcfgx file.</span></span>

<span data-ttu-id="fca4a-183">Existují některé významné rozdíly mezi jak šlo připojovací řetězec v Azure SDK 2.4 a starší a jak to funguje v Azure SDK 2.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fca4a-183">There are some notable differences between how the connection string worked in Azure SDK 2.4 and earlier and how it works in Azure SDK 2.6 and later.</span></span>

* <span data-ttu-id="fca4a-184">Azure SDK 2.4 a starší připojovací řetězec jako byl použit modulu runtime modulem plug-in diagnostiky získat informace o účtu úložiště pro přenos diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="fca4a-184">In Azure SDK 2.4 and earlier, the connection string was used as a runtime by the diagnostics plugin to get the storage account information for transferring diagnostics logs.</span></span>
* <span data-ttu-id="fca4a-185">V Azure SDK 2.6 nebo novější diagnostiky připojovací řetězec se používá Visual Studio ke konfiguraci rozšíření diagnostiky s informace o účtu příslušné úložiště během publikování.</span><span class="sxs-lookup"><span data-stu-id="fca4a-185">In Azure SDK 2.6 and later, the diagnostics connection string is used by Visual Studio to configure the diagnostics extension with the appropriate storage account information during publishing.</span></span> <span data-ttu-id="fca4a-186">Připojovací řetězec umožňuje definovat jiným účtům úložiště pro různé služby konfigurace, které Visual Studio budou používat při publikování.</span><span class="sxs-lookup"><span data-stu-id="fca4a-186">The connection string lets you define different storage accounts for different service configurations that Visual Studio will use when publishing.</span></span> <span data-ttu-id="fca4a-187">Ale vzhledem k tomu, že modul plug-in diagnostiky již není k dispozici (po Azure SDK 2.5), soubor .cscfg samostatně nelze povolit rozšíření diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="fca4a-187">However, because the diagnostics plugin is no longer available (after Azure SDK 2.5), the .cscfg file by itself can't enable the Diagnostics Extension.</span></span> <span data-ttu-id="fca4a-188">Je nutné povolit rozšíření, samostatně prostřednictvím nástrojů, jako je Visual Studio nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fca4a-188">You have to enable the extension separately through tools such as Visual Studio or PowerShell.</span></span>
* <span data-ttu-id="fca4a-189">Ke zjednodušení procesu konfigurace rozšíření diagnostiky pomocí prostředí PowerShell, balíček výstup ze sady Visual Studio také obsahuje veřejné konfigurace XML pro rozšíření diagnostiky pro každou roli.</span><span class="sxs-lookup"><span data-stu-id="fca4a-189">To simplify the process of configuring the diagnostics extension with PowerShell, the package output from Visual Studio also contains the public configuration XML for the diagnostics extension for each role.</span></span> <span data-ttu-id="fca4a-190">Visual Studio použije diagnostiky připojovací řetězec k naplnění ve veřejné konfigurace informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="fca4a-190">Visual Studio uses the diagnostics connection string to populate the storage account information present in the public configuration.</span></span> <span data-ttu-id="fca4a-191">Veřejné konfigurační soubory jsou vytvořeny ve složce rozšíření a postupujte podle vzoru PaaSDiagnostics. <RoleName>. PubConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="fca4a-191">The public config files are created in the Extensions folder and follow the pattern PaaSDiagnostics.<RoleName>.PubConfig.xml.</span></span> <span data-ttu-id="fca4a-192">Všechna nasazení na základě prostředí PowerShell můžete použít tento vzor pro mapování každé konfiguraci k roli.</span><span class="sxs-lookup"><span data-stu-id="fca4a-192">Any PowerShell based deployments can use this pattern to map each configuration to a Role.</span></span>
* <span data-ttu-id="fca4a-193">Připojovací řetězec v souboru .cscfg je také používá portál Azure pro přístup k datům diagnostiky, může se objevit v **monitorování** kartě.</span><span class="sxs-lookup"><span data-stu-id="fca4a-193">The connection string in the .cscfg file is also used by the Azure portal to access the diagnostics data so it can appear in the **Monitoring** tab.</span></span> <span data-ttu-id="fca4a-194">Připojovací řetězec je potřeba ke konfiguraci službu, kterou chcete zobrazit podrobné údaje z monitorování na portálu.</span><span class="sxs-lookup"><span data-stu-id="fca4a-194">The connection string is needed to configure the service to show verbose monitoring data in the portal.</span></span>

#### <a name="migrating-projects-to-azure-sdk-26-and-later"></a><span data-ttu-id="fca4a-195">Migrace projekty k Azure SDK 2.6 a novějším</span><span class="sxs-lookup"><span data-stu-id="fca4a-195">Migrating projects to Azure SDK 2.6 and later</span></span>
<span data-ttu-id="fca4a-196">Při migraci z Azure SDK 2.5 Azure SDK 2.6 nebo novější, pokud jste měli účet úložiště diagnostiky, který je zadaný v souboru .wadcfgx, pak zůstane existuje.</span><span class="sxs-lookup"><span data-stu-id="fca4a-196">When migrating from Azure SDK 2.5 to Azure SDK 2.6 or later, if you had a diagnostics storage account specified in the .wadcfgx file, then it will stay there.</span></span> <span data-ttu-id="fca4a-197">Abyste mohli využívat flexibilitu používání jiným účtům úložiště pro jiné úložiště, budete muset ručně přidat připojovací řetězec do projektu.</span><span class="sxs-lookup"><span data-stu-id="fca4a-197">To take advantage of the flexibility of using different storage accounts for different storage configurations, you'll have to manually add the connection string to your project.</span></span> <span data-ttu-id="fca4a-198">Pokud projekt jste migraci z Azure SDK 2.4 nebo starší na Azure SDK 2.6, se zachovají diagnostiky připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="fca4a-198">If you're migrating a project from Azure SDK 2.4 or earlier to Azure SDK 2.6, then the diagnostics connection strings are preserved.</span></span> <span data-ttu-id="fca4a-199">Upozorňujeme ale, změny v tom, jak se připojovací řetězce považují ve verzi 2.6 SDK Azure uvedeného v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="fca4a-199">However, please note the changes in how connection strings are treated in Azure SDK 2.6 as specified in the previous section.</span></span>

#### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a><span data-ttu-id="fca4a-200">Určuje, jak Visual Studio účet úložiště diagnostiky</span><span class="sxs-lookup"><span data-stu-id="fca4a-200">How Visual Studio determines the diagnostics storage account</span></span>
* <span data-ttu-id="fca4a-201">Pokud diagnostiky připojovací řetězec je zadané v souboru .cscfg, Visual Studio se používá ke konfiguraci rozšíření diagnostiky při publikování a při generování souborů xml veřejné konfigurace během balení.</span><span class="sxs-lookup"><span data-stu-id="fca4a-201">If a diagnostics connection string is specified in the .cscfg file, Visual Studio uses it to configure the diagnostics extension when publishing, and when generating the public configuration xml files during packaging.</span></span>
* <span data-ttu-id="fca4a-202">Pokud žádný diagnostiky připojovací řetězec je zadané v souboru .cscfg, pak Visual Studio se vrátí k používání účtu úložiště, zadaný v souboru .wadcfgx ke konfiguraci rozšíření diagnostiky při publikování a generování veřejného konfigurační soubory xml při balení.</span><span class="sxs-lookup"><span data-stu-id="fca4a-202">If no diagnostics connection string is specified in the .cscfg file, then Visual Studio falls back to using the storage account specified in the .wadcfgx file to configure the diagnostics extension when publishing, and generating the public configuration xml files when packaging.</span></span>
* <span data-ttu-id="fca4a-203">Diagnostika připojovací řetězec v souboru .cscfg má přednost před účet úložiště v souboru .wadcfgx.</span><span class="sxs-lookup"><span data-stu-id="fca4a-203">The diagnostics connection string in the .cscfg file takes precedence over the storage account in the .wadcfgx file.</span></span> <span data-ttu-id="fca4a-204">Pokud diagnostiky připojovací řetězec je zadané v souboru .cscfg, Visual Studio použije tento a ignoruje .wadcfgx účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="fca4a-204">If a diagnostics connection string is specified in the .cscfg file, then Visual Studio uses that and ignores the storage account in .wadcfgx.</span></span>

#### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a><span data-ttu-id="fca4a-205">Jaké jsou "aktualizace vývoj úložiště připojovací řetězce..."</span><span class="sxs-lookup"><span data-stu-id="fca4a-205">What does the "Update development storage connection strings…"</span></span> <span data-ttu-id="fca4a-206">zaškrtávací políčko dělat?</span><span class="sxs-lookup"><span data-stu-id="fca4a-206">checkbox do?</span></span>
<span data-ttu-id="fca4a-207">Zaškrtávací políčko pro **aktualizace vývoj úložiště připojovací řetězce pro diagnostiku a ukládání do mezipaměti pomocí přihlašovacích údajů účtu úložiště Microsoft Azure při publikování do služby Microsoft Azure** nabízí pohodlný způsob aktualizace jakékoli vývoj úložiště připojovací řetězce k účtu pomocí účtu úložiště Azure během publikování zadány.</span><span class="sxs-lookup"><span data-stu-id="fca4a-207">The checkbox for **Update development storage connection strings for Diagnostics and Caching with Microsoft Azure storage account credentials when publishing to Microsoft Azure** gives you a convenient way to update any development storage account connection strings with the Azure storage account specified during publishing.</span></span>

<span data-ttu-id="fca4a-208">Předpokládejme například, zaškrtněte toto políčko a diagnostiky připojovací řetězec Určuje `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="fca4a-208">For example, suppose you select this checkbox and the diagnostics connection string specifies `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="fca4a-209">Při publikování tohoto projektu v Azure, Visual Studio automaticky aktualizovat připojovací řetězec diagnostiky s účtem úložiště, který jste zadali v Průvodci publikovat.</span><span class="sxs-lookup"><span data-stu-id="fca4a-209">When you publish the project to Azure, Visual Studio will automatically update the diagnostics connection string with the storage account you specified in the Publish wizard.</span></span> <span data-ttu-id="fca4a-210">Pokud účet úložiště skutečné byl zadán jako připojovací řetězec diagnostiky, pak tento účet se ale používá místo.</span><span class="sxs-lookup"><span data-stu-id="fca4a-210">However, if a real storage account was specified as the diagnostics connection string, then that account is used instead.</span></span>

### <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a><span data-ttu-id="fca4a-211">Diagnostické funkce rozdíly mezi Azure SDK 2.4 a starší a Azure SDK, 2.5 nebo novější</span><span class="sxs-lookup"><span data-stu-id="fca4a-211">Diagnostics functionality differences between Azure SDK 2.4 and earlier and Azure SDK 2.5 and later</span></span>
<span data-ttu-id="fca4a-212">Pokud upgradujete projekt z Azure SDK 2.4 na Azure SDK 2.5 nebo novější, jste měli mít na paměti následující rozdíly funkce diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="fca4a-212">If you're upgrading your project from Azure SDK 2.4 to Azure SDK 2.5 or later, you should bear in mind the following diagnostics functionality differences.</span></span>

* <span data-ttu-id="fca4a-213">**Rozhraní API pro konfiguraci jsou zastaralé** – Programová konfigurace diagnostiky je k dispozici v Azure SDK 2.4 nebo starší verze, ale je zastaralý v Azure SDK 2.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fca4a-213">**Configuration APIs are deprecated** – Programmatic configuration of diagnostics is available in Azure SDK 2.4 or earlier versions, but is deprecated in Azure SDK 2.5 and later.</span></span> <span data-ttu-id="fca4a-214">Pokud vaše konfigurace diagnostiky je aktuálně definována v kódu, budete muset znovu nakonfigurujte tato nastavení ručně v migrovaných projektu, pro diagnostiku chcete pokračovat v práci.</span><span class="sxs-lookup"><span data-stu-id="fca4a-214">If your diagnostics configuration is currently defined in code, you'll need to reconfigure those settings from scratch in the migrated project in order for diagnostics to keep working.</span></span> <span data-ttu-id="fca4a-215">Diagnostika konfiguračního souboru pro Azure SDK 2.4 je diagnostics.wadcfg a diagnostics.wadcfgx pro Azure SDK 2.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fca4a-215">The diagnostics configuration file for Azure SDK 2.4 is diagnostics.wadcfg, and diagnostics.wadcfgx for Azure SDK 2.5 and later.</span></span>
* <span data-ttu-id="fca4a-216">**Diagnostika pro cloudové aplikace, služby se dá nakonfigurovat jenom na úrovni role, ne na úrovni instance.**</span><span class="sxs-lookup"><span data-stu-id="fca4a-216">**Diagnostics for cloud service applications can only be configured at the role level, not at the instance level.**</span></span>
* <span data-ttu-id="fca4a-217">**Pokaždé, když nasadíte aplikaci, konfiguraci diagnostiky se aktualizuje** – to může způsobit problémy parita, pokud změníte konfiguraci diagnostiky z Průzkumníka serveru a pak znovu nasadit aplikace.</span><span class="sxs-lookup"><span data-stu-id="fca4a-217">**Every time you deploy your app, the diagnostics configuration is updated** – This can cause parity issues if you change your diagnostics configuration from Server Explorer and then redeploy your app.</span></span>
* <span data-ttu-id="fca4a-218">**V Azure SDK 2.5 nebo novější, výpisy stavu systému jsou nakonfigurované v konfiguračním souboru diagnostics není v kódu** – Pokud máte nakonfigurované v kódu výpisy stavu systému, budete muset ručně přenos konfigurace z kódu do konfiguračního souboru, protože výpisy stavu systému nejsou přenést během migrace na Azure SDK 2.6.</span><span class="sxs-lookup"><span data-stu-id="fca4a-218">**In Azure SDK 2.5 and later, crash dumps are configured in the diagnostics configuration file, not in code** – If you have crash dumps configured in code, you'll have to manually transfer the configuration from code to the configuration file, because the crash dumps aren't transferred during the migration to Azure SDK 2.6.</span></span>

