---
title: aaaAzure katalogu Data Catalog terminologie | Microsoft Docs
description: "Tento článek obsahuje tooconcepts úvod a termínů používaných v dokumentaci k Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6fec74d9-4a3c-4b4b-88ba-cad5ad143331
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: b5f071db4f62c914d2c1cdef9aa686b18d5297d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-terminology"></a>Terminologie služby Azure Data Catalog
## <a name="catalog"></a>Katalogu
Hello Azure Data Catalog je metadata cloudové úložiště v data, která se dají registrovat zdroje a data prostředků. katalog Hello slouží jako umístění centrální úložiště pro strukturální metadata extrahovaná ze zdroje dat a pro popisná metadata přidané uživateli.

## <a name="data-source"></a>Zdroj dat
Zdroj dat je systém nebo kontejner, který spravuje datových prostředků. Mezi příklady patří databáze serveru SQL, Oracle – databáze, databáze SQL Server Analysis Services (tabulkové nebo multidimenzionální) a servery SQL Server Reporting Services.

## <a name="data-asset"></a>Datový prostředek
Datové prostředky jsou objekty obsažené v zdroje dat, které lze registrovat pomocí katalogu hello. Příklady tabulek systému SQL Server a zobrazení, Oracle tabulek a zobrazení, SQL Server Analysis Services míry, dimenze a klíčové ukazatele výkonu a sestav SQL Server Reporting Services.

## <a name="data-asset-location"></a>Prostředek umístění dat
Hello katalog obsahuje hello umístění zdroje dat nebo datový prostředek, který lze použít tooconnect toohello zdroje pomocí klientské aplikace. Formát Hello a podrobnosti o umístění hello lišit v závislosti na typu zdroje dat hello. Například tabulku systému SQL Server lze identifikovat podle jeho součástí čtyři název – název serveru, název databáze, název schématu, název objektu –, zatímco SQL Server Reporting Services sestavy lze identifikovat podle jeho adresa URL.

## <a name="structural-metadata"></a>Strukturální metadata
Strukturální metadata jsou hello metadat extrahovaných ze zdroje dat, který popisuje strukturu hello datový prostředek. To zahrnuje umístění hello prostředky, jeho název objektu a typu a další vlastnosti specifické pro typ. Například hello strukturální metadata pro tabulky a zobrazení obsahuje hello názvy a typy dat pro objekt hello sloupce.

