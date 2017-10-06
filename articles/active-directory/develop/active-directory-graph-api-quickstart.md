---
title: aaaQuickstart pro hello Azure AD Graph API | Microsoft Docs
description: "Hello Azure Active Directory Graph API poskytuje programový přístup tooAzure AD prostřednictvím koncové body OData REST API. Aplikace můžete použít rozhraní Graph API tooperform hello vytvářet, číst, aktualizovat a odstraňovat operace na data adresáře a objekty."
services: active-directory
documentationcenter: n/a
author: viv-liu
manager: mbaldwin
editor: 
tags: 
ms.assetid: 9dc268a9-32e8-402c-a43f-02b183c295c5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: b4d3c57f06d212b1d095578f19bb86c932dbcc33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-for-hello-azure-ad-graph-api"></a>Rychlý úvodní kurz pro Azure AD Graph API hello
Hello rozhraní Graph API Azure Active Directory (AD) poskytuje programový přístup tooAzure AD prostřednictvím koncové body OData REST API. Aplikace můžete použít rozhraní Graph API tooperform hello vytvářet, číst, aktualizovat a odstraňovat operace na data adresáře a objekty. Můžete například použít hello rozhraní Graph API toocreate nového uživatele, zobrazit nebo aktualizovat vlastnosti uživatele, změnit heslo uživatele, zkontrolovat členství ve skupinách pro přístup na základě rolí, zakázat nebo odstranit hello uživatele. Další informace o funkcích rozhraní Graph API hello a scénáře aplikací, najdete v části [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) a [Azure AD Graph API požadavky](https://msdn.microsoft.com/library/hh974476.aspx). 

> [!IMPORTANT]
> Důrazně doporučujeme použít [Microsoft Graph](https://developer.microsoft.com/graph) místo Azure AD Graph API tooaccess prostředky služby Azure Active Directory. Náš vývojový program se nyní soustředí na Microsoft Graph a pro Azure AD Graph API nejsou plánovaná žádná další vylepšení. Je velmi omezený počet scénářů, pro které Azure AD Graph API může být vhodné; Další informace najdete v tématu hello [Microsoft Graph nebo hello Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) příspěvku na blogu v hello Office Dev Center.
> 
> 

## <a name="how-tooconstruct-a-graph-api-url"></a>Jak tooconstruct adresu URL rozhraní API grafu
V rozhraní Graph API, tooaccess data adresáře a objekty (jinými slovy, prostředky nebo entity), u kterých chcete operace CRUD tooperform můžete použít adresy URL založené na hello protokol (OData Open Data). Hello adresy URL používá v rozhraní Graph API obsahovat čtyři hlavní části: služby root, identifikátor klienta, cesta prostředku a možnosti řetězec dotazu: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Trvat hello příklad hello následující adresu URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

* **Služba kořenové**: V Azure AD Graph API, je v kořenovém adresáři hello služby je vždy https://graph.windows.net.
* **Identifikátor klienta**: v této části může být název ověřené domény (registrovaný), v předchozím příkladu hello contoso.com. Lze ji klienta objekt ID nebo hello "TatoOrganizace" nebo "ME." alias. Další informace najdete v tématu [adresování entity a operace v hello rozhraní Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
* **Cesta prostředku**: Tato část adresy URL identifikuje prostředek hello toobe zpracoval s (uživatelé, skupiny, určitého uživatele, nebo konkrétní skupiny atd.) V předchozím příkladu hello je tooaddress nejvyšší úrovně "skupiny" hello, který prostředek nastaven. Můžete také vyřešit konkrétní entitu, například "uživatelé / {objectId}" nebo "uživatelé nebo userPrincipalName".
* **Parametrů dotazu**: část cesty hello prostředků z oddílu parametry dotazu hello odděluje otazník (?). parametr dotazu "api-version" Hello je požadován u všech požadavků v hello rozhraní Graph API. Hello rozhraní Graph API také podporuje následující možnosti dotazu OData hello: **$filter**, **$orderby**, **$expand**, **$top**a **$format**. aktuálně nejsou podporovány následující možnosti dotazu Hello: **$count**, **$inlinecount**, a **$skip**. Další informace najdete v tématu [podporované dotazy, filtrů a možností stránkování v Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Verze rozhraní Graph API
Určíte v parametru dotazu "api-version" hello hello verze pro žádost o rozhraní Graph API. Pro verze 1.5 a novější použijte hodnotu numerické verze; rozhraní API-version = 1.6. U starších verzí použijte datum řetězec, který dodržuje toohello formát rrrr-MM-DD; například rozhraní api-version = 2013. 11 08. Pro funkce verze preview použijte hello řetězec "beta"; například rozhraní api-version = beta. Další informace o rozdílech mezi verzemi rozhraní Graph API najdete v tématu [Správa verzí Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Graf metadat rozhraní API
tooreturn hello soubor metadat rozhraní Graph API, přidejte segment hello "$metadata" po identifikátorů klienta hello hello adresu URL pro příklad hello následující adresu URL vrátí metadata pro ukázkové společnosti: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Můžete zadat tuto adresu URL v adresním řádku hello webové prohlížeče toosee hello metadat. Hello CSDL dokument metadat vrátil popisuje hello entity a komplexní typy, jejich vlastnosti a funkce hello a vystavené hello verzi rozhraní Graph API požadovaná akce. Vynechejte parametr api-version hello vrátí metadata pro hello nejnovější verzi.

## <a name="common-queries"></a>Běžné dotazy
[Azure AD Graph API běžné dotazy](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) jsou uvedeny běžné dotazy, které lze použít s hello Azure AD Graph, včetně dotazů, které se dají použít tooaccess nejvyšší úrovně prostředky v operacích tooperform adresář a dotazů ve vašem adresáři.

Například `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` vrátí společnosti informace pro adresář contoso.com.

Nebo `https://graph.windows.net/contoso.com/users?api-version=1.6` jsou uvedené všechny uživatelské objekty v adresáři contoso.com hello.

## <a name="using-hello-graph-explorer"></a>Pomocí hello Explorer grafu
Jak sestavit aplikaci, můžete použít hello grafu Explorer pro hello dat adresáře hello tooquery Azure AD Graph API.

Hello následuje výstup hello by v tématu, pokud byste byli toonavigate toohello Explorer grafu, přihlaste se a zadejte `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` toodisplay všechny uživatele v hello hello adresáře přihlášeného uživatele:

![Azure AD graph api explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Zatížení hello grafu Explorer**: tooload hello nástroj, přejděte příliš[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/). Klikněte na tlačítko **přihlášení** a přihlaste se pomocí vaší toorun přihlašovací údaje účtu Azure AD hello grafu Explorer proti vašeho klienta. Pokud spustíte Průzkumníka grafu proti vlastního klienta, vy nebo váš správce musí tooconsent během přihlášení. Pokud máte předplatné služeb Office 365, máte automaticky klient služby Azure AD. Hello přihlašovací údaje použijete toosign v tooOffice 365 jsou ve skutečnosti, účty Azure AD a můžete použít tyto přihlašovací údaje pomocí Průzkumníka grafu.

**Spuštění dotazu**: toorun dotazu, zadejte dotaz do textového pole hello žádost a klikněte na tlačítko **získat** nebo klikněte na tlačítko hello **zadejte** klíč. Zobrazí se v odpovědi hello Hello výsledky. Například `https://graph.windows.net/myorganization/groups?api-version=1.6` zobrazí seznam všech objektů skupiny v adresáři hello přihlášeného uživatele.

Poznámka: hello následující funkce a omezení hello Explorer grafu:

* Nastaví možnost automatického dokončování na prostředek. toosee tuto funkci, kliknutí na hello žádost textového pole (kde adresa URL hello společnosti se zobrazí). Můžete vybrat nastavení z rozevíracího seznamu hello prostředku.
* Podporuje hello "mě" a "TatoOrganizace" adresování aliasy. Například můžete použít `https://graph.windows.net/me?api-version=1.6` objekt uživatele hello tooreturn hello přihlášeného uživatele nebo `https://graph.windows.net/myorganization/users?api-version=1.6` tooreturn všechny uživatele v aktuálním adresáři hello.
* Oddíl hlavičky odpovědi. V této části můžete použít toohelp řešení problémů, ke kterým dochází při spuštění dotazů.
* Prohlížečem JSON pro odpověď hello se rozbalení a sbalení možnosti.
* Žádná podpora pro zobrazení miniaturu fotografie.

## <a name="using-fiddler-toowrite-toohello-directory"></a>Použití Fiddler toowrite toohello adresáře
Pro účely hello tohoto průvodce rychlý start můžete použít toopractice webové aplikaci Fiddler ladicího programu hello provádění zápisu operace u svého adresáře Azure AD. Další informace a tooinstall aplikaci Fiddler, najdete v části [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

V příkladu hello níže použijte ladicí program webové aplikaci Fiddler toocreate novou skupinu zabezpečení, MyTestGroup' v adresáři služby Azure AD.

**Získat přístupový token**: tooaccess Azure AD Graph, klienti jsou požadované toosuccessfully nejprve ověřit tooAzure AD. Další informace najdete v tématu [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md).

**Napište a spuštění dotazu**: dokončení hello následující kroky:

1. Otevřete webovou aplikaci Fiddler ladicího programu a přepněte toohello **autora** kartě.
2. Vzhledem k tomu, že chcete toocreate novou skupinu zabezpečení, vyberte **Post** jako hello metoda HTTP z rozevírací nabídky hello. Další informace o operacích a oprávnění na objekt skupiny najdete v tématu [skupiny](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) v rámci hello [Azure AD Graph REST API – referenční informace](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. V hello pole vedle příliš**Post**, zadejte následující hello jako hello URL požadavku: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.
   
   > [!NOTE]
   > Je třeba nahradit mytenantdomain s názvem domény hello adresáře Azure AD.
   > 
   > 
4. V poli hello přímo pod rozevírací Post zadejte následující hello:
   
    ```
   Host: graph.windows.net
   Authorization: Bearer <your access token>
   Content-Type: application/json
   ```
   
   > [!NOTE]
   > SUBSTITUTE vaše &lt;přístupový token&gt; s hello přístupový token pro váš adresář Azure AD.
   > 
   > 
5. V hello **text žádosti** pole, zadejte následující hello:
   
    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
   ```
   
    Další informace o vytváření skupin najdete v tématu [vytvořit skupinu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Další informace o Azure AD entity a typy, které jsou vystavené grafu a informace o operacích hello, které lze provést na nich grafu naleznete v tématu [Azure AD Graph REST API – referenční informace](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Další kroky
* Další informace o hello [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
* Další informace o [Azure AD Graph API oprávnění oborů](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

