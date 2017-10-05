---
title: "Nastavení a použití Machine Learning API doporučení | Microsoft Docs"
description: "Rozhraní API služby doporučení Microsoft vytvořené s nástroji Azure Machine Learning – nejčastější dotazy"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 1ffc3c16-e040-4225-9d72-105129938dfa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 3851589818bb8f4309bf3c65f17b115e0dcd27fa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Nejčastější dotazy k nastavení a používání rozhraní Machine Learning Recommendations API
**Co je doporučení?**

> [!NOTE]
> Měli byste začít používat službu doporučení rozhraní API kognitivní místo tuto verzi. Službu kognitivní doporučení budou nahrazení této služby, a všechny nové funkce bude vyvinutý existuje. Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.
> Další informace o [migraci na novou službu kognitivní](http://aka.ms/recomigrate)
> 
> 

Pro organizace a firmy, které jsou závislé na doporučení, která prodává mezi a až prodává produktů a služeb zákazníkům doporučení v Azure Machine Learning poskytuje modul Samoobslužné služby doporučení. Je implementace spolupráce filtrování, která používá matice factorization jako algoritmus jádra. Vývojáři aplikace přístup pomocí rozhraní REST API doporučení. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Co můžete dělat s DOPORUČENÍMI?**

DOPORUČENÍ používá jako vstup některou položku nebo sadu položek a vrátí seznam hodnot příslušná doporučení. Příklad: zákazník online prodejce klikne produktu. Online prodejce odešle produktu jako vstup pro doporučení, získá seznam produktů na oplátku a rozhodne, která z těchto produktů se zobrazí na zákazníka. Můžete chtít použít doporučení k optimalizaci online úložiště nebo i k informování vaší uvnitř prodejní oddělení nebo volání center.

**Existují nějaká omezení využití?**

Doporučení má následující omezení využití:

* Maximální počet modelů podle předplatného: 10
* Maximální počet položek, které mohou být uloženy katalog: 100 000
* Maximální počet bodů využití, které jsou zachovány je ~ 5 000 000. Nejstarší budou odstraněna, pokud nové se nahrál nebo nahlásí.
* Maximální velikost dat, který lze poslat e-mailem (například import katalogu dat, data o využití import) je 200 MB
* Počet transakcí za sekundu (TPS) pro sestavení modelu doporučení, která není aktivní je TPS ~ 2. Až 20 TPS mohou být uloženy sestavení modelu doporučení, která je aktivní.

## <a name="purchase-and-billing"></a>Nákup a fakturace
**Kolik stojí doporučení v době spuštění?**

Doporučení je služba na základě předplatného. Ukládání je založena na objemu transakce za měsíc. Můžete zkontrolovat [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations) v Microsoft Azure Marketplace informace o cenách.

**Existují veškerých nákladech spojených s nutnosti doporučení sledovat a uložit aktivity uživatelů pro mě nejlepší?**

Není v tuto chvíli.

**Má doporučení k bezplatné zkušební verzi?**

Není k dispozici bezplatná záznam, který je omezen na 10 000 transakce za měsíc.

**Pokud bude I účtovány poplatky za doporučení?**

Placené předplatné je libovolné předplatné, pro kterou je měsíční poplatek. Pokud si koupíte předplatné, jsou okamžitě účtovat pro použití v prvním měsíci. Budou se účtovat množství, které souvisí s nabídky na stránku odběru služby (a příslušné daně). Dokud zrušit předplatné, provádí tento měsíční poplatek za každý měsíc na stejné datum kalendáře jako původního nákupu. 

**Jak upgradovat na vyšší úroveň služby?**

Můžete koupit nebo aktualizovat vaše předplatné z [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations) stránky na webu Microsoft Azure Marketplace.

Při upgradu předplatné:

* Transakce, které jsou zbývající u původního předplatného nejsou přidány do nového předplatného. 
* Platíte úplné ceny pro nové předplatné, i když můžete mít nepoužívané transakcí u původního předplatného.

Proces upgradu předplatné:

* Nevigate k [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations).
* Pokud už nejste přihlášení, přihlaste se k webu Marketplace.
* V pravém podokně jsou uvedeny všechny dostupné plány. Klikněte na přepínač pro plán, který chcete upgradovat.
* Pokud chcete upgradovat, klikněte na tlačítko **OK**. Pokud nechcete upgradovat, klikněte na tlačítko **zrušit**.

**Důležité** pečlivě přečtěte dialogové okno před provedením upgradu, protože nejsou k dispozici dopady na fakturaci a použití.

**Když se Moje předplatné doporučení ukončí?**

Vaše předplatné se ukončí, pokud ho zrušit. Pokud chcete zrušit předplatné, přečtěte si následující pokyny.

**Jak zruším Moje předplatné doporučení?**

Pokud chcete zrušit předplatné, použijte následující postup. Pokud je vaše aktuální předplatné na placené předplatné, pokračuje vaše předplatné platí až do konce aktuální fakturačního období. Pokud potřebujete zrušení být účinné okamžitě, obraťte se na nás na adrese [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Poznámka:** je zadána žádná náhrada, pokud zrušíte do konce fakturačního období nebo nepoužívané transakcí v fakturačního období.

* Přejděte na [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations).
* Pokud už nejste přihlášení, přihlaste se k webu Marketplace.
* Klikněte na tlačítko **zrušit** vpravo od názvu datové sady a stav. Můžete použít tento odběr až do konce fakturačního období aktuální nebo dosažení limit transakcí (cokoliv nastane dříve).

Pokud chcete zrušit předplatné, okamžitě, takže si můžete zakoupit nové předplatné, soubor lístek v [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

## <a name="getting-started-with-recommendations"></a>Začínáme s doporučeními
**Doporučení je pro mě nejlepší?** 

Doporučení v Machine Learning je pro organizace a firmy, které jsou závislé na doporučení, která prodává mezi a až prodává produktech či službách zákazníkům. Pokud máte web zákazníka přístupem, obchodníci, vnitřní obchodníci nebo volání centra, a Pokud nabízíte katalog více než několik desítek produktů a služeb, do dolní řádku může využívat doporučení. 

Experimentování se doporučení je určena pro být celkem jednoduché. Aktuální verze založené na rozhraní API vyžaduje základní znalosti programování. Pokud potřebujete pomoc, obraťte se na dodavatele, který vyvinul vašeho webu. Pokud máte interní IT oddělení nebo interní vývojáře, že byste měli mít získat doporučení, jak pro vás. 

**Jaké jsou požadavky pro nastavení doporučení?**

Doporučení vyžaduje, abyste měli protokolu volby uživatele, protože se týká do katalogu. Pokud nemáte takové protokolu a máte weby zákazníka, můžete doporučení shromáždit aktivity uživatelů za vás. 

Doporučení jsou taky vyžaduje katalog produktů a služeb. Pokud nemáte katalogu, můžete doporučení použít dat o využití skutečnou zákazníků a generovat katalog. Předpokládané katalogu nebude zahrnovat položky, které nejsou hlášeny v rámci uživatelské transakce.

**Jak nastavit doporučení poprvé?**

Po [odběru](https://datamarket.azure.com/dataset/amla/recommendations) na doporučení, měli byste použít v dokumentaci rozhraní API [Azure Machine Learning doporučení - úvodní příručce](machine-learning-recommendation-api-quick-start-guide.md) nastavit službu.

**Kde najdu dokumentaci k rozhraní API?** 

Dokumentace rozhraní API je [Azure Machine Learning doporučení-úvodní příručce](machine-learning-recommendation-api-quick-start-guide.md).

**Jaké možnosti jsou odeslání katalogu a využití dat do doporučení?**

Máte dvě možnosti pro odeslání vaše data o využití a katalogu: můžete exportovat data z systému CRM nebo jiné protokoly a nahrajte ho do doporučení, nebo můžete přidat značky na váš web, který bude sledovat aktivity uživatele. Pokud používáte metodu druhé, data se uloží v Azure.

## <a name="maintenance-and-support"></a>Údržba a podpora
**Jak velká může Moje datové sady být?**

Každá sada dat může obsahovat až 100 000 položek katalogu a až na 2 048 MB dat o využití.
Kromě toho předplatné může obsahovat až 10 sady dat (modelů).

**Kde lze získat technickou podporu pro doporučení?**

Technická podpora je k dispozici na [podporu Microsoft Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) lokality.

**Kde podmínky použití**

[Microsoft Azure Machine Learning podmínky doporučení rozhraní API služby](https://datamarket.azure.com/dataset/amla/recommendations#terms).

