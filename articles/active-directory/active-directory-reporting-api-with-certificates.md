---
title: "aaaGet dat pomocí Azure AD Reporting rozhraní API s certifikáty hello | Microsoft Docs"
description: "Vysvětluje, jak toouse hello Azure AD Reporting rozhraní API s daty tooget certifikát přihlašovacích údajů z adresáře bez zásahu uživatele."
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
ms.openlocfilehash: 00ddfaefe32ea6ae48f276c974a17ddcf84f7894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-data-using-hello-azure-ad-reporting-api-with-certificates"></a><span data-ttu-id="d580f-103">Získání dat pomocí rozhraní API generování sestav Azure AD hello s certifikáty</span><span class="sxs-lookup"><span data-stu-id="d580f-103">Get data using hello Azure AD Reporting API with certificates</span></span>
<span data-ttu-id="d580f-104">Tento článek popisuje, jak toouse hello Azure AD Reporting rozhraní API s daty tooget certifikát přihlašovacích údajů z adresáře bez zásahu uživatele.</span><span class="sxs-lookup"><span data-stu-id="d580f-104">This article discusses how toouse hello Azure AD Reporting API with certificate credentials tooget data from directories without user intervention.</span></span> 

## <a name="use-hello-azure-ad-reporting-api"></a><span data-ttu-id="d580f-105">Použití hello rozhraní API pro vytváření sestav Azure AD</span><span class="sxs-lookup"><span data-stu-id="d580f-105">Use hello Azure AD Reporting API</span></span> 
<span data-ttu-id="d580f-106">Rozhraní API pro vytváření sestav Azure AD vyžaduje, že provedete následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d580f-106">Azure AD Reporting API requires that you complete hello following steps:</span></span>
 *  <span data-ttu-id="d580f-107">Požadavky na instalaci</span><span class="sxs-lookup"><span data-stu-id="d580f-107">Install prerequisites</span></span>
 *  <span data-ttu-id="d580f-108">Nastavte hello certifikát v aplikaci</span><span class="sxs-lookup"><span data-stu-id="d580f-108">Set hello certificate in your app</span></span>
 *  <span data-ttu-id="d580f-109">Získání přístupového tokenu</span><span class="sxs-lookup"><span data-stu-id="d580f-109">Get an access token</span></span>
 *  <span data-ttu-id="d580f-110">Použití hello přístup tokenu toocall hello rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="d580f-110">Use hello access token toocall hello Graph API</span></span>

<span data-ttu-id="d580f-111">Informace o zdrojovém kódu najdete v tématu popisujícím [využití modulu rozhraní API pro sestavy](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils).</span><span class="sxs-lookup"><span data-stu-id="d580f-111">For information about source code, see [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils).</span></span> 

### <a name="install-prerequisites"></a><span data-ttu-id="d580f-112">Požadavky na instalaci</span><span class="sxs-lookup"><span data-stu-id="d580f-112">Install prerequisites</span></span>
<span data-ttu-id="d580f-113">Budete potřebovat toohave Azure AD PowerShell V2 a AzureADUtils modul nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="d580f-113">You will need toohave Azure AD PowerShell V2 and AzureADUtils module installed.</span></span>

1. <span data-ttu-id="d580f-114">Stáhněte a nainstalujte Azure AD Powershell V2, pokynů hello v [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).</span><span class="sxs-lookup"><span data-stu-id="d580f-114">Download and install Azure AD Powershell V2, following hello instructions at [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).</span></span>
2. <span data-ttu-id="d580f-115">Stáhnout modul Azure AD Utils hello z [AzureAD/azure-Active Directory powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1).</span><span class="sxs-lookup"><span data-stu-id="d580f-115">Download hello Azure AD Utils module from [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1).</span></span> 
  <span data-ttu-id="d580f-116">Tento modul poskytuje několik rutin nástrojů, jako jsou:</span><span class="sxs-lookup"><span data-stu-id="d580f-116">This module provides several utility cmdlets including:</span></span>
   * <span data-ttu-id="d580f-117">Hello nejnovější verzi pomocí nástroje Nuget ADAL</span><span class="sxs-lookup"><span data-stu-id="d580f-117">hello latest version of ADAL using Nuget</span></span>
   * <span data-ttu-id="d580f-118">Přístupové tokeny od uživatele, klíče aplikace a certifikáty pomocí ADAL</span><span class="sxs-lookup"><span data-stu-id="d580f-118">Access tokens from user, application keys, and certificates using ADAL</span></span>
   * <span data-ttu-id="d580f-119">Rozhraní Graph API zpracovávající stránkové výsledky</span><span class="sxs-lookup"><span data-stu-id="d580f-119">Graph API handling paged results</span></span>

<span data-ttu-id="d580f-120">**modul Azure AD Utils tooinstall hello:**</span><span class="sxs-lookup"><span data-stu-id="d580f-120">**tooinstall hello Azure AD Utils module:**</span></span>

1. <span data-ttu-id="d580f-121">Vytvořte modul directory toosave hello nástroje (například c:\azureAD) a modulu hello stáhnout z webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="d580f-121">Create a directory toosave hello utilities module (for example, c:\azureAD) and download hello module from GitHub.</span></span>
2. <span data-ttu-id="d580f-122">Otevřete relaci prostředí PowerShell a přejděte toohello adresář, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d580f-122">Open a PowerShell session, and go toohello directory you just created.</span></span> 
3. <span data-ttu-id="d580f-123">Naimportovat modul hello a nainstalujte ho v cestě modulu prostředí PowerShell hello pomocí rutiny Install-AzureADUtilsModule hello.</span><span class="sxs-lookup"><span data-stu-id="d580f-123">Import hello module, and install it in hello PowerShell module path using hello Install-AzureADUtilsModule cmdlet.</span></span> 

