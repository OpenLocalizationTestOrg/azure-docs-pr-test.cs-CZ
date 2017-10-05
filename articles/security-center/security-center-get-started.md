---
title: "Úvodní příručka Azure Security Center | Microsoft Docs"
description: "Tento článek vám pomůže rychle začít používat službu Azure Security Center. Provede vás komponentami pro správu zásad a monitorování zabezpečení a poskytne vám odkazy na další kroky."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 392c814b7d3ff6b4f0f7850a51960576775e0307
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-security-center-quick-start-guide"></a>Úvodní příručka ke službě Azure Security Center
Tento článek vám pomůže rychle začít používat Azure Security Center. Provede vás komponentami pro správu zásad a monitorování zabezpečení služby Security Center.

> [!NOTE]
> Od začátku června 2017 bude Security Center používat ke shromažďování a ukládání dat agenta Microsoft Monitoring Agent. Další informace najdete v článku o [migraci platformy pro Azure Security Center](security-center-platform-migration.md). Informace v tomto článku představují funkce služby Security Center po přechodu na agenta Microsoft Monitoring Agent.
>
>

## <a name="prerequisites"></a>Požadavky
Pokud chcete začít využívat Security Center, musíte mít předplatné pro Microsoft Azure. Pokud nemáte předplatné, můžete si vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

Pro předplatné se automaticky povolí úroveň Free služby Security Center. Tato úroveň poskytuje přehled o stavu zabezpečení vašich prostředků Azure. Nabízí základní správu zásad zabezpečení, doporučení týkající se zabezpečení a integraci s produkty zabezpečení a službami od partnerů Azure.

