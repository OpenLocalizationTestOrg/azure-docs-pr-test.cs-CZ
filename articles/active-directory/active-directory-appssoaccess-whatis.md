---
title: "aaaWhat je přístup k aplikaci a jednotné přihlašování s Azure Active Directory? | Dokumentace Microsoftu"
description: "Pomocí Azure Active Directory tooenable jeden přihlašování tooall hello SaaS a webových aplikací, které potřebujete pro firmy."
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 75d1a3fd-b3c5-4495-a5c8-c4c24145ff00
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: curyand
ms.openlocfilehash: 429522cbd570ab27359c4630c5a6d7b25b692ec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-application-access-and-single-sign-on-with-azure-active-directory"></a>Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?
Jednotné přihlašování znamená, že je schopný tooaccess všechna hello aplikací a prostředky, které potřebujete toodo podnikání, po přihlášení pouze jednou pomocí jediného uživatelského účtu. Jakmile se přihlásíte, dostanete všechny aplikace hello budete potřebovat, aniž by musel být požadované tooauthenticate (např. Zadejte heslo) ještě jednou.

Mnoho organizací závisí software jako služba (SaaS) aplikace například Office 365, pole a Salesforce pro produktivita koncového uživatele. V minulosti IT oddělení potřebuje tooindividually vytvářet a aktualizovat uživatelské účty v každé aplikaci SaaS a uživatelé mají tooremember heslo pro každou aplikaci SaaS.

Azure Active Directory rozšiřuje místní služby Active Directory do cloudu hello povolení uživatelé toouse primární účet organizace toonot pouze přihlášení tootheir připojená k doméně a prostředkům společnosti, ale také všechny hello webu a aplikace SaaS potřebné pro své úlohy.

Proto jenom uživatelé nemají toomanage více sad uživatelská jména a hesla, jejich aplikace přístup může automaticky zřídit nebo zrušte zřídit na základě jejich členové skupiny organizace, a také jejich stav jako zaměstnanci. Azure Active Directory nabízí zabezpečení a řízení přístupu zásad správného řízení, které umožňují toocentrally můžete spravovat přístup uživatelů v rámci aplikací SaaS.

Azure AD umožňuje snadnou integraci toomany dnešních oblíbených aplikací SaaS; poskytuje správu identit a přístupu a umožňuje uživatelům toosingle přihlašování tooapplications přímo, nebo zjistit a spustit je z portálu, jako je například Office 365 nebo hello přístupový panel Azure AD.

Architektura Hello hello integrace se skládá z hello následující čtyři hlavní stavební bloky:

* Jednotné přihlašování umožňuje uživatelům tooaccess jejich SaaS aplikací založených na účet své organizace v Azure AD. Jednotné přihlašování je co umožňuje uživatelům tooauthenticate tooan aplikace pomocí jednoho účet své organizace.
* Zřizování uživatelů umožňuje zřizování uživatelů a jeho rušení do cílové SaaS v závislosti na změny provedené v systému Windows Server Active Directory nebo Azure AD. Zřízeného účtu je co umožňuje toouse toobe oprávnění uživatele aplikaci, po mít ověření přes jednotné přihlašování.
* Správa přístupu k centralizované aplikací v nástroji hello portálu pro správu Azure umožňuje přístup k aplikaci SaaS jeden bod správy a, pomocí hello možnost toodelegate aplikace přístup rozhodnutí provádění a schválení tooanyone v organizaci hello
* Sjednocené vytváření sestav a monitorování aktivity uživatelů ve službě Azure AD

## <a name="how-does-single-sign-on-with-azure-active-directory-work"></a>Jak funguje jednotné přihlašování pomocí služby Azure Active Directory?
Když uživatel "přihlášení" tooan aplikace pomocí přejde prostřednictvím procesu ověřování tam, kde jsou požadované tooprove, které se skutečně říká se, že jsou. Bez jednotného přihlašování, obvykle se to dělá tak, že zadáte heslo, které je uloženo na aplikace hello a uživatel hello požadované tooknow toto heslo.

