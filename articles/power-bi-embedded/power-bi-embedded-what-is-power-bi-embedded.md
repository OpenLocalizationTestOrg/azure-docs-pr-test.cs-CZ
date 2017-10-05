---
title: Co je Microsoft Power BI Embedded?
description: "Power BI Embedded můžete integrovat sestavy Power BI na váš web nebo mobilní aplikace, takže nemusíte vytvářet vlastní řešení."
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
ms.openlocfilehash: 0b5f7a2c3fd16ac32b0bc382616ca6600d378bb8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-microsoft-power-bi-embedded"></a>Co je Microsoft Power BI Embedded?
S **Power BI Embedded**, sestavy Power BI můžete integrovat přímo do vašeho webu nebo mobilní aplikace.

![](media/powerbi-embedded-whats-is/what-is.png)

Power BI Embedded je **služba Azure** umožňující nezávislí dodavatelé softwaru a vývojáři aplikací do prostor prostředí Power BI data v rámci svých aplikací. Jako vývojář když jste sestavili aplikace a tyto aplikace mají své vlastní uživatelů a odlišnou sadu funkcí. Tyto aplikace může také vyskytnout do mají některé prvky předdefinované data jako grafech a sestavách, které můžete nyní se používá technologii Microsoft Power BI Embedded. Nepotřebujete účet Power BI pro použití vaší aplikace. Můžete nadále přihlásit do vaší aplikace, stejně jako před a zobrazení a interakci s Power BI reporting prostředí bez nutnosti další licence.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Licencování pro Microsoft Power BI Embedded
V **Microsoft Power BI Embedded** modelu využití licencí pro Power BI není odpovědnost koncových uživatelů.  Místo toho **relací** jsou koupili vývojář aplikace, která spotřebovává vizuálech a jsou zodpovědné za předplatné, které vlastní tyto prostředky. Další informace naleznete na [stránce s cenami](https://azure.microsoft.com/en-us/pricing/details/power-bi-embedded/).

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI Embedded konceptuálního modelu

![](media/powerbi-embedded-whats-is/model.png)

Stejně jako další služby v Azure, jsou zřízené prostředky pro Power BI Embedded prostřednictvím [rozhraní API Správce Azure Resource Manager](https://msdn.microsoft.com/library/mt712306.aspx). V tomto případě je prostředkem, který zřídíte, **kolekce Pracovních prostorů Power BI**.

## <a name="workspace-collection"></a>Kolekce pracovních prostorů
A **kolekce pracovních prostorů** je nejvyšší úrovně Azure kontejner pro prostředky obsahující 0 nebo více **pracovních prostorů**.  A **prostoru** **kolekce** má všechny vlastnosti standardní Azure, stejně jako následující:

* **Přístupové klíče** – klíčů používaných při bezpečně volání rozhraní API Power BI (popsané v další části).
* **Uživatelé** – Azure Active Directory (AAD) uživatele, kteří mají práva správce ke správě kolekce pracovních prostorů Power BI, který prostřednictvím portálu Azure nebo rozhraní API Správce prostředků Azure.
* **Oblast** – v rámci zřizování **kolekce pracovních prostorů**, můžete vybrat zřídit v oblasti. Další informace najdete v tématu [oblasti Azure](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Pracovní prostor
A **prostoru** je kontejner Power BI obsahu, který může obsahovat datových sad a sestav. A **prostoru** je prázdný, při prvním vytvoření. Budete vytvářet obsah pomocí Power BI Desktop a budete soubor PBIX programově nasadíte do pracovního prostoru pomocí [rozhraní API pro Power BI Import](https://msdn.microsoft.com/library/mt711504.aspx). Můžete také programově vytvořit datovou sadu a potom vytvořit sestavy v rámci vaší aplikace místo pomocí Power BI Desktop.

## <a name="using-workspace-collections-and-workspaces"></a>Pomocí kolekcí pracovní prostor a pracovní prostory
**Pracovní prostor kolekce** a **pracovních prostorů** jsou kontejnery obsahu, které se používají a jsou uspořádány do libovolného nejlepší způsob vyhovuje návrhu aplikace, kterou vytváříte. Bude mnoha různými způsoby, může uspořádání obsahu v nich. Můžete se rozhodnout blokovat veškerý obsah v rámci jednoho pracovního prostoru a později tokeny aplikací používat dále dělit další obsah mezi vašich zákazníků. Můžete se rozhodnout také uvést všichni zákazníci do samostatné pracovní prostory, tak, aby bylo některé oddělení mezi nimi. Nebo můžete se rozhodnout pro uspořádání uživatelů podle oblasti, a nikoli zákazníka. Tento návrh flexibilní umožňuje zvolit nejlepší způsob, jak uspořádání obsahu.

## <a name="cached-datasets"></a>Datové sady v mezipaměti
Datové sady v mezipaměti můžete použít.  Však nelze aktualizovat data uložená v mezipaměti, jakmile se načetl do **Microsoft Power BI Embedded**. Datové sady v mezipaměti znamená, že jste importovali data do Power BI Desktop místo použití DirectQuery.

## <a name="authentication-and-authorization-with-app-tokens"></a>Ověřování a autorizace s tokeny aplikací
**Microsoft Power BI Embedded** odkládat údaje do vaší aplikace provést všechny potřebné uživatelské ověřování a autorizace. Neexistuje žádné explicitní požadavek, aby vaši koncoví uživatelé se zákazníci služby Azure Active Directory (Azure AD).  Místo toho vyjadřoval vaší aplikace do **Microsoft Power BI Embedded** autorizace k vykreslení sestavy Power BI pomocí **tokeny ověřování aplikace (tokeny aplikací)**.  Tyto **tokeny aplikací** vytvářejí podle potřeby, když aplikace chce vykreslení sestavy.

![](media/powerbi-embedded-whats-is/app-tokens.png)

**Tokeny ověřování aplikace (tokeny aplikací)** se používají k ověřování na základě **Microsoft Power BI Embedded**.  Existují tři typy **tokeny aplikací**:

1. Zřizování tokeny - použít při zřizování nové **prostoru** do **kolekce pracovních prostorů**
2. Vývoj tokeny - používá při volání přímo na **Power BI REST API**
3. Vložení tokeny - používá při volání pro vykreslení sestavy v vloženého elementu iframe

Tyto tokeny se používají pro v různých fázích vaši interakci s **Microsoft Power BI Embedded**.  Tokeny jsou navržené tak, aby k Power BI můžete delegovat oprávnění z vaší aplikace. Další informace najdete v tématu [toku tokenu aplikace](power-bi-embedded-app-token-flow.md).

## <a name="create-or-edit-reports-within-your-application"></a>Vytvoření nebo úpravě sestav v rámci vaší aplikace

Můžete teď upravit existovat sestavy nebo vytváření nových sestav přímo ve vaší aplikaci bez nutnosti použití Power BI Desktop. To vyžaduje, že existují datové sady v rámci pracovního prostoru.

## <a name="see-also"></a>Viz také

[Běžné scénáře Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)  
[Začínáme s Microsoft Power BI Embedded](power-bi-embedded-get-started.md)  
[Začínáme s ukázkou](power-bi-embedded-get-started-sample.md)  
[Vložení sestavy](power-bi-embedded-embed-report.md)  
[Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Vložená ukázka JavaScriptu](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Úložiště Git PowerBI CSharp](https://github.com/Microsoft/PowerBI-CSharp)  
[Úložiště Git PowerBI uzlu](https://github.com/Microsoft/PowerBI-Node)  
Chcete se ještě na něco zeptat? [Vyzkoušejte komunitu Power BI](http://community.powerbi.com/)
