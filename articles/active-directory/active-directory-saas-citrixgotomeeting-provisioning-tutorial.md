---
title: 'Kurz: Azure Active Directory integrace s Citrix GoToMeeting | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 3c6eed5309dfa384c292b0cf63f8aa58988add81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a>Kurz: Konfigurace systému Citrix GoToMeeting pro zřizování automatické uživatelů

cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Citrix GoToMeeting a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooCitrix GoToMeeting.

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory.
*   Citrix GoToMeeting jednotného přihlašování povolené předplatné.
*   Uživatelský účet v Citrix GoToMeeting s oprávněními správce týmu.

## <a name="assigning-users-toocitrix-gotomeeting"></a>Přiřazení uživatelů tooCitrix GoToMeeting

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.

Než nakonfigurujete a povolíte hello zřizování služby, musíte toodecide jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k tooyour Citrix GoToMeeting aplikace. Jakmile se rozhodli, můžete přiřadit tyto uživatele tooyour aplikace Citrix GoToMeeting podle pokynů hello tady:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toocitrix-gotomeeting"></a>Důležité tipy pro přiřazení uživatelů tooCitrix GoToMeeting

*   Dále je doporučeno jednoho uživatele Azure AD je přiřazen tooCitrix GoToMeeting tootest hello zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování uživatelů tooCitrix GoToMeeting, musíte vybrat platné uživatelské role. role "Výchozí přístup" Hello nefunguje pro zřizování.

## <a name="enable-automated-user-provisioning"></a>Povolit automatické zřizování uživatelů

Tato část vás provede připojením GoToMeeting vaší služby Azure AD tooCitrix uživatelský účet zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v systému Citrix GoToMeeting na základě uživatele a skupiny přiřazení ve službě Azure AD.

> [!TIP]
> Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Citrix GoToMeeting, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatické uživatel účet zřizování:

1. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali Citrix GoToMeeting pro jednotné přihlašování, vyhledejte instanci Citrix GoToMeeting pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **Citrix GoToMeeting** v galerii aplikací hello. Vyberte Citrix GoToMeeting z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci Citrix GoToMeeting a pak vyberte hello **zřizování** kartě.

4. Sada hello **zřizování** režimu příliš**automatické**. 

    ![Zřizování](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. V části hello části přihlašovací údaje správce proveďte následující kroky hello:
   
    a. V hello **uživatelské jméno správce GoToMeeting Citrix** textovému poli, zadejte jméno uživatele hello správce.

    b. V hello **heslo správce systému Citrix GoToMeeting** textovému poli, heslo správce hello.

6. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Citrix GoToMeeting aplikace. Pokud hello připojení nezdaří, zkontrolujte účtu Citrix GoToMeeting má oprávnění správce týmu a zkuste to hello **"Přihlašovací údaje správce"** krok opakujte.

7. Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.

8. Klikněte na tlačítko **uložit.**

9. V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooCitrix GoToMeeting.**

10. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z Azure AD tooCitrix GoToMeeting. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v systému Citrix GoToMeeting pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

11. tooenable hello zřizování služby Azure AD pro Citrix GoToMeeting, změna hello **Stav zřizování** příliš**na** v části Nastavení hello

12. Klikněte na tlačítko **uložit.**

Spustí počáteční synchronizaci hello všechny uživatele nebo skupiny přiřazené tooCitrix GoToMeeting v části Uživatelé a skupiny hello. počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Citrix GoToMeeting.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-citrix-gotomeeting-tutorial.md)


