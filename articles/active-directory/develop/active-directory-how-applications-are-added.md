---
title: "aaaHow aplikace se přidají tooAzure služby Active Directory."
description: "Tento článek popisuje, jak se aplikace přidávají tooan instance služby Azure Active Directory."
services: active-directory
documentationcenter: 
author: shoatman
manager: kbrint
editor: 
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2016
ms.author: shoatman
ms.custom: aaddev
ms.openlocfilehash: 3ca710c58a403b52e8b728202ad9010f4873bcea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-and-why-applications-are-added-tooazure-ad"></a>Jak a proč se aplikace přidávají tooAzure AD
Jeden z hello původně puzzling věcí, při zobrazení seznamu aplikací ve vaší instanci Azure Active Directory je pochopení, odkud pochází hello aplikace a proč jsou dispozici.  Tento článek vám poskytne podrobný přehled jak aplikací jsou reprezentována v adresáři hello a poskytnout kontext, který vám pomůže porozumět, jak aplikaci pocházejí toobe ve vašem adresáři.

## <a name="what-services-does-azure-ad-provide-tooapplications"></a>Jaké služby Azure AD poskytuje tooapplications?
Aplikace se přidají tooAzure AD tooleverage jeden nebo více hello služeb, které poskytuje.  Tyto služby patří:

* Aplikace ověřování a autorizace
* Ověření uživatele & autorizace
* Jednotné přihlašování (SSO) pomocí federace nebo hesla
* Zřizování uživatelů a synchronizace
* Řízení přístupu na základě rolí; Použití hello directory toodefine aplikační role tooperform role na základě kontroly autorizace v aplikaci.
* služby autorizace oAuth (používá Office 365 a další zdroje společnosti Microsoft aplikace tooauthorize přístup tooAPIs /).
* Publikování aplikací pro & XY; Publikování aplikace z toohello privátní sítě internet

## <a name="how-are-applications-represented-in-hello-directory"></a>Jak jsou v adresáři hello reprezentované aplikace?
Aplikace jsou reprezentována v hello Azure AD pomocí 2 objekty: objekt aplikace a objektu zabezpečení služby.  Se jeden objekt aplikace zaregistrované v "home" nebo "vlastník" nebo "publikování" adresář a jeden nebo více služeb hlavní objekty představující hello aplikace v každém adresáři, ve kterém funguje.  

Hello objektu application popisuje tooAzure aplikace hello AD (hello víceklientské služby) a může obsahovat žádné z následujících hello: (*Poznámka*: Nejedná se o vyčerpávající seznam.)

* Název, loga a vydavatel
* Tajné klíče (symetrické nebo asymetrické klíče používá aplikace hello tooauthenticate)
* Rozhraní API závislosti (oAuth)
* Rozhraní API prostředky nebo oborům publikována (oAuth)
* Aplikace role (RBAC)
* Metadata jednotné přihlašování a konfigurace (SSO)
* Zřizování uživatelů metadata a konfigurace
* Proxy server, metadat a konfigurace

Hello instanční objekt je záznam hello aplikace v každém adresáři, kde aplikace hello funguje, včetně jeho domovského adresáře.  Hello instanční objekt:

* Odkazuje zpět objektu application tooan přes vlastnost id aplikace hello
* Zaznamenává místních uživatelů a přiřazení role aplikace skupin
* Zaznamenává místní uživatele a správce oprávnění udělené toohello aplikace
  * Příklad: oprávnění pro tooaccess aplikace hello e-mailu s konkrétní uživatelé
* Zaznamenává místní zásady, včetně zásad podmíněného přístupu
* Zaznamenává místní alternativní místní nastavení pro aplikace
  * Pravidla transformace deklarací identity
  * Mapování atributů (zřizování uživatelů)
  * Klienta konkrétní aplikaci rolí (Pokud hello aplikace podporuje vlastní role)
  * Název nebo loga

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Diagram objekty aplikací a objekty služby napříč adresáře
![Diagram znázorňující, jak aplikace objekty a služby objekty existující s instancí služby Azure AD.][apps_service_principals_directory]

