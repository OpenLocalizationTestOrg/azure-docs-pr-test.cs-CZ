---
title: "Nasazení aplikace do Azure App Service pomocí FTP/S | Microsoft Docs"
description: "Informace o nasazení aplikace do služby Azure App Service pomocí FTP a FTPS."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2016
ms.author: cephalin;dariac
ms.openlocfilehash: 9078abbc4ed7eff6975201443992f7bbb84bf57c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a><span data-ttu-id="3fb2c-103">Nasazení aplikace do Azure App Service pomocí FTP/S</span><span class="sxs-lookup"><span data-stu-id="3fb2c-103">Deploy your app to Azure App Service using FTP/S</span></span>

<span data-ttu-id="3fb2c-104">Tento článek ukazuje, jak použít protokol FTP nebo FTPS k nasazení vaší webové aplikace, back-end mobilní aplikace nebo aplikaci API [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="3fb2c-104">This article shows you how to use FTP or FTPS to deploy your web app, mobile app backend, or API app to [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="3fb2c-105">Koncový bod FTP/S pro aplikace je již aktivní.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-105">The FTP/S endpoint for your app is already active.</span></span> <span data-ttu-id="3fb2c-106">Žádná konfigurace je nutná pro povolení nasazení FTP/S.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-106">No configuration is necessary to enable FTP/S deployment.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3fb2c-107">Budeme průběžně trvá kroky pro zlepšení zabezpečení platforma Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-107">We are continuously taking steps to improve Microsoft Azure Platform security.</span></span> <span data-ttu-id="3fb2c-108">V rámci této snahy probíhající upgrade webové aplikace je plánovaná pro oblasti Německo centrální a Německo – severovýchod.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-108">As part of this ongoing effort an upgrade of Web Applications is planned for Germany Central and Germany Northeast regions.</span></span> <span data-ttu-id="3fb2c-109">Během této bude webové aplikace zakazuje použití protokolu ve formátu prostého textu FTP pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-109">During this Web Apps will be disabling the use of plain text FTP protocol for deployments.</span></span> <span data-ttu-id="3fb2c-110">Naše doporučení na naše zákazníky je přepnout do FTPS pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-110">Our recommendation to our customers is to switch to FTPS for deployments.</span></span> <span data-ttu-id="3fb2c-111">Jsme Nečekejte přerušení k službě během tohoto upgradu, která je plánovaná pro 9/5.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-111">We do not expect any disruption to your service during this upgrade which is planned for 9/5.</span></span> <span data-ttu-id="3fb2c-112">Vážíme si můžete podporovat v této úsilí.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-112">We appreciate you support in this effort.</span></span>

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a><span data-ttu-id="3fb2c-113">Krok 1: Nastavení přihlašovacích údajů nasazení</span><span class="sxs-lookup"><span data-stu-id="3fb2c-113">Step 1: Set deployment credentials</span></span>

<span data-ttu-id="3fb2c-114">Pro přístup k serveru FTP pro vaši aplikaci, musíte nejdřív přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-114">To access the FTP server for your app, you first need deployment credentials.</span></span> 

<span data-ttu-id="3fb2c-115">Chcete-li nastavit nebo obnovit přihlašovací údaje nasazení, přečtěte si téma [přihlašovací údaje nasazení Azure App Service](app-service-deployment-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="3fb2c-115">To set or reset your deployment credentials, see [Azure App Service Deployment Credentials](app-service-deployment-credentials.md).</span></span> <span data-ttu-id="3fb2c-116">Tento kurz představuje pomocí pověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-116">This tutorial demonstrates the use of user-level credentials.</span></span>

## <a name="step-2-get-ftp-connection-information"></a><span data-ttu-id="3fb2c-117">Krok 2: Získání informace o připojení FTP</span><span class="sxs-lookup"><span data-stu-id="3fb2c-117">Step 2: Get FTP connection information</span></span>

1. <span data-ttu-id="3fb2c-118">V [portál Azure](https://portal.azure.com), otevřete v této aplikaci [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="3fb2c-118">In the [Azure portal](https://portal.azure.com), open your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span>
2. <span data-ttu-id="3fb2c-119">Vyberte **přehled** v levé nabídce, poznamenejte si hodnoty pro **uživatel FTP/nasazení**, **název hostitele FTP**, a **FTPS název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-119">Select **Overview** in the left menu, then note the values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**.</span></span> 

    ![Informace o připojení FTP](./media/web-sites-deploy/FTP-Connection-Info.PNG)

    > [!NOTE]
    > <span data-ttu-id="3fb2c-121">**Uživatel FTP/nasazení** uživatele hodnotě zobrazené pomocí portálu Azure, včetně názvu aplikace s cílem poskytnout správný kontext pro FTP server.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-121">The **FTP/Deployment User** user value as displayed by the Azure Portal including the app name in order to provide proper context for the FTP server.</span></span>
    > <span data-ttu-id="3fb2c-122">Stejné informace můžete najít, když vyberete **vlastnosti** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-122">You can find the same information when you select **Properties** in the left menu.</span></span> 
    >
    > <span data-ttu-id="3fb2c-123">Navíc se nikdy zobrazí nasazení heslo.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-123">Also, the deployment password is never shown.</span></span> <span data-ttu-id="3fb2c-124">Pokud zapomenete heslo nasazení, přejděte zpátky k [krok 1](#step1) a resetování hesla nasazení.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-124">If you forget your deployment password, go back to [step 1](#step1) and reset your deployment password.</span></span>
    >
    >

## <a name="step-3-deploy-files-to-azure"></a><span data-ttu-id="3fb2c-125">Krok 3: Nasazení souborů do Azure</span><span class="sxs-lookup"><span data-stu-id="3fb2c-125">Step 3: Deploy files to Azure</span></span>

1. <span data-ttu-id="3fb2c-126">Z vašeho klienta FTP ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client)atd), použijte informace o připojení, které jste shromáždili pro připojení k vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-126">From your FTP client ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client), etc), use the connection information you gathered to connect to your app.</span></span>
3. <span data-ttu-id="3fb2c-127">Kopírování souborů a jejich odpovídajících adresářovou strukturu pro [ **/web/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) v Azure (nebo **/lokality/wwwroot/App_Data/úlohy/** adresář pro webové úlohy).</span><span class="sxs-lookup"><span data-stu-id="3fb2c-127">Copy your files and their respective directory structure to the [**/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (or the **/site/wwwroot/App_Data/Jobs/** directory for WebJobs).</span></span>
4. <span data-ttu-id="3fb2c-128">Přejděte na adresu URL vaší aplikace k ověření, že aplikace běží správně.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-128">Browse to your app's URL to verify the app is running properly.</span></span> 

> [!NOTE] 
> <span data-ttu-id="3fb2c-129">Na rozdíl od [nasazení na základě Git](app-service-deploy-local-git.md), FTP nasazení nepodporuje automatizaci následující nasazení:</span><span class="sxs-lookup"><span data-stu-id="3fb2c-129">Unlike [Git-based deployments](app-service-deploy-local-git.md), FTP deployment doesn't support the following deployment automations:</span></span> 
>
> - <span data-ttu-id="3fb2c-130">obnovení závislosti (například NuGet, NPM, PIP a autora automatizaci)</span><span class="sxs-lookup"><span data-stu-id="3fb2c-130">dependency restore (such as NuGet, NPM, PIP, and Composer automations)</span></span>
> - <span data-ttu-id="3fb2c-131">kompilace rozhraní .NET binárních souborů</span><span class="sxs-lookup"><span data-stu-id="3fb2c-131">compilation of .NET binaries</span></span>
> - <span data-ttu-id="3fb2c-132">generování souboru Web.config (tady je [Node.js příklad](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))</span><span class="sxs-lookup"><span data-stu-id="3fb2c-132">generation of web.config (here is a [Node.js example](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))</span></span>
> 
> <span data-ttu-id="3fb2c-133">Musíte obnovit, sestavení a generovat tyto potřebné soubory ručně na místním počítači a nasadit je spolu s vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-133">You must restore, build, and generate these necessary files manually on your local machine and deploy them together with your app.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="3fb2c-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3fb2c-134">Next steps</span></span>

<span data-ttu-id="3fb2c-135">Pro pokročilejší scénáře nasazení, zkuste [nasazení do Azure s Gitem](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="3fb2c-135">For more advanced deployment scenarios, try [deploying to Azure with Git](app-service-deploy-local-git.md).</span></span> <span data-ttu-id="3fb2c-136">Na základě Git nasazení do Azure umožňuje verzí, obnovení balíčků, MSBuild a další.</span><span class="sxs-lookup"><span data-stu-id="3fb2c-136">Git-based deployment to Azure enables version control, package restore, MSBuild, and more.</span></span>

## <a name="more-resources"></a><span data-ttu-id="3fb2c-137">Další materiály</span><span class="sxs-lookup"><span data-stu-id="3fb2c-137">More Resources</span></span>

* <span data-ttu-id="3fb2c-138">[Vytvoření webové aplikace PHP MySQL a nasadit pomocí FTP](web-sites-php-mysql-deploy-use-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="3fb2c-138">[Create a PHP-MySQL web app and deploy using FTP](web-sites-php-mysql-deploy-use-ftp.md).</span></span>
* [<span data-ttu-id="3fb2c-139">Přihlašovací údaje pro nasazení služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3fb2c-139">Azure App Service Deployment Credentials</span></span>](app-service-deploy-ftp.md)
