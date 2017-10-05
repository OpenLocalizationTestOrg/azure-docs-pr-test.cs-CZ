---
title: "Řešení potíží s automatickou registraci Azure AD domain k počítačům připojeným k pro Windows 10 a Windows Server 2016 | Microsoft Docs"
description: "Řešení potíží s automatickou registraci Azure AD domain k počítačům připojeným k pro Windows 10 a Windows Server 2016."
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
ms.openlocfilehash: 5b7f95f302f716d9221b5fae59aa2df5c956a524
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad--windows-10-and-windows-server-2016"></a>Řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD – Windows 10 a Windows Server 2016

Toto téma se vztahuje na následující klienty:

-   Windows 10
-   Windows Server 2016

Pro ostatní klienty systému Windows najdete v části [řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD pro klienty nižší úrovně Windows](active-directory-device-registration-troubleshoot-windows-legacy.md).

Toto téma předpokládá, že jste nakonfigurovali automatické registrace zařízení připojených k doméně jak je uvedeno v popsané v [postup konfigurace automatické registrace zařízení se systémem Windows připojených k doméně se službou Azure Active Directory](active-directory-device-registration-get-started.md) k podporují následující scénáře:

- [Podmíněný přístup využívající zařízení](active-directory-conditional-access-automatic-device-registration-setup.md)

- [Enterprise cestovní nastavení](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello pro firmy](active-directory-azureadjoin-passport-deployment.md)


Tento dokument obsahuje pokyny k odstraňování problémů, o tom, jak vyřešit potenciální problémy. 

Registrace je podporována v systému Windows 10. listopadu 2015 aktualizace a vyšší.  
Doporučujeme používat výročí aktualizace pro povolení výše uvedených scénářích.

## <a name="step-1-retrieve-the-registration-status"></a>Krok 1: Načíst stav registrace 

**Načíst stav registrace:**

1. Otevřete příkazový řádek jako správce.

2. Typ   **/dsregcmd status**



    +----------------------------------------------------------------------+
   | Stav zařízení |+----------------------------------------------------------------------+
    
        AzureAdJoined : YES
     EnterpriseJoined: Žádné DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 kryptografický otisk: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: TpmProtected zprostředkovatele kryptografických platformy společnosti Microsoft: Ano KeySignTest:: musí spouštět se zvýšenými oprávněními k testování.
                  Rozšíření IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Ano DomainName: CONTOSO
    
    +----------------------------------------------------------------------+
   | Stav uživatele |+----------------------------------------------------------------------+
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    WamDefaultAuthority: organizace WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} AzureAdPrt (AzureAd): Ano



## <a name="step-2-evaluate-the-registration-status"></a>Krok 2: Vyhodnoťte stav registrace 

Zkontrolujte následující pole a ujistěte se, že mají očekávaných hodnot:

### <a name="azureadjoined--yes"></a>AzureAdJoined: Ano  

Toto pole ukazuje, jestli je zařízení zaregistrované v Azure AD. Pokud je hodnota zobrazena jako "Ne", registrace nebyla dokončena. 

**Možné příčiny:**

- Ověřování počítače pro registraci se nezdařilo.

- V organizaci, která nemůže být zjištěny počítače je proxy server HTTP

- Počítač se nemůže připojit, Azure AD pro ověřování nebo Azure DRS pro registraci

- Počítač nemusí být v interní síti organizace nebo na VPN s přímé směrem pohledu na místní řadič domény AD.

- Pokud má počítač čip TPM, je ve špatném stavu.

- Je možné chybnou konfigurací v služby v dokumentu si předtím poznamenali, budete muset znovu ověřit. Běžných příkladů:

    - Federační server nemá WS-Trust koncových bodů povoleno

    - Federační server nemusí umožňovat příchozí ověřování z počítačů ve vaší síti použití integrovaného ověřování systému Windows.

    - Neexistuje žádný objekt spojovací bod služby, který odkazuje na název ověřené domény ve službě Azure AD v doménové struktuře AD, kam počítač patří do

---

### <a name="domainjoined--yes"></a>DomainJoined: Ano  

Toto pole ukazuje, zda je připojení zařízení do služby Active Directory v místě nebo ne. Pokud je hodnota zobrazena jako **ne**, zařízení nelze automaticky registrace s Azure AD. Nejdřív zkontrolujte, že zařízení připojí ke službě Active Directory v místě, než můžete zaregistrovat s Azure AD. Pokud hledáte připojování počítače k Azure AD přímo, přejděte na další informace o možnostech Azure Active Directory Join.

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: Ne  

Toto pole ukazuje, jestli je zařízení zaregistrované s Azure AD, ale jako osobní zařízení (označen jako připojené k síti na pracovišti). Pokud tato hodnota by měla zobrazit jako "Ne" pro počítači připojeném k doméně zaregistrován s Azure AD, ale pokud se zobrazí jako Ano znamená, že pracovní nebo školní účet byl přidán před počítače dokončení registrace. Účet v tomto případě bude ignorován, pokud používáte verzi výročí aktualizace Windows 10 (1607 při spuštění WinVer příkazu v okně "Spustit" nebo okna příkazového řádku).

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Ano a AzureADPrt: Ano
  
Tato pole ukazují, že uživatel byl úspěšně ověřen do služby Azure AD při přihlášení k zařízení. Pokud se zobrazí "Ne", že toto jsou možné příčiny:

- Klíč chybný úložiště (STK) v čipu TPM, které jsou přidružené k zařízení na základě zápisu (Kontrola KeySignTest při spuštění, které se zvýšenými oprávněními).

- Alternativním přihlašovacím ID

- Server Proxy protokolu HTTP nebyl nalezen.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Automatická registrace zařízení – nejčastější dotazy](active-directory-device-registration-faq.md) 