## <a name="descriptive-metadata"></a>Popisná metadata
Popisná metadata jsou metadata, která popisuje účel hello nebo záměr datový prostředek. Obvykle je uživatelé katalogu pomocí portálu Azure Data Catalog hello přidaný popisných metadat, ale ho lze také extrahovat ze zdroje dat hello během registrace. Například nástroj pro registraci Azure Data Catalog hello extrahuje popisy z hello vlastnosti Popis na SQL Server Analysis Services a SQL Server Reporting Services a z hello [ms_description rozšířené vlastnosti](https://technet.microsoft.com/library/ms190243.aspx)v databáze systému SQL Server, pokud tyto vlastnosti jsou vyplněna s hodnotami.

## <a name="request-access"></a>Požádat o přístup
Informace o tom, jak toorequest přistupují toohello data asset nebo zdroj dat může obsahovat datový prostředek popisných metadat. Tyto informace se zobrazí umístění asset hello dat a mohou zahrnovat jeden nebo více hello následující možnosti:

* Hello e-mailovou adresu uživatele hello nebo tým odpovědný za udělení přístupu toohello datové zdroje.
* Adresa URL Hello hello popsané proces uživatelé musí postupovat podle pokynů zdroj dat toohello toogain přístup.
* Adresa URL Hello přístupu a identit a nástroj pro správu (jako je například Microsoft Identity Manager), může být zdroje dat toohello použité toogain přístup.
* Volné položku, která popisuje, jak mohou uživatelé získat zdroj dat toohello přístup.

## <a name="preview"></a>Preview
Náhled v Azure Data Catalog je snímek až too20 záznamy, které lze extrahovat ze zdroje dat hello během registrace a uložené v katalogu hello s metadaty asset hello data. může pomoci Hello preview uživatelé, kteří je zjistili datový prostředek lépe pochopit jeho funkci a účel. Jinými slovy zobrazuje ukázkových dat může být více než zobrazuje pouze datové typy a názvy sloupců hello.
Verze Preview jsou podporovány pouze pro tabulky a zobrazení a musí být explicitně hello uživatel vybírat při registraci.

## <a name="data-profile"></a>Data profilu
Profil dat v Azure Data Catalog je snímek úroveň tabulky a sloupce metadata o registrovaných datový prostředek, můžete extrahovat ze zdroje dat hello během registrace a uložené v katalogu hello s metadaty asset hello data. může pomoci Hello data profilu uživatele, kteří je zjistili datový prostředek lépe pochopit jeho funkci a účel. Podobné toopreviews, profily data musí být výslovně vybrán hello uživatele během registrace.

> [!NOTE]
> Extrahování dat profilu může být nákladná operace pro rozsáhlé tabulky a zobrazení, a může výrazně zvýšit hello tooregister čas potřebný zdroj dat.
>
>

## <a name="user-perspective"></a>Pohledu uživatele
V Azure Data Catalog může každý uživatel, poskytují popisná metadata pro prostředek registrované datové. Každý uživatel má odlišné perspektivou na hello data a jeho použití. Například může hello správcem odpovědným za serveru uveďte hello podrobné informace o jeho smlouvu o úrovni služeb (SLA) nebo zálohování; data steward může poskytují toodocumentation odkazy pro hello firmy zpracovává hello dat podporuje; a analytik může poskytnout popis v hello podmínky, které jsou nejdůležitější tooother analytici a může být nejcennější toothose uživatele, kteří potřebují toodiscover a pochopit hello data.

Každý z těchto perspektiv jsou ze své podstaty cenné a s Azure Data Catalog každý uživatel může poskytnout hello informace, které jsou smysluplný toothem, zatímco všichni uživatelé používat informace toounderstand hello dat a jeho účel.

## <a name="expert"></a>odborné
Odborník je uživatel, který byl identifikován tak, že má informovaně "odborné" perspektivy pro datový prostředek. Každý uživatel, můžete přidat sami nebo jinému uživateli jako odborník pro určitý prostředek. Není uvedena jako odborník nesou žádná zvláštní oprávnění v Azure Data Catalog; umožňuje uživatelům tooeasily najděte tyto perspektivy, které jsou pravděpodobně toobe užitečné při revizi popisná metadata pro určitý prostředek.

## <a name="owner"></a>Vlastník
Vlastníkem je uživatel, který má další oprávnění pro správu datový prostředek v Azure Data Catalog. Uživatelé mohou převzít vlastnictví registrovaných datových prostředků a vlastníky můžete přidat další uživatele jako spoluvlastníci. Další informace najdete v části [jak toomanage datových prostředků](data-catalog-how-to-manage.md)  

> [!NOTE]
> Vlastnictví a správu jsou k dispozici pouze v hello Azure Data Catalog, Standard Edition.
>
>

## <a name="registration"></a>Registrace
Registrace je hello operace extrahování dat asset metadat ze zdroje dat a kopírování toohello služby Azure Data Catalog. Datové prostředky, které byly zaregistrovány můžete pak opatřeny poznámkami a zjistit.

## <a name="see-also"></a>Viz také
* [Co je Azure Data Catalog?](data-catalog-what-is-data-catalog.md) – Tento článek obsahuje přehled služby Azure Data Catalog hello hello hodnotu, kterou poskytuje a hello scénáře, které podporuje.
* [Začínáme s Azure Data Catalog](data-catalog-get-started.md) – Tento článek obsahuje začátku do konce kurz, který ukazuje, jak toouse Azure datového katalogu pro zjišťování zdroje.  
