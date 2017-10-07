---
title: "ověřování na základě aaaHeader s PingAccess pro proxy aplikace služby Azure AD | Microsoft Docs"
description: "Publikování aplikací pomocí Proxy aplikace a PingAccess toosupport hlavičky ověřování na základě."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 38fe3e7a41a71f4ae6c75f014e44c722f773bd22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Ověřování na základě záhlaví pro jednotné přihlašování s Proxy aplikace a PingAccess

Azure Proxy aplikace Active Directory a PingAccess společně společně tooprovide Azure Active Directory zákazníkům přístup tooeven další aplikace. PingAccess rozšíří hello [stávající nabídky Proxy aplikace](active-directory-application-proxy-get-started.md) tooapplications tooinclude přístup přihlášení, která použít záhlaví pro ověřování.

## <a name="what-is-pingaccess-for-azure-ad"></a>Co je PingAccess pro Azure AD?

PingAccess pro Azure Active Directory je nabídky PingAccess, která umožňuje toogive uživatelé přístup a jeden přihlašování tooapplications, použít záhlaví pro ověřování. Proxy aplikací považuje těchto aplikací, stejně jako jakýkoli jiný pomocí Azure AD tooauthenticate přístup a následné předání přenos prostřednictvím služby konektoru hello. PingAccess nachází před hello aplikace a překládá hello přístupového tokenu z Azure AD do záhlaví tak, aby aplikace hello obdržel hello ověřování hello formát, který může číst.

Uživatelé nebudou Všimněte si, něco jinak při přihlášení toouse podnikové aplikace. Mohou i nadále pracovat z odkudkoli na libovolném zařízení. 

Vzhledem k tomu, že konektory Proxy aplikace hello přímé vzdálenou komunikaci tooall aplikace bez ohledu na jejich typ ověřování, budou pokračovat vyrovnávání tooload automaticky, stejně.

## <a name="how-do-i-get-access"></a>Jak získám přístup?

Vzhledem k tomu, že se tento scénář nabízí prostřednictvím partnerství mezi Azure Active Directory a PingAccess, potřebujete licence pro obě služby. Však předplatných Azure Active Directory Premium zahrnují základní PingAccess licencí, které pokrývá až too20 aplikace. Pokud potřebujete více než 20 aplikace založené na záhlaví toopublish, si můžete zakoupit další licence od PingAccess. 

Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).

## <a name="publish-your-application-in-azure"></a>Publikování aplikace v Azure

