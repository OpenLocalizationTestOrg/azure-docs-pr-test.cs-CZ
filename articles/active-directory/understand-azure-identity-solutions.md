---
title: aaaUnderstand Azure Identity | Microsoft Docs
description: "Získáte základní znalosti o podmínky řešení identity Microsoft Azure, koncepty a doporučení pro jste toomake hello nejlepší identity zásad správného řízení rozhodnutí pro vaši organizaci."
keywords: 
author: jeffgilb
manager: femila
ms.reviewer: jsnow
ms.author: jeffgilb
ms.date: 7/17/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: 4d9c90bd7a6bcc9637be3107998f9da5bd4cbdae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-identity-solutions"></a>Princip řešení identit Azure
Microsoft Azure Active Directory (Azure AD) je identit a přístupu cloudové řešení pro správu poskytující adresářových služeb, zásad správného řízení identity a správa přístupu aplikací. Azure AD rychle [umožňuje jednotné přihlašování (SSO)](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) too1, 000 pro předběžně integrované komerční a vlastních aplikací v hello [galerii aplikací Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/). Mnoho z těchto aplikací, které pravděpodobně již používáte například Office 365, Salesforce.com, pole, ServiceNow a během pracovního dne.

Jeden adresář Azure AD se automaticky přidruží předplatné Azure, když je vytvořeno. Jako hello identity služby v Azure Azure AD poté poskytuje všechny funkce řízení přístupu a správu identit pro cloudové prostředky. Tyto prostředky mohou zahrnovat uživatele, aplikace a skupiny pro jednoho klienta (organizace), jak je znázorněno v následujícím diagramu hello:

![Azure Active Directory](./media/understand-azure-identity-solutions/azure-ad.png)

Microsoft Azure nabízí několik způsobů tooleverage identity jako služby (IDaaS) s různými úrovněmi složitost toomeet jednotlivých podle potřeb organizace. Hello zbytek Tento článek vám pomůže pochopit základní identit Azure terminologie a koncepty, jakož i doporučení pro jste toomake hello nejlepší volbou z dostupných možností hello.

## <a name="terms-tooknow"></a>Tooknow podmínky

Než v rámci řešení identit Azure, můžete rozhodnout pro vaši organizaci, je třeba základní znalosti o hello podmínky běžně používané při posuzování služby Azure identity.

