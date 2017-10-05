---
title: "Sestavení příkazového řádku Azure | Microsoft Docs"
description: "Sestavení příkazového řádku pro Azure."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2017
ms.author: kraigb
ms.openlocfilehash: 5fe910e2757dd5ec783538e23e7f52e2f5725b39
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="building-azure-projects-from-the-command-line"></a><span data-ttu-id="18abf-103">Sestavení projektů Azure z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="18abf-103">Building Azure projects from the command line</span></span>
<span data-ttu-id="18abf-104">Microsoft Build Engine (MSBuild) můžete vytvořit produkty v sestavení testovacích prostředích, kde není nainstalovaná sada Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="18abf-104">Using the Microsoft Build Engine (MSBuild), you can build products in build-lab environments where Visual Studio is not installed.</span></span> <span data-ttu-id="18abf-105">MSBuild používá formátu XML pro soubory projektu, které je rozšiřitelný a plně podporované společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="18abf-105">MSBuild uses an XML format for project files that's extensible and fully supported by Microsoft.</span></span> <span data-ttu-id="18abf-106">Pomocí nástroje MSBuild formát souboru, můžete popisují, co položky musí být vytvořené pro jednu nebo více platforem a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="18abf-106">Using the MSBuild file format, you can describe what items must be built for one or more platforms and configurations.</span></span>

<span data-ttu-id="18abf-107">Můžete také spouštět nástroje MSBuild v příkazovém řádku a toto téma popisuje tento přístup.</span><span class="sxs-lookup"><span data-stu-id="18abf-107">You can also run MSBuild at the command line, and this topic describes that approach.</span></span> <span data-ttu-id="18abf-108">Nastavením vlastnosti na příkazovém řádku, můžete vytvořit konkrétní konfigurace projektu.</span><span class="sxs-lookup"><span data-stu-id="18abf-108">By setting properties on the command line, you can build specific configurations of a project.</span></span> <span data-ttu-id="18abf-109">Podobně můžete také definovat cíle, které MSBuild sestavení.</span><span class="sxs-lookup"><span data-stu-id="18abf-109">Similarly, you can also define the targets that MSBuild builds.</span></span> <span data-ttu-id="18abf-110">Další informace o parametry příkazového řádku a nástroje MSBuild najdete v tématu [Reference k příkazovému řádku MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="18abf-110">For more information about command-line parameters and MSBuild, see [MSBuild Command-Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

## <a name="msbuild-parameters"></a><span data-ttu-id="18abf-111">Parametry nástroje MSBuild</span><span class="sxs-lookup"><span data-stu-id="18abf-111">MSBuild parameters</span></span>
<span data-ttu-id="18abf-112">Nejjednodušší způsob, jak vytvořit balíček je spuštění MSBuild s parametrem `/t:Publish` možnost.</span><span class="sxs-lookup"><span data-stu-id="18abf-112">The simplest way to create a package is to run MSBuild with the `/t:Publish` option.</span></span> <span data-ttu-id="18abf-113">Ve výchozím nastavení, tento příkaz vytvoří adresář ve vztahu k kořenové složce projektu, například `<ProjectDirectory>\bin\Configuration\app.publish\`.</span><span class="sxs-lookup"><span data-stu-id="18abf-113">By default, this command creates a directory in relation to the root folder for the project, such as `<ProjectDirectory>\bin\Configuration\app.publish\`.</span></span> <span data-ttu-id="18abf-114">Při sestavování projektu Azure jsou generovány dva soubory: soubor balíčku samostatně a doprovodné konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="18abf-114">When you build an Azure project, two files are generated: the package file itself and the accompanying configuration file:</span></span>

* <span data-ttu-id="18abf-115">Soubor balíčku (`project.cspkg`)</span><span class="sxs-lookup"><span data-stu-id="18abf-115">Package File (`project.cspkg`)</span></span>
* <span data-ttu-id="18abf-116">Konfigurační soubor (`ServiceConfiguration.TargetProfile.cscfg`)</span><span class="sxs-lookup"><span data-stu-id="18abf-116">Configuration File (`ServiceConfiguration.TargetProfile.cscfg`)</span></span>

<span data-ttu-id="18abf-117">Každý projekt, Azure ve výchozím nastavení, obsahuje jeden soubor konfigurace služby pro místní sestavení (ladění) a druhou pro sestavení cloudu (pracovním nebo produkčním).</span><span class="sxs-lookup"><span data-stu-id="18abf-117">By default, each Azure project includes one service-configuration file for local (debugging) builds and another for cloud (staging or production) builds.</span></span> <span data-ttu-id="18abf-118">Můžete ale přidat nebo odebrat soubory konfigurace služby podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="18abf-118">However, you can add or remove service-configuration files as needed.</span></span> <span data-ttu-id="18abf-119">Když vytvoříte balíček Visual Studia, zobrazí se výzva které souboru konfigurace služby společně se balíček.</span><span class="sxs-lookup"><span data-stu-id="18abf-119">When you build a package within Visual Studio, you are asked which service-configuration file to include alongside the package.</span></span> <span data-ttu-id="18abf-120">Když vytvoříte balíček pomocí nástroje MSBuild, místní služby konfigurační soubor je zahrnuté ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="18abf-120">When you build a package by using MSBuild, the local service-configuration file is included by default.</span></span> <span data-ttu-id="18abf-121">Chcete-li zahrnout jiný soubor konfigurace služby, nastavte `TargetProfile` vlastnosti nástroje MSBuild příkazu (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).</span><span class="sxs-lookup"><span data-stu-id="18abf-121">To include a different service-configuration file, set the `TargetProfile` property of the MSBuild command (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).</span></span>

<span data-ttu-id="18abf-122">Pokud chcete používat adresář alternativní uložené balíčku a konfigurační soubory, nastavte cestu pomocí `/p:PublishDir=Directory\` možnost, včetně koncové oddělovače zpětné lomítko.</span><span class="sxs-lookup"><span data-stu-id="18abf-122">If you want to use an alternate directory for the stored package and configuration files, set the path by using the `/p:PublishDir=Directory\` option, including the trailing backslash separator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18abf-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="18abf-123">Next steps</span></span>
<span data-ttu-id="18abf-124">Jakmile je balíček vytvořen, můžete ho nasadit do Azure.</span><span class="sxs-lookup"><span data-stu-id="18abf-124">After the package is built, you can deploy it to Azure.</span></span> <span data-ttu-id="18abf-125">Kurz, který ukazuje, jak k automatizaci tohoto procesu, najdete v části [nastavené průběžné doručování pro cloudové služby v Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="18abf-125">For a tutorial that demonstrates how to automate that process, see [Continuous Delivery for Cloud Services in Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).</span></span>

