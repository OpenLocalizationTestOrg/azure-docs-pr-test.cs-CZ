---
title: "Stažení sady Azure SDK pro PHP"
description: "Zjistěte, jak stáhnout a nainstalovat sadu Azure SDK pro jazyk PHP."
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
ms.openlocfilehash: fd3d28b133ef8e646f5c2f1c1127f654daa61b95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="download-the-azure-sdk-for-php"></a><span data-ttu-id="7393b-103">Stažení sady Azure SDK pro PHP</span><span class="sxs-lookup"><span data-stu-id="7393b-103">Download the Azure SDK for PHP</span></span>
## <a name="overview"></a><span data-ttu-id="7393b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="7393b-104">Overview</span></span>
<span data-ttu-id="7393b-105">Azure SDK pro jazyk PHP obsahuje součásti, které vám umožní vyvíjet, nasazovat a spravovat aplikace PHP pro Azure.</span><span class="sxs-lookup"><span data-stu-id="7393b-105">The Azure SDK for PHP includes components that allow you to develop, deploy, and manage PHP applications for Azure.</span></span> <span data-ttu-id="7393b-106">Konkrétně sadu Azure SDK pro jazyk PHP zahrnuje následující:</span><span class="sxs-lookup"><span data-stu-id="7393b-106">Specifically, the Azure SDK for PHP includes the following:</span></span>

* <span data-ttu-id="7393b-107">**PHP klientské knihovny pro Azure**.</span><span class="sxs-lookup"><span data-stu-id="7393b-107">**The PHP client libraries for Azure**.</span></span> <span data-ttu-id="7393b-108">Tyto knihovny tříd poskytují rozhraní pro přístup k Azure funkcí, jako jsou služby pro správu dat a cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="7393b-108">These class libraries provide an interface for accessing Azure features, such as data management services and cloud services.</span></span>  
* <span data-ttu-id="7393b-109">**Rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)**.</span><span class="sxs-lookup"><span data-stu-id="7393b-109">**The Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)**.</span></span> <span data-ttu-id="7393b-110">Toto je sadu příkazů pro nasazení a správu služby Azure, jako jsou weby Azure a virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="7393b-110">This is a set of commands for deploying and managing Azure services, such as Azure Websites and Azure Virtual Machines.</span></span> <span data-ttu-id="7393b-111">Rozhraní příkazového řádku Azure práce na jakékoli platformě, včetně Mac, Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="7393b-111">The Azure CLI work on any platform, including Mac, Linux, and Windows.</span></span>
* <span data-ttu-id="7393b-112">**Prostředí Azure PowerShell (jenom Windows)**.</span><span class="sxs-lookup"><span data-stu-id="7393b-112">**Azure PowerShell (Windows Only)**.</span></span> <span data-ttu-id="7393b-113">To je sada rutin prostředí PowerShell pro nasazení a Správa služby Azure, jako je cloudových služeb a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7393b-113">This is a set of PowerShell cmdlets for deploying and managing Azure Services, such as Cloud Services and Virtual Machines.</span></span>
* <span data-ttu-id="7393b-114">**(Jenom Windows) Azure emulátorů**.</span><span class="sxs-lookup"><span data-stu-id="7393b-114">**The Azure Emulators (Windows Only)**.</span></span> <span data-ttu-id="7393b-115">Emulátorů výpočetního prostředí a úložiště jsou místní emulátorů cloudové služby a služby pro data, které vám umožní testovat aplikaci místně.</span><span class="sxs-lookup"><span data-stu-id="7393b-115">The compute and storage emulators are local emulators of cloud services and data management services that allow you to test an application locally.</span></span> <span data-ttu-id="7393b-116">Emulátorů Azure lze spustit pouze v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="7393b-116">The Azure Emulators run on Windows only.</span></span>

<span data-ttu-id="7393b-117">V níže uvedených částech popisují, jak stáhnout a nainstalovat komponenty popsané výše.</span><span class="sxs-lookup"><span data-stu-id="7393b-117">The sections below describe how to download and install the components described above.</span></span>

<span data-ttu-id="7393b-118">Podle pokynů v tomto tématu se předpokládá, že máte [PHP] [ install-php] nainstalována.</span><span class="sxs-lookup"><span data-stu-id="7393b-118">The instructions in this topic assume that you have [PHP][install-php] installed.</span></span>

> [!NOTE]
> <span data-ttu-id="7393b-119">Musíte mít PHP 5.5 nebo vyšší použít knihovny klienta PHP pro Azure.</span><span class="sxs-lookup"><span data-stu-id="7393b-119">You must have PHP 5.5 or higher to use the PHP client libraries for Azure.</span></span>
> 
> 

## <a name="php-client-libraries-for-azure"></a><span data-ttu-id="7393b-120">Klientské knihovny PHP pro Azure</span><span class="sxs-lookup"><span data-stu-id="7393b-120">PHP client libraries for Azure</span></span>
<span data-ttu-id="7393b-121">PHP klientské knihovny pro Azure poskytují rozhraní pro přístup k Azure funkcí, jako jsou služby pro správu dat a cloudové služby ve všech operačních systémech.</span><span class="sxs-lookup"><span data-stu-id="7393b-121">The PHP Client Libraries for Azure provide an interface for accessing Azure features, such as data management services and cloud services, from any operating system.</span></span> <span data-ttu-id="7393b-122">Tyto knihovny se může nainstalovat prostřednictvím autora.</span><span class="sxs-lookup"><span data-stu-id="7393b-122">These libraries can be installed via the Composer.</span></span>

