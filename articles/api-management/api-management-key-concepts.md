---
title: "Přehled a klíč API Management koncepty aaaAzure | Microsoft Docs"
description: "Seznamte se s rozhraními API, produkty, rolemi, skupinami a dalšími klíčovými koncepty služby API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: e71da405-835a-48f3-956f-45c1a85698d7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: a77b24b4632d868afa15bd6cf88060982046cb38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-api-management"></a>Co je služba API Management?
API Management pomáhá organizacím při publikování rozhraní API tooexternal, partnerské a interní vývojáře toounlock hello potenciál jejich dat a služeb. Podnikům všude, kde jsou vyhledávání tooextend svoji činnost na digitální platformě, vytvářejí nové kanály, hledají nové zákazníky a více se propojují s těmi stávajícími. Služba API Management nabízí hello základní možnosti tooensure úspěšné programu s rozhraním API prostřednictvím zapojení vývojářů, informací o podniku, analýzy, zabezpečení a ochrany.

Podívejte se na hello následující video přehled služby Azure API Management a zjistěte, jak toouse API Management tooadd tooyour mnoho funkcí rozhraní API, včetně řízení přístupu, míra omezení, sledování, protokolování událostí a ukládání odpovědí do mezipaměti, s minimálním úsilím na vaší straně.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-API-Management-Overview/player]
> 
> 

toouse API Management vytvářejí správci rozhraní API. Každé rozhraní API se skládá z jedné nebo několika operací a každé rozhraní API je možné přidat tooone nebo více produktů. toouse rozhraní API, vývojáři předplatné tooa produkt, který obsahuje toto rozhraní API a potom můžou volat operace hello rozhraní API, předmět tooany použití zásady, které může být v platnosti.

Toto téma obsahuje přehled klíčových konceptů služby API Management.

