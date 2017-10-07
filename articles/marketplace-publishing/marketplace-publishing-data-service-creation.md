---
title: "aaaGuide toocreating datové služby pro hello Marketplace | Microsoft Docs"
description: "Podrobné pokyny, jak toocreate, certifikovat a nasadit službu Data pro zakoupit na hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 96194198-6991-43b4-918e-ee337e250339
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 0220d357ae0ec89e7d4f6399605850e57c646f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-service-publishing-guide-for-hello-azure-marketplace"></a>Příručka pro publikování dat služby pro hello Azure Marketplace
> [!IMPORTANT]
> **V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.** Pokud máte obchodní aplikace SaaS chcete toopublish na AppSource najdete další informace [zde](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo služba vývojáře by jako toopublish na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Po dokončení hello kroku 1, [vytváření účtů a jejich registrace](marketplace-publishing-accounts-creation-registration.md), jsme vám na základě prostřednictvím hello [obecné netechnické](marketplace-publishing-pre-requisites.md) a [technické požadavky](marketplace-publishing-data-service-creation-prerequisites.md) z datové služby nabídku Azure Marketplace. Nyní jsme vás provede procesem hello kroky pro vytváření dat služby nabídka na hello [publikování portál] [ link-pubportal] pro hello Azure Marketplace.

## <a name="1----login-toohello-publishing-portal"></a>1.    Přihlášení toohello publikování portálu.
Přejděte příliš[https://publish.windowsazure.com](https://publish.windowsazure.com.)

**Pro první čas tooPublishing přihlášení portálu použijte hello stejný účet, pomocí kterého vaše společnost prodejce profil byl zapsaný ve středisku pro vývojáře.**  (Později můžete přidat všechny zaměstnance ve vaší společnosti jako spolusprávce v hello publikování portálu).

Klikněte na hello **publikovat Data Services** dlaždici, pokud je to první přihlášení hello hello publikování portálu.

## <a name="2----choose-data-services-in-hello-navigation-menu-on-hello-left-side"></a>2.    Zvolte **datové služby** v hello navigační nabídce na levé straně hello.
  ![Kreslení](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3----create-a-new-data-service"></a>3.    Vytvoření nové služby dat
Vyplňte hello název pro vaše nové dat služby nabízejí a klikněte na "+" na hello správné.

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4----review-hello-sub-menu-under-hello-newly-created-data-service-in-hello-navigation-menu"></a>4.    Služba Data zkontrolujte hello dílčí nabídky v části hello nově vytvořený v navigační nabídce hello.
Klikněte na hello **návod** a zkontrolujte všechny potřebné kroky potřebné toopublish správně hello služba dat na hello Azure Marketplace.

> [!TIP]
> Vždy můžete kliknutím na odkazy hello stránce "Postup" hello nebo použití karet na nabídka služby Data hello dílčí nabídky na levé straně hello.
> 
> 

## <a name="5----create-a-new-plan"></a>5.    Vytvořte nový plán.
### <a name="offers-plans-transactions"></a>Nabízí, plánů, transakce.
Každý nabídka může mít více plánů, ale musí mít alespoň jedna (1) plánu. Když koncoví uživatelé přihlášení k odběru tooyour nabídka přihlášení odběru pro jednu nabídku hello plánu. Každý plán definuje, jak koncoví uživatelé budou mít možnost toouse služby.

Aktuálně Azure Marketplace podporovat pouze měsíční předplatné transakce založené model pro datové služby, tj. koncoví uživatelé budou platit měsíční poplatek podle toohello cena hello konkrétní plán se zaregistrovali tooand bude mít tooconsume každý měsíc počet transakce definované hello plánu.

Každou transakci, obvykle definovány jako počet záznamů, které vrátí Data služby založené na dotazu hello odeslané toohello služby. Hello výchozí hodnota je 100. Počet transakcí vrácených tooeach dotazu bude počet záznamů dělený 100 a zaokrouhlený nahoru na nejbližší celé číslo toohello.

Je služba Azure Marketplace vrstvy odpovědnost toomonitor (monitorování) počet transakcí spotřebovávají každý dotaz.

> [!IMPORTANT]
> Koncoví uživatelé, které bylo dosaženo limitu transakce hello v měsíci hello se bude blokovat pokračování služby hello toouse až konce jejich měsíčním cyklu předplatného.
> 
> Hello plán nebo jeden z plánů hello může (ale není nutné) obsahovat neomezený počet transakcí.
> 
> 

### <a name="create-a-plan"></a>Vytvoření plánu.
1. Klikněte na **"+"** další toohello "Přidat nový plán".
2. Vyberte jednu z možností hello: **neomezený** nebo **omezené** využití pro tento plán.  Je-li omezené zadejte hello počet transakcí hello plán umožňuje tooconsume za měsíc.
   
    ![Kreslení](media/marketplace-publishing-data-service-creation/step-5.1.png)  
   
    Publikování portál také navrhne "Plán identifikátor", které budou použité toocommunicate toohello koncoví uživatelé hello název plánu hello v hello uživatelského rozhraní a také používány hello Marketplace služby tooidentify hello plánu. Pokud chcete, můžete změnit hello "Plánování identifikátor".
   
   > [!NOTE]
   > Hello "Plánování identifikátor" musí být jedinečný v rámci oboru hello každou nabídku. Jako v mnoha identifikátory použité v hello identifikátor bude zablokován po hello první publikování tooproduction a nebude je možné toochange publikování plánování portálu tohoto identifikátoru.
   > 
   > 
3. Klikněte na tlačítko tooaccept svého výběru.
4. Potom budete požádáni několik další otázky týkající se nově vytvořená plánu.
   
    ![Kreslení](media/marketplace-publishing-data-service-creation/step-5.2.png)

| Otázky | Násobek. |
| --- | --- |
| **Tento plán je volné a dostupné celosvětové?** |Můžete vytvořit plán úplně bez z – zdarma. Pokud jeho hello pouze v rámci této nabídky – plánování, znamená to, že publikujete "Volné nabízejí" v hello Marketplace. Pokud se jedná pouze pro jeden (z několika) plán, nabízí více informací o služby s relativně malý počet transakcí za měsíc možnost toolearn koncoví uživatelé toooffer hello.  Pokud je odpověď hello "Ano", bude se dotaz žádné další otázky. |

> [!NOTE]
> Koncoví uživatelé můžete upgradovat vždy toohello placené plány.
> 
> 

| Otázky | Násobek. |
| --- | --- |
| **Je k dispozici bezplatná zkušební verze?** |Můžete vybrat mezi "Ne zkušební verzi" vůbec nebo poskytnout toouse možnost plánu pro "Jeden měsíc". Vydavatelé jako toouse tuto možnost tooprovide koncoví uživatelé hello možnost toounderstand hello výhody hello nabízejí zdarma pro jeden měsíc. |

> [!IMPORTANT]
> Koncoví uživatelé pouze bude možné toopurchase bezplatnou zkušební verzi, pokud se zavedly platebním nástrojem například platební karty, smlouvy enterprise.
> 
> Po jednom měsíci hello bezplatnou zkušební verzi Azure Marketplace se spustí poplatků zákazníkům hello ceníku k datu hello hello předplatného, pokud zákazník hello iniciované hello zrušení předplatného. Koncoví uživatelé toohello bude poskytnuta žádná speciální oznámení.
> 
> 

| Otázky | Násobek. |
| --- | --- |
| **Tento plán vyžaduje toopurchase kódu povýšení?** |Vydavatelé mít možnost toolimit přístup tootheir plánů služby Service tím, že poskytuje speciální kódu, nazývají "A Promocode" toospecific zákazníků. Pouze koncoví uživatelé, kteří budou mít tento Promocode bude mít toosubscribe toohello plán. Pokud zvolíte "Ne", pak souhlasíte, že je k dispozici všem uživatelům z hello oblasti, kde hello nabízejí (najdete v části [Marketplace Marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) podrobnosti) bude možné toosubscribe toothis plán. Žádné další otázky se dotaz. |
| **Také skrýt tento plán z každý, kdo nemá platný propagační kód?** |Pokud hello odpovědí toohello předchozí otázce je "Ano" hello vydavatele má toocompletely možnost odebrání zobrazovaných v hello uživatelské hello Marketplace tento plán. Znamená to, zákazníci neuvidí tento plán v stránce s podrobnostmi o nabídka hello. Koncoví uživatelé, kteří obdrží promocode toopurchase, bude možné toosubscribe tooit pomocí této promocode. |

## <a name="6----create-your-marketplace-marketing-content"></a>6.    Vytvoření vašeho webu Marketplace marketingové obsahu
Pro jak tooprovide informace požadované v **Marketing, Cenová, podpory a kategorie** karty navštivte [Marketplace Marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) což je běžný tooall artefakty publikované v hello Azure Marketplace.  

## <a name="7----connect-your-offer-tooyour-service-sql-azure-based-or-web-service-based"></a>7.    Připojte vaše tooyour nabídka služby (na základě SQL Azure nebo na základě webové služby).
Klikněte na hello **datové služby** dílčí nabídky.

V horní polovině hello stránku hello budete dotázáni, nabídka hello tooprovide **Namespace**.  

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.png)

Hello níže otázku bude určení, jak hello vydavatele bude nově vytvořený tooexpose nabídka tooAzure Marketplace. (Podrobnosti najdete v části hello [Data služby technické požadovaných průvodce](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Publikování hello databáze na základě služby**

Klikněte na **databáze**. Zobrazí se následující stránka Hello:

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.3.png)

toocreate CSDL mapování pro hello datové sady založené na hello databázi SQL Azure:

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.4.png)

A potom pro každou tabulku

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.6.png)

Pokud webová služba

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [!IMPORTANT]
> Čtení [mapování existující webové služby tooOData prostřednictvím CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) podrobné pokyny a příklady pro vytváření CSDL webové služby.
> 
> 

## <a name="next-steps"></a>Další kroky
Teď, když jste vytvořili vaši nabídku Data Service, zkontrolujte, zda dokončení hello pokyny v hello [Marketplace Marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) před přesunutím dál příliš[testování služby dat v pracovním](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Viz také
* [Začínáme: Jak toopublish toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md)
* Pokud vás zajímá Principy hello celý proces mapování OData a účel, přečtěte si tento článek [mapování dat služby OData](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definice struktury a pokynů.
* Pokud vás zajímá učení a pochopení hello konkrétní uzlů a jejich parametrů, přečtěte si tento článek [datové služby OData mapování uzly](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) pro definice a vysvětlení, příklady a kontext případů použití.
* Pokud vás zajímá kontrola příklady, přečtěte si tento článek [Data služby OData mapování příklady](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee ukázkový kód a pochopit syntaxe kódu a kontext.

[link-pubportal]:https://publish.windowsazure.com
