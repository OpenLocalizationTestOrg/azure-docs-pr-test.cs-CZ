---
title: "Poznámky k verzi sady Azure SDK pro .NET 2.9"
description: "Poznámky k verzi sady Azure SDK pro .NET 2.9"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 199f0906f73d693d7cd4b73c928f23ae83b99596
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-sdk-for-net-29-release-notes"></a><span data-ttu-id="1dec4-103">Poznámky k verzi sady Azure SDK pro .NET 2.9</span><span class="sxs-lookup"><span data-stu-id="1dec4-103">Azure SDK for .NET 2.9 release notes</span></span>

<span data-ttu-id="1dec4-104">Toto téma obsahuje poznámky k verzi pro verze 2.9 a 2.9.6 sady Azure SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="1dec4-104">This topic includes release notes for versions 2.9 and 2.9.6 of Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-296-release-summary"></a><span data-ttu-id="1dec4-105">Azure SDK pro .NET 2.9.6 verze souhrn</span><span class="sxs-lookup"><span data-stu-id="1dec4-105">Azure SDK for .NET 2.9.6 release summary</span></span>

<span data-ttu-id="1dec4-106">Datum vydání: 11/16/2016</span><span class="sxs-lookup"><span data-stu-id="1dec4-106">Release date: 11/16/2016</span></span>
 
<span data-ttu-id="1dec4-107">V této verzi byly zavedeny žádné nejnovější změny do Azure SDK 2.9.</span><span class="sxs-lookup"><span data-stu-id="1dec4-107">No breaking changes to the Azure SDK 2.9 have been introduced in this release.</span></span> <span data-ttu-id="1dec4-108">Je také žádné procesu upgradu potřeby využít tuto sadu SDK s existující projekty cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="1dec4-108">There is also no upgrade process needed to leverage this SDK with existing Cloud Service projects.</span></span>

### <a name="visual-studio-2017-release-candidate"></a><span data-ttu-id="1dec4-109">Visual Studio 2017 Release Candidate</span><span class="sxs-lookup"><span data-stu-id="1dec4-109">Visual Studio 2017 Release Candidate</span></span>

- <span data-ttu-id="1dec4-110">V sadě Visual Studio 2017 RC tato verze sady Azure SDK pro .NET je součástí pracovního vytížení Azure.</span><span class="sxs-lookup"><span data-stu-id="1dec4-110">In Visual Studio 2017 RC, this release of the Azure SDK for .NET is built in in the Azure Workload.</span></span> <span data-ttu-id="1dec4-111">Všechny nástroje, které musíte udělat Azure development budou součástí Visual Studio RC 2017 do budoucna.</span><span class="sxs-lookup"><span data-stu-id="1dec4-111">All the tools you need to do Azure development will be part of Visual Studio 2017 RC going forward.</span></span> <span data-ttu-id="1dec4-112">Pro Visual Studio 2015 a Visual Studio 2013 sadu SDK bude stále k dispozici prostřednictvím WebPI.</span><span class="sxs-lookup"><span data-stu-id="1dec4-112">For Visual Studio 2015 and Visual Studio 2013, the SDK will still be available through WebPI.</span></span> <span data-ttu-id="1dec4-113">Již Azure SDK pro .NET verze pro sadu Visual Studio 2013, když Visual Studio 2017 uvolní jako poslední produktu.</span><span class="sxs-lookup"><span data-stu-id="1dec4-113">We will be discontinuing Azure SDK for .NET releases for Visual Studio 2013, when Visual Studio 2017 releases as a final product.</span></span> <span data-ttu-id="1dec4-114">Tento odkaz ke stažení sady Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span><span class="sxs-lookup"><span data-stu-id="1dec4-114">Follow this link to download Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="1dec4-115">Diagnostika Azure</span><span class="sxs-lookup"><span data-stu-id="1dec4-115">Azure Diagnostics</span></span>

