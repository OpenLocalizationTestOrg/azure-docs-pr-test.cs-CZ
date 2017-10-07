---
title: "Příručka aaaAzure Security Center rychlý start | Microsoft Docs"
description: "Tento článek vám rychle začít používat Azure Security Center provede vás hello zabezpečení komponentami a monitorování zásad správy a propojením jste toonext kroky pomůže."
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
ms.openlocfilehash: 23b2444ba1ba30d0a1bd1a1afbc4fd0abfd0827c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-quick-start-guide"></a>Úvodní příručka ke službě Azure Security Center
Tento článek pomáhá rychle začít s Azure Security Center a provede vás hello zabezpečení komponentami a monitorování zásad správy služby Security Center.

> [!NOTE]
> Počínaje časná 2017 června, Security Center použije hello agenta Microsoft Monitoring Agent toocollect a ukládat data. V tématu [Azure Security Center platformy migrace](security-center-platform-migration.md) toolearn Další. Hello informace v tomto článku představuje funkce Security Center po přechodu toohello agenta Microsoft Monitoring Agent.
>
>

## <a name="prerequisites"></a>Požadavky
tooget začít s Security Center, musíte mít tooMicrosoft předplatné Azure. Pokud nemáte předplatné, můžete si vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

úroveň Free Hello služby Security Center je automaticky povolen s vaše předplatné a poskytuje přehled o hello stav zabezpečení vašich prostředků Azure. Nabízí základní správu zásad zabezpečení, doporučení týkající se zabezpečení a integraci s produkty zabezpečení a službami od partnerů Azure.

