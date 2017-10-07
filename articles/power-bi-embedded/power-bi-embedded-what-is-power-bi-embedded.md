---
title: aaaWhat je Microsoft Power BI Embedded?
description: "Power BI Embedded můžete sestavy Power BI toointegrate do mobilní aplikace nebo webové takže není nutné toobuild vlastní řešení."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 03649b72-b7d7-40ca-b077-12356d72d4f3
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/20/2017
ms.author: asaxton
ms.openlocfilehash: 0353938b6cdd9bb58b123b250f45f76b8cc7abe6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-microsoft-power-bi-embedded"></a>Co je Microsoft Power BI Embedded?
S **Power BI Embedded**, sestavy Power BI můžete integrovat přímo do vašeho webu nebo mobilní aplikace.

![](media/powerbi-embedded-whats-is/what-is.png)

Power BI Embedded je **služba Azure** umožňující nezávislí dodavatelé softwaru a dojde k toosurface vývojáři aplikace Power BI data v rámci svých aplikací. Jako vývojář když jste sestavili aplikace a tyto aplikace mají své vlastní uživatelů a odlišnou sadu funkcí. Tyto aplikace může také vyskytnout toohave některé předdefinované datové prvky, jako třeba grafy a sestavy, které můžete nyní se používá technologii Microsoft Power BI Embedded. Nepotřebujete toouse účet Power BI vaší aplikace. Můžete pokračovat toosign v aplikaci tooyour stejně jako před a zobrazení a interakci s hello Power BI reporting prostředí bez nutnosti další licence.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Licencování pro Microsoft Power BI Embedded
V hello **Microsoft Power BI Embedded** modelu využití licencí pro Power BI není hello zodpovědností hello koncového uživatele.  Místo toho **relací** jsou koupili hello vývojáře hello aplikace, která spotřebovává hello vizuály a budou se účtovat toohello předplatné, které vlastní tyto prostředky. Další informace naleznete na hello [stránce s cenami](https://azure.microsoft.com/en-us/pricing/details/power-bi-embedded/).

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI Embedded konceptuálního modelu

![](media/powerbi-embedded-whats-is/model.png)

Stejně jako další služby v Azure, jsou zřízené prostředky pro Power BI Embedded prostřednictvím hello [rozhraní API Správce Azure Resource Manager](https://msdn.microsoft.com/library/mt712306.aspx). V takovém případě je hello prostředek, který zřídíte **kolekce pracovních prostorů Power BI**.

## <a name="workspace-collection"></a>Kolekce pracovních prostorů
A **kolekce pracovních prostorů** je hello nejvyšší úrovně Azure kontejner pro prostředky obsahující 0 nebo více **pracovních prostorů**.  A **prostoru** **kolekce** má všechny standardní Azure vlastnosti hello, jakož i hello následující:

* **Přístupové klíče** – hello klíčů používaných při bezpečně volání rozhraní API Power BI (popsané v další části).
* **Uživatelé** – Azure Active Directory (AAD) uživatele, kteří mají toomanage práva správce hello kolekce pracovních prostorů Power BI prostřednictvím hello portál Azure nebo rozhraní API Správce prostředků Azure.
* **Oblast** – v rámci zřizování **kolekce pracovních prostorů**, můžete vybrat oblast toobe, zřízené v. Další informace najdete v tématu [oblasti Azure](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Pracovní prostor
A **prostoru** je kontejner Power BI obsahu, který může obsahovat datových sad a sestav. A **prostoru** je prázdný, při prvním vytvoření. Budete vytvářet obsah pomocí Power BI Desktop a budete hello soubor PBIX programově nasadíte do pracovního prostoru pomocí hello [rozhraní API pro Power BI Import](https://msdn.microsoft.com/library/mt711504.aspx). Můžete také programově vytvořit datovou sadu a potom vytvořit sestavy v rámci vaší aplikace místo pomocí Power BI Desktop.

## <a name="using-workspace-collections-and-workspaces"></a>Pomocí kolekcí pracovní prostor a pracovní prostory
**Pracovní prostor kolekce** a **pracovních prostorů** jsou kontejnery obsahu, které se používají a jsou uspořádány do libovolného nejlepší způsob vyhovuje hello návrhu aplikace hello vytváříte. Bude mnoha různými způsoby, může uspořádat hello obsah v nich. Můžete se rozhodnout tooput veškerý obsah v rámci jednoho pracovního prostoru, a poté novější použijte aplikaci tokeny toofurther rozdělit hello obsahu mezi vašich zákazníků. Můžete také zvolit tooput všem zákazníkům v samostatné pracovní prostory tak, aby bylo některé oddělení mezi nimi. Nebo můžete zvolit tooorganize uživatelů podle oblasti, a nikoli zákazníka. Tento návrh flexibilní vám umožní toochoose hello nejlepší způsob, jak tooorganize obsahu.

## <a name="cached-datasets"></a>Datové sady v mezipaměti
Datové sady v mezipaměti můžete použít.  Však nelze aktualizovat data uložená v mezipaměti, jakmile se načetl do **Microsoft Power BI Embedded**. Datové sady v mezipaměti znamená, že jste importovali hello dat do Power BI Desktop místo použití DirectQuery.

## <a name="authentication-and-authorization-with-app-tokens"></a>Ověřování a autorizace s tokeny aplikací
**Microsoft Power BI Embedded** tooperform tooyour aplikace odkládat údaje, všechny hello potřebné uživatelské ověřování a autorizace. Neexistuje žádné explicitní požadavek, aby vaši koncoví uživatelé se zákazníci služby Azure Active Directory (Azure AD).  Místo toho se aplikace vyjadřoval příliš**Microsoft Power BI Embedded** toorender autorizace sestavy Power BI pomocí **tokeny ověřování aplikace (tokeny aplikací)**.  Tyto **tokeny aplikací** vytvářejí podle potřeby, když aplikace chce toorender sestavy.

![](media/powerbi-embedded-whats-is/app-tokens.png)

**Tokeny ověřování aplikace (tokeny aplikací)** jsou použité tooauthenticate proti **Microsoft Power BI Embedded**.  Existují tři typy **tokeny aplikací**:

1. Zřizování tokeny - použít při zřizování nové **prostoru** do **kolekce pracovních prostorů**
2. Vývoj pro tokeny - používá při provádění volá přímo toohello **Power BI REST API**
3. Vložení tokeny - používá při provádění volání toorender sestavy v hello vložených elementu iframe

Tyto tokeny se používají pro hello fáze vaši interakci s **Microsoft Power BI Embedded**.  Hello tokeny jsou navržené tak, aby z vaší aplikace tooPower BI můžete delegovat oprávnění. Další informace najdete v tématu [toku tokenu aplikace](power-bi-embedded-app-token-flow.md).

## <a name="create-or-edit-reports-within-your-application"></a>Vytvoření nebo úpravě sestav v rámci vaší aplikace

Můžete teď upravit existovat sestavy nebo vytváření nových sestav přímo ve vaší aplikaci bez nutnosti toouse Power BI Desktop. To vyžaduje, že existují datové sady v rámci pracovního prostoru.

## <a name="see-also"></a>Viz také

[Běžné scénáře Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)  
[Začínáme s Microsoft Power BI Embedded](power-bi-embedded-get-started.md)  
[Začínáme s ukázkou](power-bi-embedded-get-started-sample.md)  
[Vložení sestavy](power-bi-embedded-embed-report.md)  
[Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Vložená ukázka JavaScriptu](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Úložiště Git PowerBI CSharp](https://github.com/Microsoft/PowerBI-CSharp)  
[Úložiště Git PowerBI uzlu](https://github.com/Microsoft/PowerBI-Node)  
Chcete se ještě na něco zeptat? [Zkuste hello komunitě Power BI](http://community.powerbi.com/)
