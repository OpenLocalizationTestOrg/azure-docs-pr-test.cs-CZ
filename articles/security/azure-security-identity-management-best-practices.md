---
title: "aaaAzure identit a přístupu osvědčené postupy zabezpečení | Microsoft Docs"
description: "Tento článek obsahuje sadu osvědčené postupy pro správu identit a přístupu k řízení pomocí součástí možnosti Azure."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2017
ms.author: yurid
ms.openlocfilehash: af07dfda84758b9124641078ac8f696f725f2bf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Osvědčené postupy zabezpečení řízení Azure správu identit a přístupu
Mnoho zvažte toobe hello nové hranice vrstvu identit pro zabezpečení, převzetí této role z hello tradiční zaměřené na síti perspektivy. Tento vývoj hello primární pivot pro zabezpečení pozornost a investice do pocházet z hello skutečnost, že se staly stále porézní hranice sítě a že hraniční obrana nemůže být efektivní, jako předchozí toohello rozbalení bylyjednou[BYOD](http://aka.ms/byodcg) zařízení a cloudových aplikací.

V tomto článku se budeme zabývat kolekce správy identit Azure a osvědčené postupy pro zabezpečení řízení přístupu. Tyto doporučené postupy jsou odvozeny od našich zkušeností s [Azure AD](../active-directory/active-directory-whatis.md) a hello prostředí zákazníků, jako sami.

Pro každý osvědčený postup vysvětlíme:

* Jaké hello osvědčeným postupem je
* Proč chcete tooenable, že osvědčeným postupem
* Pokud selže tooenable hello osvědčený postup, co mohou být výsledek hello
* Osvědčeným postupem toohello možné alternativy
* Jak můžete získat informace tooenable hello osvědčený postup

Tato správy identit Azure a řízení přístupu zabezpečení článek o osvědčených postupech je založen na konsenzu stanovisko a možnostech platformy Azure a sady funkcí, které jsou ve hello dobu, kdy byla zapsána v tomto článku. Názory a technologie mění v čase a budou v tomto článku v pravidelných intervalech tooreflect aktualizovat tyto změny.

Identit Azure správy a přístupu k řízení osvědčené postupy zabezpečení popsané v tomto článku patří:

* Centralizovat správu vaší identity
* Povolit jednotné přihlašování (SSO)
* Nasazení správy hesel
* Vynutit ověřování Multi-Factor authentication (MFA) pro uživatele
* Řízení přístupu (RBAC) na základě role pomocí
* Řízení umístění, kde jsou prostředky vytvořené pomocí resource manager
* Příručka vývojáře tooleverage identity možnosti pro aplikace SaaS
* Aktivně sledovat pro podezřelé aktivity

## <a name="centralize-your-identity-management"></a>Centralizovat správu vaší identity
Jeden důležitý krok k zabezpečení vaší identity je tooensure to můžete spravovat účty z jednoho umístění jednoho týkající se kde tento účet vytvořil. Při hello většina podniků hello IT organizace bude mít primární účet adresář místní, hybridního cloudu nebo nasazení jsou na zvýšení hello a je důležité pochopit, jak toointegrate místní a cloudové adresáře a poskytují integrované prostředí toohello koncového uživatele.

tooaccomplish to [hybridní identita](../active-directory/active-directory-hybrid-identity-design-considerations-overview.md) scénáři doporučujeme dvě možnosti:

* Synchronizace místního adresáře s adresářem cloudu pomocí služby Azure AD Connect
* Vytvořit federaci vaší identity na místě s directory cloudu s použitím [Active Directory Federation Services](https://msdn.microsoft.com/library/bb897402.aspx) (AD FS)

Organizace, které nesplní toointegrate, který bude mít svou identitu místní s jejich Cloudová identita vyšší administrativní režii při správě účtů, který zvýší pravděpodobnost hello chyb a porušení zabezpečení.

Další informace o synchronizace Azure AD, přečtěte si článek hello [integrace místních identit s Azure Active Directory](../active-directory/active-directory-aadconnect.md).

## <a name="enable-single-sign-on-sso"></a>Povolit jednotné přihlašování (SSO)
Pokud máte několik adresářů toomanage, to všechno bude problémem správy nejen pro oddělení IT, ale i pro koncové uživatele, které budou mít tooremember více hesel. Pomocí [jednotného přihlašování k](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) bude poskytovat uživatelům možnost hello hello používá stejné nastavení přihlašovacích údajů toosign-in a přístup k prostředkům hello, které potřebují, bez ohledu na to tam, kde tento prostředek je nacházejí na místních nebo v cloudu hello.

Pomocí jednotného přihlašování k tooenable uživatelé tooaccess jejich [aplikace SaaS](../active-directory/active-directory-appssoaccess-whatis.md) podle jejich organizace účtu ve službě Azure AD. Tento krok platí nejen pro aplikace Microsoft SaaS, ale také další aplikace, jako například [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) a [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Vaše aplikace může být nakonfigurované toouse Azure AD jako [identity na základě SAML](../active-directory/fundamentals-identity.md) zprostředkovatele. Jako ovládací prvek zabezpečení Azure AD nevydá token, což jim toosign do aplikace hello Pokud kterým byl udělen přístup pomocí služby Azure AD. Můžete udělit přístup přímo nebo prostřednictvím skupiny, že jsou členem.

> [!NOTE]
> Hello rozhodnutí toouse jednotné přihlašování, bude mít vliv jak integraci místního adresáře s adresářem v cloudu. Pokud chcete jednotné přihlašování, budete potřebovat toouse federace, protože synchronizace adresářů se poskytují jenom [stejné prostředí pro přihlašování](../active-directory/active-directory-aadconnect.md).
>
>

Organizace, které nebudou vynucovat jednotného přihlašování pro svoje uživatele a aplikace jsou více zveřejněné tooscenarios, kde uživatelé budou mít více hesel, které přímo zvýší pravděpodobnost hello uživatelů opakovaného použití hesla nebo pomocí vytváření silných hesel.

Další informace o Azure AD SSO přečíst článek hello [správu služby AD FS a vlastního nastavení službou Azure AD Connect](../active-directory/active-directory-aadconnect-federation-management.md).

## <a name="deploy-password-management"></a>Nasazení správy hesel
Ve scénářích, kde máte několik klientů, nebo chcete tooenable uživatelé příliš[resetovat vlastní heslo](../active-directory/active-directory-passwords-update-your-own-password.md), je důležité, abyste používali příslušná bezpečnostní zásady tooprevent zneužití. V Azure můžete využívat funkce resetování hesla pomocí samoobslužné služby hello a přizpůsobit toomeet možnosti zabezpečení hello vaše podnikové požadavky.

To je zvlášť důležité tooobtain zpětnou vazbu od těchto uživatelů a dozvědět se od jejich prostředí jako snaží tooperform tyto kroky. Podle toho, tyto činnosti, propracovanější plán toomitigate potenciální problémy, které mohou nastat během nasazení hello větší skupině. Dále je doporučeno používat hello [sestava aktivit registrace resetování hesla](../active-directory/active-directory-passwords-get-insights.md) toomonitor hello uživatele, kteří jsou registrace.

Organizace, které mají tooavoid heslo změnit volání podpory ale povolit uživatelům tooreset vlastní hesla jsou náchylná tooa vyšší volání svazku toohello technickou podporu kvůli toopassword problémy. V organizacích, které mají víc klientů je nutné implementovat tento typ funkce a povolit resetování v rámci hranice zabezpečení, které byly vytvořeny v zásadách zabezpečení hello uživatelé tooperform hesla.

Další informace o resetování přečíst článek hello hesla [nasazení správu hesel a školení uživatelů toouse ho](../active-directory/active-directory-passwords-best-practices.md).

## <a name="enforce-multi-factor-authentication-mfa-for-users"></a>Vynutit ověřování Multi-Factor authentication (MFA) pro uživatele
Pro organizace, které potřebují toobe kompatibilní s oborové standardy, jako například [PCI DSS verze 3.2](http://blog.pcisecuritystandards.org/preparing-for-pci-dss-32), službu Multi-Factor authentication je musí mít možnost pro ověřování uživatelů. Nad rámec dodržení předpisů oborovým standardům, vynucování vícefaktorového ověřování tooauthenticate uživatelů může také pomoct typ krádeže přihlašovacích údajů organizace toomitigate útoku, například [Pass-the-Hash (PtH)](http://aka.ms/PtHPaper).

Povolením Azure MFA pro uživatele přidáte druhou vrstvu zabezpečení toouser přihlášení a transakce. V takovém případě transakci může získávat přístup k dokumentu umístěná na souborovém serveru nebo ve vaší službě SharePoint Online. Azure MFA také pomáhá IT tooreduce hello pravděpodobnost, že ohrožené pověření budou mít přístup tooorganization data.

Příklad: Vynutit Azure MFA pro uživatele a ji nakonfigurovat jako ověření toouse telefonního hovoru nebo textové zprávy. Pokud dojde k ohrožení hello uživatelských přihlašovacích údajů, nebude útočník hello být schopný tooaccess jakémukoli prostředku, protože mu nebude mít přístup toouser phone. Organizace, které Nepřidávejte další vrstvy ochrany identit budou náchylnější pro útoku krádeží přihlašovacích údajů, což může vést toodata ohrožení.

Jeden alternativní pro organizace, které mají tookeep hello celý proces ověřování řízení místní je toouse [Azure Multi-Factor Authentication Server](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), označované taky jako MFA na místě. Pomocí této metody bude stále možné tooenforce služba Multi-Factor authentication, a zajistit přitom ochranu hello MFA místní server.

Další informace o Azure MFA, přečtěte si článek hello [Začínáme s Azure Multi-Factor Authentication v cloudu hello](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Řízení přístupu (RBAC) na základě role pomocí
Omezení přístupu podle hello [potřebovat tooknow](https://en.wikipedia.org/wiki/Need_to_know) a [nejnižší oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege) Principy zabezpečení je nutné pro organizace, které mají tooenforce zásady zabezpečení pro přístup k datům. Azure na základě rolí řízení přístupu (RBAC) může být použité tooassign oprávnění toousers, skupinám a aplikacím v určitých oboru. předplatné, skupinu prostředků nebo jediný zdroj, může být Hello rozsah přiřazení role.

Můžete využít [součástí RBAC](../active-directory/role-based-access-built-in-roles.md) role v Azure tooassign toousers oprávnění. Zvažte použití *Přispěvatel účet úložiště* pro operátorům cloudu, které je třeba toomanage účty úložiště a *Classic Přispěvatel účet úložiště* role toomanage klasické účty úložiště. Operátoři cloudu, které potřebuje toomanage virtuální počítače a účet úložiště, zvažte přidání je příliš*Přispěvatel virtuálních počítačů* role.

Organizace, které nebudou vynucovat řízení přístupu dat s využitím funkcí, jako je RBAC může poskytnutí více oprávnění, než nezbytné tootheir uživatele. To může způsobit ohrožení zabezpečení toodata podle mohli uživatelé získávat přístup toocertain typy typy dat (např. vysoký obchodní dopad), který by neměl mít na prvním místě hello.

Další informace o Azure RBAC přečíst článek hello [řízení přístupu](../active-directory/role-based-access-control-configure.md).

## <a name="control-locations-where-resources-are-created-using-resource-manager"></a>Řízení umístění, kde jsou prostředky vytvořené pomocí resource manager
Povolení cloudu operátory tooperform úlohy, zatímco brání nejnovější konvence, které jsou potřeba toomanage prostředkům vaší organizace je velmi důležité. Organizace, které mají toocontrol hello umístění, kde jsou vytvářeny prostředky měli pevného kódu těchto umístění.

tooachieve, organizace můžete vytvořit zásady zabezpečení, které mají definice, které popisují akce hello nebo prostředky, které jsou výslovně odepřen. Můžete přiřadit tyto definice zásady v hello požadovaného oboru, například hello předplatné, skupinu prostředků nebo jednotlivé zdroje.

> [!NOTE]
> není to stejný hello jako RBAC, ve skutečnosti využívá RBAC tooauthenticate hello uživatele, kteří mají oprávnění toocreate tyto prostředky.
>
>

Využívání [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toocreate vlastní zásady pro scénáře, kde hello organizace chce tooallow operations pouze když hello příslušné nákladové středisko je také přidružené; jinak se zakáže hello požadavku.

Organizace, které nejsou řízení vytváření prostředky jsou náchylná toousers, který může zneužití hello služby tak, že vytvoříte více prostředků, než potřebují. Posílení zabezpečení proces vytváření prostředků hello je důležitým krokem toosecure scénáři více klientů.

Další informace o vytváření zásad s Azure Resource Manager podle přečtení článku hello [zásady používání toomanage prostředků a řízení přístupu](../azure-resource-manager/resource-manager-policy.md).

## <a name="guide-developers-tooleverage-identity-capabilities-for-saas-apps"></a>Příručka vývojáře tooleverage identity možnosti pro aplikace SaaS
Identita uživatele bude využity v řadě scénářů, při přístupu uživatelů k [aplikace SaaS](https://azure.microsoft.com/marketplace/active-directory/all/) , lze integrovat s místní nebo cloudové adresáře. Především, doporučujeme vývojáři použít zabezpečené metodika toodevelop těchto aplikací, jako například [Microsoft životního cyklu SDL (Security Development)](https://www.microsoft.com/sdl/default.aspx). Azure AD zjednodušuje ověřování pro vývojáře a poskytovat identity jako služby, podpora pro standardní protokoly, jako [OAuth 2.0](http://oauth.net/2/) a [OpenID Connect](http://openid.net/connect/), stejně jako otevřete zdroje knihovny pro různé platformy.

Ujistěte se, že tooregister jakékoli aplikace, která outsources tooAzure ověřování AD, toto je povinné procedury. Důvod Hello za to je proto Azure AD potřebuje toocoordinate hello komunikace s aplikací hello při zpracování přihlašování (SSO) nebo výměna tokeny. Hello uživatelské relace vyprší platnost, když vyprší platnost hello životnost hello tokenem vydaným službou Azure AD. Vždy vyhodnoceny, pokud vaše aplikace by měla používat této doby nebo jestli můžete snížit této doby. Doba platnosti redukční hello může fungovat jako bezpečnostní opatření, která vynutí toosign uživatelé si založené na období nečinnosti.

Organizace, které není průvodci svého vývojáře na tom, jak toosecurely integraci aplikace s jejich systém správy identit a Nevynucovat identity řízení tooaccess aplikací může být náchylnější toocredential krádež typ útoku, jako například [slabé ověřování a relace správy popsané v aplikaci otevřít webový projekt zabezpečení (OWASP) Top 10](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet).

Další informace o ověřování scénáře pro aplikace SaaS načtením [scénáře ověřování pro Azure AD](../active-directory/active-directory-authentication-scenarios.md).

## <a name="actively-monitor-for-suspicious-activities"></a>Aktivně sledovat pro podezřelé aktivity
Podle příliš[Verizon 2016 Data porušení sestavy](http://www.verizonenterprise.com/verizon-insights-lab/dbir/2016/), zneužití přihlašovacích údajů je stále v hello zvýšení a jeden z hello firmy nejvíce ziskové stane pro zločinci. Z tohoto důvodu je důležité toohave systém aktivní identitu monitorování v místě, ke kterému můžete rychle zjistit aktivity podezřelého chování a aktivaci výstrahy pro další šetření. Azure AD má dvě hlavní funkce, které pomůžou organizacím monitorovat jejich identit: Azure AD Premium [anomálií sestavy](../active-directory/active-directory-view-access-usage-reports.md) a Azure AD [ochrany identit](../active-directory/active-directory-identityprotection.md) schopností.

Ujistěte se, že toouse hello anomálií sestavy tooidentify pokusy o toosign v [bez trasovány](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md), [hrubou silou](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) útoky na konkrétní účet, pokusy o toosign v několika umístěních a přihlášení z [nakažených zařízení](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md) a podezřelé IP adresy. Uvědomte si, že jsou sestavy. Jinými slovy musí mít procesy a postupy uvedené v umístit toorun správci IT tyto sestavy na každý den hello nebo na vyžádání (obvykle ve scénáři s reakcí na incidenty).

Naproti tomu ochrany identit Azure AD je aktivní sledování systém a jeho bude příznak hello aktuální rizika na svůj vlastní řídicí panel. Kromě toho, že se také zobrazí denní souhrn oznámení e-mailem. Doporučujeme vám, že můžete upravit úroveň rizika hello tooyour podle podnikových požadavků. úroveň rizika Hello události riziko je označením hello závažnosti události riziko hello (vysoká, střední nebo nízká). úroveň rizika Hello pomáhá uživatelům Identity Protection prioritu hello akce musí přijmout tooreduce hello riziko tootheir organizace.

Organizace, které aktivně nemonitorujte jejich systémy identity je riziko toho, že dojde k ohrožení přihlašovací údaje uživatele. Bez znalostní báze, který trvá podezřelé aktivity umístit pomocí těchto přihlašovacích údajů, organizace nebude mít toomitigate tento typ hrozeb.
Další informace o ochrany identit Azure načtením [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md).
