---
title: Windows Phone Silverlight SDK postupy upgradu
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
ms.openlocfilehash: f87f65788075c7f4067e77946e1bcbc8f3709317
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a><span data-ttu-id="f12b4-103">Windows Phone Silverlight SDK postupy upgradu</span><span class="sxs-lookup"><span data-stu-id="f12b4-103">Windows Phone Silverlight SDK Upgrade Procedures</span></span>
<span data-ttu-id="f12b4-104">Pokud již máte integrovanou starší verze naše sady SDK do své aplikace, je nutné zvážit následující body při upgradu sady SDK.</span><span class="sxs-lookup"><span data-stu-id="f12b4-104">If you already have integrated an older version of our SDK into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="f12b4-105">Možná budete muset několik postupy použijte, pokud provedena několik verzí sady SDK.</span><span class="sxs-lookup"><span data-stu-id="f12b4-105">You may have to follow several procedures if you missed several versions of the SDK.</span></span> <span data-ttu-id="f12b4-106">Například pokud migrujete z 0.10.1 0.11.0 budete muset nejdřív postupujte podle pokynů "od 0.9.0 k 0.10.1" pak postupu "od 0.10.1 k 0.11.0".</span><span class="sxs-lookup"><span data-stu-id="f12b4-106">For example if you migrate from 0.10.1 to 0.11.0 you have to first follow the "from 0.9.0 to 0.10.1" procedure then the "from 0.10.1 to 0.11.0" procedure.</span></span>

## <a name="from-200-to-330"></a><span data-ttu-id="f12b4-107">Z 2.0.0 k 3.3.0</span><span class="sxs-lookup"><span data-stu-id="f12b4-107">From 2.0.0 to 3.3.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="f12b4-108">Protokolů testování</span><span class="sxs-lookup"><span data-stu-id="f12b4-108">Test logs</span></span>
<span data-ttu-id="f12b4-109">Protokoly konzoly vyprodukované sady SDK teď může být povolena nebo zakázána nebo filtrovat.</span><span class="sxs-lookup"><span data-stu-id="f12b4-109">Console logs produced by the SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="f12b4-110">Chcete-li přizpůsobit tím, aktualizujte vlastnost `EngagementAgent.Instance.TestLogEnabled` na jednu z hodnota dostupná z `EngagementTestLogLevel` výčtu pro instanci:</span><span class="sxs-lookup"><span data-stu-id="f12b4-110">To customize this, update the property `EngagementAgent.Instance.TestLogEnabled` to one of the value available from the `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

## <a name="from-111-to-200"></a><span data-ttu-id="f12b4-111">Z 1.1.1 k 2.0.0</span><span class="sxs-lookup"><span data-stu-id="f12b4-111">From 1.1.1 to 2.0.0</span></span>
<span data-ttu-id="f12b4-112">Následující část popisuje postup migrace integraci sady SDK z Capptain služby, které do aplikace používá technologii Azure Mobile Engagement nabízí Capptain SAS.</span><span class="sxs-lookup"><span data-stu-id="f12b4-112">The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="f12b4-113">Capptain a Mobile Engagement nejsou stejné služby a postup níže uvedené jenom dozvíte, jak migrovat klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="f12b4-113">Capptain and Mobile Engagement are not the same services and the procedure given below only highlights how to migrate the client app.</span></span> <span data-ttu-id="f12b4-114">Migrace sady SDK v aplikaci není migrovat data ze serverů Capptain na servery Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="f12b4-114">Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="f12b4-115">Pokud provádíte migraci ze starší verze, najdete na webu společnosti Capptain nejdřív přenést 1.1.1 pak použije následující postup</span><span class="sxs-lookup"><span data-stu-id="f12b4-115">If you are migrating from an earlier version, please consult the Capptain web site to migrate to 1.1.1 first then apply the following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="f12b4-116">Balíček Nuget</span><span class="sxs-lookup"><span data-stu-id="f12b4-116">Nuget package</span></span>
<span data-ttu-id="f12b4-117">Nahraďte **Capptain.WindowsPhone** podle **MicrosoftAzure.MobileEngagement** balíček Nuget.</span><span class="sxs-lookup"><span data-stu-id="f12b4-117">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="f12b4-118">Použití Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="f12b4-118">Applying Mobile Engagement</span></span>
<span data-ttu-id="f12b4-119">Sada SDK používá termín `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="f12b4-119">The SDK uses the term `Engagement`.</span></span> <span data-ttu-id="f12b4-120">Je potřeba aktualizovat projekt tak, aby odpovídaly tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="f12b4-120">You need to update your project to match this change.</span></span>

<span data-ttu-id="f12b4-121">Je potřeba odinstalovat váš aktuální Capptain balíček nuget.</span><span class="sxs-lookup"><span data-stu-id="f12b4-121">You need to uninstall your current Capptain nuget package.</span></span> <span data-ttu-id="f12b4-122">Zvažte, se odeberou všechny změny ve složce Capptain prostředky.</span><span class="sxs-lookup"><span data-stu-id="f12b4-122">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="f12b4-123">Pokud chcete, aby tyto soubory pak proveďte jejich kopii.</span><span class="sxs-lookup"><span data-stu-id="f12b4-123">If you want to keep those files then make a copy of them.</span></span>

