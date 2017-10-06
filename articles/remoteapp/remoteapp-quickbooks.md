---
title: aaaDeploy QuickBooks v Azure Remoteappu | Microsoft Docs
description: "Zjistěte, jak tooshare QuickBooks s Azure Remoteappem."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c5d00753-77c0-4f0d-a5df-9451b46a31d3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c21b1ac311449be2281f9ce157828260e856f55d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a><span data-ttu-id="656d2-103">Jak se nasadit QuickBooks v Azure Remoteappu?</span><span class="sxs-lookup"><span data-stu-id="656d2-103">How do you deploy QuickBooks in Azure RemoteApp?</span></span>
> [!IMPORTANT]
> <span data-ttu-id="656d2-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="656d2-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="656d2-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="656d2-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="656d2-106">Použijte následující informace tooshare QuickBooks jako aplikace v Azure Remoteappu hello.</span><span class="sxs-lookup"><span data-stu-id="656d2-106">Use hello following information tooshare QuickBooks as an app in Azure RemoteApp.</span></span>

<span data-ttu-id="656d2-107">QuickBooks 2015 Enterprise můžete sdílet s Azure Remoteappem v kolekci hybridní nebo cloud.</span><span class="sxs-lookup"><span data-stu-id="656d2-107">You can share QuickBooks 2015 Enterprise with Azure RemoteApp in either a hybrid or cloud collection.</span></span> <span data-ttu-id="656d2-108">soubor společnosti Hello se musí nacházet na virtuální počítač, který je spuštěn server QuickBooks databáze, která je oddělená od serverů Azure RemoteApp hello.</span><span class="sxs-lookup"><span data-stu-id="656d2-108">hello company file must reside on a VM that is running QuickBooks database server that is separate from hello Azure RemoteApp servers.</span></span> <span data-ttu-id="656d2-109">Nikdy neukládají soubor společnosti hello na image Azure Remoteappu – ztráta dat je očekáváno, pokud to uděláte.</span><span class="sxs-lookup"><span data-stu-id="656d2-109">Never store hello company file on your Azure RemoteApp image - data loss is expected if you do this.</span></span> <span data-ttu-id="656d2-110">Pouze QuickBooks Enterprise podporuje hostování souborů QuickBooks hello externí sdílené složky s QuickBooks databázový server přístupné přes standardní sítě systému Windows.</span><span class="sxs-lookup"><span data-stu-id="656d2-110">Only QuickBooks Enterprise supports hosting hello QuickBooks file on an external share with QuickBooks database server accessible via standard Windows networking.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="656d2-111">Hello QuickBooks databázový server, který je hostitelem soubor společnosti hello se musí nacházet na samostatný virtuální počítač v rámci hello stejné virtuální síti jako hello kolekci Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="656d2-111">hello QuickBooks database server that is hosting hello company file must reside on a separate VM within hello same VNET as hello Azure RemoteApp collection.</span></span>  
> 
> 

## <a name="steps-toodeploy-quickbooks"></a><span data-ttu-id="656d2-112">Kroky toodeploy QuickBooks</span><span class="sxs-lookup"><span data-stu-id="656d2-112">Steps toodeploy QuickBooks</span></span>
1. <span data-ttu-id="656d2-113">Vytvořit virtuální počítač Azure a nainstalujte QuickBooks QuickBooks databázový server a umístit soubor společnosti hello na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="656d2-113">Create an Azure VM and install QuickBooks, QuickBooks database server, and place hello company file on a Azure VM.</span></span>  <span data-ttu-id="656d2-114">Ujistěte se, že tooproperly konfigurace pravidel brány firewall.</span><span class="sxs-lookup"><span data-stu-id="656d2-114">Make sure tooproperly configure firewall rules.</span></span>
2. <span data-ttu-id="656d2-115">Nainstalujte QuickBooks [vlastní image](remoteapp-imageoptions.md) a vytvoření [kolekci Azure Remoteappu](remoteapp-collections.md), cloud nebo hybridní v rámci hello přesnou stejnou virtuální síť, kde hello virtuální počítač hostitelem hello QuickBooks databázový server s soubory společnosti nachází.</span><span class="sxs-lookup"><span data-stu-id="656d2-115">Install QuickBooks on a [custom image](remoteapp-imageoptions.md) and create an [Azure RemoteApp collection](remoteapp-collections.md), either cloud or hybrid, within hello exact same VNET where hello VM hosting hello QuickBooks database server with company files resides.</span></span> 
3. <span data-ttu-id="656d2-116">[Publikování](remoteapp-publish.md) toousers QuickBooks aplikace</span><span class="sxs-lookup"><span data-stu-id="656d2-116">[Publish](remoteapp-publish.md) QuickBooks app toousers</span></span>
4. <span data-ttu-id="656d2-117">Spusťte hello hostovaných v Azure Remoteappu QuickBooks klienta, přejděte pomocí standardní síťový toohello virtuálních počítačů hostování hello QuickBooks databázový server Windows a otevřete soubor společnosti hello.</span><span class="sxs-lookup"><span data-stu-id="656d2-117">Launch hello Azure RemoteApp-hosted QuickBooks client, navigate using standard Windows networking toohello VM hosting hello QuickBooks database server and open hello company file.</span></span> 

## <a name="documentation-references"></a><span data-ttu-id="656d2-118">Odkazy na dokumentaci</span><span class="sxs-lookup"><span data-stu-id="656d2-118">Documentation references</span></span>
* <span data-ttu-id="656d2-119">QuickBooks [podporované konfigurace](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)</span><span class="sxs-lookup"><span data-stu-id="656d2-119">QuickBooks [supported configurations](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)</span></span>
* <span data-ttu-id="656d2-120">QuickBooks [možnosti nasazení](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)</span><span class="sxs-lookup"><span data-stu-id="656d2-120">QuickBooks [deployment options](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)</span></span>

<span data-ttu-id="656d2-121">Můžete se taky podívat na prezentace Ignite [základy pro Microsoft Azure RemoteApp správu a správu](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) -převinutí vpřed too1:02:45 tooget toohello QuickBooks část.</span><span class="sxs-lookup"><span data-stu-id="656d2-121">You can also check out my Ignite presentation, [Fundamentals of Microsoft Azure RemoteApp Management and Administration](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) - fast-forward too1:02:45 tooget toohello QuickBooks part.</span></span>

## <a name="deployment-architecture"></a><span data-ttu-id="656d2-122">Architektura nasazení služby</span><span class="sxs-lookup"><span data-stu-id="656d2-122">Deployment architecture</span></span>
![QuickBooks + nasazení Azure RemoteApp](./media/remoteapp-quickbooks/ra-quickbooks.png)

