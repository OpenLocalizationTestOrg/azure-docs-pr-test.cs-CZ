---
title: "aaaManage výstrah zabezpečení v Azure Security Center | Microsoft Docs"
description: "Tento dokument pomáhá vám toouse Azure Security Center možnosti toomanage a reagovat toosecurity výstrahy."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: yurid
ms.openlocfilehash: f1cb7e4770776827b75ed15893914678c1f44216
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-and-responding-toosecurity-alerts-in-azure-security-center"></a>Správa a zpracování výstrah toosecurity v Azure Security Center
Tento dokument vám pomůže používat Azure Security Center toomanage a reagovat toosecurity výstrahy.

> [!NOTE]
> detekce tooenable rozšířené, upgradu tooAzure Standard Center zabezpečení. K dispozici je bezplatná 60denní zkušební verze. tooupgrade, vyberte cenová úroveň v hello [zásady zabezpečení](security-center-policies.md). V tématu [ceny Azure Security Center](security-center-pricing.md) toolearn Další.
>
>

## <a name="what-are-security-alerts"></a>Co jsou výstrahy zabezpečení?
Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vašich prostředků Azure, sítě hello připojené partnerských řešení, jako jsou brány firewall a endpoint protection řešení, toodetect skutečné hrozby a snížil počet falešných poplachů. Seznam upřednostňovaných výstrah zabezpečení se zobrazí v Centru zabezpečení společně s hello informace, které potřebujete tooquickly prozkoumat hello problému a doporučení, jak tooremediate útoku.


> [!NOTE]
> Další informace o tom, jak detekce služby Security Center pracuje, najdete v článku [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md).
>
>

## <a name="managing-security-alerts"></a>Správa výstrah zabezpečení
Aktuální výstrahy můžete zkontrolovat prohlížením hello **výstrahy zabezpečení** dlaždici. Otevřete portál Azure a postupujte podle kroků hello toosee další podrobnosti o každé výstraze:

1. Na řídicím panelu Security Center hello, uvidíte hello **výstrahy zabezpečení** dlaždici.

    ![Dlaždice Výstrahy zabezpečení ve službě Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2. Klikněte na tlačítko hello dlaždice tooopen hello **výstrahy zabezpečení** okno, které obsahuje další podrobnosti o hello výstrahy, jak je uvedeno níže.

   ![okno výstrahy zabezpečení Hello v Centru zabezpečení](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

V dolní části tohoto okna hello jsou hello podrobnosti pro každou výstrahu. toosort, klikněte na tlačítko hello sloupec, který chcete toosort podle. Hello definice pro každý sloupec je uvedená dále:

* **Popis**: stručné vysvětlení výstrahy hello.
* **Count** (Počet): Seznam všech výstrah tohoto konkrétního typu, které byly zjištěny v určitý den.
* **Zjištěné podle**: hello služba, která je zodpovědná za aktivaci výstrahy hello.
* **Datum**: hello datum tuto událost hello došlo k chybě.
* **Stav**: hello aktuální stav výstrahy. Existují dva typy stavů:
  * **Aktivní**: byla zjištěna výstraha zabezpečení hello.
* **Závažnost**: úroveň závažnosti hello, což může být vysoká, střední nebo Nízká.

### <a name="filtering-alerts"></a>Filtrování výstrah
Výstrahy můžete filtrovat podle data, stavu nebo závažnosti. Filtrování výstrah může být užitečná pro scénáře, kde je nutné toonarrow hello obor zobrazených výstrah zabezpečení. Například můžete má tooaddress výstrahy zabezpečení, které nastaly v hello posledních 24 hodin, protože zjišťujete případný průnik systému hello.

1. Klikněte na tlačítko **filtru** na hello **výstrahy zabezpečení** okno. Hello **filtru** otevře se okno a vyberte hodnoty data, stav a závažnost hello chcete toosee.

    ![Filtrování výstrah ve službě Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-toosecurity-alerts"></a>Odpověď toosecurity výstrahy
Vyberte toolearn výstrahy zabezpečení informace o hello událostí, který aktivoval výstrahu hello a co, pokud existuje, kroky je nutné tootake tooremediate útoku. Výstrahy zabezpečení jsou seskupené podle typu a data. Kliknutím na výstrahu zabezpečení se otevře okno obsahující seznam hello seskupených výstrah.

![Odpověď toosecurity výstrah v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

V takovém případě hello vygenerované výstrahy naleznete toosuspicious aktivity protokolu RDP (Remote Desktop). Hello první sloupec zobrazuje, které prostředky byly napadeny; Hello druhý zobrazuje kolikrát byl napaden hello prostředků; Hello třetí zobrazuje čas hello hello útoku; Hello čtvrtý zobrazuje stav výstrahy hello; a hello páté zobrazuje závažnost útoku hello hello. Po zkontrolování těchto informací klikněte na tlačítko hello prostředek, který byl napaden a otevře se nové okno.

![Doporučení pro výstrahy co toodo o zabezpečení v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

V hello **popis** tohoto okna najdete další podrobnosti o této události. Tyto další podrobnosti nabízejí získání náhledu na jaké spouštěná hello zabezpečení výstrah, hello cílový prostředek, pokud příslušné hello zdrojovou IP adresu a doporučení, jak tooremediate.  V některých případech hello Zdrojová IP adresa prázdná (není k dispozici) protože ne všechny protokoly událostí zabezpečení systému Windows zahrnovat hello IP adresu.

Hello náprava navrhovaná službou Security Center se bude lišit podle výstrahy zabezpečení toohello. V některých případech můžete mít toouse další možnosti Azure tooimplement hello doporučené nápravy. Například hello nápravy pro tento útok je tooblacklist hello IP adresu, která generuje tento útok pomocí [seznamu ACL sítě](../virtual-network/virtual-networks-acl.md) nebo [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) pravidlo.

> [!NOTE]
> Další informace o hello různé typy výstrah, najdete v tématu [výstrahy zabezpečení podle typu v Azure Security Center](security-center-alerts-type.md).
>
>

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se naučili jak tooconfigure zásady zabezpečení ve službě Security Center. toolearn Další informace o Security Center, najdete v části hello následující:

* [Řešení bezpečnostních incidentů v Azure Security Center](security-center-incident.md)
* [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md)
* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md)
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.