<span data-ttu-id="f12b4-124">Potom nainstalujte nový balíček nuget Microsoft Azure Engagement na projektu.</span><span class="sxs-lookup"><span data-stu-id="f12b4-124">After that, install the new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="f12b4-125">Můžete najít přímo na [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span><span class="sxs-lookup"><span data-stu-id="f12b4-125">You can find it directly on [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span></span> <span data-ttu-id="f12b4-126">Tato akce nahradí všechny soubory prostředky používané Engagement a přidá nová knihovna DLL Engagement odkazy projektu.</span><span class="sxs-lookup"><span data-stu-id="f12b4-126">This action replaces all resources files used by Engagement and adds the new Engagement DLL to your project References.</span></span>

<span data-ttu-id="f12b4-127">Je nutné vyčistit odkazy projektu odstraněním Capptain DLL odkazy.</span><span class="sxs-lookup"><span data-stu-id="f12b4-127">You have to clean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="f12b4-128">Pokud to neuděláte, budou v konfliktu verze Capptain a dojde k chybám.</span><span class="sxs-lookup"><span data-stu-id="f12b4-128">If you do not make this, the version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="f12b4-129">Pokud jste upravili Capptain prostředky, zkopírujte původní soubory obsahu a vložit do nové soubory zapojení.</span><span class="sxs-lookup"><span data-stu-id="f12b4-129">If you have customized Capptain resources, copy your old files content and paste them in the new Engagement files.</span></span> <span data-ttu-id="f12b4-130">Upozorňujeme, že soubory xaml a cs muset aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f12b4-130">Please note that both xaml and cs files have to be updated.</span></span>

<span data-ttu-id="f12b4-131">Po dokončení těchto kroků stačí nahraďte staré odkazy Capptain pomocí nové odkazy zapojení.</span><span class="sxs-lookup"><span data-stu-id="f12b4-131">When those steps are completed you only have to replace old Capptain references by the new Engagement references.</span></span>

1. <span data-ttu-id="f12b4-132">Všechny obory názvů Capptain muset aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f12b4-132">All Capptain namespaces have to be updated.</span></span>
   
    <span data-ttu-id="f12b4-133">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="f12b4-133">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="f12b4-134">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="f12b4-134">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="f12b4-135">Všechny třídy Capptain, které obsahují "Capptain" by měly obsahovat "Engagement".</span><span class="sxs-lookup"><span data-stu-id="f12b4-135">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="f12b4-136">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="f12b4-136">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="f12b4-137">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="f12b4-137">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="f12b4-138">Pro soubory xaml Capptain obor názvů a atributy také změnit.</span><span class="sxs-lookup"><span data-stu-id="f12b4-138">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="f12b4-139">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="f12b4-139">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="f12b4-140">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="f12b4-140">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="f12b4-141">Pro jiné prostředky jako Capptain obrázky Upozorňujeme, že se také přejmenovaná používat "Engagement".</span><span class="sxs-lookup"><span data-stu-id="f12b4-141">For the other resources like Capptain pictures, please note that they also have been renamed to use "Engagement".</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="f12b4-142">ID aplikace nebo klíč SDK</span><span class="sxs-lookup"><span data-stu-id="f12b4-142">Application ID / SDK Key</span></span>
<span data-ttu-id="f12b4-143">Zapojení používá připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="f12b4-143">Engagement uses a connection string.</span></span> <span data-ttu-id="f12b4-144">Nemáte a zadejte ID aplikace a klíč SDK s Mobile Engagement, stačí zadat připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="f12b4-144">You don't have to specify an application ID and an SDK key with Mobile Engagement, you only have to specify a connection string.</span></span> <span data-ttu-id="f12b4-145">Můžete ho nastavit tak v souboru EngagementConfiguration.</span><span class="sxs-lookup"><span data-stu-id="f12b4-145">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="f12b4-146">Konfigurace Engagement může být nastavena v vaší `Resources\EngagementConfiguration.xml` souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="f12b4-146">The Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="f12b4-147">Upravte tento soubor k určení:</span><span class="sxs-lookup"><span data-stu-id="f12b4-147">Edit this file to specify:</span></span>

* <span data-ttu-id="f12b4-148">Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="f12b4-148">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="f12b4-149">Pokud chcete zadat za běhu, můžete volat metodu před Engagement inicializaci agenta:</span><span class="sxs-lookup"><span data-stu-id="f12b4-149">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization:</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

<span data-ttu-id="f12b4-150">Připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="f12b4-150">The connection string for your application is displayed in the Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="f12b4-151">Změna názvu položky</span><span class="sxs-lookup"><span data-stu-id="f12b4-151">Items name change</span></span>
<span data-ttu-id="f12b4-152">Všechny položky s názvem *capptain* byly pojmenovány *engagement*.</span><span class="sxs-lookup"><span data-stu-id="f12b4-152">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="f12b4-153">Podobně jako pro *Capptain* k *Engagement*.</span><span class="sxs-lookup"><span data-stu-id="f12b4-153">Similarly for *Capptain* to *Engagement*.</span></span>

<span data-ttu-id="f12b4-154">Příklady běžně používané Capptain položek:</span><span class="sxs-lookup"><span data-stu-id="f12b4-154">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="f12b4-155">CapptainConfiguration nyní s názvem EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="f12b4-155">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="f12b4-156">Nyní s názvem EngagementAgent CapptainAgent</span><span class="sxs-lookup"><span data-stu-id="f12b4-156">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="f12b4-157">Nyní s názvem EngagementReach CapptainReach</span><span class="sxs-lookup"><span data-stu-id="f12b4-157">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="f12b4-158">Nyní s názvem EngagementHttpConfig CapptainHttpConfig</span><span class="sxs-lookup"><span data-stu-id="f12b4-158">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="f12b4-159">Nyní s názvem GetEngagementPageName GetCapptainPageName</span><span class="sxs-lookup"><span data-stu-id="f12b4-159">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="f12b4-160">Všimněte si, že přejmenovat také ovlivňuje přepsat metody.</span><span class="sxs-lookup"><span data-stu-id="f12b4-160">Note that rename also affects overridden methods.</span></span>

