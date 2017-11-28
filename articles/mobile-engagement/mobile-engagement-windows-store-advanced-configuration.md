---
title: aaaAdvanced konfigurace pro Windows Universal aplikace Engagement SDK
description: "Rozšířené možnosti konfigurace pro Azure Mobile Engagement s univerzálních aplikací pro Windows"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6d85dd5d-ac07-43ba-bbe4-e91c3a17690b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 23bd05012bc25d438d8d4985a112280bed0292b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="e0eb9-103">Pokročilá konfigurace pro zapojení univerzálních aplikací pro Windows SDK</span><span class="sxs-lookup"><span data-stu-id="e0eb9-103">Advanced Configuration for Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e0eb9-104">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="e0eb9-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="e0eb9-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="e0eb9-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="e0eb9-106">iOS</span><span class="sxs-lookup"><span data-stu-id="e0eb9-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="e0eb9-107">Android</span><span class="sxs-lookup"><span data-stu-id="e0eb9-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
> 
> 

<span data-ttu-id="e0eb9-108">Tento postup popisuje, jak tooconfigure různé možnosti konfigurace aplikací pro Azure Mobile Engagement Android.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-108">This procedure describes how tooconfigure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0eb9-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e0eb9-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a><span data-ttu-id="e0eb9-110">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="e0eb9-110">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="e0eb9-111">Zakázat automatické hlášení chyb</span><span class="sxs-lookup"><span data-stu-id="e0eb9-111">Disable automatic crash reporting</span></span>
<span data-ttu-id="e0eb9-112">Můžete vypnout automatické hlášení hello funkci Engagement generování sestav.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-112">You can disable hello automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="e0eb9-113">Pak, když dojde k neošetřené výjimce Engagement neprovede žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-113">Then, when an unhandled exception occurs, Engagement does nothing.</span></span>

> [!WARNING]
> <span data-ttu-id="e0eb9-114">Pokud jste tuto funkci zakázat a pak v případě neošetřené havárie v aplikaci Engagement neodesílá hello havárií **a** nezavře hello relace a úlohy.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-114">If you disable this feature, then when an unhandled crash occurs in your app, Engagement does not send hello crash **AND** does not close hello session and jobs.</span></span>
> 
> 

<span data-ttu-id="e0eb9-115">Automatické hlášení toodisable vytváření sestav, přizpůsobit konfiguraci v závislosti na způsob hello deklarujete objekt:</span><span class="sxs-lookup"><span data-stu-id="e0eb9-115">toodisable automatic crash reporting, customize your configuration depending on hello way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="e0eb9-116">Z `EngagementConfiguration.xml` souboru</span><span class="sxs-lookup"><span data-stu-id="e0eb9-116">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="e0eb9-117">Nastavit sestavy zhroutí příliš`false` mezi `<reportCrash>` a `</reportCrash>` značky.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-117">Set report crash too`false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="e0eb9-118">Z `EngagementConfiguration` objektu v době běhu</span><span class="sxs-lookup"><span data-stu-id="e0eb9-118">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="e0eb9-119">Nastavit toofalse havárií sestavy pomocí EngagementConfiguration objektu.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-119">Set report crash toofalse using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a><span data-ttu-id="e0eb9-120">Zakázání zasílání zpráv v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-120">Disable real time reporting</span></span>
<span data-ttu-id="e0eb9-121">Ve výchozím nastavení sestavy služby zapojení hello protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-121">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="e0eb9-122">Pokud vaše aplikace často hlásí protokoly, je lepší toobuffer hello protokoly a tooreport je všechny najednou na základě pravidelné časové.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-122">If your application reports logs frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time basis.</span></span> <span data-ttu-id="e0eb9-123">Tomu se říká "burst režim".</span><span class="sxs-lookup"><span data-stu-id="e0eb9-123">This is called “burst mode”.</span></span>

<span data-ttu-id="e0eb9-124">toodo tedy volat metodu hello:</span><span class="sxs-lookup"><span data-stu-id="e0eb9-124">toodo so, call hello method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="e0eb9-125">Hello argument je hodnota **milisekundách**.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-125">hello argument is a value in **milliseconds**.</span></span> <span data-ttu-id="e0eb9-126">Vždy, když chcete protokolování v reálném čase hello tooreactivate, volejte metodu hello bez zadání parametru nebo s hodnotou hello 0.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-126">Whenever you want tooreactivate hello real-time logging, call hello method without any parameter, or with hello 0 value.</span></span>

<span data-ttu-id="e0eb9-127">Shluků režimu mírně zvyšuje hello z baterie, ale má vliv na hello monitorování Engagement: všechny dobu trvání relace a úlohy jsou prahová hodnota shluků zaokrouhlené toohello (tedy relací a úlohy kratší, než je prahová hodnota shluků hello nemusí být zobrazeny).</span><span class="sxs-lookup"><span data-stu-id="e0eb9-127">Burst mode slightly increases hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration are rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="e0eb9-128">Doporučujeme používat práh shluků než 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="e0eb9-128">We recommend using a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="e0eb9-129">Uložené protokoly jsou omezené too300 položky.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-129">Saved logs are limited too300 items.</span></span> <span data-ttu-id="e0eb9-130">Pokud odesílání je příliš dlouhý, může dojít ke ztrátě některých protokoly.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-130">If sending is too long, you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="e0eb9-131">Prahová hodnota shluků Hello nemůže být nakonfigurované tooa období menší než jedna druhý.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-131">hello burst threshold cannot be configured tooa period less than one second.</span></span> <span data-ttu-id="e0eb9-132">Pokud tak učiníte, hello SDK zobrazí trasování s chybou hello a automaticky obnoví toohello výchozí hodnota nula sekund.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-132">If you do so, hello SDK shows a trace with hello error and automatically resets toohello default value, zero seconds.</span></span> <span data-ttu-id="e0eb9-133">Této aktivační události hello SDK tooreport hello zaznamená v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="e0eb9-133">This triggers hello SDK tooreport hello logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
