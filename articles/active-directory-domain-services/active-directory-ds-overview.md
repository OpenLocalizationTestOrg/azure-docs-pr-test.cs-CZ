---
title: aaaOverview Azure Active Directory Domain Services | Microsoft Docs
description: "Přehled služby Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 0d47178f-773e-45f9-9ff4-9e8cffa4ffa2
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 2b4884b3b8b639fcca438ddbab0bd3361aac53c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="overview"></a>Přehled
Služby infrastruktury Azure umožňují toodeploy širokou škálu řešení výpočetní agilní způsobem. Pomocí Azure Virtual Machines, můžete nasadit téměř okamžitě a platíte jenom o hello minutu. Využitím podpory pro Windows, Linux, SQL Server, Oracle, SAP, IBM a BizTalk, můžete nasadit jakoukoli úlohu, žádný jazyk na téměř jakémkoliv operačním systému. Tyto výhody povolit toomigrate starší verze aplikace nasazené na místě tooAzure, toosave na provozní náklady.

Klíčovým prvkem migraci místního tooAzure aplikace zpracovává hello identity musí z těchto aplikací. Aplikace využívající technologii adresáře může spoléhají na LDAP pro čtení nebo zápis podnikové adresář toohello nebo spoléhají na integrované ověřování systému Windows (ověřování protokolu Kerberos nebo NTLM) tooauthenticate koncovým uživatelům. Obchodní (LOB) aplikace spuštěné na Windows serveru se obvykle nasazují na počítačích připojených k doméně, aby se dala spravovat, bezpečně pomocí zásad skupiny. too'lift a shift' místní aplikace toohello cloudu, třeba tyto závislosti na infrastruktuře podnikové identitě hello toobe přeložit.

Správci často zapnout tooone hello následující řešení toosatisfy hello identity potřebám své aplikace nasazené v Azure:

* Nasazení připojení site-to-site VPN mezi úlohami spuštěnými v služby infrastruktury Azure a hello firemní adresář místní.
* Hello AD podnikové doméně nebo doménové struktuře infrastrukturu rozšiřte nastavení řadičů domény repliky pomocí virtuální počítače Azure.
* Nasazení samostatné doméně v Azure pomocí řadiče domény nasazené jako virtuální počítače Azure.

Všechny tyto přístupy trpí vysoké náklady a režijní náklady na správu. Správci jsou požadované toodeploy řadičů domény pomocí virtuálních počítačů v Azure. Kromě toho potřebují toomanage, zabezpečení, opravy, monitorování, zálohování a řešení těchto virtuálních počítačů. Hello spoléhat na toohello připojení VPN místního adresáře způsobí, že úlohy nasazené v Azure toobe citlivé tootransient síťových chyb a méně výpadků. Tyto výpadky sítě zase mít za následek nižší provozu a sníženou spolehlivost pro tyto aplikace.

Jsme chtěli Azure AD Domain Services tooprovide alternativu jednodušší.

## <a name="introducing-azure-ad-domain-services"></a>Představení služby Azure AD Domain Services
Služba Azure AD Domain Services poskytuje spravované doméně služby, jako je například připojení k doméně, ověřování protokolu Kerberos nebo NTLM zásad LDAP, skupiny, které jsou plně kompatibilní s Windows Server Active Directory. Můžete využívat tyto služby domény bez hello potřebu jste toodeploy, spravovat a oprava řadiče domény v cloudu hello. Služba Azure AD Domain Services se integruje s existující klientovi Azure AD, a tím umožnit pro uživatele toolog pomocí své podnikové přihlašovací údaje. Kromě toho můžete stávající skupiny a uživatelské účty toosecure přístup tooresources, čímž zajišťuje hladší 'navýšení a změnu' místní prostředky tooAzure služby infrastruktury.

