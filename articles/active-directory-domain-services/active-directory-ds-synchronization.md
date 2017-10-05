---
title: "Azure Active Directory Domain Services: Synchronizace v doménách spravovaných | Microsoft Docs"
description: "Pochopení synchronizace ve spravované doméně služby Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 57cbf436-fc1d-4bab-b991-7d25b6e987ef
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 0c9a9a56e1489ee91fcc332beeef36cdc9c93dc1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Synchronizace ve spravované doméně služby Azure AD Domain Services
Následující diagram znázorňuje, jak funguje synchronizace v Azure AD Domain Services spravované domény.

![Topologie synchronizace v Azure AD Domain Services](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Synchronizace z místního adresáře do vašeho klienta Azure AD
Synchronizace Azure AD Connect se používá k synchronizaci uživatelských účtů, členství ve skupinách a přihlašovacích údajů, vytvoří hodnotu hash pro vašeho klienta Azure AD. Atributy uživatele účtů jako je například název UPN a místní SID (security identifier) jsou synchronizovány. Pokud používáte Azure AD Domain Services, hodnoty hash přihlašovacích údajů starší verze, které jsou požadované pro ověřování NTLM a Kerberos se synchronizují taky ke klientovi Azure AD.

Pokud nakonfigurujete zpětný zápis, změny, ke kterým dochází v adresáři služby Azure AD jsou synchronizovány zpět do služby Active Directory v místě. Například pokud změníte svoje heslo pomocí funkce změnu hesla pomocí samoobslužné služby Azure AD, změněného hesla je aktualizovat v místní doméně AD.

> [!NOTE]
> Vždy použijte nejnovější verzi služby Azure AD Connect zajistit, že máte opravy pro všechny známé chyby.
>
>

## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Synchronizace z vašeho klienta Azure AD vaší spravované domény
Uživatelské účty, členství ve skupinách a hodnoty hash přihlašovacích údajů jsou synchronizované z vašeho klienta Azure AD vaší spravované domény služby Azure AD Domain Services. Tento proces synchronizace je automatické. Není nutné konfigurovat, sledovat nebo spravovat proces synchronizace. Po dokončení jednorázové počáteční synchronizace adresáře se obvykle trvá přibližně 20 minut, než změny provedené ve službě Azure AD se projeví ve vaší spravované domény. Tento interval synchronizace se vztahuje na změny hesel nebo změny atributů provedené ve službě Azure AD.

Proces synchronizace je také jednoho-way/jednosměrný ve své podstatě. Spravované domény je z velké části jen pro čtení s výjimkou žádné vytvoříte vlastní organizační jednotky. Proto nelze provádět změny atributů uživatele, uživatelská hesla nebo členství ve skupinách v rámci spravované domény. V důsledku toho není k dispozici žádné reverzní synchronizace změn z vaší spravované domény zpět ke klientovi Azure AD.

## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Synchronizace z prostředí s více doménovými strukturami na místě
Mnoho organizací mít poměrně složitá místní infrastrukturu identity skládající se z několika doménových struktur účtů. Azure AD Connect podporuje synchronizaci uživatelů, skupin a hodnoty hash přihlašovacích údajů z prostředí s více doménovými strukturami ke klientovi Azure AD.

Naproti tomu klientovi Azure AD je mnohem jednodušší a plochý obor názvů. Pokud chcete povolit uživatelům spolehlivě přístup k aplikacím zabezpečeným službou Azure AD, řešení konfliktů UPN mezi uživatelskými účty v různých doménových strukturách. Vaše nese spravované doméně služby Azure AD Domain Services zavřete podobný původnímu klientovi Azure AD. Proto byste vidět ploché struktuře organizační jednotky ve vaší spravované domény. Všichni uživatelé a skupiny jsou uloženy v kontejneru 'AADDC uživatele', bez ohledu na místní domény nebo doménové struktury, ve kterém byly synchronizované v. Jste nakonfigurovali hierarchické organizační jednotky struktury místně. Spravované domény má však stále jednoduché ploché struktuře organizační jednotky.

## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Vyloučení – co není synchronizován k spravované doméně
Následující objekty nebo atributy nejsou synchronizované ke klientovi Azure AD nebo k vaší spravované domény:

* **Vyloučené atributy:** můžete vyloučit určité atributy synchronizaci klientovi Azure AD z vaší místní domény pomocí služby Azure AD Connect. Tyto atributy vyloučené nejsou k dispozici ve vaší spravované domény.
* **Zásady skupiny:** nakonfigurované ve vaší doméně místní zásady skupiny nejsou synchronizovány do vaší spravované domény.
* **Sdílené složky SYSVOL:** Podobně obsah sdílené složky SYSVOL v místní doméně nejsou synchronizovány do vaší spravované domény.
* **Objekty počítače:** objekty počítačů pro počítače připojené k vaší doméně místní nejsou synchronizovány s vaší spravované domény. Tyto počítače nemají vztah důvěryhodnosti s vaší spravované domény a patří k vaší místní doméně. Ve vaší spravované domény najít objekty počítače pouze pro počítače, které budete mít explicitně doméně k spravované doméně.
* **Atributy historie čísel SID pro uživatele a skupiny:** primárního uživatele a skupiny primární SID z vaší místní domény jsou synchronizovány do vaší spravované domény. Však existující atributy historie čísel SID pro uživatele a skupiny nejsou synchronizované z vaší místní domény vaší spravované domény.
* **Struktury organizační jednotky (OU):** organizační jednotky, které jsou definované v místní doméně nesynchronizovat k vaší spravované domény. Existují dvě předdefinované organizačních jednotek ve vaší spravované domény. Ve výchozím nastavení vaší spravované domény má plochý struktury organizační jednotky. Můžete však zvolit [ve vaší spravované domény vytvořit vlastní](active-directory-ds-admin-guide-create-ou.md).

## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Jak konkrétní atributy jsou synchronizovány do vaší spravované domény
Následující tabulka uvádí některé běžné atributy a popisuje, jak jsou synchronizovány do vaší spravované domény.

| Atribut vaší spravované domény | Zdroj | Poznámky |
|:--- |:--- |:--- |
| HLAVNÍ NÁZEV UŽIVATELE |Atribut uživatele (UPN) v klientovi služby Azure AD |Atribut UPN z vašeho klienta Azure AD je synchronizován, jako je vaší spravované domény. Proto je nejspolehlivější způsob, jak se přihlásit k spravované doméně pomocí vaší hlavní název uživatele. |
| Název účtu SAM |Uživatele mailNickname atribut v klientovi služby Azure AD, nebo automaticky generovaný |Atribut SAMAccountName pochází z atributu mailNickname v klientovi služby Azure AD. Pokud máte víc uživatelských účtů stejný atribut mailNickname, SAMAccountName se generuje automaticky. Pokud mailNickname nebo předpona názvu UPN uživatele je delší než 20 znaků, SAMAccountName je automaticky generovaných pro uspokojení limitu atributy SAMAccountName 20 znaků. |
| Hesla |Heslo uživatele z vašeho klienta Azure AD |Hodnoty hash přihlašovacích údajů, které jsou požadované pro ověřování protokolem NTLM nebo Kerberos (také nazývané dodatečné přihlašovací údaje) jsou synchronizované z vašeho klienta Azure AD. Pokud váš klient Azure AD je synchronizovaného klienta, tyto přihlašovací údaje pocházejí z vaší místní domény. |
| Uživatel nebo identifikátor SID primární skupiny |Automaticky generovaný |Primární identifikátor SID pro uživatele nebo skupiny účty je automaticky generovaných ve vaší spravované domény. Tento atribut neodpovídá primární uživatel nebo skupina SID objektu v místní doméně AD. Tato neshoda se totiž spravované doméně má jiný obor názvů SID než vaše místní doména. |
| Historie identifikátorů SID pro uživatele a skupiny |Místní primární uživatele a identifikátor SID skupiny |Atribut historie čísel SID pro uživatele a skupiny ve vaší spravované domény je nastaven tak, aby odpovídaly odpovídající primární uživatele nebo skupinu SID v místní doméně. Tato funkce vám pomůže snadněji navýšení a shift místní aplikací k spravované doméně, protože není nutné znovu ACL prostředky. |

> [!NOTE]
> **Přihlaste se k spravované doméně pomocí formátu UPN:** The SAMAccountName atributu může být automaticky generovaných pro některé uživatelské účty ve vaší spravované domény. Pokud více uživatelů mají stejný atribut mailNickname nebo uživatelů, kteří mají příliš dlouho předpony (UPN), může být automaticky generovaný SAMAccountName pro tyto uživatele. Proto SAMAccountName formátu (například "CONTOSO100\joeuser') není vždy spolehlivý způsob, jak se přihlásit k doméně. Automaticky generovaný SAMAccountName uživatelů může lišit od jejich předpona názvu UPN. Použijte formát UPN (například "joeuser@contoso100.com") se přihlásit k spravované doméně spolehlivě.
>
>

### <a name="attribute-mapping-for-user-accounts"></a>Mapování atributů pro uživatelské účty
Následující tabulka ukazuje, jak konkrétní atributy pro uživatelské objekty v klientovi služby Azure AD jsou synchronizovány do odpovídající atributy ve vaší spravované domény.

| Atribut uživatele v klientovi služby Azure AD | Atribut uživatele ve vaší spravované domény |
|:--- |:--- |
| accountEnabled |userAccountControl (Zapne nebo vypne ACCOUNT_DISABLED bitů) |
| city |l |
| Země |co |
| Oddělení |Oddělení |
| displayName |displayName |
| facsimileTelephoneNumber |facsimileTelephoneNumber |
| givenName |givenName |
| pracovní funkce |Název |
| E-mailu |E-mailu |
| mailNickname |msDS-AzureADMailNickname |
| mailNickname |Název účtu SAM (může být někdy automaticky vygenerovaný) |
| mobilní |mobilní |
| objectid |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |historie čísel SID |
| passwordPolicies |userAccountControl (Zapne nebo vypne DONT_EXPIRE_PASSWORD bitů) |
| physicalDeliveryOfficeName |physicalDeliveryOfficeName |
| PSČ |PSČ |
| preferredLanguage |preferredLanguage |
| state |St |
| StreetAddress |StreetAddress |
| Příjmení |sn |
| telephoneNumber |telephoneNumber |
| UserPrincipalName |UserPrincipalName |

### <a name="attribute-mapping-for-groups"></a>Mapování atributů pro skupiny
Následující tabulka ukazuje, jak konkrétní atributy pro skupinu objektů v klientovi služby Azure AD jsou synchronizovány do odpovídající atributy ve vaší spravované domény.

| Skupinu atributů v klientovi služby Azure AD | Atribut skupiny ve vaší spravované domény |
|:--- |:--- |
| displayName |displayName |
| displayName |Název účtu SAM (může být někdy automaticky vygenerovaný) |
| E-mailu |E-mailu |
| mailNickname |msDS-AzureADMailNickname |
| objectid |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |historie čísel SID |
| securityEnabled |groupType |

## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Objekty, které nejsou pro vašeho klienta Azure AD synchronizované z vaší spravované domény
Jak je popsáno v předchozí části tohoto článku, neexistuje žádná synchronizace z vaší spravované domény zpět ke klientovi Azure AD. Můžete zvolit [vytvořit vlastní organizační jednotce (OU)](active-directory-ds-admin-guide-create-ou.md) ve vaší spravované domény. Navíc můžete vytvořit jiné organizační jednotky, uživatele, skupiny nebo účty služby v rámci těchto vlastních organizačních jednotek. Žádné objekty vytvořené v rámci vlastní organizační jednotky jsou synchronizovány zpátky do vašeho klienta Azure AD. Tyto objekty lze použít pouze v rámci vaší spravované domény. Proto tyto objekty nejsou viditelné prostřednictvím rutin prostředí Azure AD PowerShell, Azure AD Graph API nebo přes uživatelské rozhraní pro správu Azure AD.

## <a name="related-content"></a>Související obsah
* [Funkce – Azure AD Domain Services](active-directory-ds-features.md)
* [Scénáře nasazení – Azure AD Domain Services](active-directory-ds-scenarios.md)
* [Požadavky sítě pro Azure AD Domain Services](active-directory-ds-networking.md)
* [Začínáme s Azure AD Domain Services](active-directory-ds-getting-started.md)
