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
# <a name="silently-install-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="3678a-103">Tiché instalaci konektoru Proxy aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="3678a-103">Silently install the Azure AD Application Proxy Connector</span></span>
<span data-ttu-id="3678a-104">Chcete být schopni poslat instalační skript pro více serverů systému Windows nebo Windows servery, které nemají povolené uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3678a-104">You want to be able to send an installation script to multiple Windows servers or to Windows Servers that don't have user interface enabled.</span></span> <span data-ttu-id="3678a-105">Toto téma vám pomůže vytvořit skript prostředí Windows PowerShell, která umožňuje bezobslužné instalace a registrace pro Azure konektor Proxy aplikace AD.</span><span class="sxs-lookup"><span data-stu-id="3678a-105">This topic helps you create a Windows PowerShell script that enables unattended installation and registration for your Azure AD Application Proxy Connector.</span></span>

<span data-ttu-id="3678a-106">Tato možnost je užitečná, když chcete:</span><span class="sxs-lookup"><span data-stu-id="3678a-106">This capability is useful when you want to:</span></span>

* <span data-ttu-id="3678a-107">Konektor nainstalujte na počítače bez uživatelského rozhraní vrstvy, nebo když nelze RDP k počítači.</span><span class="sxs-lookup"><span data-stu-id="3678a-107">Install the connector on machines with no UI layer or when you cannot RDP to the machine.</span></span>
* <span data-ttu-id="3678a-108">Nainstalujte a zaregistrujte mnoho konektory najednou.</span><span class="sxs-lookup"><span data-stu-id="3678a-108">Install and register many connectors at once.</span></span>
* <span data-ttu-id="3678a-109">Integrate instalace konektoru a registraci v rámci jiného režimu.</span><span class="sxs-lookup"><span data-stu-id="3678a-109">Integrate the connector installation and registration as part of another procedure.</span></span>
* <span data-ttu-id="3678a-110">Vytvořte standardní serverovou bitovou kopii, která obsahuje službu bits konektor, ale není registrovaný.</span><span class="sxs-lookup"><span data-stu-id="3678a-110">Create a standard server image that contains the connector bits but is not registered.</span></span>

<span data-ttu-id="3678a-111">Proxy aplikací funguje tak, že instalace tenký služba systému Windows Server s názvem konektor uvnitř vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="3678a-111">Application Proxy works by installing a slim Windows Server service called the Connector inside your network.</span></span> <span data-ttu-id="3678a-112">Pro konektor Proxy aplikace pro práci se má být registrováno v adresáři služby Azure AD pomocí globálního správce a hesla.</span><span class="sxs-lookup"><span data-stu-id="3678a-112">For the Application Proxy Connector to work it has to be registered with your Azure AD directory using a global administrator and password.</span></span> <span data-ttu-id="3678a-113">Tyto informace se normálně zadá během instalace konektoru v dialogovém okně automaticky otevírané okno.</span><span class="sxs-lookup"><span data-stu-id="3678a-113">Ordinarily this information is entered during Connector installation in a pop-up dialog box.</span></span> <span data-ttu-id="3678a-114">Můžete však použít prostředí Windows PowerShell vytvořit objekt přihlašovacích údajů k zadání informace o registraci.</span><span class="sxs-lookup"><span data-stu-id="3678a-114">However, you can use Windows PowerShell to create a credential object to enter your registration information.</span></span> <span data-ttu-id="3678a-115">Nebo můžete vytvořit vlastní token a použít ho k zadání informace o registraci.</span><span class="sxs-lookup"><span data-stu-id="3678a-115">Or you can create your own token and use it to enter your registration information.</span></span>

## <a name="install-the-connector"></a><span data-ttu-id="3678a-116">Instalace konektoru</span><span class="sxs-lookup"><span data-stu-id="3678a-116">Install the connector</span></span>
<span data-ttu-id="3678a-117">Instalace konektoru souborů MSI bez registrace konektoru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3678a-117">Install the Connector MSIs without registering the Connector as follows:</span></span>

