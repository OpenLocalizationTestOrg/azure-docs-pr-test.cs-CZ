---
title: "aplikace SaaS aaaConfigure pro spolupráci B2B ve službě Azure Active Directory | Microsoft Docs"
description: "Ukázky kódu a prostředí PowerShell pro Azure Active Directory s B2B spolupráce"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: c3f22f81567c04ac23ef2316c09de718ecb15d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a>Konfigurace aplikací SaaS pro spolupráci B2B

Spolupráce Azure Active Directory (Azure AD) s B2B pracuje s většinu aplikací, které se integrují s Azure AD. V této části jsme provede pokyny ke konfiguraci některých oblíbených aplikací SaaS pro použití s Azure AD s B2B.

Předtím, než se podíváte na konkrétní aplikaci pokyny, zde jsou některé zásady:

* Pro většinu aplikací hello nastavení uživatele musí toohappen ručně. To znamená uživatelé musí být vytvořeny ručně v aplikaci hello také.

* Pro aplikace, které podporují automatické nastavení, jako je Dropbox jsou vytvořeny samostatné pozvánek z aplikace hello. Uživatelé musí být jisti tooaccept každý pozvánku.

* V hello uživatelské atributy toomitigate všechny problémy s disk profilu pozměnění uživatele (UPD) v uživatele typu Host, vždy nastavená **uživatelský identifikátor** příliš**user.mail**.


## <a name="dropbox-business"></a>Obchodní dropbox

tooenable toosign uživatele pomocí svého účtu organizace, je nutné ručně nakonfigurovat Dropbox obchodní toouse Azure AD jako zprostředkovatele identity Security (Assertion Markup Language SAML). Pokud obchodní Dropbox nebyl nakonfigurovaný toodo tedy nemůže výzvu nebo jinak povolit toosign uživatelů v používání služby Azure AD.

1. tooadd hello Dropbox obchodní aplikace do služby Azure AD, vyberte **podnikové aplikace, které** v levém podokně text hello a potom klikněte na **přidat**.

  ![tlačítko "Přidat" Hello na stránce aplikace hello Enterprise](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

2. V hello **přidat aplikaci** okno, zadejte **dropbox** v hello vyhledávacího pole a pak vyberte **Dropbox pro firmy** v seznamu výsledků hello.

  ![Vyhledejte "dropbox" na hello přidání stránky aplikace](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

3. Na hello **jednotného přihlašování** vyberte **jednotného přihlašování** v levém podokně text hello a potom zadejte **user.mail** v hello **uživatelský identifikátor** pole. (Je nastavený jako UPN ve výchozím nastavení.)

  ![Konfigurace jednotného přihlašování pro aplikace hello](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

4. Vyberte toodownload hello certifikát toouse pro konfiguraci Dropbox, **konfigurace DropBox**a potom vyberte **SAML jeden znak na adresu URL služby** v seznamu hello.

  ![Stahování hello certifikát pro Dropbox konfigurace](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. Přihlášení tooDropbox s hello přihlašování adresu URL z hello **jednotného přihlašování** stránky.

  ![stránku Hello přihlášení Dropbox](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

6. V nabídce hello vyberte **konzoly pro správu**.

  ![Hello "Konzoly pro správu" odkaz v nabídce Dropbox hello](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

7. V hello **ověřování** dialogové okno, vyberte **Další**, nahrajte certifikát hello a pak na hello **přihlašovací adresa URL** zadejte adresu URL hello SAML jednotné přihlašování.

  !["Informace" odkaz v dialogovém okně ověřování hello sbalené Hello](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  ![Hello "Přihlášení v URL" v hello rozšířit ověřování, dialogové okno](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

8. nastavení automatického uživatele tooconfigure v hello portál Azure, vyberte **zřizování** v levém podokně hello vyberte **automatické** v hello **režimu zřizování** a potom vyberte **Autorizovat**.

  ![Konfiguraci zřizování automatické uživatelů v hello portálu Azure](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

Po hosta nebo člen uživatelů nastavili v aplikaci hello Dropbox, obdrží samostatné pozvánku z Dropbox. toouse Dropbox jednotné přihlašování, budou pozvaní uživatelé musí přijmout pozvánku hello kliknutím na odkaz v ní.

## <a name="box"></a>Box
Uživatelé tooauthenticate pole uživatele typu Host ke svému účtu Azure AD můžete povolit pomocí federace, která je založená na hello protokolu SAML. V tomto postupu nahrajte tooBox.com metadat.

1. Přidáte aplikaci Box hello z hello podnikové aplikace.

2. Nakonfigurujte jednotné přihlašování v hello následující pořadí:

  ![Konfigurace pole jednotné přihlašování](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

 a. V hello **přihlásit na adrese URL** pole, zkontrolujte, že hello přihlašovací adresa URL je správně nastavená pro pole v hello portálu Azure. Tato adresa URL je adresa URL hello klienta služby Box.com. Měl by splňovat zásady vytváření názvů hello *https://.box.com*.  
 Hello **identifikátor** doporučení se netýká toothis aplikace, ale stále se zobrazí jako povinné pole.

 b. V hello **uživatelský identifikátor** zadejte **user.mail** (pro jednotné přihlašování pro účet hosta).

 c. V části **SAML podpisový certifikát**, klikněte na tlačítko **vytvořit nový certifikát**.

 d. toobegin konfigurace vaší toouse Box.com klienta Azure AD jako zprostředkovatele identity, stažení souboru metadat hello a pak ho uložte tooyour místní disk.

 e. Předat dál hello metadata souboru toohello pole tým podpory, který nakonfiguruje jednotné přihlašování pro vás.

3. Nastavení automatického uživatele Azure AD, v levém podokně hello vyberte **zřizování**a potom vyberte **Authorize**.

  ![Autorizovat tooBox tooconnect Azure AD](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

Jako budou pozvaní uživatelé Dropbox musí pozvaným uživatelům pole uplatnit jejich pozvání aplikaci Box hello.

## <a name="next-steps"></a>Další kroky

Najdete v následujících článcích na spolupráci Azure AD B2B hello:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md)
* [Přidání role uživatele tooa spolupráce B2B](active-directory-b2b-add-guest-to-role.md)
* [Delegovat pozvánek spolupráce B2B](active-directory-b2b-delegate-invitations.md)
* [Dynamické skupiny a spolupráci B2B](active-directory-b2b-dynamic-groups.md)
* [Kód spolupráce B2B a ukázky prostředí PowerShell](active-directory-b2b-code-samples.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Deklarace uživatele spolupráce B2B mapování](active-directory-b2b-claims-mapping.md)
* [Externí sdílení Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuální omezení spolupráce B2B](active-directory-b2b-current-limitations.md)
