---
title: aaaSilent nainstalovat konektor Proxy aplikace Azure AD | Microsoft Docs
description: "Popisuje, jak tooperform bezobslužnou instalaci konektoru Proxy aplikace služby Azure AD tooprovide zabezpečený vzdálený přístup tooyour místní aplikace."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3aa1c7f2-fb2a-4693-abd5-95bb53700cbb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ce796ff45a65ba7d5f0f63c02085bdc6af494548
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="silently-install-hello-azure-ad-application-proxy-connector"></a>Bezobslužná instalace hello konektoru Proxy aplikace služby Azure AD
Chcete mít toosend toobe instalace skript toomultiple Windows servery nebo tooWindows servery, které nemají povolené uživatelské rozhraní. Toto téma vám pomůže vytvořit skript prostředí Windows PowerShell, která umožňuje bezobslužné instalace a registrace pro Azure konektor Proxy aplikace AD.

Tato možnost je užitečná, když chcete:

* Na počítače bez uživatelského rozhraní vrstvy, nebo když nelze RDP toohello počítač nainstalujte konektor hello.
* Nainstalujte a zaregistrujte mnoho konektory najednou.
* Integrate hello konektor instalace a registrace jako součást jiného režimu.
* Vytvořte standardní serverovou bitovou kopii, která obsahuje hello konektor bits, ale není registrovaný.

Proxy aplikací funguje tak, že instalace tenký služba systému Windows Server s názvem hello konektor uvnitř vaší sítě. Toowork konektor Proxy aplikace hello má toobe zaregistrována adresáře Azure AD pomocí globálního správce a hesla. Tyto informace se normálně zadá během instalace konektoru v dialogovém okně automaticky otevírané okno. Můžete však použít toocreate prostředí Windows PowerShell přihlašovací údaje objektu tooenter informace o registraci. Nebo můžete vytvořit vlastní token a použít ho tooenter informace o registraci.

## <a name="install-hello-connector"></a>Instalace konektoru hello
Instalace konektoru hello souborů MSI bez registrace hello konektor následujícím způsobem:

1. Otevřete příkazový řádek.
2. Spusťte následující příkaz, ve které hello /q znamená tichá instalace hello – hello instalace nevyzve tooaccept hello licenční smlouva s koncovým uživatelem.
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-hello-connector-with-azure-ad"></a>Registrace konektoru hello s Azure AD
Můžete použít konektor hello tooregister dvěma způsoby:

* Zaregistrovat hello connector pomocí objekt pověření prostředí Windows PowerShell
* Zaregistrovat hello connector pomocí token vytvořený v režimu offline

### <a name="register-hello-connector-using-a-windows-powershell-credential-object"></a>Zaregistrovat hello connector pomocí objekt pověření prostředí Windows PowerShell
1. Vytvořte objekt pověření prostředí PowerShell systému Windows hello spuštěním tohoto příkazu. Nahraďte  *\<uživatelské jméno\>*  a  *\<heslo\>*  s hello uživatelské jméno a heslo pro svůj adresář:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Přejděte příliš**konektoru Proxy C:\Program Files\Microsoft AAD aplikace** a spusťte skript hello pomocí prostředí PowerShell hello přihlašovací údaje objektu jste vytvořili. Nahraďte *$cred* s názvem hello hello prostředí PowerShell přihlašovací údaje objektu jste vytvořili:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-hello-connector-using-a-token-created-offline"></a>Zaregistrovat hello connector pomocí token vytvořený v režimu offline
1. Vytvořte token offline pomocí třídy kontextu AuthenticationContext hello pomocí hodnoty hello ve fragmentu kódu hello:

        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// hello AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.microsoftonline.com/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// hello application ID of hello connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// hello reply address of hello connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// hello AppIdUri of hello registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }


2. Jakmile máte hello token, vytvořte SecureString pomocí tokenu hello:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Hello spusťte následující příkaz prostředí Windows PowerShell, nahraďte \<klienta GUID\> s ID vašeho adresáře:

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a>Další kroky 
* [Publikování aplikací s použitím vlastního názvu domény](active-directory-application-proxy-custom-domains.md)
* [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
* [Řešení problémů, které máte s pomocí Proxy aplikace](active-directory-application-proxy-troubleshoot.md)


