---
title: Konfigurovat Azure AD Privileged Identity Management | Microsoft Docs
description: "Téma, které vysvětluje, co je Azure AD Privileged Identity managementu a jak používat PIM k zlepšují zabezpečení vašeho cloudu."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: c548ed2e-06e3-4eaf-a63d-0f02ee72da25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: eb7059368cb80be7dd625f9dc6ad2aab1bad709a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Co je Azure AD Privileged Identity Management?
Pomocí aplikace Azure Active Directory (AD) Privileged Identity Management můžete spravovat, řídit a sledovat přístup v rámci organizace. To zahrnuje přístup k prostředkům v Azure AD a dalších online službách Microsoftu jako Office 365 nebo Microsoft Intune.  

> [!NOTE]
> Správa privilegovaných identit je dostupná pro celou organizaci, když licence správce s na edici Premium P2 služby Azure Active Directory. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).

Organizace chcete minimalizovat počet lidí, kteří mají přístup k zabezpečeným informacím nebo prostředkům, protože který snižuje riziko uživatel se zlými úmysly získávání tento přístup. Ale uživatelé stále muset provádět privilegované operace v Azure, Office 365 nebo SaaS aplikace. Organizace poskytnout privilegovaného přístupu uživatelů ve službě Azure AD bez monitorování tyto činnosti uživatelů s jejich oprávněními správce. Azure AD Privileged Identity Management pomáhá vyřešit toto riziko.  

Azure AD Privileged Identity Management vám pomůže:  

* Uvidíte, kteří uživatelé jsou správci služby Azure AD
* Povolit na vyžádání "právě v čase" pro správu přístup k Online službám společnosti Microsoft, jako třeba Office 365 a Intune
* Získání sestavy o historii přístup správce a změny v přiřazení správců
* Získání výstrahy týkající se přístupu k privilegované role.
* Vyžadovat schválení aktivovat (Preview)

Azure AD Privileged Identity Management můžete spravovat integrované organizační role Azure AD, včetně (ale bez omezení):  

* Globální správce
* Správce fakturace
* Správce služeb  
* Správce uživatele
* Správce hesel

## <a name="just-in-time-administrator-access"></a>Právě v přístup správce čas
V minulosti může přiřadit uživatele k roli správce prostřednictvím portálu Azure classic nebo prostředí Windows PowerShell. V důsledku toho stane **trvalé správce**, vždy aktivní v přiřazenou roli. Azure AD Privileged Identity Management zavádí koncepci **oprávněné správce**. Oprávněné admins by měl být uživatelům, kteří potřebují privilegovaného přístupu a poté, ale ne každý den. Role je neaktivní, dokud uživatel potřebuje přístup, pak se dokončit proces aktivace a stane aktivní správce po předem určenou dobu.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Povolit Privileged Identity Management pro svůj adresář
Můžete začít používat Azure AD Privileged Identity Management [portál Azure](https://portal.azure.com/).

> [!NOTE]
> Musí být globální správce s účtem organizace (například @yourdomain.com), nikoli účet Microsoft (například @outlook.com), aby pro adresář Azure AD Privileged Identity Management.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/) jako globální správce adresáře.
2. Pokud má vaše organizace víc než jeden adresář, vyberte své uživatelské jméno v pravém horním rohu webu Azure Portal. Vyberte adresář, kde budete používat Azure AD Privileged Identity Management.
3. Vyberte **Další služby** a pomocí pole Filtr najděte **Azure AD Privileged Identity Management**.
4. Zaškrtněte **Připnout na řídicí panel** a potom klikněte na **Vytvořit**. Aplikace Privileged Identity Management se otevře.

Pokud jste první, kdo bude ve vašem adresáři používat aplikaci Azure AD Privileged Identity Management, [průvodce zabezpečením](active-directory-privileged-identity-management-security-wizard.md) vás provede počátečním přiřazením. Následně se automaticky stane první **správce zabezpečení** a **správce privilegovaných rolí** adresáře.

