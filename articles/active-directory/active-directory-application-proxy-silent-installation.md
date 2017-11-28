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
# <a name="silently-install-hello-azure-ad-application-proxy-connector"></a><span data-ttu-id="f4b71-103">Bezobslužná instalace hello konektoru Proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f4b71-103">Silently install hello Azure AD Application Proxy Connector</span></span>
<span data-ttu-id="f4b71-104">Chcete mít toosend toobe instalace skript toomultiple Windows servery nebo tooWindows servery, které nemají povolené uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f4b71-104">You want toobe able toosend an installation script toomultiple Windows servers or tooWindows Servers that don't have user interface enabled.</span></span> <span data-ttu-id="f4b71-105">Toto téma vám pomůže vytvořit skript prostředí Windows PowerShell, která umožňuje bezobslužné instalace a registrace pro Azure konektor Proxy aplikace AD.</span><span class="sxs-lookup"><span data-stu-id="f4b71-105">This topic helps you create a Windows PowerShell script that enables unattended installation and registration for your Azure AD Application Proxy Connector.</span></span>

<span data-ttu-id="f4b71-106">Tato možnost je užitečná, když chcete:</span><span class="sxs-lookup"><span data-stu-id="f4b71-106">This capability is useful when you want to:</span></span>

* <span data-ttu-id="f4b71-107">Na počítače bez uživatelského rozhraní vrstvy, nebo když nelze RDP toohello počítač nainstalujte konektor hello.</span><span class="sxs-lookup"><span data-stu-id="f4b71-107">Install hello connector on machines with no UI layer or when you cannot RDP toohello machine.</span></span>
* <span data-ttu-id="f4b71-108">Nainstalujte a zaregistrujte mnoho konektory najednou.</span><span class="sxs-lookup"><span data-stu-id="f4b71-108">Install and register many connectors at once.</span></span>
* <span data-ttu-id="f4b71-109">Integrate hello konektor instalace a registrace jako součást jiného režimu.</span><span class="sxs-lookup"><span data-stu-id="f4b71-109">Integrate hello connector installation and registration as part of another procedure.</span></span>
* <span data-ttu-id="f4b71-110">Vytvořte standardní serverovou bitovou kopii, která obsahuje hello konektor bits, ale není registrovaný.</span><span class="sxs-lookup"><span data-stu-id="f4b71-110">Create a standard server image that contains hello connector bits but is not registered.</span></span>

<span data-ttu-id="f4b71-111">Proxy aplikací funguje tak, že instalace tenký služba systému Windows Server s názvem hello konektor uvnitř vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="f4b71-111">Application Proxy works by installing a slim Windows Server service called hello Connector inside your network.</span></span> <span data-ttu-id="f4b71-112">Toowork konektor Proxy aplikace hello má toobe zaregistrována adresáře Azure AD pomocí globálního správce a hesla.</span><span class="sxs-lookup"><span data-stu-id="f4b71-112">For hello Application Proxy Connector toowork it has toobe registered with your Azure AD directory using a global administrator and password.</span></span> <span data-ttu-id="f4b71-113">Tyto informace se normálně zadá během instalace konektoru v dialogovém okně automaticky otevírané okno.</span><span class="sxs-lookup"><span data-stu-id="f4b71-113">Ordinarily this information is entered during Connector installation in a pop-up dialog box.</span></span> <span data-ttu-id="f4b71-114">Můžete však použít toocreate prostředí Windows PowerShell přihlašovací údaje objektu tooenter informace o registraci.</span><span class="sxs-lookup"><span data-stu-id="f4b71-114">However, you can use Windows PowerShell toocreate a credential object tooenter your registration information.</span></span> <span data-ttu-id="f4b71-115">Nebo můžete vytvořit vlastní token a použít ho tooenter informace o registraci.</span><span class="sxs-lookup"><span data-stu-id="f4b71-115">Or you can create your own token and use it tooenter your registration information.</span></span>

## <a name="install-hello-connector"></a><span data-ttu-id="f4b71-116">Instalace konektoru hello</span><span class="sxs-lookup"><span data-stu-id="f4b71-116">Install hello connector</span></span>
<span data-ttu-id="f4b71-117">Instalace konektoru hello souborů MSI bez registrace hello konektor následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f4b71-117">Install hello Connector MSIs without registering hello Connector as follows:</span></span>

