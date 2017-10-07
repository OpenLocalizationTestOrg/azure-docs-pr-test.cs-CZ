---
title: aaaBranding pokyny pro aplikace | Microsoft Docs
description: "O komplexní Průvodce orientované toodeveloper prostředky pro Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 72f4e464-1352-4a49-a18f-c37f58e7d5c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: skwan
ms.custom: aaddev
ms.openlocfilehash: e43f884c736a0dcb2e6e51293962ef1e2636ad70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="branding-guidelines-for-applications"></a>Branding pokyny pro aplikace
Toto téma popisuje hello branding pokyny, který byste měli použít při vývoji aplikací s Azure Active Directory (Azure AD). Tyto pokyny vám pomůže nasměrovat vašim zákazníkům, když chtějí toouse svého pracovního nebo školního účtu, spravované ve službě Azure AD nebo svého osobního účtu pro aplikaci tooyour registrace a přihlášení.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Osobní účty oproti pracovní nebo školní účty od Microsoftu
Microsoft spravuje dva typy uživatelských účtů:

* **Osobní účty** (dříve označované jako účet Windows Live ID). Tyto účty představují hello vztah mezi *jednotlivých* uživatelů společnosti Microsoft a jsou používají tooaccess uživatelských zařízení a služby společnosti Microsoft. Tyto účty jsou určené pro osobní použití.
* **Pracovní nebo školní účty.** Tyto účty se spravují společností Microsoft jménem organizace, které používají Azure Active Directory. Tyto účty jsou použité toosign v tooOffice 365 a jiné obchodní služby společnosti Microsoft.

Microsoft pracovní nebo školní účty jsou obvykle přiřadila jejich organizace (společnosti, školy, státní úřad) uživatelé tooend (zaměstnanci, studenty, federálních zaměstnanců). Tyto účty jsou že buď standardní přímo v cloudu hello v platformy hello Azure AD nebo tooAzure AD synchronizované z místní adresář, jako je například Windows Server Active Directory. Společnost Microsoft se hello *správce* hello pracovní nebo školní účty, ale hello účty jsou ve vlastnictví a řídí hello organizace.

## <a name="referring-tooazure-ad-accounts-in-your-application"></a>Odkazující tooAzure AD účty v aplikaci
Microsoft nezveřejňuje koncoví uživatelé toohello Azure nebo názvy brand hello služby Active Directory a ani jeden z nich by vám měl.

* Jakmile jsou uživatelé přihlášení, byste měli použít název a logo co nejvíce hello organizace. Toto je lepší, než pomocí obecné podmínky, jako je "organizaci".
* Když uživatelé nejsou přihlášení, se seznamte s účty tootheir jako "pracovní nebo školní účty" a použijte hello Microsoft logo tooconvey tyto účty jsou spravuje Microsoft. Nepoužívejte podmínky jako "enterprise účet", "obchodní účet" nebo "podnikový účet", které uživatele mást.

## <a name="user-account-pictogram"></a>Piktogramu účtu uživatele
V dřívějších verzích těchto pokynů doporučujeme pomocí piktogram "blue badge". Na základě názorů uživatelů a vývojáře, teď doporučujeme použít hello hello Microsoft logo místo. To vám pomůže pochopit, že se můžete znovu použít hello účet, který bude používat s Office 365 nebo jiné toosign Microsoft business services v aplikaci tooyour uživatele.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Registrací a přihlášení pomocí služby Azure AD
Aplikace může představovat samostatné cesty pro registrace a přihlášení a hello následující části obsahují visual pokyny pro oba scénáře.

**Pokud vaše aplikace podporuje registrace koncového uživatele (například volné tootrial nebo freemium modelu)**: můžete zobrazit **přihlášení** tlačítko, které umožňuje uživatelům tooaccess vaší aplikace pomocí svého pracovního účtu nebo svého osobního účtu. Azure AD se zobrazí výzva hello souhlasu prvním přístupu k aplikaci.

