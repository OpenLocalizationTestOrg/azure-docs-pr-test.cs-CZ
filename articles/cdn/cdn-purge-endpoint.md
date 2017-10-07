---
title: "aaaPurge koncový bod Azure CDN | Microsoft Docs"
description: "Zjistěte, jak toopurge všechny uložené v mezipaměti obsahu z koncového bodu Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: a09f4a49aa1e2d7655ecae44b5126c11c28fd599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="purge-an-azure-cdn-endpoint"></a>Vyprázdnění koncového bodu Azure CDN
## <a name="overview"></a>Přehled
Uzlů Azure CDN hraniční mezipaměti prostředky až do vypršení platnosti hello asset time to live (TTL).  Po vypršení platnosti hello asset TTL, když klient požádá o prostředek hello z hello hraniční uzel, bude hello hraniční uzel načítat aktualizované novou kopii požadavek klienta hello asset tooserve hello a aktualizace mezipaměti hello úložiště.

Hello osvědčených postupů toomake se, že uživatelé vždycky získat nejnovější verzi vaše prostředky hello je tooversion vaše prostředky pro každou aktualizaci a publikovat je jako nové adresy URL.  CDN načte okamžitě hello nové prostředky pro další požadavky klientů hello.  Někdy může přát toopurge do mezipaměti obsah ze všech uzlů okraj a vynutit je všechny tooretrieve nové aktualizované prostředky.  Může to být kvůli tooupdates tooyour webové aplikace nebo tooquickly aktualizace prostředky, které obsahují nesprávné informace.

> [!TIP]
> Všimněte si, že vyprazdňování pouze vymaže hello do mezipaměti obsah na servery edge hello CDN.  Všechny podřízené mezipaměti, jako je například proxy servery a místní prohlížeče mezipaměti, může stále obsahující kopii hello soubor v mezipaměti.  Je důležité tooremember to když nastavíte souboru time to live.  Podřízené klienta toorequest hello nejnovější verzi souboru můžete vynutit tím, že je jedinečný název pokaždé, když ho aktualizujete nebo přímým [řetězce dotazu do mezipaměti](cdn-query-string.md).  
> 
> 

Tento kurz vás provede vyprazdňování prostředky ze všech uzlů edge koncového bodu.

## <a name="walkthrough"></a>Názorný postup
1. V hello [portálu Azure](https://portal.azure.com), procházet profil CDN toohello obsahující koncový bod hello chcete toopurge.
2. Z hello okno profil CDN klikněte na tlačítko vyprázdnění hello.
   
    ![Okno profil CDN](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    Otevře se okno vyprázdnění Hello.
   
    ![Okno vyprázdnění CDN](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. Na hello Vymazat okno, vyberte adresu služby hello chcete toopurge z rozevíracího seznamu hello adresy URL.
   
    ![Vyprázdnění formuláře](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > Můžete také získat toohello vyprázdnění okno kliknutím hello **mazání** tlačítka v okně koncový bod CDN hello.  V takovém případě hello **URL** pole bude předem vyplněny adresu služby hello této konkrétní koncového bodu.
   > 
   > 
4. Vyberte, jaké prostředky chcete toopurge z hello uzly okraj.  Pokud chcete tooclear všechny prostředky, klikněte na tlačítko hello **vyprázdnit všechno** zaškrtávací políčko.  Jinak, zadejte cestu hello každého majetku chcete toopurge v hello **cesta** textové pole. Následující formáty, které jsou podporovány v cestě hello.
    1. **Vyprázdnění jedné adresy URL**: vyprázdnění jednotlivý prostředek zadáním hello úplnou adresu URL, s nebo bez hello příponu souboru, například`/pictures/strasbourg.png`;`/pictures/strasbourg`
    2. **Zástupné vyprázdnění**: hvězdičky (\*) může být použita jako zástupný znak. Vyprázdnit všechny složky, podsložky a soubory v koncovém bodě s `/*` hello cestu nebo vyprázdnit všechny podsložky a soubory v konkrétní složce, a to zadáním hello složky následuje `/*`, například`/pictures/*`.  Všimněte si, že zástupné vyprázdnění nepodporuje Azure CDN společnosti Akamai aktuálně. 
    3. **Kořenové domény vyprázdnění**: kořenové hello vyprázdnění koncového bodu hello s "/" v cestě hello.
   
   > [!TIP]
   > Cesty pro vyprázdnění musí být zadána a musí být relativní adresa URL, který umístit hello následující [regulární výraz](https://msdn.microsoft.com/library/az24scfc.aspx). **Vyprázdnit všechny** a **zástupné vyprázdnění** nepodporuje **Azure CDN společnosti Akamai** aktuálně.
   > > Vyprázdnění jedné adresy URL`@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`  
   > > Řetězec dotazu`@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`  
   > > Zástupné vyprázdnění `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`. 
   > 
   > Další **cesta** textová pole se zobrazí, když zadáte text tooallow toobuild seznam více prostředků.  Prostředky můžete odstranit ze seznamu hello kliknutím na tlačítko se třemi tečkami (...) hello.
   > 
5. Klikněte na tlačítko hello **mazání** tlačítko.
   
    ![Vyprázdnění tlačítko](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> Vyprázdnění požadavky trvat přibližně 2 – 3 minuty tooprocess s **Azure CDN společnosti Verizon** (Standard a Premium) a přibližně 7 minut s **Azure CDN společnosti Akamai**.  Limit souběžných 50 mazání požadavky v daném okamžiku je Azure CDN. 
> 
> 

## <a name="see-also"></a>Viz také
* [Předběžné načtení prostředků v koncovém bodu Azure CDN](cdn-preload-endpoint.md)
* [Azure referenční dokumentace rozhraní API REST CDN - mazání nebo předběžné načtení koncový bod](https://msdn.microsoft.com/library/mt634451.aspx)

