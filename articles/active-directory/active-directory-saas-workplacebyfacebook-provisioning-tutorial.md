---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook. | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a síti na pracovišti ve službě Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 551ec353a5ec1da936373587688c299a6f4acca7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a>Kurz: Konfigurace síti na pracovišti ve službě Facebook pro zřizování uživatelů

cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v síti na pracovišti podle Facebook a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooWorkplace ve službě Facebook.

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD se na pracovišti ve službě Facebook, je třeba hello následující položky:

- Předplatné služby Azure AD
- Firemní síti pomocí sítě Facebook jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assigning-users-tooworkplace-by-facebook"></a>Přiřazení uživatelů tooWorkplace ve službě Facebook.

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.

Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k síti na pracovišti tooyour aplikace Facebook. Jakmile se rozhodli, můžete přiřadit tyto uživatele tooyour síti na pracovišti aplikace Facebook podle pokynů hello tady:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooworkplace-by-facebook"></a>Důležité tipy pro přiřazení tooWorkplace uživatelů ve službě Facebook.

*   Dále je doporučeno jednoho uživatele Azure AD je přiřazena tooWorkplace službou Facebook tootest hello zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování tooWorkplace uživatele ve službě Facebook, musíte vybrat platné uživatelské role. role "Výchozí přístup" Hello nefunguje pro zřizování.

## <a name="enable-user-provisioning"></a>Povolit zřizování uživatelů

Tato část vás provede připojení tooWorkplace vaší služby Azure AD, uživatelský účet na Facebooku pro zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v síti na pracovišti ve službě Facebook na základě uživatele a skupiny přiřazení ve službě Azure AD.

>[!Tip]
>Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro pracoviště podle Facebook, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="tooconfigure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a>uživatelský účet tooconfigure zřizování tooWorkplace ve službě Facebook ve službě Azure AD:

Hello cílem této části je toooutline jak tooenable zřizování služby Active Directory uživatelské účty tooWorkplace ve službě Facebook.

Azure AD podporuje možnost tooautomatically hello synchronizovat Podrobnosti účtu hello přiřadit tooWorkplace uživatelů ve službě Facebook. Toto automatické synchronizace umožňuje síti na pracovišti Facebook tooget hello data tooauthorize uživatelů pro přístup, je nutné před je prvním pokusem o toosign v pro hello. Také zrušte technologii uživatelům v síti na pracovišti ve službě Facebook při odvolání přístupu ve službě Azure AD.

1. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory** > **podnikové aplikace** > **všechny aplikace** části.

2. Pokud jste již nakonfigurovali síti na pracovišti ve službě Facebook pro jednotné přihlašování, vyhledejte instanci síti na pracovišti podle Facebook pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **síti na pracovišti ve službě Facebook** v galerii aplikací hello. Vyberte síti na pracovišti ve službě Facebook z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci síti na pracovišti ve službě Facebook a pak vyberte hello **zřizování** kartě.

4. Sada hello **režimu zřizování** příliš**automatické**. 

    ![Zřizování](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. V části hello **přihlašovací údaje správce** části zadejte hello tajný klíč tokenu a hello URL klienta ze svého pracoviště správcem sítě Facebook.

6. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour síti na pracovišti aplikace Facebook. Pokud hello připojení selže, zajistěte, aby že vaše pracoviště Facebook účet má oprávnění správce týmu.

7. Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.

8. Klikněte na tlačítko **uložit.**

9. V části hello části mapování, vyberte **tooWorkplace synchronizaci uživatelů Azure Active Directory ve službě Facebook.**

10. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z Azure AD tooWorkplace ve službě Facebook. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v síti na pracovišti ve službě Facebook pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

11. tooenable hello zřizování služby Azure AD pro pracoviště podle Facebook, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části

12. Klikněte na tlačítko **uložit.**

Další informace o tom, tooconfigure automatické zřizování, najdete v části [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)

Nyní můžete vytvořit testovací účet. Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooWorkplace ve službě Facebook.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-workplacebyfacebook-tutorial.md)