Služba Security Center je přístupná prostřednictvím [portálu Azure](https://azure.microsoft.com/features/azure-portal/). Další informace o portálu Azure Portal najdete v [dokumentaci k portálu](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="permissions"></a>Oprávnění
V Centru zabezpečení zobrazí jenom informace týkající se prostředek služby Azure, když jsou přiřazeny roli vlastník, Přispěvatel nebo Čtenář pro předplatné nebo skupinu prostředků, které daný prostředek patří. V tématu [oprávnění v Azure Security Center](security-center-permissions.md) Další informace o rolích a povolených akcí v Centru zabezpečení.

## <a name="data-collection"></a>Shromažďování dat
Security Center shromažďuje data z vašich virtuálních počítačů za účelem posouzení jejich stavu, poskytování doporučení zabezpečení a upozorňování na hrozby. Při prvním přístupu ke službě Security Center je shromažďování dat povolené pro všechny virtuální počítače v rámci vašeho předplatného. Security Center zřizuje agenta Microsoft Monitoring Agent do všech existujících podporované virtuální počítače Azure a všechny nové, které jsou vytvořeny. V tématu [povolení shromažďování dat](security-center-enable-data-collection.md) Další informace o tom, jak shromažďování dat funguje.

Shromažďování dat se doporučuje. Pokud používáte úroveň Free služby Security Center, můžete zakázat shromažďování dat z virtuálních počítačů tak, že zakážete shromažďování dat v zásadě zabezpečení. Shromažďování dat je vyžadován pro odběry ve standardní vrstvě služby Security Center. V tématu [Security Center ceny](security-center-pricing.md) Další informace o volné a cenové úrovně Standard.

Následující kroky popisují, jak získat přístup ke komponentám služby Security Center a jak je používat. V rámci těchto kroků předvedeme, jak vypnout shromažďování dat, pokud se rozhodnete zrušit je.

> [!NOTE]
> Tento článek vám tuto službu představí formou ukázkového nasazení. Tento článek není podrobný průvodce.
>
>

## <a name="access-security-center"></a>Přístup ke službě Security Center
Na portálu použijte pro přístup ke službě Security Center následující kroky:

1. V nabídce **Microsoft Azure** vyberte **Security Center**.

   ![Nabídky Azure][1]
2. Pokud se službou Security Center pracujete poprvé, otevře se okno **Vítejte**. Vyberte **spuštění Security Center** otevřete **Security Center** okno a umožňuje shromažďování dat.
   ![Obrazovka Vítejte][10]
3. Po spuštění služby Security Center z okna Vítejte nebo po výběru služby Security Center z nabídky Microsoft Azure se otevře okno **Security Center**. Pro zajištění snadného přístupu k oknu **Security Center** v budoucnu vyberte možnost **Připnout okno na řídicí panel** (vpravo nahoře).
   ![Možnost Připnout okno na řídicí panel][2]

## <a name="use-security-center"></a>Použití služby Security Center
Můžete nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure. Nakonfigurujme zásady zabezpečení pro vaše předplatné:

1. V okně **Security Center** vyberte dlaždici **Zásady**.
2. Na **zásady zabezpečení - definovat zásady podle předplatného** okně, vyberte předplatné.
3. V okně **Zásady zabezpečení** je zapnutá možnost **Shromažďování dat** zajišťující automatické shromažďování protokolů. Rozšíření monitorování je zajištěné ve všech aktuálních a nových virtuálních počítačích v rámci příslušného předplatného. (Na úrovni Free služby Security Center, se můžete rozhodnout mimo shromažďování dat nastavením **shromažďování dat** k **vypnout**. Nastavení **shromažďování dat** k **vypnout** brání Security Center budete výstrahy zabezpečení a doporučení.)
4. V okně **Zásady zabezpečení** vyberte **Zásady prevence**. Otevře se okno **Zásady prevence**.
5. V okně **Zásady prevence** zapněte doporučení, která chcete zobrazovat v rámci zásad zabezpečení. Příklady:

   * Nastavení **aktualizací systému** k **na** kontroly všechny podporované virtuální počítače pro chybějící aktualizace operačního systému.
   * Nastavení **ohrožení zabezpečení operačního systému** k **na** kontroly všechny podporované virtuální počítače vyhledají konfigurace operačního systému, které může zvýšit virtuálního počítače vůči útokům.

### <a name="view-recommendations"></a>Zobrazení doporučení
1. Vraťte se do okna **Security Center** a vyberte dlaždici **Doporučení**. Security Center pravidelně analyzuje stav zabezpečení vašich prostředků Azure. Když služba Security Center identifikuje potenciální ohrožení zabezpečení, zobrazí doporučení v okně **Doporučení**.
   ![Doporučení ve službě Azure Security Center][5]
2. Po výběru doporučení v okně **Doporučení** se zobrazí další informace nebo budete moci provést akci vedoucí k vyřešení daného problému.

### <a name="view-the-security-state-of-your-resources"></a>Zobrazit stav zabezpečení vašich prostředků
1. Vraťte se do okna **Security Center**. **Prevence** část řídicího panelu obsahuje indikátory stavu zabezpečení pro virtuální počítače, sítě, datům a aplikacím.
2. Vyberte **výpočetní** zobrazíte další informace. **Výpočetní** otevře se okno zobrazuje tři karty:

  - **Přehled** – obsahuje, monitorování a doporučení pro virtuální počítače.
  - **Virtuální počítače** -obsahuje seznam všech virtuálních počítačů a aktuální zabezpečení stavy.
  - **Cloudové služby** -obsahuje seznam webových a pracovních rolí, které jsou monitorovány pomocí služby Security Center.

    ![Dlaždice stavu prostředků ve službě Azure Security Center][6]

3. Na **přehled** vyberte doporučení v části **doporučení virtuální počítače** můžete zobrazit další informace nebo akce vedoucí ke konfiguraci příslušných ovládacích prvků.
4. Na **virtuální počítače** , vyberte virtuální počítač k zobrazení dalších podrobností.

### <a name="view-security-alerts"></a>Zobrazení výstrah zabezpečení
1. Vraťte se do okna **Security Center** a vyberte dlaždici **Výstrahy zabezpečení**. Otevře se okno **Výstrahy zabezpečení** se seznamem výstrah. Tyto výstrahy generuje služba Security Center při analýze protokolů zabezpečení a síťové aktivity. Součástí jsou i výstrahy z integrovaných partnerských řešení.
   ![Výstrahy zabezpečení ve službě Azure Security Center][7]

   > [!NOTE]
   > Výstrahy zabezpečení jsou k dispozici jen v případě, že je povolena úroveň Standard služby Security Center. 60denní bezplatnou zkušební verzi na plán úrovně Standard je k dispozici. Informace o postupu při pořizování úrovně Standard najdete v části [Další kroky](#next-steps).
   >
   >
2. Výběrem výstrahy zobrazíte další informace. V tomto příkladu vybereme možnost **Zjištěna úprava binárního systémového souboru**. Otevřou se okna s dalšími podrobnostmi o příslušné výstraze.
   ![Podrobnosti výstrah zabezpečení ve službě Azure Security Center][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Zobrazení stavu partnerských řešení
1. Vraťte se do okna **Security Center**. Dlaždice **Partnerská řešení** umožňuje přehledně sledovat stav partnerských řešení integrovaných ve vašem předplatném Azure.
2. Vyberte dlaždici **Partner solutions** (Partnerská řešení). Otevře se okno se seznamem partnerských řešení připojených k Security Center.
   ![Partnerská řešení][9]
3. Vyberte jedno partnerské řešení. V tomto příkladu budeme vyberte **QualysVa1** řešení.  Otevře se okno se stavem tohoto partnerského řešení a jeho přidruženými prostředky. Výběrem možnosti **Solution console** (Konzola řešení) otevřete prostředí pro správu tohoto partnerského řešení.

## <a name="next-steps"></a>Další kroky
Tento článek vám poskytl úvod do komponent správy zásad a monitorování zabezpečení služby Security Center. Teď už jste se se službou Security Center seznámili a můžete vyzkoušet následující kroky:

* Nakonfigurujte zásady zabezpečení pro své předplatné Azure. Další informace najdete v tématu [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md).
* Použijte doporučení ve službě Security Center při ochraně svých prostředků Azure. Další informace najdete v tématu [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md).
* Kontrolujte a spravujte své aktuální výstrahy zabezpečení. Další informace najdete v tématu [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md).
- [Zabezpečení dat v Azure Security Center](security-center-data-security.md) -další způsob správy a zabezpečení ve službě Security Center.
* Přečtěte si další informace o [funkcích pokročilé detekce hrozeb](security-center-detection-capabilities.md) poskytovaných spolu s [úrovní Standard](security-center-pricing.md) ve službě Security Center. Úroveň Standard se bezplatně nabízí pro dobu prvních 60 dní.
* Pokud máte dotazy týkající se použití služby Security Center, přečtěte si článek [Azure Security Center – nejčastější dotazy](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
