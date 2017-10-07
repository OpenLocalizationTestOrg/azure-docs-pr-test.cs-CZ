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
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Pokročilá konfigurace pro zapojení univerzálních aplikací pro Windows SDK
> [!div class="op_single_selector"]
> * [Univerzální platforma Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
> 
> 

Tento postup popisuje, jak tooconfigure různé možnosti konfigurace aplikací pro Azure Mobile Engagement Android.

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a>Pokročilá konfigurace
### <a name="disable-automatic-crash-reporting"></a>Zakázat automatické hlášení chyb
Můžete vypnout automatické hlášení hello funkci Engagement generování sestav. Pak, když dojde k neošetřené výjimce Engagement neprovede žádnou akci.

> [!WARNING]
> Pokud jste tuto funkci zakázat a pak v případě neošetřené havárie v aplikaci Engagement neodesílá hello havárií **a** nezavře hello relace a úlohy.
> 
> 

Automatické hlášení toodisable vytváření sestav, přizpůsobit konfiguraci v závislosti na způsob hello deklarujete objekt:

#### <a name="from-engagementconfigurationxml-file"></a>Z `EngagementConfiguration.xml` souboru
Nastavit sestavy zhroutí příliš`false` mezi `<reportCrash>` a `</reportCrash>` značky.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Z `EngagementConfiguration` objektu v době běhu
Nastavit toofalse havárií sestavy pomocí EngagementConfiguration objektu.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Zakázání zasílání zpráv v reálném čase.
Ve výchozím nastavení sestavy služby zapojení hello protokolů v reálném čase. Pokud vaše aplikace často hlásí protokoly, je lepší toobuffer hello protokoly a tooreport je všechny najednou na základě pravidelné časové. Tomu se říká "burst režim".

toodo tedy volat metodu hello:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Hello argument je hodnota **milisekundách**. Vždy, když chcete protokolování v reálném čase hello tooreactivate, volejte metodu hello bez zadání parametru nebo s hodnotou hello 0.

Shluků režimu mírně zvyšuje hello z baterie, ale má vliv na hello monitorování Engagement: všechny dobu trvání relace a úlohy jsou prahová hodnota shluků zaokrouhlené toohello (tedy relací a úlohy kratší, než je prahová hodnota shluků hello nemusí být zobrazeny). Doporučujeme používat práh shluků než 30000 (30s). Uložené protokoly jsou omezené too300 položky. Pokud odesílání je příliš dlouhý, může dojít ke ztrátě některých protokoly.

> [!WARNING]
> Prahová hodnota shluků Hello nemůže být nakonfigurované tooa období menší než jedna druhý. Pokud tak učiníte, hello SDK zobrazí trasování s chybou hello a automaticky obnoví toohello výchozí hodnota nula sekund. Této aktivační události hello SDK tooreport hello zaznamená v reálném čase.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