|Termín tooknow| Popis|
|-----|-----|
|Předplatné Azure |Odběry se toopay používaných pro cloudové služby Azure a jsou obvykle propojené tooa platební karty. Může mít několik odběrů, ale může být obtížné tooshare prostředků mezi předplatnými.|
|Klientovi Azure | Klient služby Azure AD je typický pro potřeby jedné organizace. Je vyhrazené důvěryhodné instanci Azure AD, která se automaticky vytvoří při organizace zaregistruje k odběru služby Microsoft cloudu, například Azure, Intune nebo Office 365. Klienti mohou získat přístup tooservices v vyhrazené prostředí (jednoho klienta) nebo v prostředí sdílené s dalšími organizacemi, (víceklientská).|
|Adresář Azure AD | Každý klient Azure má vyhrazený, důvěryhodné Azure AD adresář, který obsahuje uživatele, skupiny a aplikace hello klienta. Je použité tooperform identit a přístupu funkce správy pro klientské prostředky. Protože jedinečný adresář Azure AD je automaticky zřízeného toorepresent vaší organizace při registraci v cloudové službě Microsoftu jako Azure, Microsoft Intune nebo Office 365, v některých případech uvidíte hello podmínky *klienta*, *Azure AD*, a *adresář Azure AD* zaměnitelné. |
|Vlastní doména | Při první registraci k odběru služby Microsoft cloudu, vašeho klienta (organizace) používá *. onmicrosoft.com* název domény. Ale většina organizací má jeden nebo více názvů domén, které jsou používané toodo firmy a že koncové uživatele využívat prostředky společnosti tooaccess. Vaše vlastní doména název tooAzure AD můžete přidat tak, aby hello název domény je známé tooyour uživatelů, jako například  *alice@contoso.com*  místo  *alice@contoso.onmicrosoft.com* . |
|Účet Azure AD | Jedná se o identity, které jsou vytvořené pomocí služby Azure AD nebo jiné cloudové službě Microsoftu, jako je například Office 365. Jsou uložené ve službě Azure AD a přístupné tooany hello organizace předplatných cloudové služby. |
|Správce předplatného Azure| Správce účtu Hello je hello osobě, která registraci aplikace nebo koupili hello předplatného Azure. Používají hello [centra účtů](https://account.windowsazure.com/Home/Index) tooperform různé úlohy správy, jako vytvářet odběry, zrušit předplatné, změnit hello fakturace pro předplatné nebo změnit hello Správce služeb. |
|Globální správce Azure AD | Azure AD globální Správci mají plný přístup funkce správy tooall Azure AD. ve výchozím nastavení stane Hello osoba, která automaticky přihlásí k odběru služby Microsoft cloudu globálního správce. Může mít více než jeden globální správce, ale jenom globální správci můžete přiřadit některé z [hello další role správců](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) toousers. |
|Účet Microsoft | Účty Microsoft (vytvořené vámi pro osobní použití) zadejte přístup orientovaný tooconsumer produkty společnosti Microsoft a cloudovým službám, jako je Outlook (Hotmail), OneDrive, Xbox LIVE nebo Office 365. Tyto identity jsou vytvořeny a uloženy v hello Microsoft systém identit uživatelů účtů Microsoft.|
|Pracovní nebo školní účty | Pracovní nebo školní účty (vystavený správce pro firmy nebo academic použití) poskytovat přístup k tooenterprise firemní úrovni cloudových služeb Microsoftu, jako je například Azure, Intune nebo Office 365.|


## <a name="concepts-toounderstand"></a>Koncepty toounderstand

Nyní když znáte hello základní identit Azure podmínky, musí se další informace o tyto koncepty Azure identity, které vám pomůžou provést informované Azure identity služby rozhodnutí.

|Koncept toounderstand |Popis|
|-----|-----|
|[Jak je předplatné Azure propojeno se službou Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) |Každé předplatné služby Azure má vztah důvěryhodnosti s Azure AD directory tooauthenticate uživatelů, služeb a zařízení. *Několik předplatných může důvěřovat directory hello stejnou službou Azure AD, ale předplatné bude důvěřovat pouze jeden adresář Azure AD*. Tento vztah důvěryhodnosti se liší od hello vztah, který má předplatné s další prostředky Azure (weby, databáze a tak dále), které jsou spíše podřízené prostředky předplatného. Pokud platnost předplatného vyprší, poté tooresources přístupu, které jsou přidružené k předplatnému hello než Azure AD také se zastaví. Adresář Azure AD hello však zůstane v Azure, tak, aby můžete přidružit jiné předplatné adresáře a pokračovat toomanage klientské prostředky.|
|[Jak Azure AD funguje licencování](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-get-started-azure-portal) | Při nákupu nebo aktivaci Enterprise Mobility Suite, Azure AD Premium nebo Azure AD Basic, adresáře je aktualizováno hello předplatného, včetně doby platnosti a předplacený licence. Jakmile hello předplatné je aktivní, hello služby můžete spravovat pomocí globální správce Azure AD a používá licencovaní uživatelé. Informace o vašem předplatném, včetně hello počet licencí, přiřazené nebo k dispozici, je k dispozici v hello portál Azure z hello **Azure Active Directory** > **licence** okno. To je také hello nejlepší místo toomanage vaše přiřazení licence.|
|[Řízení přístupu na základě rolí v hello portálu Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is)|Azure na základě rolí řízení přístupu (RBAC) pomáhá zajistit přesnou správu přístupu k prostředkům Azure. Příliš mnoho oprávnění můžete vystavit a účet tooattackers. Příliš málo oprávnění znamená, že zaměstnanci nelze práci efektivně. Pomocí RBAC, můžete udělit zaměstnanci hello přesný oprávnění potřebují na základě tři základní rolí, které se vztahují tooall skupiny prostředků: vlastník, Přispěvatel, čtečky. Můžete taky vytvořit zálohu too2 000 vlastní [vlastní role RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) toomeet konkrétní potřebuje. |
|[Hybridní identita](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect)|Hybridní identita se dosáhne integrací vaší místní Windows Server Active Directory (AD DS) s Azure AD pomocí [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect). To vám umožní tooprovide společnou identitu pro uživatele pro Office 365, Azure a místní aplikace nebo aplikace SaaS integrované s Azure AD. S hybridní identitou efektivně rozšíříte místní prostředí toohello cloud pro identit a přístupu.|

### <a name="hello-difference-between-windows-server-ad-ds-and-azure-ad"></a>Hello rozdíl mezi službami Windows Server AD DS a Azure AD
Azure Active Directory (Azure AD) a v místní službě Active Directory (Active Directory Domain Services nebo služby AD DS) jsou systémy, které ukládají data adresáře a spravovat komunikaci mezi uživateli a prostředky, včetně přihlášení uživatele, ověřování, a prohledávání adresáře.

Pokud jste již obeznámeni s místními systému Windows Server Active Directory Domain Services (AD DS), poprvé v systému Windows 2000 Server pak pravděpodobně pochopit hello základní koncepci služby identity. Je však také důležité toounderstand Azure AD není právě řadiče domény v cloudu hello. Jedná se o zcela nový způsob poskytování identity jako služby (IDaaS) v Azure, který vyžaduje zcela nový způsob přemýšlení toofully mohlo cloudových funkcí a vaši organizaci chránit před moderními hrozbami. 

Služba AD DS je role serveru v systému Windows Server, což znamená, že můžou být nasazené na fyzický nebo virtuální počítače. Je hierarchická struktura podle X.500. DNS se používá pro nalezením objektů, může být zpracoval pomocí protokolu LDAP, a hlavně použije Kerberos pro ověřování. Objekty zásad skupiny (GPO) a Active Directory umožňuje organizačních jednotek (OU) kromě toojoining počítače toohello domény a vztahy důvěryhodnosti se vytvoří mezi doménami.

IT chrání jejich zabezpečení hraniční let pomocí služby AD DS, ale moderní, bez hraniční podniky podpora potřebám identity pro zaměstnance, Zákazníci a partneři vyžadují nové rovině řízení. Azure AD je touto rovinou ovládací prvek identity. Zabezpečení překročil hello podniková brána firewall toohello cloudu, kde Azure AD chrání firemním prostředkům a přístup tím, že poskytuje jeden společnou identitu pro uživatele (místně nebo v cloudu hello). To dává toosecurely flexibilitu hello vaši uživatelé přístup aplikace hello tooget potřebují svou práci udělat z takřka všech zařízení. Ovládací prvky ochrany bezproblémové dat založených na riziko, zálohován možnosti strojové učení a podrobné sestavy, jsou k dispozici také zabezpečení dat společnosti tookeep potřebám IT.

Azure AD je více zákazníků veřejné adresářová služba, což znamená, že v rámci Azure AD můžete vytvořit klienta pro cloud servery a aplikace, jako je Office 365. Uživatelé a skupiny se vytvoří v ploché struktuře bez organizační jednotky nebo objekty zásad skupiny. Ověřování se provádí přes protokoly, například SAML, WS-Federation a OAuth. Je možné tooquery Azure AD, ale místo použití LDAP musí používat rozhraní REST API volat AD Graph API. Tyto všechny fungovat přes protokol HTTP a HTTPS.

### <a name="extend-office-365-management-and-security-capabilities"></a>Rozšířit možnosti správy a zabezpečení Office 365
Už používá Office 365? Vaše digitální transformace lze urychlit rozšířením integrované funkce Office 365 s Azure AD toosecure všechny prostředky, povolení zabezpečeného produktivitu pro všechny zaměstnance. Pokud používáte Azure AD, kromě tooOffice 365 možnosti, můžete zabezpečit portfolia celý aplikací se jednu identitu, která umožňuje jednotné přihlašování pro všechny aplikace. Můžete rozšířit vaše možnosti podmíněného přístupu na základě nejen na umístění stavu, ale uživatel, zařízení, aplikace a také riziko. Vícefaktorové ověřování (MFA) možnosti poskytují i další ochranu v případě potřeby. Můžete získat další dohledu uživatelská oprávnění a poskytovat přístup pro správu na vyžádání, za běhu. Vaši uživatelé budou zvýšit produktivitu a vytvářet lístky méně technickou podporu, díky schopnosti samoobslužné služby toohello Azure AD poskytuje jako resetovat zapomenutá hesla, požadavky na přístup aplikace a vytváření a Správa skupin.

> [!TIP]
> Chcete toolearn Další informace o používání správy identit Azure AD s Office 365? [Získat elektronickou knihu hello](https://info.microsoft.com/Extend-Office-365-security-with-EMS.html).

## <a name="microsoft-azure-identity-solutions"></a>Řešeních identity v Microsoft Azure

Microsoft Azure nabízí několik způsobů toomanage identity vašich uživatelů zda udržování plně místní, pouze v hello cloudu, nebo dokonce někde v rozmezí. Tyto možnosti patří: samoobslužné AD DS (DIY) v Azure, Azure Active Directory (Azure AD), hybridní identita a Azure AD Domain Services.

### <a name="do-it-yourself-diy-ad-ds"></a>Samoobslužné (DIY) AD DS
Pro společnosti, které potřebují jenom malé nároky v cloudu hello **samoobslužné (DIY) služby AD DS** v Azure lze použít. Tato volba podporuje mnoho scénářů služby Windows Server AD DS, které jsou vhodné pro nasazení jako virtuální počítače (VM) v Azure. Například můžete vytvořit virtuální počítač Azure jako řadič domény spuštěný ve vzdálených datovém centru, který je připojený toohello vzdálené sítě. Z tohoto místa hello virtuálních počítačů bude možné toosupport žádosti o ověření od vzdálených uživatelů a zlepšit výkon ověřování. Tato možnost je také vhodné jako relativně nízkonákladové substitute toootherwise nákladná po havárii obnovení lokality pomocí hostování malý počet řadičů domény a jedné virtuální sítě v Azure. Nakonec můžete potřebovat toodeploy aplikace v Azure, například SharePoint, které vyžaduje Windows Server AD DS, ale nemá žádné závislosti na hello do místní sítě a hello podnikové Windows Server Active Directory. V takovém případě můžete nasadit izolované doménové struktury na požadavky na Azure toomeet hello farmy služby SharePoint server. Je podporováno také toodeploy síťových aplikací, které vyžadují připojení toohello do místní sítě a hello místní služby Active Directory.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)
**Azure AD samostatné** je plně cloudových identit a přístupu správy jako řešení služby (IDaaS). Azure AD poskytuje výkonnou sadu funkcí toomanage uživatelů a skupin. Pomáhá zabezpečit přístup k místním tooon a cloudovým aplikacím, včetně webových služeb společnosti Microsoft jako je Office 365 a mnoha jiných společností než Microsoft software jako služba (SaaS) aplikace. Azure AD se dodává v tři edicích: volné, základní a Premium. Azure AD zvyšuje efektivitu organizace a rozšiřuje zabezpečení nad rámec hello hraniční brány firewall tooa nový ovládací prvek roviny chráněné pomocí Azure machine learning a další pokročilé funkce zabezpečení.

### <a name="hybrid-identity"></a>Hybridní identita
Místo výběr mezi místními a řešení cloudových identit, mnoho dopředného přemýšlení ředitelé informačních technologií a podnikům, které jste začali očekávání dlouhodobý směr své společnosti, jsou rozšíření místních adresářů toohello cloudu prostřednictvím **hybridní identita** řešení. S hybridní identitou získat identitu skutečně globální a řešení pro správu přístupu, která poskytuje zabezpečené a výkonné přístup uživatelům aplikace toohello potřebovat toodo svou práci.

> [!TIP]
> Další informace o toolearn jak ředitelé informačních technologií provedli Azure Active Directory centrální součástí strategie jejich IT stáhnout hello [ředitel IT 's Guide tooAzure služby Active Directory](https://aka.ms/AzureADCIOGuide).

### <a name="azure-ad-domain-services"></a>Azure AD Domain Services
**Azure AD Domain Services** poskytuje toouse možnost cloudové služby AD DS pro prosté řízení konfigurace virtuálního počítače Azure a způsob toomeet požadavky na místní identity pro sítě vývoj a testování aplikací. Azure AD Domain Services není určena toolift a posunutí místní tooAzure infrastruktury služby AD DS virtuální počítače spravované službou Azure AD Domain Services. Místo toho hello virtuálních počítačů Azure ve spravovaných domén musí být použité toosupport hello vývoj, testování a přesun místní aplikace, které vyžadují cloudu toohello metody ověřování služby AD DS.

## <a name="common-scenarios-and-recommendations"></a>Běžné scénáře a doporučení

Tady jsou některé běžné scénáře přístupu a identit s doporučeními jako toowhich možnost identit Azure může být nejvhodnější pro jednotlivé.

|Scénář identity| Doporučení|
|-----|-----|
|Moje organizace udělal velkých investic v místním systému Windows Server Active Directory, ale chceme tooextend identity toohello cloudu.| je řešení identit Azure Hello nejčastěji používá [hybridní identita](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). Pokud jste již provedli investice na místní služby AD DS, můžete snadno rozšířit identity toohello cloudu pomocí služby Azure AD Connect.|
|V cloudu hello se narodil firmy a máme žádné investice v řešení identity na místě.| [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) je nejlepší volbou hello pro firmy jenom pro cloud bez místní investice.|
|Potřebuji lightweight Azure požadavky virtuálního počítače konfigurace a řízení toomeet místní identity pro vývoj aplikací a testování.|[Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) je vhodné použít, potřebujete-li toouse služby AD DS pro prosté řízení konfigurace virtuálního počítače Azure nebo hledáte toodevelop nebo migrovat starší verze, podporou directory místní aplikace toohello cloudu.|  
|Potřebuji toosupport několik virtuálních počítačů v Azure, ale Moje společnost je stále výraznou investovali v místní službě Active Directory (AD DS).|Použití [DIY služby AD DS](https://msdn.microsoft.com/library/azure/jj156090.aspx) toouse virtuální počítače Azure, když se třeba toosupport několik virtuálních počítačů a máte velké služby AD DS investice do místní. |

## <a name="where-can-i-learn-more"></a>Kde můžete další informace?
Jsme spousta online toohelp skvělé prostředky, které zjistit všechno o Azure AD. Tady je seznam tooget skvělé články, které jste spustili:

* [Povolení adresáře pro správu hybridní službou Azure AD Connect](active-directory-aadconnect.md)
* [Další zabezpečení pro někdy připojené world](../multi-factor-authentication/multi-factor-authentication.md)
* [Automatizace zřizování uživatelů a jeho rušení tooSaaS aplikací s Azure Active Directory](active-directory-saas-app-provisioning.md)
* [Začínáme s generováním sestav Azure AD](active-directory-reporting-getting-started.md)
* [Spravovat hesla k odkudkoli.](active-directory-passwords.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Automatizace zřizování uživatelů a jeho rušení tooSaaS aplikací s Azure Active Directory](active-directory-saas-app-provisioning.md)
* [Jak tooprovide zabezpečený vzdálený přístup tooon místní aplikace](active-directory-application-proxy-get-started.md)
* [Správa přístupu tooresources pomocí skupin Azure Active Directory](active-directory-manage-groups.md)
* [Co je licencování služby Microsoft Azure Active Directory?](active-directory-licensing-what-is.md)
* [Jak může zjišťovat nedovolené cloudové aplikace, které se používají v rámci Moje organizace](active-directory-cloudappdiscovery-whatis.md)

## <a name="next-steps"></a>Další kroky

Teď, když znáte koncepty identit Azure a k dispozici tooyou hello možnosti, můžete použít následující prostředky tooget spuštění implementující hello možnost zvolená hello:

[Další informace o Azure hybridní řešení identit](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution)

[Další informace v prostředí Azure testování konceptu](https://aka.ms/aad-poc)

[Nasazení služby Azure AD v produkčním prostředí](https://aka.ms/aad-onboard)
