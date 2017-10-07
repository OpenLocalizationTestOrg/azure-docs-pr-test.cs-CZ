---
title: "aaaHow tooprovide zabezpečený vzdálený přístup tooon místní aplikace"
description: "Popisuje, jak toouse Azure AD Application Proxy tooprovide zabezpečený vzdálený přístup tooyour místní aplikace."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d5450da1-9e06-4d08-8146-011c84922ab5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 289e970ed0596fcd06ccf6b2ad92203366fbb494
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprovide-secure-remote-access-tooon-premises-applications"></a>Jak tooprovide zabezpečený vzdálený přístup tooon místní aplikace

Zaměstnanci dnes mají toobe produktivitu kdekoli, kdykoli a z jakéhokoli zařízení. Chtějí toowork na jejich vlastních zařízení a to jak tablety a telefony, přenosné počítače. A očekávané možné tooaccess toobe jejich vlastních aplikací, aplikace SaaS v hello cloudové a podnikové aplikace v místním. Poskytování přístupu tooon místní aplikace má tradičně zahrnuta, virtuálním privátním sítím (VPN) nebo demilitarizovaná zón (zóny DMZ). Nejen jsou komplexní a pevné toomake těchto řešení zabezpečení, ale jsou nákladná tooset a spravovat.

Je lepší způsob!

Moderní pracovních sil v hello včasného mobilní, cloudové první world musí řešení moderní vzdáleného přístupu. Proxy aplikace služby Azure AD je funkce služby Azure Active Directory, která nabízí vzdáleného přístupu jako služba. To znamená, že je snadno toodeploy, používat a spravovat.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="what-is-azure-active-directory-application-proxy"></a>Co je Azure Proxy aplikace služby Active Directory?
Proxy aplikace služby Azure AD poskytuje jednotné přihlašování (SSO) a zabezpečený vzdálený přístup pro webové aplikace hostované na místě. Některé aplikace, je vhodnější toopublish patří lokality, Outlook Web Access nebo jiné obchodní webové aplikace, které máte služby SharePoint. Tyto místní webová aplikace jsou integrované s Azure AD, hello stejnou identitu a řídit platforma, která je používána O365. Koncoví uživatelé můžou používat vaše místní aplikace hello stejným způsobem jako přístupu k O365 a jinými aplikacemi SaaS integrované s Azure AD. Není nutné toochange hello síťovou infrastrukturu nebo vyžadovat VPN tooprovide toto řešení pro vaše uživatele.

## <a name="why-is-application-proxy-a-better-solution"></a>Proč je Proxy aplikace lepší řešení?
Proxy aplikace služby Azure AD poskytuje místní aplikace řešení tooall jednoduchý, zabezpečenou a nákladově efektivní vzdáleného přístupu.

Proxy aplikace služby Azure AD je:

* **Jednoduché**
   * Není nutné toochange nebo aktualizovat toowork vaší aplikace pomocí Proxy aplikace. 
   * Uživatelé získají konzistentní přihlašování. Uživatelé můžou používat aplikace SaaS tooboth hello MyApps portálu tooget jednotné přihlašování v cloudu hello a aplikace místní. 
* **Zabezpečení**
   * Při publikování aplikací pomocí proxy aplikace služby Azure AD, můžete využít hello bohaté autorizace a analýza zabezpečení v Azure. Získáte cloudového škálovatelného zabezpečení a funkcí zabezpečení Azure, jako je podmíněný přístup a dvoustupňové ověření.
   * Nemáte tooopen všechna příchozí připojení prostřednictvím vaší brány firewall toogive vzdálený přístup vašich uživatelů. 
* **Nákladově efektivní**
   * Proxy aplikací funguje v cloudu hello, takže můžete ušetřit čas a peníze. Místní řešení obvykle vyžadují tooset nahoru a udržovat zóny DMZ, servery edge nebo jiné komplexní infrastruktury.  

## <a name="what-kind-of-applications-work-with-application-proxy"></a>Jaký druh pracovní aplikací pomocí Proxy aplikace?
S Azure AD Application Proxy se můžete dostat různé typy interní aplikace:

* Webové aplikace, které používají [integrované ověřování systému Windows](active-directory-application-proxy-sso-using-kcd.md) pro ověřování  
* Webové aplikace, které používají založené na formulářích nebo [na základě záhlaví](application-proxy-ping-access.md) přístup  
* Webové rozhraní API, které mají tooexpose toorich aplikací na různých zařízeních  
* Aplikace hostované za [Brána vzdálené plochy](application-proxy-publish-remote-desktop.md)  
* Bohaté klientských aplikací, které jsou integrované s hello Active Directory Authentication Library (ADAL)