<span data-ttu-id="d580f-124">relace Hello by měl vypadat podobně jako toothis obrazovky:</span><span class="sxs-lookup"><span data-stu-id="d580f-124">hello session should look similar toothis screen:</span></span>

  ![Windows Powershell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-hello-certificate-in-your-app"></a><span data-ttu-id="d580f-126">Nastavte hello certifikát v aplikaci</span><span class="sxs-lookup"><span data-stu-id="d580f-126">Set hello certificate in your app</span></span>
1. <span data-ttu-id="d580f-127">Pokud již máte aplikaci, získáte jeho ID objektu z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d580f-127">If you already have an app, get its Object ID from hello Azure Portal.</span></span> 

  ![portál Azure](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. <span data-ttu-id="d580f-129">Otevřete relaci prostředí PowerShell a připojte tooAzure AD pomocí rutiny hello Connect-AzureAD.</span><span class="sxs-lookup"><span data-stu-id="d580f-129">Open a PowerShell session and connect tooAzure AD using hello Connect-AzureAD cmdlet.</span></span>

  ![portál Azure](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. <span data-ttu-id="d580f-131">Pomocí rutiny New-AzureADApplicationCertificateCredential hello z AzureADUtils tooadd tooit přihlašovacích údajů certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d580f-131">Use hello New-AzureADApplicationCertificateCredential cmdlet from AzureADUtils tooadd a certificate credential tooit.</span></span> 

>[!Note]
><span data-ttu-id="d580f-132">Budete potřebovat aplikace hello tooprovide ID objektu, kterou jste zachytili dříve, jakož i hello objekt certifikátu (získat této konfigurace pomocí hello Cert: jednotku).</span><span class="sxs-lookup"><span data-stu-id="d580f-132">You need tooprovide hello application Object ID that you captured earlier, as well as hello certificate object (get this using hello Cert: drive).</span></span>
>


  ![portál Azure](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a><span data-ttu-id="d580f-134">Získání přístupového tokenu</span><span class="sxs-lookup"><span data-stu-id="d580f-134">Get an access token</span></span>

<span data-ttu-id="d580f-135">tooget přístupový token, použijte rutinu Get-AzureADGraphAPIAccessTokenFromCert hello z AzureADUtils.</span><span class="sxs-lookup"><span data-stu-id="d580f-135">tooget an access token, use hello Get-AzureADGraphAPIAccessTokenFromCert cmdlet from AzureADUtils.</span></span> 

>[!NOTE]
><span data-ttu-id="d580f-136">Je nutné toouse hello ID aplikace místo hello ID objektu, který jste použili v poslední části hello.</span><span class="sxs-lookup"><span data-stu-id="d580f-136">You need toouse hello Application ID instead of hello Object ID that you used in hello last section.</span></span>
>

 ![portál Azure](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-hello-access-token-toocall-hello-graph-api"></a><span data-ttu-id="d580f-138">Použití hello přístup tokenu toocall hello rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="d580f-138">Use hello access token toocall hello Graph API</span></span>

<span data-ttu-id="d580f-139">Nyní můžete vytvořit skript hello.</span><span class="sxs-lookup"><span data-stu-id="d580f-139">Now you can create hello script.</span></span> <span data-ttu-id="d580f-140">Dole je příklad pomocí rutiny Invoke-AzureADGraphAPIQuery hello z hello AzureADUtils.</span><span class="sxs-lookup"><span data-stu-id="d580f-140">Below is an example using hello Invoke-AzureADGraphAPIQuery cmdlet from hello AzureADUtils.</span></span> <span data-ttu-id="d580f-141">Tato rutina zpracovává více stránkových výsledků a pak odešle tyto výsledky toohello prostředí PowerShell kanálu.</span><span class="sxs-lookup"><span data-stu-id="d580f-141">This cmdlet handles multi-paged results, and then sends those results toohello PowerShell pipeline.</span></span> 

 ![portál Azure](./media/active-directory-report-api-with-certificates/script-completed.png)

<span data-ttu-id="d580f-143">Jsou nyní připraven tooexport tooa sdíleného svazku clusteru a uložte tooa systému SIEM.</span><span class="sxs-lookup"><span data-stu-id="d580f-143">You are now ready tooexport tooa CSV and save tooa SIEM system.</span></span> <span data-ttu-id="d580f-144">Můžete také zabalit vašeho skriptu do tooget naplánované úlohy služby Azure AD data z vašeho klienta pravidelně bez nutnosti toostore aplikace klíče ve zdrojovém kódu hello.</span><span class="sxs-lookup"><span data-stu-id="d580f-144">You can also wrap your script in a scheduled task tooget Azure AD data from your tenant periodically without having toostore application keys in hello source code.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d580f-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d580f-145">Next steps</span></span>
[<span data-ttu-id="d580f-146">Hello základy Azure identity management.</span><span class="sxs-lookup"><span data-stu-id="d580f-146">hello fundamentals of Azure identity management</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>



