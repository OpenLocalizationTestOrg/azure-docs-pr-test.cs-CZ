---
title: aaaConfigure Azure AD Privileged Identity managementu | Microsoft Docs
description: "Téma, které vysvětluje, co je Azure AD Privileged Identity managementu a jak toouse PIM tooimprove zabezpečení vašeho cloudu."
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
ms.openlocfilehash: dbe49fe4a0f6e5b46ed5a17fc7e8dcdacafe3846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Co je Azure AD Privileged Identity Management?
Pomocí aplikace Azure Active Directory (AD) Privileged Identity Management můžete spravovat, řídit a sledovat přístup v rámci organizace. To zahrnuje přístup tooresources ve službě Azure AD a dalších služeb Microsoft online services jako je Office 365 nebo Microsoft Intune.  

> [!NOTE]
> Privileged Identity Management je k dispozici tooyour celé organizace, když licence správce s hello Premium P2 edice služby Azure Active Directory. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).

Organizace má toominimize hello počet lidem, kteří mají přístup k informacím toosecure nebo prostředky, protože který snižuje pravděpodobnost, že uživatel se zlými úmysly získávání tento přístup hello. Uživatelé musí však stále toocarry out privilegované operace v Azure, Office 365 nebo SaaS aplikace. Organizace poskytnout privilegovaného přístupu uživatelů ve službě Azure AD bez monitorování tyto činnosti uživatelů s jejich oprávněními správce. Azure AD Privileged Identity Management pomáhá tooresolve toto riziko.  

Azure AD Privileged Identity Management vám pomůže:  

* Uvidíte, kteří uživatelé jsou správci služby Azure AD
* Povolit na vyžádání "právě v čase" tooMicrosoft přístup pro správu Online služeb, jako třeba Office 365 a Intune
* Získání sestavy o historii přístup správce a změny v přiřazení správců
* Dostávat upozornění na přístup tooa privilegované role
* Vyžadovat schválení tooactivate (Preview)

Azure AD Privileged Identity Management může spravovat organizační role hello předdefinované Azure AD, včetně (ale bez omezení):  

* Globální správce
* Správce fakturace
* Správce služeb  
* Správce uživatele
* Správce hesel

## <a name="just-in-time-administrator-access"></a>Právě v přístup správce čas
Je v minulosti přiřadit role správce uživatele tooan prostřednictvím hello portál Azure classic nebo prostředí Windows PowerShell. V důsledku toho stane **trvalé správce**, vždy aktivní hello přiřadit role. Azure AD Privileged Identity Management zavádí koncepci hello **oprávněné správce**. Oprávněné admins by měl být uživatelům, kteří potřebují privilegovaného přístupu a poté, ale ne každý den. Hello role je neaktivní, dokud hello uživatel potřebuje přístup, pak se dokončit proces aktivace a stane aktivní správce po předem určenou dobu.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Povolit Privileged Identity Management pro svůj adresář
Můžete začít používat Azure AD Privileged Identity Management v hello [portál Azure](https://portal.azure.com/).

> [!NOTE]
> Musí být globální správce s účtem organizace (například @yourdomain.com), nikoli účet Microsoft (například @outlook.com), tooenable pro adresář Azure AD Privileged Identity Management.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/) jako globální správce adresáře.
2. Pokud má vaše organizace více než jeden adresář, vyberte své uživatelské jméno v hello pravém horním rohu hello portálu Azure. Vyberte adresář hello, kde budete používat Azure AD Privileged Identity Management.
3. Vyberte **další služby** a používat hello filtru textbox toosearch pro **Azure AD Privileged Identity Management**.
4. Zkontrolujte **Pin toodashboard** a pak klikněte na **vytvořit**. Otevře se Hello aplikace Privileged Identity Management.

Pokud jste hello první, kdo toouse Azure AD Privileged Identity Management ve vašem adresáři, pak hello [Průvodce zabezpečení](active-directory-privileged-identity-management-security-wizard.md) vás provede počátečním přiřazením hello. Poté můžete automaticky stane hello nejprve **správce zabezpečení** a **správce privilegovaných rolí** hello adresáře.