Security Center je přístupná z hello [portál Azure](https://azure.microsoft.com/features/azure-portal/). toolearn Další informace o hello portál Azure, najdete v části hello [portálu dokumentace](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="permissions"></a>Oprávnění
V Centru zabezpečení, zobrazí jenom informace týkající se tooan prostředků Azure, když jsou přiřazeny hello roli vlastník, Přispěvatel nebo Čtenář pro předplatné nebo prostředek skupiny hello, která daný prostředek patří. V tématu [oprávnění v Azure Security Center](security-center-permissions.md) toolearn více informací o rolí a povolených akcí v Centru zabezpečení.

## <a name="data-collection"></a>Shromažďování dat
Security Center shromažďuje data z vaší virtuální počítače (VM) tooassess jejich stavu zabezpečení, zadejte doporučení zabezpečení a výstrah toothreats. Při prvním přístupu ke službě Security Center je shromažďování dat povolené pro všechny virtuální počítače v rámci vašeho předplatného. Security Center zřizuje hello agenta Microsoft Monitoring Agent na všechny stávající podporované virtuální počítače Azure a všechny nové, které jsou vytvořeny. V tématu [povolení shromažďování dat](security-center-enable-data-collection.md) toolearn informace o tom, jak shromažďování dat funguje.

Shromažďování dat se doporučuje. Pokud používáte úroveň Free hello služby Security Center, můžete zakázat shromažďování dat z virtuálních počítačů tak, že zakážete shromažďování dat v zásadách zabezpečení hello. Shromažďování dat je vyžadován pro odběry ve standardní vrstvě hello služby Security Center. V tématu [ceny Security Center](security-center-pricing.md) toolearn Další informace o hello volné a Standard cenové úrovně.

Hello následující kroky popisují, jak tooaccess a používání hello součásti služby Security Center. V následujícím postupu jsme ukazují, jak tooturn vypnout shromažďování dat, pokud se rozhodnete tooopt limitu.

> [!NOTE]
> Tento článek představuje hello služby pomocí příklad nasazení. Tento článek není podrobný průvodce.
>
>

## <a name="access-security-center"></a>Přístup ke službě Security Center
Hello portálu postupujte podle těchto kroků tooaccess Security Center:

1. Na hello **Microsoft Azure** nabídce vyberte možnost **Security Center**.

   ![Nabídky Azure][1]
2. Pokud se připojujete Security Center pro hello poprvé, hello **úvodní** otevře se okno. Vyberte **spuštění Security Center** tooopen hello **Security Center** okno a tooenable shromažďování dat.
   ![Obrazovka Vítejte][10]
3. Po spuštění Security Center v okně Vítá hello nebo vyberte z nabídky hello Microsoft Azure Security Center, hello **Security Center** otevře se okno. Pro snadný přístup toohello **Security Center** okno v budoucí, vyberte hello hello **Pin okno toodashboard** možnost (vpravo nahoře).
   ![Možnost toodashboard okno PIN kódu][2]

## <a name="use-security-center"></a>Použití služby Security Center
Můžete nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure. Nakonfigurujme zásady zabezpečení pro vaše předplatné:

1. Na hello **Security Center** okně, vyberte hello **zásad** dlaždici.
2. Na hello **zásady zabezpečení - definovat zásady podle předplatného** okně, vyberte předplatné.
3. Na hello **zásady zabezpečení** okně **shromažďování dat** je povoleno tooautomatically shromažďování protokolů. na všech aktuálních a nových virtuálních počítačích v rámci předplatného hello je zajištěna Hello rozšíření monitorování. (Na úrovni Free hello služby Security Center, se můžete rozhodnout mimo shromažďování dat nastavením **shromažďování dat** příliš**vypnout**. Nastavení **shromažďování dat** příliš**vypnout** brání Security Center budete výstrahy zabezpečení a doporučení.)
4. Na hello **zásady zabezpečení** vyberte **zásada Zabránění**. Tím se otevře hello **zásada Zabránění** okno.
5. Na hello **zásada Zabránění** okně zapnout hello doporučení, že chcete toosee jako součást vaše zásady zabezpečení. Příklady:

   * Nastavení **aktualizací systému** příliš**na** kontroly všechny podporované virtuální počítače pro chybějící aktualizace operačního systému.
   * Nastavení **ohrožení zabezpečení operačního systému** příliš**na** hello kontroly všechny podporované virtuální počítače tooidentify vyhledají konfigurace operačního systému, které může virtuální počítač zranitelnější tooattack.

### <a name="view-recommendations"></a>Zobrazení doporučení
1. Vrátí toohello **Security Center** okno a vyberte hello **doporučení** dlaždici. Security Center pravidelně analyzuje stav zabezpečení hello vašich prostředků Azure. Pokud Security Center identifikuje potenciální ohrožení zabezpečení, zobrazuje doporučení v hello **doporučení** okno.
   ![Doporučení ve službě Azure Security Center][5]
2. Vyberte doporučení na hello **doporučení** tooview okně Další informace nebo tootake hello tooresolve akce vydání.

### <a name="view-hello-security-state-of-your-resources"></a>Zobrazení hello stav zabezpečení vašich prostředků
1. Vrátí toohello **Security Center** okno. Hello **prevence** část řídicího panelu hello obsahuje indikátory stavu zabezpečení hello pro virtuální počítače, sítě, datům a aplikacím.
2. Vyberte **výpočetní** tooview Další informace. Hello **výpočetní** otevře se okno zobrazuje tři karty:

  - **Přehled** – obsahuje, monitorování a doporučení pro virtuální počítače.
  - **Virtuální počítače** -obsahuje seznam všech virtuálních počítačů a aktuální zabezpečení stavy.
  - **Cloudové služby** -obsahuje seznam webových a pracovních rolí, které jsou monitorovány pomocí služby Security Center.

    ![Hello dlaždice stavu prostředků v Azure Security Center][6]

3. Na hello **přehled** vyberte doporučení v části **virtuální počítače doporučení** tooview Další informace nebo provést akci tooconfigure příslušných ovládacích prvků.
4. Na hello **virtuální počítače** vyberte další podrobnosti tooview virtuálních počítačů.

### <a name="view-security-alerts"></a>Zobrazení výstrah zabezpečení
1. Vrátí toohello **Security Center** okno a vyberte hello **výstrahy zabezpečení** dlaždici. Hello **výstrahy zabezpečení** okno otevře a zobrazí seznam výstrah. Hello Security Center při analýze protokolů zabezpečení a síťové aktivity generuje tyto výstrahy. Součástí jsou i výstrahy z integrovaných partnerských řešení.
   ![Výstrahy zabezpečení ve službě Azure Security Center][7]

   > [!NOTE]
   > Výstrahy zabezpečení jsou dostupné, pokud úroveň Standard hello služby Security Center je povolená jenom. 60denní bezplatnou zkušební verzi hello úrovně Standard je k dispozici. V tématu [další kroky](#next-steps) informace o tom, jak tooget hello Standard vrstvy.
   >
   >
2. Vyberte výstrahy tooview Další informace. V tomto příkladu vybereme možnost **Zjištěna úprava binárního systémového souboru**. Otevře se okna, které poskytují další podrobnosti o výstraze hello.
   ![Podrobnosti výstrah zabezpečení ve službě Azure Security Center][8]

### <a name="view-hello-health-of-your-partner-solutions"></a>Zobrazit hello stav vašich partnerských řešení
1. Vrátí toohello **Security Center** okno. Hello **Partner solutions** dlaždice umožňuje sledovat, na první pohled, hello stav vašich partnerských řešení integrovaných ve vašem předplatném Azure.
2. Vyberte hello **Partner solutions** dlaždici. Otevře se okno a zobrazí seznam vašich partnerských řešení připojených tooSecurity Center.
   ![Partnerská řešení][9]
3. Vyberte jedno partnerské řešení. V tomto příkladu budeme vyberte hello **QualysVa1** řešení.  Okno otevře a ukazuje, že stav hello hello partnerského řešení a řešení hello přidružené prostředky. Vyberte **řešení konzoly** tooopen hello partnera správu prostředí pro toto řešení.

## <a name="next-steps"></a>Další kroky
Tento článek se zavedl toohello zabezpečení komponentami a monitorování zásad správy služby Security Center. Teď, když jste obeznámeni s Centrem zabezpečení, zkuste hello následující kroky:

* Nakonfigurujte zásady zabezpečení pro své předplatné Azure. Další, najdete v části toolearn [nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md).
* Použít hello doporučení v Security Center toohelp chránit prostředky v Azure. Další, najdete v části toolearn [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md).
* Kontrolujte a spravujte své aktuální výstrahy zabezpečení. Další, najdete v části toolearn [správy a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md).
- [Zabezpečení dat v Azure Security Center](security-center-data-security.md) -další způsob správy a zabezpečení ve službě Security Center.
* Další informace o hello [pokročilé funkce detekce hrozeb](security-center-detection-capabilities.md) které jsou součástí hello [úrovně Standard](security-center-pricing.md) služby Security Center. Hello úrovně Standard se nabízí zadarmo pro hello prvních 60 dní.
* Pokud máte dotazy týkající se používání Security Center, najdete v části hello [Azure Security Center – nejčastější dotazy](security-center-faq.md).

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
