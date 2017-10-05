---
title: "Získání dat pomocí rozhraní API pro generování sestav Azure AD s certifikáty | Dokumentace Microsoftu"
description: "Vysvětluje, jak používat rozhraní API pro generování sestav Azure AD s přihlašovacími údaji ve formě certifikátů k získání dat z adresářů bez zásahu uživatele."
services: active-directory
documentationcenter: 
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2017
ms.author: ramical
ms.openlocfilehash: c1345dcda6e52267a8037ffd7207e6bc3b0d3b31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-data-using-the-azure-ad-reporting-api-with-certificates"></a><span data-ttu-id="30704-103">Získání dat pomocí rozhraní API pro generování sestav Azure AD s certifikáty</span><span class="sxs-lookup"><span data-stu-id="30704-103">Get data using the Azure AD Reporting API with certificates</span></span>
<span data-ttu-id="30704-104">Tento článek probírá, jak používat rozhraní API pro generování sestav Azure AD s přihlašovacími údaji ve formě certifikátů k získání dat z adresářů bez zásahu uživatele.</span><span class="sxs-lookup"><span data-stu-id="30704-104">This article discusses how to use the Azure AD Reporting API with certificate credentials to get data from directories without user intervention.</span></span> 

## <a name="use-the-azure-ad-reporting-api"></a><span data-ttu-id="30704-105">Použití rozhraní API pro vytváření sestav Azure AD</span><span class="sxs-lookup"><span data-stu-id="30704-105">Use the Azure AD Reporting API</span></span> 
<span data-ttu-id="30704-106">Rozhraní API pro vytváření sestav Azure AD vyžaduje, abyste provedli následující kroky:</span><span class="sxs-lookup"><span data-stu-id="30704-106">Azure AD Reporting API requires that you complete the following steps:</span></span>
 *  <span data-ttu-id="30704-107">Požadavky na instalaci</span><span class="sxs-lookup"><span data-stu-id="30704-107">Install prerequisites</span></span>
 *  <span data-ttu-id="30704-108">Nastavení certifikátu v aplikaci</span><span class="sxs-lookup"><span data-stu-id="30704-108">Set the certificate in your app</span></span>
 *  <span data-ttu-id="30704-109">Získání přístupového tokenu</span><span class="sxs-lookup"><span data-stu-id="30704-109">Get an access token</span></span>
 *  <span data-ttu-id="30704-110">Použití přístupového tokenu pro volání rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="30704-110">Use the access token to call the Graph API</span></span>

<span data-ttu-id="30704-111">Informace o zdrojovém kódu najdete v tématu popisujícím [využití modulu rozhraní API pro sestavy](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils).</span><span class="sxs-lookup"><span data-stu-id="30704-111">For information about source code, see [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils).</span></span> 

### <a name="install-prerequisites"></a><span data-ttu-id="30704-112">Požadavky na instalaci</span><span class="sxs-lookup"><span data-stu-id="30704-112">Install prerequisites</span></span>
<span data-ttu-id="30704-113">Musíte mít nainstalované Azure AD PowerShell verze 2 a modul AzureADUtils.</span><span class="sxs-lookup"><span data-stu-id="30704-113">You will need to have Azure AD PowerShell V2 and AzureADUtils module installed.</span></span>

1. <span data-ttu-id="30704-114">Stáhněte a nainstalujte Azure AD PowerShell verze 2 podle pokynů pro [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).</span><span class="sxs-lookup"><span data-stu-id="30704-114">Download and install Azure AD Powershell V2, following the instructions at [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).</span></span>
2. <span data-ttu-id="30704-115">Stáhněte modul Azure AD Utils z umístění [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1).</span><span class="sxs-lookup"><span data-stu-id="30704-115">Download the Azure AD Utils module from [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1).</span></span> 
  <span data-ttu-id="30704-116">Tento modul poskytuje několik rutin nástrojů, jako jsou:</span><span class="sxs-lookup"><span data-stu-id="30704-116">This module provides several utility cmdlets including:</span></span>
   * <span data-ttu-id="30704-117">Nejnovější verze ADAL pomocí Nugetu</span><span class="sxs-lookup"><span data-stu-id="30704-117">The latest version of ADAL using Nuget</span></span>
   * <span data-ttu-id="30704-118">Přístupové tokeny od uživatele, klíče aplikace a certifikáty pomocí ADAL</span><span class="sxs-lookup"><span data-stu-id="30704-118">Access tokens from user, application keys, and certificates using ADAL</span></span>
   * <span data-ttu-id="30704-119">Rozhraní Graph API zpracovávající stránkové výsledky</span><span class="sxs-lookup"><span data-stu-id="30704-119">Graph API handling paged results</span></span>

<span data-ttu-id="30704-120">**Instalace modulu Azure AD Utils:**</span><span class="sxs-lookup"><span data-stu-id="30704-120">**To install the Azure AD Utils module:**</span></span>