Azure AD podporuje tři různé způsoby toosign v tooapplications:

* **Federované jednotné přihlašování** umožňuje aplikacím tooredirect tooAzure AD pro ověřování uživatelů místo výzvy pro vlastní heslo. Toto je podporováno pro aplikace, který podporu protokoly například SAML 2.0, WS-Federation nebo OpenID Connect a je hello nejkomplexnější režim jednotné přihlašování.
* **Založené na heslech jednotné přihlašování** umožňuje zabezpečit ukládání hesel aplikací a přehráním pomocí mobilní aplikace nebo rozšíření webové prohlížeče. Toto využívá hello existující přihlašovací proces poskytované hello aplikace, ale umožňuje hesly správce toomanage hello a nevyžaduje hello uživatele tooknow hello heslo.
* **Existující jednotné přihlašování** všechny existující jednotné přihlašování, nebyla konfigurována hello aplikace, ale umožňuje tyto toohello toobe propojené aplikací Office 365 nebo Azure AD přístup k panelu portálů a taky umožňuje umožňuje tooleverage Azure AD Další vytváření sestav v Azure AD, pokud existuje spouštění aplikací hello.

Jakmile se s aplikací mít ověření uživatele, potřebují toohave záznamu klienta zřídí v hello aplikace, která říká službě aplikace hello kde existuje oprávnění a úroveň přístupu jsou uvnitř aplikace hello. Hello zřizování tento záznam účtu může buď dojít automaticky, nebo může k ní dojít ručně správcem předtím, než uživatel hello zajišťuje přístup přihlášení.

 Další informace o těchto režimech jednoho přihlášení a zřizování níže.

### <a name="federated-single-sign-on"></a>Federované jednotné přihlašování
Federované jednotné přihlašování umožňuje přihlašování umožňuje hello uživatele ve vaší organizaci toobe automaticky přihlášení aplikace SaaS jiných výrobců tooa službou Azure AD pomocí informací o účtu uživatele hello z Azure AD.

V tomto scénáři Pokud jste již byly zaprotokolovány do služby Azure AD, a chcete tooaccess prostředky, které jsou řízené aplikace SaaS jiných výrobců, federační eliminuje potřebu hello uživatele toobe, znovu ověřit.

Federované jednotné přihlašování s aplikacemi, které podporují hello SAML 2.0, WS-Federation, může podporovat Azure AD nebo OpenID connect protokoly.

Viz také: [Správa certifikátů pro federované jednotné přihlašování](active-directory-sso-certs.md)

### <a name="password-based-single-sign-on"></a>Založené na heslech jednotné přihlašování
Konfigurace založené na heslech jednotné přihlašování umožňuje hello uživatele ve vaší organizaci toobe automaticky přihlášení aplikace SaaS jiných výrobců tooa službou Azure AD pomocí informací o účtu uživatele hello z aplikace SaaS jiných výrobců hello. Když povolíte tuto funkci, Azure AD shromažďuje a bezpečně ukládá informace o uživatelském účtu hello a související heslo hello.

Azure AD může podporovat založené na heslech jednotného přihlašování na pro všechny cloudové aplikaci, která má HTML na přihlašovací stránku. Pomocí modulu plug-in vlastním prohlížeči AAD automatizuje v procesu prostřednictvím bezpečně načítání pověření aplikací, jako je například hello uživatelské jméno a heslo hello z adresáře hello přihlašování hello uživatele a zadá tyto přihlašovací údaje do aplikace hello stránku pro přihlášení jménem uživatele hello. Existují dva případy použití:

