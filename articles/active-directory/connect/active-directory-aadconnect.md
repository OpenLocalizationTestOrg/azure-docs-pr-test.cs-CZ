---
title: "Připojení Active Directory s Azure Active Directory | Dokumentace Microsoftu"
description: "Azure AD Connect integruje vaše místní adresáře do služby Azure Active Directory. To vám umožní tooprovide společnou identitu pro aplikace Office 365, Azure a SaaS integrované s Azure AD."
keywords: "Úvod tooAzure AD Connect, Azure AD Connect přehled, co je Azure AD Connect, instalace služby active directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e49f2af4b67e9ed3ad093888541da7c82af0e052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-on-premises-directories-with-azure-active-directory"></a>Integrace místních adresářů do služby Azure Active Directory
Azure AD Connect integruje vaše místní adresáře do služby Azure Active Directory. To vám umožní tooprovide společnou identitu pro uživatele pro aplikace Office 365, Azure a SaaS integrované s Azure AD. Toto téma vás provede hello plánování, nasazení a kroky operace. Jedná se o kolekci odkazů toohello témata související toothis oblasti.

> [!IMPORTANT]
> [Azure AD Connect je nejlepší způsob, jak tooconnect hello vašeho místního adresáře s Azure AD a Office 365. Toto je tooupgrade tooAzure nejvhodnější doba, AD Connect z Windows Azure Active Directory Sync (DirSync) nebo Azure AD Sync tyto nástroje jsou nyní zastaralé a bude dosáhnout konce podporu na 13. dubna 2017.](active-directory-aadconnect-dirsync-deprecated.md)
> 
> 

![Co je služba Azure AD Connect](media/active-directory-aadconnect/arch.png)

## <a name="why-use-azure-ad-connect"></a>Proč používat Azure AD Connect
Integrace místních adresářů se službou Azure AD zvyšuje produktivitu uživatelů tím, že jim poskytuje společnou identitu pro přístup ke cloudovým i místním prostředkům. Uživatelé a organizace můžete využít výhod hello následující:

* Uživatelé mohou používat jednou identitou tooaccess místními aplikacemi a cloudových služeb, jako je například Office 365.
* Nástroj tooprovide jednotné prostředí snadné nasazení pro synchronizaci a přihlašování.
* Poskytuje hello nejnovější schopnosti pro vaše scénáře. Azure AD Connect nahrazuje starší verze nástrojů pro integraci identity, jako jsou například DirSync nebo Azure AD Sync. Další informace najdete v článku o [orovnání nástrojů pro integraci adresáře hybridní identity](../active-directory-hybrid-identity-design-considerations-tools-comparison.md).

### <a name="how-azure-ad-connect-works"></a>Jak Azure AD Connect funguje
Azure Active Directory Connect se skládá z tři hlavní komponenty: hello synchronizační služby, volitelná komponenta Active Directory Federation Services hello a hello monitorovací komponenta nazvaná [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md) .

<center>![Sada komponent Azure AD Connect](./media/active-directory-aadconnect-how-it-works/AADConnectStack2.png)
</center>

* Synchronizace – tato komponenta odpovídá za vytváření uživatelů, skupin a dalších objektů. Je také zajišťuje, aby se, že informace o identitě místních uživatelů a skupin shodovaly hello cloudu.
* Služby AD FS - federace je volitelná součást Azure AD Connect a může být použité tooconfigure hybridní prostředí pomocí místní infrastruktury služby AD FS. Tímto lze podle organizace tooaddress komplexních nasazení, jako je například připojení k doméně jednotné přihlašování, vynucení přihlášení zásady služby Active Directory a čipové karty nebo 3. stran vícefaktorového ověřování.
* Monitorování stavu – Azure AD Connect Health může poskytovat robustní monitorování a poskytují centrální umístění na portálu Azure tooview hello této aktivity. Další informace najdete v článku [Azure Active Directory Connect Health](../connect-health/active-directory-aadconnect-health.md).

## <a name="install-azure-ad-connect"></a>Instalace služby Azure AD Connect
Můžete najít hello stažení pro Azure AD Connect na [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).