Azure AD Domain Services funkce funguje bezproblémově bez ohledu na to, jestli klientovi Azure AD je jenom pro cloud nebo synchronizované s vaší místní Active Directory.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure AD Domain Services pro organizace, jenom pro cloud
Jenom pro cloud Azure AD klienta (často označují tooas "spravované klienty") nemá žádné místní identity nároky. Jinými slovy uživatelské účty, jejich hesla a členství ve skupinách jsou všechny nativní toohello cloudu – to znamená, vytvořit a spravovat ve službě Azure AD. Vezměte v úvahu na chvíli že Contoso je čistě cloudové klienta Azure AD. Jak ukazuje následující obrázek hello, správce společnosti Contoso nakonfiguroval virtuální sítě ve službách infrastruktury Azure. V této virtuální sítě ve virtuálních počítačích Azure nasazených aplikací a zatížení serveru. Vzhledem k tomu, že Contoso je jenom pro cloud klienta, všechny identity uživatelů, jejich přihlašovacích údajů a členství ve skupinách jsou vytvořit a spravovat ve službě Azure AD.

![Přehled služby Azure AD Domain Services](./media/active-directory-domain-services-overview/aadds-overview.png)

Společnosti Contoso správce IT může povolení služby Azure AD Domain Services pro svého tenanta Azure AD a vyberte toomake domény služby k dispozici v této virtuální síti. Po tomto datu Azure AD Domain Services zřizuje spravované domény a zpřístupní ve virtuální síti hello. Všechny uživatelské účty, členství ve skupinách a k dispozici v klientovi Azure AD společnosti Contoso přihlašovací údaje uživatele jsou taky k dispozici v této doméně nově vytvořený. Tato funkce umožňuje uživatelům v organizaci toosign hello v toohello doméně pomocí podnikových přihlašovacích – například při připojení počítače vzdáleně připojené k toodomain přes vzdálenou plochu. Správci můžou zřizovat tooresources přístup v doméně hello pomocí existující členství ve skupinách. Aplikace nasazené ve virtuálních počítačích na hello virtuální sítě můžete použít funkce, například připojení k doméně, LDAP pro čtení, LDAP bind, ověřování NTLM a Kerberos a zásad skupiny.

Několik nejdůležitějšími aspekty tohoto hello spravované domény, který je zajištěný nástrojem Azure AD Domain Services jsou následující:

* Společnosti Contoso správce IT nemusí toomanage, oprava nebo monitorování tuto doménu nebo všechny řadiče domény pro toto spravované domény.
* Se nereplikují toomanage AD nutné pro tuto doménu. Uživatelské účty, členství ve skupinách a přihlašovacích údajů z klienta Azure AD společnosti Contoso jsou automaticky dostupné v rámci této spravované domény.
* Vzhledem k tomu, že je hello domény spravované službou Azure AD Domain Services, Contoso na správce IT nemá oprávnění správce domény nebo správce podnikové sítě v této doméně.

### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure AD Domain Services pro hybridní organizace
Organizace s hybridní infrastrukturu IT využívat kombinaci prostředků cloudu a místních prostředků. Tyto organizace synchronizovat informace o identitě z klienta jejich místní tootheir adresář Azure AD. Hybridní organizace vypadat toomigrate další své místní aplikace toohello cloudu především starší verze podporující adresáře aplikace, může mít užitečné toothem Azure AD Domain Services.

Litware Corporation nasadil [Azure AD Connect](../active-directory/active-directory-aadconnect.md), informace o identitě toosynchronize ze své místní adresář tootheir Azure AD klienta. informace o identitě Hello, který je synchronizován se obsahuje uživatelské účty, jejich hodnoty hash přihlašovacích údajů pro ověřování (heslo sync) a členství ve skupinách.

> [!NOTE]
> **Synchronizace hesla je povinné pro hybridní organizace toouse Azure AD Domain Services**. Tento požadavek je vzhledem k tomu, že jsou vyžadována pověření uživatelů v hello spravované domény poskytované Azure AD Domain Services, tooauthenticate tyto uživatele pomocí metody ověřování protokolem NTLM nebo Kerberos.
>
>

