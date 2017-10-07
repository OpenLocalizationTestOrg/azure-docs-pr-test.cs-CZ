---
title: "data XML aaaConvert s transformací - Azure Logic Apps | Microsoft Docs"
description: "Vytvořit transformací nebo mapps data XML tooconvert mezi formáty v aplikacích logiky pomocí hello Enterprise integraci sady SDK"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: b56ec1072c5058d3aefc7f88ac9b2748ebe56456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-integration-with-xml-transforms"></a>Enterprise integrace s transformace XML
## <a name="overview"></a>Přehled
Hello Enterprise integrace transformace konektor převádí data z jednoho formátu tooanother formátu. Například můžete mít příchozí zprávy, která obsahuje aktuální datum ve formátu YearMonthDay hello hello. Můžete vytvořit toobe transformace tooreformat hello datum ve formátu MonthDayYear hello.

## <a name="what-does-a-transform-do"></a>Transformace k čemu slouží?
Transformace, která je také označována jako mapu, se skládá z schéma XML pro zdroj (hello vstup) a cíl XML schema (výstup hello). Různé integrované funkce toohelp pracovat s nebo řízení hello data, můžete použít, včetně manipulace s řetězci, podmíněného přiřazení, aritmetických výrazech, datum čas formátování a i opakování ve smyčce konstrukce.

## <a name="how-toocreate-a-transform"></a>Jak toocreate transformace?
Transformace nebo mapu můžete vytvořit pomocí sady Visual Studio hello [Enterprise integrace SDK](https://aka.ms/vsmapsandschemas). Po dokončení vytváření a testování hello transformace nahrajete do účtu integrace hello transformace. 

## <a name="how-toouse-a-transform"></a>Jak toouse transformace
Po odeslání hello transformace/mapovat do účtu integrace, můžete ji použít toocreate aplikace logiky. aplikace logiky Hello běží vaše transformace vždy, když se aktivuje aplikaci logiky hello (a vstupní obsah, který potřebuje toobe Transformovat).

**Tady jsou kroky toouse hello transformace**:

### <a name="prerequisites"></a>Požadavky

* Vytvoření účtu integrace a přidejte tooit mapy  

Teď, když jste postaráno, hello požadavky, je čas toocreate svou aplikaci logiky:  

1. Vytvoření aplikace logiky a [propojit účet integrace tooyour](../logic-apps/logic-apps-enterprise-integration-accounts.md "další toolink aplikace logiky integrace účet tooa") obsahující hello mapy.
2. Přidat **požadavku** aplikace logiky tooyour aktivační události  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. Přidat hello **transformace XML** akce první výběrem **přidat akci**   
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. Zadejte slova hello *transformace* v toofilter pole hledání hello všechny akce toohello, jednu, které chcete toouse hello  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. Vyberte hello **transformace XML** akce   
6. Přidat hello XML **obsahu** , můžete transformace. Můžete použít libovolná data XML, který se zobrazí v žádosti o hello HTTP jako hello **obsahu**. V tomto příkladu vyberte hello textu hello požadavku protokolu HTTP, který aktivoval aplikace logiky hello.
7. Vyberte hello název hello **MAPY** , které chcete toouse tooperform hello transformace. Mapa Hello již musí být v účtu integrace. V předchozím kroku dáte již vaše logiku aplikace přístup tooyour integrace účet, který obsahuje mapu.      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. Uložte si práci  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

V tomto okamžiku jste dokončili nastavení mapy. V reálné aplikaci můžete chtít toostore hello transformovat data obchodní aplikace, například služby SalesForce. Můžete snadno jako výstup hello toosend akce hello transformace tooSalesforce. 

Nyní můžete otestovat váš transformace tak, že koncový bod žádosti toohello protokolu HTTP.  

## <a name="features-and-use-cases"></a>Funkce a případy použití
* Transformace Hello vytvořené v mapu může být jednoduchý, jako je například kopírování názvu a adresy z jednoho tooanother dokumentu. Nebo můžete vytvořit složitější transformace pomocí operace hello se na pole mapy.  
* Více mapy operace nebo funkce jsou snadno dostupné, včetně řetězce, datum časové funkce a tak dále.  
* Můžete to udělat kopii přímá dat mezi hello schémat. V hello Mapper součástí hello SDK to je jednoduché, kreslení čáry propojující hello elementy ve schématu zdroje hello se svými protějšky ve schématu cílové hello.  
* Při vytváření mapy, zobrazí grafické reprezentace hello map, který zobrazuje všechny vztahy hello a odkazy, které vytvoříte.
* Použijte hello testovací mapy funkce tooadd ukázkovou zprávu XML. S jediným kliknutím můžete otestovat hello mapy jste vytvořili a výstup hello vygenerovat.  
* Nahrát existující mapy  
* Zahrnuje podporu pro formát XML hello.

## <a name="learn-more"></a>Další informace
* [Další informace o hello Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")  
* [Další informace o mapování](../logic-apps/logic-apps-enterprise-integration-maps.md "Další informace o enterprise integrace mapy")  

