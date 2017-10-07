---
title: "aaaTechnical požadavky pro vytváření dat služby pro hello Marketplace | Microsoft Docs"
description: "Rady pro pochopení hello požadavky pro vytváření dat služby toodeploy a prodeje na hello Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: aaff609a-1cd1-4146-98f4-d04166b0fce0
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 2bba4282473fed63c3fcab43043a97e179705844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-hello-azure-marketplace"></a>Nabídka služeb pro Azure Marketplace hello technické požadavky pro vytvoření datové služby
> [!IMPORTANT]
> **V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.** Pokud máte obchodní aplikace SaaS chcete toopublish na AppSource najdete další informace [zde](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo služba vývojáře by jako toopublish na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Přečtěte si hello proces důkladně před zahájením a pochopit, kde a proč se provádí každý krok. Co nejvíce, měli byste Příprava informací o společnosti a další data, stáhněte potřebné nástroje, a vytvořit technické součásti před zahájením procesu vytváření nabídka hello.

Měli byste hello následující položky, které jsou připravené před zahájením procesu hello:

## <a name="make-a-decision-on-what-technology-will-be-used-toopublish-your-data-service-offer"></a>A rozhodnout, na jakou technologii bude použité toopublish vaši nabídku Data Service
Vydavatel můžete rozhodnout mezi více technologií při publikování dat služby v Azure Marketplace. Hello hlavní technologie, které jsou podporované popsané dole. Bez ohledu na to jakou technologii je použité toopublish hello dat služby, koncový uživatel hello používá hello data prostřednictvím hello **datový kanál OData** vystavený službou Azure Marketplace. Úplné informace týkající se služby OData lze najít na [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL databáze Azure
Datová sada připravena v produktech SQL Azure je odpovědnost vydavatele. Budete potřebovat toosubscribe tooAzure zřizovat odpovídající velikost databáze a nahrát Data do databáze SQL Azure. Vydavatel je také zodpovědná tookeep jejich vždy aktuální data. Další informace o přihlášení k odběru služby tooAzure můžete najít na [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)

Při přesouvání hello dat do SQL Azure, můžou zpřístupnit hello Azure Marketplace, tabulky a zobrazení. Hello vydavatele můžete určit, které tabulek nebo zobrazení a sloupce jsou zveřejněné toohello koncového uživatele. Další hello poskytovateli obsahu můžete také určit sloupce, které může dotazovat hello koncového uživatele a ty, které jsou vrácena pouze v datové části hello. To poskytuje vysokou úroveň flexibilitu o tom, které by měly být vystaveny data v databázi hello. Sloupce, které může být dotazován potřebovat toobe založenou na jeden nebo více indexů databáze.

## <a name="rest-based-web-service"></a>Na základě REST webové služby
Podporované protokol: **pouze HTTPS**

Existující služby REST, na základě mohou být zpřístupněny prostřednictvím hello Azure Marketplace. Protože hello datová sada vždy zveřejněné toohello koncového uživatele jako kanálu OData, hello služby Azure Marketplace musí mít toomap toobe hello tooa služby OData na základě služby. toodo, takže koncové body REST na základě hello třeba tooexpose všech parametrů jako parametry protokolu HTTP.

datová část Hello musí toobe ve formuláři, který lze mapovat do odpovědi ATOM. Proto hello odpověď ze služby hello potřebuje toobe ve formátu XML a může obsahovat pouze jednu opakující se element, který obsahuje hodnoty hello datová část (např. sada záznamů). Hello služby Azure Marketplace namapuje hello uzlu toohello položka uzel ATOM a hello datové hodnoty opakování do vlastnost uzlů v rámci uzlu položka hello.

Informace o ověření (například rozhraní API klíče, ověřování tokenu, atd.) musí toobe zadaný jako parametr HTTP nebo v hlavičce protokolu HTTP hello (pár klíčových hodnot) – základní ověřování je podporováno také. Platný klíč musí zadat toobe a tento klíč jsou určené všechny požadavky prostřednictvím Azure Marketplace. Ve vrstvě hello Azure Marketplace se stane uživatele monitorování a fakturace.

Chyby vrácené službou hello potřebovat toobe namapovat na stavové kódy HTTP. V případě, že služba hello vrací kód XML, který obsahuje chybu hello tyto budou toobe mapovat pomocí hello Azure Marketplace služby tooHTTP stavové kódy.

## <a name="soap-based-web-services"></a>Webové služby SOAP na základě
Protokol: **pouze HTTPS**

Hello požadavky jsou stejné jako části služby REST, na základě hello hello. Hello jediným rozdílem je, že parametry se dá zadat i v textu XML účtované služby toohello vydavatele s každou žádost odeslanou prostřednictvím Azure Marketplace. To znamená, že uživatel hello parametry protokolu HTTP poskytuje v hello front-end jsou se překlad vztahuje do elementů XML dokumentu XML účtované hello toohello obsahu zprostředkovatel požadavků na webové službě.

## <a name="odata-based-web-services"></a>OData na základě webové služby
Protokol: **pouze HTTPS**

Data mohou být zpřístupněny jako tooAzure služby OData Marketplace. Hello systému je toopass má služba hello prostřednictvím a nahradí hello kořenový server služby hello kořenovém adresáři služby Azure Marketplace hello – tooensure, všechny následné volání procházejí přes Azure Marketplace.

Služby OData pouze nepotřebují toogo na databázi v back-end hello. OData podporuje jakýkoli druh úložiště nebo obchodní logiky toodrive hello služby.

## <a name="next-steps"></a>Další kroky
Zkontrolovat hello předpoklady a dokončit nezbytné úlohy hello, můžete dál přesunout s hello vytváření vaši nabídku služby Data jako podrobné v hello [Průvodce publikování dat služby](marketplace-publishing-data-service-creation.md).

Nebo, pokud chcete tooreview hello celkové procesů a hello příslušné články pro každou hello publikování fáze, naleznete v článku hello [Začínáme: jak toopublish toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
