---
title: "Kurz: Konfigurace síti na pracovišti ve službě Facebook pro zřizování uživatelů | Microsoft Docs"
description: "Zjistěte, jak tooautomatically zřídit a deaktivace zřízení uživatelských účtů ze služby Azure AD tooWorkplace ve službě Facebook."
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
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 33d294dbc8f441b29138408b3c9ca41f2141f8af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a>Kurz: Konfigurace síti na pracovišti ve službě Facebook pro zřizování uživatelů

Tento kurz ukazuje hello kroky nezbytné tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooWorkplace Azure Active Directory (Azure AD) ve službě Facebook.

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD se na pracovišti ve službě Facebook, potřebujete následující hello:

- Předplatné služby Azure AD
- Předplatné povolené firemní síti pomocí sítě Facebook jednotné přihlašování (SSO)

tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [nabídka zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-tooworkplace-by-facebook"></a>Přiřazení uživatelů tooWorkplace ve službě Facebook.

Azure AD používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele jsou synchronizovány pouze hello uživatelů a skupin, které byly přiřazeny tooan aplikace ve službě Azure AD.

Než nakonfigurujete a povolíte hello zřizování služby, rozhodněte, jaké uživatelů a skupin ve službě Azure AD představují hello uživatelů, kteří potřebují přístup k síti na pracovišti tooyour aplikace Facebook. Pak můžete přiřadit tyto uživatele tooyour síti na pracovišti Facebook aplikací pomocí následujících hello pokyny v [přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

>[!IMPORTANT]
>*   Test hello zřizování konfigurace přiřazením jedné tooWorkplace uživatele Azure AD ve službě Facebook. Přiřaďte dalších uživatelů a skupin později.
>*   Když přiřadíte uživatele tooWorkplace ve službě Facebook, musíte vybrat platné uživatelské role. role Hello výchozí přístup nefunguje pro zřizování.

## <a name="enable-automated-user-provisioning"></a>Povolit zřizování automatizované uživatelů

Tato část vás provede připojením účtu uživatele Azure AD toohello zřizování API pracovního místa ve službě Facebook. Také zjistíte, jak tooconfigure hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v síti na pracovišti ve službě Facebook. To je založené na uživatele a přiřazení skupiny ve službě Azure AD.

>[!Tip]
>Můžete také tooenabled na základě SAML jednotného přihlašování pro pracoviště ve službě Facebook, pomocí pokynů hello součástí hello [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují.

### <a name="configure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a>Konfigurace uživatelského účtu ve službě Azure AD zřizování tooWorkplace ve službě Facebook.

Azure AD podporuje možnost tooautomatically hello synchronizovat Podrobnosti účtu hello přiřadit tooWorkplace uživatelů ve službě Facebook. Toto automatické synchronizace umožňuje síti na pracovišti Facebook tooget hello data tooauthorize uživatelů pro přístup, je nutné před je prvním pokusem o toosign v pro hello. Také zrušte technologii uživatelům v síti na pracovišti ve službě Facebook při odvolání přístupu ve službě Azure AD.

1. V hello [portál Azure](https://portal.azure.com), vyberte **Azure Active Directory** > **podnikové aplikace** > **všechny aplikace**.

2. Pokud jste již nakonfigurovali síti na pracovišti ve službě Facebook pro jednotné přihlašování, vyhledejte pomocí pole hledání text hello instanci síti na pracovišti ve službě Facebook. Jinak vyberte možnost **přidat** a vyhledejte **síti na pracovišti ve službě Facebook** v galerii aplikací hello. Vyberte **síti na pracovišti ve službě Facebook** z hello výsledky hledání a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci síti na pracovišti ve službě Facebook a pak vyberte hello **zřizování** kartě.

4. Nastavit **režimu zřizování** příliš**automatické**. 

    ![Snímek obrazovky pracovního místa ve službě Facebook možnosti zřizování](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. V části hello **přihlašovací údaje správce** zadejte hello **tajný klíč tokenu** a hello **URL klienta** ze svého pracoviště správcem sítě Facebook.

6. V hello portálu Azure, vyberte **Test připojení** tooensure Azure AD můžete připojit tooyour síti na pracovišti aplikace Facebook. Pokud hello připojení nezdaří, zkontrolujte, zda vaše pracoviště pomocí účtu sítě Facebook oprávnění správce týmu.

7. Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.

8. Vyberte **Uložit**.

9. V části hello části mapování, vyberte **tooWorkplace synchronizaci uživatelů Azure Active Directory ve službě Facebook**.

10. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z Azure AD tooWorkplace ve službě Facebook. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v síti na pracovišti ve službě Facebook pro operace aktualizace. Vyberte všechny změny toocommit **Uložit**.

11. tooenable hello zřizování služby Azure AD pro pracoviště ve službě Facebook, v hello **nastavení** změňte hello **Stav zřizování** příliš**na**.

12. Vyberte **Uložit**.

Další informace o tom, tooconfigure automatické zřizování, najdete v části [hello Facebook dokumentaci](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).

Nyní můžete vytvořit testovací účet. Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooWorkplace ve službě Facebook.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-facebook-at-work-tutorial.md)

