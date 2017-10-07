---
title: "Windows 10 a Windows Server 2016 zařízení připojená k hybridní aaaTroubleshooting Azure Active Directory | Microsoft Docs"
description: "Řešení potíží s hybridní Azure Active Directory připojené zařízení s Windows 10 a Windows Server 2016."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: cc252d1d0684d6632694afc8a367327794228c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a>Řešení potíží s hybridní Azure Active Directory připojené zařízení s Windows 10 a Windows Server 2016 

Toto téma se vztahuje toohello následující klienty:

-   Windows 10
-   Windows Server 2016

Pro ostatní klienty systému Windows najdete v části [nižší úrovně zařízení připojená k hybridní Poradce při potížích s Azure Active Directory](device-management-troubleshoot-hybrid-join-windows-legacy.md).

Toto téma předpokládá, že máte [nakonfigurované hybridní Azure Active Directory zařízení připojená k](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello následující scénáře:

- Podmíněný přístup využívající zařízení

- [Enterprise cestovní nastavení](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello pro firmy](active-directory-azureadjoin-passport-deployment.md)


Tento dokument obsahuje pokyny k odstraňování problémů na tom, jak tooresolve potenciální problémy. 


Pro Windows 10 a Windows Server 2016, hybridní hello podporuje připojení k Azure Active Directory systému Windows 10. listopadu 2015 aktualizace a vyšší. Doporučujeme používat hello výročí aktualizace.

## <a name="step-1-retrieve-hello-join-status"></a>Krok 1: Načíst stav spojení hello 

**Stav připojení k tooretrieve hello:**

1. Hello otevřete příkazový řádek jako správce

2. Typ   **/dsregcmd status**



    +----------------------------------------------------------------------+
   | Stav zařízení |+----------------------------------------------------------------------+
    
        AzureAdJoined: YES
     EnterpriseJoined: Žádné DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 kryptografický otisk: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: TpmProtected zprostředkovatel kryptografických služeb platformy Microsoft: Ano KeySignTest:: musíte spustit zvýšenými tootest.
                  Rozšíření IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/ msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https:// Portal.Manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https:// enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs: enterpriseregistration.Windows.NET DomainJoined: Ano DomainName: CONTOSO
    
    +----------------------------------------------------------------------+
   | Stav uživatele |+----------------------------------------------------------------------+
    
                 NgcSet: YES
               NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined: NO
          WamDefaultSet: YES
    WamDefaultAuthority: organizace WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} AzureAdPrt (AzureAd): Ano



## <a name="step-2-evaluate-hello-join-status"></a>Krok 2: Vyhodnocení stavu připojení k hello 

Prohlédněte si hello následující pole a ujistěte se, že mají hello očekávaných hodnot:

### <a name="azureadjoined--yes"></a>AzureAdJoined: Ano  

Toto pole určuje, zda text hello zařízení je spojen s Azure AD. Pokud je hodnota hello **ne**, hello spojení tooAzure AD dosud nebyl dokončen. 

**Možné příčiny:**

- Ověřování počítače hello spojení se nezdařilo.

- V organizaci hello, která nemůže být zjištěny hello počítače je proxy server HTTP

- Hello počítače nebudou moci připojit tooauthenticate Azure AD nebo Azure DRS pro registraci

- Hello počítač nemusí být v interní síti organizace hello nebo na VPN s přímé směrem pohledu tooan místní řadič domény AD.

- Pokud má počítač hello čip TPM, může být ve špatném stavu.

- Může být chybnou konfigurací v hello služby v dokumentu hello si předtím poznamenali, budete potřebovat tooverify znovu. Běžných příkladů:

    - Federační server nemá WS-Trust koncových bodů povoleno

    - Federační server nepovoluje příchozí ověřování z počítačů ve vaší síti použití integrovaného ověřování systému Windows.

    - Neexistuje žádný objekt spojovací bod služby, který odkazuje tooyour název ověřené domény ve službě Azure AD v doménové struktuře hello AD kde hello počítač patří do

---

### <a name="domainjoined--yes"></a>DomainJoined: Ano  

Toto pole určuje, zda text hello zařízení se připojilo k tooan místní služby Active Directory nebo ne. Pokud je hodnota hello **ne**, hello zařízení nelze provést připojení k hybridní Azure AD.  

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: Ne  

Toto pole určuje, zda text hello zařízení zaregistrované v Azure AD jako osobní zařízení (označen jako *připojené k síti na pracovišti*). Tato hodnota by měla být **ne** pro počítač připojený k doméně, která je také připojené k hybridní Azure AD. Pokud je hodnota hello **Ano**, pracovní nebo školní účet se přidal dokončení předchozí toohello hello hybridní Azure AD join. V takovém případě hello účtu se ignoruje při použití hello výročí aktualizovanou verzi Windows 10 (1607).

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Ano a AzureADPrt: Ano
  
Tato pole označuje, zda uživatel hello byl úspěšně ověřen tooAzure AD při přihlášení toohello zařízení. Pokud jsou hodnoty hello **ne**, může to být kvůli:

- Klíč chybný úložiště (STK) v čipu TPM přidružené hello zařízení při registraci (hello kontrola KeySignTest při spuštění, které se zvýšenými oprávněními).

- Alternativním přihlašovacím ID

- Server Proxy protokolu HTTP nebyl nalezen.

## <a name="next-steps"></a>Další kroky

Máte dotazy, viz hello [nejčastější dotazy ke správě zařízení](device-management-faq.md) 
