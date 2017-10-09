---
title: "aaaAzure Data Catalog nejčastější dotazy | Microsoft Docs"
description: "Nejčastější dotazy k Azure Data Catalog, včetně funkcí pro zjišťování zdroje dat, poznámky a správu."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5c7e209a-458c-4bb4-96bb-7ed178f9528a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 03f9f4b801640b2e14232c62c8fc168e42e2c393
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-frequently-asked-questions"></a>Nejčastější dotazy k Azure Data Catalog
Tento článek obsahuje odpovědi toofrequently výzva otázky související toohello služby Azure Data Catalog.

## <a name="what-is-azure-data-catalog"></a>Co je Azure Data Catalog?
Data Catalog je plně spravovaná služba, hostované ve službě Microsoft Azure, která slouží jako systém registrace a zjišťování datových zdrojů organizace. Pomocí katalogu Data Catalog každý uživatel, z analytici toodata vědců a vývojářů, může zaregistrovat, zjišťovat, pochopit a využívat zdroje dat..

## <a name="what-customer-challenges-does-it-solve"></a>Jaké zákazníka vyzve nemá vyřešit?
Data katalogu adresy hello výzvy "tmavý data" a zdroj dat zjišťování tak, aby uživatelé mohli zjistit a pochopení datových zdrojů organizace.

## <a name="what-are-its-target-audiences"></a>Jaké jsou jeho cílové skupiny?
Katalog data Catalog je určen pro technická a Netechnická uživatelů, včetně:

* Data vývojáři a profesionálové BI a analýzy: uživatelé, kteří jsou zodpovědní za vytváření dat a analýzy obsahu pro jiné tooconsume.
* Data stewards: uživatelé, kteří mají hello znalosti o hello dat, co znamená a jak je určený toobe použít.
* Spotřebitelé dat: uživatelé, kdo musí mít tooeasily toobe zjišťovat, pochopit a připojení dat toohello potřebují svou práci pro toodo hello nástroj podle svého výběru.
* Centrální IT: uživatelé, kteří potřebují toomake stovky zdrojů dat zjistitelný obchodní uživatelé, a kteří potřebují toomaintain dohled nad využití dat a kdo.

## <a name="what-is-its-availability-by-region"></a>Co je jeho dostupnost podle oblasti?
Data katalogu služby jsou momentálně k dispozici v následujících datových centrech hello:

* Západní USA
* Východ USA
* Západní Evropa
* Severní Evropa
* Austrálie – východ
* Jihovýchodní Asie

## <a name="what-are-its-limits-on-hello-number-of-data-assets"></a>Jaké jsou jeho omezení hello počtu datových prostředků?
Hello bezplatná edice katalogu Data Catalog je omezená too5 000 registrované datové prostředky.

Hello Data Catalog, Standard Edition podporuje až too100, 000 registrovaných datových prostředků.

## <a name="what-are-its-supported-data-source-and-asset-types"></a>Jaké jsou jeho podporované datové typy zdroje a asset?
Seznam aktuálně podporované zdroje dat, naleznete v části [DSR katalogu dat](data-catalog-dsr.md).

