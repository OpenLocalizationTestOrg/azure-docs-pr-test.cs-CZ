---
title: "Pokročilá konfigurace pro zapojení univerzálních aplikací pro Windows SDK"
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
ms.openlocfilehash: cb9454212c94cf65093219c3d24c71277ede7877
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="282e0-103">Pokročilá konfigurace pro zapojení univerzálních aplikací pro Windows SDK</span><span class="sxs-lookup"><span data-stu-id="282e0-103">Advanced Configuration for Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="282e0-104">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="282e0-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="282e0-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="282e0-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="282e0-106">iOS</span><span class="sxs-lookup"><span data-stu-id="282e0-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="282e0-107">Android</span><span class="sxs-lookup"><span data-stu-id="282e0-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
> 
> 

<span data-ttu-id="282e0-108">Tento postup popisuje, jak nakonfigurovat různé možnosti konfigurace aplikací pro Azure Mobile Engagement Android.</span><span class="sxs-lookup"><span data-stu-id="282e0-108">This procedure describes how to configure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="282e0-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="282e0-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a><span data-ttu-id="282e0-110">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="282e0-110">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="282e0-111">Zakázat automatické hlášení chyb</span><span class="sxs-lookup"><span data-stu-id="282e0-111">Disable automatic crash reporting</span></span>
<span data-ttu-id="282e0-112">Můžete vypnout automatické hlášení funkci Engagement generování sestav.</span><span class="sxs-lookup"><span data-stu-id="282e0-112">You can disable the automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="282e0-113">Pak, když dojde k neošetřené výjimce Engagement neprovede žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="282e0-113">Then, when an unhandled exception occurs, Engagement does nothing.</span></span>

> [!WARNING]
> <span data-ttu-id="282e0-114">Pokud jste tuto funkci zakázat a pak v případě neošetřené havárie v aplikaci Engagement neodesílá havárii **a** nezavře relace a úlohy.</span><span class="sxs-lookup"><span data-stu-id="282e0-114">If you disable this feature, then when an unhandled crash occurs in your app, Engagement does not send the crash **AND** does not close the session and jobs.</span></span>
> 
> 

<span data-ttu-id="282e0-115">Chcete-li zakázat automatické hlášení chyb, přizpůsobit konfiguraci v závislosti na způsobu, jakým jste ji deklarovaný:</span><span class="sxs-lookup"><span data-stu-id="282e0-115">To disable automatic crash reporting, customize your configuration depending on the way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="282e0-116">Z `EngagementConfiguration.xml` souboru</span><span class="sxs-lookup"><span data-stu-id="282e0-116">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="282e0-117">Nastavte Sestavy havárií na `false` mezi `<reportCrash>` a `</reportCrash>` značky.</span><span class="sxs-lookup"><span data-stu-id="282e0-117">Set report crash to `false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="282e0-118">Z `EngagementConfiguration` objektu v době běhu</span><span class="sxs-lookup"><span data-stu-id="282e0-118">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="282e0-119">Nastavit na hodnotu false pomocí objektu EngagementConfiguration sestavy havárií.</span><span class="sxs-lookup"><span data-stu-id="282e0-119">Set report crash to false using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a><span data-ttu-id="282e0-120">Zakázání zasílání zpráv v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="282e0-120">Disable real time reporting</span></span>
<span data-ttu-id="282e0-121">Ve výchozím nastavení sestavy služby zapojení protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="282e0-121">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="282e0-122">Pokud vaše aplikace často hlásí protokoly, je lepší ukládat do vyrovnávací paměti v protokolech a informuje všechny najednou na základě pravidelné časové.</span><span class="sxs-lookup"><span data-stu-id="282e0-122">If your application reports logs frequently, it is better to buffer the logs and to report them all at once on a regular time basis.</span></span> <span data-ttu-id="282e0-123">Tomu se říká "burst režim".</span><span class="sxs-lookup"><span data-stu-id="282e0-123">This is called “burst mode”.</span></span>

<span data-ttu-id="282e0-124">Chcete-li tak učinit, zavolejte metodu:</span><span class="sxs-lookup"><span data-stu-id="282e0-124">To do so, call the method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="282e0-125">Argument je hodnota **milisekundách**.</span><span class="sxs-lookup"><span data-stu-id="282e0-125">The argument is a value in **milliseconds**.</span></span> <span data-ttu-id="282e0-126">Vždy, když chcete znovu aktivovat protokolování v reálném čase, volejte metodu bez zadání parametru nebo s hodnotou 0.</span><span class="sxs-lookup"><span data-stu-id="282e0-126">Whenever you want to reactivate the real-time logging, call the method without any parameter, or with the 0 value.</span></span>

<span data-ttu-id="282e0-127">Režim shluků mírně zvyšuje výdrž baterie, ale má vliv na zapojení monitorování: všechny dobu trvání relace a úlohy jsou zaokrouhleny na shluků prahovou hodnotu (tedy relací a úlohy kratší, než je prahová hodnota shluků nemusí být zobrazeny).</span><span class="sxs-lookup"><span data-stu-id="282e0-127">Burst mode slightly increases the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration are rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="282e0-128">Doporučujeme používat práh shluků než 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="282e0-128">We recommend using a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="282e0-129">Uložené protokoly jsou omezeny na 300 položek.</span><span class="sxs-lookup"><span data-stu-id="282e0-129">Saved logs are limited to 300 items.</span></span> <span data-ttu-id="282e0-130">Pokud odesílání je příliš dlouhý, může dojít ke ztrátě některých protokoly.</span><span class="sxs-lookup"><span data-stu-id="282e0-130">If sending is too long, you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="282e0-131">Prahová hodnota shluků nelze nakonfigurovat na dobu menší než jedna sekunda.</span><span class="sxs-lookup"><span data-stu-id="282e0-131">The burst threshold cannot be configured to a period less than one second.</span></span> <span data-ttu-id="282e0-132">Pokud tak učiníte, sada SDK ukazuje trasování došlo k chybě a automaticky obnoví na výchozí hodnotu nula sekund.</span><span class="sxs-lookup"><span data-stu-id="282e0-132">If you do so, the SDK shows a trace with the error and automatically resets to the default value, zero seconds.</span></span> <span data-ttu-id="282e0-133">Tím se spustí SDK. hlásí protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="282e0-133">This triggers the SDK to report the logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
