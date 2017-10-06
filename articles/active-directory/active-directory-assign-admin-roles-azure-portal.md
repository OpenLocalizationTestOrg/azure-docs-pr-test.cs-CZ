---
title: "aaaAssigning rolí správce ve službě Azure Active Directory | Microsoft Docs"
description: "Roli správce můžete vytvořit nebo upravit uživatele, přiřadit role správců tooothers, resetovat uživatelská hesla, spravovat uživatelské licence nebo spravovat domény. Uživatel, který je přiřazen roli správce má hello stejná oprávnění mezi všechny cloudové služby toowhich vaše organizace předplatila."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7fc27e8e-b55f-4194-9b8f-2e95705fb731
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.reviewer: Vince.Smith
ms.custom: it-pro;
ms.openlocfilehash: 41cddbf311767d9995c99ee386e6d276745dad18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Přiřazení rolí správce v Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-assign-admin-roles-azure-portal.md)
> * [Portál Azure Classic](active-directory-assign-admin-roles.md)
>
>

Pomocí služby Azure Active Directory (Azure AD), můžete určit samostatné správci tooserve různé funkce. Tyto správce bude mít přístup toovarious funkce na hello portál Azure nebo portál Azure classic a v závislosti na jejich role, budou mít toocreate nebo úprava uživatelů, přiřazení role správců tooothers, resetovat hesla uživatelů, správě uživatelských licencí, a Spravujte domény, mimo jiné. Uživatel, který je přiřazen roli správce bude mít hello stejná oprávnění napříč všemi hello cloudové služby, které vaše organizace předplatila, bez ohledu na to, jestli přiřadíte hello role v hello portál Office 365 nebo na hello portál Azure classic, nebo pomocí hello Modul Azure AD pro prostředí Windows PowerShell.

Hello následující role správce jsou k dispozici:

* **Správce fakturace**: může dělat nákupy, spravovat předplatná, spravovat lístky žádostí o podporu a sledovat stav služeb.

* **Správce dodržování předpisů**: uživatelé s touto rolí mají oprávnění pro správu v rámci v hello zabezpečení Office 365 & centru dodržování předpisů a Centrum pro správu Exchange. Další informace "[o rolích správce Office 365](https://support.office.com/en-us/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)."

