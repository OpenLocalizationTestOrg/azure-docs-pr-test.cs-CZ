---
title: "aaaProtect osobních dat s ovládacími prvky Azure přístupu a identit | Microsoft Docs"
description: "Pomocí Azure přístupu a identit a ovládací prvky toohelp ochrana vašich osobních údajů"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 3132c2af25f86662668e5b555eab1d81de7f2e6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-and-multi-factor-authentication-protect-personal-data-with-identity-and-access-controls"></a>Azure Active Directory a služby Multi-Factor Authentication: ochrana osobních dat s ovládacími prvky identit a přístupu

Tento článek obsahuje informace a postupy tooprotect osobní data pomocí Azure Active Directory a službou Multi-Factor authentication zabezpečení funkcích a službách, můžete použít.

## <a name="scenario"></a>Scénář

Velké výletních společnosti, centrálou ve Spojených státech amerických hello je rozšířit jeho itineráře toooffer operace v hello Středozemního, Jaderského a baltský moři, jakož i hello Britské ostrovy. toosupport těchto úsilí získala menší výletních Víceřádkový na základě v Itálii Německo, Dánsko a hello Spojené království 

Hello společnost používá Microsoft Azure toostore firemní data v cloudu hello. To zahrnuje osobní identifikovatelné údaje, například jména, adresy, telefonních čísel a informace o kreditní kartě z jeho základní globální zákazníka. Ve všech umístěních zahrnuje také tradiční informace lidských zdrojů, jako jsou adresy, telefonních čísel, daň identifikačními čísly a lékařské informace o zaměstnance společnosti. Hello výletních řádku také udržuje velké databáze potřebu a věrnost program členů, která zahrnuje osobní údaje tootrack vztahů se zákazníky aktuální a starší.

Zaměstnanci společnosti přístupu hello síti ze vzdálených pobočkách a agenty cesta hello společnosti nachází kolem hello, world mít přístup k prostředkům společnosti toosome.

## <a name="problem-statement"></a>Popis problému

Hello společnosti musí ochrany osobních údajů hello zaměstnanců a zákazníků osobních údajů z útočníci, kteří přístup toogain identity toouse ohrožení zabezpečení. Také musí zajistit tento přístup toopersonal, které se data podle oprávněných uživatelů je omezena pouze na ty, kteří ho potřebují toodo svou práci.

## <a name="company-goal"></a>Cílem společnosti

Cílem společnosti Hello je, že tooensure, který přístup k datům toopersonal přísnou kontrolu. Je nezbytné, aby identit uživatelů s přístup k datům toopersonal chráněn silné ověřování. Zásady [nejnižší oprávnění] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) musí být vynucená tak, aby oprávněných uživatelů pouze hello úroveň přístupu, které potřebují a žádné další.

## <a name="solutions"></a>Řešení

Microsoft Azure poskytuje nástroje pro správu identit a přístupu toohelp společností určovat, kdo má přístup tooresources, které obsahují osobní údaje.

### <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) spravuje identit a řídí přístup tooAzure stejně jako další místní a dalších prostředků cloudu, datům a aplikacím. [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) pomáhá Azure správci toominimize hello počet lidí, kteří mají přístup toocertain informace, jako je osobní data. Umožňuje jim toodiscover, omezte a monitorujte privilegované identity a jejich přístup tooresources a tooassign dočasné, JIT (JIT) právy tooeligible uživatelů. Také poskytuje přehled o uživatelů, kteří mají oprávnění správce AAD.

Hello činnosti spojené s použitím AAD PIM, patří:

- Povolení Privileged Identity Management pro svůj adresář

- Použití Privileged Identity Management správce řídicího panelu toosee důležité informace na první pohled

- Správa identit hello privilegovaný (administrators) přidáním nebo odebráním rolí tooeach trvalé nebo oprávněné správce

- Konfigurace nastavení aktivace role hello

- Aktivace role

- Kontrola role aktivity

#### <a name="how-do-i-enable-aad-pim"></a>Povolení AAD PIM