Pouze správce privilegovaných rolí můžete spravovat přístup pro jiné správce. Můžete [poskytnout další uživatelé hello možnost toomanage v PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-admin-dashboard"></a>Privilegované řídicí panel Správce Identity Management
Azure AD Privileged Identity Manager poskytuje správce řídicího panelu, který poskytuje důležité informace, jako:

* Výstrahy, které odkazují na příležitosti tooimprove zabezpečení
* Hello počet uživatelů, kteří jsou přiřazeni tooeach privilegované role  
* Hello počet oprávněných a trvalých správců
* Graf aktivací privilegované role ve vašem adresáři

![Řídicí panel PIM – snímek obrazovky][2]

## <a name="privileged-role-management"></a>Správa privilegovaných rolí
S Azure AD Privileged Identity Management můžete spravovat správce hello přidáním nebo odebráním rolí tooeach trvalé nebo oprávněné správce.

![Správci přidat nebo odebrat PIM – snímek obrazovky][3]

## <a name="configure-hello-role-activation-settings"></a>Konfigurace nastavení aktivace role hello
Pomocí hello [nastavení role](active-directory-privileged-identity-management-how-to-change-default-settings.md) můžete nakonfigurovat vlastnosti aktivace vhodné role hello včetně:

* Hello trvání hello období aktivace role
* oznámení o aktivaci role Hello
* Hello informace, které uživatel potřebuje tooprovide během procesu aktivace role hello
* Číslo služby lístků nebo incidenty.
* [Požadavky na pracovní postup schválení – náhled](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![Snímek obrazovky nastavení – správce aktivace – PIM][4]

Všimněte si, že obrázek hello hello tlačítka pro **Multi-Factor Authentication** jsou zakázány. Pro některé, vysoce privilegované role, jsme vícefaktorové ověřování vyžadovat pro zvýšenou ochranu.

## <a name="role-activation"></a>Aktivace role
příliš[aktivovat roli](active-directory-privileged-identity-management-how-to-activate-role.md), požadavky časově vázaných "aktivace" pro roli hello oprávněné správce. Hello aktivace může být požadováno pomocí hello **aktivovat Moje role** možnost v Azure AD Privileged Identity Management.

Kdo chce tooactivate roli správce musí tooinitialize Azure AD Privileged Identity Management hello portálu Azure.

Aktivaci role je přizpůsobitelná. V nastavení PIM hello můžete určit hello délka hello aktivace a jaké informace Dobrý den, Správce musí tooprovide tooactivate hello role.

![Aktivace role PIM správce žádost – snímek obrazovky][5]

## <a name="review-role-activity"></a>Kontrolní aktivita role
Existují dva způsoby tootrack jak zaměstnanci a správci používají privilegované role. první možnost Hello používá [historie auditu role Directory](active-directory-privileged-identity-management-how-to-use-audit-log.md). historie auditu Hello zaznamená sledovat změny v přiřazení privilegované role a role aktivace historie.

![Historie aktivace PIM – snímek obrazovky][6]

Hello druhou možností je tooset až regular [přístup recenze](active-directory-privileged-identity-management-how-to-start-security-review.md). Tyto recenze přístup může být provádí a přiřazení kontrolor (jako je team manager) nebo hello zaměstnanci můžete zkontrolovat sami. Toto je hello nejlepší způsob, jak toomonitor kdo stále vyžaduje přístup a kteří už nepodporuje.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Azure AD PIM na vypršení platnosti odběru
Předchozí tooreaching obecné dostupnosti Azure AD PIM byl ve verzi preview a nebyly k dispozici žádná licence zkontroluje toopreview klienta Azure AD PIM.  Teď, když Azure AD PIM byl dosažen obecné dostupnosti, musí být přiřazeni správci toohello toocontinue hello klienta pomocí PIM zkušební nebo placené licence.  Pokud si vaše organizace není koupit Azure AD Premium P2 nebo vypršení zkušební doby, většinou všechny funkce Azure AD PIM hello bude k dispozici ve vašem klientovi.  Další informace v hello [požadavků předplatné Azure AD PIM](./privileged-identity-management/subscription-requirements.md)

## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
