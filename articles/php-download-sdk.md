---
title: aaaDownload hello Azure SDK pro jazyk PHP
description: "Zjistěte, jak toodownload a nainstalujte hello Azure SDK pro jazyk PHP."
documentationcenter: php
services: app-service\web
author: allclark
manager: douge
editor: 
ms.assetid: bac355ac-4c25-42f4-8273-c5112eafa8d4
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 06/01/2016
ms.author: allclark;yaqiyang
ms.openlocfilehash: 94f56fc4f91bb175c08b9f7a43cb221c827694a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="download-hello-azure-sdk-for-php"></a><span data-ttu-id="a24ec-103">Stáhnout hello Azure SDK pro jazyk PHP</span><span class="sxs-lookup"><span data-stu-id="a24ec-103">Download hello Azure SDK for PHP</span></span>
## <a name="overview"></a><span data-ttu-id="a24ec-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="a24ec-104">Overview</span></span>
<span data-ttu-id="a24ec-105">Hello Azure SDK pro jazyk PHP obsahuje součásti, které vám umožňují toodevelop, nasazení a Správa aplikací PHP pro Azure.</span><span class="sxs-lookup"><span data-stu-id="a24ec-105">hello Azure SDK for PHP includes components that allow you toodevelop, deploy, and manage PHP applications for Azure.</span></span> <span data-ttu-id="a24ec-106">Konkrétně hello Azure SDK pro jazyk PHP obsahuje hello následující:</span><span class="sxs-lookup"><span data-stu-id="a24ec-106">Specifically, hello Azure SDK for PHP includes hello following:</span></span>

* <span data-ttu-id="a24ec-107">**Hello PHP klientské knihovny pro Azure**.</span><span class="sxs-lookup"><span data-stu-id="a24ec-107">**hello PHP client libraries for Azure**.</span></span> <span data-ttu-id="a24ec-108">Tyto knihovny tříd poskytují rozhraní pro přístup k Azure funkcí, jako jsou služby pro správu dat a cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="a24ec-108">These class libraries provide an interface for accessing Azure features, such as data management services and cloud services.</span></span>  
* <span data-ttu-id="a24ec-109">**Hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)**.</span><span class="sxs-lookup"><span data-stu-id="a24ec-109">**hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)**.</span></span> <span data-ttu-id="a24ec-110">Toto je sadu příkazů pro nasazení a správu služby Azure, jako jsou weby Azure a virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="a24ec-110">This is a set of commands for deploying and managing Azure services, such as Azure Websites and Azure Virtual Machines.</span></span> <span data-ttu-id="a24ec-111">rozhraní příkazového řádku Azure pracovní na jakékoli platformě, včetně Mac, Linux a Windows Hello.</span><span class="sxs-lookup"><span data-stu-id="a24ec-111">hello Azure CLI work on any platform, including Mac, Linux, and Windows.</span></span>
* <span data-ttu-id="a24ec-112">**Prostředí Azure PowerShell (jenom Windows)**.</span><span class="sxs-lookup"><span data-stu-id="a24ec-112">**Azure PowerShell (Windows Only)**.</span></span> <span data-ttu-id="a24ec-113">To je sada rutin prostředí PowerShell pro nasazení a Správa služby Azure, jako je cloudových služeb a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a24ec-113">This is a set of PowerShell cmdlets for deploying and managing Azure Services, such as Cloud Services and Virtual Machines.</span></span>
* <span data-ttu-id="a24ec-114">**Hello (pouze Windows) Azure emulátorů**.</span><span class="sxs-lookup"><span data-stu-id="a24ec-114">**hello Azure Emulators (Windows Only)**.</span></span> <span data-ttu-id="a24ec-115">Hello emulátorů výpočetního prostředí a úložiště jsou místní emulátorů cloudové služby a služby pro data, které vám umožňují tootest aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="a24ec-115">hello compute and storage emulators are local emulators of cloud services and data management services that allow you tootest an application locally.</span></span> <span data-ttu-id="a24ec-116">Hello Azure emulátorů lze spustit pouze v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a24ec-116">hello Azure Emulators run on Windows only.</span></span>

<span data-ttu-id="a24ec-117">Hello části níže popisují, jak toodownload a nainstalujte hello komponent popsaných výše.</span><span class="sxs-lookup"><span data-stu-id="a24ec-117">hello sections below describe how toodownload and install hello components described above.</span></span>

<span data-ttu-id="a24ec-118">Hello pokyny v tomto tématu se předpokládá, že máte [PHP] [ install-php] nainstalována.</span><span class="sxs-lookup"><span data-stu-id="a24ec-118">hello instructions in this topic assume that you have [PHP][install-php] installed.</span></span>

> [!NOTE]
> <span data-ttu-id="a24ec-119">PHP 5.5 nebo vyšší toouse hello PHP klientské knihovny musí mít pro Azure.</span><span class="sxs-lookup"><span data-stu-id="a24ec-119">You must have PHP 5.5 or higher toouse hello PHP client libraries for Azure.</span></span>
> 
> 

## <a name="php-client-libraries-for-azure"></a><span data-ttu-id="a24ec-120">Klientské knihovny PHP pro Azure</span><span class="sxs-lookup"><span data-stu-id="a24ec-120">PHP client libraries for Azure</span></span>
<span data-ttu-id="a24ec-121">Hello PHP klientské knihovny pro Azure poskytují rozhraní pro přístup k Azure funkcí, jako jsou služby pro správu dat a cloudové služby ve všech operačních systémech.</span><span class="sxs-lookup"><span data-stu-id="a24ec-121">hello PHP Client Libraries for Azure provide an interface for accessing Azure features, such as data management services and cloud services, from any operating system.</span></span> <span data-ttu-id="a24ec-122">Tyto knihovny se může nainstalovat prostřednictvím hello autora.</span><span class="sxs-lookup"><span data-stu-id="a24ec-122">These libraries can be installed via hello Composer.</span></span>