* **Správce služeb CRM**: uživatelé s touto rolí mají globální oprávnění v rámci aplikace Microsoft CRM Online, když je služba hello, a také možnost toomanage hello lístky podpory a sledovat stav služeb. Další informace v [role Správce služeb Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Správci zařízení**: uživatelé s touto rolí stane správci místního počítače na všech zařízeních Windows 10, které jsou připojené k tooAzure služby Active Directory. Nemají hello možnost toomanage zařízení objekty ve službě Azure Active Directory.

* **Directory čtečky**: to je starší verze role, která je přiřazena toobe tooapplications, která nepodporuje hello [souhlas Framework](active-directory-integrating-applications.md). Neměla být přiřazena tooany uživatele.

* **Účty synchronizace adresáře**: nepoužívejte. Tato role se automaticky přiřadí služba toohello Azure AD Connect a není určena nebo podporována pro jiné použití.

* **Directory zapisovače**: to je starší verze role, která je přiřazena toobe tooapplications, která nepodporuje hello [souhlas Framework](active-directory-integrating-applications.md). Neměla být přiřazena tooany uživatele.

* **Správce služby Exchange**: uživatelé s touto rolí mají globální oprávnění v rámci Microsoft Exchange Online, když je služba hello. Další informace v [role Správce služeb Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Globální správce nebo správce společnosti**: uživatelé s touto rolí mají přístup k funkcím pro správu tooall v Azure Active Directory, jakož i služeb Active Directory, jako je Exchange Online, SharePoint Online, že federate tooAzure a Online Skype pro firmy. zaregistruje klienta Azure Active Directory hello Hello osoba se změní na globálního správce. Jenom globální správci můžete přiřadit další role správců. Ve vaší společnosti může být více než jeden globální správce. Globální správci můžete resetovat heslo hello pro všechny uživatele a všechny ostatní správce.

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API a Azure AD PowerShell je tato role označený "Správce společnosti". Je "Globální správce" v hello [portál Azure](https://portal.azure.com).
  >
  >

* **Pozvání hosta odeslal**: uživatelům v této roli, když hello "Členy můžete pozvat" uživatele je nastavení tooNo, můžete spravovat pozvánek uživatele Azure Active Directory s B2B hosta. Další informace o spolupráci B2B v [o preview spolupráce Azure AD B2B hello](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Neobsahuje žádné další oprávnění.

* **Správce služby Intune**: uživatelé s touto rolí mají globální oprávnění v rámci Microsoft Intune Online, když je služba hello. Kromě toho tato role obsahuje hello možnost toomanage uživatelů a zařízení v zásadách tooassociate pořadí, a také vytvořit a spravovat skupiny.

* **Poštovní schránky správce**: Tato role se používá pouze v rámci Exchange Online e-mailová podpora pro RIM Blackberry zařízení. Pokud vaše organizace nepoužívá Exchange Online e-mailu na zařízeních RIM Blackberry, nepoužívejte tuto roli.

* **Partner podpora vrstvy 1**: nepoužívejte. Tato role se už nepoužívá a bude odebrána z Azure AD v budoucnu hello. Tato role je určená pro malý počet prodej partnery společnosti Microsoft a není určena pro obecné použití.

* **Partner podpory vrstvy 2**: nepoužívejte. Tato role se už nepoužívá a bude odebrána z Azure AD v budoucnu hello. Tato role je určená pro malý počet prodej partnery společnosti Microsoft a není určena pro obecné použití.

* **Heslo správce nebo správce technické podpory**: uživatelé s touto rolí můžete resetovat hesla, spravovat žádosti o služby a sledovat stav služeb. Správci hesel můžou resetovat hesla jenom uživatelům a jiným správcům hesel.

  > [!NOTE]
  > V Microsoft Graph API, Azure AD Graph API a Azure AD PowerShell tato role je označeno "Helpdesk Administrator". Je "heslo správce" v hello [portál Azure](https://portal.azure.com/).
  >
  >
  
* **Správce služby Power BI**: uživatelé s touto rolí mají globální oprávnění v rámci Microsoft Power BI, když je služba hello, a také možnost toomanage hello lístky podpory a sledovat stav služeb. Další informace v [role Správce služeb Office 365](https://support.office.com/en-us/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-001&ad=US).

* **Privilegované Role správce**: uživatelé s touto rolí můžete spravovat přiřazení rolí v Azure Active Directory, a také v rámci Azure AD Privileged Identity Management. Kromě toho tato role umožňuje správu všech aspektů Privileged Identity Management.

* **Správce zabezpečení**: uživatelé s touto rolí mají všechna oprávnění jen pro čtení hello role Čtenář zabezpečení hello plus hello možnost toomanage konfigurace související se zabezpečením služby: Azure Active Directory Identity Protection, Správa privilegovaných identit a zabezpečení Office 365 & centru dodržování předpisů. Další informace o oprávněních Office 365 je k dispozici na [oprávnění v hello Office 365 zabezpečení a dodržování předpisů Center](https://support.office.com/en-us/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Zabezpečení čtečky**: uživatelé s touto rolí mají globální přístup jen pro čtení, včetně všech informací v Azure Active Directory, ochrany identit, Privileged Identity Management, a také hello možnost tooread Azure Active Directory přihlášení sestavy a protokoly auditu. Hello role uděluje také oprávnění jen pro čtení v centru dodržování předpisů a zabezpečení Office 365. Další informace o oprávněních Office 365 je k dispozici na [oprávnění v hello Office 365 zabezpečení a dodržování předpisů Center](https://support.office.com/en-us/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Správce podpory služeb**: uživatelé s touto rolí můžete otevřít žádosti o podporu se společností Microsoft pro služby Azure a Office 365 a zobrazení hello center řídicí panel a zpráv služby v hello portál Azure a portálu pro správu Office 365. Další informace v [role Správce služeb Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Správce služby SharePoint**: uživatelé s touto rolí mají globální oprávnění v rámci služby Microsoft SharePoint Online, když je služba hello, a také možnost toomanage hello lístky podpory a sledovat stav služeb. Další informace v [role Správce služeb Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Skype pro firmy nebo správce služby Lync**: uživatelé s touto rolí mají globální oprávnění v rámci Microsoft Skype pro firmy, když je služba hello, a také spravovat uživatelské atributy specifické pro Skype ve službě Azure Active Directory. Kromě toho tato role uděluje hello možnost toomanage lístky žádostí o podporu a sledování služby stavu. Další informace v [role Správce služeb Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API a Azure AD PowerShell je tato role označený "Správce služby Lync". Je "Skype pro firmy Správce služby" v hello [portál Azure](https://portal.azure.com/).
  >
  >

* **Správce účtu uživatele**: uživatelé k této roli mohou vytvářet a spravovat všechny aspekty uživatelů a skupin. Kromě toho tato role zahrnuje lístky žádostí o hello možnost toomanage podporu a monitorování služby stavu. Platí určitá omezení. Například tato role neumožňuje odstraňování globální správce, a při povolit, změna hesla pro bez oprávnění správce, neumožňuje Změna hesla pro globální správci nebo jiné privilegované správce.

## <a name="administrator-permissions"></a>Oprávnění správce

### <a name="billing-administrator"></a>Správce fakturace

| Můžete provést | Nelze provést |
| --- | --- |
|<p>Zobrazení informací o společnosti a uživatele</p><p>Spravovat lístky žádostí o podporu Office</p><p>Provádět operace fakturace a nákupu produktů Office</p> |<p>Resetovat uživatelská hesla</p><p>Vytvoření a Správa zobrazení uživatele</p><p>Vytvářet, upravovat a odstraňovat uživatele a skupiny a spravovat uživatelské licence</p><p>Spravovat domény</p><p>Spravovat informace o společnosti</p><p>Delegovat role správců tooothers</p><p>Používat synchronizaci adresářů</p><p>Zobrazení protokolů auditování</p>|

### <a name="global-administrator"></a>Globální správce
| Můžete provést | Nelze provést |
| --- | --- |
| <p>Zobrazení informací o společnosti a uživatele</p><p>Spravovat lístky žádostí o podporu Office</p><p>Provádět operace fakturace a nákupu produktů Office</p><p>Resetovat uživatelská hesla</p>
<p>Resetovat hesla jiných správce</p> <p>Vytvoření a Správa zobrazení uživatele</p><p>Vytvářet, upravovat a odstraňovat uživatele a skupiny a spravovat uživatelské licence</p><p>Spravovat domény</p><p>Spravovat informace o společnosti</p><p>Delegovat role správců tooothers</p><p>Používat synchronizaci adresářů</p><p>Povolení nebo zakázání služby Multi-Factor authentication</p><p>Zobrazení protokolů auditování</p> |Není k dispozici |

### <a name="password-administrator"></a>Správce hesel
| Můžete provést | Nelze provést |
| --- | --- |
| <p>Zobrazení informací o společnosti a uživatele</p><p>Spravovat lístky žádostí o podporu Office</p><p>Resetovat uživatelská hesla</p> <p>Resetovat hesla jiných správce</p>|<p>Provádět operace fakturace a nákupu produktů Office</p><p>Vytvoření a Správa zobrazení uživatele</p><p>Vytvářet, upravovat a odstraňovat uživatele a skupiny a spravovat uživatelské licence</p><p>Spravovat domény</p><p>Spravovat informace o společnosti</p><p>Delegovat role správců tooothers</p><p>Používat synchronizaci adresářů</p><p>Zobrazení sestav</p>|

### <a name="service-administrator"></a>Správce služeb
| Můžete provést | Nelze provést |
| --- | --- |
| <p>Zobrazení informací o společnosti a uživatele</p><p>Spravovat lístky žádostí o podporu Office</p> |<p>Resetovat uživatelská hesla</p><p>Provádět operace fakturace a nákupu produktů Office</p><p>Vytvoření a Správa zobrazení uživatele</p><p>Vytvářet, upravovat a odstraňovat uživatele a skupiny a spravovat uživatelské licence</p><p>Spravovat domény</p><p>Spravovat informace o společnosti</p><p>Delegovat role správců tooothers</p><p>Používat synchronizaci adresářů</p><p>Zobrazení protokolů auditování</p> |

### <a name="user-administrator"></a>Správce uživatele
| Můžete provést | Nelze provést |
| --- | --- |
| <p>Zobrazení informací o společnosti a uživatele</p><p>Spravovat lístky žádostí o podporu Office</p><p>Resetujte uživatelská hesla, ale s omezením.</p><p>Resetovat hesla jiných správce</p><p>Resetovat hesla jiných uživatelů</p><p>Vytvoření a Správa zobrazení uživatele</p><p>Vytvořit, upravit a odstraňovat uživatele a skupiny a spravovat uživatelské licence, ale s omezením. Potvrdí nelze odstraňovat globální správce ani vytvářet jiné správce.</p> |<p>Provádět operace fakturace a nákupu produktů Office</p><p>Spravovat domény</p><p>Spravovat informace o společnosti</p><p>Delegovat role správců tooothers</p><p>Používat synchronizaci adresářů</p><p>Povolení nebo zakázání služby Multi-Factor authentication</p><p>Zobrazení protokolů auditování</p> |

### <a name="security-reader"></a>Čtečka zabezpečení
| V | Můžete provést |
| --- | --- |
| Centrum pro ochranu identity |Číst všechny sestavy zabezpečení a informace o nastavení pro funkce zabezpečení<ul><li>Proti spamu<li>Šifrování<li>Zabránění ztrátě dat<li>Proti malwaru<li>Pokročilé threat protection<li>Ochrana proti podvodným<li>Mailflow pravidla |
| Privileged Identity Management |<p>Obsahuje informace o oprávnění jen pro čtení tooall prezentované v Azure AD PIM: zásady a sestav pro přiřazení rolí Azure AD, zkontroluje zabezpečení a v hello číst budoucí přístup k datům toopolicy a sestav pro scénáře kromě přiřazení role Azure AD.<p>**Nelze** zaregistrovat Azure AD PIM nebo se přesvědčte, tooit žádné změny. PIM na portálu nebo pomocí prostředí PowerShell někdo v této roli můžete aktivovat další role (například globální správce nebo správce privilegovaných rolí), pokud uživatel hello kandidátem pro ně. |
| <p>Monitorování stavu služby Office 365</p><p>Centru dodržování předpisů a zabezpečení Office 365</p> |<ul><li>Číst a spravovat výstrahy<li>Zásady zabezpečení pro čtení<li>Číst analýzou hrozeb, Cloud App Discovery a karantény v hledání a prošetřit<li>Číst všechny sestavy |

### <a name="security-administrator"></a>Správce zabezpečení.
| V | Můžete provést |
| --- | --- |
| Centrum pro ochranu identity |<ul><li>Všechna oprávnění role zabezpečení čtečky hello.<li>Kromě toho hello tooperform možnost všechny operace IPC s výjimkou resetování hesla. |
| Privileged Identity Management |<ul><li>Všechna oprávnění role zabezpečení čtečky hello.<li>**Nelze** spravovat členství v rolích Azure AD nebo nastavení. |
| <p>Monitorování stavu služby Office 365</p><p>Centru dodržování předpisů a zabezpečení Office 365 |<ul><li>Všechna oprávnění role zabezpečení čtečky hello.<li>Můžete nakonfigurovat všechna nastavení ve funkci Advanced Threat Protection hello (ochrany proti malwaru a virů, škodlivý konfigurace adresy URL, adresa URL trasování atd.). |

## <a name="details-about-hello-global-administrator-role"></a>Podrobnosti o roli globálního správce hello
Hello globálního správce má přístup tooall funkcím pro správu. Ve výchozím nastavení je přiřazen hello osoba zaregistruje předplatné Azure hello roli globálního správce pro adresář hello. Jenom globální správci můžete přiřadit další role správců.

### <a name="tooadd-a-colleague-as-a-global-administrator"></a>tooadd kolegu o jako globální správce

1. Přihlaste se toohello [Centrum správy služby Azure Active Directory](https://aad.portal.azure.com) pomocí účtu, který je globálním správcem pro adresář tenanta hello.

   ![Otevření centra pro správu azure AD](./media/active-directory-assign-admin-roles-azure-portal/active-directory-admin-center.png)

2. Vyberte **uživatelů a skupin &gt; všichni uživatelé**

3. Najít uživatele hello toodesignate jako globální správce a otevřete okno hello pro daného uživatele.

4. V okně hello uživatele, vyberte **Directory role**.
 
5. V okně role hello adresář, vyberte hello **globálního správce** role a uložte.

## <a name="assign-or-remove-administrator-roles"></a>Přiřazení nebo odebrání role správce
jak zjistit, tooassign správní role tooa uživatele v Azure Active Directory, toolearn [přiřadit uživatele tooadministrator role ve verzi preview služby Azure Active Directory](active-directory-users-assign-role-azure-portal.md).

## <a name="deprecated-roles"></a>Nepoužívané role

Nepoužívejte Hello následující role. Budou se nepoužívá a bude odebrána z Azure AD v budoucnu hello.

* Správce licencí ad hoc
* Tvůrce ověřené uživatele e-mailu
* Připojení zařízení k
* Správce zařízení.
* Uživatelé zařízení
* Připojení zařízení k síti na pracovišti

## <a name="next-steps"></a>Další kroky

* Další informace o toolearn jak správci toochange pro předplatné Azure, najdete v části [jak tooadd nebo změna role Správce služby Azure](../billing-add-change-azure-subscription-administrator.md)
* toolearn Další informace o tom, jak je přístup k prostředkům řídí ve službě Microsoft Azure, najdete v části [Principy přístupu k prostředkům v Azure](active-directory-understanding-resource-access.md)
* Další informace o tom, jak Azure Active Directory má vztah tooyour předplatné Azure, najdete v části [asociování předplatných Azure se službou Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Správa uživatelů](active-directory-create-users.md)
* [Správa hesel](active-directory-manage-passwords.md)
* [Správa skupin](active-directory-manage-groups.md)
