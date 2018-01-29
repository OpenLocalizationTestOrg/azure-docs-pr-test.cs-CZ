# Přehled
## [Co je služba Azure AD Connect?](active-directory-aadconnect.md)
## [Co je Azure AD Connect Sync?](active-directory-aadconnectsync-whatis.md)
### [Uživatelé a kontakty](active-directory-aadconnectsync-understanding-users-and-contacts.md)
### [Architektura](active-directory-aadconnectsync-understanding-architecture.md)
### [Deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md)
#### [Výrazy deklarativního zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
### [Výchozí konfigurace](active-directory-aadconnectsync-understanding-default-configuration.md)
## [Co je Azure AD Connect a federace?](active-directory-aadconnectfed-whatis.md)


# Začínáme
## [Požadavky](active-directory-aadconnect-prerequisites.md)
## [Instalace služby Azure AD Connect](active-directory-aadconnect-select-installation.md)
### [Expresní nastavení](active-directory-aadconnect-get-started-express.md)
### [Vlastní nastavení](active-directory-aadconnect-get-started-custom.md)
### [Upgrade z nástroje DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md)
### [Upgrade z předchozí verze](active-directory-aadconnect-upgrade-previous-version.md)
### [Instalace s využitím existující databáze ADSync](active-directory-aadconnect-existing-database.md)

# Postup
## Plánování a návrh
### [Koncepty návrhu](active-directory-aadconnect-design-concepts.md)
### [Topologie pro Azure AD Connect](active-directory-aadconnect-topologies.md)
### [Služba AD FS (Active Directory Federation Service) v Azure](active-directory-aadconnect-azure-adfs.md)
### [Speciální aspekty pro instance](active-directory-aadconnect-instances.md)
### [Pokud už máte Azure AD](active-directory-aadconnect-existing-tenant.md)
## [Správa služby Azure AD Connect](active-directory-aadconnect-whats-next.md)
### [Obnovení certifikátů pro O365 a Azure AD](active-directory-aadconnect-o365-certs.md)
### [Aktualizace certifikátu SSL pro farmu Active Directory Federation Services (AD FS)](active-directory-aadconnectfed-ssl-update.md)
### [Povolení zpětného zápisu zařízení](active-directory-aadconnect-feature-device-writeback.md)
### [Možnosti přihlášení uživatele](active-directory-aadconnect-user-signin.md)
#### [Bezproblémové jednotné přihlašování](active-directory-aadconnect-sso.md)
##### [Rychlý start](active-directory-aadconnect-sso-quick-start.md)
##### [Jak to funguje?](active-directory-aadconnect-sso-how-it-works.md)
##### [Nejčastější dotazy](active-directory-aadconnect-sso-faq.md)
##### [Řešení problémů](active-directory-aadconnect-troubleshoot-sso.md)
#### [Předávací ověřování](active-directory-aadconnect-pass-through-authentication.md)
##### [Rychlý start](active-directory-aadconnect-pass-through-authentication-quick-start.md)
##### [Aktuální omezení](active-directory-aadconnect-pass-through-authentication-current-limitations.md)
##### [Jak to funguje?](active-directory-aadconnect-pass-through-authentication-how-it-works.md)
##### [Upgrade agentů Preview](active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md)
##### [Inteligentní uzamčení](active-directory-aadconnect-pass-through-authentication-smart-lockout.md)
##### [Nejčastější dotazy](active-directory-aadconnect-pass-through-authentication-faq.md)
##### [Řešení problémů](active-directory-aadconnect-troubleshoot-pass-through-authentication.md)
##### [Podrobné informace o zabezpečení](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md)
### [Podpora více domén pro federaci](active-directory-aadconnect-multiple-domains.md)
### [Automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md)
### [Použití zprostředkovatele identity (IdP) SAML 2.0 pro Jednotné přihlašování](active-directory-aadconnect-federation-saml-idp.md)



## Správa synchronizace Azure AD Connect
### [Prevence náhodného odstranění](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
### [Synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md)
### [Účet služby Azure AD](active-directory-aadconnectsync-howto-azureadaccount.md)
### [Průvodce instalací](active-directory-aadconnectsync-installation-wizard.md)
### [Změna výchozí konfigurace](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)
### [Konfigurace filtrování](active-directory-aadconnectsync-configure-filtering.md)
### [Scheduler](active-directory-aadconnectsync-feature-scheduler.md)
### [Rozšíření adresáře](active-directory-aadconnectsync-feature-directory-extensions.md)

### [Změna hesla účtu služby Azure AD Sync](active-directory-aadconnectsync-change-serviceacct-pass.md)
### [Změna hesla účtu služby AD DS](active-directory-aadconnectsync-change-addsacct-pass.md)
### [Povolení odpadkového koše AD](active-directory-aadconnectsync-recycle-bin.md)

### [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md)
#### [Provoz](active-directory-aadconnectsync-service-manager-ui-operations.md)
#### [Konektory](active-directory-aadconnectsync-service-manager-ui-connectors.md)
#### [Návrhář Metaverse](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)
#### [Vyhledávání Metaverse](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)


## Správa federačních služeb
### [Správa a přizpůsobení](active-directory-aadconnect-federation-management.md)
### [Federování několika instancí Azure AD s jednou instancí AD FS](active-directory-aadconnectfed-single-adfs-multitenant-federation.md)


## Řešení potíží
### [Připojení](active-directory-aadconnect-troubleshoot-connectivity.md)
### [Chyby při synchronizaci](active-directory-aadconnect-troubleshoot-sync-errors.md)
### [Objekt není synchronizovaný](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)
### [Synchronizace hesel](active-directory-aadconnectsync-troubleshoot-password-synchronization.md)
### [Chyba LargeObject způsobená certifikátem userCertificate](active-directory-aadconnectsync-largeobjecterror-usercertificate.md)
### [Jak provést obnovení při dosažení 10GB limitu pro LocalDB](active-directory-aadconnect-recover-from-localdb-10gb-limit.md)

# Referenční informace
## [Ukázky kódu](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory)
## [Synchronizace identit a odolnost duplicitních atributů](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
## [Porty a protokoly, které vyžaduje hybridní identita](active-directory-aadconnect-ports.md)
## [Funkce ve verzi Preview](active-directory-aadconnect-feature-preview.md)
## [Historie verzí](active-directory-aadconnect-version-history.md)
## [Účty a oprávnění](active-directory-aadconnect-accounts-permissions.md)

## Synchronizace služby Azure AD Connect
### [Atributy synchronizované se službou Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md)
### [Historie vydaných verzí konektoru](active-directory-aadconnectsync-connector-version-history.md)
### [Reference k funkcím](active-directory-aadconnectsync-functions-reference.md)
### [Provozní úlohy a důležité informace](active-directory-aadconnectsync-operations.md)
### [Seznam kompatibilit pro federaci Azure AD](active-directory-aadconnect-federation-compatibility.md)
### [Technické koncepce](active-directory-aadconnectsync-technical-concepts.md)
### [Funkce služby](active-directory-aadconnectsyncservice-features.md)




# Související
## [Monitorování místní infrastruktury identit a synchronizačních služeb v cloudu](../connect-health/active-directory-aadconnect-health.md)
## [Příručka návrhu hybridní identity](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/)


# Zdroje a prostředky
## [Plány Azure do budoucna](https://azure.microsoft.com/roadmap/?category=security-identity)
##[Azure AD Connect – nejčastější dotazy](active-directory-aadconnect-faq.md)
##[Zastarání DirSync](active-directory-aadconnect-dirsync-deprecated.md)
## [ Cenová kalkulačka](https://azure.microsoft.com/pricing/calculator/)

