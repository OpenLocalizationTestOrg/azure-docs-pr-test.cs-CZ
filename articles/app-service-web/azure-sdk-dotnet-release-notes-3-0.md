---
title: "Azure SDK pro rozhraní .NET 3.0 poznámky k verzi | Microsoft Docs"
description: "Poznámky k verzi sady Azure SDK pro rozhraní .NET 3.0"
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
ms.date: 03/07/2017
ms.author: juliako
ms.openlocfilehash: eea4e569ac2d0192ed7872d2fcb9bed03614832b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-sdk-for-net-30-release-notes"></a><span data-ttu-id="dedcb-103">Poznámky k verzi sady Azure SDK pro rozhraní .NET 3.0</span><span class="sxs-lookup"><span data-stu-id="dedcb-103">Azure SDK for .NET 3.0 release notes</span></span>

<span data-ttu-id="dedcb-104">Toto téma obsahuje poznámky k verzi pro verzi 3.0 sady Azure SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="dedcb-104">This topic includes release notes for version 3.0 of the Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-30-release-summary"></a><span data-ttu-id="dedcb-105">Azure SDK pro verzi rozhraní .NET 3.0 souhrn</span><span class="sxs-lookup"><span data-stu-id="dedcb-105">Azure SDK for .NET 3.0 release summary</span></span>

<span data-ttu-id="dedcb-106">Datum vydání: 03/07/2017</span><span class="sxs-lookup"><span data-stu-id="dedcb-106">Release date: 03/07/2017</span></span>
 
<span data-ttu-id="dedcb-107">V této verzi byly zavedeny žádné nejnovější změny do Azure SDK 3.0.</span><span class="sxs-lookup"><span data-stu-id="dedcb-107">No breaking changes to the Azure SDK 3.0 have been introduced in this release.</span></span> <span data-ttu-id="dedcb-108">Je také žádné procesu upgradu potřeby využít tuto sadu SDK s existující projekty cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="dedcb-108">There is also no upgrade process needed to leverage this SDK with existing Cloud Service projects.</span></span> <span data-ttu-id="dedcb-109">Povolit použití Azure SDK 3.0 bez nutnosti upgradu, Azure SDK 3.0 nainstaluje do stejných adresářích jako sadu Azure SDK 2.9.</span><span class="sxs-lookup"><span data-stu-id="dedcb-109">To allow use of the Azure SDK 3.0 without requiring an upgrade process, Azure SDK 3.0 installs to the same directories as Azure SDK 2.9.</span></span> <span data-ttu-id="dedcb-110">Většina komponenty nezměnila hlavní verzi z 2.9, ale místo toho právě aktualizovat číslo sestavení.</span><span class="sxs-lookup"><span data-stu-id="dedcb-110">Most the components did not change the major version from 2.9 but instead just updated the build number.</span></span>

## <a name="visual-studio-2017-rtw"></a><span data-ttu-id="dedcb-111">Visual Studio 2017 RTW</span><span class="sxs-lookup"><span data-stu-id="dedcb-111">Visual Studio 2017 RTW</span></span>

- <span data-ttu-id="dedcb-112">V aplikaci Visual Studio 2017 tato verze sady Azure SDK pro .NET je založená na pracovního vytížení Azure.</span><span class="sxs-lookup"><span data-stu-id="dedcb-112">In Visual Studio 2017, this release of the Azure SDK for .NET is built in to the Azure Workload.</span></span> <span data-ttu-id="dedcb-113">Všechny nástroje, které musíte udělat Azure development budou součástí Visual Studio 2017 do budoucna.</span><span class="sxs-lookup"><span data-stu-id="dedcb-113">All the tools you need to do Azure development will be part of Visual Studio 2017 going forward.</span></span> <span data-ttu-id="dedcb-114">Pro Visual Studio 2015 SDK bude stále k dispozici prostřednictvím WebPI.</span><span class="sxs-lookup"><span data-stu-id="dedcb-114">For Visual Studio 2015 the SDK will still be available through WebPI.</span></span> <span data-ttu-id="dedcb-115">Azure SDK pro .NET verze jsme mít nepoužívá pro Visual Studio 2013 teď, když byla vydána Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="dedcb-115">We have discontinued Azure SDK for .NET releases for Visual Studio 2013 now that Visual Studio 2017 has been released.</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="dedcb-116">Diagnostika Azure</span><span class="sxs-lookup"><span data-stu-id="dedcb-116">Azure Diagnostics</span></span>

