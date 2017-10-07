---
title: "AAA(Deprecated) časté otázky – publikování a používání aplikace Machine Learning v Azure Marketplace | Microsoft Docs"
description: "(nepoužívané) Časté otázky k publikování aplikací Machine Learning v Azure Marketplace hello"
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
redirect_document_id: True
ms.openlocfilehash: b3ae45dfff211fe9baccaf54faaf360a8309c780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-publishing-and-using-machine-learning-apps-in-hello-azure-marketplace-faq"></a>(nepoužívané) Publikování a použití aplikace Machine Learning v Azure Marketplace hello: Nejčastější dotazy

> [!NOTE]
> DataMarket a Data služby se postupně vyřazuje z provozu a odběry vyřadí a zrušena od 3/31/2017. V důsledku toho je zastaralá v tomto článku. 
> 
> Jako alternativu, můžete publikovat vaše Machine Learning experimenty toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) hello Benefit hello datové vědy komunity. Další informace najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).


## <a name="questions-about-consuming-from-marketplace"></a>Dotazy týkající se použití z webu Marketplace
**1. Proč se zobrazí následující chybová zpráva po zadání vstupu pro webovou službu hello hello:**

**výsledkem požadavku Hello back-end časového limitu nebo chyby back-end. Hello tým pracuje na odstranění příčin problému hello. Omlouváme se za nepříjemnosti hello se. (500)**

Vstupní parametry nemusí vyhovovat toohello požadovaný formát pro hello konkrétní webovou službu. Pro vstupní parametry a hello omezení této webové služby naleznete toohello odpovídající dokumentace odkaz toofind hello správný formát.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**2. Pokud lze zkopírovat odkaz hello rozhraní API pro hello webová služba, která se zobrazují na stránce "Prozkoumat tuto datovou sadu" hello a vložte ho do jiného okna prohlížeče, jaké přihlašovací údaje musí I použít tooaccess hello výsledky a jak zobrazit jejich?**

Účtu webu Marketplace má použít jako uživatelské jméno hello a hello účtu primární klíč jako heslo hello. klíč účtu primární Hello naleznete na hello **prozkoumat tuto datovou sadu** stránky v části Popis hello hello webové služby (klikněte na tlačítko hello **zobrazit** tlačítko). výsledek Hello se může zobrazovat v prohlížeči hello nebo to může být k dispozici příliš stažení, v závislosti na tom, které prohlížeče, kterou používáte.

**3. Proč se zobrazí následující chybová zpráva, když zadáte hello vstup pro webovou službu hello na stránce "Prozkoumat tuto datovou sadu" hello hello:** 

**Při zpracování vaší žádosti došlo k neočekávané chybě. Zkuste to prosím znovu.**

Jeden nebo více vstupních parametrů webové služby byl překročen limit délky hello při využívání hello webové služby na hello marketplace **prozkoumat tuto datovou sadu** stránky. Hello služby nelze volat s již vstupní délkou pomocí metod HTTP POST. Příklady najdete v tématu [ukázkové řešení pomocí R na Machine Learning a publikovaná tooMarketplace](machine-learning-r-csharp-web-service-examples.md).

**4. Proč nevidím v hello "API EXPLORER" karta int hello úložiště v hello portálu Azure Classic nic?** 

Jde o známý problém s hello Marketplace klasického portálu Azure. Hello tým pracuje tooresolve tento problém. 

## <a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a>Dotazy o publikování z Azure Machine Learning v Marketplace.
**1. Proč nejsou Moje transakce loga nebo bitové kopie aktualizace pro webovou službu?** 

Loga a image jsou uložené v mezipaměti v hello publikování portálu, a to může trvat až too10 dnů pro nové logo hello nebo tooupdate bitové kopie na portálu hello.

**2. Proč je karty "Podrobnosti" hello webovou službu na Marketplace zobrazuje chybovou zprávu?**

Při připojování tooAzure Machine Learning podrobnosti služby není známý problém Marketplace. Hello tým pracuje tooresolve tento problém.

**3. Proč hello R ukázkový kód v webové služby Azure Machine Learning hello nefunguje pro využívání hello webové služby v Marketplace?**

systémy Hello ověřování se liší, webových služeb Machine Learning tooAzure připojení přímo srovnání tooconnecting toothese webové služby pomocí hello Marketplace. Hello služeb v Marketplace jsou služby OData a volat pomocí metody GET nebo POST. 

**4. Proč se hello podporu odkazů Moje webová služba nabízí není správně aktualizace pro některé Moje nabízí?**

odkazy na podporu Hello jsou globální za vydavatele, není za nabídky. 

**5. Jak lze publikovat webové služby pomocí vstupní režimu dávky v Marketplace?**

vstupní režim Hello batch není aktuálně podporován v Marketplace webové služby.

**6. Kdo by měl I obraťte se na nápovědu tooget Pokud dotazy týkající se stává stále vydavatel dat, nebo pokud mám problémy při publikování?**

Kontaktujte prosím tým Azure Marketplace hello v < mailto:datamarketbd@microsoft.com > Další informace.

