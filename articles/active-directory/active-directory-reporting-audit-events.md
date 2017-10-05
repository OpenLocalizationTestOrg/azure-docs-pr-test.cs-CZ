---
title: "Události sestavy auditování Azure Active Directory | Microsoft Docs"
description: "Auditované události, které jsou k dispozici pro zobrazení a stažení ze služby Azure Active Directory"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 307eedf7-05bc-448d-a84d-bead5a4c5770
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 1620d917acf5a2c36767b5b03750c405f3631ee2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-audit-report-events"></a>Události sestavy auditování služby Azure Active Directory
*Tato dokumentace je součástí [Příručky generování sestav v Azure Active Directory](active-directory-reporting-guide.md).*

Sestavy auditování Azure Active Directory pomáhá zákazníkům identifikovat privilegovaných akcí, které došlo k chybě v Azure Active Directory. Privilegované akce zahrnují změny zvýšení oprávnění (například role vytvoření nebo resetování hesla), změna konfigurace zásad (třeba zásady pro hesla) nebo změny konfigurace adresáře (například změny nastavení federace domén). Sestavy poskytují záznam auditu z názvu události objektu actor, který provedl akci, cílový prostředek vliv na změny a datum a čas (ve formátu UTC). Zákazníci mohou načíst seznam událostí auditu pro Azure Active Directory pomocí [portálu Azure](https://portal.azure.com/), jak je popsáno v [zobrazit protokoly auditu](active-directory-reporting-azure-portal.md).

## <a name="list-of-audit-report-events"></a>Seznam události sestavy auditování
<!--- audit event descriptions should be in the past tense --->

| Události | Popis události |
| --- | --- |
| **Události uživatele** | |
| Přidání uživatele |Přidat uživatele do adresáře. |
| Odstranění uživatele |Odstranit uživatele z adresáře. |
| Nastavení licenčních vlastností |Nastavte vlastnosti licencí pro uživatele v adresáři. |
| Resetovat heslo uživatele |Resetování hesla pro uživatele v adresáři. |
| Změnit heslo uživatele |Změnit heslo pro uživatele v adresáři. |
| Změna uživatelské licence |Změnit licence přiřazen k uživateli v adresáři. Chcete-li zobrazit, jaké licence byly aktualizovány, podívejte se [aktualizace uživatele](#update-user-attributes) vlastnosti níže |
| Aktualizovat uživatele |Aktualizovat uživatele v adresáři. [Viz níže](#update-user-attributes) pro atributy, které mohou být aktualizovány. |
| Platnost sady změnit heslo uživatele |Nastavte vlastnost, která určuje, že uživatel, chcete-li změnit své heslo při přihlášení. |
| Aktualizovat pověření uživatele |Uživatel změnit heslo |
| **Skupiny událostí** | |
| Přidat skupinu |Vytvořit skupinu v adresáři. |
| Skupiny aktualizací |Aktualizovat skupinu v adresáři. Chcete-li zobrazit, jaké vlastnosti skupiny byly aktualizovány, přečtěte [auditovat vlastnosti skupiny](#update-group-attributes) níže v části |
| Odstranění skupiny |Odstranit skupinu z adresáře. |
| CreateGroupSettings |Vytvořenou skupinu nastavení |
| UpdateGroupSettings |Aktualizovat nastavení skupiny. Chcete-li zobrazit nastavení skupiny, které byly aktualizovány, přečtěte [auditovat vlastnosti skupiny](#update-group-attributes) níže v části |
| DeleteGroupSettings |Odstraněné skupiny nastavení |
| SetGroupLicense |Nastavit skupinu licencí. |
| SetGroupManagedBy |Nastavit skupinu pro správu podle uživatele |
| AddGroupMember |Přidání člena do skupiny |
| RemoveGroupMember |Odebrání člena ze skupiny |
| AddGroupOwner |Vlastník přidaný do skupiny |
| RemoveGroupOwner |Odebrané vlastníka ze skupiny |
| **Události aplikace** | |
| Přidat instančního objektu |Přidat hlavní název služby do adresáře. |
| Odebrat instančního objektu |Objekt služby odebrat z adresáře. |
| Přidat hlavní pověření služby |Přidání pověření pro hlavní název služby. |
| Odeberte hlavní pověření služby |Odebrané přihlašovací údaje z instanční objekt. |
| Přidat položku delegování |Vytvoření [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) v adresáři. |
| Záznam delegování sady |Aktualizované [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) v adresáři. |
| Odeberte záznam delegování |Odstranit [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) v adresáři. |
| **Role události** | |
| Přidat člena role do Role |Přidat uživatele k roli adresáře. |
| Odebrat člen role z Role |Odebrat uživatele z role adresáře. |
| AddRoleDefinition |Přidání role definice. |
| UpdateRoleDefinition |Aktualizovat definice role. Chcete-li zobrazit nastavení role, které byly aktualizovány, přečtěte [auditovat vlastnosti definice Role](#update-role-definition-attributes) níže v části |
| DeleteRoleDefinition |Odstranit definici role. |
| AddRoleAssignmentToRoleDefinition |Přiřazení role přidané do definice role. |
| RemoveRoleAssignmentFromRoleDefinition |Přiřazení role odebrané z definice role. |
| AddRoleFromTemplate |Přidání role z šablony. |
| UpdateRole |Aktualizované role. |
| AddRoleScopeMemberToRole |Přidání oboru člena do role. |
| RemoveRoleScopedMemberFromRole |Odebrat člen v rámci oboru z role. |
| **Události zařízení (všechny nové události)** | |
| AddDevice |Přidání zařízení. |
| UpdateDevice |Aktualizované zařízení. Chcete-li zobrazit, jaké vlastnosti zařízení byly aktualizovány, přečtěte [vlastnosti zařízení Audited](#update-device-attributes) níže v části |
| DeleteDevice |Zařízení. |
| AddDeviceConfiguration |Konfigurace přidané zařízení. |
| UpdateDeviceConfiguration |Konfigurace aktualizované zařízení. Chcete-li zobrazit, jaké vlastnosti konfigurace zařízení byly aktualizovány, přečtěte [vlastnosti konfigurace zařízení Audited](#update-device-configuration-attributes) níže v části |
| DeleteDeviceConfiguration |Konfigurace zařízení. |
| AddRegisteredOwner |Přidání registrovaný vlastník zařízení. |
| AddRegisteredUsers |Registrovaní uživatelé přidat do zařízení. |
| RemoveRegisteredOwner |Registrovaný vlastník odeberte ze zařízení. |
| RemoveRegisteredUsers |Registrovaní uživatelé odeberte ze zařízení. |
| RemoveDeviceCredentials |Odeberte přihlašovací údaje zařízení. |
| **Události B2B** | |
| Batch pozve nahrát. |Správce odeslal soubor obsahující zasílání požadavků uživatelům partnera. |
| Pozve dávkové zpracování. |Soubor obsahující pozvánek uživatelům z partnerských byla zpracována. |
| Pozvěte externího uživatele. |Externí uživatel dostali pozvánku, k adresáři. |
| Uplatněte pozvání externího uživatele. |Externí uživatel má uplatněn svoje pozvánku k adresáři. |
| Přidáte externího uživatele do skupiny. |Externí uživatel má přiřazeno členství ve skupině v adresáři. |
| Externí uživatele přiřadíte k aplikaci. |Externí uživatel má přiřazeno přímý přístup k aplikaci. |
| Vytváření virální klienta. |Pozvánka se vytvořila nového klienta ve službě Azure AD. |
| Vytvoření virální uživatele. |Uživatel byl vytvořen v existujícího klienta ve službě Azure AD se pozvánku. |
| **Administrativních jednotek (všechny nové události)** | |
| AddAdministrativeUnit |Přidáte správce jednotku. |
| UpdateAdministrativeUnit |Aktualizujte správce jednotku. Chcete-li zobrazit, jaké vlastnosti pro správu jednotky byly aktualizovány, přečtěte [vlastnosti pro správu jednotky auditovat](#update-administrative-unit-attributes) níže v části |
| DeleteAdministrativeUnit |Odstraní správu jednotku. |
| AddMemberToAdministrativeUnit |Přidáte člena do správce jednotky. |
| RemoveMemberFromAdministrativeUnit |Odebrání člena z administrativní jednotky. |
| **Directory události** | |
| Přidat partnera společnosti |Přidat partnera k adresáři. |
| Odebrání partnera společnosti |Odebrat partnera z adresáře. |
| DemotePartner |Snížení úrovně partnera. |
| Přidání domény do společnosti |Přidání domény k adresáři. |
| Odebrat domény z společnosti |Odebrat domény z adresáře. |
| Aktualizace domény |Aktualizovat k doméně v adresáři. Chcete-li zobrazit, jaké vlastnosti domény byly aktualizovány, přečtěte [vlastnosti domény auditovat](#update-domain-attributes) níže v části |
| Ověřování v doméně sady |Změnit výchozí nastavení domény společnosti. |
| Nastavte kontaktní informace společnosti |Nastavit předvolby kontaktu společnosti úrovni. To zahrnuje e-mailové adresy pro marketing, jakož i technické oznámení o služeb Microsoft Online Services. |
| Nastavení federace v doméně |Aktualizace nastavení federace pro doménu. |
| Ověření domény |Ověřit k doméně v adresáři. |
| Ověření ověřené domény e-mailu |Ověření domény na adresář pomocí ověření e-mailu. |
| Nastavte příznak DirSyncEnabled na společnosti |Nastavte vlastnost, která umožňuje adresáře pro Azure AD Sync. |
| Nastavení zásad pro hesla |Nastavte omezení délky a znak pro hesla uživatele. |
| Nastavení informací o společnosti |Aktualizované informace o úrovni společnosti. Najdete v článku [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) rutiny prostředí PowerShell pro další podrobnosti. |
| SetCompanyAllowedDataLocation |Sada společnosti povoleno umístění dat. |
| SetCompanyDirSyncEnabled |Nastavte příznak DirSyncEnabled. |
| SetCompanyDirSyncFeature |Nastavte funkci nástroje DirSync. |
| SetCompanyInformation |Informace o sadě společnosti. |
| SetCompanyMultiNationalEnabled |Nastavit společnosti mezinárodní funkce povolena. |
| SetDirectoryFeatureOnTenant |Nastavit adresář funkce na klienta. |
| SetTenantLicenseProperties |Nastavit vlastnosti licenci klienta. |
| CreateCompanySettings |Vytvoření nastavení společnosti |
| UpdateCompanySettings |Aktualizujte nastavení společnosti. Chcete-li zobrazit, jaké vlastnosti společnosti byly aktualizovány, přečtěte [společnosti vlastnosti auditovat](#update-company-attributes) níže v části |
| DeleteCompanySettings |Odstranit nastavení společnosti |
| SetAccidentalDeletionThreshold |Nastavení prahové hodnoty náhodného odstranění. |
| SetRightsManagementProperties |Nastavit vlastnosti správy práv. |
| PurgeRightsManagementProperties |Vyprázdnit vlastnosti rights management. |
| UpdateExternalSecrets |Aktualizovat externí tajné klíče |
| **Události zásad (všechny nové události)** | |
| AddPolicy |Přidání zásad. |
| UpdatePolicy |Aktualizujte zásady. |
| Odstranit zásady |Odstraňte zásadu. |
| AddDefaultPolicyApplication |Přidáte zásadu k aplikaci. |
| AddDefaultPolicyServicePrincipal |Přidáte zásady instančního objektu. |
| RemoveDefaultPolicyApplication |Odeberte zásady z aplikace. |
| RemoveDefaultPolicyServicePrincipal |Odebrání zásad instanční objekt. |
| RemovePolicyCredentials |Odeberte zásady přihlašovací údaje. |

## <a name="audit-report-retention"></a>Uchování sestavy auditu

Nejnovější informace o uchování najdete v tématu [zásady uchování sestav Azure Active Directory](active-directory-reporting-retention.md).


Pro zákazníky ukládat svoje události auditu delší dobu uchování rozhraní API pro vytváření sestav lze pravidelně vyžádání události auditu do samostatné datové úložiště. V tématu [Začínáme s rozhraním API pro vytváření sestav](active-directory-reporting-api-getting-started.md) podrobnosti.

## <a name="properties-included-with-each-audit-event"></a>Vlastnosti, které jsou součástí jednotlivých událostí auditu
| Vlastnost | Popis |
| --- | --- |
| Datum a čas |Datum a čas výskytu události auditu |
| Objektu actor |Uživatel nebo objektu služby, která akci provedla |
| Akce |Akce, která byla provedena |
| cíl |Uživatel nebo objektu služby, která akci provedla na |

## <a name="update-user-attributes"></a>"Aktualizovat uživatele" atributy
Události auditu "Aktualizace uživatel" obsahuje další informace o atributy uživatele, které byly aktualizovány. Pro každý atribut je součástí předchozí hodnotu a nová hodnota.

| Atribut | Popis |
| --- | --- |
| accountEnabled |Uživatel může přihlásit. |
| AssignedLicense |Všechny licence, které byly přiřazeny uživateli. |
| AssignedPlan |Služba plány vyplývající z licencí přiřazených uživateli. |
| LicenseAssignmentDetail |Podrobnosti o uživateli přiřazené licence. Například pokud se jedná o na základě skupiny licencí, bude zahrnovat skupině udělit licenci. |
| Mobilní |Mobilní telefon uživatele. |
| OtherMail |Alternativní e-mailovou adresu uživatele. |
| otherMobile |Uživatele alternativního mobilního telefonu. |
| StrongAuthenticationMethod |Seznam metod ověření nakonfigurovaná uživatelem pro službu Multi-Factor Authentication, jako je například hlasový hovor, SMS nebo ověřovací kód z mobilní aplikace. |
| StrongAuthenticationRequirement |Pokud je vynutit službu Multi-Factor Authentication, povolit nebo zakázat pro tohoto uživatele. |
| StrongAuthenticationUserDetails |Telefonní číslo uživatele, alternativní telefonní číslo a e-mailová adresa použitá pro ověření služby Multi-Factor Authentication a heslo resetovat. |
| StrongAuthenticationPhoneAppDetail |Podrobnosti o forof telefonní aplikace zaregistrované k provedení 2FA přihlášení |
| telephoneNumber |Telefonní číslo uživatele. |
| AlternativeSecurityId |ID alternativní zabezpečení pro objekt. |
| CreationType |Způsob vytvoření uživatele (buď pozvánku nebo virální). |
| InviteTicket |Seznam pozvánku lístky pro uživatele. |
| InviteReplyUrl |Seznam adres URL pro odpověď při přijetí pozvánky. |
| InviteResources |Seznam prostředků, do kterých byla pozvat uživatele. |
| LastDirSyncTime |Čas poslední objekt byl aktualizován z důvodu synchronizuje se službou autoritativní adresáře (zákazníka, místní). |
| MSExchRemoteRecipientType |Přiřadí se k příjemce typy MSO. Odkazovat na https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx [MSO příjemce typy] pro příjemce typy |
| PreferredDataLocation |Upřednostňované umístění pro uživatele, skupiny, kontaktu, veřejné složky, nebo data na zařízení. |
| ProxyAddresses |Adresa, podle kterého se rozpozná příjemce objekt serveru Exchange Server v systému cizí e-mailu. |
| StsRefreshTokensValidFrom |Má u sebe aktualizovat informace o tokenu odvolání – všechny tokeny obnovení služby tokenů zabezpečení vydané před této doby jsou považovány za vypršela platnost. |
| UserPrincipalName |Názvu UPN, které se Internetu stylu přihlašovací jméno pro uživatele. |
| UserState |Stav uživatele čeká na schválení nebo PendingAcceptance/přijmout nebo PendingVerification. |
| UserStateChangedOn |Časové razítko poslední změny UserState. Použít k aktivaci pracovních životního cyklu. |
| UserType |Typ uživatele. Člen (0), Host (1), Viral(2). |

## <a name="update-group-attributes"></a>Atributy "Skupina aktualizací"
| Atribut | Popis |
| --- | --- |
| klasifikace |Klasifikace pro skupinu Unified (HBI, MBI atd.). |
| Popis |Čitelný slovní o objektu. |
| displayName |Zobrazovaný název pro objekt |
| dirSyncEnabled |Určuje, zda dochází k synchronizaci ze autoritativní adresáře (zákazníka, místní). |
| GroupLicenseAssignment |Přiřazení licencí skupiny |
| groupType |Typ skupiny Unified (0) |
| IsMembershipRuleLocked |Označuje, že vlastnost MembershipRule nastavena službou management samoobslužné služby skupiny a uživatelé nelze změnit. Platí pouze pro skupiny, kde GroupType obsahuje GroupType.DynamicMembership |
| IsPublic |Příznak, který označuje, pokud skupina je veřejného a privátního. |
| LastDirSyncTime |Čas poslední objekt byl aktualizován v důsledku synchronizuje se službou autoritativní directory (místní zákazníka). |
| Pošta |Primární e-mailová adresa |
| MailEnabled |Označuje, zda tato skupina má e-mailové funkce. |
| mailNickname |Přezdívka pro objekt adresáře adresu, obvykle část jeho předchozí název e-mailu "@" symbol. |
| MembershipRule |Řetězec vyjadřující kritéria službou management samoobslužné služby skupiny a určit, kteří členové by měly patřit do této skupiny. Další informace najdete v části IsMembershipRuleLocked. Platí pouze pro skupiny, kde GroupType obsahuje GroupType.DynamicMembership. |
| MembershipRuleProcessingState |Hodnotu výčtu definované službu správy samoobslužné služby skupiny definování stav členství zpracování pro tuto skupinu. Platí pouze pro skupiny, kde GroupType obsahuje GroupType.DynamicMembership. |
| ProxyAddresses |Adresa, podle kterého se rozpozná příjemce objekt serveru Exchange Server v systému cizí e-mailu. |
| RenewedDateTime |Pokud byla skupina naposledy obnoven záznam časové razítko. |
| securityEnabled |Určuje, zda členství ve skupině může ovlivnit autorizačních rozhodnutích. |
| WellKnownObject |Označuje objekt adresáře určení jako jeden z předem definované sadě. |

## <a name="update-device-attributes"></a>"Aktualizovat zařízení" atributy
| Atribut | Popis |
| --- | --- |
| accountEnabled |Určuje, zda může ověřit objekt zabezpečení. |
| CloudAccountEnabled |Určuje, zda může ověřit objekt zabezpečení. Služba InTune zapíše, když zařízení řídí se hlavním místní. |
| CloudDeviceOSType |Typ zařízení podle operačního systému například Windows RT, iOS. Pokud nastavíte pomocí cloudové služby (například Intune), tento atribut se změní na autoritativní v adresáři pro DeviceOSType. |
| CloudDeviceOSVersion |Verze operačního systému. Pokud nastavíte pomocí cloudové služby (například Intune), tento atribut se změní na autoritativní v adresáři pro DeviceOSVersion. |
| CloudDisplayName |Hodnota atributu displayName LDAP. Pokud nastavíte pomocí cloudové služby (například Intune), bude tento atribut v adresáři displayName autoritativní. |
| CloudCreated |Určuje, zda byl vytvořen objekt cloudové služby. |
| CompliantUntil |Do jaké doby je považován za kompatibilní zařízení. |
| DeviceMetadata |Vlastních metadat pro zařízení |
| DeviceObjectVersion |Tento atribut slouží k identifikaci verze schématu pro zařízení. |
| deviceOSType |Typ zařízení podle operačního systému například Windows RT, iOS. Služba správy light – zapsané službou registrace a má být aktualizován MDM služba pro správu nebo služby tokenů zabezpečení. |
| DeviceOSVersion |Verze operačního systému. Služba správy light – zapsané službou registrace a má být aktualizován MDM služba pro správu nebo služby tokenů zabezpečení. |
| DevicePhysicalIds |Vícehodnotový atribut určené k ukládání identifikátory fyzického zařízení. To může zahrnovat ID systému BIOS, čipu TPM kryptografických otisků, hardwaru konkrétní ID atd. |
| dirSyncEnabled |Označuje, zda autoritativní (zákazníkovi, místní) dochází k synchronizaci adresáře. |
| displayName |Zobrazovaný název pro objekt |
| IsCompliant |Tento atribut slouží ke správě mobilních zařízení stav správy zařízení. |
| ismanaged – |Tento atribut slouží k označení, že je zařízení spravované pomocí cloudové správy mobilních zařízení. |
| LastDirSyncTime |Čas poslední objekt byl aktualizován z důvodu synchronizuje se službou autoritativní directory (místní zákazníka). |

## <a name="update-device-configuration-attributes"></a>"Aktualizovat konfiguraci zařízení" atributy
| Atribut | Popis |
| --- | --- |
| MaximumRegistrationInactivityPeriod |Maximální počet dnů, po který zařízení může být neaktivní dříve, než je považován za pro odebrání. |
| RegistrationQuota |Zásady se používá k omezení číslo registrací zařízení, které jsou povoleny pro jednoho uživatele. |

## <a name="update-service-principal-configuration-attributes"></a>"Aktualizovat objektu konfigurace služby" atributy
| Atribut | Popis |
| --- | --- |
| accountEnabled |Určuje, zda může ověřit objekt zabezpečení. |
| AppPrincipalId |Externí, definované aplikací identitu pro objekt zabezpečení. |
| displayName |Zobrazovaný název pro objekt |
| ServicePrincipalName |Hlavní název služby, který obsahuje "název/autorita", kde název určuje hodnotu – třída aplikace a autority obsahuje alespoň název hostitele [: port] nebo "název" určující identifikátor pro objekt služby. |

## <a name="update-app-attributes"></a>"Aktualizace aplikace" atributy
| Atribut | Popis |
| --- | --- |
| AppAddress |Sada adresy (adresy URL pro přesměrování), které jsou přiřazeny k objektu služby. |
| appId |ID aplikace aplikace |
| AppIdentifierUri |URI aplikace, které identifikuje aplikaci.  Obvykle je adresa URL aplikace přístup. |
| AppLogoUrl |Adresa url pro obrázek loga aplikace uložené v název CDN. |
| AvailableToOtherTenants |Hodnota TRUE, aplikace je víceklientské aplikace (tj. můžete použít další klienty). |
| displayName |Zobrazovaný název pro název aplikace |
| Nároku |Seznam oprávnění aplikace. |
| ExternalUserAccountDelegationsAllowed |Příznak indikující, zda je důvěryhodný prostředků aplikace a můžete vytvořit záznamy delegování pro externí uživatelské účty. |
| GroupMembershipClaims |Členství ve skupině deklarací zásad. |
| PublicClient |Hodnota TRUE, pokud klient nemůže nadále tajný (tj.-důvěrné client OAuth2.0) |
| RecordConsentConditions |Typy souhlasu podmínek, podle definice smluvních podmínek: žádný (0), SilentConsentForPartnerManagedApp(1). Tato hodnota se zveřejní ve schématu rozhraní Graph API a může být pouze sadu či změnit podle správců klientů. |
| RequiredResourceAccess |Obsah XML hodnoty vlastnosti RequiredResourceAccess. |
| WebApp |V případě hodnoty true označuje, že tato aplikace je webovou aplikaci. |
| WwwHomepage |Primární webové stránky. |

## <a name="update-role-attributes"></a>"Aktualizovat roli" atributy
| Atribut | Popis |
| --- | --- |
| AppAddress |Sada adresy (adresy URL pro přesměrování), které jsou přiřazeny k objektu služby. |
| BelongsToFirstLoginObjectSet |V případě hodnoty true označuje, že tento objekt náleží do sady objektů vyžadovaného k povolení přihlášení první správce nového klienta. |
| Předdefinované |Určuje, zda systém vlastní doba života objektu. |
| Popis |Čitelný slovní o objektu. |
| displayName |Zobrazovaný název pro objekt |
| mailNickname |Přezdívka pro objekt adresáře adresu, obvykle část jeho předchozí název e-mailu "@" symbol. |
| RoleDisabled |Určuje, zda role třeba ji ignorovat pro účely kontroly přístupu. |
| RoleTemplateId |Identita šablony role. |
| ServiceInfo |Zřizování informace specifické pro služby, které mohou být spotřebovávána MOAC nebo jiné instance služby (o stejné nebo jiné služby typy). |
| TaskSetScopeReference |Identifikuje TaskSet a sadu obory přidružené k roli nebo šablony role. |
| ValidationError |Informace, které zveřejnil federované služby popisující bez přechodná, specifickou pro službu chyba týkající se všech vlastností či odkaz z objektu správce akce k vyřešení. |
| WellKnownObject |Označuje objekt adresáře určení jako jeden z předem definované sadě. |

## <a name="update-role-definition-attributes"></a>"Aktualizovat definice Role" atributy
| Atribut | Popis |
| --- | --- |
| AssignableScopes |Kolekce autorizace obory, které může být odkazováno při přiřazování tento RoleDefinition objekt zabezpečení. |
| displayName |Zobrazovaný název pro objekt |
| GrantedPermissions |Oprávnění udělená podle této RoleDefinition. |

## <a name="update-administrative-unit-attributes"></a>"Aktualizovat správní jednotku" atributy
| Atribut | Popis |
| --- | --- |
| Popis |Tato vlastnost je aktualizována při změně popis administrativní jednotky. |
| displayName |Tato vlastnost je aktualizována při změně názvu administrativní jednotky. |

## <a name="update-company-attributes"></a>Atributy "Společnost aktualizace"
| Atribut | Popis |
| --- | --- |
| AllowedDataLocation |Umístění, ve kterém jsou uživatelé společnosti povoleno zřízení. |
| AuthorizedServiceInstance |Názvy instancí služby, ke kterým může nasadit plánu. |
| dirSyncEnabled |Označuje, zda autoritativní (zákazníkovi, místní) dochází k synchronizaci adresáře. |
| DirSyncStatus |Značí, zda synchronizace adresáře objektů adresu v tomto kontextu klienta autoritativní (zákazníkovi, místní) directory; rozšíření vlastnosti DirSyncEnabled na objekty společnosti. |
| DirSyncFeatures |Bitový příznak ke sledování sadu funkcí pro povolení i Zakázaní dirsync pro klienta. |
| DirectoryFeatures |Funkce directory povoleno nebo zakázáno. |
| DirSyncConfiguration |Obsahuje všechny konfigurace nástroje DirSync specifické pro aktuální klienta. |
| displayName |Zobrazovaný název pro objekt |
| IsMnc |Logický příznak nastaven na "PRAVDA", je-li společnosti je povolena pro funkci mezinárodní společnosti. |
| ObjectSettings |Kolekce nastavení pro oboru objektu. |
| PartnerCommerceUrl |Adresa URL, na jeho web obchodu. |
| PartnerHelpUrl |Adresa URL do jeho lokality nápovědy. |
| PartnerSupportEmail |Adresa URL k jeho podporu e-mailu. |
| PartnerSupportTelephone |Adresa URL pro jeho podporu telefonu. |
| PartnerSupportUrl |Adresa URL pro jeho podporu lokality. |
| StrongAuthenticationDetails |Podrobnosti související s StrongAuthentication. |
| StrongAuthenticationPolicy |Silné ověřování zásad společnosti. |
| TechnicalNotificationMail |E-mailová adresa oznámení technické problémy týkající se společnosti. |
| telephoneNumber |Telefonní čísla v souladu s E.123 doporučení ITU. |
| TenantType |Typ klienta. Pokud tato hodnota není zadaná, klient je společnost. Jinak, možné hodnoty jsou: MicrosoftSupport (0), SyndicatePartner (1), BreadthPartner (2) BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5). |
| VerifiedDomain |Sada názvů domén DNS vázána na společnosti. |

## <a name="update-domain-attributes"></a>"Aktualizovat domény" atributy
| Atribut | Popis |
| --- | --- |
| Možnosti |Bitové příznaky popisující možnosti domény, pokud existuje. |
| Výchozí |Určuje, zda domény je výchozí hodnota. například výchozí UserPrincipalName přípona při správce vytvoří nového uživatele v MOAC. |
| Počáteční |Určuje, zda je doména počáteční domény pro společnosti, jako přidělené OCP. Počáteční doména je jedinečné subdomény domény Microsoft Online; e.g.contoso3.microsoftonline.com. |
| LiveType |Typ odpovídající Windows Live oboru názvů, pokud existuje. |
| Name (Název) |Identifikátor pro koncový bod. |
| PasswordNotificationWindowDays |Počet dnů před vypršením platnosti hesla uživatele, je upozornění. |
| PasswordValidityPeriodDays |Počet dnů, po který je vhodný pro heslo, než je nutné změnit. |

Záznamy auditu jsou požadovaný ovládací prvek pro mnoho předpisy. Pro zákazníky používající Azure Active Directory Audit sestavy podle jejich předpisy se doporučuje, aby zákazník odeslat kopii Toto téma nápovědy s kopii sestavy exportovaný auditu zákazníka vysvětlení sestava obsahuje podrobnosti o. Pokud auditor chtěli pochopit předpisy, které Azure aktuálně splňuje, přímé auditor k [stránky dodržování předpisů](https://azure.microsoft.com/support/trust-center/compliance/) z Microsoft Azure Trust Center.

