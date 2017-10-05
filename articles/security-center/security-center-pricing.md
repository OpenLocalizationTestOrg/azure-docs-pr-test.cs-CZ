---
title: Ceny Security Center | Microsoft Docs
description: "Tento článek obsahuje informace o cenách pro Azure Security Center."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 367b8f38cb9fcf3dc36db83641cb1696710608ef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-security-center-pricing"></a>Azure Security Center – ceny
Azure Security Center pomáhá předcházet hrozbám, zjišťovat je a reagovat na ně a nabízí lepší přehled o zabezpečení prostředků Azure a kontrolu nad nimi. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

## <a name="pricing-tiers"></a>Cenové úrovně
Security Center je k dispozici v dvěma vrstvami:

* **Úroveň Free** je automaticky povolené na všechny odběry služby Azure. Úroveň Free poskytuje přehled o stavu zabezpečení prostředků Azure, zásady základního zabezpečení, doporučení zabezpečení a integrace s zabezpečovací produkty a služby od partnerů.
* **Úrovně Standard** přidá rozšířené hrozba možností detekce, včetně hrozby intelligence, analýzy chování, detekce anomálií, incidenty zabezpečení a hrozby assessment sestavy. Úroveň Standard se bezplatně nabízí pro dobu prvních 60 dní.

Další informace najdete v tématu Security Center [stránce s cenami](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="try-standard-free-for-60-days"></a>Zkuste Standard zdarma 60 dnů.
Úroveň Standard se bezplatně nabízí pro dobu prvních 60 dní. Na konci 60 dnů by měl můžete rozhodnout pokračovat v používání služby, můžeme automaticky spustí poplatků za využití.

Pokud chcete získat na plán úrovně Standard:

1. Vyberte dlaždici **Zásady** v okně **Security Center**.
2. Vyberte odběr, který chcete upgradovat na Standard.
3. Na **zásady zabezpečení** vyberte **cenová úroveň**.
4. Na **zvolte cenovou úroveň** vyberte **standardní**.
5. Klikněte na **Vybrat**.


## <a name="why-upgrade-to-standard"></a>Proč upgradovat na Standard?
Služby Security Center na plán úrovně Standard poskytuje všechny funkce úroveň Free plus detekce pokročilé hrozeb. Detekce hrozeb pokročilé pomáhá identifikovat active hrozeb cílení na vašich prostředků Azure a poskytuje přehled potřeby rychle reagovat.

Služba Security Center využívá pokročilou analýzu zabezpečení, která daleko překračuje možnosti detekce založené na signaturách či příznacích. Změnám ve velkých objemů dat a strojové učení technologie se používají k vyhodnocení události napříč celou cloudových prostředků infrastruktury – zjišťování hrozeb, které by bylo možné zjistit pomocí ruční přístupy a predikci vývoj útoků.

Analýza zabezpečení, které jsou součástí na plán úrovně Standard jsou:

* **Hrozby intelligence** -hledá známé nesprávnými účastníky pomocí globální analýzou hrozeb z produktů společnosti Microsoft a služby, Microsoft jejichž šíření tým Dcu, Microsoft Security Response Center a externích informačních kanálů.
* **Analýzy chování** -platí známé vzorce ke zjištění škodlivé chování
* **Detekce anomálií** -vytvoří pomocí statistické profilace historických směrného plánu. Zobrazuje výstrahy na odchylky od zavedených standardních hodnot, které odpovídají potenciální vektor útoku

V **výstrahy zabezpečení** níže, okno Security Center zjistila zabezpečení **incident**. Incidentu zabezpečení je agregaci všechny výstrahy pro prostředek, které zarovnat kill řetězu vzory. Výběr bezpečnostního incidentu zobrazí další informace o incidentu a uvádí související výstrahy. Výběrem výstrahy poskytuje další informace o tomto výskyt.

![Incident zabezpečení][2]

**Síťová komunikace** výstraha níže poskytuje podrobnosti o výstraze. Podrobnosti jsou jeho úplný popis, jeho závažnost, jeho aktuální stav (v takovém případě se zruší, což znamená by uživatel provedl akci, zavřete ji), attacked prostředků a nápravy kroky. Je také seznam odkazů na sestavy analýzou hrozeb Microsoftu. Tyto sestavy slouží k zabezpečení nápravy a obranným účelům.

![Podrobnosti výstrahy zabezpečení][3]

## <a name="enable-data-collection"></a>Povolení shromažďování dat
Pokud chcete povolit pro vypracování analýzy chování virtuálního počítače, musí být zapnut shromažďování dat.

Chcete-li ověřit, zda je povoleno shromažďování dat:

1. Vyberte **zásad** dlaždici. **Zásady zabezpečení** otevře se okno výpis předplatné Azure.
2. Vyberte předplatné.
3. Pokud **shromažďování dat** vypnout, změňte jej na on a uložte změnu.

> [!NOTE]
> Pokud používáte Azure Security Center volné, můžete zakázat shromažďování dat z virtuálních počítačů v zásadách zabezpečení. Pro předplatná na úrovni Standard se shromažďování dat požaduje.
>
>

V tématu [povolení shromažďování dat v Azure Security Center](security-center-enable-data-collection.md) Další informace.

## <a name="next-steps"></a>Další kroky
* V tomto dokumentu jste se seznámili s ceny Security Center. Další informace o cenách, najdete v Centru zabezpečení [stránce s cenami](https://azure.microsoft.com/pricing/details/security-center/).
* Další informace o možnostech Rozšířené zjišťování Security Center najdete v tématu [funkce zjišťování služby Azure Security Center](security-center-detection-capabilities.md).
* Další informace o způsob správy a zabezpečení v Centru zabezpečení dat najdete v tématu [zabezpečení dat v Azure Security Center](security-center-data-security.md).
* Pokud máte dotazy týkající se použití služby Security Center, přečtěte si článek [Azure Security Center – nejčastější dotazy](security-center-faq.md).
* Pokud máte dotazy týkající se používání Security Center nebo Azure, přejděte [fóra Azure](https://social.msdn.microsoft.com/Forums/home?forum=AzureSecurityCenter&filter=alltypes&sort=lastpostdesc).

<!--Image references-->
[1]: ./media/security-center-pricing/standard.png
[2]: ./media/security-center-pricing/incident.png
[3]: ./media/security-center-pricing/network-alert.png