- <span data-ttu-id="1dec4-116">Změnit chování pouze ukládat částečné připojovací řetězec s klíčem nahrazuje token pro připojovací řetězec úložiště diagnostiky cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="1dec4-116">Changed the behavior to only store a partial connection string with the key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="1dec4-117">Klíč skutečné úložiště jsou teď uložená v složce profilu uživatele, přístup se dá řídit.</span><span class="sxs-lookup"><span data-stu-id="1dec4-117">The actual storage key is now stored in the user profile folder so its access can be controlled.</span></span> <span data-ttu-id="1dec4-118">Visual Studio bude číst ze složky profilu uživatele pro místní ladění a proces publikování klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="1dec4-118">Visual Studio will read the storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="1dec4-119">V reakci na změnu popsané výše Visual Studio Online team rozšířené šablony úloh nasazení Azure Cloud Services, mohou uživatelé zadat klíč úložiště pro nastavení rozšíření diagnostiky při publikování do Azure na průběžnou integraci a nasazení.</span><span class="sxs-lookup"><span data-stu-id="1dec4-119">In response to the change described above, Visual Studio Online team enhanced the Azure Cloud Services deployment task template so users could specify the storage key for setting diagnostics extension when publishing to Azure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="1dec4-120">Jsme provedli jsme je umožněno ukládání zabezpečené připojovací řetězec a tokenizaci pro Azure Diagnostics (WAD), které vám pomůžou vyřešit problémy s konfigurací napříč environements.</span><span class="sxs-lookup"><span data-stu-id="1dec4-120">We’ve made it possible to store secure connection string and tokenization for Azure Diagnostics (WAD), to help you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="1dec4-121">Windows Server 2016 virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="1dec4-121">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="1dec4-122">Visual Studio teď podporuje nasazení cloudové služby na virtuální počítače operačního systému rodiny 5 (Windows Server 2016).</span><span class="sxs-lookup"><span data-stu-id="1dec4-122">Visual Studio now supports deploying Cloud Services to OS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="1dec4-123">Pro existující cloudové služby můžete změnit nastavení pro nové řada operačního systému.</span><span class="sxs-lookup"><span data-stu-id="1dec4-123">For existing cloud services, you can change your settings to target the new OS Family.</span></span> <span data-ttu-id="1dec4-124">Při vytváření nových cloudových služeb, pokud zvolíte možnost vytvoření služby pomocí rozhraní .net 4.6 nebo vyšší, bude použita výchozí služba používají operačního systému rodiny 5.</span><span class="sxs-lookup"><span data-stu-id="1dec4-124">When creating new cloud services, if you choose to create the service using .net 4.6 or higher, it will default the service to use OS Family 5.</span></span>  <span data-ttu-id="1dec4-125">Další informace najdete [skupina hostovaných operačních systémů podporují tabulky](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span><span class="sxs-lookup"><span data-stu-id="1dec4-125">For more information, you can review the [Guest OS Family support table](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span></span>

#### <a name="known-issues"></a><span data-ttu-id="1dec4-126">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="1dec4-126">Known issues</span></span>

- <span data-ttu-id="1dec4-127">Azure .NET SDK 2.9.6 zavedená omezení, které blokuje nasazení projektů pomocí nepodporované rozhraní .NET Framework (například .NET 4.6) do všech operačního systému rodiny < 5.</span><span class="sxs-lookup"><span data-stu-id="1dec4-127">Azure .NET SDK 2.9.6 introduced a restriction that blocks deployment of projects using unsupported .NET frameworks (such as .NET 4.6) to any OS Family < 5.</span></span> <span data-ttu-id="1dec4-128">Alternativní řešení [zde](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span><span class="sxs-lookup"><span data-stu-id="1dec4-128">A workaround is provided [here](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="1dec4-129">Mezipaměť hostovaná v instanci Role na Azure</span><span class="sxs-lookup"><span data-stu-id="1dec4-129">Azure In-Role Cache</span></span> 

- <span data-ttu-id="1dec4-130">Podpora pro Azure Cache v roli končí na 30. listopadu 2016.</span><span class="sxs-lookup"><span data-stu-id="1dec4-130">Support for Azure In-Role Cache ends on November 30, 2016.</span></span> <span data-ttu-id="1dec4-131">Další podrobnosti získáte kliknutím na [zde](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="1dec4-131">For more details, click [here](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>

### <a name="azure-resource-manager-templates-for-azure-stack"></a><span data-ttu-id="1dec4-132">Šablony Azure Resource Manageru pro Azure zásobníku</span><span class="sxs-lookup"><span data-stu-id="1dec4-132">Azure Resource Manager Templates for Azure Stack</span></span>

- <span data-ttu-id="1dec4-133">Zavedli jsme zásobník Azure jako cíl nasazení pro vaše šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1dec4-133">We’ve introduced Azure Stack as a deployment target for your Azure Resource Manager Templates.</span></span>


## <a name="azure-sdk-for-net-29-summary"></a><span data-ttu-id="1dec4-134">Azure SDK pro .NET 2.9 souhrn</span><span class="sxs-lookup"><span data-stu-id="1dec4-134">Azure SDK for .NET 2.9 summary</span></span>

## <a name="overview"></a><span data-ttu-id="1dec4-135">Přehled</span><span class="sxs-lookup"><span data-stu-id="1dec4-135">Overview</span></span>
<span data-ttu-id="1dec4-136">Tento dokument obsahuje poznámky k verzi sady Azure SDK pro .NET 2.9 verze.</span><span class="sxs-lookup"><span data-stu-id="1dec4-136">This document contains the release notes for the Azure SDK for .NET 2.9 release.</span></span> 

<span data-ttu-id="1dec4-137">Podrobné informace o aktualizacích v této verzi najdete v tématu [sadu Azure SDK 2.9 oznámení post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span><span class="sxs-lookup"><span data-stu-id="1dec4-137">For detailed information about updates in this release, see the [Azure SDK 2.9 announcement post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span></span>

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a><span data-ttu-id="1dec4-138">Azure SDK 2.9 pro Visual Studio 2015 Update 2 a Visual Studio "15" Preview</span><span class="sxs-lookup"><span data-stu-id="1dec4-138">Azure SDK 2.9 for Visual Studio 2015 Update 2 and Visual Studio "15" Preview</span></span>
<span data-ttu-id="1dec4-139">Tato aktualizace zahrnuje následující opravy chyb:</span><span class="sxs-lookup"><span data-stu-id="1dec4-139">This update includes the following bug fixes:</span></span>

* <span data-ttu-id="1dec4-140">Problém související s REST API generování klienta v, ve kterém se zobrazí řetězec "Neznámý typ" jako název složky generace kód nebo název oboru názvů vyřadit do generovaného kódu.</span><span class="sxs-lookup"><span data-stu-id="1dec4-140">Issue related to REST API Client Generation in in which the string "Unknown Type” would appear as the name of the code-gen folder and/or the name of the namespace dropped into the generated code.</span></span>
* <span data-ttu-id="1dec4-141">Problém týkající se naplánované WebJobs, ve kterém ověřovacích informací došlo k selhání mají být předány Plánovač procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="1dec4-141">Issue related to Scheduled WebJobs in which the authentication information was failing to be passed to the Scheduler provisioning process.</span></span>

<span data-ttu-id="1dec4-142">Tato aktualizace zahrnuje následující nové funkce:</span><span class="sxs-lookup"><span data-stu-id="1dec4-142">This update includes the following new feature:</span></span>

* <span data-ttu-id="1dec4-143">Podpora pro sekundární aplikační služby na kartě "Služby" dialogové okno zřizování App Service.</span><span class="sxs-lookup"><span data-stu-id="1dec4-143">Support for secondary App Services in the "Services" tab of the App Service provisioning dialog.</span></span> 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a><span data-ttu-id="1dec4-144">Nástroje Azure Data Lake pro Visual Studio 2015 Update 2</span><span class="sxs-lookup"><span data-stu-id="1dec4-144">Azure Data Lake Tools for Visual Studio 2015 Update 2</span></span>
<span data-ttu-id="1dec4-145">Tato aktualizace zahrnuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="1dec4-145">This updates includes the following:</span></span>

* <span data-ttu-id="1dec4-146">**Nástroje Azure Data Lake** pro Visual Studio je nyní sloučeny do Azure SDK pro .NET verze.</span><span class="sxs-lookup"><span data-stu-id="1dec4-146">**Azure Data Lake Tools** for Visual Studio is now merged into the Azure SDK for .NET release.</span></span> <span data-ttu-id="1dec4-147">Tento nástroj se automaticky nainstaluje při instalaci sady Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="1dec4-147">The tool is automatically installed when you install Azure SDK.</span></span> 
  
    <span data-ttu-id="1dec4-148">Tento nástroj se často aktualizuje, přejděte [sem](http://aka.ms/datalaketool) získat aktualizace.</span><span class="sxs-lookup"><span data-stu-id="1dec4-148">The tool is updated frequently, go [here](http://aka.ms/datalaketool) to get the updates.</span></span>
* <span data-ttu-id="1dec4-149">**V Průzkumníku serveru** teď umožňuje zobrazit všechny a vytvořit některé entity metadata U-SQL.</span><span class="sxs-lookup"><span data-stu-id="1dec4-149">**Server Explorer** now enables you to view all and create some U-SQL metadata entities.</span></span> <span data-ttu-id="1dec4-150">Další informace najdete v tématu [to](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blogu.</span><span class="sxs-lookup"><span data-stu-id="1dec4-150">For more information, see [this](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.</span></span>

## <a name="hdinsight-tools"></a><span data-ttu-id="1dec4-151">Nástroje HDInsight</span><span class="sxs-lookup"><span data-stu-id="1dec4-151">HDInsight Tools</span></span>
<span data-ttu-id="1dec4-152">**Nástroje HDInsight** pro sadu Visual Studio teď podporuje HDInsight verze 3.3, včetně grafech Tez a dalších jazyků opravy.</span><span class="sxs-lookup"><span data-stu-id="1dec4-152">**HDInsight Tools** for Visual Studio now supports HDInsight version 3.3, including showing Tez graphs and other language fixes.</span></span>

## <a name="azure-resource-manager"></a><span data-ttu-id="1dec4-153">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1dec4-153">Azure Resource Manager</span></span>
<span data-ttu-id="1dec4-154">Tato verze přidává [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) podporu pro šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="1dec4-154">This release adds [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) support for Resource Manager templates.</span></span>

## <a name="see-also"></a><span data-ttu-id="1dec4-155">Viz také</span><span class="sxs-lookup"><span data-stu-id="1dec4-155">See also</span></span>
[<span data-ttu-id="1dec4-156">Azure SDK 2.9 oznámení post</span><span class="sxs-lookup"><span data-stu-id="1dec4-156">Azure SDK 2.9 announcement post</span></span>](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

