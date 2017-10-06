---
title: "chování aaaOverride HTTP pomocí stroj pravidel Azure CDN hello | Microsoft Docs"
description: "Stroj pravidel Hello vám umožní toocustomize zpracování požadavků HTTP pomocí Azure CDN, třeba blokování hello doručování určité typy obsahu, definovat zásady ukládání do mezipaměti a změnit hlavičky protokolu HTTP."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: dd7194be9dbda43180c64568d3e1f52c5c513a7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="override-http-behavior-using-hello-azure-cdn-rules-engine"></a>Potlačení chování HTTP pomocí stroj pravidel hello Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Přehled
Stroj pravidel Hello umožňuje toocustomize způsob zpracování požadavků HTTP, jako je například blokování hello doručování určité typy obsahu, definování zásady ukládání do mezipaměti a úpravy hlaviček protokolu HTTP.  V tomto kurzu se ukazují, že vytvoření pravidla, která se změní hello ukládání do mezipaměti chování CDN prostředků.  Je také video obsah k dispozici v hello "[viz také](#see-also)" oddílu.

   > [!TIP] 
   > Odkaz na syntaxi toohello podrobně naleznete v tématu [referenční dokumentace pravidel modul](cdn-rules-engine-reference.md).
   > 


## <a name="tutorial"></a>Kurz
1. Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    Otevře portál pro správu Hello CDN.
2. Klikněte na hello **HTTP velké** kartě, za nímž následuje **stroj pravidel**.
   
    Zobrazí se možnosti pro nové pravidlo.
   
    ![Nové možnosti pravidlo CDN](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > Hello pořadí, ve kterém jsou uvedeny víc pravidel, ovlivňuje způsob zpracování. Následující pravidlo může přepsat hello akce zadané předchozí pravidlem.
   > 
   > 
3. Zadejte název v hello **název nebo popis** textové pole.
4. Identifikace typu hello požadavků, které hello pravidlo se použije k.  Ve výchozím nastavení, hello **vždy** je vybraná podmínka shodu.  Budete používat **vždy** pro účely tohoto kurzu, takže políčko nechte který vybrali.
   
   ![Stav shody CDN](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > Existuje mnoho typů odpovídal podmínky, které jsou k dispozici v rozevírací nabídce hello.  Kliknete na informační ikonu toohello hello blue nalevo od hello shodu podmínku vysvětluje podmínku hello aktuálně vybrané podrobně.
   > 
   >  Úplný seznam hello podmíněných výrazů podrobně najdete v tématu [pravidla modul podmíněné výrazy](cdn-rules-engine-reference-match-conditions.md).
   >  
   > Úplný seznam hello shodu podmínek podrobně najdete v tématu [podmínky odpovídají stroj pravidel](cdn-rules-engine-reference-match-conditions.md).
   > 
   > 
5. Klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce** tooadd nové funkce.  V rozevírací nabídce hello na levé straně hello, vyberte **Force interní Max-Age**.  V textovém poli hello, který se zobrazí, zadejte **300**.  Ponechejte zbývající výchozí hodnoty hello.
   
   ![Funkce CDN](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Jako s podmínkami shoda, kliknete na informační ikonu toohello hello blue levé hello nové funkce zobrazí podrobnosti o této funkci.  V případě hello **Force interní Max-Age**, jsme jsou přepsání hello asset **Cache-Control** a **Expires** hlavičky toocontrol při hello CDN hraniční uzel aktualizuje hello Asset z počátku hello.  Náš příklad 300 sekund znamená, že hello CDN hraniční uzel po dobu 5 minut před aktualizací hello asset z jeho počátku mezipaměti hello asset.
   > 
   > Hello úplný seznam funkcí podrobně najdete v tématu [podrobnosti o funkcích stroj pravidel](cdn-rules-engine-reference-features.md).
   > 
   > 
6. Klikněte na tlačítko hello **přidat** tlačítko toosave hello nové pravidlo.  nové pravidlo Hello je nyní čeká na schválení. Jakmile byla schválena, hello stav se změní z **čekající XML** příliš**Active XML**.
   
   > [!IMPORTANT]
   > Změny pravidel může trvat až minut toopropagate too90 prostřednictvím hello CDN.
   > 
   > 

## <a name="see-also"></a>Viz také
* [Přehled Azure CDN](cdn-overview.md)
* [Referenční dokumentace pravidel modulu](cdn-rules-engine-reference.md)
* [Stav shody motoru pravidla](cdn-rules-engine-reference-match-conditions.md)
* [Podmíněné výrazy stroj pravidel](cdn-rules-engine-reference-conditional-expressions.md)
* [Funkce modulu pravidla](cdn-rules-engine-reference-features.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel hello](cdn-rules-engine.md)
* [Azure pátek: Azure CDN výkonné nové prémiové funkce](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)