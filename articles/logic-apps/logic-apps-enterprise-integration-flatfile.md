---
title: "aaaEncode nebo dekódování plochých souborů v Azure logic apps | Microsoft Docs"
description: "Jak toouse hello souboru souboru kodér nebo dekodér v hello Enterprise integrační balíček ve vašich logic apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 82152dab-c7ad-43df-b721-596559703be8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 2c295586625fd84366ec7cbafdcebf0489ba234d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-enterprise-integration-with-flat-files"></a>Přehled integrace enterprise s plochých souborů

Můžete chtít tooencode XML obsahu před jejím odesláním tooa obchodním partnerům ve scénáři business-to-business (B2B). V aplikaci logiky, aplikaci můžete hello plochý soubor kódování konektor toodo to. Hello aplikaci logiky, kterou vytvoříte může získat jeho XML obsah z různých zdrojů, včetně aktivační procedury pro požadavek HTTP, z jiné aplikace nebo dokonce od hello mnoha [konektory](../connectors/apis-list.md). Další informace o aplikacích logiky najdete v tématu hello [dokumentaci aplikace logiky](logic-apps-what-are-logic-apps.md "Další informace o aplikacích logiky").  

## <a name="create-hello-flat-file-encoding-connector"></a>Vytvořit konektor kódování hello plochý soubor
Postupujte podle těchto kroků tooadd plochý soubor kódování aplikace logiky tooyour konektor.

1. Vytvoření aplikace logiky a [propojit účet integrace tooyour](logic-apps-enterprise-integration-accounts.md "další toolink aplikace logiky integrace účet tooa"). Tento účet obsahuje schéma hello použijete tooencode hello XML data.  
2. Přidat **požadavku - obdrží žádost HTTP při** aplikace logiky tooyour aktivační události.  
   ![Snímek obrazovky tooselect aktivační události](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
3. Přidejte hello plochý soubor kódování akce, následujícím způsobem:
   
    a. Vyberte hello **plus** přihlášení.
   
    b. Vyberte hello **přidat akci** odkaz (se zobrazí po výběru znaménko plus hello).
   
    c. Hello vyhledávacího pole zadejte *ploché* toofilter všechny akce toohello, jednu, které chcete toouse hello.
   
    d. Vyberte hello **ploché kódování souborů** možnost ze seznamu hello.   
   ![Možnost snímek z plochých kódování souborů](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
4. Na hello **ploché kódování souborů** dialogové okno, vyberte hello **obsahu** textové pole.  
   ![Snímek obrazovky obsahu textového pole](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)  
5. Vyberte značku textu hello jako obsahu, který má tooencode hello. značka textu Hello naplní pole obsahu hello.     
   ![Snímek obrazovky značky textu](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  
6. Vyberte hello **název schématu** seznamu pole a zvolte hello schématu chcete toouse tooencode hello vstupní obsah.    
   ![Pole se seznamem název schématu snímek obrazovky](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)  
7. Uložte práci.   
   ![Snímek obrazovky uložit ikonu](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

V tomto okamžiku jste dokončili nastavení kódování konektor vaše plochý soubor. V reálné aplikaci můžete data toostore hello kódovaný v-obchodní aplikaci, například služby Salesforce. Nebo můžete odeslat tuto kódovaného data tooa obchodního partnera. Můžete snadno přidat výstup hello toosend akce hello kódování tooSalesforce akce nebo tooyour obchodního partnera, pomocí některého z hello ostatní konektory zadat.

Nyní můžete otestovat váš konektor tím, že koncový bod žádosti toohello HTTP a včetně obsah XML hello v textu hello hello požadavku.  

## <a name="create-hello-flat-file-decoding-connector"></a>Vytvořit plochý soubor hello dekódování konektoru

> [!NOTE]
> toocomplete tyto kroky, musíte toohave soubor schématu již odeslány do integrace účtu.

1. Přidat **požadavku - obdrží žádost HTTP při** aplikace logiky tooyour aktivační události.  
   ![Snímek obrazovky tooselect aktivační události](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
2. Přidejte hello plochý soubor dekódování akce, následujícím způsobem:
   
    a. Vyberte hello **plus** přihlášení.
   
    b. Vyberte hello **přidat akci** odkaz (se zobrazí po výběru znaménko plus hello).
   
    c. Hello vyhledávacího pole zadejte *ploché* toofilter všechny akce toohello, jednu, které chcete toouse hello.
   
    d. Vyberte hello **plochý soubor dekódování** možnost ze seznamu hello.   
   ![Snímek obrazovky z plochých dekódování souboru možnost](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
3. Vyberte hello **obsahu** ovládacího prvku. Tímto se vytvoří seznam hello obsah z dřívějších krocích, které můžete použít jako obsahu toodecode hello. Všimněte si, že hello *textu* z hello příchozí požadavek HTTP je k dispozici toobe použít jako hello toodecode obsahu. Můžete také zadat hello obsahu toodecode přímo do hello **obsah** ovládacího prvku.     
4. Vyberte hello *textu* značky. Značka textu hello oznámení je nyní v hello **obsahu** ovládacího prvku.
5. Vyberte název hello hello schématu, které chcete toouse toodecode hello obsah. Hello následující snímek obrazovky ukazuje, že *OrderFile* je název vybrané schéma hello. Tento název schématu měl nahraný v úvahu integrace hello dříve.
   
   ![Dialogové okno snímek obrazovky z plochých dekódování souboru](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png)    
6. Uložte práci.  
   ![Snímek obrazovky uložit ikonu](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

V tomto okamžiku jste dokončili nastavení plochého souboru dekódování konektor. V reálné aplikaci můžete data hello dekódovat toostore-obchodní aplikaci, například služby Salesforce. Můžete snadno přidat výstup hello toosend akce hello dekódování tooSalesforce akce.

Nyní můžete otestovat váš konektor tím, že koncový bod žádosti toohello HTTP a včetně obsah XML hello chcete toodecode v textu hello hello požadavku.  

## <a name="next-steps"></a>Další kroky
* [Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku").  