1. <span data-ttu-id="3678a-118">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="3678a-118">Open a command prompt.</span></span>
2. <span data-ttu-id="3678a-119">Spusťte následující příkaz, ve kterém /q znamená tichá instalace – instalace nevyzve přijmout licenční smlouvu s koncovým uživatelem.</span><span class="sxs-lookup"><span data-stu-id="3678a-119">Run the following command in which the /q means quiet installation - the installation doesn't prompt you to accept the End-User License Agreement.</span></span>
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a><span data-ttu-id="3678a-120">Registrace konektoru s Azure AD</span><span class="sxs-lookup"><span data-stu-id="3678a-120">Register the connector with Azure AD</span></span>
<span data-ttu-id="3678a-121">Existují dvě metody, které můžete použít k registraci konektoru:</span><span class="sxs-lookup"><span data-stu-id="3678a-121">There are two methods you can use to register the connector:</span></span>

* <span data-ttu-id="3678a-122">Registrace konektoru pomocí objekt pověření prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="3678a-122">Register the connector using a Windows PowerShell credential object</span></span>
* <span data-ttu-id="3678a-123">Registrace konektoru pomocí token vytvořený v režimu offline</span><span class="sxs-lookup"><span data-stu-id="3678a-123">Register the connector using a token created offline</span></span>

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a><span data-ttu-id="3678a-124">Registrace konektoru pomocí objekt pověření prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="3678a-124">Register the connector using a Windows PowerShell credential object</span></span>
1. <span data-ttu-id="3678a-125">Vytvořte objekt pověření prostředí PowerShell systému Windows tak, že spustíte tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="3678a-125">Create the Windows PowerShell Credentials object by running this command.</span></span> <span data-ttu-id="3678a-126">Nahraďte  *\<uživatelské jméno\>*  a  *\<heslo\>*  pomocí uživatelského jména a hesla pro váš adresář:</span><span class="sxs-lookup"><span data-stu-id="3678a-126">Replace *\<username\>* and *\<password\>* with the username and password for your directory:</span></span>
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. <span data-ttu-id="3678a-127">Přejděte na **konektoru Proxy C:\Program Files\Microsoft AAD aplikace** a spusťte skript prostředí PowerShell pomocí přihlašovacích údajů objektu jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3678a-127">Go to **C:\Program Files\Microsoft AAD App Proxy Connector** and run the script using the PowerShell credentials object you created.</span></span> <span data-ttu-id="3678a-128">Nahraďte *$cred* s názvem PowerShell přihlašovací údaje objektu jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="3678a-128">Replace *$cred* with the name of the PowerShell credentials object you created:</span></span>
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-the-connector-using-a-token-created-offline"></a><span data-ttu-id="3678a-129">Registrace konektoru pomocí token vytvořený v režimu offline</span><span class="sxs-lookup"><span data-stu-id="3678a-129">Register the connector using a token created offline</span></span>
1. <span data-ttu-id="3678a-130">Vytvořte token offline pomocí třídy kontextu AuthenticationContext pomocí hodnot ve fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="3678a-130">Create an offline token using the AuthenticationContext class using the values in the code snippet:</span></span>

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


2. <span data-ttu-id="3678a-131">Jakmile máte token, vytvořte SecureString pomocí tokenu:</span><span class="sxs-lookup"><span data-stu-id="3678a-131">Once you have the token, create a SecureString using the token:</span></span>

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. <span data-ttu-id="3678a-132">Spusťte následující příkaz prostředí Windows PowerShell, nahraďte \<klienta GUID\> s ID vašeho adresáře:</span><span class="sxs-lookup"><span data-stu-id="3678a-132">Run the following Windows PowerShell command, replacing \<tenant GUID\> with your directory ID:</span></span>

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a><span data-ttu-id="3678a-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3678a-133">Next steps</span></span> 
* [<span data-ttu-id="3678a-134">Publikování aplikací s použitím vlastního názvu domény</span><span class="sxs-lookup"><span data-stu-id="3678a-134">Publish applications using your own domain name</span></span>](active-directory-application-proxy-custom-domains.md)
* [<span data-ttu-id="3678a-135">Povolení jednoduchého přihlášení</span><span class="sxs-lookup"><span data-stu-id="3678a-135">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
* [<span data-ttu-id="3678a-136">Řešení problémů, které máte s pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="3678a-136">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)


