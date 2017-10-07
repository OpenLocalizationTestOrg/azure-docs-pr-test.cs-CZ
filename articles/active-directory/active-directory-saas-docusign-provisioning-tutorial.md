---
title: 'Kurz: Azure Active Directory integrace s DocuSign | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a DocuSign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 294cd6b8-74d7-44bc-92bc-020ccd13ff12
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 8562a8f9e05fb72d3331507b7da5c6afee38f9b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-docusign-for-user-provisioning"></a>Kurz: Konfigurace DocuSign pro zřizování uživatelů

cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v DocuSign a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooDocuSign Azure AD.

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory.
*   DocuSign jednotné přihlašování povolené předplatné.
*   Uživatelský účet v DocuSign s oprávněními správce týmu.

## <a name="assigning-users-toodocusign"></a>Přiřazení uživatelů tooDocuSign

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele jsou synchronizovány pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.

Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci DocuSign tooyour. Jakmile se rozhodli, můžete přiřadit tyto aplikace DocuSign tooyour uživatelů podle pokynů hello zde:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toodocusign"></a>Důležité tipy pro přiřazení uživatelů tooDocuSign

*   Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooDocuSign zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování tooDocuSign uživatele, musíte vybrat platné uživatelské role. role "Výchozí přístup" Hello nefunguje pro zřizování.

## <a name="enable-user-provisioning"></a>Povolit zřizování uživatelů

Tato část vás provede připojením vaší služby Azure AD tooDocuSign uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v DocuSign podle přiřazení uživatelů a skupin ve službě Azure AD.

> [!Tip]
> Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro DocuSign, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="tooconfigure-user-account-provisioning"></a>tooconfigure uživatel účet zřizování:

Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele tooDocuSign účty.

1. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali DocuSign pro jednotné přihlašování, vyhledejte instanci DocuSign pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **DocuSign** v galerii aplikací hello. Vyberte DocuSign z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci DocuSign a pak vyberte hello **zřizování** kartě.

4. Sada hello **režimu zřizování** příliš**automatické**. 

    ![Zřizování](./media/active-directory-saas-docusign-provisioning-tutorial/provisioning.png)

5. V části hello **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace hello:
   
    a. V hello **uživatelské jméno správce** textovému poli, typ DocuSign účet název, který má hello **správce systému** profil v DocuSign.com přiřazen.
   
    b. V hello **heslo správce** textovému poli, zadejte hello heslo pro tento účet.

6. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour DocuSign aplikaci.

7. V hello **e-mailové oznámení** zadejte hello e-mailovou adresu uživatele nebo skupiny, kdo by měly dostávat oznámení zřizování chyby a zaškrtněte políčko hello.

8. Klikněte na tlačítko **uložit.**

9. V části hello části mapování, vyberte **tooDocuSign synchronizaci uživatelů Azure Active Directory.**

10. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooDocuSign Azure AD. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v DocuSign pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

11. tooenable hello zřizování služby Azure AD pro DocuSign, změna hello **Stav zřizování** příliš**na** v části Nastavení hello

12. Klikněte na tlačítko **uložit.**

Spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooDocuSign v hello uživatelé a skupiny oddílu. počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci DocuSign.

Nyní můžete vytvořit testovací účet. Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooDocuSign.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-docusign-tutorial.md)