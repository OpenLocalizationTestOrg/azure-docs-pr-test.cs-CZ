---
title: 'Kurz: Azure Active Directory integrace s pole | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a pole."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: e92baabb174642c22c99e2a30bc9c71845b3b75f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-box-for-automatic-user-provisioning"></a>Kurz: Konfigurace pole pro zřizování automatické uživatelů

cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v poli a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooBox Azure AD.

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory.
*   Pole jednotného přihlašování povolené předplatné.
*   Uživatelský účet v poli s oprávněními správce týmu.

## <a name="assigning-users-toobox"></a>Přiřazení uživatelů tooBox 

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.

Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci Box tooyour. Jakmile se rozhodli, můžete přiřadit aplikaci Box tooyour těchto uživatelů podle pokynů hello tady:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a>Přiřazení uživatelů a skupin
Hello **pole > Uživatelé a skupiny** karta v hello portál Azure vám umožní toospecify, kteří uživatelé a skupiny udělení přístupu tooBox. Přiřazení uživatele nebo skupiny, způsobí, že hello následující toooccur věcí:

* Azure AD umožňuje tooBox tooauthenticate hello přiřadit uživatele (buď přímé přiřazení nebo členství ve skupině). Pokud uživatel není přiřazen, Azure AD nepovoluje je toosign v tooBox a vrátí chybu na stránce přihlášení hello Azure AD.
* Dlaždici aplikace pro pole se přidá uživatele toohello [Spouštěč aplikace](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).
* Pokud je povoleno automatické zřizování, pak hello přiřazené uživatelů nebo skupin se přidají toohello zřizování fronty toobe zřizuje automaticky.
  
  * Pokud pouze uživatelské objekty byly nakonfigurované toobe zřízený, pak všechny přímo přiřazené uživatele jsou umístěny ve frontě zřizování hello a všechny uživatele, kteří jsou členy jakékoli přiřazených skupin jsou umístěny v hello zřizování fronty. 
  * Pokud objekty skupiny byly nakonfigurované toobe zřízený, všechny objekty přiřazené skupiny jsou zřízené tooBox a všechny uživatele, kteří jsou členy těchto skupin. Při zápisu tooBox zůstanou zachovány Hello skupiny a uživatele ve skupinách.

Můžete použít hello **atributy > jednotné přihlašování** kartě tooconfigure, které atributy uživatele (nebo deklarace identity) jsou uvedené tooBox během ověřování na základě SAML a hello **atributy > zřizování** kartě tooconfigure jak toku atributy uživatelů a skupin ze služby Azure AD tooBox při zřizování operace.

### <a name="important-tips-for-assigning-users-toobox"></a>Důležité tipy pro přiřazení uživatelů tooBox 

*   Dále je doporučeno jednoho Azure AD uživatel s přiřazenou tooBox tootest hello zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování toobox uživatele, musíte vybrat platné uživatelské role. role "Výchozí přístup" Hello nefunguje pro zřizování.

## <a name="enable-automated-user-provisioning"></a>Povolit automatické zřizování uživatelů

V této části se provede připojení vaší služby Azure AD tooBox uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v poli podle přiřazení uživatelů a skupin ve službě Azure AD.

Pokud je povoleno automatické zřizování, pak hello přiřazené uživatelů nebo skupin se přidají toohello zřizování fronty toobe zřizuje automaticky.
    
 * Pokud pouze uživatelské objekty jsou nakonfigurované toobe zřízený, pak přímo přiřazené uživatele jsou umístěny v hello zřizování fronty, a všechny uživatele, kteří jsou členy jakékoli přiřazených skupin jsou umístěny v hello zřizování fronty. 
    
 * Pokud objekty skupiny byly nakonfigurované toobe zřízený, všechny objekty přiřazené skupiny jsou zřízené tooBox a všechny uživatele, kteří jsou členy těchto skupin. Při zápisu tooBox zůstanou zachovány Hello skupiny a uživatele ve skupinách.

> [!TIP] 
> Můžete také tooenabled na základě SAML jednotné přihlašování pro pole, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatické uživatel účet zřizování:

Hello cílem této části je toooutline jak tooenable zřizování služby Active Directory uživatelské účty tooBox.

1. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali pole pro jednotné přihlašování, vyhledejte instanci pole s použitím pole hledání hello. Jinak vyberte možnost **přidat** a vyhledejte **pole** v galerii aplikací hello. Vyberte pole ze hello výsledky vyhledávání a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci pole a pak vyberte hello **zřizování** kartě.

4. Sada hello **režimu zřizování** příliš**automatické**. 

    ![Zřizování](./media/active-directory-saas-box-userprovisioning-tutorial/provisioning.png)

5. V části hello **přihlašovací údaje správce** klikněte na tlačítko **Authorize** tooopen přihlašovací dialogové okno pole v novém okně prohlížeče.

6. Na hello **přihlášení toogrant přístup tooBox** zadejte hello vyžaduje pověření a pak klikněte na tlačítko **Authorize**. 
   
    ![Povolit automatické uživatele zajišťování](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "povolit zřizování automatické uživatelů")

7. Klikněte na tlačítko **udělit přístup tooBox** tooauthorize tuto operaci a tooreturn toohello portálu Azure. 
   
    ![Povolit automatické uživatele zajišťování](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "povolit zřizování automatické uživatelů")

8. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour pole aplikace. Pokud hello připojení nezdaří, zkontrolujte účtu pole má oprávnění správce týmu a zkuste to hello **"Ověřit"** krok opakujte.

9. Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.

10. Klikněte na tlačítko **uložit.**

11. V části hello části mapování, vyberte **tooBox synchronizaci uživatelů Azure Active Directory.**

12. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooBox Azure AD. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v poli pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

13. tooenable hello zřizování služby Azure AD pro pole, změna hello **Stav zřizování** příliš**na** v části Nastavení hello

14. Klikněte na tlačítko **uložit.**

Který spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooBox v hello uživatelé a skupiny oddílu. počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci pole.

Nyní můžete vytvořit testovací účet. Počkejte, až minut too20 tooverify, který hello účet byl synchronizován toobox.

Ve vašem klientovi políčko synchronizovaní uživatelé jsou uvedeny v části **spravované uživatele** v hello **konzoly pro správu**.

![Stav integrace](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "stav integrace")


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-box-tutorial.md)