---
title: "Nastavení samoobslužné správy přístupu k aplikacím ve službě Azure Active Directory | Dokumentace Microsoftu"
description: "Vytváření a správa skupin zabezpečení nebo skupin Office 365 ve službě Azure Active Directory a žádosti o členství ve skupině zabezpečení nebo skupině Office 365"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 904d5c70-c34a-46c4-a9a7-d1efecf4821c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/07/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 7a7370eb076ba8602a58a260a14bb863c55bc803
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-azure-active-directory-for-self-service-group-management"></a>Nastavení služby Azure Active Directory pro samoobslužnou správu skupin
Vaši uživatelé můžou vytvářet a spravovat své vlastní skupiny zabezpečení nebo skupiny Office 365 ve službě Azure Active Directory (Azure AD). Uživatelé mohou také požádat o členství ve skupině zabezpečení nebo ve skupině Office 365. Vlastník skupiny pak může členství schválit nebo odepřít. Každodenní řízení členství ve skupině můžete delegovat na uživatele, kteří chápou obchodní kontext takového členství. Funkce samoobslužné správy skupin jsou dostupné jenom pro skupiny zabezpečení a skupiny Office 365. Nejsou dostupné pro skupiny zabezpečení s povoleným e-mailem a pro distribuční seznamy.

Samoobslužná správa skupin v současné době obsluhuje dva základní scénáře: delegovanou správu skupin a samoobslužnou správu skupin.

* **Delegovaná správa skupin** – příkladem je správce, který spravuje přístup k aplikaci SaaS, kterou používá jeho společnost. Správa těchto přístupových práv je čím dál náročnější, takže správce požádá majitele firmy, aby vytvořil novou skupinu. Správce přiřadí nové skupině přístup k aplikaci a do této skupiny přidá všechny uživatele, kteří už přístup k aplikaci mají. Majitel firmy potom může přidat další uživatele a tito uživatelé budou automaticky přiřazeni k aplikaci. Kvůli správě přístupu pro uživatele tak majitel firmy nemusí čekat na správce. Pokud správce udělí stejné oprávnění správci v jiné firemní skupině, potom tato osoba může spravovat také přístup svých vlastních uživatelů. Majitel firmy ani správce nemůžou zobrazit nebo spravovat uživatele toho druhého. Správce může stále vidět všechny uživatele, kteří mají přístup k aplikaci, a v případě potřeby je může zablokovat.
* **Samoobslužná správa skupin** – příkladem tohoto scénáře jsou dva uživatelé, kteří mají nezávisle nastavené weby SharePoint Online. Oba chtějí týmu toho druhého poskytnout přístup na svůj web. Aby toho dosáhli, můžou vytvořit jednu skupinu v Azure AD a potom v SharePointu Online každý z nich vybere tu skupinu, které chce poskytnout přístup na svůj web. Pokud chce uživatel získat přístup, požádá o něj na přístupovém panelu a po schválení získá přístup k oběma webům SharePoint Online automaticky. Později se jeden z nich rozhodne, že všichni uživatelé s přístupem k webu by měli získat přístup i k určité aplikaci SaaS. Správce aplikace SaaS může přidat přístupová práva k aplikacím na web SharePoint Online. Od toho okamžiku získají veškeré jím schválené žádosti přístup na oba weby SharePoint Online a také k této aplikaci SaaS.

## <a name="make-a-group-available-for-user-self-service"></a>Zpřístupnění skupiny pro uživatelskou samoobsluhu
1. Přihlaste se k [centru pro správu Azure AD](https://aad.portal.azure.com) pomocí účtu, který má k adresáři oprávnění globálního správce.
2. Vyberte **Uživatelé a skupiny** a pak vyberte **Nastavení skupin**.
3. Nastavte možnost **Samoobslužná správa skupin povolena** na **Ano**.
4. Nastavte možnost **Uživatelé můžou vytvářet skupiny zabezpečení** nebo **Uživatelé můžou vytvářet skupiny Office 365** na **Ano**.
  * Pokud jsou tato nastavení povolena, všichni uživatelé ve vašem adresáři mají povoleno vytvářet nové skupiny zabezpečení a přidávat do těchto skupin členy. Tyto nové skupiny se zobrazí také všem ostatním uživatelům na přístupovém panelu. Pokud to nastavení zásad skupiny umožňuje, můžou ostatní uživatelé vytvářet žádosti o připojení k těmto skupinám. 
  * Pokud jsou tato nastavení zakázána, uživatelé nemůžou vytvářet skupiny a nemůžou měnit existující skupiny, ve kterých figurují jako vlastníci. Můžou však stále spravovat členství v těchto skupinách a schvalovat žádosti o členství od jiných uživatelů.

Pomocí možností **Uživatelé, kteří můžou spravovat skupiny zabezpečení** a **Uživatelé, kteří můžou spravovat skupiny Office 365** můžete také zajistit podrobnější řízení přístupu, který vám samoobslužná správa skupin nenabídne. Pokud je povolená možnost **Uživatelé můžou vytvářet skupiny**, mají všichni uživatelé ve vašem tenantovi povoleno vytvářet nové skupiny a přidávat do těchto skupin členy. Nastavením těchto možností na **Někteří** omezíte správu skupin pouze na omezenou skupinu uživatelů. Pokud je tento přepínač nastavený na možnost **Někteří**, musíte uživatele nejprve přidat do skupiny SSGMSecurityGroupsUsers a teprve potom budou tito uživatelé moct vytvářet nové skupiny a přidávat do nich členy. Nastavením možností **Uživatelé, kteří můžou využívat samoobslužné funkce pro skupiny zabezpečení** a **Uživatelé, kteří můžou spravovat skupiny Office 365** na **Všichni** povolíte všem uživatelům ve vašem tenantovi vytvářet nové skupiny.

Pomocí možnosti **Skupina, která může spravovat skupiny zabezpečení** nebo **Skupina, která může spravovat skupiny Office 365** můžete také určit jednu skupinu, jejíž členové můžou používat samoobslužné funkce.

## <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrace místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
