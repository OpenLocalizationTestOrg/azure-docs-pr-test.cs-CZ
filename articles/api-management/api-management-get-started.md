---
title: "aaaManage vašeho prvního rozhraní API v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toocreate rozhraní API, přidávat operace a začít pracovat s API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 51b7df8b-1c43-43c6-90c9-0aa24f48206b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7d43f33aa359c4d1e605e9fb41e43d323ca6a777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-first-api-in-azure-api-management"></a>Správa vašeho prvního rozhraní API ve službě Azure API Management
## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak tooquickly začít s používáním Azure API Management a vaše první volání rozhraní API.

## <a name="concepts"></a>Co je Azure API Management?
Můžete použít jakýkoli back-end tootake Azure API Management a spustit plnohodnotný program API založené na něm.

Mezi obvyklé scénáře patří:

* **Zabezpečení mobilní infrastruktury** pomocí přístupu s klíči rozhraní API prostřednictvím brány, ochrana před útoky DOS pomocí omezování nebo používání rozšířených zásad zabezpečení, například ověřování tokenů JWT.
* **Povolování ekosystémů partnera** prostřednictvím nabídky rychlého připojení partnera skrze hello vývojáře portál a vytváření rozhraní API průčelí za toodecouple od interních implementací, které nejsou zralé k partner využíval tomu.
* **Spuštění interního programu s rozhraní API** prostřednictvím nabídky centralizovaného umístění hello organizace toocommunicate o dostupnosti hello a nejnovější změny tooAPIs, prostřednictvím brány přístup na základě organizační účtů, všechny na základě zabezpečeného kanálu mezi Brána Hello rozhraní API a back-end hello.

Hello systému se skládá z hello následující součásti:

* Hello **Brána rozhraní API** je koncový bod hello který:
  
  * Přijímá volání rozhraní API a směruje je back-EndY tooyour.
  * Ověřuje klíče rozhraní API, tokeny JWT, certifikáty a další přihlašovací údaje.
  * Vynucuje kvóty využití a omezení četnosti.
  * Transformuje rozhraní API hello chodu beze změny kódu.
  * Ukládá odezvy back-endu do určené mezipaměti.
  * Protokoluje metadata volání pro účely analýzy.
* Hello **portál vydavatele** je rozhraní pro správu hello kterém můžete nastavit program s rozhraním API. Použijte ho k následujícím akcím:
  
  * definování nebo import schématu rozhraní API
  * balení rozhraní API do produktů
  * Nastavení zásad, například kvót nebo transformací hello rozhraní API.
  * získání přehledů z analýz
  * správa uživatelů
* Hello **portál pro vývojáře** funguje jako hlavní webová služba hello pro vývojáře, kde můžete:
  
  * Číst dokumentaci k rozhraní API.
  * Vyzkoušejte rozhraní API prostřednictvím interaktivní konzoly hello.
  * Vytvořit účet a přihlásit se klíče tooget rozhraní API.
  * Přistupovat k analýzám jejich využití.

## <a name="create-service-instance"></a>Vytvoření instance API Managementu
> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][Azure Free Trial].
> 
> 

Hello prvním krokem při práci se službou API Management je toocreate instance služby. Přihlaste se toohello [portálu Azure] [ Azure Portal] a klikněte na tlačítko **nový**, **Web + mobilní**, **API Management**.

![Nová instance služby API Management][api-management-create-instance-menu]

Pro **název**, zadejte adresu URL služby hello toouse název jedinečné subdomény.

Zvolte hello potřeby **předplatné**, **skupiny prostředků** a **umístění** pro instanci služby.

Zadejte **Contoso Ltd.** pro hello **název organizace**a zadejte e-mailovou adresu v hello **e-mailu správce** pole.

> [!NOTE]
> Tato e-mailová adresa se používá pro oznámení z hello systému API Management. Další informace najdete v tématu [jak tooconfigure oznámení a e-mailových šablon ve službě Azure API Management][How tooconfigure notifications and email templates in Azure API Management].
> 
> 

![Nová služba API Management][api-management-create-instance-step1]

Instance služby API Management jsou dostupné ve třech úrovních: Developer (vývojář), Standard a Premium.

