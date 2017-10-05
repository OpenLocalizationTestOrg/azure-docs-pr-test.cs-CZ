---
title: "Bezobslužnou instalaci konektoru Proxy aplikace Azure AD | Microsoft Docs"
description: "Popisuje, jak provést bezobslužnou instalaci konektoru Proxy aplikace Azure AD poskytnout zabezpečený vzdálený přístup k místní aplikace."
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
ms.openlocfilehash: 9e28c89d8f64f0ae3d4150017ca544e606075c45
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="silently-install-the-azure-ad-application-proxy-connector"></a>Tiché instalaci konektoru Proxy aplikace Azure AD
Chcete být schopni poslat instalační skript pro více serverů systému Windows nebo Windows servery, které nemají povolené uživatelské rozhraní. Toto téma vám pomůže vytvořit skript prostředí Windows PowerShell, která umožňuje bezobslužné instalace a registrace pro Azure konektor Proxy aplikace AD.

Tato možnost je užitečná, když chcete:

* Konektor nainstalujte na počítače bez uživatelského rozhraní vrstvy, nebo když nelze RDP k počítači.
* Nainstalujte a zaregistrujte mnoho konektory najednou.
* Integrate instalace konektoru a registraci v rámci jiného režimu.
* Vytvořte standardní serverovou bitovou kopii, která obsahuje službu bits konektor, ale není registrovaný.

Proxy aplikací funguje tak, že instalace tenký služba systému Windows Server s názvem konektor uvnitř vaší sítě. Pro konektor Proxy aplikace pro práci se má být registrováno v adresáři služby Azure AD pomocí globálního správce a hesla. Tyto informace se normálně zadá během instalace konektoru v dialogovém okně automaticky otevírané okno. Můžete však použít prostředí Windows PowerShell vytvořit objekt přihlašovacích údajů k zadání informace o registraci. Nebo můžete vytvořit vlastní token a použít ho k zadání informace o registraci.

## <a name="install-the-connector"></a>Instalace konektoru
Instalace konektoru souborů MSI bez registrace konektoru následujícím způsobem:

1. Otevřete příkazový řádek.
2. Spusťte následující příkaz, ve kterém /q znamená tichá instalace – instalace nevyzve přijmout licenční smlouvu s koncovým uživatelem.
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a>Registrace konektoru s Azure AD
Existují dvě metody, které můžete použít k registraci konektoru:

* Registrace konektoru pomocí objekt pověření prostředí Windows PowerShell
* Registrace konektoru pomocí token vytvořený v režimu offline

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Registrace konektoru pomocí objekt pověření prostředí Windows PowerShell
1. Vytvořte objekt pověření prostředí PowerShell systému Windows tak, že spustíte tento příkaz. Nahraďte  *\<uživatelské jméno\>*  a  *\<heslo\>*  pomocí uživatelského jména a hesla pro váš adresář:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Přejděte na **konektoru Proxy C:\Program Files\Microsoft AAD aplikace** a spusťte skript prostředí PowerShell pomocí přihlašovacích údajů objektu jste vytvořili. Nahraďte *$cred* s názvem PowerShell přihlašovací údaje objektu jste vytvořili:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-the-connector-using-a-token-created-offline"></a>Registrace konektoru pomocí token vytvořený v režimu offline
1. Vytvořte token offline pomocí třídy kontextu AuthenticationContext pomocí hodnot ve fragmentu kódu:

        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.microsoftonline.com/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
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


2. Jakmile máte token, vytvořte SecureString pomocí tokenu:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Spusťte následující příkaz prostředí Windows PowerShell, nahraďte \<klienta GUID\> s ID vašeho adresáře:

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a>Další kroky 
* [Publikování aplikací s použitím vlastního názvu domény](active-directory-application-proxy-custom-domains.md)
* [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
* [Řešení problémů, které máte s pomocí Proxy aplikace](active-directory-application-proxy-troubleshoot.md)


