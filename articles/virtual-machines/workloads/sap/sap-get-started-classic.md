---
title: "Pomocí SAP na virtuální počítače s Linuxem | Microsoft Docs"
description: "Přečtěte si o používání systému SAP na virtuálních počítačích s Linuxem v prostředí Microsoft Azure."
services: virtual-machines-linux,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: timlt
editor: 
tags: azure-service-management
keywords: 
ms.assetid: f9cd93dc-71ad-48a4-8778-4e48aec484a6
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: campaign-page
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: 66eb53f99ce4b30ec283243deb3649c9ca897a2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-sap-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="22574-103">Pomocí SAP na virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="22574-103">Using SAP on Linux virtual machines in Azure</span></span>
<span data-ttu-id="22574-104">Cloud Computing je často používaný termín, který získává v oblasti IT čím dál větší význam – od malých firem až po velké a nadnárodní společnosti.</span><span class="sxs-lookup"><span data-stu-id="22574-104">Cloud Computing is a widely used term which is gaining more and more importance within the IT industry, from small companies up to large and multinational corporations.</span></span> <span data-ttu-id="22574-105">Microsoft Azure je platforma cloudových služeb od společnosti Microsoft, která nabízí široké spektrum nových možností.</span><span class="sxs-lookup"><span data-stu-id="22574-105">Microsoft Azure is the Cloud Services Platform from Microsoft which offers a wide spectrum of new possibilities.</span></span> <span data-ttu-id="22574-106">Nyní mohou zákazníci rychle zřizovat a zrušit aplikace jako cloudové služby, takže už nejsou omezeni technickými nebo rozpočtovými limity.</span><span class="sxs-lookup"><span data-stu-id="22574-106">Now customers are able to rapidly provision and de-provision applications as Cloud-Services, so they are not limited to technical or budgeting restrictions.</span></span> <span data-ttu-id="22574-107">Namísto investování času a prostředků do hardwarové infrastruktury se společností můžou zaměřit na aplikace, obchodní procesy a jejich výhody pro zákazníky a uživatele.</span><span class="sxs-lookup"><span data-stu-id="22574-107">Instead of investing time and budget into hardware infrastructure, companies can focus on the application, business processes and its benefits for customers and users.</span></span>

<span data-ttu-id="22574-108">S virtuálními počítači Microsoft Azure společnost Microsoft nabízí komplexní infrastruktury jako služby (IaaS) platformu.</span><span class="sxs-lookup"><span data-stu-id="22574-108">With Microsoft Azure virtual machines, Microsoft offers a comprehensive Infrastructure as a Service (IaaS) platform.</span></span> <span data-ttu-id="22574-109">Služba Azure Virtual Machines teď v rámci IaaS podporuje aplikace využívající SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="22574-109">SAP NetWeaver based applications are supported on Azure Virtual Machines (IaaS).</span></span> <span data-ttu-id="22574-110">Dokumenty White Paper níže popisují, jak naplánovat a implementovat aplikace SAP NetWeaver založené na virtuálních počítačích s Windows v Azure.</span><span class="sxs-lookup"><span data-stu-id="22574-110">The whitepapers below describe how to plan and implement SAP NetWeaver based applications on Windows virtual machines in Azure.</span></span> <span data-ttu-id="22574-111">Můžete taky implementovat SAP NetWeaver na základě aplikací na [virtuální počítače s Windows](../../virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="22574-111">You can also implement SAP NetWeaver based applications on [Windows virtual machines](../../virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure-suse-linux-virtual-machines"></a><span data-ttu-id="22574-112">SAP NetWeaver na virtuálních počítačích Azure SUSE Linux</span><span class="sxs-lookup"><span data-stu-id="22574-112">SAP NetWeaver on Azure SUSE Linux Virtual Machines</span></span>
<span data-ttu-id="22574-113">Title: Testování SAP NetWeaver na virtuálních počítačích Microsoft Azure SUSE Linux</span><span class="sxs-lookup"><span data-stu-id="22574-113">Title: Testing SAP NetWeaver on Microsoft Azure SUSE Linux VMs</span></span>

<span data-ttu-id="22574-114">Souhrn: Neexistuje žádná oficiální SAP podpora pro spouštění SAP NetWeaver na virtuálních počítačích Azure Linux v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="22574-114">Summary: There is no official SAP support for running SAP NetWeaver on Azure Linux VMs at this point in time.</span></span> <span data-ttu-id="22574-115">Nicméně zákazníci chtít provést některé testování nebo zvažte spustit ukázkový SAP nebo systémy školení na virtuálních počítačích Azure Linux, dokud není nutné pro kontaktování podpory SAP.</span><span class="sxs-lookup"><span data-stu-id="22574-115">Nevertheless customers might want to do some testing or might consider to run SAP demo or training systems on Azure Linux VMs as long as there is no need for contacting SAP support.</span></span> <span data-ttu-id="22574-116">Tento článek by měly pomoci nastavení virtuálních počítačích Azure SUSE Linux ke spuštění SAP a nabízí několik základních informací, aby se zabránilo Potenciální nástrahy běžné.</span><span class="sxs-lookup"><span data-stu-id="22574-116">This article should help setting up Azure SUSE Linux VMs for running SAP and gives some basic hints in order to avoid common potential pitfalls.</span></span>

<span data-ttu-id="22574-117">Aktualizované: Prosince 2015</span><span class="sxs-lookup"><span data-stu-id="22574-117">Updated: December 2015</span></span>

[<span data-ttu-id="22574-118">V tomto článku naleznete zde</span><span class="sxs-lookup"><span data-stu-id="22574-118">This article can be found here</span></span>](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

