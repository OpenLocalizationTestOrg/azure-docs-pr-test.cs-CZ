---
title: "aaaProblems přihlášení tooan aplikace Azure AD Galerie konfigurovaná pro heslo jednotného přihlašování | Microsoft Docs"
description: "Popisuje problémových oblastí, které poskytují pokyny tootroubleshoot problémy související toosigning v tooAzure AD Galerie aplikace, které jsou nakonfigurované pro heslo jednotné přihlašování"
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
ms.openlocfilehash: f53ef4176db37dc6b1da2d61027155a6ba8f331e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-azure-ad-gallery-application-configured-for-password-single-sign-on"></a>Potíže s přihlášením tooan Galerie Azure AD aplikace konfigurovaná pro heslo jednotné přihlašování

Hello přístupový Panel je, že je webový portál, který umožňuje uživatel, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spouštět cloudové aplikace tento správce hello Azure AD udělil přístup. Uživatel, který má edice Azure AD můžete také skupiny samoobslužné služby a možnosti správy aplikace prostřednictvím hello přístupového panelu. Hello přístupový Panel je oddělená od hello portál Azure a nevyžaduje, aby uživatelé toohave předplatné Azure.

toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele. Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.

## <a name="meeting-browser-requirements-for-hello-access-panel"></a>Splňuje požadavky na prohlížeč pro hello přístupového panelu

Hello přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS. toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele. Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.

Pro jednotné přihlašování založené na heslech může být prohlížeče hello koncového uživatele:

-   Internet Explorer 8, 9, 10, 11 – v systému Windows 7 nebo novější

-   Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější

-   Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější

>[!NOTE]
>rozšíření jednotného přihlašování založené na heslech Hello k dispozici pro Edge ve Windows 10, při rozšíření prohlížeče stát podporované pro okraj.
>
>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Jak tooinstall hello rozšíření přístup k panelu prohlížeče

tooinstall hello rozšíření prohlížeče panely přístup, postupujte podle kroků hello níže:

1.  Otevřete hello [přístupový Panel](https://myapps.microsoft.com) v jednom z hello podporované prohlížeče a přihlaste se jako **uživatele** ve službě Azure AD.

2.  Klikněte na tlačítko **SSO heslo aplikace** v hello přístupového panelu.

3.  V hello výzva s dotazem tooinstall hello softwaru, vyberte **instalovat nyní**.

4.  Založené na prohlížeči, je možné směrovanou toohello odkaz ke stažení. **Přidat** hello rozšíření tooyour prohlížeče.

5.  Pokud prohlížeč zobrazí dotaz, vyberte tooeither **povolit** nebo **povolit** hello rozšíření.

6.  Po instalaci **restartujte** relaci prohlížeče.

7.  Přihlaste se do hello přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO

Může také stáhnout hello rozšíření pro Chrome a Firefox z hello přímé odkazy níže:

-   [Rozšíření pro Chrome přístup panely](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Rozšíření panelu Firefox přístup](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Nastavení zásad skupiny pro Internet Explorer

Můžete nastavit zásady skupiny, které umožňují tooremotely instalace hello přístupový Panel rozšíření pro Internet Explorer na počítačích uživatelů.

Hello požadavky patří:

-   Jste nastavili [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a připojení uživatelů počítačů tooyour domény.

-   Musíte mít hello "Upravit nastavení" oprávnění tooedit hello objekt zásad skupiny (GPO). Ve výchozím nastavení, toto oprávnění mají členové hello následující skupiny zabezpečení: Domain Administrators, Enterprise Administrators a Group Policy Creator Owners. [Další informace](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Postupujte podle kurzu hello [jak tooDeploy hello rozšíření přístup k panelu pro Internet Explorer pomocí zásad skupiny](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) pro podrobné pokyny, jak tooconfigure hello zásad skupiny a nasadit ji toousers.

## <a name="troubleshoot-hello-access-panel-in-internet-explorer"></a>Řešení potíží s hello přístupového panelu v aplikaci Internet Explorer

Postupujte podle hello [Poradce při potížích hello rozšíření panely přístup pro prohlížeč Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) Průvodce pro přístup k nástroj pro diagnostiku a podrobné informace o konfiguraci hello rozšíření pro aplikaci Internet Explorer.

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a>Jak tooconfigure heslo jednotného přihlašování pro aplikace bez Galerie

tooconfigure aplikaci z Galerie Azure AD hello budete muset:

-   [Přidání aplikace bez Galerie](#add-a-non-gallery-application)

-   [Konfigurace aplikace hello pro heslo jednotné přihlašování](#configure-the-application-for-password-single-sign-on)

-   [Přiřazení uživatelů toohello aplikace](#assign-users-to-the-application)

### <a name="add-a-non-gallery-application"></a>Přidání aplikace bez Galerie

tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno

6.  Klikněte na tlačítko **Galerie jiná aplikace.**

7.  Zadejte název vaší aplikace hello v hello **název** textové pole. Vyberte **přidat.**

Po krátké době být okno Konfigurace aplikací mít toosee hello.

### <a name="configure-hello-application-for-password-single-sign-on"></a>Konfigurace aplikace hello pro heslo jednotné přihlašování

tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Vyberte hello režimu **založené na heslech přihlášení.**

9.  Zadejte hello **přihlašovací adresa URL**. Toto je hello URL, kde uživatelé zadat toosign své uživatelské jméno a heslo v k. Zkontrolujte, zda jsou viditelné na adrese URL hello hello přihlášení pole.

10. Přiřazení uživatelů toohello aplikace.

11. Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo. Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.

### <a name="assign-users-toohello-application"></a>Přiřazení uživatelů toohello aplikace

tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.

7.  Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.

8.  Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.

9.  Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.

10. Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.

11. Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**. Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.

12. **Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.

13. Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.

14. **Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.

15. Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.

Po krátké době hello uživatelů, které jste vybrali být schopný toolaunch tyto aplikace v hello přístupového panelu.

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a>Pokud tento postup řešení není hello hello problém vyřešte

Otevřete lístek podpory se hello, pokud je k dispozici následující informace:

-   ID chyby korelace

-   Hlavní název uživatele (e-mailovou adresu uživatele)

-   TenantID

-   Typ prohlížeče

-   Dojde k časové pásmo a čas nebo období při chybě

-   Fiddler trasování

## <a name="next-steps"></a>Další kroky
[Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)

