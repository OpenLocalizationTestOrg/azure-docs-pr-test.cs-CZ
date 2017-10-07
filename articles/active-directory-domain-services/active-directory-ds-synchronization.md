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
ms.openlocfilehash: 9be25b61823a6b031906f3576395782e73831fc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Synchronizace ve spravované doméně služby Azure AD Domain Services
Hello následující diagram ukazuje, jak funguje synchronizace v Azure AD Domain Services spravovaných domén.

![Topologie synchronizace v Azure AD Domain Services](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-tooyour-azure-ad-tenant"></a>Synchronizace z vašeho místního adresáře tooyour Azure AD klienta
Synchronizace Azure AD Connect je použité toosynchronize uživatelských účtů, členství ve skupinách a klienta tooyour Azure AD hodnoty hash přihlašovacích údajů. Atributy uživatele účtů například hello UPN a místní SID (security identifier) jsou synchronizovány. Pokud používáte Azure AD Domain Services, je klient Azure AD synchronizované tooyour taky hodnoty hash přihlašovacích údajů starší verze, které jsou požadované pro ověřování NTLM a Kerberos.

Pokud nakonfigurujete zpětný zápis, jsou změny, ke kterým dochází v adresáři služby Azure AD synchronizovány back tooyour místní služby Active Directory. Například pokud změníte svoje heslo pomocí funkce změnu hesla pomocí samoobslužné služby Azure AD, hello změněného hesla je aktualizovat v místní doméně AD.

> [!NOTE]
> Vždy použít nejnovější verzi hello z Azure AD Connect tooensure máte opravy pro všechny známé chyby.
>
>

## <a name="synchronization-from-your-azure-ad-tenant-tooyour-managed-domain"></a>Synchronizace z vaší tooyour klienta Azure AD spravovat domény
Uživatelské účty, členství ve skupinách a hodnoty hash přihlašovacích údajů jsou synchronizované z vaší služby Azure AD klienta tooyour spravované doméně služby Azure AD Domain Services. Tento proces synchronizace je automatické. Není nutné tooconfigure, sledování a správě tohoto procesu synchronizace. Po dokončení hello jednorázové počáteční synchronizace adresáře se obvykle trvá přibližně 20 minut, než změny provedené v Azure AD toobe projeví ve vaší spravované domény. Tento interval synchronizace použije toopassword změny nebo změny tooattributes provedené ve službě Azure AD.

Proces synchronizace Hello je také jednoho-way/jednosměrný ve své podstatě. Spravované domény je z velké části jen pro čtení s výjimkou žádné vytvoříte vlastní organizační jednotky. Proto nelze provádět změny toouser atributy, uživatelská hesla nebo členství ve skupinách v rámci hello spravované domény. V důsledku toho není k dispozici žádné reverzní synchronizace změn z vaší spravované domény back tooyour klienta Azure AD.

## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Synchronizace z prostředí s více doménovými strukturami na místě
Mnoho organizací mít poměrně složitá místní infrastrukturu identity skládající se z několika doménových struktur účtů. Azure AD Connect podporuje synchronizaci uživatelů, skupin a hodnoty hash přihlašovacích údajů z klienta prostředí s více doménovými strukturami tooyour Azure AD.

Naproti tomu klientovi Azure AD je mnohem jednodušší a plochý obor názvů. tooenable uživatelé tooreliably přístup k aplikacím zabezpečeným službou Azure AD, řešení konfliktů UPN mezi uživatelskými účty v různých doménových strukturách. Vaše nese spravované doméně služby Azure AD Domain Services zavřete klienta Azure AD tooyour podobají. Proto byste vidět ploché struktuře organizační jednotky ve vaší spravované domény. Všichni uživatelé a skupiny jsou uloženy v rámci kontejneru "AADDC uživatele" hello, bez ohledu na to, hello místní domény nebo doménové struktury, ve kterém byly synchronizované v. Jste nakonfigurovali hierarchické organizační jednotky struktury místně. Spravované domény má však stále jednoduché ploché struktuře organizační jednotky.

## <a name="exclusions---what-isnt-synchronized-tooyour-managed-domain"></a>Vyloučení – co není synchronizovaná tooyour spravované doméně
Hello následující objekty nebo atributy nejsou synchronizované tooyour klienta Azure AD nebo tooyour spravované domény:

* **Vyloučené atributy:** můžete se rozhodnout tooexclude určité atributy ze synchronizace tooyour Azure AD klienta z vaší místní domény pomocí služby Azure AD Connect. Tyto atributy vyloučené nejsou k dispozici ve vaší spravované domény.
* **Zásady skupiny:** nakonfigurované ve vaší doméně místní zásady skupiny nejsou synchronizované tooyour spravované domény.
* **Sdílené složky SYSVOL:** podobně hello obsah hello sdílenou složku SYSVOL v místní doméně nejsou synchronizované tooyour spravované domény.
* **Objekty počítače:** objektů počítače pro počítače připojené k tooyour místní domény nejsou synchronizované tooyour spravované domény. Tyto počítače nemají vztah důvěryhodnosti s vaší spravované domény a patří pouze tooyour místní domény. Ve vaší spravované domény najít objekty počítače pouze pro počítače, je nutné explicitně připojený k doméně toohello spravované domény.
* **Atributy historie čísel SID pro uživatele a skupiny:** hello primárního uživatele a skupiny primární SID z vaší místní domény jsou synchronizované tooyour spravované domény. Existující atributy historie čísel SID pro uživatele a skupiny však nejsou synchronizované z vaší spravované domény místní domény tooyour.
* **Struktury organizační jednotky (OU):** organizační jednotky, které jsou definované v místní doméně nesynchronizovat tooyour spravované domény. Existují dvě předdefinované organizačních jednotek ve vaší spravované domény. Ve výchozím nastavení vaší spravované domény má plochý struktury organizační jednotky. Můžete však zvolit příliš[ve vaší spravované domény vytvořit vlastní](active-directory-ds-admin-guide-create-ou.md).

## <a name="how-specific-attributes-are-synchronized-tooyour-managed-domain"></a>Jak konkrétní atributy jsou synchronizované tooyour spravované doméně
Hello následující tabulka uvádí některé běžné atributy a popisuje, jak jsou synchronizované tooyour spravované domény.

| Atribut vaší spravované domény | Zdroj | Poznámky |
|:--- |:--- |:--- |
| HLAVNÍ NÁZEV UŽIVATELE |Atribut uživatele (UPN) v klientovi služby Azure AD |atribut UPN Hello z vašeho klienta Azure AD je synchronizován, jako je tooyour spravované domény. Proto hello nejspolehlivější způsob toosign ve spravované doméně tooyour používá vaše UPN. |
| Název účtu SAM |Uživatele mailNickname atribut v klientovi služby Azure AD, nebo automaticky generovaný |Hello SAMAccountName atribut pochází z atributu mailNickname hello v klientovi služby Azure AD. Pokud máte víc uživatelských účtů stejný atribut mailNickname, hello SAMAccountName je automaticky generovaný hello. Pokud hello mailNickname nebo předpona názvu UPN uživatele je delší než 20 znaků, hello SAMAccountName je automaticky generovaný toosatisfy hello omezena na 20 znaků na atributy SAMAccountName. |
| Hesla |Heslo uživatele z vašeho klienta Azure AD |Hodnoty hash přihlašovacích údajů, které jsou požadované pro ověřování protokolem NTLM nebo Kerberos (také nazývané dodatečné přihlašovací údaje) jsou synchronizované z vašeho klienta Azure AD. Pokud váš klient Azure AD je synchronizovaného klienta, tyto přihlašovací údaje pocházejí z vaší místní domény. |
| Uživatel nebo identifikátor SID primární skupiny |Automaticky generovaný |Hello primární identifikátor SID pro uživatele nebo skupiny účty je automaticky generovaných ve vaší spravované domény. Tento atribut neodpovídá hello uživatele nebo identifikátor SID primární skupiny hello objektu v místní doméně AD. Tato neshoda se totiž hello spravované domény má jiný obor názvů SID než vaše místní doména. |
| Historie identifikátorů SID pro uživatele a skupiny |Místní primární uživatele a identifikátor SID skupiny |Hello historie čísel SID pro uživatele a skupiny ve vaší spravované domény nastavený atribut toomatch hello odpovídající primární uživatele nebo skupinu SID v místní doméně. Tato funkce pomáhá zkontrolujte navýšení a shift z místní aplikace toohello spravované doméně jednodušší, protože není nutné toore ACL prostředky. |

> [!NOTE]
> **Přihlaste se toohello spravované doméně pomocí formátu UPN hello:** hello SAMAccountName atributu může být automaticky generovaných pro některé uživatelské účty ve vaší spravované domény. Pokud máte hello více uživatelů stejný atribut mailNickname nebo uživatelé mají příliš dlouho UPN předpony, hello SAMAccountName pro tyto uživatele může být automaticky generovaný. Proto hello SAMAccountName formátu (například "CONTOSO100\joeuser') není vždy spolehlivý způsob toosign v doméně toohello. Automaticky generovaný SAMAccountName uživatelů může lišit od jejich předpona názvu UPN. Použití hello UPN formátu (například "joeuser@contoso100.com') toosign v toohello spolehlivě spravované domény.
>
>

### <a name="attribute-mapping-for-user-accounts"></a>Mapování atributů pro uživatelské účty
Hello následující tabulka znázorňuje způsob konkrétní atributy pro uživatelské objekty v klientovi služby Azure AD jsou synchronizované toocorresponding atributy ve vaší spravované domény.

| Atribut uživatele v klientovi služby Azure AD | Atribut uživatele ve vaší spravované domény |
|:--- |:--- |
| accountEnabled |userAccountControl (sady nebo vymaže hello ACCOUNT_DISABLED bitů) |
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
| passwordPolicies |userAccountControl (sady nebo vymaže hello DONT_EXPIRE_PASSWORD bitů) |
| physicalDeliveryOfficeName |physicalDeliveryOfficeName |
| PSČ |PSČ |
| preferredLanguage |preferredLanguage |
| state |St |
| StreetAddress |StreetAddress |
| Příjmení |sn |
| telephoneNumber |telephoneNumber |
| UserPrincipalName |UserPrincipalName |

### <a name="attribute-mapping-for-groups"></a>Mapování atributů pro skupiny
Hello následující tabulka znázorňuje způsob konkrétní atributy pro objekty skupiny v klientovi služby Azure AD jsou synchronizované toocorresponding atributy ve vaší spravované domény.

| Skupinu atributů v klientovi služby Azure AD | Atribut skupiny ve vaší spravované domény |
|:--- |:--- |
| displayName |displayName |
| displayName |Název účtu SAM (může být někdy automaticky vygenerovaný) |
| E-mailu |E-mailu |
| mailNickname |msDS-AzureADMailNickname |
| objectid |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |historie čísel SID |
| securityEnabled |groupType |

## <a name="objects-that-are-not-synchronized-tooyour-azure-ad-tenant-from-your-managed-domain"></a>Objekty, které nejsou synchronizovány klienta Azure AD tooyour z vaší spravované domény
Jak je popsáno v předchozí části tohoto článku, neexistuje žádná synchronizace z back tooyour spravované domény klienta Azure AD. Můžete se rozhodnout příliš[vytvořit vlastní organizační jednotce (OU)](active-directory-ds-admin-guide-create-ou.md) ve vaší spravované domény. Navíc můžete vytvořit jiné organizační jednotky, uživatele, skupiny nebo účty služby v rámci těchto vlastních organizačních jednotek. Žádná z hello objekty vytvořené v rámci vlastní organizační jednotky jsou synchronizovány back tooyour klienta Azure AD. Tyto objekty lze použít pouze v rámci vaší spravované domény. Proto tyto objekty nejsou viditelné prostřednictvím rutin prostředí Azure AD PowerShell, Azure AD Graph API nebo přes uživatelské rozhraní pro správu hello Azure AD.

## <a name="related-content"></a>Související obsah
* [Funkce – Azure AD Domain Services](active-directory-ds-features.md)
* [Scénáře nasazení – Azure AD Domain Services](active-directory-ds-scenarios.md)
* [Požadavky sítě pro Azure AD Domain Services](active-directory-ds-networking.md)
* [Začínáme s Azure AD Domain Services](active-directory-ds-getting-started.md)
