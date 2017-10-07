---
title: "Konfigurace federované jednotné přihlašování pro aplikace bez Galerie aaaProblem | Microsoft Docs"
description: "Adresa některé hello běžné problémy, se můžete setkat při konfiguraci federované jeden přihlašování tooyour vlastní SAML aplikaci, která není uvedena v hello Azure AD Application Gallery"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 8c80f0001de01cbc9930ef0441cd804082ee8578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Problém konfigurace federované jednotné přihlašování pro aplikace bez Galerie

Pokud dojde k potížím při konfiguraci aplikace. Ověřte všechny kroky hello v článku hello [konfigurace jedné přihlašování tooapplications které nejsou v galerii aplikací Azure Active Directory hello.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)

## <a name="cant-add-another-instance-of-hello-application"></a>Nelze přidat jiná instance aplikace hello

tooadd druhou instanci aplikace, je třeba toobe moci:

-   Jedinečný identifikátor pro druhou instanci hello nakonfigurujte. Nebudete moct tooconfigure hello, používá stejný identifikátor pro první instanci hello.

-   Nakonfigurujte jiný certifikát než hello, jedna pro hello první instance.

Pokud hello aplikace nepodporuje žádné z výše uvedených hello. Potom nebudete moct tooconfigure druhou instanci.

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a>Kde lze nastavit formát hello EntityID (identifikátor uživatele)

Nebudete moct tooselect hello EntityID (identifikátor uživatele) formát, Azure AD odešle toohello aplikace v odpovědi hello po ověření uživatele.

Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest. Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části hello NameIDPolicy,

## <a name="where-do-i-get-hello-application-metadata-or-certificate-from-azure-ad"></a>Kde získat metadata aplikace hello nebo certifikát z Azure AD

metadata aplikace hello toodownload nebo certifikát z Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Přejděte příliš**SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce. V závislosti na tom, jaké aplikace hello vyžaduje konfiguraci jednotné přihlašování najdete v části buď možnost toodownload hello hello soubor XML s metadaty nebo hello certifikátu.

Azure AD neposkytuje adresu URL tooget hello metadat. Hello metadata mohou být načteny pouze jako soubor XML.

## <a name="dont-know-how-toocustomize-saml-claims-sent-tooan-application"></a>Nevíte, jak deklarace SAML toocustomize odeslané tooan aplikace

toolearn způsobu deklarací identity atributu toocustomize hello SAML odeslané tooyour aplikace, najdete v [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.

## <a name="next-steps"></a>Další kroky
[Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md)
