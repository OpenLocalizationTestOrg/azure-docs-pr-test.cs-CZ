---
title: "(nepoužívané) Časté otázky – publikování a používání aplikace Machine Learning v Azure Marketplace | Microsoft Docs"
description: "(nepoužívané) Časté otázky k publikování aplikací Machine Learning v Azure Marketplace"
services: machine-learning
documentationcenter: 
author: bharaths
manager: jhubbard
editor: cgronlun
ms.assetid: 26b3a1e7-8b9a-4004-98bc-17456d4c25e8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: a4631dfeb2f817b3a3c612a8f6af48398e4f2ab9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-publishing-and-using-machine-learning-apps-in-the-azure-marketplace-faq"></a>(nepoužívané) Publikování a použití aplikace Machine Learning v Azure Marketplace: Nejčastější dotazy

> [!NOTE]
> DataMarket a Data služby se postupně vyřazuje z provozu a odběry vyřadí a zrušena od 3/31/2017. V důsledku toho je zastaralá v tomto článku. 
> 
> Jako alternativu, můžete publikovat Machine Learning experimentů k [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) prospěch komunity vědecké účely data. Další informace najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).


## <a name="questions-about-consuming-from-marketplace"></a>Dotazy týkající se použití z webu Marketplace
**1. Proč se po zadání vstupu pro webovou službu zobrazí následující chybová zpráva:**

**Žádost byla vygenerována v back-end časového limitu nebo chyby back-end. Tým pracuje na odstranění příčin problému. Omlouváme se za nepříjemnosti se. (500)**

Vstupní parametry nemusí vyhovovat požadovaný formát pro konkrétní webovou službu. Podrobnosti naleznete na odpovídající odkaz dokumentaci najít správný formát pro vstupní parametry a omezení této webové služby.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**2. Pokud lze zkopírovat odkaz na rozhraní API pro webovou službu, která je na stránky "Prozkoumat tuto datovou sadu" a vložíte ho do jiného okna prohlížeče, jaké přihlašovací údaje by je možné používat pro přístup k výsledky a jak je zobrazit?**

Účtu webu Marketplace má použít jako uživatelské jméno a primární účet klíč jako heslo. Klíč primární účtu naleznete na **prozkoumat tuto datovou sadu** stránky v části Popis webové služby (klikněte na tlačítko **zobrazit** tlačítko). Výsledek se může zobrazovat v prohlížeči nebo může být k dispozici ke stažení, v závislosti na tom, které prohlížeče, kterou používáte.

**3. Proč se po zadání vstupu pro webovou službu na stránce "Prozkoumat tuto datovou sadu" zobrazí následující chybová zpráva:** 

**Při zpracování vaší žádosti došlo k neočekávané chybě. Zkuste to prosím znovu.**

Jeden nebo více vstupních parametrů webové služby byl překročen limit délky při využívání webovou službu na webu marketplace **prozkoumat tuto datovou sadu** stránky. Službu nelze volat s již vstupní délkou pomocí metod HTTP POST. Příklady najdete v tématu [ukázkové řešení pomocí R na Machine Learning a publikované na webu Marketplace](machine-learning-r-csharp-web-service-examples.md).

**4. Proč nevidím v kartě int "API EXPLORER" úložiště na portálu Azure Classic nic?** 

Jedná se o známý problém s klasického portálu Azure Marketplace. Tým pracuje k vyřešení tohoto problému. 

## <a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a>Dotazy o publikování z Azure Machine Learning v Marketplace.
**1. Proč nejsou Moje transakce loga nebo bitové kopie aktualizace pro webovou službu?** 

Loga a image jsou uložené v mezipaměti v portálu pro publikování a že může trvat až 10 dnů pro nové logo nebo bitovou kopii k aktualizaci na portálu.

**2. Proč je na kartě "Podrobné" webovou službu na Marketplace zobrazuje chybovou zprávu?**

Při připojování k Azure Machine Learning podrobnosti služby není známý problém Marketplace. Tým pracuje k vyřešení tohoto problému.

**3. Proč ukázkový kód R ve webové služby Azure Machine Learning nefunguje pro využívání webových služeb v Marketplace?**

Systémy ověřování se liší, při připojení k webovým službám Azure Machine Learning přímo ve srovnání s připojení k webovým službám přes Marketplace. Služeb v Marketplace jsou služby OData a volat pomocí metody GET nebo POST. 

**4. Proč se podporu odkazů Moje webová služba nabízí není správně aktualizace pro některé Moje nabízí?**

Globální za vydavatele, není za nabídka jsou odkazy na podporu. 

**5. Jak lze publikovat webové služby pomocí vstupní režimu dávky v Marketplace?**

Vstupní režim batch není aktuálně podporován v Marketplace webové služby.

**6. Kdo je měli kontaktovat nápovědu, pokud je nutné dotazy týkající se stává stále vydavatel dat, nebo pokud mám problémy při publikování?**

Kontaktujte tým služby Azure Marketplace v < mailto:datamarketbd@microsoft.com > Další informace.