## <a name="how-does-application-proxy-work"></a>Jak funguje Proxy aplikace?
Existují dvě součásti, je nutné, aby tooconfigure toomake pracovní Proxy aplikace: konektoru a externí koncový bod. 

konektor Hello je lightweight agenta, která se nachází na serveru Windows uvnitř vaší sítě. konektor Hello usnadňuje hello přenosy z hello Proxy aplikace služby v hello cloudu tooyour aplikace místně. Proto nemusíte mít tooopen žádné příchozí porty nebo nic umístit v hraniční síti hello odchozí připojení, používá se jenom. Hello konektory jsou bezstavové a načítat informace z cloudu hello podle potřeby. Další informace o konektory, podobně jako postupy jejich vyrovnávání zatížení a ověření, najdete v části [pochopit Azure AD Application Proxy konektory](application-proxy-understand-connectors.md). 

externí koncový bod Hello je, jak uživatelé kontaktovat vaše aplikace při mimo vaši síť. Můžete buď přejde se přímo externí adresu URL tooan, který určíte, nebo přístupem hello aplikace prostřednictvím portálu MyApps hello. Když uživatelé přejít tooone těchto koncových bodů, ověřování ve službě Azure AD a jsou poté směrován přes hello konektor toohello místní aplikace.

 ![Diagram AzureAD Proxy aplikace](./media/active-directory-application-proxy-get-started/azureappproxxy.png)

1. Hello uživatel přistupuje k aplikaci hello prostřednictvím hello Proxy aplikace služby a je tooauthenticate přihlašovací stránce směrovanou toohello Azure AD.
2. Token je po úspěšného přihlášení, generovány a odeslány toohello klientského zařízení.
3. Hello klient odešle hello tokenu toohello Proxy aplikace služby, která načte hello hlavní název uživatele (UPN) a název objektu zabezpečení (SPN) z tokenu hello pak přesměruje konektor Proxy aplikace toohello požadavek hello.
4. Pokud jste nakonfigurovali jednotné přihlašování, provede hello konektoru vyžadováno jménem uživatele hello žádné další ověření.
5. konektor Hello odesílá hello požadavek toohello místní aplikace.  
6. Hello odpovědi se budou odesílat prostřednictvím Proxy aplikace služby a konektor toohello uživatele.

### <a name="single-sign-on"></a>Jednotné přihlašování
Proxy aplikace služby Azure AD poskytuje jednotné přihlašování (SSO) tooapplications, které používají integrované ověřování systému Windows (IWA) nebo deklaracemi identity aplikace. Pokud vaše aplikace používá integrované ověřování systému Windows, zosobňuje Proxy aplikace hello uživatele s využitím omezené delegování Kerberos tooprovide jednotné přihlašování. Pokud máte deklaracemi identity aplikace, která vztahy důvěryhodnosti služby Azure Active Directory, jednotné přihlašování funguje, protože uživatel hello byl již ověřen službou Azure AD.

Další informace o protokolu Kerberos najdete v tématu [všechny chcete tooknow o použitím protokolu Kerberos omezené delegování (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

### <a name="managing-apps"></a>Správa aplikací
Jednu aplikaci je publikována pomocí Proxy aplikace, můžete ji spravovat stejně jako jiná podnikové aplikace v hello portálu Azure. Můžete použít funkce zabezpečení Azure Active Directory, jako je podmíněný přístup a dvoustupňové ověření, řídit oprávnění uživatele a přizpůsobit hello branding pro vaši aplikaci. 

## <a name="get-started"></a>Začínáme

Než začnete konfigurovat Proxy aplikace, zajistěte, aby byla podporována [edice služby Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/) a adresář Azure AD, pro který jste globálním správcem.

Začínáme s Proxy aplikace ve dvou krocích:

1. [Povolení Proxy aplikace a nakonfigurujte konektor hello](active-directory-application-proxy-enable.md).    
2. [Publikování aplikací](active-directory-application-proxy-publish.md) -použití hello rychlý a snadný Průvodce tooget vaše místní aplikace publikována a dostupné vzdáleně.

## <a name="whats-next"></a>Co dále?
Jakmile publikujete první aplikace, existuje mnoho dalších úkonů, které můžete provést pomocí Proxy aplikace:

* [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
* [Publikování aplikací s použitím vlastního názvu domény](active-directory-application-proxy-custom-domains.md)
* [Další informace o Azure AD Application Proxy konektory](application-proxy-understand-connectors.md)
* [Práce s existující místní Proxy servery](application-proxy-working-with-proxy-servers.md) 
* [Nastavit vlastní domovskou stránku](application-proxy-office365-app-launcher.md)

Hello nejnovější novinky a aktualizace, najdete na naší hello [blogu Proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)