1. **Správce spravuje přihlašovací údaje** – můžete vytvořit a spravovat přihlašovací údaje aplikací a přiřaďte tyto přihlašovací údaje toousers nebo skupiny, kteří potřebují přístup k aplikaci toohello správci. V těchto případech hello koncový uživatel nemusí tooknow hello pověření, ale stále získá přístup přihlášení toohello aplikací jednoduše tak, že na ni kliknete v jejich přístupového panelu nebo prostřednictvím zadaný odkaz. To umožňuje obě, Správa životního cyklu hello přihlašovacích údajů správce hello, jakož i pohodlí pro koncové uživatele, které nebudou potřebovat tooremember ani spravovat hesla konkrétní aplikaci. Hello přihlašovací údaje jsou zamaskované z hello koncového uživatele při přihlášení automatizované hello v procesu; ale nebudou technicky zjistitelný uživatelem hello pomocí ladění webové nástroje a uživatelé a správci musí postupovat podle hello stejné zásady zabezpečení, jako kdyby hello pověření prezentovaly přímo uživatelem hello. Zadaný správce přihlašovacích údajů jsou velmi užitečné při poskytování přístupu účtu, která je sdílena mezi mnoha uživateli, jako je například sociálních médií nebo aplikace pro sdílení dokumentu.
2. **Spravuje přihlašovací údaje uživatele** – správci můžete přiřadit aplikace tooend uživatele nebo skupiny a povolit hello koncoví uživatelé tooenter svoje vlastní přihlašovací údaje přímo na přístup k aplikaci hello hello poprvé v jejich přístupového panelu. Tím se vytvoří v zájmu usnadnění pro koncové uživatele, které nepotřebují toocontinually zadejte hesla pro konkrétní aplikace hello pokaždé, když chtějí získat přístup hello aplikaci. Tento případ použití mohou sloužit také jako taktování správu kamenné tooadministrative hello přihlašovacích údajů, které hello správce může nastavit nové přihlašovací údaje pro aplikace hello později beze změny prostředí přístup aplikace hello hello koncového uživatele.

V obou případech přihlašovací údaje jsou uložené v šifrovaném stavu v adresáři hello a se předávají pouze prostřednictvím protokolu HTTPS během hello automatizované přihlášení. Pomocí jednotného přihlašování založené na heslech na, Azure AD nabízí řešení pro aplikace, které nejsou podporovat protokoly federační správu přístupu pohodlný identity.

Jednotné přihlašování založené na heslech využívá prohlížeči rozšíření toosecurely načtení hello aplikaci a uživatele konkrétní informace ze služby Azure AD a použijte ho toohello služby. Tuto funkci podporovat většinu aplikací SaaS jiných výrobců, které jsou podporovány službou Azure AD.

Pro jednotné přihlašování založené na heslech může být prohlížeče hello koncového uživatele:

* Internet Explorer 8, 9, 10, 11 – na systému Windows 7 nebo novější (viz také [IE rozšíření Deployment Guide](active-directory-saas-ie-group-policy.md))
* Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější
* Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější

**Poznámka:** rozšíření jednotného přihlašování založené na heslech hello bude k dispozici pro Edge ve Windows 10 při rozšíření prohlížeče stát podporované pro okraj.

### <a name="existing-single-sign-on"></a>Existující jednotné přihlašování
Při konfiguraci jednotné přihlašování pro aplikace, portál pro správu Azure hello poskytuje třetí možnost z "existující Single Sign-On". Tato možnost jednoduše umožňuje toocreate správce hello k aplikaci tooan odkaz a umístěte ji na hello panel přístupu pro vybraného uživatele.

Například pokud je, že aplikace, která je nakonfigurovaná tooauthenticate uživatele služby Active Directory Federation Services 2.0, správce použít hello "existující Single Sign-On" možnost toocreate tooit odkaz na panel přístupu hello. Když uživatelé přistupují k hello odkaz, musí se ověřit pomocí Active Directory Federation Services 2.0 nebo jakoukoli existující jeden přihlašování řešení je k dispozici aplikace hello.

### <a name="user-provisioning"></a>Zřizování uživatelů
U aplikací, vyberte možnost Azure AD umožňuje zřizování automatizované uživatele a jeho rušení účtů v jiných výrobců aplikace SaaS v rámci hello portálu pro správu Azure, pomocí informací o vašem Windows Server Active Directory nebo Azure AD identity. Pokud má uživatel oprávnění ve službě Azure AD pro jednu z těchto aplikací, účet se dají automaticky vytvořit (zřízená) hello cíl aplikací SaaS.