**Pokud vaše aplikace vyžaduje oprávnění, která mohou pouze správci souhlas, nebo pokud vaše aplikace vyžaduje organizační licencování**: oddělíte pořízení správce z přihlášení uživatele. Hello **"get tuto aplikaci" tlačítko** bude přesměrování toosign admins v pak požádejte toogrant souhlas jménem uživatelů ve své organizaci. To má hello dodatečná výhoda potlačení koncoví uživatelé souhlasu výzvy tooyour aplikace.

## <a name="visual-guidance-for-app-acquisition"></a>Visual pokyny pro získání aplikace
Odkaz na vaši "get aplikace hello" musí přesměrovat hello uživatele toohello Azure AD udělení přístupu (autorizovat) stránky, tooallow tooauthorize správce organizace toohave vaší aplikace přístup k organizaci tootheir data, která je hostovaná společností Microsoft. Podrobnosti o tom, jak toorequest přístup, jsou popsané v hello [integrace aplikací s Azure Active Directory](active-directory-integrating-applications.md) článku.

Po správci souhlas tooyour aplikace, můžete tooadd ho prostředí Spouštěč aplikace Office 365 tootheir uživatelů (přístupný z hello waffle a [https://portal.office.com/myapps](https://portal.office.com/myapps)). Pokud chcete tooadvertise tato možnost, můžete použít podmínky jako "Přidat této organizace tooyour aplikace" a zobrazit tlačítko takto:

![Typy aplikací a scénáře](./media/active-directory-branding-guidelines/add-to-my-org.png)

Doporučujeme však, že napíšete vysvětlující text, aniž byste museli spoléhat na tlačítka. Například:

> *Pokud už používáte Office 365 nebo jiné obchodní služby společnosti Microsoft, můžete jednoduše udělit < your_app_name > přístup tooyour dat organizace. To vám umožní uživatelům tooaccess < your_app_name > pomocí stávajícího pracovního účtu.*
> 
> 

## <a name="visual-guidance-for-sign-in"></a>Visual pokyny pro přihlášení
Aplikace by měl zobrazit znaménkem v tlačítka, který přesměruje uživatele toohello přihlášení koncový bod, který odpovídá toohello protokol, který používají toointegrate s Azure AD. Hello následující část obsahuje podrobné informace na co by měl vypadat toto tlačítko.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Piktogramu a "Přihlásit se pomocí Microsoft"
To je přidružení hello hello Microsoft logo a podmínky "Přihlášení v with Microsoft" hello, který jedinečně reprezentuje mezi jiných poskytovatelů identit, které vaše aplikace může podporovat Azure AD. Pokud nemáte k dispozici dostatek místa pro "Přihlášení v with Microsoft", je ok tooshorten je příliš "přihlásit".

![Typy aplikací a scénáře](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Typy aplikací a scénáře](./media/active-directory-branding-guidelines/sign-in-light.png)

Můžete taky tmavý barevné schéma pro tlačítka hello.

![Typy aplikací a scénáře](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Typy aplikací a scénáře](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Doporučené značky a nedoporučené postupy
**PROVEĎTE** použití "pracovní nebo školní účet" v kombinaci s hello "Přihlášení v with Microsoft" tlačítko tooprovide další vysvětlení koncoví uživatelé toohelp rozpoznat, jestli se může použít. **Nemáte** použít jiné podmínky, jako je například "enterprise účet", "obchodní účet" nebo "podnikový účet."

**Nemáte** použijte "Office 365 ID" nebo "Azure ID". Office 365 je také název hello příjemce nabídky od Microsoftu, který nepoužívá Azure AD pro ověřování.

**Nemáte** změnit logo společnosti Microsoft hello.

**Nemáte** vystavit toohello koncoví uživatelé služby Active Directory nebo Azure značky. Je to v pořádku ale toouse tyto podmínky s vývojáři, IT specialisté a správci.

## <a name="navigation-dos-and-donts"></a>Navigace doporučené a nedoporučené postupy
**PROVEĎTE** poskytují způsob pro uživatele toosign out a přepínače tooanother uživatelský účet. I když většina lidí má jeden osobní účet od společnosti Microsoft nebo sítě Facebook/Google nebo Twitter, kteří jsou často přidružen více než jedné organizace. Podpora pro více přihlášených uživatelů bude brzo.

