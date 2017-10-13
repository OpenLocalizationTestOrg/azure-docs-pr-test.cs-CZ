---
title: 'Kurz: Azure Active Directory integrace s Jive | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Jive."
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
ms.openlocfilehash: 957b152fdd40d08a867e788b0cb9f7d57ed481e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-jive-for-user-provisioning"></a>Kurz: Konfigurace Jive pro zřizování uživatelů

Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Jive a Azure AD do automaticky zřizovat a deaktivace zřízení uživatelských účtů ze služby Azure AD k Jive.

## <a name="prerequisites"></a>Požadavky

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

*   Klienta služby Azure Active directory.
*   Jive jednotného přihlašování povolené předplatné.
*   Uživatelský účet v Jive s oprávněními správce týmu.

## <a name="assigning-users-to-jive"></a>Přiřazení uživatelů k Jive

Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace. V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.

Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Jive. Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace Jive podle pokynů tady:

[Přiřazení uživatele nebo skupiny do aplikace enterprise](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a>Důležité tipy pro přiřazování uživatelů do Jive

*   Dále je doporučeno jednoho uživatele Azure AD pro Jive přidělí otestovat konfiguraci zřizování. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování Jive uživatele, musíte vybrat platné uživatelské role. Roli "Výchozí přístup" nefunguje pro zřizování.

## <a name="enable-user-provisioning"></a>Povolit zřizování uživatelů

Tato část vás provede připojení k Jive na uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v Jive podle přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP]
> Můžete také povolit na základě SAML jednotné přihlašování pro Jive, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="to-configure-user-account-provisioning"></a>Ke konfiguraci zřizování účtu uživatele:

Cílem této části se popisují postup povolení zřizování uživatelů z uživatelských účtů služby Active Directory pro Jive.
V rámci tohoto postupu jsou nezbytné, které budete muset požádat Jive.com token zabezpečení uživatele.

1. V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali Jive pro jednotné přihlašování, vyhledejte instanci Jive pomocí pole hledání. Jinak vyberte možnost **přidat** a vyhledejte **Jive** v galerii aplikací. Vyberte Jive ve výsledcích hledání a přidejte ji do seznamu aplikací.

3. Vyberte instanci Jive a pak vyberte **zřizování** kartě.

4. Nastavte **režimu zřizování** k **automatické**. 

    ![Zřizování](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. V části **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace:
   
    a. V **uživatelské jméno správce Jive** textovému poli, zadejte název, který má účtu Jive **správce systému** profil v Jive.com přiřazen.
   
    b. V **heslo správce Jive** textovému poli, zadejte heslo pro tento účet.
   
    c. V **URL klienta Jive** textovému poli, zadejte adresu URL Jive klienta.
      
      > [!NOTE]
      > Adresa URL Jive klienta je adresa URL, která je vaše organizace používá k přihlášení do Jive.  
      > Obvykle se adresa URL má následující formát: **www.\< organizace\>. jive.com**.          

6. Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Jive.

7. Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko níže.

8. Klikněte na tlačítko **uložit.**

9. V části mapování vyberte **synchronizaci Azure Active Directory uživatelům Jive.**

10. V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Jive. Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Jive pro operace aktualizace. Kliknutím na tlačítko Uložit potvrzení změny.

11. Povolit zřizování služby pro Jive Azure AD, změňte **Stav zřizování** k **na** v části Nastavení

12. Klikněte na tlačítko **uložit.**

Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k Jive v části Uživatelé a skupiny. Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést. Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci Jive.

Nyní můžete vytvořit testovací účet. Chcete-li ověřit, že účet byly synchronizovány Jive Počkejte až 20 minut.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-jive-tutorial.md)