| Řešení | Scénář |
| --- | --- |
| Ještě než začnete – [hardware a požadavky](active-directory-aadconnect-prerequisites.md) |<li>Kroky toocomplete před zahájením tooinstall Azure AD Connect.</li> |
| [Expresní nastavení](active-directory-aadconnect-get-started-express.md) |<li>Pokud máte jednu doménovou strukturu AD poté, to se hello doporučuje toouse možnost.</li> <li>Uživatel přihlásit pomocí hello stejné heslo pomocí synchronizace hesel.</li> |
| [Vlastní nastavení](active-directory-aadconnect-get-started-custom.md) |<li>Toto nastavení použijte, pokud máte více doménových struktur. Podporuje mnoho místních [topologií](active-directory-aadconnect-topologies.md).</li> <li>Upravte možnost přihlašování, jako například službu AD FS pro federaci nebo použití zprostředkovatele identity od jiného výrobce.</li> <li>Přizpůsobte funkce synchronizace, jako je například filtrování nebo zpětný zápis.</li> |
| [Upgrade z nástroje DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Používá se, pokud již máte existující server DirSync.</li> |
| [Upgrade z Azure AD Sync nebo z Azure AD Connect](active-directory-aadconnect-upgrade-previous-version.md) |<li>Můžete si vybrat z několika různých metod.</li> |

[Po instalaci](active-directory-aadconnect-whats-next.md) ověřte je funguje podle očekávání a přiřadit licence toohello uživatele.

### <a name="next-steps-tooinstall-azure-ad-connect"></a>Další kroky tooInstall Azure AD Connect
|Téma |Odkaz|  
| --- | --- |
|Stažení služby Azure AD Connect | [Stažení služby Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771)|
|Instalace s expresním nastavením | [Expresní instalace služby Azure AD Connect](./active-directory-aadconnect-get-started-express.md)|
|Instalace s vlastním nastavením | [Vlastní instalace služby Azure AD Connect](./active-directory-aadconnect-get-started-custom.md)|
|Upgrade z nástroje DirSync | [Upgrade ze synchronizačního nástroje služby Azure AD (DirSync)](./active-directory-aadconnect-dirsync-upgrade-get-started.md)|
|Po instalaci | [Ověření instalace hello a přiřazení licencí](active-directory-aadconnect-whats-next.md)|

### <a name="learn-more-about-install-azure-ad-connect"></a>Další informace o instalaci Azure AD Connect
Chcete taky tooprepare pro [provozní](active-directory-aadconnectsync-operations.md) otázky. Můžete chtít toohave úsporný režim s připojením serveru, můžete snadno může převzít funkci v Pokud je [po havárii](active-directory-aadconnectsync-operations.md#disaster-recovery). Pokud máte v plánu změny časté konfigurace toomake, měli byste pro [pracovním režimu](active-directory-aadconnectsync-operations.md#staging-mode) serveru.

|Téma |Odkaz|  
| --- | --- |
|Podporované topologie | [Topologie pro Azure AD Connect](active-directory-aadconnect-topologies.md)|
|Koncepty návrhu | [Koncepty návrhu Azure AD Connect](active-directory-aadconnect-design-concepts.md)|
|Účty použité k instalaci | [Další informace o účtech a oprávněních služby Azure AD Connect](./active-directory-aadconnect-accounts-permissions.md)|
|Provozní plánování | [Synchronizace Azure AD Connect: Provozní úlohy a požadavky](active-directory-aadconnectsync-operations.md)|
|Možnosti přihlášení uživatele | [Možnosti přihlášení uživatele Azure AD Connect](active-directory-aadconnect-user-signin.md)|

## <a name="configure-sync-features"></a>Konfigurace synchronizačních funkcí
Azure AD Connect obsahuje několik funkcí, které můžete volitelně zapnout nebo které jsou ve výchozím nastavení povolené. Některé funkce mohou v rámci určitých scénářů a topologií vyžadovat další konfiguraci.

[Filtrování](active-directory-aadconnectsync-configure-filtering.md) se používá, pokud chcete, aby toolimit, jaké objekty jsou synchronizovány tooAzure AD. Ve výchozím nastavení jsou synchronizováni všichni uživatelé, kontakty, skupiny a počítače s Windows 10. Můžete změnit hello filtrování podle domén, organizačních jednotek nebo atributů.

[Synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md) synchronizuje hodnoty hash hesla hello ve službě Active Directory tooAzure AD. Hello koncového uživatele můžete použít hello stejné heslo místně a v cloudu hello ale pouze ho spravovat na jednom místě. Vzhledem k tomu, že jako autorita hello používá služby Active Directory v místě, můžete také použít vlastní zásady hesel.

[Zpětný zápis hesla](../active-directory-passwords-getting-started.md) povolit vaší toochange uživatelů a resetování hesel v cloudu hello a mít vaše místní zásady hesel použít.

[Zpětný zápis zařízení](active-directory-aadconnect-feature-device-writeback.md) vám umožní zařízení registrovaná v toobe Azure AD zapisovat zpátky tooon místní službě Active Directory, takže ho můžete použít pro podmíněný přístup.

Hello [prevence náhodného odstranění](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) funkce je ve výchozím nastavení zapnutá a chrání cloudový adresář před příliš mnoha odstraněními v hello stejnou dobu. Na jedno spuštění implicitně povoluje 500 odstranění. Toto nastavení se dá změnit v závislosti na velikosti vaší organizace.

[Automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) je povolená ve výchozím nastavení pro expresní nastavení instalace a zajistí, Azure AD Connect bude vždy až toodate s nejnovější verzí hello.

### <a name="next-steps-tooconfigure-sync-features"></a>Další kroky tooconfigure synchronizace funkce
|Téma |Odkaz|  
| --- | --- |
|Konfigurace filtrování | [Synchronizace Azure AD Connect: Konfigurace filtrování](active-directory-aadconnectsync-configure-filtering.md)|
|Synchronizace hesel | [Synchronizace Azure AD Connect: Implementace synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md)|
|Zpětný zápis hesla | [Začínáme se správou hesel](../active-directory-passwords-getting-started.md)|
|Zpětný zápis zařízení | [Povolení zpětného zápisu zařízení v Azure AD Connect](active-directory-aadconnect-feature-device-writeback.md)|
|Prevence náhodného odstranění | [Synchronizace Azure AD Connect: Prevence náhodného odstranění](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)|
|Automatický upgrade | [Azure AD Connect: Automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md)|

## <a name="customize-azure-ad-connect-sync"></a>Přizpůsobení synchronizace služby Azure AD Connect
Synchronizace Azure AD Connect se dodává s výchozí konfigurací, která je určený toowork pro většinu zákazníků a většinu topologií. Vždycky ale dochází situacích, kdy výchozí konfigurace hello nefunguje a musí být upravena. Je podporované toomake změny popsáno, jak v této části a související témata.

Pokud jste ještě nepracovali s topologií synchronizace předtím, než má toostart toounderstand hello základy a hello termínů používaných, jak je popsáno v hello [technické koncepty](active-directory-aadconnectsync-technical-concepts.md). Azure AD Connect je hello vývoj MIIS2003, ILM2007 a FIM2010. Přestože některé věci zůstávají stejné, došlo také k mnoha změnám.

Hello [výchozí konfigurace](active-directory-aadconnectsync-understanding-default-configuration.md) předpokládá v konfiguraci hello může být více než jedné doménové struktuře. V těchto topologiích může být objekt uživatele reprezentován v jiné doménové struktuře jako kontakt. Hello uživatel také může mít poštovní schránku propojenou v jiné doménové struktuře prostředků. chování Hello hello výchozí konfigurace je popsaná v [uživatelů a kontaktů](active-directory-aadconnectsync-understanding-users-and-contacts.md).

model konfigurace Hello v synchronizaci se označuje jako [deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md). Hello pokročilé toky atributů využívají [funkce](active-directory-aadconnectsync-functions-reference.md) tooexpress atribut transformace. Můžete zobrazit a prozkoumat celou konfiguraci hello pomocí nástrojů, který se dodává s Azure AD Connect. Pokud budete potřebovat změny konfigurace toomake, ujistěte se, postupujte hello [osvědčené postupy](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) tak, aby byl snadno tooadopt nové verze.

### <a name="next-steps-toocustomize-azure-ad-connect-sync"></a>Další kroky toocustomize Azure AD Connect sync
|Téma |Odkaz|  
| --- | --- |
|Všechny články o synchronizaci služby Azure AD Connect | [Synchronizace služby Azure AD Connect](active-directory-aadconnectsync-whatis.md)|
|Technické koncepty | [Synchronizace služby Azure AD Connect: Technické koncepty](active-directory-aadconnectsync-technical-concepts.md)|
|Principy hello výchozí konfigurace | [Synchronizace Azure AD Connect: Principy hello výchozí konfigurace](active-directory-aadconnectsync-understanding-default-configuration.md)|
|Principy uživatelů a kontaktů | [Synchronizace služby Azure AD Connect: Principy uživatelů a kontaktů](active-directory-aadconnectsync-understanding-users-and-contacts.md)|
|Deklarativní zřizování | [Synchronizace služby Azure AD Connect: Principy výrazů deklarativního zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)|
|Změna hello výchozí konfigurace | [Osvědčené postupy pro změnu výchozí konfigurace hello](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)|

## <a name="configure-federation-features"></a>Konfigurace funkcí federace
Služba AD FS může být nakonfigurované toosupport [více domén](active-directory-aadconnect-multiple-domains.md). Například můžete mít více hlavních domén pro federaci musíte toouse.

Pokud váš server služby AD FS nebyl nakonfigurovaný tooautomatically aktualizace certifikátů z Azure AD nebo pokud používáte řešení než AD FS, pak budete upozorněni když máte příliš[aktualizovat certifikáty](active-directory-aadconnect-o365-certs.md).

### <a name="next-steps-tooconfigure-federation-features"></a>Další kroky tooconfigure federation funkce
|Téma |Odkaz|  
| --- | --- |
|Všechny články o službě AD FS | [Azure AD Connect a federace](active-directory-aadconnectfed-whatis.md)|
|Konfigurace služby ADFS se subdoménami | [Podpora více domén pro federaci s Azure AD](active-directory-aadconnect-multiple-domains.md)|
|Správa farmy služby AD FS | [Správa služby AD FS a vlastní nastavení se službou Azure AD Connect](active-directory-aadconnect-federation-management.md)|
|Ruční aktualizace federačních certifikátů | [Obnovení federačních certifikátů pro Office 365 a Azure AD](active-directory-aadconnect-o365-certs.md)|

## <a name="more-information-and-references"></a>Další informace a odkazy
|Téma |Odkaz|  
| --- | --- |
|Historie verzí | [Historie verzí](active-directory-aadconnect-version-history.md)|
|Porovnání DirSync, Azure ADSync a Azure AD Connect | [Porovnání nástrojů pro integraci adresářů](../active-directory-hybrid-identity-design-considerations-tools-comparison.md)|
|Seznam kompatibility pro Azure AD bez služby AD FS | [Seznam kompatibilit pro federaci Azure AD](active-directory-aadconnect-federation-compatibility.md)|
|Konfigurace zprostředkovatele identity SAML 2.0|[Použití zprostředkovatele identity (IdP) SAML 2.0 pro jednotné přihlašování](active-directory-aadconnect-federation-saml-idp.md)|
|Synchronizované atributy | [Synchronizované atributy](active-directory-aadconnectsync-attributes-synchronized.md)|
|Monitorování pomocí služby Azure AD Connect Health | [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md)|
|Nejčastější dotazy | [Azure AD Connect – nejčastější dotazy](active-directory-aadconnect-faq.md)|

**Další zdroje**

Prezentace ignite 2015 na rozšíření vašeho místního adresáře toohello cloudu.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3862/player]
> 
> 

