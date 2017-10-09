---
title: aaaAdd tooan CDN Azure App Service | Microsoft Docs
description: "Přidejte toocache Content Delivery Network (CDN) tooan Azure App Service a poskytovat statické soubory ze serverů zavřít tooyour zákazníků ohledně hello, world."
services: app-service\web
author: syntaxc4
ms.author: cfowler
ms.date: 05/31/2017
ms.topic: tutorial
ms.service: app-service-web
manager: erikre
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 88b7fd884517279064472b804a6d1dc2921cbd24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-content-delivery-network-cdn-tooan-azure-app-service"></a>Přidat tooan Content Delivery Network (CDN) Azure App Service

[Sítě doručování Azure obsahu (CDN)](../cdn/cdn-overview.md) ukládá do mezipaměti na strategicky umístěných místech tooprovide maximální propustnost pro doručování obsahu toousers statický webový obsah. Hello CDN také snižuje zatížení serveru ve webové aplikaci. Tento kurz ukazuje, jak Azure CDN tooa tooadd [webové aplikace v Azure App Service](app-service-web-overview.md). 

Tady je hello domovskou stránku hello ukázka statické HTML webového serveru, který bude fungovat s:

![Domovská stránka ukázkové aplikace](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

Získáte informace:

> [!div class="checklist"]
> * Vytvořit koncový bod CDN.
> * Aktualizovat prostředky uložené v mezipaměti.
> * Použití dotazu řetězce verze toocontrol do mezipaměti.
> * Použijte vlastní doménu pro koncový bod CDN hello.

## <a name="prerequisites"></a>Požadavky

toocomplete v tomto kurzu:

- [Nainstalovat Git](https://git-scm.com/).
- [Instalace rozhraní příkazového řádku Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-web-app"></a>Vytvoření webové aplikace hello

toocreate hello webovou aplikaci, která bude fungovat s, postupujte podle hello [statické rychlý start HTML](app-service-web-get-started-html.md) prostřednictvím hello **procházet toohello aplikace** krok.

### <a name="have-a-custom-domain-ready"></a>Připravení vlastní domény

toocomplete hello vlastní domény krok tohoto kurzu potřebujete tooown vlastní doménu a mít přístup tooyour DNS registru pro poskytovatele domény (například GoDaddy). Tooadd záznamy DNS, třeba pro `contoso.com` a `www.contoso.com`, musíte mít přístup tooconfigure hello nastavení DNS pro hello `contoso.com` kořenové domény.

Pokud ještě nemáte název domény, zvažte následující hello [kurzu domény služby App Service](custom-dns-web-site-buydomains-web-app.md) hello toopurchase domény pomocí portálu Azure. 

## <a name="log-in-toohello-azure-portal"></a>Přihlaste se toohello portálu Azure

Otevřete prohlížeč a přejděte toohello [portál Azure](https://portal.azure.com).

## <a name="create-a-cdn-profile-and-endpoint"></a>Vytvoření koncového bodu a profilu CDN

V levé navigační hello, vyberte **App Services**a pak vyberte hello aplikaci, kterou jste vytvořili v hello [statické rychlý start HTML](app-service-web-get-started-html.md).

![Vyberte aplikace App Service hello portálu](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

V hello **služby App Service** stránku hello **nastavení** vyberte **sítě > Konfigurovat Azure CDN pro vaši aplikaci**.

![Vyberte CDN hello portálu](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

V hello **Azure Content Delivery Network** zadejte hello **nový koncový bod** nastavení uvedeného v tabulce hello.

![Vytvoření profilu a koncového bodu hello portálu](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| Nastavení | Navrhovaná hodnota | Popis |
| ------- | --------------- | ----------- |
| **Profil CDN** | myCDNProfile | Vyberte **vytvořit nový** toocreate profilu CDN. Profil CDN je kolekce koncové body CDN pomocí hello stejné cenová úroveň. |
| **Cenová úroveň** | Akamai Standard | Hello [cenová úroveň](../cdn/cdn-overview.md#azure-cdn-features) určuje hello zprostředkovatele a dostupných funkcí. V tomto kurzu používáme Standard Akamai. |
| **Název koncového bodu CDN** | Libovolný název, který je jedinečný v doméně azureedge.net hello | Přístup k prostředkům v mezipaměti v doméně hello  *\<endpointname >. azureedge.net*.

Vyberte **Vytvořit**.

Azure vytvoří hello profilu a koncového bodu. Nový koncový bod Hello se zobrazí v hello **koncové body** seznam na hello stejné stránce, a pokud je zřízený hello stav **systémem**.

![Nový koncový bod v seznamu](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-hello-cdn-endpoint"></a>Test hello koncový bod CDN

Pokud jste vybrali cenovou úroveň Verizon, rozšíření koncového bodu obvykle trvá přibližně 90 minut. U Akamai trvá rozšíření několik minut.

Ukázková aplikace Hello má `index.html` souboru a *šablon stylů css*, *img*, a *js* složek, které obsahují další statické prostředky. Hello obsahu cesty pro všechny tyto soubory jsou stejné hello na koncový bod CDN hello. Například obě hello následující adresy URL přístup hello *bootstrap.css* souboru v hello *šablon stylů css* složky:

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

Přejděte následující adresu URL toohello prohlížeče:

```
http://<endpointname>.azureedge.net/index.html
```

![Domovská stránka ukázkové aplikace poskytnutá z CDN](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

 Zobrazí hello stejné stránka byla spuštěna dříve v webové aplikace Azure. Azure CDN má načíst prostředky hello počátek webové aplikace a jejich obsluhuje z koncového bodu CDN hello

tooensure, tato stránka je do mezipaměti v hello CDN, stránku hello aktualizace. Dva požadavky pro hello stejné asset jsou někdy požadované pro hello CDN toocache hello požadovaný obsah.

Další informace o vytváření koncových bodů a profilů CDN najdete v tématu [Začínáme s Azure CDN](../cdn/cdn-create-new-endpoint.md).

## <a name="purge-hello-cdn"></a>Vyprázdnění hello CDN

Hello CDN se pravidelně aktualizuje jejích prostředcích z hello počátek webové aplikace na základě hello time to live (TTL) konfigurace. Hello výchozí hodnota TTL je sedm dní.

V některých případech bude pravděpodobně nutné toorefresh hello CDN před hello vypršení platnosti TTL – například když nasadíte aktualizované obsahu toohello webové aplikace. tootrigger aktualizace, můžete ručně vyprázdnit hello CDN prostředky. 

V této části kurzu hello nasazení změn toohello webové aplikace a mazání hello CDN tootrigger hello CDN toorefresh své mezipaměti.

### <a name="deploy-a-change-toohello-web-app"></a>Nasazení webové aplikace toohello změn

Otevřete hello `index.html` souboru a přidejte "-V2" toohello H1 záhlaví, jak je znázorněno v hello následující ukázka: 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

Potvrdit změny a nasaďte ji toohello webové aplikace.

```bash
git commit -am "version 2"
git push azure master
```

Po dokončení nasazení adresy URL procházet toohello webové aplikace a v tématu hello změnit.

```
http://<appname>.azurewebsites.net/index.html
```

![V2 v nadpisu ve webové aplikaci](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

Vyhledejte koncový bod CDN toohello adresa URL pro domovskou stránku hello a nevidíte hello změnit, protože v mezipaměti verzi hello v hello CDN ještě nevypršela. 

```
http://<endpointname>.azureedge.net/index.html
```

![Žádné V2 v nadpisu v CDN](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-hello-cdn-in-hello-portal"></a>Vyprázdnění hello CDN hello portálu

tootrigger hello CDN tooupdate jeho verze v mezipaměti, vyprázdnit hello CDN.

V levém navigačním hello portálu, vyberte **skupiny prostředků**a pak vyberte skupinu prostředků hello, kterou jste vytvořili pro webové aplikace (myResourceGroup).

![Výběr skupiny prostředků](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

V seznamu hello prostředků vyberte koncový bod CDN.

![Výběr koncového bodu](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

Hello horní části hello **koncový bod** klikněte na tlačítko **mazání**.

![Výběr možnosti Vyprázdnit](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

Zadejte cesty obsahu hello chcete toopurge. Můžete předat toopurge cesta dokončení souboru jednotlivých souborů nebo toopurge segmentu cesty a obnovit veškerý obsah ve složce. Vzhledem k tomu, že jste změnili `index.html`, ujistěte se, který je jedním z cesty hello.

V dolní části hello hello stránky, vyberte **mazání**.

![Stránka Vyprázdnit](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-hello-cdn-is-updated"></a>Ověřte, že hello, který se aktualizuje CDN

Počkejte, dokud žádost o vyprázdnění hello dokončí zpracování, obvykle během několika minut. toosee hello aktuální stav, vyberte hello ikonu zvonku v horní části hello hello stránky. 

![Oznámení vyprázdnění](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

Vyhledejte adresu URL koncového bodu CDN toohello pro `index.html`, a nyní se zobrazí hello V2 přidat název toohello na domovskou stránku hello. Ukazuje to, že hello CDN mezipaměti byla aktualizována.

```
http://<endpointname>.azureedge.net/index.html
```

![V2 v nadpisu v CDN](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

Další informace najdete v tématu [Vyprázdnění koncového bodu Azure CDN](../cdn/cdn-purge-endpoint.md). 

## <a name="use-query-strings-tooversion-content"></a>Použít obsah tooversion řetězce dotazu

Hello Azure CDN nabízí následující možnosti ukládání do mezipaměti chování hello:

* Ignorovat řetězce dotazu
* Nepoužívat ukládání do mezipaměti pro řetězce dotazu
* Ukládat do mezipaměti každou jedinečnou adresu URL 

Hello první z nich je hello výchozí, což znamená, existuje jenom jedna verze uložené v mezipaměti majetku bez ohledu na to hello řetězec dotazu v hello URL. 

V této části kurzu hello změníte hello ukládání do mezipaměti chování toocache každou jedinečnou adresu URL.

### <a name="change-hello-cache-behavior"></a>Změna chování mezipaměti hello

V portálu Azure hello **koncový bod CDN** vyberte **mezipaměti**.

Vyberte **do mezipaměti každou jedinečnou adresu URL** z hello **chování ukládání řetězců s dotazy** rozevíracího seznamu.

Vyberte **Uložit**.

![Výběr chování při ukládání řetězce dotazu do mezipaměti](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a>Ověření samostatného ukládání jedinečných adres URL do mezipaměti

V prohlížeči přejděte toohello domovské stránce na koncový bod CDN hello, ale obsahovat řetězec dotazu: 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

Hello CDN vrátí hello aktuální webové aplikace obsah, který obsahuje v záhlaví hello "V2". 

tooensure, tato stránka je do mezipaměti v hello CDN, stránku hello aktualizace. 

Otevřete `index.html` a změňte "V2" příliš "V3" a změnu hello. 

```bash
git commit -am "version 3"
git push azure master
```

Přejděte v prohlížeči, adresa URL koncového bodu CDN toohello s novou řetězce dotazu, jako `q=2`. Hello CDN získá hello aktuální `index.html` souboru a zobrazí "V3".  Ale pokud přejdete koncový bod CDN toohello s hello `q=1` dotaz na řetězec, najdete v části "V2".

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![V3 v nadpisu v CDN, řetězec dotazu 2](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![V2 v nadpisu v CDN, řetězec dotazu 1](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

Výstup ukazuje, že každý řetězec dotazu je zpracovávat odděleně:

* q = 1 byl používán před, takže obsah uložený v mezipaměti, jsou vráceny (V2).
* q = 2 je novou, proto se obsah hello nejnovější webové aplikace načíst a vrátí (V3).

Další informace najdete v tématu [Řízení chování Azure CDN při ukládání řetězců dotazu do mezipaměti](../cdn/cdn-query-string.md).

## <a name="map-a-custom-domain-tooa-cdn-endpoint"></a>Mapa koncový bod CDN tooa vlastní doménu.

Vaše vlastní doména tooyour koncový bod CDN budete mapovat vytvořením záznamu CNAME. Záznam CNAME je funkce DNS, který se mapuje zdrojové domény tooa cílové domény. Například může mapování `cdn.contoso.com` nebo `static.contoso.com` příliš`contoso.azureedge.net`.

Pokud nemáte vlastní doménu, zvažte následující hello [kurzu domény služby App Service](custom-dns-web-site-buydomains-web-app.md) hello toopurchase domény pomocí portálu Azure. 

### <a name="find-hello-hostname-toouse-with-hello-cname"></a>Najít název hostitele toouse hello s hello CNAME

V portálu Azure hello **koncový bod** se přesvědčte, že **přehled** je vybraný v levé navigaci a potom vyberte hello hello **+ vlastní domény** tlačítko hello horní části stránky hello.

![Výběr možnosti Přidat vlastní doménu](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

V hello **přidat vlastní doménu** stránky, uvidíte hello koncový bod hostitele název toouse vytvořit záznam CNAME. název hostitele Hello je odvozený od vaše adresa URL koncového bodu CDN:  **&lt;EndpointName >. azureedge.net**. 

![Stránka Přidat doménu](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-hello-cname-with-your-domain-registrar"></a>Konfigurace hello CNAME u vašeho registrátora domény

Navigace webu tooyour doménového registrátora a vyhledejte oddíl hello pro vytvoření záznamů DNS. Může to být v části, jako je **Název domény**, **DNS** nebo **Správa názvového serveru**.

Najít oddíl hello správy záznamů CNAME. Může mít toogo tooan Upřesnit nastavení stránky a vyhledejte slova hello CNAME, Alias nebo subdomény.

Vytvořit záznam CNAME, který mapuje vaši zvolenou subdomény (například **statické** nebo **cdn**) toohello **názvu hostitele koncového bodu** uvedené výše v portálu hello. 

### <a name="enter-hello-custom-domain-in-azure"></a>Zadejte vlastní domény hello v Azure

Vrátí toohello **přidat vlastní doménu** stránky a zadejte vlastní domény, včetně hello subdomény, v dialogovém okně hello. Zadejte například `cdn.contoso.com`.   
   
Azure ověřuje, zda existuje záznam CNAME hello pro hello název domény, který jste zadali. Pokud hello CNAME je správná, je ověřit vaši vlastní doménu.

Pro hello CNAME záznamů toopropagate tooname servery na hello Internetu může trvat dobu. Pokud vaše doména není ověřený okamžitě, počkejte několik minut a zkuste to znovu.

### <a name="test-hello-custom-domain"></a>Test hello vlastní domény

V prohlížeči přejděte toohello `index.html` souboru používání vlastní domény (například `cdn.contoso.com/index.html`) tooverify, která je výsledkem hello text hello, stejně jako když přejdete přímo příliš`<endpointname>azureedge.net/index.html`.

![Domovská stránka ukázkové aplikace s použitím adresy URL vlastní domény](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

Další informace najdete v tématu [vlastní doménu CDN Azure mapa obsahu tooa](../cdn/cdn-map-content-to-custom-domain.md).

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

Co jste se naučili:

> [!div class="checklist"]
> * Vytvořit koncový bod CDN.
> * Aktualizovat prostředky uložené v mezipaměti.
> * Použití dotazu řetězce verze toocontrol do mezipaměti.
> * Použijte vlastní doménu pro koncový bod CDN hello.

Zjistěte, jak toooptimize CDN výkon v hello následující články:

> [!div class="nextstepaction"]
> [Vylepšení výkonu prostřednictvím komprimace souborů v Azure CDN](../cdn/cdn-improve-performance.md)

> [!div class="nextstepaction"]
> [Předběžné načtení prostředků v koncovém bodu Azure CDN](../cdn/cdn-preload-endpoint.md)
