---
title: "prostředky aaaPre zatížení na koncový bod Azure CDN | Microsoft Docs"
description: "Zjistěte, jak toopre zatížení mezipaměti obsahu na koncový bod Azure CDN."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 08ac4b834f1ac8ce59d22e65fa8adea11bafcf17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Předběžné načtení prostředků v koncovém bodu Azure CDN
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Ve výchozím nastavení prostředky nejprve mezipaměti, jako jsou požadovali. To znamená, že první požadavek hello z každé oblasti může trvat déle, vzhledem k tomu, že nebude mít servery edge hello hello obsah do mezipaměti a bude nutné tooforward hello požadavek toohello zdrojový server. Předběžné načítání obsahu zabraňuje tato první přístupů latence.

Kromě toho tooproviding lepší zkušeností zákazníků, předem načítání vaše prostředky uložené v mezipaměti může také přinést snížení síťový provoz na původním serveru hello.

> [!NOTE]
> Předběžné načítání prostředků je užitečné pro velké události nebo obsahu, který bude současně dostupné tooa velký počet uživatelů, jako je nová verze film nebo aktualizace softwaru.
> 
> 

Tento kurz vás provede předběžné načítání obsahu v mezipaměti na všech uzlech edge Azure CDN.

## <a name="walkthrough"></a>Názorný postup
1. V hello [portálu Azure](https://portal.azure.com), procházet profil CDN toohello obsahující koncový bod hello chcete toopre zatížení.  Otevře se okno profil Hello.
2. Klikněte na tlačítko hello koncového bodu v seznamu hello.  Otevře se okno Hello koncový bod.
3. V okně koncový bod CDN hello klikněte na tlačítko zatížení hello.
   
    ![Okno koncový bod CDN](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    Otevře se okno zatížení Hello.
   
    ![Okno zatížení CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Zadejte úplnou cestu hello každého majetku chcete tooload (například `/pictures/kitten.png`) v hello **cesta** textové pole.
   
   > [!TIP]
   > Další **cesta** textová pole se zobrazí, když zadáte text tooallow toobuild seznam více prostředků.  Prostředky můžete odstranit ze seznamu hello kliknutím na tlačítko se třemi tečkami (...) hello.
   > 
   > Cesty musí být relativní adresa URL, která odpovídá hello následující [regulární výraz](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > >Načíst cestu k souboru jedním `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;  
   > >Načíst jeden soubor s řetězec dotazu`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`  
   > 
   > Každý prostředek musí mít svůj vlastní cesta.  Není k dispozici žádná funkce zástupných znaků pro předběžné načítání prostředků.
   > 
   > 
   
    ![Tlačítko zatížení](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Klikněte na tlačítko hello **zatížení** tlačítko.
   
    ![Tlačítko zatížení](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> Existuje omezení 10 zatížení požadavků za minutu za profil CDN. 50 cest jsou povoleny na základě požadavku. Každá cesta může mít délku cesty 1024 znaků.
> 
> 

## <a name="see-also"></a>Viz také
* [Vyprázdnění koncového bodu Azure CDN](cdn-purge-endpoint.md)
* [Azure referenční dokumentace rozhraní API REST CDN - mazání nebo předběžné načtení koncový bod](https://msdn.microsoft.com/library/mt634451.aspx)

