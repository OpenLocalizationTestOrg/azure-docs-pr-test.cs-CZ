---
title: "aaaPublish Vzdálená plocha s Proxy aplikace Azure AD | Microsoft Docs"
description: "Popisuje hello základní informace o Azure AD Application Proxy konektory."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: kgremban
ms.custom: it-pro
ms.reviewer: harshja
ms.openlocfilehash: 1174161d0b5ef1157c334970f00ef4f0702a9545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-remote-desktop-with-azure-ad-application-proxy"></a>Publikování vzdálené plochy s proxy aplikace služby Azure AD

Tento článek popisuje jak toodeploy služby Vzdálená plocha (RDS) pomocí Proxy aplikace tak, aby vzdálení uživatelé stále může být produktivní.

Hello určena cílová skupina pro tento článek je:
- Aktuální Azure AD Application Proxy zákazníci, kteří mají toooffer další aplikace tootheir koncovým uživatelům pomocí publikování místní aplikací prostřednictvím služby Vzdálená plocha.
- Aktuální služby Vzdálená plocha zákazníci, kteří mají tooreduce hello prostor pro útok jejich nasazení pomocí proxy aplikace služby Azure AD. Tento scénář nabízí omezenou sadu dvoustupňové ověřování a tooRDS řízení podmíněného přístupu.

## <a name="how-application-proxy-fits-in-hello-standard-rds-deployment"></a>Jak vyhovuje Proxy aplikace v hello standardní nasazení vzdálené plochy

Standardní nasazení vzdálené plochy zahrnuje různé služby role Vzdálená plocha systémem Windows Server. Prohlížení hello [architektura služby Vzdálená plocha](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture), existuje několik možností nasazení. Hello nejvýraznější rozdíl mezi hello [nasazení vzdálené plochy s Azure AD Application Proxy](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) (zobrazené v hello následující diagram) a hello jiné možnosti nasazení je tento scénář Proxy aplikace hello má trvalou odchozí připojení z hello serveru se spuštěnou službou konektoru hello. Ostatní nasazení ponechat otevřené příchozí připojení přes nástroj pro vyrovnávání zatížení.

![Aplikace Proxy nachází mezi hello virtuálních počítačů vzdálené plochy a hello veřejného Internetu](./media/application-proxy-publish-remote-desktop/rds-with-app-proxy.png)

V nasazení služby Vzdálená plocha hello VP webovou roli a roli služby Brána VP hello spouštět na internetových počítačích. Tyto koncové body jsou viditelné pro hello následujících důvodů:
- RD Web poskytuje hello uživatele toosign veřejný koncový bod v a zobrazení hello různými místními aplikacemi a stolních počítačů, které získají přístup k. Po výběru prostředek, se vytvoří připojení ke vzdálené ploše pomocí nativní aplikace hello na hello operačního systému.
- Brána VP hello obrázek stává, když uživatel spustí připojení RDP hello. Hello Brána VP zpracovává provoz protokolu RDP hello šifrované procházející přes hello Internet a převede jej toohello, které na místním serveru, který hello uživatele se připojuje k. V tomto scénáři hello hello přenosy, které přijímá Brána VP pochází z hello proxy aplikace služby Azure AD.

>[!TIP]
>Pokud jste nenasadili RDS před nebo chcete další informace, než začnete, přečtěte si jak příliš[bezproblémově nasazení vzdálené plochy s Azure Resource Manageru a webu Azure Marketplace](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure).

## <a name="requirements"></a>Požadavky

- Obě hello webové VP a Brána VP koncové body se musí nacházet na stejné počítače a s společný kořen hello. Web VP a Brána VP bude publikován jako jednu aplikaci, můžete mít jeden přihlašování mezi hello dvěma aplikacemi.

- Byste již měli mít [nasazení vzdálené plochy](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure), a [povolit Proxy aplikace](active-directory-application-proxy-enable.md).

