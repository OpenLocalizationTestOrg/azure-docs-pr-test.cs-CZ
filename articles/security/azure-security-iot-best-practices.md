---
title: "aaaInternet osvědčené postupy zabezpečení věcí | Microsoft Docs"
description: "Hello článek obsahuje seznam kurátorované Microsoft Internet věcí osvědčené postupy zabezpečení a obecná doporučení."
services: security
documentationcenter: na
author: TomShinder
manager: StevenPo
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: yurid
ms.openlocfilehash: 7ee31c912e8ac230ffa5efcd5b4c2b0b0713584f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-best-practices"></a>Internet věcí osvědčené postupy zabezpečení
Zabezpečení Internetu věcí (IoT) infrastruktura hello je důležité podniku pro každý, kdo spojené s řešení IoT. Distribuovaná povaha tato zařízení hello dopad událostí zabezpečení z důvodu hello počet zařízení zahrnutých a hello související toocompromise miliony zařízení IoT je netriviální a může mít rozšířeným dopad.

Z tohoto důvodu musí IoT zabezpečení zabezpečení do hloubky přístup. Data potřebám toobe zabezpečení v cloudu hello a jak se přesune přes privátní a veřejné sítě. Metody potřebovat toobe v místě toosecurely zřídit hello zařízení IoT sami. Jednotlivé úrovně ze zařízení, toonetwork, toocloud back-end vyžaduje silné zabezpečení záruky.

Osvědčené postupy IoT může rozdělené do hello následujícím způsobem:

* Výrobce hardwaru IoT nebo integrátor
* Vývojář řešení IoT
* Nástroje pro nasazení řešení IoT
* Operátor řešení IoT

Tento článek shrnuje [Internetu z věcí osvědčené postupy zabezpečení](../iot-suite/iot-security-best-practices.md). Podrobnější informace naleznete v článku toothat.

## <a name="iot-hardware-manufacturer-or-integrator"></a>Výrobce hardwaru IoT nebo integrátor
Pokud jste IoT výrobce hardwaru nebo integrátor hardwaru, držte se osvědčených postupů hello níže:

* **Obor požadavky na hardware toominimum**: návrh hardwaru hello by měla zahrnovat nutných pro operaci hello hardwaru a nic další minimální funkce. 
* **Ujistěte se, hardwaru manipulovat ověření**: sestavení v mechanismy toodetect fyzické manipulaci hardwaru, jako je například otevírání hello zařízení titulní, odebrání součástí hello zařízení atd. 
* **Sestavení kolem zabezpečený hardware**: Pokud [spotřebu](https://en.wikipedia.org/wiki/Cost_of_goods_sold) povolení, funkce zabezpečení, jako je zabezpečená a šifrovaná úložiště a na základě Trusted Platform Module TPM spouštěcí funkce sestavení.
* **Zabezpečit upgrady**: upgrade firmwaru během životního cyklu zařízení hello je nevyhnutelné.

## <a name="iot-solution-developer"></a>Vývojář řešení IoT
Pokud jste vývojář řešení IoT, držte se osvědčených postupů hello níže:

* **Postupujte podle zabezpečené softwaru vývoj metodika**: vývoj zabezpečené softwaru vyžaduje základů přemýšlíte o zabezpečení z hello zahájení projektu hello všechny hello způsob tooits implementace, testování a nasazení.
* **Vyberte software s otevřeným zdrojem dát pozor**: software s otevřeným zdrojem vám dává příležitost tooquickly vývoj řešení.
* **Integrovat dát pozor**: řadu nedostatků zabezpečení softwaru hello existovat hello hranice knihovny a rozhraní API. 

## <a name="iot-solution-deployer"></a>Nástroje pro nasazení řešení IoT
Pokud jste deployer řešení IoT, držte se osvědčených postupů hello níže:

* **Nasazení hardwaru bezpečně**: IoT nasazení může vyžadovat toobe hardware nasazený v nezabezpečená umístění, například veřejné mezery nebo bez dohledu národní prostředí.
* **Chránit ověřovací klíče**: během nasazování každé zařízení vyžaduje ID zařízení a související ověřovací klíče generované hello cloudové služby. Chránit tyto klíče fyzicky i po nasazení hello. Všechny ohrožené klíč lze toomasquerade škodlivý zařízení jako ze stávajících zařízení.

## <a name="iot-solution-operator"></a>Operátor řešení IoT
Pokud jste operátor řešení IoT, držte se osvědčených postupů hello níže:

* **Udržování systémů až toodate**: Zkontrolujte aktualizované toohello nejnovější verze jsou operační systémy zařízení a všechny ovladače zařízení. 
* **Ochrana proti škodlivé aktivity**: Pokud hello operačního systému povolí, umístěte hello nejnovější antivirový a antimalwarové funkce na každý operační systém zařízení. 
* **Audit často**: auditování IoT infrastrukturu pro zabezpečení týkající se problémů je klíč při odpovědi toosecurity incidenty.
* **Fyzicky ochrana hello IoT infrastruktury**: hello nejhorší zabezpečení útoky na infrastrukturu IoT spustily pomocí toodevices fyzický přístup.
* **Ochranu přihlašovacích údajů cloudu**: cloudové ověřování pověření použitá pro konfiguraci a provozní nasazení služby IoT se pravděpodobně hello nejjednodušší způsob, jak toogain přístup a ohrozit systém IoT. 

