---
title: "aaaBuild mobilních aplikací s Xamarin a Azure Cosmos DB | Microsoft Docs"
description: "Kurz, který vytvoří Xamarin iOS, Android nebo formuláře aplikace pomocí Azure Cosmos DB. Azure Cosmos DB je fast, planetu škálování, cloudu databáze pro mobilní aplikace."
services: cosmos-db
documentationcenter: .net
author: arramac
manager: monicar
editor: 
ms.assetid: ff97881a-b41a-499d-b7ab-4f394df0e153
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: arramac
ms.openlocfilehash: 362a0e239a61e1309e1cf720c23151760952cf69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-mobile-applications-with-xamarin-and-azure-cosmos-db"></a>Vytváření mobilních aplikací s Xamarin a Azure Cosmos DB
Většina mobilních aplikací potřebovat toostore dat v cloudu hello a Azure Cosmos databáze je databáze cloudu pro mobilní aplikace. Obsahuje všechny objekty, které musí vývojář mobilní. Je plně spravovaná databáze jako služba, která je škálovatelná na vyžádání. Aplikace data tooyour ho můžete zahrnout transparentně, bez ohledu na umístění uživatele kolem hello zeměkouli. Pomocí hello [Azure Cosmos DB .NET Core SDK](documentdb-sdk-dotnet-core.md), můžete povolit toointeract mobilní aplikace Xamarin přímo s Azure Cosmos DB bez střední vrstvy.

Tento článek obsahuje návod pro vytváření mobilních aplikací s Xamarin a Azure Cosmos DB. Hello úplný zdrojový kód najdete hello kurzu na [Xamarin a Azure DB Cosmos na Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin), včetně jak toomanage uživatele a oprávnění.

## <a name="azure-cosmos-db-capabilities-for-mobile-apps"></a>Možnosti Azure Cosmos DB pro mobilní aplikace
Azure Cosmos DB poskytuje hello následující klíčové funkce pro vývojáře mobilních aplikací:

![Možnosti Azure Cosmos DB pro mobilní aplikace](media/mobile-apps-with-xamarin/documentdb-for-mobile.png)