Pouze správce privilegovaných rolí můžete spravovat přístup pro jiné správce. Můžete [ostatním uživatelům předat, schopnost spravovat v PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-admin-dashboard"></a>Privilegované řídicí panel Správce Identity Management
Azure AD Privileged Identity Manager poskytuje správce řídicího panelu, který poskytuje důležité informace, jako:

* Výstrahy, které odkazují na příležitosti pro zlepšení zabezpečení
* Počet uživatelů, kteří jsou přiřazeni k každé privilegované role  
* Počet oprávněných a trvalých správců
* Graf aktivací privilegované role ve vašem adresáři

![Řídicí panel PIM – snímek obrazovky][2]

## <a name="privileged-role-management"></a>Správa privilegovaných rolí
S Azure AD Privileged Identity Management můžete spravovat správce přidáním nebo odebráním trvalé nebo oprávněné správce ke každé roli.

![Správci přidat nebo odebrat PIM – snímek obrazovky][3]

## <a name="configure-the-role-activation-settings"></a>Konfigurace nastavení aktivace role
Pomocí [nastavení role](active-directory-privileged-identity-management-how-to-change-default-settings.md) můžete konfigurovat vlastnosti aktivace vhodné role, včetně:

* Doba trvání v období aktivace role
* Oznámení o aktivaci role
* Informace, které uživatel musí poskytnout během procesu aktivace role
* Číslo služby lístků nebo incidenty.
* [Požadavky na pracovní postup schválení – náhled](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![Snímek obrazovky nastavení – správce aktivace – PIM][4]

Všimněte si, že se na obrázku tlačítka pro **Multi-Factor Authentication** jsou zakázány. Pro některé, vysoce privilegované role, jsme vícefaktorové ověřování vyžadovat pro zvýšenou ochranu.

## <a name="role-activation"></a>Aktivace role
K [aktivovat roli](active-directory-privileged-identity-management-how-to-activate-role.md), oprávněné správce požadavky časově vázaných "aktivace" pro roli. Aktivace může být požadováno pomocí **aktivovat Moje role** možnost v Azure AD Privileged Identity Management.

Kdo chce aktivovat roli správce musí inicializovat Azure AD Privileged Identity Management na portálu Azure.

Aktivaci role je přizpůsobitelná. V nastavení PIM, můžete zjistit délku aktivace a co informace nutné poskytnout při aktivaci role správce.

![Aktivace role PIM správce žádost – snímek obrazovky][5]

## <a name="review-role-activity"></a>Kontrolní aktivita role
Existují dva způsoby, jak sledovat, jak zaměstnancům a správci používají privilegované role. První možností je použití [historie auditu role Directory](active-directory-privileged-identity-management-how-to-use-audit-log.md). Historie auditu zaznamená sledovat změny v přiřazení privilegované role a role aktivace historie.

![Historie aktivace PIM – snímek obrazovky][6]

Druhou možností je nastavit regular [přístup recenze](active-directory-privileged-identity-management-how-to-start-security-review.md). Tyto recenze přístup může být provádí a přiřazení kontrolor (jako je team manager) nebo zaměstnanci můžete zkontrolovat sami. Toto je nejlepší způsob, jak monitorovat kdo stále vyžaduje přístup a kteří už nepodporuje.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Azure AD PIM na vypršení platnosti odběru
Před dosažení obecné dostupnosti Azure AD PIM byl ve verzi preview a k dispozici nebyly žádné kontroly licence pro klienta pro náhled Azure AD PIM.  Teď, když Azure AD PIM byl dosažen obecné dostupnosti, musí přiřadit licence zkušební nebo placený správcům klienta chcete pokračovat v používání PIM.  Pokud si vaše organizace není koupit Azure AD Premium P2 nebo vypršení zkušební doby, většinou všechny funkce Azure AD PIM bude k dispozici ve vašem klientovi.  Další informace v [požadavků předplatné Azure AD PIM](./privileged-identity-management/subscription-requirements.md)

## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
