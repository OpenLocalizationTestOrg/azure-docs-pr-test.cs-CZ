---
title: aaaAzure sestavy intelligence threat Security Center | Microsoft Docs
description: "Tento dokument vám pomůže toouse Azure Security Center Threat inteligentního sestavy během šetření toofind Další informace týkající se výstraha zabezpečení."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5662e312-e8c2-4736-974e-576eeb333484
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: c888cfac1dd8b057616a6b8e6c6f6b67b552f2e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-threat-intelligence-report"></a>Sestava analýzy hrozeb v Azure Security Center
Tento dokument vysvětluje, jakým způsobem vám mohou sestavy analýzy hrozeb v Azure Security Center pomoci zjistit více o hrozbě, který vygenerovala výstrahu zabezpečení.

## <a name="what-is-a-threat-intelligence-report"></a>Co je sestava analýzy hrozeb?
Detekce hrozeb Security Center funguje tak, že monitorování zabezpečení informací v prostředky Azure, sítě hello a připojených partnerských řešení. Analyzuje tyto informace, často korelace informace z více zdrojů, tooidentify hrozeb. Tento proces je součástí hello Security Center [možností detekce](security-center-detection-capabilities.md).

Když Security Center identifikuje hrozbu, aktivuje [výstrahu zabezpečení](security-center-managing-and-responding-alerts.md), která obsahuje podrobné informace týkající se konkrétní události, včetně návrhů na odstranění problémů. reakce na incidenty týmy tooassist prozkoumat ohrožení a oprava, Security Center obsahuje sestavu intelligence hrozba, která obsahuje informace o hello hrozba, která byla zjištěna, včetně informací, jako:

* Identita nebo přidružení útočníka (pokud je tato informace k dispozici)
* Cíle útočníků
* Současné a historické útočné kampaně (pokud je tato informace k dispozici)
* Taktika, nástroje a postupy útočníků
* Přidružené ukazatele ohrožení zabezpečení, například adresy URL a hodnoty hash souborů
* Victimology, což je oborový hello a jejich zeměpisné rozšíření tooassist můžete při určování, zda vašich prostředků Azure jsou v ohrožení
* Informace o zmírnění a odstraňování problémů

> [!NOTE]
> Hello množství informací v konkrétní sestavách se bude lišit; Hello úroveň podrobností vychází z aktivity a jejich rozšíření hello malwaru.
>
>

Security Center má tři typy sestav hrozeb, které se může lišit podle toohello útoku. jsou k dispozici Hello sestavy:

* **Sestava skupiny aktivit**: poskytuje podrobné informace o útočnících, jejich cílech a taktice.
* **Sestava kampaně**: zaměřuje se na podrobnosti o konkrétních útočných kampaních.
* **Souhrnná sestava hrozby**: obsahuje všechny položky hello v předchozí dva sestavách hello.

Tento typ informací je velmi užitečná při hello [reakcí na incidenty](security-center-incident-response.md) procesy, kterých je zdrojem probíhající šetření toounderstand hello hello útoku, útočník hello motivace a co toodo toomitigate to problém postoupíte.

## <a name="how-tooaccess-hello-threat-intelligence-report"></a>Jak tooaccess hello threat intelligence sestavy?
Aktuální výstrahy můžete zkontrolovat prohlížením hello **výstrahy zabezpečení** dlaždici. Otevřete hello portálu Azure a postupujte podle kroků hello toosee další podrobnosti o každé výstraze:

1. Na řídicím panelu Security Center hello, uvidíte hello **výstrahy zabezpečení** dlaždici.
2. Klikněte na tlačítko hello dlaždice tooopen hello **výstrahy zabezpečení** okno, které obsahuje další podrobnosti o výstrahách hello a klikněte na v hello výstrahu zabezpečení, které chcete tooobtain Další informace o.

    ![Výstrahy zabezpečení](./media/security-center-threat-report/security-center-threat-report-fig1.png)
3. V takovém případě hello **podezřelé proces spuštěný** hello podrobnosti o výstraze hello, jak je znázorněno na následujícím obrázku hello se zobrazí okno:

    ![Podrobnosti výstrahy zabezpečení](./media/security-center-threat-report/security-center-threat-report-fig2.png)
4. Hello množství informací, které jsou k dispozici pro každou výstrahu zabezpečení se budou lišit podle typu toohello výstrahy. V hello **sestavy** pole máte sestavu intelligence odkaz toohello hrozeb. Klikněte na něj a otevře se další okno prohlížeče se souborem PDF.

   ![Výběr úložiště](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Odsud můžete stáhnout hello PDF pro tuto sestavu a číst informace o zabezpečení hello problém, která byla zjištěna a provádět akce založené na informacích hello.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli, jak mohou sestavy analýzy hrozeb v Azure Security Center pomoci během vyšetřování výstrah zabezpečení. toolearn Další informace o službě Azure Security Center, najdete v části hello následující:

* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Nejčastější dotazy o použití hello služby najít.
* [Využití Azure Security Center při reakci na incidenty](security-center-incident-response.md)
* [Možnosti detekce v Azure Security Center](security-center-detection-capabilities.md)
* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md). Zjistěte, jak tooplan a pochopit hello návrhu aspekty tooadopt Azure Security Center.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md). Zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Řešení bezpečnostních incidentů v Azure Security Center](security-center-incident.md)
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.