<span data-ttu-id="a24ec-123">Informace o tom, jak toouse hello PHP klientské knihovny pro Azure najdete v tématu [jak tooUse hello služby objektů Blob][blob-service], [jak tooUse hello služby Table] [ table-service] a [jak tooUse hello služby front][queue-service].</span><span class="sxs-lookup"><span data-stu-id="a24ec-123">For information about how toouse hello PHP Client Libraries for Azure, see [How tooUse hello Blob Service][blob-service], [How tooUse hello Table Service][table-service] and [How tooUse hello Queue Service][queue-service].</span></span>

### <a name="install-via-composer"></a><span data-ttu-id="a24ec-124">Nainstalovat prostřednictvím autora</span><span class="sxs-lookup"><span data-stu-id="a24ec-124">Install via Composer</span></span>
1. <span data-ttu-id="a24ec-125">[Nainstalovat Git][install-git].</span><span class="sxs-lookup"><span data-stu-id="a24ec-125">[Install Git][install-git].</span></span>

    > [AZURE.NOTE] <span data-ttu-id="a24ec-126">V systému Windows budete také potřebovat proměnné prostředí PATH spustitelné tooyour Git tooadd hello.</span><span class="sxs-lookup"><span data-stu-id="a24ec-126">On Windows, you will also need tooadd hello Git executable tooyour PATH environment variable.</span></span>

1. <span data-ttu-id="a24ec-127">Vytvořte soubor s názvem **composer.json** v kořenu projektu hello a přidejte následující kód tooit hello:</span><span class="sxs-lookup"><span data-stu-id="a24ec-127">Create a file named **composer.json** in hello root of your project and add hello following code tooit:</span></span>
   
        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }
2. <span data-ttu-id="a24ec-128">Stáhněte si  **[composer.phar] [ composer-phar]**  v kořenového adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="a24ec-128">Download **[composer.phar][composer-phar]** in your project root.</span></span>
3. <span data-ttu-id="a24ec-129">Otevřete příkazový řádek a spusťte tento v kořenového adresáře projektu</span><span class="sxs-lookup"><span data-stu-id="a24ec-129">Open a command prompt and execute this in your project root</span></span>
   
        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a><span data-ttu-id="a24ec-130">Prostředí Azure PowerShell a Azure emulátorů</span><span class="sxs-lookup"><span data-stu-id="a24ec-130">Azure PowerShell and Azure Emulators</span></span>
<span data-ttu-id="a24ec-131">Prostředí Azure PowerShell je sada rutin prostředí PowerShell pro nasazení a Správa služby Azure (například cloudové služby a virtuální počítače).</span><span class="sxs-lookup"><span data-stu-id="a24ec-131">Azure PowerShell is a set of PowerShell cmdlets for deploying and managing Azure Services (such as Cloud Services and Virtual Machines).</span></span> <span data-ttu-id="a24ec-132">Hello Azure emulátorů jsou emulátorů cloudové služby a služby pro data, které vám umožňují tootest aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="a24ec-132">hello Azure Emulators are emulators of cloud services and data management services that allow you tootest an application locally.</span></span> <span data-ttu-id="a24ec-133">Tyto součásti jsou podporovány pouze v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a24ec-133">These components are supported Windows only.</span></span>

<span data-ttu-id="a24ec-134">Hello doporučeným způsobem tooinstall prostředí Azure PowerShell a hello emulátorů Azure je toouse hello [instalačního programu webové platformy Microsoft][download-wpi].</span><span class="sxs-lookup"><span data-stu-id="a24ec-134">hello recommended way tooinstall Azure PowerShell and hello Azure Emulators is toouse hello [Microsoft Web Platform Installer][download-wpi].</span></span> <span data-ttu-id="a24ec-135">Všimněte si, že je také možné tooinstall jiné komponenty, vývoj, například PHP, SQL Server, hello Drivers společnosti Microsoft pro systém SQL Server pro PHP a službě WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="a24ec-135">Note that you can also choose tooinstall other development components, such as PHP, SQL Server, hello Microsoft Drivers for SQL Server for PHP, and WebMatrix.</span></span>

<span data-ttu-id="a24ec-136">Informace o tom najdete v části toouse prostředí Azure PowerShell [jak tooUse prostředí Azure PowerShell][powershell-tools].</span><span class="sxs-lookup"><span data-stu-id="a24ec-136">For information about how toouse Azure PowerShell, see [How tooUse Azure PowerShell][powershell-tools].</span></span>

## <a name="azure-cli"></a><span data-ttu-id="a24ec-137">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a24ec-137">Azure CLI</span></span>
<span data-ttu-id="a24ec-138">Hello rozhraní příkazového řádku Azure je sadu příkazů pro nasazení a správu služby Azure, jako jsou weby Azure a virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="a24ec-138">hello Azure CLI is a set of commands for deploying and managing Azure services, such as Azure Websites and Azure Virtual Machines.</span></span> <span data-ttu-id="a24ec-139">Informace o instalaci rozhraní příkazového řádku Azure najdete v tématu [hello instalace rozhraní příkazového řádku Azure](cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a24ec-139">For information about installing Azure CLI, see [Install hello Azure CLI](cli-install-nodejs.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a24ec-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a24ec-140">Next steps</span></span>
<span data-ttu-id="a24ec-141">Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="a24ec-141">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
