---
title: "aaaControl chování ukládání do mezipaměti Azure CDN řetězce dotazu | Microsoft Docs"
description: "Azure CDN při ukládání řetězců dotazů ovládací prvky, jak jsou soubory toobe do mezipaměti, které obsahují řetězce dotazu."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e7a138b2decec624a29eb703ad9a291d19c44ee8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a>Ovládací prvek Azure CDN ukládání do mezipaměti chování řetězce dotazu
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium od společnosti Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Přehled
Ovládací prvky, jak jsou soubory v mezipaměti, které obsahují řetězce dotazu toobe ukládání řetězců s dotazy.

> [!IMPORTANT]
> Hello Standard a Premium CDN produkty poskytují hello stejné funkce ukládání do mezipaměti řetězce dotazu, ale liší se hello uživatelské rozhraní.  Tento dokument popisuje hello rozhraní pro **Azure CDN Standard od společnosti Akamai** a **Azure CDN Standard od společnosti Verizon**.  Pro dotaz řetězec ukládání do mezipaměti s **Azure CDN Premium od společnosti Verizon**, najdete v části [řízení chování ukládání do mezipaměti CDN požadavky s řetězci dotazů - Premium](cdn-query-string-premium.md).
> 
> 

K dispozici jsou tři režimy:

* **Ignorovat řetězce dotazů**: Toto je výchozí režim hello.  Hello CDN hraniční uzel, projdou hello řetězec dotazu z počátku toohello hello žadatele na první požadavek hello a mezipaměti hello asset.  Všechny následné žádosti pro tento prostředek, které se zpracovávají z uzlu edge hello bude ignorovat hello řetězec dotazu do vypršení platnosti mezipaměti asset hello.
* **Nepoužívat ukládání do mezipaměti pro URL s řetězci dotazů**: V tomto režimu žádostí s řetězci dotazu se neukládají do mezipaměti v uzlu edge hello CDN.  Hello hraniční uzel načte hello asset přímo z počátku hello a předává je toohello žadatele s každou žádostí.
* **Ukládat do mezipaměti každou jedinečnou adresu URL**: Tento režim považuje za jedinečný prostředek s vlastní mezipaměti každou žádost s řetězec dotazu.  Například hello odpověď z počátku hello pro žádost o *foo.ashx?q=bar* by uloží do mezipaměti na hello hraniční uzel a vrátit pro následné mezipaměti s Tento stejný řetězec dotazu.  Žádost o *foo.ashx?q=somethingelse* by do mezipaměti jako samostatné asset s vlastním toolive časem.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Změna nastavení pro standardní profily CDN ukládání řetězců s dotazy
1. Z hello okno profil CDN klikněte na koncový bod CDN hello chcete toomanage.
   
    ![Koncové body okno profil CDN](./media/cdn-query-string/cdn-endpoints.png)
   
    Otevře se okno koncový bod CDN Hello.
2. Klikněte na tlačítko hello **konfigurace** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-query-string/cdn-config-btn.png)
   
    Otevře se okno Konfigurace CDN Hello.
3. Vyberte nastavení hello **chování ukládání řetězců s dotazy** rozevíracího seznamu.
   
    ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string/cdn-query-string.png)
4. Po provedení váš výběr, klikněte na hello **Uložit** tlačítko.

> [!IMPORTANT]
> změny nastavení Hello nemusí být okamžitě viditelné, jak dlouho trvá dobu hello registrace toopropagate prostřednictvím hello CDN.  V případě profilů <b>Azure CDN společnosti Akamai</b> je šíření obvykle hotové během jedné minuty.  V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.
> 
> 