> [!NOTE]
> Úroveň Developer Hello je pro vývoj, testování a pilotní programy rozhraní API, kde vysokou dostupnost nehrají důležitou roli. V hello Standard a Premium vrstev je možné škálovat vaše toohandle počet jednotku rezervovanou větší provoz. úrovně Standard a Premium Hello poskytnout služby API Management s hello většina výpočetní výkon a zlepšit výkon. Tento kurz můžete dokončit pomocí libovolné úrovně. Další informace o úrovních služby API Management najdete v článku [Ceny služby API Management][API Management pricing].
> 
> 

Klikněte na tlačítko **vytvořit** toostart zřizování instanci služby.

![Nová služba API Management][api-management-instance-created]

Po vytvoření instance služby hello hello dalším krokem je toocreate nebo import rozhraní API.

## <a name="create-api"></a>Import rozhraní API
Rozhraní API se skládá ze sady operací, které můžete vyvolat z klientské aplikace. Operace rozhraní API jsou směrovány přes proxy server tooexisting webové služby.

Rozhraní API můžete vytvořit (a operace přidat) ručně, nebo je můžete importovat. V tomto kurzu naimportujeme rozhraní API hello ukázka kalkulačku webové služby poskytované společností Microsoft a je hostovaná v Azure.

> [!NOTE]
> Pokyny týkající se vytváření rozhraní API a ručnímu přidávání operací najdete v tématu [jak toocreate rozhraní API](api-management-howto-create-apis.md) a [jak tooan tooadd operace rozhraní API](api-management-howto-add-operations.md).
> 
> 

Rozhraní API se konfigurují na portálu vydavatele hello. tooreach, klikněte na tlačítko **portál vydavatele** z panelu nástrojů služby hello.

![Portál vydavatele][api-management-management-console]

tooimport hello kalkulačky rozhraní API, klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **rozhraní API pro Import**.

![Tlačítko Importovat rozhraní API][api-management-import-api]

Proveďte následující kroky rozhraní API kalkulačky hello tooconfigure hello:

1. Klikněte na tlačítko **z adresy URL**, zadejte **http://calcapi.cloudapp.net/calcapi.json** do hello **adresu URL dokumentu specifikace** textové pole a klikněte na tlačítko hello **Swagger**  přepínač.
2. Typ **calc** do hello **přípona adresy URL webového rozhraní API** textové pole.
3. Klikněte na tlačítko v hello **produkty (volitelné)** pole a zvolte **Starter**.
4. Klikněte na tlačítko **Uložit** tooimport hello rozhraní API.

![Přidání nového rozhraní API][api-management-import-new-api]

