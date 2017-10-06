---
title: "aaaSecure vaše Internet věcí (IoT) v Azure | Microsoft Docs"
description: " Azure pro internet věcí (IoT) services nabízí celou řadu funkcí. Tento článek vám pomůže pochopit jak toosecure řešení IoT v Azure. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 1473c8dd-8669-48fb-86db-b3c50e2eaf59
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: b6cb2ea1c1facada854fb52c55066f34a8289e47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-overview"></a>Přehled zabezpečení Internetu věcí
Azure pro internet věcí (IoT) services nabízí celou řadu funkcí. Tyto služby na úrovni řešení pro velké firmy umožňují:

* sběr dat ze zařízení
* analýzu datových proudů za provozu
* ukládání objemných datových sad a dotazy na ně
* vizualizaci historických dat i dat v reálném čase
* integraci se systémy administrativní podpory (back-office)

Tyto funkce Azure IoT Suite balíčky současně toodeliver více Azure služby s vlastními rozšířeními jako předkonfigurovaná řešení. Tato předkonfigurovaná řešení jsou základní implementací běžných vzorů řešení IoT, které pomáhají tooreduce hello čas trvat toodeliver řešení IoT. Hello IoT software development Kit můžete přizpůsobit a rozšířit těchto řešení toomeet vlastní požadavky. Tato řešení můžete využít i jako příklady či šablony při vývoji nových řešení IoT.

Hello Azure IoT suite je výkonné řešení pro vaše potřeby IoT. Je však upmost význam, vašich vlastních IoT řešení jsou navržená tak, s důrazem na bezpečnost od začátku hello. Kvůli hello velkému počtu zařízení IoT incidentů, zabezpečení rychle stát událost související s rozšířeným se významné důsledky.

toohelp porozumíte jak toosecure řešení IoT máme hello následující informace.

## <a name="security-architecture"></a>Architektura zabezpečení
Při návrhu systému, je důležité toounderstand hello potenciální hrozby toothat systém a přidejte příslušné obrany podle toho, jak systém hello je navržená tak a navržen. Je důležité, protože pochopení, jak útočník může být schopný toocompromise systému pomáhá, ujistěte se, že jsou splněné od začátku hello odpovídající jejich zmírnění toodesign hello produktu od začátku hello s důrazem na bezpečnost.

Můžete další informace o architektuře IoT zabezpečení načtením [Internet věcí zabezpečení architektura](../iot-suite/iot-security-architecture.md).

Tento článek popisuje hello následující témata:

* [Zabezpečení začíná Model hrozeb](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
* [Zabezpečení v IoT](../iot-suite/iot-security-architecture.md#security-in-iot)
* [Hello modelování hrozeb referenční architektura IoT Azure](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-hello-ground-up"></a>Zabezpečení z hello pozadí
Hello IoT představuje jedinečné zabezpečení, ochrany osobních údajů a dodržování předpisů výzvy toobusinesses po celém světě. Na rozdíl od tradičních internetový technologie, kde tyto problémy základem softwaru a o tom, jak je implementována IoT se vztahuje na co se stane, když internetový hello a fyzické světů hello sloučit. Ochrana řešení IoT vyžaduje zajištění zabezpečené zřizování zařízení, zabezpečené připojení mezi těchto zařízení a hello cloudu a ochranu zabezpečení dat v cloudu hello při zpracování a úložiště. Pracovat se tyto funkce, ale jsou zařízení s omezenými zdroji, geografické rozptýlení nasazení a mnoho zařízení v rámci řešení.

Dozvíte, jak toohandle zabezpečení v těchto oblastech načtením [zabezpečení Internetu věcí z hello pozadí](../iot-suite/securing-iot-ground-up.md).

Hello článek popisuje hello následující témata:

* [Zabezpečení infrastruktury z hello pozadí](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
* [Microsoft Azure – zabezpečené IoT infrastrukturu pro vaši organizaci](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Osvědčené postupy
Zabezpečení infrastruktury IoT vyžaduje přísných strategii zabezpečení do hloubky. Z zabezpečení dat v cloudu hello chrání integritu dat při přenosu přes hello veřejný internet, toosecurely zřizování zařízení, jednotlivé úrovně sestavení větší zajištění zabezpečení hello celé infrastruktury.

Dozvíte se o zabezpečení Internetu věcí osvědčené postupy načtením [osvědčené postupy pro zabezpečení Internetu věcí](../iot-suite/iot-security-best-practices.md).

Hello článek popisuje hello následující témata:

* [Výrobce hardwaru IoT/integrátor](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
* [Vývojář řešení IoT](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
* [Nástroje pro nasazení řešení IoT](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
* [Operátor řešení IoT](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
