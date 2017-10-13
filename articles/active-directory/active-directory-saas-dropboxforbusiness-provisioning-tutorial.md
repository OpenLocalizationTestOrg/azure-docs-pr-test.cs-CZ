---
title: 'Kurz: Azure Active Directory integrace s Dropbox pro firmy | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Dropbox pro firmy."
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
ms.openlocfilehash: 6f7616e47322242f01a13d763f71c93d4ac06a92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a>Kurz: Konfigurace Dropbox pro firmy pro zřizování automatické uživatelů

Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Dropbox pro firmy s Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do Dropboxu pro firmy.

## <a name="prerequisites"></a>Požadavky

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

*   Klienta služby Azure Active directory.
*   Dropbox pro obchodní jednotného přihlašování povolené předplatné.
*   Uživatelský účet v Dropbox pro firmy s oprávněními správce týmu.

## <a name="assigning-users-to-dropbox-for-business"></a>Přiřazování uživatelů do Dropboxu pro firmy

Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.

Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší schránky pro obchodní aplikace. Jakmile se rozhodli, můžete přiřadit tyto uživatele do vaší schránky pro obchodní aplikace podle pokynů tady:

[Přiřazení uživatele nebo skupiny do aplikace enterprise](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a>Důležité tipy pro přiřazování uživatelů do Dropboxu pro firmy

*   Dále je doporučeno jednoho uživatele Azure AD je přiřazen do Dropboxu pro firmy k testování této konfigurace zřizování. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování uživatele Dropbox pro firmy, musíte vybrat platné uživatelské role. Roli "Výchozí přístup" nefunguje pro zřizování...

## <a name="enable-automated-user-provisioning"></a>Povolit automatické zřizování uživatelů

Tato část vás provede připojení služby Azure AD k Dropboxu pro firmy na uživatelský účet zřizování rozhraní API a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakázat přiřazené uživatelské účty v Dropbox pro firmy na základě uživatele a skupiny přiřazení ve službě Azure AD.

>[!Tip]
>Také můžete povolit na základě SAML jednotné přihlašování pro Dropbox pro firmy, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="to-configure-automatic-user-account-provisioning"></a>Konfigurace automatického uživatele zřizování účtu:

1. V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali Dropbox pro firmy pro jednotné přihlašování, vyhledejte instanci Dropboxu pro firmy pomocí pole hledání. Jinak vyberte možnost **přidat** a vyhledejte **Dropbox pro firmy** v galerii aplikací. Ve výsledcích hledání vyberte Dropbox pro firmy a přidat do seznamu aplikací.

3. Vyberte instanci Dropboxu pro firmy a pak vyberte **zřizování** kartě.

4. Nastavte **režimu zřizování** k **automatické**. 

    ![Zřizování](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. V části **přihlašovací údaje správce** klikněte na tlačítko **Authorize**. Dropbox pro obchodní přihlašovací dialogové okno otevře v novém okně prohlížeče.

6. Na **přihlášení do Dropboxu k propojení s Azure AD** dialogové okno, přihlášení do vaší schránky pro obchodní klienta.

     ![Zřizování uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "zřizování uživatelů")

7. Potvrďte, že byste chtěli poskytnout Azure Active Directory oprávnění k provádění změn do vaší schránky pro obchodní klienta. Klikněte na tlačítko **povolit**.
    
      ![Zřizování uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "zřizování uživatelů")

8. Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k vaší schránky pro obchodní aplikace. Pokud připojení nezdaří, zkontrolujte vaši Dropbox pro obchodní účet má oprávnění správce týmu a zkuste to **"Ověřit"** krok opakujte.

9. Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.

10. Klikněte na tlačítko **uložit.**

11. V části mapování vyberte **synchronizaci Azure Active Directory Users na Dropbox pro firmy.**

12. V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Dropbox pro firmy. Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Dropbox pro firmy pro operace aktualizace. Kliknutím na tlačítko Uložit potvrzení změny.

13. Povolit Azure AD zřizování služby pro Dropbox pro firmy, změňte **Stav zřizování** k **na** v části Nastavení

14. Klikněte na tlačítko **uložit.**

Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k Dropboxu pro firmy v části Uživatelé a skupiny. Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést. Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce, které provádí službu zřizování na vaší schránky pro obchodní aplikace.

Nyní můžete vytvořit testovací účet. Chcete-li ověřit, že účet umístění byl synchronizován do Dropboxu pro firmy Počkejte až 20 minut.

Související stav je indikován úspěšně dokončila uživatele zřizování cyklu.

![Přiřazení uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "přiřazení uživatelů")


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-dropboxforbusiness-tutorial.md)