---
title: "Nasazení QuickBooks v Azure Remoteappu | Microsoft Docs"
description: "Naučte se sdílet QuickBooks s Azure Remoteappem."
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
ms.openlocfilehash: bbfac45220f6ef36c9951daae0ced1974e8ddabb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a><span data-ttu-id="cf356-103">Jak se nasadit QuickBooks v Azure Remoteappu?</span><span class="sxs-lookup"><span data-stu-id="cf356-103">How do you deploy QuickBooks in Azure RemoteApp?</span></span>
> [!IMPORTANT]
> <span data-ttu-id="cf356-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="cf356-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="cf356-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="cf356-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="cf356-106">Použijte tyto informace sdílet QuickBooks jako aplikace v Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="cf356-106">Use the following information to share QuickBooks as an app in Azure RemoteApp.</span></span>

<span data-ttu-id="cf356-107">QuickBooks 2015 Enterprise můžete sdílet s Azure Remoteappem v kolekci hybridní nebo cloud.</span><span class="sxs-lookup"><span data-stu-id="cf356-107">You can share QuickBooks 2015 Enterprise with Azure RemoteApp in either a hybrid or cloud collection.</span></span> <span data-ttu-id="cf356-108">Soubor společnosti se musí nacházet na virtuální počítač, který je spuštěn server QuickBooks databáze, která je oddělená od serverů Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="cf356-108">The company file must reside on a VM that is running QuickBooks database server that is separate from the Azure RemoteApp servers.</span></span> <span data-ttu-id="cf356-109">Nikdy uložit soubor společnosti na image Azure Remoteappu – ztráta dat je očekáváno, pokud to uděláte.</span><span class="sxs-lookup"><span data-stu-id="cf356-109">Never store the company file on your Azure RemoteApp image - data loss is expected if you do this.</span></span> <span data-ttu-id="cf356-110">Pouze QuickBooks Enterprise podporuje hostování soubor QuickBooks externí sdílené složky s QuickBooks databázový server přístupné přes standardní sítě systému Windows.</span><span class="sxs-lookup"><span data-stu-id="cf356-110">Only QuickBooks Enterprise supports hosting the QuickBooks file on an external share with QuickBooks database server accessible via standard Windows networking.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="cf356-111">QuickBooks databázový server, který je hostitelem soubor společnosti se musí nacházet na samostatný virtuální počítač v rámci stejné virtuální síti jako kolekci Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="cf356-111">The QuickBooks database server that is hosting the company file must reside on a separate VM within the same VNET as the Azure RemoteApp collection.</span></span>  
> 
> 

## <a name="steps-to-deploy-quickbooks"></a><span data-ttu-id="cf356-112">Postup nasazení QuickBooks</span><span class="sxs-lookup"><span data-stu-id="cf356-112">Steps to deploy QuickBooks</span></span>
1. <span data-ttu-id="cf356-113">Vytvořit virtuální počítač Azure a nainstalujte QuickBooks QuickBooks databázový server a umístit soubor společnosti na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="cf356-113">Create an Azure VM and install QuickBooks, QuickBooks database server, and place the company file on a Azure VM.</span></span>  <span data-ttu-id="cf356-114">Ujistěte se, že jste správně nakonfigurovat pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="cf356-114">Make sure to properly configure firewall rules.</span></span>
2. <span data-ttu-id="cf356-115">Nainstalujte QuickBooks [vlastní image](remoteapp-imageoptions.md) a vytvoření [kolekci Azure Remoteappu](remoteapp-collections.md), cloud nebo hybridní v rámci přesně stejnou virtuální síť, kde je umístěn virtuální počítač hostuje QuickBooks databázový server s firemním souborům.</span><span class="sxs-lookup"><span data-stu-id="cf356-115">Install QuickBooks on a [custom image](remoteapp-imageoptions.md) and create an [Azure RemoteApp collection](remoteapp-collections.md), either cloud or hybrid, within the exact same VNET where the VM hosting the QuickBooks database server with company files resides.</span></span> 
3. <span data-ttu-id="cf356-116">[Publikování](remoteapp-publish.md) QuickBooks aplikaci pro uživatele</span><span class="sxs-lookup"><span data-stu-id="cf356-116">[Publish](remoteapp-publish.md) QuickBooks app to users</span></span>
4. <span data-ttu-id="cf356-117">Spusťte klienta QuickBooks hostovaných v Azure Remoteappu, přejděte pomocí standardní sítě systému Windows na virtuální počítač, který je hostitelem databáze serveru QuickBooks a otevřete soubor společnosti.</span><span class="sxs-lookup"><span data-stu-id="cf356-117">Launch the Azure RemoteApp-hosted QuickBooks client, navigate using standard Windows networking to the VM hosting the QuickBooks database server and open the company file.</span></span> 

## <a name="documentation-references"></a><span data-ttu-id="cf356-118">Odkazy na dokumentaci</span><span class="sxs-lookup"><span data-stu-id="cf356-118">Documentation references</span></span>
* <span data-ttu-id="cf356-119">QuickBooks [podporované konfigurace](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)</span><span class="sxs-lookup"><span data-stu-id="cf356-119">QuickBooks [supported configurations](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)</span></span>
* <span data-ttu-id="cf356-120">QuickBooks [možnosti nasazení](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)</span><span class="sxs-lookup"><span data-stu-id="cf356-120">QuickBooks [deployment options](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)</span></span>

<span data-ttu-id="cf356-121">Můžete se taky podívat na prezentace Ignite [základy pro Microsoft Azure RemoteApp správu a správu](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) -rychlé převíjení vpřed na 1:02:45 získat QuickBooks část.</span><span class="sxs-lookup"><span data-stu-id="cf356-121">You can also check out my Ignite presentation, [Fundamentals of Microsoft Azure RemoteApp Management and Administration](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) - fast-forward to 1:02:45 to get to the QuickBooks part.</span></span>

## <a name="deployment-architecture"></a><span data-ttu-id="cf356-122">Architektura nasazení služby</span><span class="sxs-lookup"><span data-stu-id="cf356-122">Deployment architecture</span></span>
![QuickBooks + nasazení Azure RemoteApp](./media/remoteapp-quickbooks/ra-quickbooks.png)

