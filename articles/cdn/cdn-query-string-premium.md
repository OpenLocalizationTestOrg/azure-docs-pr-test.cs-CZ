---
title: "aaaControl chování ukládání do mezipaměti Azure CDN s řetězci dotazů - Premium | Microsoft Docs"
description: "Azure CDN při ukládání řetězců dotazů ovládací prvky, jak jsou soubory toobe do mezipaměti, které obsahují řetězce dotazu."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5c97cf0230ae13fd378bfce49481f3135a5ef101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a>Ovládací prvek Azure CDN ukládání do mezipaměti chování řetězce dotazu - Premium
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium od společnosti Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Přehled
Ovládací prvky, jak jsou soubory v mezipaměti, které obsahují řetězce dotazu toobe ukládání řetězců s dotazy.

> [!IMPORTANT]
> Hello Standard a Premium CDN produkty poskytují hello stejné funkce ukládání do mezipaměti řetězce dotazu, ale liší se hello uživatelské rozhraní.  Tento dokument popisuje hello rozhraní pro **Azure CDN Premium od společnosti Verizon**.  Pro dotaz řetězec ukládání do mezipaměti s **Azure CDN Standard od společnosti Akamai** a **Azure CDN Standard od společnosti Verizon**, najdete v části [řízení chování ukládání do mezipaměti CDN požadavky s řetězci dotazů](cdn-query-string.md).
> 
> 

K dispozici jsou tři režimy:

* **Standardní mezipaměti**: Toto je výchozí režim hello.  Hello CDN hraniční uzel, projdou hello řetězec dotazu z počátku toohello hello žadatele na první požadavek hello a mezipaměti hello asset.  Všechny následné žádosti pro tento prostředek, které se zpracovávají z uzlu edge hello bude ignorovat hello řetězec dotazu do vypršení platnosti mezipaměti asset hello.
* **Ne mezipaměti**: V tomto režimu žádostí s řetězci dotazu se neukládají do mezipaměti v uzlu edge hello CDN.  Hello hraniční uzel načte hello asset přímo z počátku hello a předává je toohello žadatele s každou žádostí.
* **Jedinečný mezipaměti**: Tento režim považuje za jedinečný prostředek s vlastní mezipaměti každou žádost s řetězec dotazu.  Například hello odpověď z počátku hello pro žádost o *foo.ashx?q=bar* by uloží do mezipaměti na hello hraniční uzel a vrátit pro následné mezipaměti s Tento stejný řetězec dotazu.  Žádost o *foo.ashx?q=somethingelse* by do mezipaměti jako samostatné asset s vlastním toolive časem.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Změna nastavení pro profily CDN premium ukládání řetězců s dotazy
1. Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    Otevře portál pro správu Hello CDN.
2. Hover přes hello **HTTP velké** kartu a potom hover přes hello **nastavení mezipaměti** plovoucím panelem.  Klikněte na **ukládání do mezipaměti řetězce dotazu**.
   
    Zobrazí se možnosti ukládání do mezipaměti řetězce dotazu.
   
    ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string-premium/cdn-query-string.png)
3. Po provedení váš výběr, klikněte na hello **aktualizace** tlačítko.

> [!IMPORTANT]
> změny nastavení Hello nemusí být okamžitě viditelné, jak dlouho trvá dobu hello registrace toopropagate prostřednictvím hello CDN.  V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.
> 
> 

