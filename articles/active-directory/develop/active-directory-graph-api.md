---
title: aaaAzure Active Directory Graph API | Microsoft Docs
description: "Příručka přehled a rychlý start pro hello rozhraní Graph API, která umožňuje programový přístup k tooAzure AD prostřednictvím koncových bodů rozhraní REST API."
services: active-directory
documentationcenter: 
author: viv-liu
manager: mbaldwin
editor: mbaldwin
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: cde1dd86b0ca1dc24a5b46dc578b6245ba98751f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API
> [!IMPORTANT]
> Důrazně doporučujeme použít [Microsoft Graph](https://graph.microsoft.io/) místo Azure AD Graph API tooaccess prostředky služby Azure Active Directory. Náš vývojový program se nyní soustředí na Microsoft Graph a pro Azure AD Graph API nejsou plánovaná žádná další vylepšení. Je velmi omezený počet scénářů, pro které Azure AD Graph API může být vhodné; Další informace najdete v tématu hello [Microsoft Graph nebo hello Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) příspěvku na blogu v hello Office Dev Center.
> 
> 

Hello Azure Active Directory Graph API poskytuje programový přístup tooAzure AD prostřednictvím koncových bodů rozhraní REST API. Aplikace můžete použít rozhraní Graph API tooperform hello vytvářet, číst, aktualizovat a odstraňovat operace na data adresáře a objekty. Například hello rozhraní Graph API podporuje hello následující běžných operací pro objekt uživatele:

* Vytvoření nového uživatele v adresáři
* Získat podrobné vlastnosti uživatele, jako je například jejich skupin
* Aktualizovat vlastnosti uživatele, například jejich umístění a telefonní číslo, nebo změnit své heslo
* Zkontrolovat členství ve skupinách uživatele pro přístup na základě rolí
* Zakažte účet uživatele nebo zcela odstranit

Přidání toouser objekty můžete provést podobnými operacemi na jiné objekty, jako jsou skupiny a aplikace. toocall hello rozhraní Graph API v adresáři, hello aplikace musí být zaregistrované v Azure AD a být nakonfigurované tooallow přístup k toohello adresáři. Dosahuje se obvykle prostřednictvím k toku souhlasu uživatele nebo správce.

pomocí toobegin hello Azure Active Directory Graph API, najdete v části hello [Průvodce rychlým zahájením Graph API](active-directory-graph-api-quickstart.md), nebo zobrazení hello [interaktivní referenční dokumentace rozhraní Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Funkce
Hello rozhraní Graph API poskytuje hello následující funkce:

* **Koncové body REST API**: hello rozhraní Graph API je služba RESTful skládá z koncových bodů, které jsou přístupné pomocí standardní požadavky HTTP. Hello rozhraní Graph API podporuje typy obsahu XML nebo Javascript Object Notation (JSON) pro požadavky a odpovědi. Další informace najdete v tématu [Azure AD Graph REST API – referenční informace](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Ověřování s Azure AD**: každý požadavek toohello rozhraní Graph API musí být ověřeny připojením JSON Web Token (JWT) v hlavičce autorizace hello hello požadavku. Tento token se získal tím, že koncový bod tokenu tooAzure požadavek na AD a poskytnutí platné přihlašovací údaje. Můžete použít hello tok přihlašovacích údajů klienta OAuth 2.0 nebo poskytování autorizačních kódů hello tooacquire tok tokenu toocall hello grafu. Další informace najdete [OAuth 2.0 ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Na základě rolí autorizace (RBAC)**: skupiny zabezpečení jsou použité tooperform RBAC v hello rozhraní Graph API. Pokud chcete toodetermine, zda má uživatel přístup tooa konkrétní prostředek, například může aplikace hello volání hello [zkontrolovat členství ve skupině (přenosné)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) operaci, která vrátí hodnotu true nebo false.
* **Rozdílovou dotazu**: Pokud chcete toocheck pro změny v adresáři mezi dvěma časových období bez nutnosti toomake časté dotazy toohello rozhraní Graph API, musíte provést žádost rozdílové dotazu. Tento typ požadavku vrátí pouze hello změny mezi hello předchozí požadavek rozdílové dotazu a aktuální žádost hello. Další informace najdete v tématu [Azure AD Graph API rozdílové dotazu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Rozšíření adresáře**: Pokud vyvíjíte aplikaci, která potřebuje tooread nebo zapisovat jedinečné vlastnosti pro objekty adresáře, můžete zaregistrovat a používání rozšíření hodnoty pomocí hello rozhraní Graph API. Například pokud vaše aplikace vyžaduje Skype ID vlastnosti pro každého uživatele, můžete zaregistrovat nové vlastnosti hello v adresáři hello a bude k dispozici na každý objekt uživatele. Další informace najdete v tématu [Azure AD Graph API rozšíření schématu služby Directory](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Zabezpečené obory oprávnění**: rozhraní AAD Graph API zpřístupní obory oprávnění, které umožňují zabezpečit souhlas přístup tooAAD dat a podporují celou řadu typů aplikace klienta, včetně:
  
  * ty s uživatelským rozhraním, které jsou uvedeny Delegovaný přístup k toodata prostřednictvím autorizace z hello přihlášeného uživatele (delegovaný)
  * ty, které používají aplikace definovat řízení přístupu na základě rolí například klienti služby nebo démon (role aplikace)
    
    Obě delegovat a obory oprávnění role aplikace představují oprávnění vystavené hello rozhraní Graph API a může požadovat klientské aplikace prostřednictvím oprávnění k registraci aplikací [funkcí v hello portál Azure](https://portal.azure.com). Klienty můžete ověřit hello obory oprávnění udělená toothem zkontrolováním deklarace oboru (spojovací bod "služby") hello dostali hello přístupový token pro přidělená oprávnění a rolí hello ("role") deklarace identity pro oprávnění role aplikace. Další informace o [Azure AD Graph API oprávnění obory](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).

## <a name="scenarios"></a>Scénáře
Hello rozhraní Graph API umožňuje mnoho scénářů s aplikací. Hello následující scénáře jsou hello nejběžnější:

* **Obchodní (jednoho klienta) aplikace**: V tomto scénáři funguje vývojář enterprise pro organizaci, která má předplatné služeb Office 365. Hello vývojáře je vytváření webové aplikace, která komunikuje s Azure AD tooperform úlohy, například přiřazení licence uživatele tooa. Tato úloha vyžaduje toohello přístup k rozhraní Graph API, tak, aby hello vývojáře zaregistruje hello jednoho klienta aplikace v Azure AD a nakonfiguruje oprávnění čtení a zápisu pro hello rozhraní Graph API. Potom hello aplikace je nakonfigurovaná toouse buď svoje vlastní přihlašovací údaje nebo těch, které aktuálně přihlášení uživatele tooacquire hello tokenu toocall hello rozhraní Graph API.
* **Software jako služba aplikace (víceklientské)**: V tomto scénáři je nezávislý dodavatel softwaru (ISV) vývoj hostované víceklientské webové aplikace, která poskytuje funkce správy uživatele pro jiné organizace, které používají Azure AD. Tyto funkce vyžadují přístup k objektům toodirectory, a proto aplikace hello musí toocall hello rozhraní Graph API. Hello vývojáře zaregistruje hello aplikace ve službě Azure AD, nakonfiguruje jej číst toorequire a oprávnění pro hello rozhraní Graph API pro zápis a pak umožňuje externí přístup tak, aby dalšími organizacemi, můžete souhlas aplikace hello toouse v jejich adresáře. Při ověření uživatele v jiné organizaci toohello aplikace hello poprvé, zobrazí se dialogové okno souhlasu s hello oprávnění, která hello aplikace požaduje.  Udělení souhlasu pak získáte aplikace hello ty požadovaná oprávnění toohello rozhraní Graph API adresáře hello uživatele. Další informace o hello souhlasu framework najdete v tématu [přehled hello souhlas Framework](active-directory-integrating-applications.md).

## <a name="see-also"></a>Viz také
[Průvodce rychlým zahájením Azure AD Graph API](active-directory-graph-api-quickstart.md)

[Dokumentace k AD Graph REST](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Příručka pro vývojáře pro službu Azure Active Directory](active-directory-developers-guide.md)

