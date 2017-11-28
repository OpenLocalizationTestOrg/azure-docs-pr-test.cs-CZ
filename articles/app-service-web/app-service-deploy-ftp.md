---
title: "aaaDeploy tooAzure vaše aplikace služby App Service pomocí FTP/S | Microsoft Docs"
description: "Zjistěte, jak toodeploy tooAzure vaše aplikace služby App Service pomocí serveru FTP nebo FTPS."
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
ms.openlocfilehash: 318ae79d4fae269f853ea5c3ce28353b0864131e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-tooazure-app-service-using-ftps"></a><span data-ttu-id="36055-103">Nasazení vaší aplikace tooAzure služby App Service pomocí FTP/S</span><span class="sxs-lookup"><span data-stu-id="36055-103">Deploy your app tooAzure App Service using FTP/S</span></span>

<span data-ttu-id="36055-104">Tento článek ukazuje, jak toouse FTP nebo FTPS toodeploy vaší webové aplikace, back-end mobilní aplikace nebo aplikace API příliš[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="36055-104">This article shows you how toouse FTP or FTPS toodeploy your web app, mobile app backend, or API app too[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="36055-105">Hello koncový bod FTP/S pro aplikace je již aktivní.</span><span class="sxs-lookup"><span data-stu-id="36055-105">hello FTP/S endpoint for your app is already active.</span></span> <span data-ttu-id="36055-106">Nezbytné tooenable FTP/S nasazení není žádná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="36055-106">No configuration is necessary tooenable FTP/S deployment.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="36055-107">Budeme průběžně trvá kroky tooimprove zabezpečení platforma Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="36055-107">We are continuously taking steps tooimprove Microsoft Azure Platform security.</span></span> <span data-ttu-id="36055-108">V rámci této snahy probíhající upgrade webové aplikace je plánovaná pro oblasti Německo centrální a Německo – severovýchod.</span><span class="sxs-lookup"><span data-stu-id="36055-108">As part of this ongoing effort an upgrade of Web Applications is planned for Germany Central and Germany Northeast regions.</span></span> <span data-ttu-id="36055-109">Během této bude webové aplikace zakazuje použití hello protokolu ve formátu prostého textu FTP pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="36055-109">During this Web Apps will be disabling hello use of plain text FTP protocol for deployments.</span></span> <span data-ttu-id="36055-110">Naše zákazníky tooour doporučení je tooswitch tooFTPS pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="36055-110">Our recommendation tooour customers is tooswitch tooFTPS for deployments.</span></span> <span data-ttu-id="36055-111">Při tomto upgradu, která je plánovaná pro 9/5 jsme Nečekejte jakoukoli službu, tooyour přerušení.</span><span class="sxs-lookup"><span data-stu-id="36055-111">We do not expect any disruption tooyour service during this upgrade which is planned for 9/5.</span></span> <span data-ttu-id="36055-112">Vážíme si můžete podporovat v této úsilí.</span><span class="sxs-lookup"><span data-stu-id="36055-112">We appreciate you support in this effort.</span></span>

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a><span data-ttu-id="36055-113">Krok 1: Nastavení přihlašovacích údajů nasazení</span><span class="sxs-lookup"><span data-stu-id="36055-113">Step 1: Set deployment credentials</span></span>

<span data-ttu-id="36055-114">server hello FTP tooaccess pro vaši aplikaci, musíte nejdřív přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="36055-114">tooaccess hello FTP server for your app, you first need deployment credentials.</span></span> 

<span data-ttu-id="36055-115">tooset nebo resetování najdete v části přihlašovací údaje nasazení, [přihlašovací údaje nasazení Azure App Service](app-service-deployment-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="36055-115">tooset or reset your deployment credentials, see [Azure App Service Deployment Credentials](app-service-deployment-credentials.md).</span></span> <span data-ttu-id="36055-116">Tento kurz představuje hello používání přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="36055-116">This tutorial demonstrates hello use of user-level credentials.</span></span>

## <a name="step-2-get-ftp-connection-information"></a><span data-ttu-id="36055-117">Krok 2: Získání informace o připojení FTP</span><span class="sxs-lookup"><span data-stu-id="36055-117">Step 2: Get FTP connection information</span></span>

1. <span data-ttu-id="36055-118">V hello [portál Azure](https://portal.azure.com), otevřete v této aplikaci [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="36055-118">In hello [Azure portal](https://portal.azure.com), open your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span>
2. <span data-ttu-id="36055-119">Vyberte **přehled** v levé nabídce hello, poznamenejte si hodnoty hello **uživatel FTP/nasazení**, **název hostitele FTP**, a **FTPS název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="36055-119">Select **Overview** in hello left menu, then note hello values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**.</span></span> 

    ![Informace o připojení FTP](./media/web-sites-deploy/FTP-Connection-Info.PNG)

    > [!NOTE]
    > <span data-ttu-id="36055-121">Hello **uživatel FTP/nasazení** hodnota uživatele, jak se zobrazuje při hello portálu Azure, včetně názvu aplikace hello v pořadí tooprovide správný kontext hello FTP serveru.</span><span class="sxs-lookup"><span data-stu-id="36055-121">hello **FTP/Deployment User** user value as displayed by hello Azure Portal including hello app name in order tooprovide proper context for hello FTP server.</span></span>
    > <span data-ttu-id="36055-122">Můžete najít hello stejné informace, když vyberete **vlastnosti** v levé nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="36055-122">You can find hello same information when you select **Properties** in hello left menu.</span></span> 
    >
    > <span data-ttu-id="36055-123">Navíc se nikdy zobrazuje hello nasazení heslo.</span><span class="sxs-lookup"><span data-stu-id="36055-123">Also, hello deployment password is never shown.</span></span> <span data-ttu-id="36055-124">Pokud zapomenete heslo nasazení, přejděte zpátky příliš[krok 1](#step1) a resetování hesla nasazení.</span><span class="sxs-lookup"><span data-stu-id="36055-124">If you forget your deployment password, go back too[step 1](#step1) and reset your deployment password.</span></span>
    >
    >

## <a name="step-3-deploy-files-tooazure"></a><span data-ttu-id="36055-125">Krok 3: Nasazení souborů tooAzure</span><span class="sxs-lookup"><span data-stu-id="36055-125">Step 3: Deploy files tooAzure</span></span>

1. <span data-ttu-id="36055-126">Z vašeho klienta FTP ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client)atd), použijte informace o připojení hello, které jste shromáždili tooconnect tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="36055-126">From your FTP client ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client), etc), use hello connection information you gathered tooconnect tooyour app.</span></span>
3. <span data-ttu-id="36055-127">Kopírování souborů a jejich odpovídajících directory struktura toohello [ **/web/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) v Azure (nebo hello **/lokality/wwwroot/App_Data/úlohy/** adresář pro Webové úlohy).</span><span class="sxs-lookup"><span data-stu-id="36055-127">Copy your files and their respective directory structure toohello [**/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (or hello **/site/wwwroot/App_Data/Jobs/** directory for WebJobs).</span></span>
4. <span data-ttu-id="36055-128">Procházet tooyour aplikace URL tooverify hello aplikace běží správně.</span><span class="sxs-lookup"><span data-stu-id="36055-128">Browse tooyour app's URL tooverify hello app is running properly.</span></span> 

> [!NOTE] 
> <span data-ttu-id="36055-129">Na rozdíl od [nasazení na základě Git](app-service-deploy-local-git.md), FTP nasazení nepodporuje hello následující automatizaci nasazení:</span><span class="sxs-lookup"><span data-stu-id="36055-129">Unlike [Git-based deployments](app-service-deploy-local-git.md), FTP deployment doesn't support hello following deployment automations:</span></span> 
>
> - <span data-ttu-id="36055-130">obnovení závislosti (například NuGet, NPM, PIP a autora automatizaci)</span><span class="sxs-lookup"><span data-stu-id="36055-130">dependency restore (such as NuGet, NPM, PIP, and Composer automations)</span></span>
> - <span data-ttu-id="36055-131">kompilace rozhraní .NET binárních souborů</span><span class="sxs-lookup"><span data-stu-id="36055-131">compilation of .NET binaries</span></span>
> - <span data-ttu-id="36055-132">generování souboru Web.config (tady je [Node.js příklad](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))</span><span class="sxs-lookup"><span data-stu-id="36055-132">generation of web.config (here is a [Node.js example](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))</span></span>
> 
> <span data-ttu-id="36055-133">Musíte obnovit, sestavení a generovat tyto potřebné soubory ručně na místním počítači a nasadit je spolu s vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="36055-133">You must restore, build, and generate these necessary files manually on your local machine and deploy them together with your app.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="36055-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36055-134">Next steps</span></span>

<span data-ttu-id="36055-135">Pro pokročilejší scénáře nasazení, zkuste [nasazení tooAzure s Gitem](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="36055-135">For more advanced deployment scenarios, try [deploying tooAzure with Git](app-service-deploy-local-git.md).</span></span> <span data-ttu-id="36055-136">Nasazení na základě Git tooAzure umožňuje verzí, obnovení balíčků, MSBuild a další.</span><span class="sxs-lookup"><span data-stu-id="36055-136">Git-based deployment tooAzure enables version control, package restore, MSBuild, and more.</span></span>

## <a name="more-resources"></a><span data-ttu-id="36055-137">Další materiály</span><span class="sxs-lookup"><span data-stu-id="36055-137">More Resources</span></span>

* <span data-ttu-id="36055-138">[Vytvoření webové aplikace PHP MySQL a nasadit pomocí FTP](web-sites-php-mysql-deploy-use-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="36055-138">[Create a PHP-MySQL web app and deploy using FTP](web-sites-php-mysql-deploy-use-ftp.md).</span></span>
* [<span data-ttu-id="36055-139">Přihlašovací údaje pro nasazení služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="36055-139">Azure App Service Deployment Credentials</span></span>](app-service-deploy-ftp.md)