> [!NOTE]
> Další informace najdete v tématu hello [cloudové API Management: využití síly rozhraní API hello](http://j.mp/ms-apim-whitepaper) dokument White Paper PDF. Tato bílá kniha s úvodními informacemi o službě API Management vytvořená společností CITO Research obsahuje: 
> 
> * Běžné požadavky a problémy rozhraní API
> * Oddělení rozhraní API a představení průčelí
> * Rychlý úvod pro vývojáře
> * Zabezpečení přístupu
> * Analýzy a metriky
> * Získání kontroly a přehledu pomocí platformy API Management
> * Použití cloudového nebo místního řešení
> * Azure API Management
> 
> 

## <a name="apis"></a>Rozhraní API a operace
Rozhraní API jsou základem hello instanci služby API Management. Každé rozhraní API představuje sadu operací k dispozici toodevelopers. Každé rozhraní API obsahuje odkaz na toohello back-end službu, která implementuje rozhraní API hello a jeho operací mapování toohello operace implementované back endové službě hello. Operace ve službě API Management jsou vysoce konfigurovatelné a umožňují kontrolu nad mapováním adres URL, parametry dotazů a cest, obsahem požadavků a odezev a ukládáním operací do mezipaměti. Limit rychlosti, kvóty a omezení zásady protokolu IP můžete implementovat také na úrovni jednotlivých operací nebo hello rozhraní API.

Další informace najdete v tématu [jak toocreate rozhraní API] [ How toocreate APIs] a [jak tooan tooadd operace rozhraní API][How tooadd operations tooan API].

## <a name="products"></a> Produkty
Jak rozhraní API jsou prezentované toodevelopers jsou produkty. Produkty v API Management mají jedno nebo několik rozhraní API a mají nakonfigurovaný název, popis a podmínky použití. Produkty můžou být **otevřené** nebo **chráněné**. Chráněných produktů musí být odebírané toobefore použitím, otevřené produkty můžete používat bez předplatného. Jakmile je produkt připravený k použití pro vývojáře, můžete ho publikovat. Jakmile je publikována, je možné zobrazit (a v hello případě chráněných produktů přihlásit k odběru) vývojáři. Schválení předplatného se konfiguruje na úrovni produktu hello a může vyžadovat schválení správce nebo schvalovány automaticky.

Skupiny jsou použité toomanage hello viditelnost toodevelopers produkty. Produkty udělují viditelnost toogroups a vývojáři můžou zobrazovat a odebírat toohello produkty, které jsou viditelné toohello skupiny, do které patří. 

Další informace najdete v tématu [jak toocreate a publikování produktu] [ How toocreate and publish a product] a hello následující videa.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="groups"></a> Skupiny
Skupiny jsou použité toomanage hello viditelnost toodevelopers produkty. API Management má následující neměnné systémové skupiny hello.

* **Správci** – členy této skupiny jsou správci předplatného Azure. Správci spravují instance služby API Management, vytváření hello rozhraní API, operace a produkty, které používají vývojáři.
* **Vývojáři** – do této skupiny patří ověření uživatelé portálu pro vývojáře. Vývojáři jsou zákazníci hello, kteří vytvářejí aplikace pomocí vašich rozhraní API. Vývojáři mají přístup k portálu pro vývojáře toohello a vytvářet aplikace, které volají operace rozhraní API hello.
* **Hosté** -neověření uživatelé portálu pro vývojáře, například potenciální zákazníci, kteří navštěvují portál pro vývojáře hello patří instance API Management do této skupiny. Je možné udělit určité jen pro čtení přístup, jako je například hello možnost tooview rozhraní API, ale není volat.

V přidání toothese systémových skupin můžou správci vytvářet vlastní skupiny nebo [využívat externí skupiny v přidružených klientech služby Azure Active Directory](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Vlastní a externí skupiny můžete používat společně se systémovými skupinami, která poskytuje vývojářům viditelnost a přístup k tooAPI produkty. Například může vytvořit jednu vlastní skupinu pro vývojáře spojené s konkrétní partnerské organizaci účty a povolit jim přístup toohello rozhraní API z produktu, který obsahuje jenom příslušná rozhraní API. Uživatel může být členem několika skupin.

Další informace najdete v tématu [jak toocreate a používání skupin][How toocreate and use groups].

## <a name="developers"></a> Vývojáři
Vývojáři představují hello uživatelské účty v instanci služby API Management. Vývojáře můžou vytvořit nebo pozvat toojoin správci, nebo se můžete zaregistrovat hello [portál pro vývojáře][Developer portal]. Každý vývojář je členem více skupin a může být přihlášení k odběru produktů toohello, která udělují viditelnost toothose skupiny.

Vývojáři se odběru tooa produktu, získají hello primární a sekundární klíč produktu hello. Tento klíč se používá při volání do rozhraní API produktu hello.

Další informace najdete v tématu [jak vývojáři toocreate nebo pozvání] [ How toocreate or invite developers] a [jak tooassociate skupin k vývojářům][How tooassociate groups with developers].

## <a name="policies"></a> Zásady
Zásady jsou vynikající funkcí služby API Management, který umožní hello vydavatele toochange hello chování hello rozhraní API prostřednictvím konfigurace. Zásady představují kolekci příkazů, které se postupně provádí na hello požadavku nebo odezvy z rozhraní API. Mezi oblíbené příkazy patří převod formátu XML tooJSON a volání míru omezení toorestrict hello množství příchozích volání od vývojáře a mnoho dalších zásad jsou k dispozici.

Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných hello zásad služby API Management, pokud hello zásady neurčí jinak. Některé zásady, jako je například hello [řízení toku](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) a [nastavená proměnná](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) jsou založené na výrazech zásad. Další informace najdete v tématu [pokročilé zásady](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx), a sledovat hello následující videa.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

Úplný seznam zásad služby API Management najdete v [referenční příručce o zásadách][Policy reference]. Další informace o používání a konfiguraci zásad najdete v článku [Zásady služby API Management][API Management policies]. Kurz týkající se vytváření produktu se zásadami kvót a omezování četnosti najdete v článku [Vytvoření a konfigurace pokročilých nastavení produktu][How create and configure advanced product settings]. Ukázku najdete v následujícím videu hello.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="developer-portal"></a> Portál pro vývojáře
portál pro vývojáře Hello je, kde vývojáři můžete další informace o vaše rozhraní API, zobrazit a volat operace a přihlásit se tooproducts. Potenciální zákazníci můžou navštívit portál pro vývojáře hello, zobrazení rozhraní API a operace a zaregistrovat. Hello adresa URL portálu pro vývojáře se nachází na panelu hello v hello portálu Azure Classic vaší instance služby API Management.

Hello vzhledu a chování portálu pro vývojáře můžete přizpůsobit přidáním vlastního obsahu, přizpůsobením stylů a přidáním brandingu.

## <a name="api-management-and-hello-api-economy"></a>API Management a hello hospodářství rozhraní API
Další informace o rozhraní API Management, sledovat hello následující prezentaci z konference Microsoft Ignite 2015 hello toolearn.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3708/player]
> 
> 

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Developer portal]: #developer-portal

[How toocreate APIs]: api-management-howto-create-apis.md
[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer
[How create and configure advanced product settings]: api-management-howto-product-with-rules.md
[How toocreate or invite developers]: api-management-howto-create-or-invite-developers.md
[Policy reference]: api-management-policy-reference.md
[API Management policies]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance




