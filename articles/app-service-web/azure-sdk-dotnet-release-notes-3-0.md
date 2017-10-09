---
title: "aaaAzure SDK poznámky k verzi rozhraní .NET 3.0 | Microsoft Docs"
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
ms.openlocfilehash: 8970b4c9b64de40dc29a33d69006a00ae5e38a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-30-release-notes"></a><span data-ttu-id="e2c50-103">Poznámky k verzi sady Azure SDK pro rozhraní .NET 3.0</span><span class="sxs-lookup"><span data-stu-id="e2c50-103">Azure SDK for .NET 3.0 release notes</span></span>

<span data-ttu-id="e2c50-104">Toto téma obsahuje poznámky k verzi pro verzi 3.0 hello Azure SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="e2c50-104">This topic includes release notes for version 3.0 of hello Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-30-release-summary"></a><span data-ttu-id="e2c50-105">Azure SDK pro verzi rozhraní .NET 3.0 souhrn</span><span class="sxs-lookup"><span data-stu-id="e2c50-105">Azure SDK for .NET 3.0 release summary</span></span>

<span data-ttu-id="e2c50-106">Datum vydání: 03/07/2017</span><span class="sxs-lookup"><span data-stu-id="e2c50-106">Release date: 03/07/2017</span></span>
 
<span data-ttu-id="e2c50-107">V této verzi byly zavedeny žádné narušující změny toohello Azure SDK 3.0.</span><span class="sxs-lookup"><span data-stu-id="e2c50-107">No breaking changes toohello Azure SDK 3.0 have been introduced in this release.</span></span> <span data-ttu-id="e2c50-108">Je také žádné tooleverage procesu upgradu potřeba tuto sadu SDK s existující projekty cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e2c50-108">There is also no upgrade process needed tooleverage this SDK with existing Cloud Service projects.</span></span> <span data-ttu-id="e2c50-109">použití tooallow hello Azure SDK 3.0 bez nutnosti upgradu procesu, Azure SDK 3.0 nainstaluje toohello stejných adresářích jako sadu Azure SDK 2.9.</span><span class="sxs-lookup"><span data-stu-id="e2c50-109">tooallow use of hello Azure SDK 3.0 without requiring an upgrade process, Azure SDK 3.0 installs toohello same directories as Azure SDK 2.9.</span></span> <span data-ttu-id="e2c50-110">Většina komponent hello nezměnila hello hlavní verzi z 2.9, ale místo toho právě aktualizovat číslo sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="e2c50-110">Most hello components did not change hello major version from 2.9 but instead just updated hello build number.</span></span>

## <a name="visual-studio-2017-rtw"></a><span data-ttu-id="e2c50-111">Visual Studio 2017 RTW</span><span class="sxs-lookup"><span data-stu-id="e2c50-111">Visual Studio 2017 RTW</span></span>

- <span data-ttu-id="e2c50-112">V aplikaci Visual Studio 2017 tato verze hello Azure SDK pro .NET je součástí toohello pracovního vytížení Azure.</span><span class="sxs-lookup"><span data-stu-id="e2c50-112">In Visual Studio 2017, this release of hello Azure SDK for .NET is built in toohello Azure Workload.</span></span> <span data-ttu-id="e2c50-113">Všechny hello nástroje, které potřebujete toodo Azure development budou součástí Visual Studio 2017 do budoucna.</span><span class="sxs-lookup"><span data-stu-id="e2c50-113">All hello tools you need toodo Azure development will be part of Visual Studio 2017 going forward.</span></span> <span data-ttu-id="e2c50-114">Pro Visual Studio 2015 hello SDK bude stále k dispozici prostřednictvím WebPI.</span><span class="sxs-lookup"><span data-stu-id="e2c50-114">For Visual Studio 2015 hello SDK will still be available through WebPI.</span></span> <span data-ttu-id="e2c50-115">Azure SDK pro .NET verze jsme mít nepoužívá pro Visual Studio 2013 teď, když byla vydána Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e2c50-115">We have discontinued Azure SDK for .NET releases for Visual Studio 2013 now that Visual Studio 2017 has been released.</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="e2c50-116">Diagnostika Azure</span><span class="sxs-lookup"><span data-stu-id="e2c50-116">Azure Diagnostics</span></span>