- Tento scénář předpokládá, že vaši koncoví uživatelé projít Internet Exploreru na stolní počítače Windows 7 nebo Windows 10, které se připojují prostřednictvím vzdálené plochy webovou stránku hello. Pokud potřebujete toosupport jinými operačními systémy, přečtěte si [podporu pro ostatní konfigurace klienta](#support-for-other-client-configurations).

  >[!NOTE]
  >Aktualizace Windows 10 Creator není aktuálně podporován.

- V Internet Exploreru povolte rozšíření hello ActiveX vzdálené plochy.

## <a name="deploy-hello-joint-rds-and-application-proxy-scenario"></a>Nasazení připojení vzdálené plochy a Proxy aplikací scénáře hello

Po nastavení vzdálené plochy a Azure AD Application Proxy pro vaše prostředí, postupujte podle hello kroky toocombine hello dvě řešení. Tyto kroky provede publikování hello dva webové směřujících RDS koncové body (RD Web a Brána VP) jako aplikace a pak směrují přenosy na vaše toogo vzdálené plochy prostřednictvím Proxy aplikace.

### <a name="publish-hello-rd-host-endpoint"></a>Publikování hello VP hostitele koncového bodu

1. [Publikovat nové aplikace Proxy aplikace](application-proxy-publish-azure-portal.md) s hello následující hodnoty:
   - Interní adresa URL: https://\<rdhost\>.com /, kde \<rdhost\> je hello společný kořen, Web VP a Brána VP sdílet.
   - Externí adresu URL: Toto pole se vyplní automaticky na základě hello název aplikace hello, ale můžete ho upravit. Vaši uživatelé přejde toothis adresu URL, když přistupují k vzdálené plochy
   - Metoda předběžného ověření: Azure Active Directory
   - Převede adresu URL hlavičky: Ne
2. Přiřazení uživatelů toohello publikované aplikace vzdálené plochy. Ujistěte se, že všechny mají přístup tooRDS příliš.
3. Nechte hello jednotné přihlašování metody pro aplikace hello jako **Azure AD jednotné přihlašování zakázáno**. Uživatelům se zobrazí výzva tooauthenticate až tooAzure AD a jednou tooRD Web, ale mít jednu přihlašování tooRD brány.
4. Přejděte příliš**Azure Active Directory** > **registrace aplikace** > *aplikace* > **nastavení**.
5. Vyberte **vlastnosti** a aktualizace hello **Domovská stránka URL** pole toopoint tooyour RD Web koncového bodu (jako https://\<rdhost\>.com nebo RDWeb).

### <a name="direct-rds-traffic-tooapplication-proxy"></a>Směrovat provoz tooApplication RDS Proxy

Připojte nasazení vzdálené plochy toohello jako správce a změňte název serveru služby Brána VP hello hello nasazení. Tím se zajistí, že připojení projít hello proxy aplikace služby Azure AD.

1. Připojte server služby Vzdálená plocha toohello rolí Zprostředkovatel připojení k VP hello.
2. Spusťte **správce serveru**.
3. Vyberte **služby Vzdálená plocha** z hello podokna na levé straně hello.
4. Vyberte **přehled**.
5. V oddílu Přehled nasazení hello, vyberte hello rozevírací nabídky a zvolte **upravit vlastnosti nasazení**.
6. Na kartě hello brány VP, změnit hello **název serveru** pole toohello externí adresu URL, který nastavíte pro koncový bod hostitele hello VP v Proxy aplikace.
7. Změna hello **přihlášení metoda** pole příliš**ověřování hesla**.

  ![Vlastnosti obrazovky nasazení na vzdálené plochy](./media/application-proxy-publish-remote-desktop/rds-deployment-properties.png)

8. Pro každou kolekci spusťte následující příkaz hello. Nahraďte  *\<yourcollectionname\>*  a  *\<proxyfrontendurl\>*  nahraďte svými vlastními informacemi. Tento příkaz umožňuje jednotné přihlašování mezi Web VP a Brána VP a je optimalizován výkon:

   ```
   Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s:<proxyfrontendurl>`nrequire pre-authentication:i:1"
   ```

   **Například:**
   ```
   Set-RDSessionCollectionConfiguration -CollectionName "QuickSessionCollection" -CustomRdpProperty "pre-authentication server address:s:https://gateway.contoso.msappproxy.net/`nrequire pre-authentication:i:1"
   ```

9. Změna hello tooverify hello vlastní vlastnosti protokolu RDP, jakož i zobrazit obsah souboru RDP v hello, které budou staženy z RDWeb pro tuto kolekci, spusťte následující příkaz hello:
    ```
    (get-wmiobject -Namespace root\cimv2\terminalservices -Class Win32_RDCentralPublishedRemoteDesktop).RDPFileContents
    ```

Nyní, když jste nakonfigurovali vzdálené plochy, Azure AD Application Proxy trvá jako hello internetového součást vzdálené plochy Můžete odebrat hello jiné veřejné koncové body internetového na vaše webové VP a Brána VP počítače.

## <a name="test-hello-scenario"></a>Scénáře hello testu

Testovací scénář hello pomocí Internet Exploreru na počítači 10 nebo Windows 7.

1. Přejděte toohello externí adresu URL, nastavíte nebo najít aplikaci v hello [MyApps panel](https://myapps.microsoft.com).
2. Zobrazí se výzva tooauthenticate tooAzure služby Active Directory. Používáte účet, který jste přiřadili toohello aplikace.
3. Zobrazí se výzva tooauthenticate tooRD Web.
4. Po úspěšné ověřování vzdálené plochy, můžete vybrat hello plocha nebo aplikace a začít pracovat.

## <a name="support-for-other-client-configurations"></a>Podpora pro ostatní konfigurace klienta

Konfigurace Hello uvedených v tomto článku je pro uživatele Windows 7 nebo Windows 10 se aplikace Internet Explorer a rozšíření hello ActiveX vzdálené plochy. Pokud potřebujete, může podporovat jiné operační systémy nebo prohlížeče. Hello rozdíl je v hello metodu ověřování, který používáte.

| Metoda ověřování | Podporované klientské konfigurace |
| --------------------- | ------------------------------ |
| Předběžné ověření    | Windows 7 nebo 10 pomocí Internet Exploreru + rozšíření ActiveX vzdálené plochy |
| Atribut PassThrough | Všechny ostatní operační systém, který podporuje aplikace hello Vzdálená plocha od Microsoftu |

tok předběžného ověření Hello nabízí další výhody zabezpečení než hello průchozí toku. Předběžné ověřování můžete využít funkce ověřování Azure AD jako jednotné přihlašování, podmíněného přístupu a dvoustupňové ověřování pro vaše místní prostředky. Můžete také zajistěte, aby který pouze ověřené přenosy dosáhnou vaší sítě.

toouse ověřování průchozí, že se ještě dvě úpravy toohello kroky uvedené v tomto článku:
1. V [publikování hello VP hostitele endpoint](#publish-the-rd-host-endpoint) kroku 1, nastavte metodu předběžného ověření hello příliš**průchozí**.
2. V [přímé RDS provozu tooApplication Proxy](#direct-rds-traffic-to-application-proxy), zcela přeskočit krok 8.

## <a name="next-steps"></a>Další kroky

[Povolit tooSharePoint vzdáleného přístupu s proxy aplikace služby Azure AD](application-proxy-enable-remote-access-sharepoint.md)  
[Důležité informace o zabezpečení pro vzdálený přístup k aplikací pomocí proxy aplikace služby Azure AD](application-proxy-security-considerations.md)
