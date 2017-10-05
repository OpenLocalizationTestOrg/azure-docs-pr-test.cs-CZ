---
title: "Hybridní pracovní proces Runbooku: Ukončí úlohy runbooku se stavem pozastaveno | Microsoft Docs"
description: "Příznaky příčiny a řešení ukončení úlohy Hybrid Runbook Worker došlo k chybě."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 02c6606e-8924-4328-a196-45630c2255e9
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/17/2016
ms.author: magoedte
ms.openlocfilehash: 7c6365b729d73f1c5b9bc57952b1723255d9e9f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hybrid Runbook Worker: Úloha runbooku se ukončí se stavem Pozastaveno
## <a name="summary"></a>Souhrn
Vaše sada runbook je pozastaven krátce po pokusu o spuštění ho třikrát. Existují podmínky, které mohou přerušit sady runbook nelze úspěšně dokončit a související chybová zpráva neobsahuje žádné další údaje, která udává, proč. Tento článek obsahuje pokyny k odstraňování potíží pro problémy související s selhání spuštění sady runbook hybridní pracovní proces Runbooku.

Pokud není Azure problém řešený v tomto článku, navštivte fórech Azure na [MSDN a Stack Overflow](https://azure.microsoft.com/support/forums/). Problém můžete účtovat na tyto fóra nebo na [ @AzureSupport na Twitteru](https://twitter.com/AzureSupport). Navíc můžete soubor žádost o podporu Azure tak, že vyberete **získat podporu** na [podporu Azure](https://azure.microsoft.com/support/options/) lokality.

## <a name="symptom"></a>Příznaky
Spuštění sady Runbook se nezdaří a Vrácená chyba je "úlohy akci, kterou 'Aktivovat' nelze spustit, protože proces se neočekávaně zastavil. Akce úlohy byl proveden pokus o 3krát."

## <a name="cause"></a>Příčina
Existuje několik možných příčin chyby: 

1. Hybridní pracovní proces je za proxy nebo brány firewall
2. Hybridní pracovní proces běží na počítač má menší než minimální hardwarové [požadavky](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
3. Sady runbook se nemohou ověřovat se místní prostředky

## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Příčina 1: Hybridní pracovní proces Runbooku je za proxy nebo brány firewall
Počítač, na kterém běží hybridní pracovní proces Runbooku na je za serverem brány firewall nebo proxy server a odchozí síťový přístup nesmí být povoleny a správně nakonfigurován.

### <a name="solution"></a>Řešení
Ověřte má počítač odchozí přístup k *. cloudapp.net na porty 443, 9354 a 30000 30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>2 příčina: Počítač má menší než minimální požadavky na hardware
Počítače se systémem hybridní pracovní proces Runbooku by měl splňovat minimální požadavky na hardware před označením ho k hostování této funkce. V závislosti na využití prostředků jiné procesy na pozadí a kolizí způsobené sady runbook během provádění, jinak hodnota počítače se stane přetížen a způsobit zpoždění úlohy sady runbook nebo vypršení časových limitů. 

### <a name="solution"></a>Řešení
Nejdřív ověřte, zda počítač určený pro spouštění funkce Hybrid Runbook Worker splňuje minimální požadavky na hardware.  Pokud ano, sledujte využití procesoru a paměti k určení všech korelace mezi výkon procesů Hybrid Runbook Worker a Windows.  Pokud je paměť nebo zatížení procesoru, může to znamenat potřeba upgradovat nebo přidáním dalších procesorů, případně zvyšte paměti adres problémové místo prostředků a opravte případné chyby. Nebo vyberte jiný výpočtový prostředek, který může podporovat minimální požadavky a škálování při vytížení indikovat, že se o zvýšení nezbytné.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Příčina 3: Sady Runbook se nemohou ověřovat se místní prostředky
### <a name="solution"></a>Řešení
Zkontrolujte **Microsoft SMA** v protokolu událostí je odpovídající událost s popis *Win32 proces skončil s kódem [4294967295]*.  Příčina této chyby je nebyly nakonfigurované ověřování ve vašich sadách runbook nebo zadaná pověření spustit jako pro skupinu hybridních pracovních procesů.  Zkontrolujte [oprávnění sady Runbook](automation-hybrid-runbook-worker.md#runbook-permissions) potvrďte jste správně nakonfigurovali ověřování pro vaše sady runbook.  

