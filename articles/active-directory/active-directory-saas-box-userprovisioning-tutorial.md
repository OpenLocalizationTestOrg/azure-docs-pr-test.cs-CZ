---
title: 'Kurz: Azure Active Directory integrace s pole | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a pole."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 0f13fa47ef8eacd822f5744b9dde2bb1f0f2909f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-box-for-automatic-user-provisioning"></a>Kurz: Konfigurace pole pro zřizování automatické uživatelů

Cílem tohoto kurzu je zobrazit kroky že nutné k provedení v poli a Azure AD automaticky zřizovat a deaktivace zřízení uživatelských účtů ze služby Azure AD pole.

## <a name="prerequisites"></a>Požadavky

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

*   Klienta služby Azure Active directory.
*   Pole jednotného přihlašování povolené předplatné.
*   Uživatelský účet v poli s oprávněními správce týmu.

## <a name="assigning-users-to-box"></a>Přiřazování uživatelů do pole 

Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.

Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci pole. Jakmile se rozhodli, můžete přiřadit tyto uživatele do pole aplikace podle pokynů tady:

[Přiřazení uživatele nebo skupiny do aplikace enterprise](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a>Přiřazení uživatelů a skupin
**Pole > Uživatelé a skupiny** na portálu Azure umožňuje určit, kteří uživatelé a skupiny musí mít udělen přístup k poli. Přiřazení uživatele nebo skupiny, způsobí, že následující kroky, kterými dojít:

* Azure AD umožňuje přiřazený uživatel (buď přímé přiřazení nebo členství ve skupině) k ověření pole. Pokud uživatel není přiřazen, Azure AD je pro přihlášení k pole nepovoluje a vrátí chybu na stránce pro přihlášení k Azure AD.
* Dlaždici aplikace pro pole se přidá do uživatele [Spouštěč aplikace](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).
* Pokud je povoleno automatické zřizování, pak přiřazené uživatele nebo skupin se přidají do zřizování fronty automaticky zřídit.
  
  * Pokud pouze uživatelské objekty byly nakonfigurovány zřídit, pak všechny přímo přiřazené uživatele jsou umístěny ve frontě zřizování a všechny uživatele, kteří jsou členy jakékoli přiřazených skupin jsou umístěny ve frontě zřizování. 
  * Pokud objekty skupiny byly nakonfigurovány zřídit, jsou všechny objekty přiřazené skupiny zřízené pole a všechny uživatele, kteří jsou členy těchto skupin. Členství ve skupině a uživateli se zachovají při zápisu do pole.

Můžete použít **atributy > jednotné přihlašování** karta konfigurace, které atributy uživatele (nebo deklarace identity) jsou uvedené pole během ověřování na základě SAML a **atributy > zřizování** kartu a nakonfigurovat způsob, jakým se atributy uživatelů a skupin probíhá z Azure AD pole během zřizování operace.

### <a name="important-tips-for-assigning-users-to-box"></a>Důležité tipy pro přiřazování uživatelů do pole 

*   Dále je doporučeno jednoho přiřazeného pole testování zřizování konfigurace uživatele Azure AD. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování uživatele pole, musíte vybrat platné uživatelské role. Roli "Výchozí přístup" nefunguje pro zřizování.

## <a name="enable-automated-user-provisioning"></a>Povolit automatické zřizování uživatelů

Tato část provede připojení k pole uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v poli podle přiřazení uživatelů a skupin ve službě Azure AD.

Pokud je povoleno automatické zřizování, pak přiřazené uživatele nebo skupin se přidají do zřizování fronty automaticky zřídit.
    
 * Pokud pouze uživatelské objekty jsou nakonfigurované zřídit, pak přímo přiřazené uživatele jsou umístěny v zřizování fronty, a všechny uživatele, kteří jsou členy jakékoli přiřazených skupin jsou umístěny ve frontě zřizování. 
    
 * Pokud objekty skupiny byly nakonfigurovány zřídit, jsou všechny objekty přiřazené skupiny zřízené pole a všechny uživatele, kteří jsou členy těchto skupin. Členství ve skupině a uživateli se zachovají při zápisu do pole.

> [!TIP] 
> Můžete také povolit na základě SAML jednotné přihlašování pro pole, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="to-configure-automatic-user-account-provisioning"></a>Konfigurace automatického uživatele zřizování účtu:

Cílem této části se popisují postup povolení zřizování uživatelských účtů služby Active Directory do pole.

1. V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali pole pro jednotné přihlašování, vyhledejte instanci pole s použitím pole hledání. Jinak vyberte možnost **přidat** a vyhledejte **pole** v galerii aplikací. Vyberte pole ve výsledcích hledání a přidejte ji do seznamu aplikací.

3. Vyberte instanci pole a pak vyberte **zřizování** kartě.

4. Nastavte **režimu zřizování** k **automatické**. 

    ![Zřizování](./media/active-directory-saas-box-userprovisioning-tutorial/provisioning.png)

5. V části **přihlašovací údaje správce** klikněte na tlačítko **Autorizovat** otevřete dialogové okno přihlášení pole v novém okně prohlížeče.

6. Na **přihlášení k udělení přístupu k poli** zadejte požadované pověření a pak klikněte na tlačítko **Authorize**. 
   
    ![Povolit automatické uživatele zajišťování](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "povolit zřizování automatické uživatelů")

7. Klikněte na tlačítko **udělit přístup k poli** autorizovat tuto operaci a vrátit na portál Azure. 
   
    ![Povolit automatické uživatele zajišťování](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "povolit zřizování automatické uživatelů")

8. Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k vaší aplikaci Box. Pokud připojení nezdaří, zkontrolujte účtu pole má oprávnění správce týmu a zkuste to **"Ověřit"** krok opakujte.

9. Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.

10. Klikněte na tlačítko **uložit.**

11. V části mapování vyberte **synchronizaci Azure Active Directory Users pole.**

12. V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD pole. Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v poli pro operace aktualizace. Kliknutím na tlačítko Uložit potvrzení změny.

13. Povolit Azure AD zřizování služby pro pole, změňte **Stav zřizování** k **na** v části Nastavení

14. Klikněte na tlačítko **uložit.**

Která spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené pole v části Uživatelé a skupiny. Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést. Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce, které provádí službu zřizování na pole aplikace.

Nyní můžete vytvořit testovací účet. Chcete-li ověřit, že účet byl synchronizován čekat až 20 minut pole.

Ve vašem klientovi políčko synchronizovaní uživatelé jsou uvedeny v části **spravované uživatele** v **konzoly pro správu**.

![Stav integrace](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "stav integrace")


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-box-tutorial.md)