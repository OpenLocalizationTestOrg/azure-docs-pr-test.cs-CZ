---
title: "události sestavy auditování služby Active Directory aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: 4a84cce2be56bde006164abf10ad1e72ca6e99bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-audit-report-events"></a>Události sestavy auditování služby Azure Active Directory
*Tato dokumentace je součástí hello [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).*

Hello Azure Active Directory auditu sestava pomáhá zákazníkům identifikovat privilegovaných akcí, které došlo k chybě v Azure Active Directory. Privilegované akce zahrnují změny zvýšení oprávnění (například role vytvoření nebo resetování hesla), změna konfigurace zásad (třeba zásady pro hesla) nebo změny toodirectory konfigurace (například změny toodomain federation nastavení). Hello sestavy poskytují hello záznam auditu pro název události hello, objektu actor hello, který provedl hello akce, cílový prostředek hello vliv hello změnu a hello datum a čas (ve formátu UTC). Je možné tooretrieve hello seznam událostí auditu pro Azure Active Directory prostřednictvím hello zákazníků [portálu Azure](https://portal.azure.com/), jak je popsáno v [zobrazit protokoly auditu](active-directory-reporting-azure-portal.md).

## <a name="list-of-audit-report-events"></a>Seznam události sestavy auditování
<!--- audit event descriptions should be in hello past tense --->

| Události | Popis události |
| --- | --- |
| **Události uživatele** | |
| Přidání uživatele |Přidat uživatele toohello adresář. |
| Odstranění uživatele |Odstranit uživatele z adresáře hello. |
| Nastavení licenčních vlastností |Nastavit vlastnosti hello licencí pro uživatele v adresáři hello. |
| Resetovat heslo uživatele |Resetovat hello heslo pro uživatele v adresáři hello. |
| Změnit heslo uživatele |Změnit hello heslo pro uživatele v adresáři hello. |
| Změna uživatelské licence |Změnit přiřazenou tooa uživatele v adresáři hello hello licenci. Jaké licence byly aktualizovány, vyhledejte v hello toosee [aktualizace uživatele](#update-user-attributes) níže uvedené vlastnosti |
| Aktualizovat uživatele |Aktualizovat uživatele v adresáři hello. [Viz níže](#update-user-attributes) pro atributy, které mohou být aktualizovány. |
| Platnost sady změnit heslo uživatele |Nastavte své heslo hello vlastnost, která vynutí toochange uživatele na přihlášení. |
| Aktualizovat pověření uživatele |Změněné hello heslo uživatele |
| **Skupiny událostí** | |
| Přidat skupinu |Vytvořit skupinu v adresáři hello. |
| Skupiny aktualizací |Aktualizovat skupinu v adresáři hello. toosee se aktualizovaly vlastnosti jaké skupiny, najdete v příliš[auditovat vlastnosti skupiny](#update-group-attributes) v následující části hello |
| Odstranění skupiny |Odstranit skupinu z adresáře hello. |
| CreateGroupSettings |Vytvořenou skupinu nastavení |
| UpdateGroupSettings |Aktualizovat nastavení skupiny. toosee skupiny nastavení, které byly aktualizovány, najdete v příliš[auditovat vlastnosti skupiny](#update-group-attributes) v následující části hello |
| DeleteGroupSettings |Odstraněné skupiny nastavení |
| SetGroupLicense |Nastavit skupinu licencí. |
| SetGroupManagedBy |Nastavit skupinu toobe spravuje uživatele |
| AddGroupMember |Přidaný člen toogroup |
| RemoveGroupMember |Odebrání člena ze skupiny |
| AddGroupOwner |Přidání vlastníka toogroup |
| RemoveGroupOwner |Odebrané vlastníka ze skupiny |
| **Události aplikace** | |
| Přidat instančního objektu |Přidat hlavní toohello adresář služby. |
| Odebrat instančního objektu |Objekt služby odebrat z adresáře hello. |
| Přidat hlavní pověření služby |Přidání pověření tooa instanční objekt. |
| Odeberte hlavní pověření služby |Odebrané přihlašovací údaje z instanční objekt. |
| Přidat položku delegování |Vytvoření [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) v adresáři hello. |
| Záznam delegování sady |Aktualizované [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) v adresáři hello. |
| Odeberte záznam delegování |Odstranit [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) v adresáři hello. |
| **Role události** | |
| Přidat tooRole člen role |Přidat roli uživatele tooa adresáře. |
| Odebrat člen role z Role |Odebrat uživatele z role adresáře. |
| AddRoleDefinition |Přidání role definice. |
| UpdateRoleDefinition |Aktualizovat definice role. toosee nastavení role, které byly aktualizovány, najdete v příliš[auditovat vlastnosti definice Role](#update-role-definition-attributes) v následující části hello |
| DeleteRoleDefinition |Odstranit definici role. |
| AddRoleAssignmentToRoleDefinition |Přidat definici toorole přiřazení role. |
| RemoveRoleAssignmentFromRoleDefinition |Přiřazení role odebrané z definice role. |
| AddRoleFromTemplate |Přidání role z šablony. |
| UpdateRole |Aktualizované role. |
| AddRoleScopeMemberToRole |Přidat toorole člen v rámci oboru. |
| RemoveRoleScopedMemberFromRole |Odebrat člen v rámci oboru z role. |
| **Události zařízení (všechny nové události)** | |
| AddDevice |Přidání zařízení. |
| UpdateDevice |Aktualizované zařízení. toosee se aktualizovaly vlastnosti jaké zařízení, najdete v příliš[vlastnosti zařízení Audited](#update-device-attributes) v následující části hello |
| DeleteDevice |Zařízení. |
| AddDeviceConfiguration |Konfigurace přidané zařízení. |
| UpdateDeviceConfiguration |Konfigurace aktualizované zařízení. toosee aktualizace vlastností konfigurace jaké zařízení, najdete v příliš[vlastnosti konfigurace zařízení Audited](#update-device-configuration-attributes) v následující části hello |
| DeleteDeviceConfiguration |Konfigurace zařízení. |
| AddRegisteredOwner |Přidat toodevice registrovaný vlastník. |
| AddRegisteredUsers |Přidat toodevice registrovaní uživatelé. |
| RemoveRegisteredOwner |Registrovaný vlastník odeberte ze zařízení. |
| RemoveRegisteredUsers |Registrovaní uživatelé odeberte ze zařízení. |
| RemoveDeviceCredentials |Odeberte přihlašovací údaje zařízení. |
| **Události B2B** | |
| Batch pozve nahrát. |Správce odeslal soubor obsahující pozvánek toobe odeslané toopartner uživatele. |
| Pozve dávkové zpracování. |Soubor obsahující pozvánek toopartner uživatele byla zpracována. |
| Pozvěte externího uživatele. |Externí uživatel byl pozvané toohello adresáře. |
| Uplatněte pozvání externího uživatele. |Externí uživatel má uplatněn adresáře toohello pozvánku. |
| Přidejte toogroup externího uživatele. |Externí uživatel byl přiřazen tooa skupinu členství v adresáři hello. |
| Přiřaďte tooapplication externího uživatele. |Externí uživatel byl přiřazen aplikace tooan přímý přístup. |
| Vytváření virální klienta. |Hello pozvánku se vytvořila nového klienta ve službě Azure AD. |
| Vytvoření virální uživatele. |Uživatel byl vytvořen v existujícího klienta ve službě Azure AD hello pozvánku k uplatnění. |
| **Administrativních jednotek (všechny nové události)** | |
| AddAdministrativeUnit |Přidáte správce jednotku. |
| UpdateAdministrativeUnit |Aktualizujte správce jednotku. toosee byly aktualizovány jaké vlastnosti administrativní jednotky odkazovat příliš[vlastnosti pro správu jednotky auditovat](#update-administrative-unit-attributes) v následující části hello |
| DeleteAdministrativeUnit |Odstraní správu jednotku. |
| AddMemberToAdministrativeUnit |Přidáte člena tooadministrative jednotku. |
| RemoveMemberFromAdministrativeUnit |Odebrání člena z administrativní jednotky. |
| **Directory události** | |
| Přidat toocompany partnera |Přidat adresář toohello partnera. |
| Odebrání partnera společnosti |Odebrat z adresáře hello partnera. |
| DemotePartner |Snížení úrovně partnera. |
| Přidání domény toocompany |Přidat adresář toohello domény. |
| Odebrat domény z společnosti |Odebrat domény z adresáře hello. |
| Aktualizace domény |Aktualizovat domény v adresáři hello. toosee se aktualizovaly vlastnosti jaké domény, najdete v příliš[vlastnosti domény auditovat](#update-domain-attributes) v následující části hello |
| Ověřování v doméně sady |Změnit hello výchozí nastavení domény hello společnosti. |
| Nastavte kontaktní informace společnosti |Nastavit předvolby kontaktu společnosti úrovni. To zahrnuje e-mailové adresy pro marketing, jakož i technické oznámení o služeb Microsoft Online Services. |
| Nastavení federace v doméně |Aktualizace nastavení hello federace pro doménu. |
| Ověření domény |Ověření domény v adresáři hello. |
| Ověření ověřené domény e-mailu |Ověření domény v adresáři hello pomocí ověření e-mailu. |
| Nastavte příznak DirSyncEnabled na společnosti |Nastavte vlastnost hello, která umožňuje adresáře pro Azure AD Sync. |
| Nastavení zásad pro hesla |Nastavte omezení délky a znak pro hesla uživatele. |
| Nastavení informací o společnosti |Informace na úrovni společnosti aktualizované hello. V tématu hello [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) rutiny prostředí PowerShell pro další podrobnosti. |
| SetCompanyAllowedDataLocation |Sada společnosti povoleno umístění dat. |
| SetCompanyDirSyncEnabled |Nastavte příznak DirSyncEnabled. |
| SetCompanyDirSyncFeature |Nastavte funkci nástroje DirSync. |
| SetCompanyInformation |Informace o sadě společnosti. |
| SetCompanyMultiNationalEnabled |Nastavit společnosti mezinárodní funkce povolena. |
| SetDirectoryFeatureOnTenant |Nastavit adresář funkce na klienta. |
| SetTenantLicenseProperties |Nastavit vlastnosti licenci klienta. |
| CreateCompanySettings |Vytvoření nastavení společnosti |
| UpdateCompanySettings |Aktualizujte nastavení společnosti. toosee, jaké vlastnosti společnosti byly aktualizovány, najdete v příliš[společnosti vlastnosti auditovat](#update-company-attributes) v následující části hello |
| DeleteCompanySettings |Odstranit nastavení společnosti |
| SetAccidentalDeletionThreshold |Nastavení prahové hodnoty náhodného odstranění. |
| SetRightsManagementProperties |Nastavit vlastnosti správy práv. |
| PurgeRightsManagementProperties |Vyprázdnit vlastnosti rights management. |
| UpdateExternalSecrets |Aktualizovat externí tajné klíče |
| **Události zásad (všechny nové události)** | |
| AddPolicy |Přidání zásad. |
| UpdatePolicy |Aktualizujte zásady. |
| Odstranit zásady |Odstraňte zásadu. |
| AddDefaultPolicyApplication |Přidejte tooapplication zásad. |
| AddDefaultPolicyServicePrincipal |Přidejte hlavní tooservice zásad. |
| RemoveDefaultPolicyApplication |Odeberte zásady z aplikace. |
| RemoveDefaultPolicyServicePrincipal |Odebrání zásad instanční objekt. |
| RemovePolicyCredentials |Odeberte zásady přihlašovací údaje. |

## <a name="audit-report-retention"></a>Uchování sestavy auditu

Hello nejnovější informace o uchování najdete v tématu [zásady uchování sestav Azure Active Directory](active-directory-reporting-retention.md).


Pro zákazníky ukládat svoje události auditu delší dobu uchování hello Reporting rozhraní API může být použité tooregularly vyžádání obsahu události auditu do samostatné datové úložiště. V tématu [Začínáme s hello Reporting rozhraní API](active-directory-reporting-api-getting-started.md) podrobnosti.

## <a name="properties-included-with-each-audit-event"></a>Vlastnosti, které jsou součástí jednotlivých událostí auditu
| Vlastnost | Popis |
| --- | --- |
| Datum a čas |Hello datum a čas, které hello událostí auditu došlo k chybě |
| Objektu actor |Hello uživatele nebo objektu služby, které provádí akce hello |
| Akce |Hello akci, která byla provedena |
| cíl |Hello uživatele nebo instanční objekt, který hello akce se provedla na |

## <a name="update-user-attributes"></a>"Aktualizovat uživatele" atributy
události auditu Hello "aktualizace uživatel" obsahuje další informace o atributy uživatele, které byly aktualizovány. Pro každý atribut obě hello předchozí hodnotu a nová hodnota hello je součástí.

| Atribut | Popis |
| --- | --- |
| accountEnabled |Hello uživatel může přihlásit. |
| AssignedLicense |Všechny licence, které byly přiřazeny toohello uživatele. |
| AssignedPlan |Plány služeb vyplývající z hello licence přiřadit toohello uživatele. |
| LicenseAssignmentDetail |Podrobnosti o licence přiřadit toohello uživatele. Například pokud se jedná o na základě skupiny licencí, to by mělo zahrnovat hello skupinu, která uděleno hello licence. |
| Mobilní |Hello uživatele mobilního telefonu. |
| OtherMail |Hello alternativní e-mailovou adresu uživatele. |
| otherMobile |Hello alternativního mobilního telefonu uživatele. |
| StrongAuthenticationMethod |Seznam metod ověření nakonfiguroval hello uživatel pro službu Multi-Factor Authentication, jako je například hlasový hovor, SMS nebo ověřovací kód z mobilní aplikace. |
| StrongAuthenticationRequirement |Pokud je vynutit službu Multi-Factor Authentication, povolit nebo zakázat pro tohoto uživatele. |
| StrongAuthenticationUserDetails |Hello uživatele telefonní číslo, alternativní telefonní číslo a e-mailová adresa se používá pro službu Multi-Factor Authentication a ověření resetování hesla. |
| StrongAuthenticationPhoneAppDetail |Podrobnosti o forof telefonní aplikace zaregistrované tooperform 2FA přihlášení |
| telephoneNumber |telefonní číslo uživatele Hello. |
| AlternativeSecurityId |ID alternativní zabezpečení pro objekt hello. |
| CreationType |Způsob vytvoření hello uživatele, (buď pozvánku nebo virální). |
| InviteTicket |Seznam pozvánku lístky pro uživatele hello. |
| InviteReplyUrl |Seznam adres URL tooreply při přijetí pozvánky. |
| InviteResources |Seznam prostředků toowhich hello uživatele dostali pozvánku. |
| LastDirSyncTime |Čas poslední hello objekt byl aktualizován z důvodu synchronizace z autoritativních hello (zákazníka, místní) adresáře. |
| MSExchRemoteRecipientType |Mapuje tooMSO příjemce typy. Odkazovat příliš https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx [MSO příjemce typy] pro příjemce typy |
| PreferredDataLocation |umístění Hello preferované hello uživatele, skupiny, kontaktu, veřejné složky, nebo data na zařízení. |
| ProxyAddresses |Hello adresa, podle kterého se rozpozná příjemce objekt serveru Exchange Server v systému cizí e-mailu. |
| StsRefreshTokensValidFrom |Má u sebe aktualizovat informace o tokenu odvolání – všechny tokeny obnovení služby tokenů zabezpečení vydané před této doby jsou považovány za vypršela platnost. |
| UserPrincipalName |Hello názvu UPN, které se Internetu stylu přihlašovací jméno pro uživatele. |
| UserState |Stav uživatele čeká na schválení nebo PendingAcceptance/přijmout nebo PendingVerification. |
| UserStateChangedOn |Časové razítko poslední tooUserState změnu. Použít pracovní postupy tootrigger životního cyklu. |
| UserType |Typ uživatele. Člen (0), Host (1), Viral(2). |

## <a name="update-group-attributes"></a>Atributy "Skupina aktualizací"
| Atribut | Popis |
| --- | --- |
| klasifikace |Hello klasifikace pro skupinu Unified (HBI, MBI atd.). |
| Popis |Čitelný slovní o hello objektu. |
| displayName |Hello zobrazovaný název pro objekt |
| dirSyncEnabled |Určuje, zda dochází k synchronizaci ze autoritativní adresáře (zákazníka, místní). |
| GroupLicenseAssignment |Přiřazení licencí skupiny |
| groupType |Typ skupiny Unified (0) |
| IsMembershipRuleLocked |Určuje, že hello MembershipRule vlastnost nastavena službou management hello samoobslužné služby skupiny a nelze změnit pomocí uživatele. Použít pouze toogroups kde GroupType obsahuje GroupType.DynamicMembership |
| IsPublic |Příznak tooindicate, pokud skupina hello veřejného a privátního. |
| LastDirSyncTime |Čas poslední hello objekt byl aktualizován v důsledku synchronizuje se službou hello autoritativní (místní zákazníka) adresáře. |
| Pošta |Primární e-mailová adresa |
| MailEnabled |Označuje, zda tato skupina má e-mailové funkce. |
| mailNickname |Přezdívka pro objekt adresáře adresu, obvykle hello část názvu e-mailu předcházející hello "@" symbol. |
| MembershipRule |Řetězec vyjadřující kritéria hello používá hello samoobslužné služby skupiny správy služby toodetermine členy, které by měly patřit toothis skupiny. Další informace najdete v části IsMembershipRuleLocked. Použít pouze toogroups kde GroupType obsahuje GroupType.DynamicMembership. |
| MembershipRuleProcessingState |Hodnotu výčtu definované definování hello stav zpracování pro tuto skupinu členství ve službě pro správu hello samoobslužné služby skupiny. Použít pouze toogroups kde GroupType obsahuje GroupType.DynamicMembership. |
| ProxyAddresses |Hello adresa, podle kterého se rozpozná příjemce objekt serveru Exchange Server v systému cizí e-mailu. |
| RenewedDateTime |Pokud byla skupina naposledy obnoven záznam časové razítko. |
| securityEnabled |Určuje, zda členství ve skupině hello může ovlivnit autorizačních rozhodnutích. |
| WellKnownObject |Označuje objekt adresáře určení jako jeden z předem definované sadě. |

## <a name="update-device-attributes"></a>"Aktualizovat zařízení" atributy
| Atribut | Popis |
| --- | --- |
| accountEnabled |Určuje, zda může ověřit objekt zabezpečení. |
| CloudAccountEnabled |Určuje, zda může ověřit objekt zabezpečení. Služba InTune zapíše, když zařízení hello řídí se hlavním místní. |
| CloudDeviceOSType |Typ hello zařízení podle operačního systému, např. Windows RT, iOS hello. Pokud nastavíte pomocí cloudové služby (například Intune), tento atribut se změní v adresáři hello autoritativní pro DeviceOSType. |
| CloudDeviceOSVersion |Verze hello operačního systému. Pokud nastavíte pomocí cloudové služby (například Intune), tento atribut se změní v adresáři hello autoritativní pro DeviceOSVersion. |
| CloudDisplayName |Hodnota atributu LDAP displayName hello. Pokud nastavíte pomocí cloudové služby (například Intune), bude tento atribut v adresáři hello displayName autoritativní. |
| CloudCreated |Určuje, zda text hello objekt byl vytvořen cloudové služby. |
| CompliantUntil |Do jaké doby je považován za kompatibilní zařízení. |
| DeviceMetadata |Vlastních metadat pro hello zařízení |
| DeviceObjectVersion |Tento atribut je verze schématu hello použité tooidentify hello zařízení. |
| deviceOSType |Typ hello zařízení podle operačního systému, např. Windows RT, iOS hello. Služba zapíše hello Registration Service a určený toobe aktualizovat hello služba správy MDM nebo služby tokenů zabezpečení light služba pro správu. |
| DeviceOSVersion |Verze hello operačního systému. Služba zapíše hello Registration Service a určený toobe aktualizovat hello služba správy MDM nebo služby tokenů zabezpečení light služba pro správu. |
| DevicePhysicalIds |Vícehodnotový atribut určený toostore identifikátory hello fyzického zařízení. To může zahrnovat ID systému BIOS, čipu TPM kryptografických otisků, hardwaru konkrétní ID atd. |
| dirSyncEnabled |Označuje, zda autoritativní (zákazníkovi, místní) dochází k synchronizaci adresáře. |
| displayName |Hello zobrazovaný název pro objekt |
| IsCompliant |Tento atribut je stav správy mobilního zařízení používaných toomanage hello hello zařízení. |
| ismanaged – |Tento atribut slouží tooindicate hello zařízení spravuje cloudu správy mobilních zařízení. |
| LastDirSyncTime |Čas poslední hello objekt byl aktualizován z důvodu synchronizace z autoritativních hello (místní zákazníka) adresáře. |

## <a name="update-device-configuration-attributes"></a>"Aktualizovat konfiguraci zařízení" atributy
| Atribut | Popis |
| --- | --- |
| MaximumRegistrationInactivityPeriod |Hello maximální počet dní zařízení může být neaktivní, než je považován za pro odebrání. |
| RegistrationQuota |Zásada používá toolimit hello číslo registrací zařízení, které jsou povoleny pro jednoho uživatele. |

## <a name="update-service-principal-configuration-attributes"></a>"Aktualizovat objektu konfigurace služby" atributy
| Atribut | Popis |
| --- | --- |
| accountEnabled |Určuje, zda může ověřit objekt zabezpečení. |
| AppPrincipalId |Externí, definované aplikací identitu pro objekt zabezpečení. |
| displayName |Hello zobrazovaný název pro objekt |
| ServicePrincipalName |Hlavní název služby, který obsahuje "název/autorita", kde název určuje hodnotu – třída aplikace a autority obsahuje alespoň název hostitele [: port] nebo "name", který určuje identifikátor pro hello instanční objekt. |

## <a name="update-app-attributes"></a>"Aktualizace aplikace" atributy
| Atribut | Popis |
| --- | --- |
| AppAddress |Sada Hello adresy (adresy URL pro přesměrování), které jsou přiřazeny tooa instanční objekt. |
| appId |ID aplikace hello aplikace |
| AppIdentifierUri |URI aplikace, které identifikuje aplikace hello.  Je obvykle hello adresu URL pro přístup k aplikaci. |
| AppLogoUrl |Adresa url Hello pro obrázek loga aplikace hello uložené v název CDN. |
| AvailableToOtherTenants |True hello aplikace je víceklientské aplikace (tj. můžete použít další klienty). |
| displayName |Hello zobrazovaný název pro název aplikace |
| Nároku |Seznam oprávnění aplikace. |
| ExternalUserAccountDelegationsAllowed |Příznak indikující, zda je důvěryhodný prostředků aplikace a můžete vytvořit záznamy delegování pro externí uživatelské účty. |
| GroupMembershipClaims |členství ve skupině Hello deklarací zásad. |
| PublicClient |Hodnota TRUE, pokud klient hello nemůže nadále tajný (tj.-důvěrné client OAuth2.0) |
| RecordConsentConditions |Typy souhlasu podmínek, podle definice hello smluvní podmínky: žádné (0), SilentConsentForPartnerManagedApp(1). Tato hodnota se zveřejní ve schématu rozhraní Graph API hello a může být pouze sadu či změnit podle správců klientů. |
| RequiredResourceAccess |Obsah XML hodnotu hello RequiredResourceAccess vlastnost. |
| WebApp |V případě hodnoty true označuje, že tato aplikace je webovou aplikaci. |
| WwwHomepage |Hello primární webové stránky. |

## <a name="update-role-attributes"></a>"Aktualizovat roli" atributy
| Atribut | Popis |
| --- | --- |
| AppAddress |Sada Hello adresy (adresy URL pro přesměrování), které jsou přiřazeny tooa instanční objekt. |
| BelongsToFirstLoginObjectSet |V případě hodnoty true označuje, že tento objekt patří toohello sadu objektů požadované tooenable přihlášení hello první správce nového klienta. |
| Předdefinované |Určuje, zda hello životnost objektu je vlastníkem hello systému. |
| Popis |Čitelný slovní o hello objektu. |
| displayName |Hello zobrazovaný název pro objekt |
| mailNickname |Přezdívka pro objekt adresáře adresu, obvykle hello část názvu e-mailu předcházející hello "@" symbol. |
| RoleDisabled |Určuje, zda hello role se musí ignorovat pro účely kontroly přístupu. |
| RoleTemplateId |Identita šablony role hello. |
| ServiceInfo |Specifickou pro službu zřizování informace, které mohou být spotřebovávána MOAC nebo jiné instance služby (z hello stejné nebo různých typů služeb). |
| TaskSetScopeReference |Identifikuje TaskSet a sadu obory přidružené k roli nebo šablony role. |
| ValidationError |Informace, které zveřejnil federované služby popisující bez přechodná, specifickou pro službu chyba týkající se vlastností hello nebo odkaz z tooresolve akce správce k objektu. |
| WellKnownObject |Označuje objekt adresáře určení jako jeden z předem definované sadě. |

## <a name="update-role-definition-attributes"></a>"Aktualizovat definice Role" atributy
| Atribut | Popis |
| --- | --- |
| AssignableScopes |Kolekce autorizace obory, které může být odkazováno při přiřazování zaregistrovaných RoleDefinition tooa. |
| displayName |Hello zobrazovaný název pro objekt |
| GrantedPermissions |Oprávnění udělená podle této RoleDefinition. |

## <a name="update-administrative-unit-attributes"></a>"Aktualizovat správní jednotku" atributy
| Atribut | Popis |
| --- | --- |
| Popis |Tato vlastnost je aktualizována při změně hello popis administrativní jednotky. |
| displayName |Tato vlastnost je aktualizována při změně názvu hello administrativní jednotky. |

## <a name="update-company-attributes"></a>Atributy "Společnost aktualizace"
| Atribut | Popis |
| --- | --- |
| AllowedDataLocation |Umístění, ve které hello společnosti můžou uživatelé toobe zřízený. |
| AuthorizedServiceInstance |Názvy toowhich instancí služby plán může být nasazeny. |
| dirSyncEnabled |Označuje, zda autoritativní (zákazníkovi, místní) dochází k synchronizaci adresáře. |
| DirSyncStatus |Značí, zda synchronizace adresáře objektů adresu v tomto kontextu klienta autoritativní (zákazníkovi, místní) directory; rozšíření hello vlastnost DirSyncEnabled na objekty společnosti. |
| DirSyncFeatures |Bit příznaku tookeep zaznamenávat sadu funkcí pro povolení i Zakázaní dirsync pro klienta hello. |
| DirectoryFeatures |Funkce directory povoleno nebo zakázáno. |
| DirSyncConfiguration |Obsahuje všechny DirSync konfigurace konkrétní toohello aktuální klienta. |
| displayName |Hello zobrazovaný název pro objekt |
| IsMnc |Společnost hello příliš "PRAVDA", je-li sadu logický příznak je povoleno pro funkci mezinárodní společnosti hello. |
| ObjectSettings |Kolekce nastavení oboru příslušné toohello hello objektu. |
| PartnerCommerceUrl |Adresa URL toohello partnera web obchodu. |
| PartnerHelpUrl |Adresa URL toohello partnera nápovědy lokality. |
| PartnerSupportEmail |Adresa URL toohello partnera podpory e-mailu. |
| PartnerSupportTelephone |Adresa URL toohello partnera telefonní podpory. |
| PartnerSupportUrl |Partnera toohello adresu URL webu podpory. |
| StrongAuthenticationDetails |Podrobnosti související s tooStrongAuthentication. |
| StrongAuthenticationPolicy |Silné ověřování zásad společnosti hello. |
| TechnicalNotificationMail |E-mailovou adresu toonotify technických problémů tooa společnosti, která se týkají. |
| telephoneNumber |Telefonní čísla v souladu s hello E.123 doporučení ITU. |
| TenantType |Typ Hello klienta. Pokud tato hodnota není zadaná, klient hello je společnost. Jinak, možné hodnoty jsou: MicrosoftSupport (0), SyndicatePartner (1), BreadthPartner (2) BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5). |
| VerifiedDomain |Sada názvů domén DNS vázán tooa společnosti. |

## <a name="update-domain-attributes"></a>"Aktualizovat domény" atributy
| Atribut | Popis |
| --- | --- |
| Možnosti |Bitové příznaky popisující hello možnosti domény hello, pokud existuje. |
| Výchozí |Určuje, zda hello domény je výchozí hodnota hello; například hello výchozí UserPrincipalName příponu při správce vytvoří nového uživatele v MOAC. |
| Počáteční |Určuje, zda je hello domény hello počáteční domény hello společnosti, jako přidělené OCP. počáteční doména Hello je jedinečné subdomény domény Microsoft Online; e.g.contoso3.microsoftonline.com. |
| LiveType |Typ hello odpovídající Windows Live oboru názvů, pokud existuje. |
| Name (Název) |Identifikátor pro koncový bod hello. |
| PasswordNotificationWindowDays |Hello počet dní před vypršením platnosti hello uživatele hesla, je upozornění. |
| PasswordValidityPeriodDays |Hello počet dnů, po který je vhodný pro heslo, než je nutné změnit. |

Záznamy auditu jsou požadovaný ovládací prvek pro mnoho předpisy. Pro zákazníky používající hello Azure Active Directory Audit sestavy toomeet jejich předpisy se doporučuje tohoto zákazníka hello odeslání, kopie Toto téma nápovědy s hello kopii hello zákazníka exportovat sestava auditu toohelp vysvětlují hello sestavy Podrobnosti. Pokud hello auditor chtěli toounderstand hello předpisy, které Azure aktuálně splňuje, přímé hello auditor toohello [stránky dodržování předpisů](https://azure.microsoft.com/support/trust-center/compliance/) z hello Microsoft Azure Trust Center.

