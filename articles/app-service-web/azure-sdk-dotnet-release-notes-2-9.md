---
title: "aaaAzure SDK pro .NET 2.9 poznámky"
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
ms.openlocfilehash: 96df2b80224190cc2093e6bf350eaec224ac2e98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-29-release-notes"></a><span data-ttu-id="0c7c3-103">Poznámky k verzi sady Azure SDK pro .NET 2.9</span><span class="sxs-lookup"><span data-stu-id="0c7c3-103">Azure SDK for .NET 2.9 release notes</span></span>

<span data-ttu-id="0c7c3-104">Toto téma obsahuje poznámky k verzi pro verze 2.9 a 2.9.6 sady Azure SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-104">This topic includes release notes for versions 2.9 and 2.9.6 of Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-296-release-summary"></a><span data-ttu-id="0c7c3-105">Azure SDK pro .NET 2.9.6 verze souhrn</span><span class="sxs-lookup"><span data-stu-id="0c7c3-105">Azure SDK for .NET 2.9.6 release summary</span></span>

<span data-ttu-id="0c7c3-106">Datum vydání: 11/16/2016</span><span class="sxs-lookup"><span data-stu-id="0c7c3-106">Release date: 11/16/2016</span></span>
 
<span data-ttu-id="0c7c3-107">V této verzi byly zavedeny žádné toohello změny nejnovější sadu Azure SDK 2.9.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-107">No breaking changes toohello Azure SDK 2.9 have been introduced in this release.</span></span> <span data-ttu-id="0c7c3-108">Je také žádné tooleverage procesu upgradu potřeba tuto sadu SDK s existující projekty cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-108">There is also no upgrade process needed tooleverage this SDK with existing Cloud Service projects.</span></span>

### <a name="visual-studio-2017-release-candidate"></a><span data-ttu-id="0c7c3-109">Visual Studio 2017 Release Candidate</span><span class="sxs-lookup"><span data-stu-id="0c7c3-109">Visual Studio 2017 Release Candidate</span></span>

- <span data-ttu-id="0c7c3-110">V sadě Visual Studio 2017 RC tato verze hello Azure SDK pro .NET je součástí hello pracovního vytížení Azure.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-110">In Visual Studio 2017 RC, this release of hello Azure SDK for .NET is built in in hello Azure Workload.</span></span> <span data-ttu-id="0c7c3-111">Všechny hello nástroje, které potřebujete toodo Azure development budou součástí Visual Studio RC 2017 do budoucna.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-111">All hello tools you need toodo Azure development will be part of Visual Studio 2017 RC going forward.</span></span> <span data-ttu-id="0c7c3-112">Pro Visual Studio 2015 a Visual Studio 2013 hello SDK bude stále k dispozici prostřednictvím WebPI.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-112">For Visual Studio 2015 and Visual Studio 2013, hello SDK will still be available through WebPI.</span></span> <span data-ttu-id="0c7c3-113">Již Azure SDK pro .NET verze pro sadu Visual Studio 2013, když Visual Studio 2017 uvolní jako poslední produktu.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-113">We will be discontinuing Azure SDK for .NET releases for Visual Studio 2013, when Visual Studio 2017 releases as a final product.</span></span> <span data-ttu-id="0c7c3-114">Použijte tento odkaz toodownload Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span><span class="sxs-lookup"><span data-stu-id="0c7c3-114">Follow this link toodownload Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="0c7c3-115">Diagnostika Azure</span><span class="sxs-lookup"><span data-stu-id="0c7c3-115">Azure Diagnostics</span></span>

