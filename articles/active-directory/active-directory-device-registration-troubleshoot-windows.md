---
title: "aaaTroubleshooting hello automatickou registraci Azure AD domény k počítačům připojeným k pro Windows 10 a Windows Server 2016 | Microsoft Docs"
description: "Řešení potíží s hello automatickou registraci Azure AD domény k počítačům připojeným k pro Windows 10 a Windows Server 2016."
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
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 3795323ce9392368b412b3e1208868431e59a74b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad--windows-10-and-windows-server-2016"></a>Řešení potíží s automatickou registraci domény připojené počítače tooAzure AD – Windows 10 a Windows Server 2016

Toto téma se vztahuje toohello následující klienty:

-   Windows 10
-   Windows Server 2016

Pro ostatní klienty systému Windows najdete v části [řešení potíží s automatickou registraci domény připojené počítače tooAzure AD pro klienty nižší úrovně Windows](active-directory-device-registration-troubleshoot-windows-legacy.md).

Toto téma předpokládá, že jste nakonfigurovali automatické registrace zařízení připojených k doméně jak je uvedeno v popsané v [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-device-registration-get-started.md) hello toosupport následující scénáře:

- [Podmíněný přístup využívající zařízení](active-directory-conditional-access-automatic-device-registration-setup.md)

- [Enterprise cestovní nastavení](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello pro firmy](active-directory-azureadjoin-passport-deployment.md)


Tento dokument obsahuje pokyny k odstraňování problémů na tom, jak tooresolve potenciální problémy. 

Hello registrace je podporované v systému Windows hello aktualizace 10 listopad 2015 a vyšší.  
Doporučujeme používat hello výročí aktualizace pro povolení výše hello scénáře.

## <a name="step-1-retrieve-hello-registration-status"></a>Krok 1: Načíst stav registrace hello 

**Stav registrace tooretrieve hello:**

1. Otevřete hello příkazový řádek jako správce.

2. Typ   **/dsregcmd status**



    +----------------------------------------------------------------------+
   | Stav zařízení |+----------------------------------------------------------------------+
    
        AzureAdJoined : YES
     EnterpriseJoined: Žádné DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 kryptografický otisk: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: TpmProtected zprostředkovatel kryptografických služeb platformy Microsoft: Ano KeySignTest:: musíte spustit zvýšenými tootest.
                  Rozšíření IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Ano DomainName: CONTOSO
    
    +----------------------------------------------------------------------+
   | Stav uživatele |+----------------------------------------------------------------------+
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    WamDefaultAuthority: organizace WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} AzureAdPrt (AzureAd): Ano



## <a name="step-2-evaluate-hello-registration-status"></a>Krok 2: Vyhodnoťte stav registrace hello 

Prohlédněte si hello následující pole a ujistěte se, že mají hello očekávaných hodnot:

### <a name="azureadjoined--yes"></a>AzureAdJoined: Ano  

Toto pole ukazuje, zda text hello zařízení zaregistrované v Azure AD. Pokud hodnota hello zobrazí jako "Ne", registrace nebyla dokončena. 

**Možné příčiny:**

- Ověřování počítače hello, registrace se nezdařila.

- V organizaci hello, která nemůže být zjištěny hello počítače je proxy server HTTP

- Hello počítače nebudou moci připojit Azure AD pro ověřování nebo Azure DRS pro registraci

- Hello počítač nemusí být v interní síti organizace hello nebo na VPN s přímé směrem pohledu tooan místní řadič domény AD.

- Pokud má počítač hello čip TPM, je ve špatném stavu.

- Je možné chybnou konfigurací v služby v dokumentu hello si předtím poznamenali, budete potřebovat tooverify znovu. Běžných příkladů:

    - Federační server nemá WS-Trust koncových bodů povoleno

    - Federační server nemusí umožňovat příchozí ověřování z počítačů ve vaší síti použití integrovaného ověřování systému Windows.

    - Neexistuje žádný objekt spojovací bod služby, který odkazuje tooyour název ověřené domény ve službě Azure AD v doménové struktuře hello AD kde hello počítač patří do

---

### <a name="domainjoined--yes"></a>DomainJoined: Ano  

Toto pole ukazuje, jestli hello zařízení je připojené k tooan místní služby Active Directory, nebo ne. Pokud hodnota hello zobrazí jako **ne**, hello zařízení nelze automaticky registrace s Azure AD. Nejdřív zkontrolujte, že hello zařízení spojení toohello místní služby Active Directory předtím, než můžete zaregistrovat s Azure AD. Pokud chcete pro připojení hello tooAzure počítače AD přímo, přejděte prosím tooLearn o možnostech Azure Active Directory Join.

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: Ne  

Toto pole ukazuje, zda text hello zařízení je zaregistrované s Azure AD, ale jako osobní zařízení (označen jako připojené k síti na pracovišti). Pokud tato hodnota by měla zobrazit jako "Ne" pro počítač připojený k doméně zaregistrován s Azure AD, ale pokud se zobrazí na hodnotu Ano, znamená to, že pracovní nebo školní účet byl přidaný předchozí toohello počítač dokončuje registraci. Hello účet v tomto případě bude ignorován, pokud používáte verzi hello výročí aktualizace Windows 10 (1607 při spuštění příkazu hello WinVer hello "Spustit" okno nebo okna příkazového řádku).

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Ano a AzureADPrt: Ano
  
Tato pole ukazují, že tento uživatel hello úspěšně ověřen tooAzure AD při přihlášení toohello zařízení. Pokud se zobrazí "Žádný" hello Toto jsou možné příčiny:

- Klíč chybný úložiště (STK) v čipu TPM přidružené hello zařízení při registraci (hello kontrola KeySignTest při spuštění, které se zvýšenými oprávněními).

- Alternativním přihlašovacím ID

- Server Proxy protokolu HTTP nebyl nalezen.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu hello [Automatická registrace zařízení – nejčastější dotazy](active-directory-device-registration-faq.md) 