## <a name="how-do-i-request-support-for-another-data-source"></a>Jak lze požádat o podporu pro jiný zdroj dat?
toosubmit funkcí, požadavků a dalších zpětnou vazbu, přejděte toohello [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-do-i-get-started-with-data-catalog"></a>Jak můžu začít pracovat s katalogem Data Catalog?
Hello tooget nejlepší způsob, jak spustit je příliš přechodem[Začínáme s katalogem Data Catalog](data-catalog-get-started.md). Tento článek je přehled začátku do konce hello možností v hello služby.

## <a name="how-do-i-register-my-data"></a>Jak se zaregistrovat svá data?
tooregister vaše data v katalogu Data Catalog:
1. V portálu Azure Data Catalog hello v hello **publikovat** oblasti, nástroj pro registraci počáteční hello Azure Data Catalog. 
2. V katalogu Data Catalog hello publikování aplikace, přihlaste se pomocí hello stejné přihlašovací údaje, že používáte tooaccess hello portál katalogu Data Catalog.
3. Vyberte zdroj dat hello a hello konkrétní prostředky, které chcete tooregister.

## <a name="what-properties-does-it-extract-for-data-assets-that-are-registered"></a>Jaké vlastnosti ho extrahování datových prostředků, které jsou registrovány?
specifické vlastnosti Hello se liší od zdroj toodata zdroje dat, ale obecně hello publikování služby Data Catalog extrahuje hello následující informace:

* Název prostředku
* Typ prostředku
* Popis prostředku
* Názvy atributů nebo sloupce
* Atribut nebo sloupce datové typy
* Popis atributu nebo sloupce

> [!IMPORTANT]
> Registrace datové prostředky pomocí katalogu Data Catalog přesuňte nebo zkopírujte vaší toohello dat v cloudu. Registrace prostředky ze zdroje dat kopie hello tooAzure metadata prostředky, ale hello data zůstane v hello existující zdroj dat umístění. pravidlo toothis výjimky Hello je, pokud zvolíte tooupload preview záznamy nebo profil dat při registraci hello prostředky. Když zahrnete náhledu, až too20 záznamy zkopírovány z každé asset a uložený jako snímek v katalogu Data Catalog. Když zahrnete data profilu, agregační informace se počítá a součástí hello metadata, která je uložena v katalogu hello. Agregační informace mohou zahrnovat hello velikost tabulky, hello procento hodnoty null na sloupec nebo hello minimální, maximální a průměrné hodnoty pro sloupce. 
>
>

> [!NOTE]
> Zdroje dat, jako je například SQL Server Analysis Services, které mají první třídy **popis** vlastnost hello katalogu Data Catalog publikování aplikace extrahuje hodnota této vlastnosti. Pro relační databáze systému SQL Server, které nemají první třídy **popis** vlastnost hello publikování aplikace katalogu Data Catalog extrahuje hello hodnotu z hello **ms_description** rozšířené vlastnosti pro objekty a sloupce. Další informace najdete v tématu [pomocí rozšířené vlastnosti pro databázové objekty](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).
>
>

## <a name="how-long-should-it-take-for-newly-registered-assets-tooappear-in-hello-catalog"></a>Jak dlouho má trvat pro nově registrovaných aktiv tooappear v katalogu hello?
Po registraci prostředky pomocí katalogu Data Catalog, může být po dobu 5 sekund too10 předtím, než se objeví v katalogu Data Catalog portál hello.

## <a name="how-do-i-annotate-and-enrich-hello-metadata-for-my-registered-data-assets"></a>Jak opatřit poznámkami a zlepšit komunikaci hello metadata pro moje registrované datové prostředky?
Hello nejjednodušší způsob, jak tooprovide metadata pro registrovaných aktiv je tooselect hello asset hello katalogu Data Catalog portálu a poté zadejte hodnoty hello v podokně Vlastnosti hello nebo schéma pro vybraný objekt hello.

Některé metadata, například odborníky a značky, můžete zadat taky během procesu registrace hello. Hello hodnoty, které jsou uvedeny v hello publikování služby katalogu dat použít tooall prostředky registrované v daném čase. tooview hello nedávno zaregistrovali objekty portálu hello další poznámky, vyberte hello **zobrazit portál** tlačítko na poslední obrazovce hello hello katalogu Data Catalog publikování aplikace.

## <a name="how-do-i-delete-my-registered-data-objects"></a>Jak lze odstranit mé registrované datové objekty?
Při odstraňování objektu z katalogu Data Catalog výběr objektu hello hello portálu a poté kliknutím na hello **odstranit** tlačítko. Odebrání objektu hello odebere jeho metadata z katalogu Data Catalog, ale nemá vliv na podkladový zdroj dat hello.

## <a name="what-is-an-expert"></a>Co je odborník?
Odborník je osoba, která má informovanému perspektivy o datový objekt. Objekt může mít několik odborníky. Odborník nepotřebuje toobe hello "vlastník" pro objekt, ale je jednoduše někoho, kdo vědět, jak může hello dat a mělo by se.

## <a name="how-do-i-share-information-with-hello-data-catalog-team-if-i-encounter-problems"></a>Jak sdílet informace s týmem katalogu Data Catalog hello problémy dojde-li?
tooreport problémy, sdílet informace a klást otázky, přejděte toohello [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="does-hello-catalog-work-with-another-data-source-that-im-interested-in"></a>Nepodporuje hello katalogu pracují s jiným zdrojem dat, která mám zájem?
Aktivně pracujeme na přidání další datové zdroje tooData katalogu. Pokud chcete toosee konkrétní zdroj dat. podporována, ho navrhne (nebo hlasové podporu, pokud již byla navrhované) podle budete toohello [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-is-azure-data-catalog-related-toohello-data-catalog-in-power-bi-for-office-365"></a>Jak je Azure Data Catalog související toohello katalogu Data v Power BI pro Office 365?
Azure Data Catalog si můžete představit jako vývoje hello katalogu Data v Power BI. Od verze pružiny 2017 Azure Data Catalog je použité tooenable hello sdílení a zjišťování dotazy v aplikaci Excel 2016 a Power Query pro Excel. Možnosti katalogu data Catalog v aplikaci Excel jsou k dispozici toousers s Power BI Pro licence.

## <a name="what-permissions-do-i-need-tooregister-assets-with-data-catalog"></a>Co dělat, oprávnění potřebuji tooregister prostředky s katalogem Data Catalog?
Nástroj pro registraci katalogu Data Catalog hello toorun, potřebujete oprávnění u hello zdroje dat, který vám umožní tooread hello metadata z hello zdroje. tooalso zahrnout náhled, musíte mít oprávnění, které vám umožňují tooread v datech hello hello objekty, které jsou registrované.

## <a name="will-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Data Catalog budou dostupné pro místní nasazení také?
Data Catalog je Cloudová služba, které může fungovat pro cloudové a místní toodeliver zdroje dat hybridní řešení zdroj dat zjišťování. Aktuálně neexistují žádné plány pro verzi hello služby katalogu dat, který spustí místně.

## <a name="can-i-extract-more-or-richer-metadata-from-hello-data-sources-i-register"></a>Můžete rozbalit další nebo bohatší metadata z hello zdroje dat, které mohu zaregistrovat?
Aktivně pracujeme tooexpand hello možností katalogu Data Catalog. Pokud chcete toohave další metadata extrahovaná ze zdroje dat hello během registrace, navrhnout ho (nebo hlas pro něj, pokud již byla navrhované) v hello [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). V budoucích hello můžeme vám umožní třetí strany tooadd nové datové zdroje typy prostřednictvím rozšíření rozhraní API.

## <a name="how-do-i-restrict-hello-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Jak se tak, aby je můžete zjišťovat pouze určitým uživatelům omezit viditelnost hello registrované datové prostředky?
Vyberte hello datových prostředcích v hello katalogu Data Catalog a pak klikněte na tlačítko hello **převzít vlastnictví** tlačítko. Vlastníci datových prostředcích v katalogu Data Catalog můžete změnit viditelnost hello tooeither nastavení povolit všechny uživatele toodiscover hello prostředky ve vlastnictví nebo omezit viditelnost toospecific uživatele.

## <a name="how-do-i-update-hello-registration-for-a-data-asset-so-that-changes-in-hello-data-source-are-reflected-in-hello-catalog"></a>Jak se tak, aby změny ve zdroji dat hello se projeví v katalogu hello aktualizovat hello registrace pro datový prostředek?
tooupdate hello metadat pro datové prostředky, které jsou již zaregistrovány v katalogu hello, jednoduše znovu registrovat zdroj dat hello, který obsahuje prostředky hello. Jakékoli změny v hello zdroje dat, například přidávání nebo odebírání z tabulky a zobrazení, sloupce jsou aktualizovány v katalogu hello, ale žádné poznámky poskytované uživatele zachovány.

## <a name="my-question-isnt-answered-here-where-can-i-go-for-answers"></a>Není můj dotaz zodpovězené. Kde lze přejít odpovědi?
Přejděte toohello [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Existuje kladené otázky najdete zde jejich představ.
