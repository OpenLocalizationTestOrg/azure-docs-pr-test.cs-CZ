---
title: "Kurz: Konfigurace GitHub pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooGitHub."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: c1f0f7a42e4f8a94db3f409cd463e13bb1bc13bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-github-for-automatic-user-provisioning"></a>Kurz: Konfigurace GitHub pro zřizování automatické uživatelů


cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Githubu a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooGitHub Azure AD. 

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory
*   Github klienta s hello [obchodní plán](https://help.github.com/articles/organization-billing-plans/#business-plan) nebo lépe povolena. 
*   Uživatelský účet na webu GitHub s oprávnění správce 

> [!NOTE]
> Hello Azure AD zřizování integrace spoléhá na hello [Githubu SCIM API](https://developer.github.com/v3/scim/), která je k dispozici tooGithub týmy hello firmy plán nebo vyšší.

## <a name="assigning-users-toogithub"></a>Přiřazení uživatelů tooGitHub

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD. 

Než nakonfigurujete a povolíte hello zřizování služby, musíte toodecide jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci tooyour Githubu. Jakmile se rozhodli, můžete přiřadit tyto aplikace Githubu tooyour uživatelů podle pokynů hello zde:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toogithub"></a>Důležité tipy pro přiřazení uživatelů tooGitHub

*   Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooGitHub zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování tooGitHub uživatele, je nutné vybrat buď hello **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení hello. Hello **výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.


## <a name="configuring-user-provisioning-toogithub"></a>Konfigurace tooGitHub zřizování uživatelů 

Tato část vás provede připojením vaší služby Azure AD tooGitHub uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Githubu podle přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP]
> Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro GitHub, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.


### <a name="configure-automatic-user-account-provisioning-toogithub-in-azure-ad"></a>Konfigurace automatického uživatelského účtu zřizování tooGitHub ve službě Azure AD


1. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali GitHub pro jednotné přihlašování, vyhledejte instanci Githubu pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **Githubu** v galerii aplikací hello. Vyberte Githubu z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci Githubu a pak vyberte hello **zřizování** kartě.

4. Sada hello **režimu zřizování** příliš**automatické**.

    ![Zřizování Githubu](./media/active-directory-saas-github-provisioning-tutorial/GitHub1.png)

5. V části hello **přihlašovací údaje správce** klikněte na tlačítko **Authorize**. Tato operace otevře dialogové okno Githubu autorizace v nové okno prohlížeče. 

6. V novém okně hello se přihlaste pomocí účtu správce Githubu. V dialogu autorizace výsledné hello, vyberte hello Githubu týmu, který má být tooenable zřizování pro a potom vyberte **Authorize**. Po dokončení, vrátí toohello Azure portálu toocomplete hello zřizování konfigurace.

    ![Dialogové okno autorizace](./media/active-directory-saas-github-provisioning-tutorial/GitHub2.png)

7. V hello portálu Azure, zadejte **URL klienta** a klikněte na tlačítko **Test připojení** tooensure Azure AD můžete připojit tooyour Githubu aplikaci. Pokud hello připojení nezdaří, ujistěte se, váš účet GitHub má oprávnění správce a **URl klienta** je správně zadané hodnoty a zkuste to znovu "Autorizace" krok text hello (mohou představovat **URL klienta** pravidlem: "https : //api.github.com/scim/v2/organizations/ + < Organizations_name > ", vaší organizace můžete najít v rámci účtu GitHub: **nastavení** > **organizace**).

    ![Dialogové okno autorizace](./media/active-directory-saas-github-provisioning-tutorial/GitHub3.png)

8. Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello "odesílat e-mailové oznámení, pokud dojde k chybě."

9. Klikněte na **Uložit**. 

10. V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooGitHub**.

11. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooGitHub Azure AD. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Githubu pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

12. tooenable hello zřizování služby Azure AD pro GitHub, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části

13. Klikněte na **Uložit**. 

Tato operace spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooGitHub v hello uživatelé a skupiny oddílu. počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizováním služby.

Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-enterprise-apps-manage-provisioning.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Další kroky

* [Zjistěte, jak tooreview protokoly a získat sestavy o zřizování aktivity](active-directory-saas-provisioning-reporting.md)
