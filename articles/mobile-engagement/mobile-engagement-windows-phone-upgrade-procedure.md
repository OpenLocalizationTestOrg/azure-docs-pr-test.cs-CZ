---
title: aaaWindows Phone Silverlight SDK Upgrade procedury
description: Windows Phone Silverlight SDK postupy upgradu pro Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 87130026-9759-4659-9184-788a3627a165
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: d72e7b8a59ef2c0a95b22efbf1e5257271399ddc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a><span data-ttu-id="4983a-103">Windows Phone Silverlight SDK postupy upgradu</span><span class="sxs-lookup"><span data-stu-id="4983a-103">Windows Phone Silverlight SDK Upgrade Procedures</span></span>
<span data-ttu-id="4983a-104">Pokud již máte integrovanou starší verze naše sady SDK do své aplikace, musíte tooconsider hello následující body při upgradu hello SDK.</span><span class="sxs-lookup"><span data-stu-id="4983a-104">If you already have integrated an older version of our SDK into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="4983a-105">Toofollow může mít několik postupů, pokud provedena několik verzí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="4983a-105">You may have toofollow several procedures if you missed several versions of hello SDK.</span></span> <span data-ttu-id="4983a-106">Například pokud migrujete z 0.10.1 too0.11.0 máte toofirst postupujte podle hello "z 0.9.0 too0.10.1" postup pak hello "z 0.10.1 too0.11.0" postup.</span><span class="sxs-lookup"><span data-stu-id="4983a-106">For example if you migrate from 0.10.1 too0.11.0 you have toofirst follow hello "from 0.9.0 too0.10.1" procedure then hello "from 0.10.1 too0.11.0" procedure.</span></span>

## <a name="from-200-too330"></a><span data-ttu-id="4983a-107">Z 2.0.0 too3.3.0</span><span class="sxs-lookup"><span data-stu-id="4983a-107">From 2.0.0 too3.3.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="4983a-108">Protokolů testování</span><span class="sxs-lookup"><span data-stu-id="4983a-108">Test logs</span></span>
<span data-ttu-id="4983a-109">Protokoly konzoly vyprodukované hello SDK teď může být povolena nebo zakázána nebo filtrovat.</span><span class="sxs-lookup"><span data-stu-id="4983a-109">Console logs produced by hello SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="4983a-110">toocustomize se aktualizovat hello vlastnost `EngagementAgent.Instance.TestLogEnabled` tooone hello hodnota dostupná z hello `EngagementTestLogLevel` výčtu, například:</span><span class="sxs-lookup"><span data-stu-id="4983a-110">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

