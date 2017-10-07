---
title: 'Kurz: Azure Active Directory integrace s Netsuite | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Netsuite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 5bb2989c1296b9f2abc9e8c84855731adc484aab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>Kurz: Konfigurace Netsuite pro zřizování automatické uživatelů

cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Netsuite a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooNetsuite Azure AD.

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory.
*   Netsuite jednotného přihlašování povolené předplatné.
*   Uživatelský účet v Netsuite s oprávněními správce týmu.

## <a name="assigning-users-toonetsuite"></a>Přiřazení uživatelů tooNetsuite

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele jsou synchronizovány pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.

Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci Netsuite tooyour. Jakmile se rozhodli, můžete přiřadit tyto aplikace Netsuite tooyour uživatelů podle pokynů hello zde:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toonetsuite"></a>Důležité tipy pro přiřazení uživatelů tooNetsuite

*   Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooNetsuite zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování tooNetsuite uživatele, musíte vybrat platné uživatelské role. role "Výchozí přístup" Hello nefunguje pro zřizování.

## <a name="enable-user-provisioning"></a>Povolit zřizování uživatelů

Tato část vás provede připojením vaší služby Azure AD tooNetsuite uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Netsuite podle přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP] 
> Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Netsuite, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="tooconfigure-user-account-provisioning"></a>tooconfigure uživatel účet zřizování:

Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele tooNetsuite účty.

1. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali Netsuite pro jednotné přihlašování, vyhledejte instanci Netsuite pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **Netsuite** v galerii aplikací hello. Vyberte Netsuite z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci Netsuite a pak vyberte hello **zřizování** kartě.

4. Sada hello **režimu zřizování** příliš**automatické**. 

    ![Zřizování](./media/active-directory-saas-netsuite-provisioning-tutorial/provisioning.png)

5. V části hello **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace hello:
   
    a. V hello **uživatelské jméno správce** textovému poli, typ Netsuite účet název, který má hello **správce systému** profil v Netsuite.com přiřazen.
   
    b. V hello **heslo správce** textovému poli, zadejte hello heslo pro tento účet.
      
6. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Netsuite aplikaci.

7. V hello **e-mailové oznámení** zadejte hello e-mailovou adresu uživatele nebo skupiny, kdo by měly dostávat oznámení zřizování chyby a zaškrtněte políčko hello.

8. Klikněte na tlačítko **uložit.**

9. V části hello části mapování, vyberte **tooNetsuite synchronizaci uživatelů Azure Active Directory.**

10. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooNetsuite Azure AD. Všimněte si, že hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Netsuite pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

11. tooenable hello zřizování služby Azure AD pro Netsuite, změna hello **Stav zřizování** příliš**na** v části Nastavení hello

12. Klikněte na tlačítko **uložit.**

Spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooNetsuite v hello uživatelé a skupiny oddílu. Všimněte si, že počáteční synchronizace hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Netsuite.

Nyní můžete vytvořit testovací účet. Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooNetsuite.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-netsuite-tutorial.md)