- <span data-ttu-id="dedcb-117">Změnit chování pouze ukládat částečné připojovací řetězec s klíčem nahrazuje token pro připojovací řetězec úložiště diagnostiky cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="dedcb-117">Changed the behavior to only store a partial connection string with the key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="dedcb-118">Klíč skutečné úložiště jsou teď uložená v složce profilu uživatele, přístup se dá řídit.</span><span class="sxs-lookup"><span data-stu-id="dedcb-118">The actual storage key is now stored in the user profile folder so its access can be controlled.</span></span> <span data-ttu-id="dedcb-119">Visual Studio bude číst ze složky profilu uživatele pro místní ladění a proces publikování klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="dedcb-119">Visual Studio will read the storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="dedcb-120">V reakci na změnu popsané výše Visual Studio Online team rozšířené šablony úloh nasazení Azure Cloud Services, mohou uživatelé zadat klíč úložiště pro nastavení rozšíření diagnostiky při publikování do Azure na průběžnou integraci a nasazení.</span><span class="sxs-lookup"><span data-stu-id="dedcb-120">In response to the change described above, Visual Studio Online team enhanced the Azure Cloud Services deployment task template so users could specify the storage key for setting diagnostics extension when publishing to Azure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="dedcb-121">Jsme provedli jsme je umožněno ukládání zabezpečené připojovací řetězec a tokenizaci pro Azure Diagnostics (WAD), které vám pomůžou vyřešit problémy s konfigurací napříč environements.</span><span class="sxs-lookup"><span data-stu-id="dedcb-121">We’ve made it possible to store secure connection string and tokenization for Azure Diagnostics (WAD), to help you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="dedcb-122">Windows Server 2016 virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="dedcb-122">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="dedcb-123">Visual Studio teď podporuje nasazení cloudové služby na virtuální počítače operačního systému rodiny 5 (Windows Server 2016).</span><span class="sxs-lookup"><span data-stu-id="dedcb-123">Visual Studio now supports deploying Cloud Services to OS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="dedcb-124">Pro existující cloudové služby můžete změnit nastavení pro nové řada operačního systému.</span><span class="sxs-lookup"><span data-stu-id="dedcb-124">For existing cloud services, you can change your settings to target the new OS Family.</span></span> <span data-ttu-id="dedcb-125">Při vytváření nových cloudových služeb, pokud zvolíte možnost vytvoření služby pomocí rozhraní .net 4.6 nebo vyšší, bude použita výchozí služba používají operačního systému rodiny 5.</span><span class="sxs-lookup"><span data-stu-id="dedcb-125">When creating new cloud services, if you choose to create the service using .net 4.6 or higher, it will default the service to use OS Family 5.</span></span>  <span data-ttu-id="dedcb-126">Další informace najdete [skupina hostovaných operačních systémů podporují tabulky](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="dedcb-126">For more information, you can review the [Guest OS Family support table](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span>

### <a name="known-issues"></a><span data-ttu-id="dedcb-127">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="dedcb-127">Known issues</span></span>

- <span data-ttu-id="dedcb-128">Azure .NET SDK 3.0 zavedla problém při odebírání Visual Studio 2017 v konfiguraci vedle sebe s Visual Studiem 2015.</span><span class="sxs-lookup"><span data-stu-id="dedcb-128">Azure .NET SDK 3.0 introduced an issue when removing Visual Studio 2017 in a side by side configuration with Visual Studio 2015.</span></span>  <span data-ttu-id="dedcb-129">Pokud máte sadu Azure SDK pro Visual Studio 2015 nainstalována, emulátor úložiště Microsoft Azure a Microsoft Azure výpočetní emulátor budou odebrány, pokud odinstalujete Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="dedcb-129">If you have the Azure SDK installed for Visual Studio 2015, the Microsoft Azure Storage Emulator and Microsoft Azure Compute Emulator will be removed if you uninstall Visual Studio 2017.</span></span>  <span data-ttu-id="dedcb-130">To způsobí chybu při vytváření a ladění nové projekty cloudových služeb v sadě Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="dedcb-130">This will produce an error when creating and debugging new Cloud services projects in Visual Studio 2015.</span></span> <span data-ttu-id="dedcb-131">Chcete-li tento problém vyřešit, znovu nainstalujte sadu Azure SDK z webové platformy.</span><span class="sxs-lookup"><span data-stu-id="dedcb-131">In order to fix this issue,  reinstall the Azure SDK from the Web Platform Installer.</span></span>  <span data-ttu-id="dedcb-132">Tento problém bude vyřešen v budoucí aktualizaci Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="dedcb-132">The issue will be resolved in a future Visual Studio 2017 update.</span></span>  <span data-ttu-id="dedcb-133">.</span><span class="sxs-lookup"><span data-stu-id="dedcb-133">.</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="dedcb-134">Mezipaměť hostovaná v instanci Role na Azure</span><span class="sxs-lookup"><span data-stu-id="dedcb-134">Azure In-Role Cache</span></span> 

- <span data-ttu-id="dedcb-135">Podpora pro mezipaměť hostovaná v instanci Role Azure skončila 30. listopadu 2016.</span><span class="sxs-lookup"><span data-stu-id="dedcb-135">Support for Azure In-Role Cache ended on November 30, 2016.</span></span> <span data-ttu-id="dedcb-136">Další podrobnosti získáte kliknutím na [zde](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="dedcb-136">For more details, click [here](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>