Při odstranění uživatele nebo jeho informace o změny ve službě Azure AD, tyto změny se projeví také v hello aplikaci SaaS. To znamená, konfigurace automatizované identity životního cyklu správy umožňuje správcům toocontrol a poskytnout automatické zřizování a jeho rušení z aplikací SaaS. Ve službě Azure AD je povolena tato automatizace správy životního cyklu identit podle zřizování uživatelů.

Další, najdete v části toolearn [automatické zřizování uživatelů a jeho rušení tooSaaS aplikace](active-directory-saas-app-provisioning.md)

## <a name="get-started-with-hello-azure-ad-application-gallery"></a>Začínáme s hello galerii aplikací Azure AD
Připraveno tooget spustit? toodeploy jednotné přihlašování mezi službou Azure AD a aplikace SaaS, které vaše organizace používá, postupujte podle těchto pokynů.

### <a name="using-hello-azure-ad-application-gallery"></a>Pomocí hello galerii aplikací Azure AD
Hello [Azure Active Directory Application Gallery](https://azure.microsoft.com/marketplace/active-directory/all/) poskytuje seznam aplikací, které jsou známé toosupport forma jednotné přihlašování s Azure Active Directory.

![][1]

Zde jsou některé tipy pro hledání aplikace podle jaké funkce podporují:

* Azure AD podporuje automatické zřizování a jeho rušení pro všechny aplikace "Doporučený" hello [Azure Active Directory Application Gallery](https://azure.microsoft.com/marketplace/active-directory/all/).
* Seznam federované aplikace, které podporují konkrétně federovaného jednotného přihlašování pomocí protokolu, například SAML, WS-Federation, nebo OpenID Connect najdete [zde](http://social.technet.microsoft.com/wiki/contents/articles/20235.azure-active-directory-application-gallery-federated-saas-apps.aspx).

Jakmile jste najde aplikaci, můžete začít postupujte podle hello podrobné pokyny uvedené v galerii aplikací hello a v tooenable portálu správy Azure hello jednotného přihlašování.

### <a name="application-not-in-hello-gallery"></a>Aplikace není v galerii hello?
Pokud vaše aplikace nebyl nalezen v galerii aplikací Azure AD hello, máte tyto možnosti:

* **Přidat neuvedené aplikaci používáte** -použití hello vlastní kategorii v galerii aplikací hello v rámci portálu správy Azure hello – tooconnect neuvedené aplikaci, která vaše organizace používá. Přidáním jakékoli aplikace, který podporuje SAML 2.0 jako federované aplikace nebo jakékoli aplikace, která má HTML na přihlašovací stránku jako heslo jednotného přihlašování k aplikaci. Další podrobnosti najdete v tomto článku na [přidání vlastní aplikace](active-directory-saas-custom-apps.md).
* **Přidat vlastní aplikaci vyvíjíte** – Pokud jste si vytvořili hello aplikaci sami, postupujte podle pokynů hello v hello Azure AD vývojáře dokumentaci tooimplement federovaného jednotného přihlašování nebo zřizování s využitím hello rozhraní Azure AD graph API. Další informace naleznete v následujících zdrojích:
  
  * [Scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md)
  * [https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore)
* **Žádosti o aplikace integrační** -žádosti o podporu pro aplikace hello musíte pomocí hello [fóru pro zpětnou vazbu Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory/).

### <a name="using-hello-azure-management-portal"></a>Pomocí portálu pro správu Azure hello
Hello rozšíření Active Directory můžete použít v hello portálu pro správu Azure tooconfigure hello aplikace jednotného přihlašování. Jako první krok je třeba tooselect adresáře ze služby Active Directory části portálu hello hello:

![][2]

toomanage vaše aplikace SaaS jiných výrobců, můžete přepnout na kartě aplikace hello hello vybraný adresář. Toto zobrazení umožňuje správcům:

* Přidat nové aplikace z Galerie Azure AD hello, jakož i aplikace, které vyvíjíte
* Odstranit integrované aplikace
* Spravovat hello aplikací, které již mají integrované

Typické úlohy správy pro aplikace SaaS jiných výrobců jsou:

* Povolení jednotného přihlašování s Azure AD, heslem jednotného přihlašování nebo, pokud je k dispozici pro cíl hello SaaS federovaného jednotného přihlašování
* Volitelně můžete povolit pro uživatele, zřizování a jeho rušení (Správa životního cyklu identity) zřizování uživatelů
* Pro aplikace, kde je povoleno zřizování uživatelů, výběr uživatelů, kteří mají přístup aplikace toothat

Pro galerii aplikace, které podporují federované jednotné přihlašování konfigurace obvykle vyžaduje další konfiguraci nastavení tooprovide jako třeba certifikáty a metadata toocreate federovaného vztahu důvěryhodnosti mezi aplikací třetích stran hello a Azure AD. Hello Průvodce konfigurací vás provede hello podrobnosti a poskytuje vám toohello snadný přístup k datům konkrétní aplikace SaaS a pokyny.

Pro aplikace Galerie, které podporují zřizování automatické uživatelů to vyžaduje jste toogive Azure AD oprávnění toomanage vaše účty v aplikaci SaaS hello. Minimálně je třeba tooprovide, které přihlašovací údaje Azure AD má použít při ověřování přes toohello cílová aplikace. Jestli další konfiguraci nastavení je třeba toobe poskytuje závisí na hello požadavky aplikace hello.

## <a name="deploying-azure-ad-integrated-applications-toousers"></a>Nasazení služby Azure AD integrované aplikace toousers
Azure AD poskytuje několik způsobů přizpůsobitelné toodeploy aplikace tooend – uživatelé ve vaší organizaci:

* Azure AD přístupového panelu
* Spouštěč aplikace Office 365
* Přímé přihlášení toofederated aplikace
* Přímé odkazy toofederated, založené na heslech, nebo existující aplikace

Které metody zvolte toodeploy ve vaší organizaci je vašeho vlastního rozhodnutí.

### <a name="azure-ad-access-panel"></a>Azure AD přístupového panelu
v https://myapps.microsoft.com Hello přístupový Panel je webový portál, který umožňuje koncový uživatel s účtem organizace v Azure Active Directory tooview a spuštění toowhich cloudové aplikace, které nejsou udělen přístup podle hello Azure AD správce. Pokud jste koncovým uživatelem s [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), můžete také používat skupiny samoobslužné služby možnosti správy prostřednictvím hello přístupového panelu.

![][3]

Hello přístupový Panel je oddělená od hello portálu pro správu Azure a nevyžaduje, aby uživatelé toohave předplatného Azure nebo předplatné služeb Office 365.

Další informace o přístupovém panelu hello Azure AD, najdete v části hello [Úvod toohello přístupový panel](active-directory-saas-access-panel-introduction.md).

### <a name="office-365-application-launcher"></a>Spouštěč aplikace Office 365
Pro organizace, které jste nasadili Office 365 aplikace, které jsou přiřazené toousers prostřednictvím služby Azure AD zobrazí také na https://portal.office.com/myapps portál hello Office 365. Díky tomu snadný a pohodlný pro uživatele v organizaci toolaunch svoje aplikace bez nutnosti toouse druhý portál a doporučuje hello aplikace spuštění řešení pro organizace používá Office 365.

![][4]

Další informace o spuštění aplikace hello Office 365 najdete v tématu [mít vaše aplikace se zobrazí v Spouštěč aplikace hello Office 365](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

### <a name="direct-sign-on-toofederated-apps"></a>Přímé přihlášení toofederated aplikace
Nejvíce federovaným aplikacím, které podporují SAML 2.0 a WS-Federation, OpenID připojte také možnost hello podpory pro uživatele toostart na hello aplikace a potom získat přihlášení prostřednictvím služby Azure AD pomocí automatické přesměrování nebo kliknutím na odkaz toosign v. To se označuje jako poskytovatel služeb-zahájí přihlášení, a podporují nejvíce federovaným aplikacím v galerii aplikací Azure AD hello (viz dokumentace hello odkazované z Průvodce konfigurace přihlášení aplikace hello hello portálu správy Azure Podrobnosti).

![][5]

### <a name="direct-sign-on-links-for-federated-password-based-or-existing-apps"></a>Přímé odkazy přihlášení pro federované, založené na heslech nebo existující aplikace
Azure AD podporuje aplikace tooindividual přímé jeden přihlašování odkazy, které podporují založené na heslech jednotné přihlašování, existující jednotné přihlašování a jakoukoli formu federované jednotné přihlašování.

Tyto odkazy jsou adresy URL specificky vytvořený, které odeslat uživatele prostřednictvím hello Azure AD přihlášení proces pro danou aplikaci bez nutnosti hello uživatele je spustit z přístupového panelu hello Azure AD nebo Office 365. Tyto jedné adresy URL přihlašování najdete v části karty řídicí panel hello všechny předem integrované aplikace v hello služby Active Directory části hello portál pro správu Azure, jak ukazuje následující snímek obrazovky hello.

![][6]

Tyto odkazy můžete zkopírovat a vložit libovolné že místo tooprovide u přihlášení odkaz toohello vybrané aplikace. To může být v e-mailu nebo v jakékoli vlastní webový portál, který jste nastavili pro přístup k aplikaci uživatele. Tady je příklad Azure AD přímé jeden přihlašovací adresa URL pro Twitter:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Podobné tooorganization konkrétní adresy URL pro hello přístupového panelu, můžete dále přizpůsobit tuto adresu URL jednu z hello aktivní nebo ověřených domén pro váš adresář přidáním po hello myapps.microsoft.com domény. To zajistí, že organizace branding je načíst okamžitě na přihlašovací stránku hello bez nutnosti tooenter jejich ID uživatele nejprve uživatele hello:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Když oprávněný uživatel klikne na jeden z těchto odkazů specifické pro aplikace, se nejprve zobrazit jejich organizace přihlašovací stránka (za předpokladu, že se ještě nejste přihlášeni) a po přihlášení k aplikaci přesměrované tootheir bez zastavení na panel přístupu hello. Pokud uživatel hello chybí požadavky, že tooaccess hello aplikace, jako je například rozšíření prohlížeče hello založené na heslech jednotné přihlašování, zobrazí výzvu hello odkaz hello uživatele tooinstall hello chybějící rozšíření. Adresa URL odkazu Hello také konstantní konfigurace, pokud hello jeden přihlašování pro aplikace hello.

Použijte tyto odkazy hello stejné mechanismy řízení přístupu jako hello přístup k panelu a Office 365 a pouze tito uživatelé nebo skupiny, kteří mají přiřazený toohello aplikace v portálu správy Azure hello bude mít toosuccessfully ověřit. Každý uživatel, který není autorizovaný se však zobrazí zprávu s vysvětlením, že nebyl udělen přístup a jsou uvedeny na odkaz tooload hello přístup k panelu tooview k dispozici aplikacím ke kterým mají přístup.

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vyhledání aplikace neschválená cloudových aplikací s Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md)
* [Úvod tooManaging tooApps přístup](active-directory-managing-access-to-apps.md)
* [Porovnání funkcí pro správu externí identity ve službě Azure AD](active-directory-b2b-compare-external-identities.md)

<!--Image references-->
[1]: ./media/active-directory-appssoaccess-whatis/onlineappgallery.png
[2]: ./media/active-directory-appssoaccess-whatis/azuremgmtportal.png
[3]: ./media/active-directory-appssoaccess-whatis/accesspanel.png
[4]: ./media/active-directory-appssoaccess-whatis/officeapphub.png
[5]: ./media/active-directory-appssoaccess-whatis/workdaymobile.png
[6]: ./media/active-directory-appssoaccess-whatis/deeplink.png
