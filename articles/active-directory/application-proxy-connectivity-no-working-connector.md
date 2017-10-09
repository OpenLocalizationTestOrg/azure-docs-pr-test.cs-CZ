---
title: "Najít aaaNo pracovní konektor skupinu pro aplikaci Proxy aplikace | Microsoft Docs"
description: "Řešení problémů, které se můžete setkat, když není žádný funkční konektor ve skupině konektor pro vaši aplikaci s hello proxy aplikace služby Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4c4baf296b316db131929c9a7c618fb9960713e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Žádná pracovní skupina konektor najít pro aplikaci Proxy aplikace

Tento článek nápovědy vám tooresolve hello běžné problémy potýkají při konektor pro aplikaci Proxy aplikace zjištěn není integrované s Azure Active Directory.

## <a name="overview-of-steps"></a>Přehled kroků
Pokud není žádný funkční konektor ve skupině konektor pro vaši aplikaci, existuje několik způsobů tooresolve hello problému:

-   Pokud máte ve skupině hello žádné konektory, můžete:

    -   Stáhněte si nový konektor na hello správné místního serveru a přiřaďte ho toothis skupiny

    -   Konektor služby active přesunout do skupiny hello

-   Pokud máte ve skupině hello žádné aktivní konektory, můžete:

    -   Vaše konektor je neaktivní hello příčiny identifikovat a vyřešit

    -   Konektor služby active přesunout do skupiny hello

tooknow, který z nich je hello problém, otevřete nabídku "Proxy aplikace" hello ve vaší aplikaci a podívejte se na uvítací zprávu upozornění konektor skupiny. Zadejte ho této skupiny hello musí mít alespoň jeden konektor (nemáte žádné ve skupině hello) nebo že nemá žádné aktivní konektory (i když máte pravděpodobně neaktivní konektory).

   ![Výběr skupiny konektor portálu Azure](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Podrobnosti na každém z těchto možností najdete v tématu hello odpovídající části. Každá z těchto předpokládá, že začínáte ze stránky správy konektor hello. Pokud se díváte hello chybová zpráva výše, můžete přejít toothis stránku kliknutím na uvítací zprávu upozornění. V opačném případě lze najít tak, že přejdete příliš**Azure Active Directory**, kliknutím na na **podnikové aplikace, které**, pak **Proxy aplikace.**

   ![Správa skupin konektor portálu Azure](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Stáhněte si nový konektor

toodownload nový konektor, použijte tlačítko "Stáhnout konektor" hello hello horní části stránky hello.

hello Poznámka hello konektor potřebám toobe nainstalovaná na počítači s přímé směrem pohledu toohello back-end aplikace a je obvykle umístěny na stejném serveru jako aplikace hello. Po stažení, by hello konektor zobrazit v této nabídky. Klikněte na hello konektoru a použít toomake rozevíracího seznamu "Konektor skupinu" hello se, že patří toohello správné skupiny. Uložte změnu hello.

   ![Stáhnout hello konektor z hello portálu Azure](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Přesunout konektoru služby Active

Pokud máte active konektor, který by měly patřit toohello skupiny a má směrem pohledu toohello cílová back-end aplikace, můžete přesunout hello konektor do skupiny hello přiřazen. toodo proto, klikněte na hello konektor. V poli "Konektor skupinu" hello použijte správnou skupinu hello hello tooselect rozevíracího seznamu a klikněte na Uložit.

## <a name="resolve-an-inactive-connector"></a>Vyřešte neaktivní konektoru

Pokud hello konektory ve skupině hello jsou neaktivní, pouze je pravděpodobné, že na počítači, který nemá mít všechny porty potřebné hello odblokováno.

Viz hello porty Poradce při potížích dokumentu podrobnosti na odstranění příčin tohoto problému.

## <a name="next-steps"></a>Další kroky
[Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)