Jak je vidět z hello diagramu výše.  Společnost Microsoft spravuje dva adresáře interně (na levé straně hello) používá toopublish aplikace.

* Jeden pro Microsoft Apps (Microsoft services directory)
* Jeden pro předběžně integrované aplikace 3. stran (adresář Galerie aplikací)

Aplikace vydavatelů nebo dodavatelů, kteří integraci se službou Azure AD jsou požadované toohave publikování adresáře.  (Adresář některé SAAS).

Aplikace, které přidáte sami patří:

* Aplikace, které jste vytvořili (integrovaný s AAD)
* Aplikace, které jste připojeni pro jednotného přihlašování
* Aplikace, které jste publikovali pomocí hello proxy aplikace služby Azure AD.

### <a name="a-couple-of-notes-and-exceptions"></a>Několik poznámek a výjimek
* Ne všechny objekty služby bodu back tooapplication objekty.  Huh? Pokud Azure AD bylo původně vytvořeno hello služby poskytovány tooapplications byly mnohem omezenější a dostatečné pro vytvoření identity aplikace hello instanční objekt.  Hello původní instanční objekt byl co nejblíže ve tvaru toohello účet služby Windows Server Active Directory.  Z tohoto důvodu, že je stále možné toocreate hello objekty služby pomocí Azure AD PowerShell bez vytvoření první objekt aplikace.  Hello rozhraní Graph API vyžaduje objekt aplikace před vytvořením objektu služby.
* Ne všechny hello informace popsané výše je aktuálně zveřejněný prostřednictvím kódu programu.  Následující Hello jsou dostupné jenom v hello uživatelského rozhraní:
  * Pravidla transformace deklarací identity
  * Mapování atributů (zřizování uživatelů)