toostart pomocí PIM pro váš adresář hello následující:

1. Přihlaste se toohello portálu Azure jako globální správce adresáře.

2. Pokud má vaše organizace více než jeden adresář, vyberte své uživatelské jméno v hello pravém horním rohu hello portálu Azure. Vyberte adresář hello, kde budete používat Azure AD Privileged Identity Management.

3. Vyberte **další služby** a používat hello **filtru** toosearch textové pole pro Azure AD Privileged Identity Management.

4. Zkontrolujte **Pin toodashboard** a pak klikněte na **vytvořit**. Otevře se Hello aplikace Privileged Identity Management.

Jakmile Azure AD Privileged Identity Management nastavená, zobrazí se při každém otevření aplikace hello hello navigačním okně.

![](media/protect-personal-data-identity-access-controls/azure-pim.png)

Další informace a pokyny, Začínáme se službou AAD PIM najdete v tématu [spustit pomocí Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)

### <a name="azure-role-based-access-control"></a>Řízení přístupu Azure na základě rolí

[Řízení přístupu Azure na základě rolí](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) pomáhá Azure správcům spravovat přístup k prostředkům tooAzure povolením hello udělení přístupu na základě role přiřazené hello uživatele. Můžete povinnosti v rámci týmu oddělit a udělit pouze hello množství toousers přístup, skupiny nebo aplikace, které potřebují tooperform svou práci.

Toousers pomocí hello portálu Azure, nástroje příkazového řádku Azure nebo rozhraní API pro správu Azure můžete udělit přístup na základě rolí.

Další informace o základní informace o Azure RBAC najdete v tématu [Začínáme s řízením přístupu na základě rolí v hello portálu Azure.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)

#### <a name="how-do-i-manage-azure-rbac-with-powershell"></a>Jak lze spravovat Azure RBAC pomocí prostředí PowerShell?

Můžete použít toomanage rutiny prostředí PowerShell Azure RBAC, včetně hello následující úlohy správy:

- Seznam rolí

- Zjistit, kdo má přístup

- Udělení přístupu

- Odebrání přístupu

- Vytvořit vlastní roli

- Získat akce pro poskytovatele prostředků

- Upravit vlastní roli

- Odstranit vlastní roli

- Vlastní role seznamu

Pokyny, jak toomanage Azure RBAC v prostředí PowerShell najdete v části [přístupu na základě Role spravovat pomocí prostředí Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).

### <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) je řešení dvoustupňové ověření, které pomáhá chránit přístup toodata a aplikace, při splnění požadavků uživatelů pro jednoduchý proces přihlášení. Zajišťuje silné ověřování prostřednictvím celé řady metod ověřování, včetně ověřování pomocí telefonních hovorů, textových zpráv nebo mobilních aplikací.

toodeploy MFA v cloudu Azure hello, budete potřebovat toofirst ji povolit a potom zapnout dvoustupňové ověřování pro uživatele.

#### <a name="how-do-i-enable-azure-toouse-mfa"></a>Povolení Azure toouse MFA?

Pokud mají vaši uživatelé licencí, které zahrnují Azure Multi-Factor Authentication, není nic, je nutné, aby toodo tooturn na Azure MFA. Pokud ne, je nutné toocreate poskytovatele vícefaktorového ověřování ve vašem adresáři. toodo, postupujte takto:

1. Vyberte **služby Active Directory** v hello portál Azure classic (přihlášeni jako správce).

2. Vyberte **poskytovatelé služby Multi-Factor Authentication.**

3. Vyberte **nový,** a potom v části **aplikační služby,** vyberte **zprostředkovatel vícefaktorového ověřování.**

4. Vyberte **rychle vytvořit.**

5. Vyplňte pole s názvem hello a vyberte modelu využití (za ověření nebo za povoleného uživatele).

6. Určíte adresář, ke které hello je přiřazeno zprostředkovatele vícefaktorového ověřování.

7. Klikněte na tlačítko hello **vytvořit** tlačítko.

![](media/protect-personal-data-identity-access-controls/quick-create.png)

