---
title: "Samoobslužné resetování hesla Azure AD z přihlašovací obrazovky Windows 10 | Dokumentace Microsoftu"
description: "Konfigurace resetování hesla Azure AD na přihlašovací obrazovce Windows 10 a nápověda k zapomenutému kódu PIN"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: d38d2790ae73d147209bb0ccdbbbe20bf880daa4
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="azure-ad-password-reset-from-the-login-screen"></a>Resetování hesla Azure AD z přihlašovací obrazovky

Už jste nasadili samoobslužné resetování hesla Azure AD, ale vaši uživatelé stále volají na helpdesk, když zapomenou své heslo. Volají na helpdesk, protože nemají přístup k webovému prohlížeči, aby se dostali k samoobslužnému resetování hesla.

V nové aktualizaci Windows 10 Fall Creators Update se uživatelům se zařízeními připojenými k Azure AD zobrazí na přihlašovací obrazovce odkaz na resetování hesla. Když na tento odkaz kliknou, přesměrují se na stejné prostředí samoobslužného resetování hesla, které už znají.

Pokud chcete uživatelům povolit resetování hesla Azure AD z přihlašovací obrazovky Windows 10, je nutné splnit následující požadavky:

* Klient Windows 10 verze 1709 nebo novější [připojený k Azure AD](device-management-azure-portal.md).
* Samoobslužné resetování hesla Azure AD musí být povolené.
* Některou z následujících metod nakonfigurujte a nasaďte nastavení, které povolí odkaz na resetování hesla:
   * [Profil konfigurace zařízení Intune](active-directory-passwords-login.md#configure-reset-password-link-using-intune)
   * [Klíč registru](active-directory-passwords-login.md#configure-reset-password-link-using-the-registry)

## <a name="configure-reset-password-link-using-intune"></a>Konfigurace odkazu na resetování hesla pomocí Intune

### <a name="create-a-device-configuration-policy-in-intune"></a>Vytvoření zásad konfigurace zařízení v Intune

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) a klikněte na **Intune**.
2. Vytvořte nový profil konfigurace zařízení tak, že přejdete do **Konfigurace zařízení** > **Profily** > **Vytvořit profil**.
   * Zadejte výstižný název profilu.
   * Volitelně zadejte výstižný popis profilu.
   * Jako platformu vyberte **Windows 10 a novější**.
   * Jako typ profilu vyberte **Vlastní**.

   ![Vytvoření profilu][CreateProfile]

3. Nakonfigurujte **Nastavení**.
   * **Přidejte** následující Nastavení identifikátoru OMA-URI, které povolí odkaz na resetování hesla.
      * Zadejte výstižný název vysvětlující, co toto nastavení dělá.
      * Volitelně zadejte výstižný popis nastavení.
      * Identifikátor **OMA-URI** nastavte na `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset`.
      * **Datový typ** nastavte na **Integer**.
      * Jako **Hodnota** nastavte **1**.
      * Klikněte na tlačítko **OK**.
   * Klikněte na tlačítko **OK**.
4. Klikněte na **Vytvořit**

### <a name="assign-a-device-configuration-policy-in-intune"></a>Přiřazení zásad konfigurace zařízení v Intune

#### <a name="create-a-group-to-apply-device-configuration-policy-to"></a>Vytvoření skupiny, na kterou se použijí zásady konfigurace zařízení

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) a klikněte na **Azure Active Directory**.
2. Přejděte do **Uživatelé a skupiny** > **Všechny skupiny** > **Nová skupina**.
3. Zadejte název skupiny a v části **Typ členství** zvolte **Přiřazený**. 
   * V části **Členové** zvolte zařízení s Windows 10 připojená k Azure AD, pro která chcete zásady použít.
   * Klikněte na **Vybrat**.
4. Klikněte na **Vytvořit**

Další informace o vytváření skupin najdete v článku [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md).

#### <a name="assign-device-configuration-policy-to-device-group"></a>Přiřazení zásad konfigurace zařízení ke skupině zařízení

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) a klikněte na **Intune**.
2. Přejděte do **Konfigurace zařízení** > **Profily**, vyhledejte dříve vytvořený profil konfigurace zařízení a klikněte na něj.
3. Přiřazení profilu ke skupině zařízení 
   * V části **Zahrnout** > **Vybrat skupiny, které se zahrnou** klikněte na **Přiřazení**.
   * Vyberte dříve vytvořenou skupinu a klikněte na **Vybrat**.
   * Klikněte na **Uložit**.

   ![Přiřazení][Assignment]

Právě jste vytvořili a přiřadili zásady konfigurace zařízení a povolili jste tak odkaz na resetování hesla na přihlašovací obrazovce pomocí Intune.

## <a name="configure-reset-password-link-using-the-registry"></a>Konfigurace odkazu na resetování hesla pomocí registru

Tuto metodu doporučujeme použít pouze k testování změny nastavení.

1. Přihlaste se k zařízení připojenému k Azure AD s použitím přihlašovacích údajů pro správu.
2. Spusťte program **regedit** jako správce.
3. Nastavte následující klíč registru:
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## <a name="what-do-users-see"></a>Co vidí uživatelé

Co se po konfiguraci a přiřazení zásad změnilo pro uživatele? Jak se dozví, že své heslo můžou resetovat na přihlašovací obrazovce?

![Přihlašovací obrazovka][LoginScreen]

Při pokusu o přihlášení se teď uživatelům na přihlašovací obrazovce zobrazí odkaz Resetovat heslo, který otevře prostředí samoobslužného resetování hesla. Tato funkce umožňuje uživatelům resetovat své heslo, aniž by museli použít jiné zařízení pro přístup k webovému prohlížeči.

Vaši uživatelé najdou pokyny k použití této funkce v tématu popisujícím [resetování hesla k pracovnímu nebo školnímu účtu](active-directory-passwords-update-your-own-password.md#reset-password-at-sign-in).

## <a name="common-issues"></a>Běžné problémy

Při testování této funkce s použitím Hyper-V se odkaz Resetovat heslo nezobrazí.

* Přejděte do virtuálního počítače, který používáte k testování, klikněte na **Zobrazit** a pak zrušte zaškrtnutí políčka **Rozšířená relace**.

Při testování této funkce s použitím Vzdálené plochy se odkaz Resetovat heslo nezobrazí.

* Resetování hesla u Vzdálené plochy se v současné době nepodporuje.

## <a name="next-steps"></a>Další kroky
Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD

* [Jak nasadím samoobslužné resetování hesla?](active-directory-passwords-best-practices.md)
* [Jak povolím resetování kódu PIN z přihlašovací obrazovky?](https://docs.microsoft.com/intune/device-windows-pin-reset)
* [Další informace o zásadách ověřování MDM](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-authentication)

[CreateProfile]: ./media/active-directory-passwords-login/create-profile.png "Vytvoření profilu konfigurace zařízení Intune pro povolení odkazu na resetování hesla na přihlašovací obrazovce Windows 10"
[Assignment]: ./media/active-directory-passwords-login/profile-assignment.png "Přiřazení zásad konfigurace zařízení Intune ke skupině zařízení s Windows 10"
[LoginScreen]: ./media/active-directory-passwords-login/logon-reset-password.png "Odkaz na resetování hesla na přihlašovací obrazovce Windows 10"