<span data-ttu-id="7393b-123">Informace o tom, jak použít knihovny klienta PHP pro Azure najdete v tématu [jak používat služby objektů Blob][blob-service], [použití služby Table] [ table-service]a [jak používat fronty služby][queue-service].</span><span class="sxs-lookup"><span data-stu-id="7393b-123">For information about how to use the PHP Client Libraries for Azure, see [How to Use the Blob Service][blob-service], [How to Use the Table Service][table-service] and [How to Use the Queue Service][queue-service].</span></span>

### <a name="install-via-composer"></a><span data-ttu-id="7393b-124">Nainstalovat prostřednictvím autora</span><span class="sxs-lookup"><span data-stu-id="7393b-124">Install via Composer</span></span>
1. <span data-ttu-id="7393b-125">[Nainstalovat Git][install-git].</span><span class="sxs-lookup"><span data-stu-id="7393b-125">[Install Git][install-git].</span></span>

    > [AZURE.NOTE] <span data-ttu-id="7393b-126">V systému Windows bude také muset přidat Git spustitelný soubor do vaší proměnné prostředí PATH.</span><span class="sxs-lookup"><span data-stu-id="7393b-126">On Windows, you will also need to add the Git executable to your PATH environment variable.</span></span>

1. <span data-ttu-id="7393b-127">Vytvořte soubor s názvem **composer.json** v kořenu projektu a přidejte do ní následující kód:</span><span class="sxs-lookup"><span data-stu-id="7393b-127">Create a file named **composer.json** in the root of your project and add the following code to it:</span></span>
   
        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }
2. <span data-ttu-id="7393b-128">Stáhněte si  **[composer.phar] [ composer-phar]**  v kořenového adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="7393b-128">Download **[composer.phar][composer-phar]** in your project root.</span></span>
3. <span data-ttu-id="7393b-129">Otevřete příkazový řádek a spusťte tento v kořenového adresáře projektu</span><span class="sxs-lookup"><span data-stu-id="7393b-129">Open a command prompt and execute this in your project root</span></span>
   
        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a><span data-ttu-id="7393b-130">Prostředí Azure PowerShell a Azure emulátorů</span><span class="sxs-lookup"><span data-stu-id="7393b-130">Azure PowerShell and Azure Emulators</span></span>
<span data-ttu-id="7393b-131">Prostředí Azure PowerShell je sada rutin prostředí PowerShell pro nasazení a Správa služby Azure (například cloudové služby a virtuální počítače).</span><span class="sxs-lookup"><span data-stu-id="7393b-131">Azure PowerShell is a set of PowerShell cmdlets for deploying and managing Azure Services (such as Cloud Services and Virtual Machines).</span></span> <span data-ttu-id="7393b-132">Emulátorů Azure jsou emulátorů cloudové služby a služby pro data, které vám umožní testovat aplikaci místně.</span><span class="sxs-lookup"><span data-stu-id="7393b-132">The Azure Emulators are emulators of cloud services and data management services that allow you to test an application locally.</span></span> <span data-ttu-id="7393b-133">Tyto součásti jsou podporovány pouze v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="7393b-133">These components are supported Windows only.</span></span>

<span data-ttu-id="7393b-134">Doporučený způsob, jak nainstalovat Azure PowerShell a emulátorů Azure je použití [instalačního programu webové platformy Microsoft][download-wpi].</span><span class="sxs-lookup"><span data-stu-id="7393b-134">The recommended way to install Azure PowerShell and the Azure Emulators is to use the [Microsoft Web Platform Installer][download-wpi].</span></span> <span data-ttu-id="7393b-135">Všimněte si, že můžete nainstalovat jiné komponenty, vývoj, například PHP, SQL Server, Drivers společnosti Microsoft pro systém SQL Server pro PHP a službě WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="7393b-135">Note that you can also choose to install other development components, such as PHP, SQL Server, the Microsoft Drivers for SQL Server for PHP, and WebMatrix.</span></span>

<span data-ttu-id="7393b-136">Informace o tom, jak pomocí prostředí Azure PowerShell najdete v tématu [jak používat Azure PowerShell][powershell-tools].</span><span class="sxs-lookup"><span data-stu-id="7393b-136">For information about how to use Azure PowerShell, see [How to Use Azure PowerShell][powershell-tools].</span></span>

## <a name="azure-cli"></a><span data-ttu-id="7393b-137">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7393b-137">Azure CLI</span></span>
<span data-ttu-id="7393b-138">Rozhraní příkazového řádku Azure je sadu příkazů pro nasazení a správu služby Azure, jako jsou weby Azure a virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="7393b-138">The Azure CLI is a set of commands for deploying and managing Azure services, such as Azure Websites and Azure Virtual Machines.</span></span> <span data-ttu-id="7393b-139">Informace o instalaci rozhraní příkazového řádku Azure najdete v tématu [nainstalovat Azure CLI](cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="7393b-139">For information about installing Azure CLI, see [Install the Azure CLI](cli-install-nodejs.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7393b-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7393b-140">Next steps</span></span>
<span data-ttu-id="7393b-141">Další informace najdete v tématu [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="7393b-141">For more information, see the [PHP Developer Center](/develop/php/).</span></span>

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