Tento článek je určený pro uživatele, kteří jsou publikování aplikace pomocí tento scénář pro hello poprvé. Ji provede jak tooget pracovat s aplikací a PingAccess, kromě toohello publikování kroky. Pokud jste již nakonfigurovali obě služby, ale chcete aktualizačnímu programu na hello publikování kroky, můžete přeskočit hello instalace konektoru a přesunout příliš[přidat vaší aplikace tooAzure AD s Proxy aplikace](#add-your-app-to-Azure-AD-with-Application-Proxy).

>[!NOTE]
>Vzhledem k tomu, že tento scénář je spolupráci mezi službou Azure AD a PingAccess, některé pokyny hello nebudou na hello Ping Identity lokality.

### <a name="install-an-application-proxy-connector"></a>Instalace konektoru Proxy aplikace

Pokud už máte Proxy aplikace povolena a máte nainstalovaný konektor, můžete tuto část přeskočit a přesunout příliš[přidat vaší aplikace tooAzure AD s Proxy aplikace](#add-your-app-to-azure-ad-with-application-proxy).

konektor Proxy aplikace Hello je Windows Server služba, která přesměruje přenosy hello z vaší vzdálení zaměstnanci tooyour publikované aplikace. Podrobné pokyny k instalaci, najdete v části [povolení Proxy aplikace v portálu Azure hello](active-directory-application-proxy-enable.md).

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) jako globální správce.
2. Vyberte **Azure Active Directory** > **proxy aplikace**.
3. Vyberte **stáhnout konektor** stažení konektoru Proxy aplikace hello toostart. Postupujte podle pokynů pro instalaci hello.

   ![Povolení Proxy aplikace a stáhnout konektor hello](./media/application-proxy-ping-access/install-connector.png)

4. Stahování konektoru hello by měl automaticky povolení Proxy aplikace pro váš adresář, ale pokud ne, můžete si vybrat **povolení Proxy aplikace**.


### <a name="add-your-app-tooazure-ad-with-application-proxy"></a>Přidání vaší aplikace tooAzure AD s Proxy aplikace

Existují dvě akce je nutné tootake v hello portálu Azure. Nejprve je třeba toopublish vaší aplikace pomocí Proxy aplikací. Pak musíte toocollect některé informace o aplikaci hello, který můžete použít během hello PingAccess kroky.

Postupujte podle těchto kroků toopublish vaší aplikace. Pro podrobnější návod kroky 1-8, viz [publikování aplikací pomocí proxy aplikace služby Azure AD](application-proxy-publish-azure-portal.md).

1. Jestliže jste v poslední části hello, přihlaste se toohello [portál Azure](https://portal.azure.com) jako globální správce.
2. Vyberte **Azure Active Directory** > **podnikové aplikace, které**.
3. Vyberte **přidat** hello horní části okna hello.
4. Vyberte **místní aplikace**.
5. Vyplňte pole hello požadované informace o nové aplikace. Použijte následující pokyny pro nastavení hello hello:
   - **Interní adresa URL**: normálně je zadat adresu URL hello, kterým přejdete toohello aplikace přihlašovací stránce když jste v podnikové síti hello. V tomto scénáři musí hello konektor tootreat hello PingAccess proxy jako předního stránku hello aplikace hello. Použijte tento formát: `https://<host name of your PA server>:<port>`. Hello port je 3000 ve výchozím nastavení, ale můžete ji nakonfigurovat v PingAccess.
   - **Metoda předběžného ověřování**: Azure Active Directory
   - **Převede adresu URL v hlavičkách**: Ne

   >[!NOTE]
   >Pokud je vaší první aplikace, použijte port 3000 toostart a vraťte tooupdate toto nastavení je-li změnit konfiguraci svého PingAccess. Pokud je aplikace druhý nebo novější, to bude nutné toomatch hello jste nakonfigurovali v PingAccess naslouchací proces. Další informace o [naslouchací procesy v PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).

6. Vyberte **přidat** na hello dolní části okna hello. Přidání vaší aplikace a otevře se nabídka hello rychlý start.
7. V nabídce rychlý start hello vyberte **přiřadit uživatele pro testování**a přidejte alespoň jeden uživatel toohello aplikace. Ujistěte se, zda že má tento účet testovací přístup toohello místní aplikace.
8. Vyberte **přiřadit** toosave hello testovací uživatele přiřazení.
9. V okně správy aplikace hello, vyberte **jednotného přihlašování**.
10. Zvolte **na základě záhlaví přihlašování** z rozevírací nabídky hello. Vyberte **Uložit**.

   >[!TIP]
   >Pokud je to poprvé pomocí na základě záhlaví jednotné přihlašování, musíte tooinstall PingAccess. toomake opravdu vašeho předplatného Azure se automaticky přidruží PingAccess instalace, použití hello odkaz na tuto stránku přihlášení toodownload PingAccess. Můžete nyní otevřete stránku pro stažení hello nebo vraťte toothis stránku později. 

   ![Vyberte na základě záhlaví přihlášení](./media/application-proxy-ping-access/sso-header.PNG)

11. Zavřete okno aplikace hello Enterprise nebo přejděte všechny hello způsob toohello levém tooreturn toohello Azure Active Directory nabídky.
12. Vyberte **registrace aplikace**.

   ![Vyberte aplikaci registrace](./media/application-proxy-ping-access/app-registrations.png)

13. Vyberte hello aplikace, které jste právě přidali, pak **adresy URL odpovědí**.

   ![Vyberte adresy URL odpovědí](./media/application-proxy-ping-access/reply-urls.png)

14. Zkontrolujte toosee, pokud hello externí adresu URL můžete přiřazené tooyour aplikaci v kroku 5, je v seznamu adresy URL odpovědí hello. Pokud není, přidejte ji.
15. V okně Nastavení aplikace hello, vyberte **požadovaná oprávnění**.

  ![Vyberte požadovaná oprávnění](./media/application-proxy-ping-access/required-permissions.png)

16. Vyberte **Přidat**. Hello rozhraní API, zvolte **Windows Azure Active Directory**, pak **vyberte**. Hello oprávnění, zvolte **pro čtení a zápis všech aplikací** a **přihlášení a čtení profilu uživatele**, pak **vyberte** a **provádí**.  

  ![Vyberte oprávnění](./media/application-proxy-ping-access/select-permissions.png)

### <a name="collect-information-for-hello-pingaccess-steps"></a>Shromažďovat informace o krocích PingAccess hello

1. V okně nastavení vaší aplikace, vyberte **vlastnosti**. 

  ![Vyberte vlastnosti](./media/application-proxy-ping-access/properties.png)

2. Uložit hello **Id aplikace** hodnotu. Používá se pro ID klienta hello při konfiguraci PingAccess.
3. V okně Nastavení aplikace hello, vyberte **klíče**.

  ![Vyberte klíče](./media/application-proxy-ping-access/Keys.png)

4. Vytvořte klíč zadáním klíče popis a datum vypršení platnosti výběr z rozevírací nabídky hello.
5. Vyberte **Uložit**. Identifikátor GUID se zobrazí v hello **hodnotu** pole.

  Uložit tato hodnota, nebudou moct toosee ho znovu po toto okno zavřít.

  ![Vytvořit nový klíč.](./media/application-proxy-ping-access/create-keys.png)

6. Zavřete okno registrace aplikace hello nebo přejděte všechny hello způsob toohello levém tooreturn toohello Azure Active Directory nabídky.
7. Vyberte **vlastnosti**.
8. Uložit hello **ID adresáře** identifikátor GUID.

### <a name="optional---update-graphapi-toosend-custom-fields"></a>Volitelné – aktualizace GraphAPI toosend vlastní pole

Seznam tokeny zabezpečení, které odesílá Azure AD pro ověřování najdete v tématu [odkaz tokenu Azure AD](./develop/active-directory-token-and-claims.md). Pokud potřebujete vlastní deklarace identity, který odesílá dalších tokenů, použijte pole aplikaci hello tooset GraphAPI *acceptMappedClaims* příliš**True**. Tuto konfiguraci můžete použít Azure AD Graph Explorer nebo MS Graph toomake. 

Tento příklad používá Explorer grafu:

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```

## <a name="download-pingaccess-and-configure-your-app"></a>Stáhnout PingAccess a konfigurace aplikace

Teď, když jste dokončili všechny kroky instalace hello Azure Active Directory, můžete přesunout na tooconfiguring PingAccess. 

Hello podrobné kroky pro hello PingAccess součástí tohoto scénáře pokračovat v hello dokumentace ke službě Ping Identity, [PingAccess nakonfigurovat pro Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).

Tyto kroky vás provedou procesem hello získávání PingAccess účtu, pokud jste ještě nemáte, instalace hello PingAccess serveru a vytvoření připojení k Azure AD OIDC zprostředkovatele s hello ID adresáře, který jste zkopírovali ze hello portálu Azure. Poté použijte hello ID aplikace a klíč hodnoty toocreate relace webu na PingAccess. Potom můžete nastavit mapování identit a vytvořit virtuální hostitele, webu nebo aplikace.

### <a name="test-your-app"></a>Testování aplikace

Po dokončení těchto kroků, musí být aplikace spuštěná. tootest, otevřete prohlížeč a přejděte externí adresu URL toohello, kterou jste vytvořili při publikování aplikace hello v Azure. Přihlaste se pomocí účtu testovací hello přiřazené toohello aplikaci jste.

## <a name="next-steps"></a>Další kroky

- [Konfigurace PingAccess pro Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [Jak Azure AD Application Proxy poskytovat jednotné přihlašování?](application-proxy-sso-overview.md)
- [Řešení potíží s Proxy aplikace](active-directory-application-proxy-troubleshoot.md)
