---
title: "Kurz: Konfigurace GitHub pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Informace o konfiguraci Azure Active Directory a automaticky zřizovat a zrušte zřídit uživatelské účty na Githubu."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 7e202899b697872227da4d7bea54406f99089000
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-github-for-automatic-user-provisioning"></a>Kurz: Konfigurace GitHub pro zřizování automatické uživatelů


Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Githubu a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD na Githubu. 

## <a name="prerequisites"></a>Požadavky

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

*   Klienta služby Azure Active directory
*   Github klienta s [obchodní plán](https://help.github.com/articles/organization-billing-plans/#business-plan) nebo lépe povolena. 
*   Uživatelský účet na webu GitHub s oprávnění správce 

> [!NOTE]
> Azure AD zřizování integrace spoléhá na [Githubu SCIM API](https://developer.github.com/v3/scim/), který je k dispozici na Githubu týmy na obchodní plán nebo lepší.

## <a name="assigning-users-to-github"></a>Přiřazení uživatelů ke Githubu

Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD. 

Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Githubu. Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace Githubu podle pokynů tady:

[Přiřazení uživatele nebo skupiny do aplikace enterprise](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-github"></a>Důležité tipy pro přiřazení uživatelů ke Githubu

*   Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke Githubu a otestovat konfiguraci zřizování. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazení uživatele k Githubu, je nutné vybrat buď **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení. **Výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.


## <a name="configuring-user-provisioning-to-github"></a>Konfiguraci zřizování uživatelů na Githubu 

Tato část vás provede připojení k Githubu uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v Githubu podle přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP]
> Můžete také povolit na základě SAML jednotné přihlašování pro GitHub, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.


### <a name="configure-automatic-user-account-provisioning-to-github-in-azure-ad"></a>Konfigurace automatického účet zřizování uživatelů ke Githubu ve službě Azure AD


1. V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali GitHub pro jednotné přihlašování, vyhledejte instanci Githubu pomocí pole hledání. Jinak vyberte možnost **přidat** a vyhledejte **Githubu** v galerii aplikací. Vyberte Githubu ve výsledcích hledání a přidejte ji do seznamu aplikací.

3. Vyberte instanci Githubu a pak vyberte **zřizování** kartě.

4. Nastavte **režimu zřizování** k **automatické**.

    ![Zřizování Githubu](./media/active-directory-saas-github-provisioning-tutorial/GitHub1.png)

5. V části **přihlašovací údaje správce** klikněte na tlačítko **Authorize**. Tato operace otevře dialogové okno Githubu autorizace v nové okno prohlížeče. 

6. V novém okně se přihlaste pomocí účtu správce Githubu. V dialogovém okně výsledné autorizace, vyberte tým Githubu, který chcete povolit zajišťování pro a pak vyberte **Authorize**. Po dokončení se vraťte k portálu Azure k dokončení konfigurace zřizování.

    ![Dialogové okno autorizace](./media/active-directory-saas-github-provisioning-tutorial/GitHub2.png)

7. Na portálu Azure vstupní **URL klienta** a klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Githubu. Pokud se nepovede připojit, ujistěte se, váš účet GitHub má oprávnění správce a **URl klienta** je správně zadané hodnoty, a akci opakujte krok "Ověřit" (mohou představovat **URL klienta** pravidlem: "https://api.github.com/scim/v2/organizations/ + <Organizations_name>", vaší organizace můžete najít v rámci účtu GitHub: **nastavení** > **organizace**).

    ![Dialogové okno autorizace](./media/active-directory-saas-github-provisioning-tutorial/GitHub3.png)

8. Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko "Odesílat e-mailové oznámení, pokud dojde k chybě."

9. Klikněte na **Uložit**. 

10. V části mapování vyberte **synchronizaci Azure Active Directory Users na Githubu**.

11. V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Githubu. Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Githubu pro operace aktualizace. Kliknutím na tlačítko Uložit potvrzení změny.

12. Povolit Azure AD zřizování služby pro GitHub, změňte **Stav zřizování** k **na** v **nastavení** části

13. Klikněte na **Uložit**. 

Tato operace spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené ke Githubu v části Uživatelé a skupiny. Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést. Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby.

Další informace o tom, jak číst zřizování protokoly služby Azure AD najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-enterprise-apps-manage-provisioning.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Další kroky

* [Zjistěte, jak získat sestavy o zřizování aktivity a zkontrolujte protokoly](active-directory-saas-provisioning-reporting.md)
