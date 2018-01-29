---
title: "Vlastní pravidla výstrah v Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže vytvořit vlastní pravidla výstrah v Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f335d8c4-0234-4304-b386-6f1ecda07833
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: yurid
ms.openlocfilehash: 10c5c3e6f714aac4c08cb810f54eb09c3d2bbe70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="custom-alert-rules-in-azure-security-center-preview"></a>Vlastní pravidla výstrah v Azure Security Center (Preview)
Tento dokument vám pomůže vytvořit vlastní pravidla výstrah v Azure Security Center.

## <a name="what-are-custom-alert-rules-in-security-center"></a>Co jsou vlastní pravidla výstrah ve službě Security Center?

Služba Security Center obsahuje sadu předdefinovaných [výstrah zabezpečení](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts), které se aktivují v případě výskytu hrozby nebo podezřelé aktivity. V některých scénářích můžete chtít vytvořit vlastní výstrahu, která by řešila konkrétní požadavky vašeho prostředí. 

Vlastní pravidla výstrah ve službě Security Center umožňují definovat nové výstrahy zabezpečení na základě již shromážděných dat z vašeho prostředí. Můžete vytvořit dotazy a výsledky těchto dotazů použít jako kritéria pro vlastní pravidlo, které se provede po splnění těchto kritérií. K vytváření vlastních dotazů můžete použít také události zabezpečení počítačů, protokoly partnerských řešení zabezpečení nebo data ingestovaná pomocí rozhraní API. 

## <a name="how-to-create-a-custom-alert-rule-in-security-center"></a>Jak vytvořit vlastní pravidlo výstrah ve službě Security Center?

Otevřete řídicí panel **Security Center** a podle těchto kroků vytvořte vlastní pravidlo výstrah:

1.  V levém podokně v části **Detekce** klikněte na **Vlastní pravidla výstrah (Preview)**.
2.  Na stránce **Security Center – Vlastní pravidla výstrah (Preview)** klikněte na **Nové vlastní pravidlo výstrah**.

    ![Vlastní výstraha](./media/security-center-custom-alert/security-center-custom-alert-fig1.png)
    
3.  Zobrazí se stránka Vytvořit vlastní pravidlo výstrah, která obsahuje následující možnosti:
    
    ![Vytvořit](./media/security-center-custom-alert/security-center-custom-alert-fig2.png)

4.  Do pole **Název** zadejte název tohoto vlastního pravidla.
5.  Do pole **Popis** zadejte stručný popis vyjadřující záměr tohoto pravidla.
6.  V poli **Závažnost** vyberte úroveň závažnosti (Vysoká, Střední, Nízká) podle svých potřeb.
7.  V poli **Předplatné** vyberte předplatné, na které se toto pravidlo vztahuje.
8.  V poli **Pracovní prostor** vyberte pracovní prostor, který chcete pomocí tohoto pravidla monitorovat, a do pole **Vyhledávací dotaz** zadejte dotaz, který používáte k získání výsledků. Výsledek tohoto dotazu aktivuje výstrahu. Všimněte si, že při zadání platného dotazu se v pravém rohu tohoto pole zobrazí zelená značka zaškrtnutí:

    ![Dotaz](./media/security-center-custom-alert/security-center-custom-alert-fig3.png)

10. V poli **Období** vyberte časové období, během kterého se výše uvedený dotaz bude provádět. Všimněte si, že v závislosti na vybraném časovém období se změní výsledek vyhledávání v dolní části tohoto pole.

    ![Období](./media/security-center-custom-alert/security-center-custom-alert-fig4.png)

11. V poli **Vyhodnocování** vyberte frekvenci, s jakou se má toto pravidlo vyhodnocovat a provádět.
12. V poli **Počet výsledků** vyberte operátor (větší než nebo menší než).
13. Do pole **Prahová hodnota** zadejte číslo, které bude sloužit jako referenční hodnota pro operátor vybraný dříve.
14. Pokud chcete nastavit dobu čekání před tím, než služba Security Center pro toto pravidlo odešle další výstrahu, povolte možnost **Potlačit výstrahy**.
15. Kliknutím na **OK** vytváření dokončete.

Po dokončení vytváření se nové pravidlo výstrah zobrazí v seznamu vlastních pravidel výstrah. Jakmile budou splněny podmínky tohoto pravidla, aktivuje se nová výstraha, kterou můžete zobrazit na řídicím panelu **Výstrahy zabezpečení**.

![Výstrahy](./media/security-center-custom-alert/security-center-custom-alert-fig5.png)

Všimněte si, že výstraha vlastního pravidla obsahuje parametry (vyhledávací dotaz, prahová hodnota atd.) vytvořené při vytváření tohoto pravidla.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak vytvořit vlastní pravidlo výstrah v Azure Security Center. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Zjistěte, jak spravovat výstrahy a reagovat na incidenty zabezpečení ve službě Security Center.
* [Monitorování stavu zabezpečení ve službě Azure Security Center](security-center-monitoring.md). Zjistěte, jak monitorovat stav svých prostředků Azure.
* [Principy výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Seznamte se s dalšími typy výstrah zabezpečení.
* [Průvodce odstraňováním potíží pro službu Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Zjistěte, jak řešit běžné problémy ve službě Security Center. 
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