1. <span data-ttu-id="f4b71-118">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="f4b71-118">Open a command prompt.</span></span>
2. <span data-ttu-id="f4b71-119">Spusťte následující příkaz, ve které hello /q znamená tichá instalace hello – hello instalace nevyzve tooaccept hello licenční smlouva s koncovým uživatelem.</span><span class="sxs-lookup"><span data-stu-id="f4b71-119">Run hello following command in which hello /q means quiet installation - hello installation doesn't prompt you tooaccept hello End-User License Agreement.</span></span>
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-hello-connector-with-azure-ad"></a><span data-ttu-id="f4b71-120">Registrace konektoru hello s Azure AD</span><span class="sxs-lookup"><span data-stu-id="f4b71-120">Register hello connector with Azure AD</span></span>
<span data-ttu-id="f4b71-121">Můžete použít konektor hello tooregister dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="f4b71-121">There are two methods you can use tooregister hello connector:</span></span>

* <span data-ttu-id="f4b71-122">Zaregistrovat hello connector pomocí objekt pověření prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="f4b71-122">Register hello connector using a Windows PowerShell credential object</span></span>
* <span data-ttu-id="f4b71-123">Zaregistrovat hello connector pomocí token vytvořený v režimu offline</span><span class="sxs-lookup"><span data-stu-id="f4b71-123">Register hello connector using a token created offline</span></span>

### <a name="register-hello-connector-using-a-windows-powershell-credential-object"></a><span data-ttu-id="f4b71-124">Zaregistrovat hello connector pomocí objekt pověření prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="f4b71-124">Register hello connector using a Windows PowerShell credential object</span></span>
1. <span data-ttu-id="f4b71-125">Vytvořte objekt pověření prostředí PowerShell systému Windows hello spuštěním tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="f4b71-125">Create hello Windows PowerShell Credentials object by running this command.</span></span> <span data-ttu-id="f4b71-126">Nahraďte  *\<uživatelské jméno\>*  a  *\<heslo\>*  s hello uživatelské jméno a heslo pro svůj adresář:</span><span class="sxs-lookup"><span data-stu-id="f4b71-126">Replace *\<username\>* and *\<password\>* with hello username and password for your directory:</span></span>
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. <span data-ttu-id="f4b71-127">Přejděte příliš**konektoru Proxy C:\Program Files\Microsoft AAD aplikace** a spusťte skript hello pomocí prostředí PowerShell hello přihlašovací údaje objektu jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f4b71-127">Go too**C:\Program Files\Microsoft AAD App Proxy Connector** and run hello script using hello PowerShell credentials object you created.</span></span> <span data-ttu-id="f4b71-128">Nahraďte *$cred* s názvem hello hello prostředí PowerShell přihlašovací údaje objektu jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="f4b71-128">Replace *$cred* with hello name of hello PowerShell credentials object you created:</span></span>
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-hello-connector-using-a-token-created-offline"></a><span data-ttu-id="f4b71-129">Zaregistrovat hello connector pomocí token vytvořený v režimu offline</span><span class="sxs-lookup"><span data-stu-id="f4b71-129">Register hello connector using a token created offline</span></span>
1. <span data-ttu-id="f4b71-130">Vytvořte token offline pomocí třídy kontextu AuthenticationContext hello pomocí hodnoty hello ve fragmentu kódu hello:</span><span class="sxs-lookup"><span data-stu-id="f4b71-130">Create an offline token using hello AuthenticationContext class using hello values in hello code snippet:</span></span>

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


2. <span data-ttu-id="f4b71-131">Jakmile máte hello token, vytvořte SecureString pomocí tokenu hello:</span><span class="sxs-lookup"><span data-stu-id="f4b71-131">Once you have hello token, create a SecureString using hello token:</span></span>

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. <span data-ttu-id="f4b71-132">Hello spusťte následující příkaz prostředí Windows PowerShell, nahraďte \<klienta GUID\> s ID vašeho adresáře:</span><span class="sxs-lookup"><span data-stu-id="f4b71-132">Run hello following Windows PowerShell command, replacing \<tenant GUID\> with your directory ID:</span></span>

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a><span data-ttu-id="f4b71-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4b71-133">Next steps</span></span> 
* [<span data-ttu-id="f4b71-134">Publikování aplikací s použitím vlastního názvu domény</span><span class="sxs-lookup"><span data-stu-id="f4b71-134">Publish applications using your own domain name</span></span>](active-directory-application-proxy-custom-domains.md)
* [<span data-ttu-id="f4b71-135">Povolení jednoduchého přihlášení</span><span class="sxs-lookup"><span data-stu-id="f4b71-135">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
* [<span data-ttu-id="f4b71-136">Řešení problémů, které máte s pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="f4b71-136">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)


