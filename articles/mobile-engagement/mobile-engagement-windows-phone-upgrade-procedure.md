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
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK postupy upgradu
Pokud již máte integrovanou starší verze naše sady SDK do své aplikace, musíte tooconsider hello následující body při upgradu hello SDK.

Toofollow může mít několik postupů, pokud provedena několik verzí hello SDK. Například pokud migrujete z 0.10.1 too0.11.0 máte toofirst postupujte podle hello "z 0.9.0 too0.10.1" postup pak hello "z 0.10.1 too0.11.0" postup.

## <a name="from-200-too330"></a>Z 2.0.0 too3.3.0
### <a name="test-logs"></a>Protokolů testování
Protokoly konzoly vyprodukované hello SDK teď může být povolena nebo zakázána nebo filtrovat. toocustomize se aktualizovat hello vlastnost `EngagementAgent.Instance.TestLogEnabled` tooone hello hodnota dostupná z hello `EngagementTestLogLevel` výčtu, například:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

## <a name="from-111-too200"></a>Z 1.1.1 too2.0.0
Hello následující text popisuje, jak toomigrate integraci sady SDK z hello Capptain služby nabízených Capptain SAS do aplikace používá technologii Azure Mobile Engagement. 

> [!IMPORTANT]
> Capptain a Mobile Engagement nejsou hello stejné služby a postup hello níže uvedené jenom označuje, jak toomigrate hello klientskou aplikaci. Migrace hello SDK v aplikaci hello není migrace dat ze sady Mobile Engagement pro toohello hello Capptain servery
> 
> 

Pokud provádíte migraci ze starší verze, prosím nejdřív najdete hello Capptain webu toomigrate too1.1.1 pak použít následující postup hello

### <a name="nuget-package"></a>Balíček Nuget
Nahraďte **Capptain.WindowsPhone** podle **MicrosoftAzure.MobileEngagement** balíček Nuget.

### <a name="applying-mobile-engagement"></a>Použití Mobile Engagement
Hello SDK používá termín hello `Engagement`. Je třeba tooupdate vašeho projektu toomatch tuto změnu.

Je nutné toouninstall váš aktuální Capptain balíček nuget. Zvažte, se odeberou všechny změny ve složce Capptain prostředky. Pokud chcete, aby tookeep tyto soubory pak proveďte jejich kopii.

Potom nainstalujte balíček nuget Microsoft Azure Engagement nové hello na projektu. Můžete najít přímo na [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Tato akce nahradí všechny soubory prostředky používané Engagement a přidá nové tooyour Engagement DLL hello projektu odkazy.

Tooclean máte odstraněním Capptain DLL odkazy odkazy projektu. Pokud to neuděláte, budou v konfliktu hello verzi Capptain a dojde k chybám.

Pokud jste upravili Capptain prostředky, zkopírujte původní soubory obsahu a vložit do nové soubory Engagement hello. Upozorňujeme, že soubory xaml a cs obsahují toobe aktualizovat.

Po dokončení těchto kroků stačí tooreplace staré odkazy Capptain podle hello nové Engagement odkazy.

1. Všechny obory názvů Capptain mít toobe aktualizovat.
   
    Před migrací:
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    Po dokončení migrace:
   
        using Microsoft.Azure.Engagement;
2. Všechny třídy Capptain, které obsahují "Capptain" by měly obsahovat "Engagement".
   
    Před migrací:
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    Po dokončení migrace:
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. Pro soubory xaml Capptain obor názvů a atributy také změnit.
   
    Před migrací:
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    Po dokončení migrace:
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. Pro hello jiné prostředky, jako je Capptain obrázky, Všimněte si, že také byly přejmenovat toouse "Engagement".

### <a name="application-id--sdk-key"></a>ID aplikace nebo klíč SDK
Zapojení používá připojovací řetězec. Nemáte toospecify ID aplikací a klíčem SDK s Mobile Engagement, stačí toospecify připojovací řetězec. Můžete ho nastavit tak v souboru EngagementConfiguration.

Hello Engagement konfigurace může být nastavena v vaší `Resources\EngagementConfiguration.xml` souboru projektu.

Upravte tento soubor toospecify:

* Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.

Pokud chcete, aby ho za běhu místo toho můžete volat hello následující toospecify metoda před inicializací agenta hello Engagement:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

Hello připojovací řetězec pro vaši aplikaci se zobrazí v hello portálu Azure Classic.

### <a name="items-name-change"></a>Změna názvu položky
Všechny položky s názvem *capptain* byly pojmenovány *engagement*. Podobně jako pro *Capptain* příliš*Engagement*.

Příklady běžně používané Capptain položek:

* CapptainConfiguration nyní s názvem EngagementConfiguration
* Nyní s názvem EngagementAgent CapptainAgent
* Nyní s názvem EngagementReach CapptainReach
* Nyní s názvem EngagementHttpConfig CapptainHttpConfig
* Nyní s názvem GetEngagementPageName GetCapptainPageName

Všimněte si, že přejmenovat také ovlivňuje přepsat metody.