- <span data-ttu-id="e2c50-117">Změněné hello chování tooonly ukládat částečné připojovací řetězec s klíčem hello nahrazuje token pro připojovací řetězec úložiště diagnostiky cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e2c50-117">Changed hello behavior tooonly store a partial connection string with hello key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="e2c50-118">klíč Hello skutečné úložiště jsou teď uložená v složky profilu uživatele hello tak přístup se dá řídit.</span><span class="sxs-lookup"><span data-stu-id="e2c50-118">hello actual storage key is now stored in hello user profile folder so its access can be controlled.</span></span> <span data-ttu-id="e2c50-119">Visual Studio, bude číst klíč úložiště hello ze složky profilu uživatele pro místní ladění a proces publikování.</span><span class="sxs-lookup"><span data-stu-id="e2c50-119">Visual Studio will read hello storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="e2c50-120">V odpovědi toohello změn popsané výše Visual Studio Online team rozšířené hello šablonu úloh nasazení Azure Cloud Services, uživatelé mohou zadat klíč hello úložiště pro nastavení rozšíření diagnostiky při publikování tooAzure v průběžnou integraci a nasazení.</span><span class="sxs-lookup"><span data-stu-id="e2c50-120">In response toohello change described above, Visual Studio Online team enhanced hello Azure Cloud Services deployment task template so users could specify hello storage key for setting diagnostics extension when publishing tooAzure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="e2c50-121">Provedli jsme ji možné toostore zabezpečené připojovací řetězec a tokenizaci pro Azure Diagnostics (WAD), toohelp řešení problémů s konfigurací napříč environements.</span><span class="sxs-lookup"><span data-stu-id="e2c50-121">We’ve made it possible toostore secure connection string and tokenization for Azure Diagnostics (WAD), toohelp you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="e2c50-122">Windows Server 2016 virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="e2c50-122">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="e2c50-123">Visual Studio teď podporuje nasazení cloudové služby tooOS rodiny 5 (Windows Server 2016) virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e2c50-123">Visual Studio now supports deploying Cloud Services tooOS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="e2c50-124">Pro existující cloudové služby, můžete změnit vaše nastavení tootarget hello nové řada operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e2c50-124">For existing cloud services, you can change your settings tootarget hello new OS Family.</span></span> <span data-ttu-id="e2c50-125">Při vytváření nových cloudových služeb, pokud se rozhodnete toocreate hello služby pomocí rozhraní .net 4.6 nebo vyšší, bude použita výchozí služba toouse hello operačního systému rodiny 5.</span><span class="sxs-lookup"><span data-stu-id="e2c50-125">When creating new cloud services, if you choose toocreate hello service using .net 4.6 or higher, it will default hello service toouse OS Family 5.</span></span>  <span data-ttu-id="e2c50-126">Další informace, můžete zkontrolovat hello [skupina hostovaných operačních systémů podporují tabulky](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="e2c50-126">For more information, you can review hello [Guest OS Family support table](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span>

### <a name="known-issues"></a><span data-ttu-id="e2c50-127">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="e2c50-127">Known issues</span></span>

- <span data-ttu-id="e2c50-128">Azure .NET SDK 3.0 zavedla problém při odebírání Visual Studio 2017 v konfiguraci vedle sebe s Visual Studiem 2015.</span><span class="sxs-lookup"><span data-stu-id="e2c50-128">Azure .NET SDK 3.0 introduced an issue when removing Visual Studio 2017 in a side by side configuration with Visual Studio 2015.</span></span>  <span data-ttu-id="e2c50-129">Pokud máte hello Azure SDK pro Visual Studio 2015 nainstalována, hello emulátor úložiště Microsoft Azure a Microsoft Azure výpočetní emulátor budou odebrány, pokud odinstalujete Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e2c50-129">If you have hello Azure SDK installed for Visual Studio 2015, hello Microsoft Azure Storage Emulator and Microsoft Azure Compute Emulator will be removed if you uninstall Visual Studio 2017.</span></span>  <span data-ttu-id="e2c50-130">To způsobí chybu při vytváření a ladění nové projekty cloudových služeb v sadě Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="e2c50-130">This will produce an error when creating and debugging new Cloud services projects in Visual Studio 2015.</span></span> <span data-ttu-id="e2c50-131">V pořadí toofix tento problém, přeinstalujte hello Azure SDK z hello instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="e2c50-131">In order toofix this issue,  reinstall hello Azure SDK from hello Web Platform Installer.</span></span>  <span data-ttu-id="e2c50-132">Hello problém bude vyřešen v budoucí aktualizaci Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e2c50-132">hello issue will be resolved in a future Visual Studio 2017 update.</span></span>  <span data-ttu-id="e2c50-133">.</span><span class="sxs-lookup"><span data-stu-id="e2c50-133">.</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="e2c50-134">Mezipaměť hostovaná v instanci Role na Azure</span><span class="sxs-lookup"><span data-stu-id="e2c50-134">Azure In-Role Cache</span></span> 

- <span data-ttu-id="e2c50-135">Podpora pro mezipaměť hostovaná v instanci Role Azure skončila 30. listopadu 2016.</span><span class="sxs-lookup"><span data-stu-id="e2c50-135">Support for Azure In-Role Cache ended on November 30, 2016.</span></span> <span data-ttu-id="e2c50-136">Další podrobnosti získáte kliknutím na [zde](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="e2c50-136">For more details, click [here](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>




