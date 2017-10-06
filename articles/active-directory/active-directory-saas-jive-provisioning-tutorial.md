---
title: 'Kurz: Azure Active Directory integrace s Jive | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Jive."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: b1c0d0bc2d79427c055f577fe5f9d30d10f1bbdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-jive-for-user-provisioning"></a>Kurz: Konfigurace Jive pro zřizování uživatelů

cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Jive a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooJive Azure AD.

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory.
*   Jive jednotného přihlašování povolené předplatné.
*   Uživatelský účet v Jive s oprávněními správce týmu.

## <a name="assigning-users-toojive"></a>Přiřazení uživatelů tooJive

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.

Před konfigurací a povolení hello zřizování služby, musíte toodecide jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k aplikaci Jive tooyour. Jakmile se rozhodli, můžete přiřadit tyto aplikace Jive tooyour uživatelů podle pokynů hello zde:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toojive"></a>Důležité tipy pro přiřazení uživatelů tooJive

*   Dále je doporučeno jednoho uživatele Azure AD přiřadit hello tootest tooJive zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování tooJive uživatele, musíte vybrat platné uživatelské role. role "Výchozí přístup" Hello nefunguje pro zřizování.

## <a name="enable-user-provisioning"></a>Povolit zřizování uživatelů

Tato část vás provede připojením vaší služby Azure AD tooJive uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Jive podle přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP]
> Můžete také tooenabled na základě SAML jednotné přihlašování pro Jive, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="tooconfigure-user-account-provisioning"></a>tooconfigure uživatel účet zřizování:

Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele tooJive účty.
V rámci tohoto postupu jsou požadované tooprovide potřebujete toorequest z Jive.com token zabezpečení uživatele.

1. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali Jive pro jednotné přihlašování, vyhledejte instanci Jive pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **Jive** v galerii aplikací hello. Vyberte Jive z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci Jive a pak vyberte hello **zřizování** kartě.

4. Sada hello **režimu zřizování** příliš**automatické**. 

    ![Zřizování](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. V části hello **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace hello:
   
    a. V hello **uživatelské jméno správce Jive** textovému poli, typ Jive účet název, který má hello **správce systému** profil v Jive.com přiřazen.
   
    b. V hello **heslo správce Jive** textovému poli, zadejte hello heslo pro tento účet.
   
    c. V hello **URL klienta Jive** textovému poli, URL typu hello Jive klienta.
      
      > [!NOTE]
      > Hello Jive klienta adresa URL je adresa URL, která se používá ve vaší organizaci toolog v tooJive.  
      > Obvykle hello adresa URL má hello následující formát: **www.\< organizace\>. jive.com**.          

6. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Jive aplikaci.

7. Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello níže.

8. Klikněte na tlačítko **uložit.**

9. V části hello části mapování, vyberte **tooJive synchronizaci uživatelů Azure Active Directory.**

10. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooJive Azure AD. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Jive pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

11. tooenable hello zřizování služby Azure AD pro Jive, změna hello **Stav zřizování** příliš**na** v části Nastavení hello

12. Klikněte na tlačítko **uložit.**

Spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooJive v hello uživatelé a skupiny oddílu. počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Jive.

Nyní můžete vytvořit testovací účet. Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooJive.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-jive-tutorial.md)