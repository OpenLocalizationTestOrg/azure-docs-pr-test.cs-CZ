---
title: aaaMonitor Surface Huby se s Azure Log Analytics | Microsoft Docs
description: "Použijte hello Surface Hub řešení tootrack hello stav Surface Huby a pochopit, jak se právě používají."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 623d30e749cafdd4a34ba0c5b3408164f1b4a95b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-surface-hubs-with-log-analytics-tootrack-their-health"></a>Monitorování Surface Huby s tootrack analýzy protokolů jejich stav

![Surface Hub – symbol](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

Tento článek popisuje, jak můžete použít hello řešení Surface Hub v zařízení Microsoft Surface Hub toomonitor analýzy protokolů s hello Microsoft Operations Management Suite (OMS). Přihlaste se analýza pomáhá sledovat stav hello Surface Huby stejně jako pochopit, jak se právě používají.

Každý Surface Hub má hello instalace agenta Microsoft Monitoring Agent. Jeho prostřednictvím, může odesílat data z vaší tooOMS Surface Hub agenta hello. Soubory protokolu se načítají z Surface Huby a jsou pak odešlou toohello OMS služby. Problémy jako servery je offline, hello kalendáře nesynchronizuje, nebo pokud je účet zařízení hello nelze toolog do Skype se zobrazí v OMS na řídicím panelu hello Surface Hub. Pomocí dat hello hello řídicím panelu můžete identifikovat zařízení, nejsou spuštěné nebo které jsou jiné problémy s a potenciálně použít opravy problémů hello zjištěna.

## <a name="installing-and-configuring-hello-solution"></a>Instalace a konfigurace řešení hello
Použijte následující informace tooinstall hello a nakonfigurujte hello řešení. V pořadí toomanage Surface Huby se z hello Microsoft Operations Management Suite (OMS), budete potřebovat hello následující:

* Platné předplatné příliš[OMS](http://www.microsoft.com/oms).
* [OMS předplatné](https://azure.microsoft.com/pricing/details/log-analytics/) úroveň, která bude podporovat hello číslo zařízení chcete toomonitor. OMS ceny se liší v závislosti na tom, kolik zařízení jsou zaregistrovaná a kolik dat se procesy. Budete muset tootake to v úvahu při plánování zavádění řešení Surface Hub.

Dále budete buď přidejte OMS předplatné tooyour stávající Microsoft Azure předplatné nebo vytvořit nový pracovní prostor přímo přes portál OMS hello. Podrobné pokyny pro použití metody je v [začít pracovat s analýzy protokolů](log-analytics-get-started.md). Až nastavíte hello OMS odběr existují dva způsoby tooenroll zařízení Surface Hub:

* Automaticky prostřednictvím služby Intune
* Ručně pomocí **nastavení** na vašem zařízení Surface Hub.

## <a name="set-up-monitoring"></a>Nastavte monitorování
Můžete monitorovat hello stavu a činností vaší Surface Hub pomocí analýzy protokolů v OMS. Hello Surface Hub v OMS můžete zaregistrovat pomocí Intune nebo místně pomocí **nastavení** na hello Surface Hub.

## <a name="connect-surface-hubs-toooms-through-intune"></a>Připojení Surface Huby tooOMS přes Intune
ID pracovního prostoru a klíč pracovního prostoru pro pracovní prostor OMS hello, která bude spravovat Surface Huby, budete potřebovat hello. Můžete získat z portálu OMS hello.

Intune je produkt společnosti Microsoft, který vám umožní toocentrally spravovat hello OMS nastavení, které jsou použité tooone nebo více zařízení. Postupujte podle těchto kroků tooconfigure zařízení prostřednictvím služby Intune:

1. Přihlaste se tooIntune.
2. Přejděte příliš**nastavení** > **připojené zdroje**.
3. Vytvořte nebo upravte zásadu na základě šablony hello Surface Hub.
4. Přejděte toohello OMS (Statistika provozu Azure) části hello zásad a přidejte hello *ID pracovního prostoru* a *klíč pracovního prostoru* toohello zásad.
5. Uložte zásadu hello.
6. Přidružení zásad hello hello příslušné skupiny zařízení.

   ![Zásady Intune](./media/log-analytics-surface-hubs/intune.png)

Intune pak synchronizuje hello OMS nastavení s hello zařízení v hello cílové skupiny je zaregistrujete v pracovním prostoru OMS.

## <a name="connect-surface-hubs-toooms-using-hello-settings-app"></a>Připojit tooOMS Surface Huby pomocí hello nastavení aplikace
ID pracovního prostoru a klíč pracovního prostoru pro pracovní prostor OMS hello, která bude spravovat Surface Huby, budete potřebovat hello. Můžete získat z portálu OMS hello.

Pokud nepoužíváte Intune toomanage prostředí, můžete zaregistrovat zařízení ručně pomocí **nastavení** na každý Surface Hub:

1. Surface Hub, otevřete **nastavení**.
2. Zadejte pověření správce zařízení hello po zobrazení výzvy.
3. Klikněte na tlačítko **toto zařízení**a v části hello **monitorování**, klikněte na tlačítko **konfigurovat nastavení OMS**.
4. Vyberte **povolit sledování**.
5. V dialogovém okně Nastavení hello OMS, zadejte hello **ID pracovního prostoru** a typ hello **klíč pracovního prostoru**.  
   ![nastavení](./media/log-analytics-surface-hubs/settings.png)
6. Klikněte na tlačítko **OK** toocomplete hello konfigurace.

Zobrazení potvrzení o tom, zda text hello, které OMS konfigurace byla úspěšně použít toohello zařízení. Pokud byl, zobrazí se zpráva s informacemi o tom, že tento agent hello úspěšně připojeno toohello OMS služby. Hello zařízení pak spustí odesílání dat tooOMS, kde můžete zobrazit a pracovat.

## <a name="monitor-surface-hubs"></a>Monitorování Surface Hubů
Monitorování Surface Huby pomocí OMS je mnohem jako monitorování jiných zaregistrovaných zařízení.

1. Přihlaste se toohello portálu OMS.
2. Přejděte toohello Surface Hub řešení pack řídicího panelu.
3. Zobrazí se stav vašeho zařízení.

   ![Surface Hub řídicí panel](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Můžete vytvořit [výstrahy](log-analytics-alerts.md) založené na protokolu hledání existujících nebo vlastních. Pomocí hello hello dat, které OMS shromažďuje z Surface Huby, můžete vyhledat problémy a výstrahy na hello podmínky, které definujete pro vaše zařízení.

## <a name="next-steps"></a>Další kroky
* Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) tooview podrobná data Surface Hub.
* Vytvoření [výstrahy](log-analytics-alerts.md) toonotify případě problémy, ke kterým dochází u Surface Huby.
