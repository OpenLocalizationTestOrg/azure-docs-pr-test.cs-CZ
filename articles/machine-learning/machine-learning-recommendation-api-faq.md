---
title: "aaaSet nahoru a používání hello Machine Learning doporučení API | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 980bf1a36f3291275d9ef0fee9b4446f7e0cbecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Nejčastější dotazy k nastavení a používání rozhraní Machine Learning Recommendations API
**Co je doporučení?**

> [!NOTE]
> Měli byste začít používat hello kognitivní služby API doporučení místo tuto verzi. Hello kognitivní službu doporučení budou nahrazení této služby, a všechny nové funkce hello bude vyvinutý existuje. Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.
> Další informace o [toohello migrace nové kognitivní služby](http://aka.ms/recomigrate)
> 
> 

Pro organizace a firmy, které jsou závislé na doporučení toocross prodává a až prodává produktů a služeb tootheir zákazníků doporučení v Azure Machine Learning poskytuje modul Samoobslužné služby doporučení. Je implementace spolupráce filtrování, která používá matice factorization jako algoritmus jádra. Vývojáři aplikace přístup pomocí rozhraní REST API doporučení. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Co můžete dělat s DOPORUČENÍMI?**

DOPORUČENÍ používá jako vstup některou položku nebo sadu položek a vrátí seznam hodnot příslušná doporučení. Příklad: zákazník online prodejce klikne produktu. online prodejce Hello odešle jako vstupní tooRECOMMENDATIONS produktu, získá seznam produktů na oplátku a rozhodne, která z těchto produktů se zobrazí toohello zákazníka. Můžete chtít toouse doporučení toooptimize online úložiště nebo dokonce i tooinform vaše uvnitř prodejní oddělení nebo volání center.

**Existují nějaká omezení využití?**

Doporučení má následující omezení využití hello:

* Maximální počet modelů podle předplatného: 10
* Maximální počet položek, které mohou být uloženy katalog: 100 000
* maximální počet bodů využití, které jsou zachovány Hello je ~ 5 000 000. Pokud nové se nahrál nebo nahlásí se odstraní nejstarší Hello.
* Maximální velikost dat, který lze poslat e-mailem (například import katalogu dat, data o využití import) je 200 MB
* Počet transakcí za sekundu (TPS) pro sestavení modelu doporučení, která není aktivní je TPS ~ 2. Až too20 TPS mohou být uloženy sestavení modelu doporučení, která je aktivní.

## <a name="purchase-and-billing"></a>Nákup a fakturace
**Kolik doporučení náklady během doby spuštění hello?**

Doporučení je služba na základě předplatného. Ukládání je založena na objemu transakce za měsíc. Můžete zkontrolovat hello [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations) v Microsoft Azure Marketplace informace o cenách.

**Existují veškerých nákladech spojených s nutnosti doporučení sledovat a uložit aktivity uživatelů pro mě nejlepší?**

Není momentálně hello.

**Má doporučení k bezplatné zkušební verzi?**

Není k dispozici bezplatná záznam, který je s omezeným přístupem too10 000 transakce za měsíc.

**Pokud bude I účtovány poplatky za doporučení?**

Placené předplatné je libovolné předplatné, pro kterou je měsíční poplatek. Pokud si koupíte předplatné, okamžitě účtujeme pro hello nejprve použijte měsíci. Budou se účtovat hello množství, které souvisí s hello nabídky na stránku odběru hello (a příslušné daně). Tento měsíční poplatek je probíhají každý měsíc hello stejná data jako původního nákupu kalendář až do zrušení předplatného hello. 

**Jak mohu provést upgrade tooa vyšší úroveň služby?**

Můžete koupit nebo aktualizovat vaše předplatné z hello [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations) stránky na webu Microsoft Azure Marketplace.

Při upgradu předplatné:

* Transakce, které jsou zbývající u původního předplatného nebyly přidány tooyour nové předplatné. 
* Platíte úplné ceny hello nové předplatné, i když můžete mít nepoužívané transakcí u původního předplatného.

Proces tooupgrade předplatné:

* Nevigate toohello [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations).
* Pokud už nejste přihlášení, přihlaste toohello Marketplace.
* V pravém podokně hello jsou uvedeny všechny dostupné plány hello. Klikněte na přepínač hello pro hello plánu, který chcete tooupgrade k.
* Pokud chcete tooupgrade, klikněte na tlačítko **OK**. Pokud nechcete, aby tooupgrade, klikněte na tlačítko **zrušit**.

**Důležité** pečlivě čtení hello dialogové okno před provedením upgradu, protože nejsou k dispozici dopady na fakturaci a použití.

**Když se ukončí tooRecommendations Moje předplatné?**

Vaše předplatné se ukončí, pokud ho zrušit. Pokud chcete toocancel vašich předplatných, najdete v části hello pokynů.

**Jak zruším Moje předplatné doporučení?**

toocancel, které vaše předplatné hello použijte následující kroky. Pokud je vaše aktuální předplatné na placené předplatné, vaše předplatné pokračuje v platnosti až hello konce hello aktuální fakturační období. Pokud třeba hello zrušení toobe efektivní okamžitě, obraťte se na nás na adrese [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Poznámka:** je zadána žádná náhrada, pokud zrušíte hello konce fakturačního období nebo nepoužívané transakcí v fakturačního období.

* Přejděte toohello [nabízejí stránky](https://datamarket.azure.com/dataset/amla/recommendations).
* Pokud už nejste přihlášení, přihlaste toohello Marketplace.
* Klikněte na tlačítko **zrušit** toohello napravo od hello název datové sady a stav. Toto předplatné můžete používat, dokud je dosaženo hello konce hello aktuální fakturační období nebo limit transakcí (cokoliv nastane dříve).

Pokud chcete toocancel vaše předplatné, okamžitě, takže si můžete zakoupit nové předplatné, soubor lístek v [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

## <a name="getting-started-with-recommendations"></a>Začínáme s doporučeními
**Doporučení je pro mě nejlepší?** 

Doporučení v Machine Learning je pro organizace a firmy, které jsou závislé na doporučení toocross prodává až prodává produktech či službách tootheir zákazníky a. Pokud máte web zákazníka přístupem, obchodníci, vnitřní obchodníci nebo volání centra, a Pokud nabízíte katalog více než několik desítek produktů a služeb, do dolní řádku může využívat doporučení. 

Experimentování se doporučení je navrženou toobe docela jednoduché. aktuální verze založené na rozhraní API Hello vyžaduje základní znalosti programování. Pokud potřebujete pomoc, obraťte se na dodavatele hello, který vyvinul vašeho webu. Pokud máte interní IT oddělení nebo interní vývojáře, měly by být možné tooget toowork doporučení pro vás. 

**Jaké jsou požadavky hello k nastavení doporučení?**

Doporučení vyžaduje, abyste měli protokolu volby uživatele, protože se týká tooyour katalogu. Pokud nemáte takové protokolu a máte weby zákazníka, můžete doporučení shromáždit aktivity uživatelů za vás. 

Doporučení jsou taky vyžaduje katalog produktů a služeb. Pokud nemáte hello katalogu, můžete doporučení použít data o využití hello skutečnou zákazníků a generovat katalog. Předpokládané katalogu nebude zahrnovat položky, které nejsou hlášeny v rámci uživatelské transakce.

**Jak nastavím doporučení pro hello poprvé?**

Po [odběru](https://datamarket.azure.com/dataset/amla/recommendations) tooRecommendations, měli byste použít dokumentaci hello rozhraní API v hello [Azure Machine Learning doporučení - úvodní příručce](machine-learning-recommendation-api-quick-start-guide.md) tooset až hello služby.

**Kde najdu dokumentaci k rozhraní API?** 

Hello dokumentaci k rozhraní API je [Azure Machine Learning doporučení-úvodní příručce](machine-learning-recommendation-api-quick-start-guide.md).

**Co dělat možnosti je nutné tooRecommendations tooupload katalogu a využití dat?**

Máte dvě možnosti pro odeslání vaše data o využití a katalogu: můžete exportovat hello data ze systému CRM nebo jiné protokoly a nahrajte ho tooRecommendations, nebo můžete přidat web tooyour značky, který bude sledovat aktivity uživatele. Pokud používáte hello druhá metoda, hello data se uloží v Azure.

## <a name="maintenance-and-support"></a>Údržba a podpora
**Jak velká může Moje datové sady být?**

Každá sada dat může obsahovat až too100 000 položek katalogu a až too2048 MB dat o využití.
Kromě toho předplatné může obsahovat až too10 datové sady (modelů).

**Kde lze získat technickou podporu pro doporučení?**

Technická podpora je k dispozici na hello [podporu Microsoft Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) lokality.

**Kde hello podmínky použití**

[Microsoft Azure Machine Learning podmínky doporučení rozhraní API služby](https://datamarket.azure.com/dataset/amla/recommendations#terms).

