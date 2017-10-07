---
title: 'Kurz: Azure Active Directory integrace s Dropbox pro firmy | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Dropbox pro firmy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 0fb01eab4f7c6c4516eac64a4343e46ea221f98d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a>Kurz: Konfigurace Dropbox pro firmy pro zřizování automatické uživatelů

cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Dropbox pro firmy a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooDropbox pro firmy.

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory.
*   Dropbox pro obchodní jednotného přihlašování povolené předplatné.
*   Uživatelský účet v Dropbox pro firmy s oprávněními správce týmu.

## <a name="assigning-users-toodropbox-for-business"></a>Přiřazení uživatelů tooDropbox pro firmy

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.

Před konfigurací a povolení hello zřizování služby, musíte toodecide jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k tooyour Dropbox pro obchodní aplikace. Jakmile se rozhodli, můžete přiřadit tyto uživatele tooyour Dropbox pro obchodní aplikace podle pokynů hello tady:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toodropbox-for-business"></a>Důležité tipy pro přiřazení uživatelů tooDropbox pro firmy

*   Dále je doporučeno jednoho uživatele Azure AD je přiřazen tooDropbox pro hello firmy tootest zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování tooDropbox uživatele pro firmy, musíte vybrat platné uživatelské role. role "Výchozí přístup" Hello nefunguje pro zřizování...

## <a name="enable-automated-user-provisioning"></a>Povolit automatické zřizování uživatelů

Tato část vás provede připojením tooDropbox vaší služby Azure AD pro firmy na uživatelský účet zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Dropbox pro firmy na základě uživatele a skupiny přiřazení ve službě Azure AD.

>[!Tip]
>Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Dropbox pro firmy, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatické uživatel účet zřizování:

1. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali Dropbox pro firmy pro jednotné přihlašování, vyhledejte instanci Dropboxu pro firmy pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **Dropbox pro firmy** v galerii aplikací hello. Vyberte Dropbox pro firmy ze hello výsledky vyhledávání a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci Dropboxu pro firmy a pak vyberte hello **zřizování** kartě.

4. Sada hello **režimu zřizování** příliš**automatické**. 

    ![Zřizování](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. V části hello **přihlašovací údaje správce** klikněte na tlačítko **Authorize**. Dropbox pro obchodní přihlašovací dialogové okno otevře v novém okně prohlížeče.

6. Na hello **přihlášení toolink tooDropbox s Azure AD** dialogové okno, přihlášení tooyour Dropbox pro obchodní klienta.

     ![Zřizování uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "zřizování uživatelů")

7. Zkontrolujte, které chcete toogive Azure Active Directory oprávnění toomake změny tooyour Dropbox pro obchodní klienta. Klikněte na tlačítko **povolit**.
    
      ![Zřizování uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "zřizování uživatelů")

8. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Dropbox pro obchodní aplikace. Pokud hello připojení nezdaří, zkontrolujte vaši Dropbox pro obchodní účet má oprávnění správce týmu a zkuste to hello **"Ověřit"** krok opakujte.

9. Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.

10. Klikněte na tlačítko **uložit.**

11. V části hello části mapování, vyberte **tooDropbox synchronizaci uživatelů Azure Active Directory pro firmy.**

12. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z Azure AD tooDropbox pro firmy. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Dropbox pro firmy pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

13. tooenable hello zřizování služby Azure AD pro Dropbox pro firmy, změna hello **Stav zřizování** příliš**na** v části Nastavení hello

14. Klikněte na tlačítko **uložit.**

Spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooDropbox pro firmy v hello uživatelé a skupiny oddílu. počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby na vaší schránky pro obchodní aplikace.

Nyní můžete vytvořit testovací účet. Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooDropbox pro firmy.

Související stav je indikován úspěšně dokončila uživatele zřizování cyklu.

![Přiřazení uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "přiřazení uživatelů")


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-dropboxforbusiness-tutorial.md)