1. <span data-ttu-id="30704-121">Vytvořte adresář pro uložení modulu nástrojů (například c:\azureAD) a stáhněte modul z GitHubu.</span><span class="sxs-lookup"><span data-stu-id="30704-121">Create a directory to save the utilities module (for example, c:\azureAD) and download the module from GitHub.</span></span>
2. <span data-ttu-id="30704-122">Otevřete relaci PowerShellu a přejděte do adresáře, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="30704-122">Open a PowerShell session, and go to the directory you just created.</span></span> 
3. <span data-ttu-id="30704-123">Naimportujte modul a nainstalujte ho v cestě modulů PowerShellu pomocí rutiny Install-AzureADUtilsModule.</span><span class="sxs-lookup"><span data-stu-id="30704-123">Import the module, and install it in the PowerShell module path using the Install-AzureADUtilsModule cmdlet.</span></span> 

<span data-ttu-id="30704-124">Relace by měla vypadat podobně jako tato obrazovka:</span><span class="sxs-lookup"><span data-stu-id="30704-124">The session should look similar to this screen:</span></span>

  ![Windows Powershell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-the-certificate-in-your-app"></a><span data-ttu-id="30704-126">Nastavení certifikátu v aplikaci</span><span class="sxs-lookup"><span data-stu-id="30704-126">Set the certificate in your app</span></span>
1. <span data-ttu-id="30704-127">Pokud již máte aplikaci, získejte její ID objektu na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="30704-127">If you already have an app, get its Object ID from the Azure Portal.</span></span> 

  ![portál Azure](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. <span data-ttu-id="30704-129">Otevřete relaci PowerShellu a připojte se ke službě Azure AD pomocí rutiny Connect-AzureAD.</span><span class="sxs-lookup"><span data-stu-id="30704-129">Open a PowerShell session and connect to Azure AD using the Connect-AzureAD cmdlet.</span></span>

  ![portál Azure](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. <span data-ttu-id="30704-131">Pomocí rutiny New-AzureADApplicationCertificateCredential z AzureADUtils do ni přidejte přihlašovací údaje ve formě certifikátu.</span><span class="sxs-lookup"><span data-stu-id="30704-131">Use the New-AzureADApplicationCertificateCredential cmdlet from AzureADUtils to add a certificate credential to it.</span></span> 

>[!Note]
><span data-ttu-id="30704-132">Je třeba zadat ID objektu aplikace, který jste si poznamenali dříve, a také objekt certifikátu (ten získáte pomocí jednotky Cert:).</span><span class="sxs-lookup"><span data-stu-id="30704-132">You need to provide the application Object ID that you captured earlier, as well as the certificate object (get this using the Cert: drive).</span></span>
>


  ![portál Azure](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a><span data-ttu-id="30704-134">Získání přístupového tokenu</span><span class="sxs-lookup"><span data-stu-id="30704-134">Get an access token</span></span>

<span data-ttu-id="30704-135">Chcete-li získat přístupový token, použijte rutinu Get-AzureADGraphAPIAccessTokenFromCert z AzureADUtils.</span><span class="sxs-lookup"><span data-stu-id="30704-135">To get an access token, use the Get-AzureADGraphAPIAccessTokenFromCert cmdlet from AzureADUtils.</span></span> 

>[!NOTE]
><span data-ttu-id="30704-136">Budete muset použít ID aplikace, místo ID objektu použitého v poslední části.</span><span class="sxs-lookup"><span data-stu-id="30704-136">You need to use the Application ID instead of the Object ID that you used in the last section.</span></span>
>

 ![portál Azure](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-the-access-token-to-call-the-graph-api"></a><span data-ttu-id="30704-138">Použití přístupového tokenu pro volání rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="30704-138">Use the access token to call the Graph API</span></span>

<span data-ttu-id="30704-139">Nyní můžete vytvořit skript.</span><span class="sxs-lookup"><span data-stu-id="30704-139">Now you can create the script.</span></span> <span data-ttu-id="30704-140">Dále je uvedený příklad používající rutinu Invoke-AzureADGraphAPIQuery z AzureADUtils.</span><span class="sxs-lookup"><span data-stu-id="30704-140">Below is an example using the Invoke-AzureADGraphAPIQuery cmdlet from the AzureADUtils.</span></span> <span data-ttu-id="30704-141">Tato rutina zpracuje vícestránkové výsledky a pak tyto výsledky odešle do kanálu PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="30704-141">This cmdlet handles multi-paged results, and then sends those results to the PowerShell pipeline.</span></span> 

 ![portál Azure](./media/active-directory-report-api-with-certificates/script-completed.png)

<span data-ttu-id="30704-143">Nyní jste připraveni k exportu do souboru CSV a jeho uložení do systému SIEM.</span><span class="sxs-lookup"><span data-stu-id="30704-143">You are now ready to export to a CSV and save to a SIEM system.</span></span> <span data-ttu-id="30704-144">Můžete také zabalit váš skript do naplánované úlohy, abyste získávali data Azure AD z vašeho klienta pravidelně bez nutnosti ukládat klíče aplikace ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="30704-144">You can also wrap your script in a scheduled task to get Azure AD data from your tenant periodically without having to store application keys in the source code.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="30704-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30704-145">Next steps</span></span>
[<span data-ttu-id="30704-146">Základy správy identit Azure</span><span class="sxs-lookup"><span data-stu-id="30704-146">The fundamentals of Azure identity management</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>