![Služba Azure AD Domain Services pro Litware Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

Hello předchozí obrázek ukazuje, jak organizace s hybridní infrastrukturu IT, jako je například Litware Corporation, můžete použít Azure AD Domain Services. Aplikace a úlohy serveru, které vyžadují služby domény litware's se nasadí ve virtuální síti ve službách infrastruktury Azure. Litware's správce IT může povolení služby Azure AD Domain Services pro svého tenanta Azure AD a zvolte toomake spravované doméně, která je k dispozici v této virtuální síti. Protože Litware organizaci s hybridní infrastrukturu IT, uživatelské účty, skupiny a přihlašovací údaje jsou klienta Azure AD synchronizované tootheir ze svého místního adresáře. Tato funkce umožňuje uživatelům toosign v toohello doméně pomocí podnikových přihlašovacích – například při vzdáleném připojení toomachines připojené k doméně toohello přes vzdálenou plochu. Správci můžou zřizovat tooresources přístup v doméně hello pomocí existující členství ve skupinách. Aplikace nasazené ve virtuálních počítačích na hello virtuální sítě můžete použít funkce, například připojení k doméně, LDAP pro čtení, LDAP bind, ověřování NTLM a Kerberos a zásad skupiny.

Několik nejdůležitějšími aspekty tohoto hello spravované domény, který je zajištěný nástrojem Azure AD Domain Services jsou následující:

* spravované domény Hello je samostatné domény. Není rozšíření Litware's místní domény.
* Litware's správce IT nepotřebuje toomanage, patch nebo monitorování řadičů domény pro toto spravované domény.
* Neexistuje žádné nutné toomanage AD replikace toothis domény. Uživatelské účty, členství ve skupinách a přihlašovacích údajů z místního adresáře Litware's jsou synchronizované tooAzure AD přes Azure AD Connect. Tyto uživatelské účty, členství ve skupinách a přihlašovacích údajů jsou automaticky dostupné v rámci hello spravované domény.
* Vzhledem k tomu, že je hello domény spravované službou Azure AD Domain Services, Litware na správce IT nemá oprávnění správce domény nebo správce podnikové sítě v této doméně.

## <a name="benefits"></a>Výhody
S Azure AD Domain Services můžete využívat hello následující výhody:

* **Jednoduché** – můžete vyhovět potřebám identity hello virtuální počítače nasazené služby infrastruktury tooAzure s jenom několik jednoduchých kliknutí. Není nutné toodeploy a správě infrastruktury identity v Azure nebo instalační program připojení back tooyour místní infrastruktury identit.
* **Integrované** – Azure AD Domain Services se úzce integruje se klientovi služby Azure AD. Teď můžete použít Azure AD jako adresář služby integrované cloudové enterprise, který je určen toohello potřebám moderních aplikací a tradiční aplikace využívající technologii adresáře.
* **Kompatibilní** – Azure AD Domain Services je založený na úrovni infrastruktury hello Principy enterprise systému Windows Server Active Directory. Aplikace můžete tedy spoléhat na vyšší úroveň kompatibility s funkcemi, Windows Server Active Directory. Ne všechny funkce, které jsou k dispozici v systému Windows Server AD jsou aktuálně dostupné v Azure AD Domain Services. K dispozici funkce jsou však kompatibilní s hello odpovídající systému Windows Server AD funkce, které byste tedy spoléhat na ve vaší místní infrastruktuře. LDAP Hello možnosti připojení pomocí protokolu Kerberos, NTLM, zásady skupiny a domény tvoří vyspělá nabídky, které byly testovány a vylepšení v různých verzích systému Windows Server.
* **Nákladově efektivní** – službou Azure AD Domain Services, se můžete vyhnout hello infrastruktury a správu zatížení, který je přidružen Správa identity infrastruktury toosupport tradiční adresář aplikace využívající technologii. Můžete přesunout tyto aplikace tooAzure služby infrastruktury a využívat větší úsporu na provozní náklady.
