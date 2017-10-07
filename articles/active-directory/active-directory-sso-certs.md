---
title: "aaaManage federačních certifikátů ve službě Azure AD | Microsoft Docs"
description: "Zjistěte, jak datum vypršení platnosti hello toocustomize federačních certifikátů a jak toorenew certifikáty, které brzy vyprší."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f516f7f0-b25a-4901-8247-f5964666ce23
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2017
ms.author: jeedes
ms.openlocfilehash: e17622d25c9babfa295cc0bb68c24e21b8c574d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Spravovat certifikáty pro federované jednotné přihlašování v Azure Active Directory
Tento článek popisuje běžné otázky a informace týkající se, že certifikáty toohello, Azure Active Directory (Azure AD) vytvoří tooestablish federovaný jeden přihlašování (SSO) tooyour aplikací SaaS. Přidáte aplikace z Galerie aplikace hello Azure AD nebo pomocí šablony aplikace bez galerie. Konfigurace aplikace hello pomocí možnosti hello federovaného jednotného přihlašování.

Tento článek je relevantní pouze tooapps, které jsou nakonfigurované toouse Azure AD SSO pomocí SAML federování, jak je znázorněno v hello následující ukázka:

![Azure AD jednotné přihlášení](./media/active-directory-sso-certs/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Automaticky vygeneruje certifikát pro galerii a bez Galerie aplikace
Při přidání nové aplikace z Galerie hello a konfiguraci základě SAML přihlašování, Azure AD vygeneruje certifikát pro aplikaci hello, který je platný po dobu tří let. Tento certifikát si můžete stáhnout z hello **SAML podpisový certifikát** části. U aplikací, galerie může zobrazovat v této části možnost toodownload hello certifikát nebo metadata, v závislosti na požadavek hello aplikace hello.

![Azure AD jednotné přihlašování](./media/active-directory-sso-certs/saml_certificate_download.png)

## <a name="customize-hello-expiration-date-for-your-federation-certificate-and-roll-it-over-tooa-new-certificate"></a>Přizpůsobení hello datum vypršení platnosti certifikátu federace a vrátit přes tooa nový certifikát
Ve výchozím nastavení certifikáty, se nastavují tooexpire po tři roky. Provedením následujících kroků hello můžete různých platnosti certifikátu.
snímky obrazovky Hello používat Salesforce pro hello zájmu příklad, ale tyto kroky můžete použít tooany federované aplikace SaaS.

1. V hello [portál Azure](https://aad.portal.azure.com), klikněte na tlačítko **podniková aplikace** v levém podokně hello a pak klikněte na **novou aplikaci** na hello **přehled** stránka:

   ![Průvodce konfigurací otevřete hello jednotného přihlašování](./media/active-directory-sso-certs/enterprise_application_new_application.png)

2. Vyhledejte hello Galerie aplikace a pak vyberte hello aplikace, které chcete tooadd. Pokud nemůžete najít hello požadované aplikace, přidání aplikace hello pomocí hello **aplikace bez Galerie** možnost. Tato funkce je k dispozici pouze v hello SKU Azure AD Premium (P1 a P2).

    ![Azure AD jednotné přihlašování](./media/active-directory-sso-certs/add_gallery_application.png)

3. Klikněte na tlačítko hello **jednotného přihlašování** odkaz v levé podokno změnu hello **režimu přihlašování** příliš**na základě SAML přihlašování**. Tento krok vygeneruje certifikát tři roky pro vaši aplikaci.

4. toocreate nový certifikát, klikněte na tlačítko hello **vytvořit nový certifikát** odkaz v hello **SAML podpisový certifikát** části.

    ![Vygenerovat nový certifikát](./media/active-directory-sso-certs/create_new_certficate.png)

5. Hello **vytvořit nový certifikát** odkaz otevře ovládacího prvku Kalendář hello. Z hello aktuální datum můžete nastavit žádné datum a čas, až toothree let. Hello vybrané datum a čas hello nové datum a čas vypršení nového certifikátu. Klikněte na **Uložit**.

    ![Stáhněte si pak nahrajte certifikát hello](./media/active-directory-sso-certs/certifcate_date_selection.PNG)

6. Hello nový certifikát je nyní k dispozici toodownload. Klikněte na tlačítko hello **certifikát** odkaz toodownload ho. V tomto okamžiku by váš certifikát není aktivní. Když chcete tooroll přes toothis certifikát, vyberte hello **aktivujte nový certifikát** zaškrtávací políčko a klikněte na tlačítko **Uložit**. Od tohoto okamžiku spustí Azure AD pomocí hello nový certifikát pro podepisování hello odpovědi.

7.  toolearn jak tooupload hello certifikátu tooyour určité aplikaci SaaS, klikněte na tlačítko hello **kurz konfigurace zobrazení aplikace** odkaz.

## <a name="renew-a-certificate-that-will-soon-expire"></a>Obnovení certifikátu, kterému brzy vyprší
Hello následující kroky obnovení musí mít za následek žádné významné výpadky pro vaše uživatele. snímky obrazovky Hello v této části funkce Salesforce jako příklad, ale tyto kroky můžete použít tooany federovaných aplikací SaaS.

1. Na hello **Azure Active Directory** aplikace **jednotného přihlašování** stránky, vygenerujte hello nový certifikát pro vaši aplikaci. To provedete kliknutím hello **vytvořit nový certifikát** odkaz v hello **SAML podpisový certifikát** části.

    ![Vygenerovat nový certifikát](./media/active-directory-sso-certs/create_new_certficate.png)

2. Vyberte hello požadovaného vypršení platnosti datum a čas pro nový certifikát a klikněte na tlačítko **Uložit**.

3. Stáhněte si certifikát hello v hello **certifikát pro podpis SAML** možnost. Odeslání obrazovky Konfigurace přihlášení hello nový certifikát toohello SaaS aplikace. toolearn jak toodo to pro konkrétní aplikace SaaS, klikněte na tlačítko hello **kurz konfigurace zobrazení aplikace** odkaz.
   
4. tooactivate hello nového certifikátu ve službě Azure AD, vyberte hello **aktivujte nový certifikát** zaškrtněte políčko a klikněte na tlačítko hello **Uložit** tlačítko hello horní části stránky hello. To zahrnuje přes hello nový certifikát na hello straně Azure AD. Hello stav certifikátu hello změní z **nový** příliš**Active**. Od tohoto okamžiku spustí Azure AD pomocí hello nový certifikát pro podepisování hello odpovědi. 
   
    ![Vygenerovat nový certifikát](./media/active-directory-sso-certs/new_certificate_download.png)

## <a name="related-articles"></a>Související články
* [Seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [Řešení potíží s na základě SAML jednotné přihlašování](active-directory-saml-debugging.md)