## <a name="from-111-too200"></a><span data-ttu-id="4983a-111">Z 1.1.1 too2.0.0</span><span class="sxs-lookup"><span data-stu-id="4983a-111">From 1.1.1 too2.0.0</span></span>
<span data-ttu-id="4983a-112">Hello následující text popisuje, jak toomigrate integraci sady SDK z hello Capptain služby nabízených Capptain SAS do aplikace používá technologii Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="4983a-112">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="4983a-113">Capptain a Mobile Engagement nejsou hello stejné služby a postup hello níže uvedené jenom označuje, jak toomigrate hello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4983a-113">Capptain and Mobile Engagement are not hello same services and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="4983a-114">Migrace hello SDK v aplikaci hello není migrace dat ze sady Mobile Engagement pro toohello hello Capptain servery</span><span class="sxs-lookup"><span data-stu-id="4983a-114">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="4983a-115">Pokud provádíte migraci ze starší verze, prosím nejdřív najdete hello Capptain webu toomigrate too1.1.1 pak použít následující postup hello</span><span class="sxs-lookup"><span data-stu-id="4983a-115">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too1.1.1 first then apply hello following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="4983a-116">Balíček Nuget</span><span class="sxs-lookup"><span data-stu-id="4983a-116">Nuget package</span></span>
<span data-ttu-id="4983a-117">Nahraďte **Capptain.WindowsPhone** podle **MicrosoftAzure.MobileEngagement** balíček Nuget.</span><span class="sxs-lookup"><span data-stu-id="4983a-117">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="4983a-118">Použití Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="4983a-118">Applying Mobile Engagement</span></span>
<span data-ttu-id="4983a-119">Hello SDK používá termín hello `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="4983a-119">hello SDK uses hello term `Engagement`.</span></span> <span data-ttu-id="4983a-120">Je třeba tooupdate vašeho projektu toomatch tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="4983a-120">You need tooupdate your project toomatch this change.</span></span>

<span data-ttu-id="4983a-121">Je nutné toouninstall váš aktuální Capptain balíček nuget.</span><span class="sxs-lookup"><span data-stu-id="4983a-121">You need toouninstall your current Capptain nuget package.</span></span> <span data-ttu-id="4983a-122">Zvažte, se odeberou všechny změny ve složce Capptain prostředky.</span><span class="sxs-lookup"><span data-stu-id="4983a-122">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="4983a-123">Pokud chcete, aby tookeep tyto soubory pak proveďte jejich kopii.</span><span class="sxs-lookup"><span data-stu-id="4983a-123">If you want tookeep those files then make a copy of them.</span></span>

<span data-ttu-id="4983a-124">Potom nainstalujte balíček nuget Microsoft Azure Engagement nové hello na projektu.</span><span class="sxs-lookup"><span data-stu-id="4983a-124">After that, install hello new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="4983a-125">Můžete najít přímo na [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span><span class="sxs-lookup"><span data-stu-id="4983a-125">You can find it directly on [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span></span> <span data-ttu-id="4983a-126">Tato akce nahradí všechny soubory prostředky používané Engagement a přidá nové tooyour Engagement DLL hello projektu odkazy.</span><span class="sxs-lookup"><span data-stu-id="4983a-126">This action replaces all resources files used by Engagement and adds hello new Engagement DLL tooyour project References.</span></span>

<span data-ttu-id="4983a-127">Tooclean máte odstraněním Capptain DLL odkazy odkazy projektu.</span><span class="sxs-lookup"><span data-stu-id="4983a-127">You have tooclean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="4983a-128">Pokud to neuděláte, budou v konfliktu hello verzi Capptain a dojde k chybám.</span><span class="sxs-lookup"><span data-stu-id="4983a-128">If you do not make this, hello version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="4983a-129">Pokud jste upravili Capptain prostředky, zkopírujte původní soubory obsahu a vložit do nové soubory Engagement hello.</span><span class="sxs-lookup"><span data-stu-id="4983a-129">If you have customized Capptain resources, copy your old files content and paste them in hello new Engagement files.</span></span> <span data-ttu-id="4983a-130">Upozorňujeme, že soubory xaml a cs obsahují toobe aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="4983a-130">Please note that both xaml and cs files have toobe updated.</span></span>

<span data-ttu-id="4983a-131">Po dokončení těchto kroků stačí tooreplace staré odkazy Capptain podle hello nové Engagement odkazy.</span><span class="sxs-lookup"><span data-stu-id="4983a-131">When those steps are completed you only have tooreplace old Capptain references by hello new Engagement references.</span></span>

1. <span data-ttu-id="4983a-132">Všechny obory názvů Capptain mít toobe aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="4983a-132">All Capptain namespaces have toobe updated.</span></span>
   
    <span data-ttu-id="4983a-133">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="4983a-133">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="4983a-134">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="4983a-134">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="4983a-135">Všechny třídy Capptain, které obsahují "Capptain" by měly obsahovat "Engagement".</span><span class="sxs-lookup"><span data-stu-id="4983a-135">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="4983a-136">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="4983a-136">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="4983a-137">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="4983a-137">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="4983a-138">Pro soubory xaml Capptain obor názvů a atributy také změnit.</span><span class="sxs-lookup"><span data-stu-id="4983a-138">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="4983a-139">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="4983a-139">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="4983a-140">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="4983a-140">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="4983a-141">Pro hello jiné prostředky, jako je Capptain obrázky, Všimněte si, že také byly přejmenovat toouse "Engagement".</span><span class="sxs-lookup"><span data-stu-id="4983a-141">For hello other resources like Capptain pictures, please note that they also have been renamed toouse "Engagement".</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="4983a-142">ID aplikace nebo klíč SDK</span><span class="sxs-lookup"><span data-stu-id="4983a-142">Application ID / SDK Key</span></span>
<span data-ttu-id="4983a-143">Zapojení používá připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="4983a-143">Engagement uses a connection string.</span></span> <span data-ttu-id="4983a-144">Nemáte toospecify ID aplikací a klíčem SDK s Mobile Engagement, stačí toospecify připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="4983a-144">You don't have toospecify an application ID and an SDK key with Mobile Engagement, you only have toospecify a connection string.</span></span> <span data-ttu-id="4983a-145">Můžete ho nastavit tak v souboru EngagementConfiguration.</span><span class="sxs-lookup"><span data-stu-id="4983a-145">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="4983a-146">Hello Engagement konfigurace může být nastavena v vaší `Resources\EngagementConfiguration.xml` souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="4983a-146">hello Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="4983a-147">Upravte tento soubor toospecify:</span><span class="sxs-lookup"><span data-stu-id="4983a-147">Edit this file toospecify:</span></span>

* <span data-ttu-id="4983a-148">Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="4983a-148">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="4983a-149">Pokud chcete, aby ho za běhu místo toho můžete volat hello následující toospecify metoda před inicializací agenta hello Engagement:</span><span class="sxs-lookup"><span data-stu-id="4983a-149">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

<span data-ttu-id="4983a-150">Hello připojovací řetězec pro vaši aplikaci se zobrazí v hello portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="4983a-150">hello connection string for your application is displayed in hello Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="4983a-151">Změna názvu položky</span><span class="sxs-lookup"><span data-stu-id="4983a-151">Items name change</span></span>
<span data-ttu-id="4983a-152">Všechny položky s názvem *capptain* byly pojmenovány *engagement*.</span><span class="sxs-lookup"><span data-stu-id="4983a-152">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="4983a-153">Podobně jako pro *Capptain* příliš*Engagement*.</span><span class="sxs-lookup"><span data-stu-id="4983a-153">Similarly for *Capptain* too*Engagement*.</span></span>

<span data-ttu-id="4983a-154">Příklady běžně používané Capptain položek:</span><span class="sxs-lookup"><span data-stu-id="4983a-154">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="4983a-155">CapptainConfiguration nyní s názvem EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="4983a-155">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="4983a-156">Nyní s názvem EngagementAgent CapptainAgent</span><span class="sxs-lookup"><span data-stu-id="4983a-156">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="4983a-157">Nyní s názvem EngagementReach CapptainReach</span><span class="sxs-lookup"><span data-stu-id="4983a-157">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="4983a-158">Nyní s názvem EngagementHttpConfig CapptainHttpConfig</span><span class="sxs-lookup"><span data-stu-id="4983a-158">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="4983a-159">Nyní s názvem GetEngagementPageName GetCapptainPageName</span><span class="sxs-lookup"><span data-stu-id="4983a-159">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="4983a-160">Všimněte si, že přejmenovat také ovlivňuje přepsat metody.</span><span class="sxs-lookup"><span data-stu-id="4983a-160">Note that rename also affects overridden methods.</span></span>