Další pokyny toomanage vaše zprostředkovatel vícefaktorového ověřování najdete v části [Začínáme s poskytovatele Azure Multi-Factor Auth.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)

#### <a name="how-do-i-turn-on-two-step-verification-for-users"></a>Jak lze zapnout dvoustupňové ověřování pro uživatele?

Můžete vynutit dvoustupňové ověřování pro všechny přihlášení nebo podmíněného přístupu zásady toorequire dvoustupňové ověření můžete vytvořit pouze v případě, že určité podmínky.

Povolení Azure MFA změnou stavů uživatele je hello tradiční přístup pro vyžádání dvoustupňové ověřování. Budou mít všichni uživatelé hello, které povolíte hello stejné požadavek tooperform dvoustupňové ověřování při každém přihlášení. Povolení uživatele potlačí všechny zásady podmíněného přístupu, které může mít vliv na tohoto uživatele.

Povolení Azure MFA se zásadami podmíněného přístupu je flexibilnější přístup pro vyžádání dvoustupňové ověřování. Můžete vytvořit zásady podmíněného přístupu, které se vztahují toogroups, jakož i jednotlivé uživatele. S vysokým rizikem skupiny je možné přidělit další omezení než nízkým rizikem skupiny nebo dvoustupňové ověřování můžete vyžaduje se jenom pro vysoce rizikové cloudové aplikace a pro ty, které jsou nízkým rizikem přeskočeno. Podmíněný přístup je však placené funkce služby Azure Active Directory.

tooenable MFA změnou stavu uživatele, hello následující:

1. Přihlaste se toohello portálu Azure jako správce.
2. Přejděte příliš**Azure Active Directory \> uživatelů a skupin \> všichni uživatelé**.
3. Vyberte **služby Multi-Factor Authentication**.
4. Najít hello uživatele, že chcete tooenable pro Azure MFA. Může být nutné toochange hello zobrazení v horní části hello.
5. Zkontrolujte název hello pole Další toohello uživatele.
6. Na pravé straně v části Rychlé kroky hello, zvolte **povolit**.

   ![](media/protect-personal-data-identity-access-controls/quick-create.png)

7. Potvrďte výběr v hello místní okno, které se otevře.  Uživatelé, pro které bylo povoleno vícefaktorové ověřování vyzve tooregister hello při příštím přihlášení.

tooenable Azure MFA se zásadami podmíněného přístupu, hello následující:

1. Přihlaste se toohello portálu Azure jako správce.

2. Přejděte příliš**Azure Active Directory \> podmíněného přístupu**.

3. Vyberte **nové zásady**.

4. V části **přiřazení**, vyberte **uživatelů a skupin**. Použití hello **zahrnout** a **vyloučit** karty toospecify, kteří uživatelé a skupiny bude spravovat zásady hello.

5. V části **přiřazení**, vyberte **cloudových aplikací.** Zvolte příliš**zahrnují všechny cloudové aplikace**.
6.  V části **přístup k ovládacím prvkům**, vyberte **Grant**. Zvolte **vyžadovat vícefaktorové ověřování**.
7.  Zapnout **povolit zásady** příliš**na** a pak vyberte **Uložit**.

Informace o způsobu vytvoření tooset nastavení Azure MFA tooconfigure si upozornění na podvod, jednorázové přihlášení, použít vlastní hlasové zprávy, konfigurovat ukládání do mezipaměti, zadejte důvěryhodné IP adresy, vytvoření hesel aplikací, povolte zapamatování vícefaktorového ověřování pro zařízení, které důvěřují uživatele, vyberte metody ověřování, najdete v části [konfigurace nastavení Azure Multi-Factor Authentication.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)

## <a name="next-steps"></a>Další kroky

- [Zabezpečení privilegovaného přístupu ve službě Azure AD](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)

- [Nejčastější dotazy ohledně služby Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-faq)

- [Na základě rolí řešení potíží s řízení přístupu](https://docs.microsoft.com/azure/active-directory/role-based-access-control-troubleshooting)

- [Ochrany identit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)
