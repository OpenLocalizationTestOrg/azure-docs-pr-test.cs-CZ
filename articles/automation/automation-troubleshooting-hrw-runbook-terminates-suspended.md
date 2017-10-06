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
ms.openlocfilehash: 513a90d144e7ade9c21cd7f3b718578989702c25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hybrid Runbook Worker: Úloha runbooku se ukončí se stavem Pozastaveno
## <a name="summary"></a>Souhrn
Vaše sada runbook je pozastaveno krátce po pokusu o tooexecute ho třikrát. Existují podmínky, které mohou přerušit hello runbook úspěšnému dokončení a hello související chybové zprávě se neobjeví žádné další údaje, která udává, proč. Tento článek obsahuje pokyny k odstraňování potíží pro problémy související toohello hybridní pracovní proces Runbooku runbook provádění selhání.

Pokud není Azure problém řešený v tomto článku, navštivte hello fóra Azure na [MSDN a hello Stack Overflow](https://azure.microsoft.com/support/forums/). Problém můžete zveřejnit na tyto fóra nebo příliš[ @AzureSupport na Twitteru](https://twitter.com/AzureSupport). Navíc můžete soubor žádost o podporu Azure tak, že vyberete **získat podporu** na hello [podporu Azure](https://azure.microsoft.com/support/options/) lokality.

## <a name="symptom"></a>Příznaky
Spuštění sady Runbook selže a je vrácena chyba hello "hello akce úlohy 'Aktivovat' nelze spustit, protože hello proces byl neočekávaně zastaven. Akce úlohy Hello došlo k pokusu o 3krát."

## <a name="cause"></a>Příčina
Existuje několik možných příčin hello došlo k chybě: 

1. hybridní pracovní proces Hello je za proxy nebo brány firewall
2. Hello počítače hello hybridní pracovní proces běží na má menší než minimální hardwarové hello [požadavky](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
3. sady runbook Hello nemůže ověřit pomocí místních prostředků

## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Příčina 1: Hybridní pracovní proces Runbooku je za proxy nebo brány firewall
hello Hello počítače, které hybridní pracovní proces Runbooku běží na je za serverem brány firewall nebo proxy server a odchozí síťový přístup nesmí být povoleny a správně nakonfigurován.

### <a name="solution"></a>Řešení
Ověřte hello počítači odchozí přístup too*.cloudapp .net na porty 443, 9354 a 30000 30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>2 příčina: Počítač má menší než minimální požadavky na hardware
Počítače se systémem hello hybridní pracovní proces Runbooku by měl splňovat minimální požadavky na hardware hello před označením ho toohost tuto funkci. V závislosti na využití prostředků hello jiné procesy na pozadí a kolizí způsobené sady runbook během provádění, jinak, hello počítač se stane přetížen a způsobit zpoždění úlohy sady runbook nebo vypršení časových limitů. 

### <a name="solution"></a>Řešení
Nejdřív ověřte počítač hello určené toorun hello hybridní pracovní proces Runbooku funkce splňuje minimální hardwarové požadavky hello.  Pokud ano, sledujte všechny korelace mezi hello výkon procesů Hybrid Runbook Worker a Windows toodetermine využití procesoru a paměti.  Pokud je paměť nebo zatížení procesoru, to může znamenat tooupgrade nutné hello nebo přidáním dalších procesorů nebo paměti tooaddress zvýšení hello problémové místo prostředků a hello chybu vyřešit. Nebo vyberte jiný výpočtový prostředek, který může podporovat hello minimální požadavky a škálování při vytížení indikovat, že se o zvýšení nezbytné.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Příčina 3: Sady Runbook se nemohou ověřovat se místní prostředky
### <a name="solution"></a>Řešení
Zkontrolujte hello **Microsoft SMA** v protokolu událostí je odpovídající událost s popis *Win32 proces skončil s kódem [4294967295]*.  Hello příčinu této chyby je nebyly nakonfigurované ověřování ve vašich sadách runbook nebo zadali hello spustit jako pověření pro skupinu hybridních pracovních procesů hello.  Zkontrolujte [oprávnění sady Runbook](automation-hybrid-runbook-worker.md#runbook-permissions) tooconfirm jste správně nakonfigurovali ověřování pro vaše sady runbook.  