* Bohaté dotazy nad datovým. Azure Cosmos DB ukládá data jako schemaless dokumenty JSON v heterogenní kolekce. Nabízí [bohatý a rychlé dotazy](documentdb-sql-query.md) bez hello potřebovat tooworry o schémata nebo indexů.
* Rychlé propustnost. Trvá jen několik milisekund tooread a zápisu dokumentů pomocí Azure Cosmos DB. Vývojářům můžete zadat hello propustnost, které potřebují a Azure Cosmos DB ctí ho s SLA 99,99 %.
* Neomezený škálování. Kolekce Azure Cosmos DB [růst s růstem aplikace](partition-data.md). Můžete spustit s malou velikost dat a propustnost stovky požadavků za sekundu. Kolekce můžou růst toopetabytes dat a libovolně velké propustnosti se stovkami miliony požadavků za sekundu.
* Globálně distribuované. Mobilní aplikace, které uživatelé jsou na hello přejděte často napříč hello, world. Je Azure Cosmos DB [globálně distribuované databáze](distribute-data-globally.md). Klikněte na tlačítko hello mapy toomake uživatelům přístupné tooyour data.
* Předdefinované bohaté autorizace. S Azure Cosmos databáze, můžete snadno implementovat oblíbených vypadají podobně jako [uživatelská data](https://aka.ms/documentdb-xamarin-todouser) nebo s více uživateli sdílet data, bez komplexní vlastní autorizační kód.
* Geoprostorové dotazy. Mnoha mobilních aplikacích nabídku geograficky kontextové prostředí ještě dnes. S prvotřídní podporu pro [geoprostorové typy](geospatial.md), Azure Cosmos DB díky vytváření snadno tooaccomplish tyto činnosti.
* Binární přílohy. Data aplikací často obsahují binární objekty BLOB. Nativní podpora pro přílohy umožňuje snazší toouse Azure Cosmos DB jako centralizované pro data aplikace.

## <a name="azure-cosmos-db-and-xamarin-tutorial"></a>Kurz pro Azure Cosmos DB a Xamarin
Hello následujícím kurzu se dozvíte, jak toobuild mobilních aplikací pomocí Xamarin a Azure Cosmos DB. Hello úplný zdrojový kód najdete hello kurzu na [Xamarin a Azure DB Cosmos na Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).

### <a name="get-started"></a>Začínáme
Je snadno tooget začít s Azure Cosmos DB. Přejděte toohello portál Azure a vytvořit nový účet Azure Cosmos DB. Klikněte na tlačítko hello **úvodní** kartě. Stažení hello Xamarin Forms úkolů seznamu vzorku, který je již připojen tooyour účet Azure Cosmos DB. 

![Azure Cosmos DB rychlý start pro mobilní aplikace](media/mobile-apps-with-xamarin/cosmos-db-quickstart.png)

Nebo pokud máte existující aplikaci Xamarin, můžete přidat hello [balíčku NuGet pro Azure Cosmos DB](documentdb-sdk-dotnet-core.md). Podporuje Azure Cosmos DB Xamarin.IOS, Xamarin.Android, a Xamarin Forms sdílené knihovny.

### <a name="work-with-data"></a>Práce s daty
Záznamy dat jsou uloženy v databázi Azure Cosmos jako schemaless dokumenty JSON v heterogenní kolekce. Můžete ukládat dokumenty s jinou struktury v hello stejné kolekci:

```cs
    var result = await client.CreateDocumentAsync(collectionLink, todoItem);
```

V Xamarin projekty můžete použít dotazy na language-integrated nad datovým:

```cs
    var query = await client.CreateDocumentQuery<ToDoItem>(collectionLink)
                    .Where(todoItem => todoItem.Complete == false)
                    .AsDocumentQuery();

    Items = new List<TodoItem>();
    while (query.HasMoreResults) {
        Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
    }
```
### <a name="add-users"></a>Přidání uživatelů
Jako mnoho získat Začínáme ukázky, ověří ukázkové databáze Azure Cosmos hello, které jste stáhli toohello služby pomocí pevně zakódované hlavní klíč v kódu aplikace hello. Toto výchozí nastavení není vhodné pro aplikace, který chcete toorun kdekoli s výjimkou na vaše místní emulátor. Pokud neoprávněný uživatel získat hello hlavního klíče, může dojít k ohrožení všechna data hello napříč účtu Azure Cosmos DB. Místo toho budete chtít že vaší aplikace tooaccess pouze hello záznamy pro hello přihlášeného uživatele. Azure Cosmos DB umožňuje vývojářům čtení nebo pro čtení a zápis kolekce tooa oprávnění, sada dokumenty seskupené podle klíč oddílu, nebo konkrétní dokument toogrant aplikace. 

Postupujte podle těchto kroků toomodify hello úkolů aplikace tooa víceuživatelská úkolů seznamu app seznamu: 

  1. Přidáte aplikaci tooyour přihlášení pomocí Facebooku, služby Active Directory nebo jiného poskytovatele.

  2. Vytvořte kolekci Azure Cosmos DB UserItems s **/UserId** jako klíč oddílu hello. Určení, že klíč oddílu hello kolekce umožňuje Azure Cosmos DB tooscale nekonečnou jako hello počet uživatelů vaší aplikace roste, přitom dál toooffer rychlé dotazy.

  3. Přidejte Token zprostředkovatele prostředků Azure Cosmos DB. Tento jednoduchý webového rozhraní API ověřuje uživatele a vydá krátkodobou tokeny toosigned-in Uživatelé s přístupem jen toohello dokumenty v rámci svého oddílu. V tomto příkladu je Token zprostředkovatele prostředků hostované ve službě App Service.

  4. Upravte hello aplikace tooauthenticate tooResource tokenu zprostředkovatele se sítí Facebook a žádosti o tokeny prostředků hello hello přihlášeného uživatele služby Facebook. Potom můžete přistupovat k datům v hello UserItems kolekce.  

Najdete kompletní příklad tohoto vzoru v [tokenu zprostředkovatele prostředků na Githubu](http://aka.ms/documentdb-xamarin-todouser). Tento diagram znázorňuje hello řešení:

![Zprostředkovatel uživatele a oprávnění Azure Cosmos DB](media/mobile-apps-with-xamarin/documentdb-resource-token-broker.png)

Pokud chcete, aby dva uživatelé toohave přístup toohello stejný seznam úkolů, můžete přidat další oprávnění toohello přístupový token v tokenu zprostředkovatele prostředků.

### <a name="scale-on-demand"></a>Škálování na vyžádání
Azure Cosmos DB je spravovaný databáze jako služba. S růstem vaší uživatelské základny, nepotřebujete tooworry o zřizování virtuálních počítačů nebo zvýšení jader. Stačí tootell Azure Cosmos DB je vaše aplikace musí kolik operací za sekundu (propustnost). Můžete zadat hello propustnost prostřednictvím hello **škálování** karta pomocí míru propustnosti názvem jednotky žádosti (ruština) za sekundu. Například operace čtení pro 1 KB dokumentu vyžaduje 1 RU. Můžete také přidat výstrahy toohello **propustnost** metriky toomonitor hello provoz růstu a programově změnit hello propustnost jako výstrahy ještě efektivněji.

![Azure Cosmos DB škálování propustnosti na vyžádání](media/mobile-apps-with-xamarin/cosmos-db-xamarin-scale.png)

### <a name="go-planet-scale"></a>Přejděte planetu škálování
Jako oblíbenosti zvýšení vaší aplikace může získat uživatelé v celém světě hello. Nebo možná budete chtít toobe připravené pro nepředpokládaného události. Přejděte toohello portál Azure a otevřete váš účet Azure Cosmos DB. Klikněte na hello mapy toomake vaše data nepřetržitě replikace tooany počet oblastí napříč hello, world. Tato funkce zpřístupňuje data, kde jsou vaši uživatelé. Můžete také přidat toobe zásady převzetí služeb při selhání připravené pro událostí odpovídajících odvětvím.

![Azure Cosmos DB škálování napříč zeměpisné oblasti](media/mobile-apps-with-xamarin/cosmos-db-xamarin-replicate.png)

Blahopřejeme. Byly dokončeny hello řešení a mít mobilní aplikaci s Xamarin a Azure Cosmos DB. Postupujte podle podobné kroky toobuild Cordova aplikace pomocí hello Azure Cosmos DB JavaScript SDK a nativní aplikace iOS nebo Android pomocí rozhraní API REST Azure Cosmos DB.

## <a name="next-steps"></a>Další kroky
* Zobrazit zdrojový kód hello [Xamarin a Azure DB Cosmos na Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).
* Stáhnout hello [Azure Cosmos DB .NET Core SDK](documentdb-sdk-dotnet-core.md).
* Najít další ukázky kódu pro [aplikací .NET](documentdb-dotnet-samples.md).
* Další informace o [Azure Cosmos DB bohaté dotazování možnosti](documentdb-sql-query.md).
* Další informace o [geoprostorové podpory v Azure Cosmos DB](geospatial.md).