* Další podrobné informace o objektu služby hello a objekty aplikací naleznete v referenční dokumentaci toohello Azure AD Graph REST API.  *Pomocný parametr*: hello dokumentace k Azure AD Graph API je hello nejbližší věc tooa schéma – referenční informace pro Azure AD, který je aktuálně k dispozici.  
  * [Aplikace](https://msdn.microsoft.com/library/azure/dn151677.aspx)
  * [Instanční objekt](https://msdn.microsoft.com/library/azure/dn194452.aspx)

## <a name="how-are-apps-added-toomy-azure-ad-instance"></a>Jak jsou přidané aplikace toomy instancí Azure AD?
Aplikaci lze přidat tooAzure AD mnoha způsoby:

* Přidat aplikaci z hello [Galerie Azure Active Directory aplikací](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Přihlášení/do 3rd, aplikace jiných výrobců integrované s Azure Active Directory (například: [Smartsheet](https://app.smartsheet.com/b/home) nebo [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
  * Při přihlášení/v uživatelé jsou kladené toogive oprávnění toohello aplikace tooaccess svůj profil a další oprávnění.  první osoba toogive Hello souhlas příčiny instanční objekt reprezentující hello aplikace toobe přidané toohello adresáře.
* Přihlášení/do služeb Microsoft online services, jako [Office 365](http://products.office.com/)
  * Když se přihlásíte tooOffice 365 nebo zahájit zkušební jeden nebo více objekty služby jsou vytvořené v hello directory představující hello různé služby, které jsou používané toodeliver všechny hello funkce související s Office 365.
  * Některé služby Office 365, jako je SharePoint vytvořit objekty služby průběžné základ tooallow zabezpečenou komunikaci mezi součástmi, včetně pracovních postupů.
* Přidat aplikaci vyvíjíte v portálu pro správu Azure, najdete v části hello: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Přidání, najdete v části aplikace, které vyvíjíte pomocí sady Visual Studio:
  * [Metody ověřování ASP.Net](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
  * [Připojených služeb](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Přidat aplikaci toouse toouse hello [proxy aplikace služby Azure AD](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Připojit aplikaci pro jednotné přihlašování pomocí SAML nebo jednotného přihlašování k heslo
* Mnoho dalších včetně různé možnosti pro vývojáře v Azure a nebo v Průzkumníku API výsledky v různých centra pro vývojáře

## <a name="who-has-permission-tooadd-applications-toomy-azure-ad-instance"></a>Kdo má instancí Azure AD toomy aplikace tooadd oprávnění?
Jenom globální správci postupovat následovně:

* Přidání aplikací z Galerie aplikace hello Azure AD (předem integrovaných 3. stran aplikace)
* Publikování aplikace pomocí hello proxy aplikace služby Azure AD

Všichni uživatelé v adresáři mít práva tooadd aplikace, které jejich vývoj a uvážení přes aplikace, které se sdílená složka nebo udělení přístupu tootheir organizačním datům.  *Mějte na paměti, přihlášení uživatele nahoru nebo v aplikaci tooan a udělení oprávnění může mít za následek objekt při vytváření služby.*

Tato původně zvukové týkající se může, ale zachovat hello na paměti následující:

* Aplikace se může tooleverage Windows Server Active Directory pro ověřování uživatelů pro mnoho let bez nutnosti toobe aplikace hello zaznamenávají v adresáři hello jejich zaregistrován.  Teď hello organizace bude mít lepší viditelnost tooexactly jak velký počet aplikací používají hello adresář a what for.
* Pro správce řízené proces publikování/registrace aplikace není nutné.  S Active Directory Federation Services je pravděpodobné, že správce měl tooadd aplikace jako předávající strana jménem vývojáři.  Nyní mohou vývojáři samoobslužné služby.
* Uživatelé podepisování vstupně -až tooapps pomocí účtu organizace pro obchodní účely je dobré.  Pokud následně opustí organizaci hello dojde ke ztrátě účet pro přístup k tootheir hello aplikace, které používají.
* S záznam jaká data byla sdílené, pomocí kterého aplikace je dobré.  Data jsou přenositelné více než kdy dřív a nutnosti zrušte záznam kdo jaká data sdílet s aplikací, které jsou užitečné.
* Aplikace, které používají Azure AD pro oAuth rozhodnout přesně uvedete, jaká oprávnění, že jsou uživatelé moct toogrant tooapplications a oprávnění, která vyžadují služby Správce tooagree k.  Má přejít bez informacemi o tom, že můžete pouze správci souhlas toolarger obory a větších oprávnění.
* Uživatele, přidání a umožní, že aplikace tooaccess, svá data jsou auditované události tak můžete zobrazit sestavy hello auditu v rámci portálu toodetermine hello Azure Managment jak aplikaci byl přidán toohello adresáře.

**Poznámka:** *Microsoft samotné provozována pomocí hello výchozí konfiguraci pro mnoho měsíců teď.*

Se všemi této uvedli, že je možné tooprevent uživatelé v adresáři z přidání aplikací a výkonu uvážení nad jaké informace sdílejí s aplikacemi změnou konfigurace Directory na portálu pro správu Azure hello.  Hello následující konfigurace jsou přístupné v rámci portálu pro správu Azure hello na svého adresáře na kartě "Konfigurace".

![Snímek obrazovky hello uživatelského rozhraní pro konfiguraci nastavení integrované aplikace][app_settings]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
Další informace o tooadd tooAzure aplikací AD a jak tooconfigure služeb pro aplikace.

* Vývojáři: [informace jak toointegrate aplikace v AAD](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Vývojáři: [zkontrolujte ukázkový kód pro aplikace, které jsou integrované s Azure Active Directory na Githubu](https://github.com/AzureADSamples)
* Vývojářů a IT profesionály: [zkontrolujte hello dokumentace k REST API pro Azure Active Directory Graph API hello](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* IT specialisté: [zjistěte, jak toouse Azure Active Directory předem integrovaných aplikací od hello Galerie aplikací](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* IT specialisté: [kurzy pro konfiguraci konkrétní předběžně integrované aplikace](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* IT specialisté: [zjistěte, jak hello toopublish aplikace pomocí Azure Proxy aplikace služby Active Directory](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Viz také
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](../active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:../media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