- <span data-ttu-id="0c7c3-116">Změněné hello chování tooonly ukládat částečné připojovací řetězec s klíčem hello nahrazuje token pro připojovací řetězec úložiště diagnostiky cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-116">Changed hello behavior tooonly store a partial connection string with hello key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="0c7c3-117">klíč Hello skutečné úložiště jsou teď uložená v složky profilu uživatele hello tak přístup se dá řídit.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-117">hello actual storage key is now stored in hello user profile folder so its access can be controlled.</span></span> <span data-ttu-id="0c7c3-118">Visual Studio, bude číst klíč úložiště hello ze složky profilu uživatele pro místní ladění a proces publikování.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-118">Visual Studio will read hello storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="0c7c3-119">V odpovědi toohello změn popsané výše Visual Studio Online team rozšířené hello šablonu úloh nasazení Azure Cloud Services, uživatelé mohou zadat klíč hello úložiště pro nastavení rozšíření diagnostiky při publikování tooAzure v průběžnou integraci a nasazení.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-119">In response toohello change described above, Visual Studio Online team enhanced hello Azure Cloud Services deployment task template so users could specify hello storage key for setting diagnostics extension when publishing tooAzure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="0c7c3-120">Provedli jsme ji možné toostore zabezpečené připojovací řetězec a tokenizaci pro Azure Diagnostics (WAD), toohelp řešení problémů s konfigurací napříč environements.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-120">We’ve made it possible toostore secure connection string and tokenization for Azure Diagnostics (WAD), toohelp you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="0c7c3-121">Windows Server 2016 virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="0c7c3-121">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="0c7c3-122">Visual Studio teď podporuje nasazení cloudové služby tooOS rodiny 5 (Windows Server 2016) virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-122">Visual Studio now supports deploying Cloud Services tooOS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="0c7c3-123">Pro existující cloudové služby, můžete změnit vaše nastavení tootarget hello nové řada operačního systému.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-123">For existing cloud services, you can change your settings tootarget hello new OS Family.</span></span> <span data-ttu-id="0c7c3-124">Při vytváření nových cloudových služeb, pokud se rozhodnete toocreate hello služby pomocí rozhraní .net 4.6 nebo vyšší, bude použita výchozí služba toouse hello operačního systému rodiny 5.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-124">When creating new cloud services, if you choose toocreate hello service using .net 4.6 or higher, it will default hello service toouse OS Family 5.</span></span>  <span data-ttu-id="0c7c3-125">Další informace, můžete zkontrolovat hello [skupina hostovaných operačních systémů podporují tabulky](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span><span class="sxs-lookup"><span data-stu-id="0c7c3-125">For more information, you can review hello [Guest OS Family support table](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0c7c3-126">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="0c7c3-126">Known issues</span></span>

- <span data-ttu-id="0c7c3-127">Azure .NET SDK 2.9.6 zavedená omezení, které blokuje nasazení projektů pomocí nepodporované .NET rozhraní (například .NET 4.6) tooany operačních systémů < 5.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-127">Azure .NET SDK 2.9.6 introduced a restriction that blocks deployment of projects using unsupported .NET frameworks (such as .NET 4.6) tooany OS Family < 5.</span></span> <span data-ttu-id="0c7c3-128">Alternativní řešení [zde](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span><span class="sxs-lookup"><span data-stu-id="0c7c3-128">A workaround is provided [here](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="0c7c3-129">Mezipaměť hostovaná v instanci Role na Azure</span><span class="sxs-lookup"><span data-stu-id="0c7c3-129">Azure In-Role Cache</span></span> 

- <span data-ttu-id="0c7c3-130">Podpora pro Azure Cache v roli končí na 30. listopadu 2016.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-130">Support for Azure In-Role Cache ends on November 30, 2016.</span></span> <span data-ttu-id="0c7c3-131">Další podrobnosti získáte kliknutím na [zde](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="0c7c3-131">For more details, click [here](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>

### <a name="azure-resource-manager-templates-for-azure-stack"></a><span data-ttu-id="0c7c3-132">Šablony Azure Resource Manageru pro Azure zásobníku</span><span class="sxs-lookup"><span data-stu-id="0c7c3-132">Azure Resource Manager Templates for Azure Stack</span></span>

- <span data-ttu-id="0c7c3-133">Zavedli jsme zásobník Azure jako cíl nasazení pro vaše šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-133">We’ve introduced Azure Stack as a deployment target for your Azure Resource Manager Templates.</span></span>


## <a name="azure-sdk-for-net-29-summary"></a><span data-ttu-id="0c7c3-134">Azure SDK pro .NET 2.9 souhrn</span><span class="sxs-lookup"><span data-stu-id="0c7c3-134">Azure SDK for .NET 2.9 summary</span></span>

## <a name="overview"></a><span data-ttu-id="0c7c3-135">Přehled</span><span class="sxs-lookup"><span data-stu-id="0c7c3-135">Overview</span></span>
<span data-ttu-id="0c7c3-136">Tento dokument obsahuje hello poznámky k verzi pro hello Azure SDK pro .NET 2.9 verzi.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-136">This document contains hello release notes for hello Azure SDK for .NET 2.9 release.</span></span> 

<span data-ttu-id="0c7c3-137">Podrobné informace o aktualizacích v této verzi najdete v tématu hello [sadu Azure SDK 2.9 oznámení post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span><span class="sxs-lookup"><span data-stu-id="0c7c3-137">For detailed information about updates in this release, see hello [Azure SDK 2.9 announcement post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span></span>

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a><span data-ttu-id="0c7c3-138">Azure SDK 2.9 pro Visual Studio 2015 Update 2 a Visual Studio "15" Preview</span><span class="sxs-lookup"><span data-stu-id="0c7c3-138">Azure SDK 2.9 for Visual Studio 2015 Update 2 and Visual Studio "15" Preview</span></span>
<span data-ttu-id="0c7c3-139">Tato aktualizace zahrnuje hello následující opravy chyb:</span><span class="sxs-lookup"><span data-stu-id="0c7c3-139">This update includes hello following bug fixes:</span></span>

* <span data-ttu-id="0c7c3-140">Vystavení související tooREST rozhraní API klienta generování v které hello řetězce "Neznámý typ" se zobrazí jako hello název složky kód generace hello nebo hello název oboru názvů hello do hello vygenerovat kód.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-140">Issue related tooREST API Client Generation in in which hello string "Unknown Type” would appear as hello name of hello code-gen folder and/or hello name of hello namespace dropped into hello generated code.</span></span>
* <span data-ttu-id="0c7c3-141">Vydejte související tooScheduled webové úlohy, ve které hello došlo ověřovací informace k selhání toobe předán procesu zřizování toohello plánovače.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-141">Issue related tooScheduled WebJobs in which hello authentication information was failing toobe passed toohello Scheduler provisioning process.</span></span>

<span data-ttu-id="0c7c3-142">Tato aktualizace zahrnuje hello následující nové funkce:</span><span class="sxs-lookup"><span data-stu-id="0c7c3-142">This update includes hello following new feature:</span></span>

* <span data-ttu-id="0c7c3-143">Podpora pro sekundární App Services kartě "Služby" hello hello dialogové okno zřizování App Service.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-143">Support for secondary App Services in hello "Services" tab of hello App Service provisioning dialog.</span></span> 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a><span data-ttu-id="0c7c3-144">Nástroje Azure Data Lake pro Visual Studio 2015 Update 2</span><span class="sxs-lookup"><span data-stu-id="0c7c3-144">Azure Data Lake Tools for Visual Studio 2015 Update 2</span></span>
<span data-ttu-id="0c7c3-145">Tato aktualizace zahrnuje následující hello:</span><span class="sxs-lookup"><span data-stu-id="0c7c3-145">This updates includes hello following:</span></span>

* <span data-ttu-id="0c7c3-146">**Nástroje Azure Data Lake** pro Visual Studio je nyní sloučeny do hello Azure SDK pro .NET verze.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-146">**Azure Data Lake Tools** for Visual Studio is now merged into hello Azure SDK for .NET release.</span></span> <span data-ttu-id="0c7c3-147">Nástroj Hello se automaticky nainstaluje při instalaci sady Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-147">hello tool is automatically installed when you install Azure SDK.</span></span> 
  
    <span data-ttu-id="0c7c3-148">Hello nástroj se často aktualizuje, přejděte [sem](http://aka.ms/datalaketool) tooget hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-148">hello tool is updated frequently, go [here](http://aka.ms/datalaketool) tooget hello updates.</span></span>
* <span data-ttu-id="0c7c3-149">**V Průzkumníku serveru** nyní vám umožní tooview všechny nebo vytvořit některé entity metadata U-SQL.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-149">**Server Explorer** now enables you tooview all and create some U-SQL metadata entities.</span></span> <span data-ttu-id="0c7c3-150">Další informace najdete v tématu [to](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blogu.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-150">For more information, see [this](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.</span></span>

## <a name="hdinsight-tools"></a><span data-ttu-id="0c7c3-151">Nástroje HDInsight</span><span class="sxs-lookup"><span data-stu-id="0c7c3-151">HDInsight Tools</span></span>
<span data-ttu-id="0c7c3-152">**Nástroje HDInsight** pro sadu Visual Studio teď podporuje HDInsight verze 3.3, včetně grafech Tez a dalších jazyků opravy.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-152">**HDInsight Tools** for Visual Studio now supports HDInsight version 3.3, including showing Tez graphs and other language fixes.</span></span>

## <a name="azure-resource-manager"></a><span data-ttu-id="0c7c3-153">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0c7c3-153">Azure Resource Manager</span></span>
<span data-ttu-id="0c7c3-154">Tato verze přidává [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) podporu pro šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="0c7c3-154">This release adds [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) support for Resource Manager templates.</span></span>

## <a name="see-also"></a><span data-ttu-id="0c7c3-155">Viz také</span><span class="sxs-lookup"><span data-stu-id="0c7c3-155">See also</span></span>
[<span data-ttu-id="0c7c3-156">Azure SDK 2.9 oznámení post</span><span class="sxs-lookup"><span data-stu-id="0c7c3-156">Azure SDK 2.9 announcement post</span></span>](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