> [!NOTE]
> **API Management** aktuálně podporuje import dokumentu Swagger ve verzi 1.2 a 2.0. I když [specifikace Swagger 2.0](http://swagger.io/specification) uvádí, že vlastnosti `host`, `basePath` a `schemes` jsou volitelné, váš dokument Swagger 2.0 **MUSÍ** tyto vlastnosti obsahovat, jinak nepůjde importovat. 
> 
> 

Po importu rozhraní API hello hello souhrnná stránka rozhraní API hello zobrazí na portálu vydavatele hello.

![Souhrn rozhraní API][api-management-imported-api-summary]

Hello část rozhraní API obsahuje několik karet. Hello **Souhrn** karta zobrazuje základní metriky a informace o hello rozhraní API. Hello [nastavení](api-management-howto-create-apis.md#configure-api-settings) karta je použité tooview a úpravy hello konfiguraci pro rozhraní API. Hello [Operations](api-management-howto-add-operations.md) karta je operace použité toomanage hello rozhraní API. Hello **zabezpečení** karta může být použité tooconfigure ověřování brány pro back-end server hello pomocí základního ověření nebo [vzájemného ověření certifikátů](api-management-howto-mutual-certificates.md)a tooconfigure [ autorizace uživatelů pomocí standardu OAuth 2.0](api-management-howto-oauth2.md).  Hello **problémy** karta je použité tooview problémy hlášené hello vývojáře, kteří používají vaše rozhraní API. Hello **produkty** karta je použité tooconfigure hello produkty, které toto rozhraní API obsahují.

Ve výchozím nastavení každá instance služby API Management obsahuje dva ukázkové produkty:

* **Starter**
* **Unlimited**

V tomto kurzu přidala hello rozhraní API základní kalkulačky produktu Starter toohello při importu hello rozhraní API.

V pořadí toomake volání tooan rozhraní API vývojáři musí nejdřív přihlásit k odběru tooa produkt, který jim poskytne přístup tooit. Vývojáři se přihlásit k odběru tooproducts v portálu pro vývojáře hello nebo správci přihlásit k odběru tooproducts vývojáři portálu vydavatele hello. Vzhledem k tomu, že jste vytvořili instance služby API Management hello v hello předchozí kroky v kurzu hello, takže jste už odebírané tooevery produktu ve výchozím nastavení jste správcem.

## <a name="call-operation"></a>Volání operace z portálu pro vývojáře hello
Operace lze volat přímo z portálu pro vývojáře hello, který nabízí pohodlný způsob tooview a otestovat hello operace rozhraní API. V tomto kroku kurzu budete volat hello základní rozhraní API kalkulačky **přidat dvě celá čísla** operaci. Klikněte na tlačítko **portál pro vývojáře** hello nabídce v hello top pravé části portálu vydavatele hello.

![Portál pro vývojáře][api-management-developer-portal-menu]

Klikněte na tlačítko **rozhraní API** z hello horní nabídce a pak klikněte na tlačítko **Basic Calculator** toosee hello dostupné operace.

![Portál pro vývojáře][api-management-developer-portal-calc-api]

Všimněte si hello ukázkových popisů a parametrů naimportovaných společně s hello rozhraní API a operacemi, které poskytují dokumentaci vývojářům hello, které budou používat tuto operaci. Tyto popisy můžete přidat i v případě, kdy operace přidáváte ručně.

toocall hello **přidat dvě celá čísla** operace, klikněte na tlačítko **vyzkoušet**.

![Vyzkoušet][api-management-developer-portal-calc-api-console]

Zadejte hodnoty pro parametry hello nebo ponechat výchozí nastavení hello a pak klikněte na tlačítko **odeslat**.

![Metoda HTTP Get][api-management-invoke-get]

Po vyvolání operace portál pro vývojáře hello zobrazí hello **stav odpovědi**, hello **hlavičky odpovědi**a jakýkoli **obsah odpovědi**.

![Odpověď][api-management-invoke-get-response]

## <a name="view-analytics"></a>Zobrazení analýzy
tooview analýzu základní kalkulačky, portál vydavatele back toohello přepínač tak, že vyberete **spravovat** hello nabídce v hello top napravo od portál pro vývojáře hello.

![Spravovat][api-management-manage-menu]

Hello výchozím zobrazením portálu vydavatele hello je hello **řídicí panel**, který nabízí přehled o instanci služby API Management.

![Řídicí panel][api-management-dashboard]

Hover hello myš graf hello pro **Basic Calculator** toosee hello konkrétní metriky využití hello hello rozhraní API pro dané časové období.

> [!NOTE]
> Pokud na grafu nevidíte žádné čáry, přepněte zpět toohello portál pro vývojáře a proveďte několik volání do hello rozhraní API, chvíli počkejte a pak se vraťte toohello řídicího panelu.
> 
> 

Klikněte na tlačítko **zobrazit podrobnosti** tooview hello souhrnná stránka hello API včetně větší verze hello zobrazí metriky.

![Analýza][api-management-mouse-over]

![Souhrn][api-management-api-summary-metrics]

Podrobné metriky a sestavy, klikněte na tlačítko **Analytics** z hello **API Management** nabídky na levé straně hello.

![Přehled][api-management-analytics-overview]

Hello **Analytics** obsahuje hello následující čtyři karty:

* **Na první pohled** poskytuje celkové využití a stavu metriky, stejně jako hello nejlepší vývojáře, nejlepší produkty, nejlepší rozhraní API a nejlepší operace.
* **Využití** poskytuje hlubší pohled na volání a šířku pásma rozhraní API, včetně zeměpisného rozlišení.
* **Stav** se zaměřuje na stavové kódy, úspěšnost mezipaměti, doby odezvy a doby odezvy rozhraní API a služby.
* **Aktivita** poskytuje sestavy, které podrobně uvádějí hello konkrétní aktivitu podle vývojáře, produktu, rozhraní API a operace.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[ochrana rozhraní API omezením četnosti](api-management-howto-product-with-rules.md).

[Azure Free Trial]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add hello new API tooa product]: #add-api-to-product
[Subscribe toohello product that contains hello API]: #subscribe
[Call an operation from hello Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How toomanage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[How tooconfigure notifications and email templates in Azure API Management]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[API Management pricing]: http://azure.microsoft.com/pricing/details/api-management/

[Azure Portal]: https://portal.azure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
