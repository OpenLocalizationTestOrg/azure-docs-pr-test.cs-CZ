---
title: "aaaUsing SAP na virtuální počítače s Linuxem | Microsoft Docs"
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
ms.openlocfilehash: a805cdecb515239057e185a92bf5c4d4e707f72f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-sap-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="544dd-103">Pomocí SAP na virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="544dd-103">Using SAP on Linux virtual machines in Azure</span></span>
<span data-ttu-id="544dd-104">Cloud Computing je často používaný termín, který je získání více význam v rámci hello IT odvětví, z malých společností až toolarge a mezinárodních společnosti.</span><span class="sxs-lookup"><span data-stu-id="544dd-104">Cloud Computing is a widely used term which is gaining more and more importance within hello IT industry, from small companies up toolarge and multinational corporations.</span></span> <span data-ttu-id="544dd-105">Microsoft Azure je hello cloudové služby společnosti Microsoft, který nabízí široké spektrum nové možnosti.</span><span class="sxs-lookup"><span data-stu-id="544dd-105">Microsoft Azure is hello Cloud Services Platform from Microsoft which offers a wide spectrum of new possibilities.</span></span> <span data-ttu-id="544dd-106">Nyní zákazníci jsou zřídit a deaktivace zřízení aplikací mít toorapidly jako cloudové služby, takže nejsou omezené tootechnical nebo rozpočet omezení.</span><span class="sxs-lookup"><span data-stu-id="544dd-106">Now customers are able toorapidly provision and de-provision applications as Cloud-Services, so they are not limited tootechnical or budgeting restrictions.</span></span> <span data-ttu-id="544dd-107">Společnosti můžete místo času a investovat do hardwaru infrastruktury, soustředit na aplikace hello, podnikové procesy a jeho výhody pro zákazníky a uživatele.</span><span class="sxs-lookup"><span data-stu-id="544dd-107">Instead of investing time and budget into hardware infrastructure, companies can focus on hello application, business processes and its benefits for customers and users.</span></span>

<span data-ttu-id="544dd-108">S virtuálními počítači Microsoft Azure společnost Microsoft nabízí komplexní infrastruktury jako služby (IaaS) platformu.</span><span class="sxs-lookup"><span data-stu-id="544dd-108">With Microsoft Azure virtual machines, Microsoft offers a comprehensive Infrastructure as a Service (IaaS) platform.</span></span> <span data-ttu-id="544dd-109">Služba Azure Virtual Machines teď v rámci IaaS podporuje aplikace využívající SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="544dd-109">SAP NetWeaver based applications are supported on Azure Virtual Machines (IaaS).</span></span> <span data-ttu-id="544dd-110">Hello dokumenty White Paper níže popisují, jak tooplan a implementujte SAP NetWeaver na základě aplikací na virtuálních počítačích s Windows v Azure.</span><span class="sxs-lookup"><span data-stu-id="544dd-110">hello whitepapers below describe how tooplan and implement SAP NetWeaver based applications on Windows virtual machines in Azure.</span></span> <span data-ttu-id="544dd-111">Můžete taky implementovat SAP NetWeaver na základě aplikací na [virtuální počítače s Windows](../../windows/classic/sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="544dd-111">You can also implement SAP NetWeaver based applications on [Windows virtual machines](../../windows/classic/sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure-suse-linux-virtual-machines"></a><span data-ttu-id="544dd-112">SAP NetWeaver na virtuálních počítačích Azure SUSE Linux</span><span class="sxs-lookup"><span data-stu-id="544dd-112">SAP NetWeaver on Azure SUSE Linux Virtual Machines</span></span>
<span data-ttu-id="544dd-113">Title: aaaTesting SAP NetWeaver na virtuálních počítačích Microsoft Azure SUSE Linux</span><span class="sxs-lookup"><span data-stu-id="544dd-113">title: aaaTesting SAP NetWeaver on Microsoft Azure SUSE Linux VMs</span></span>

<span data-ttu-id="544dd-114">Souhrn: Neexistuje žádná oficiální SAP podpora pro spouštění SAP NetWeaver na virtuálních počítačích Azure Linux v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="544dd-114">Summary: There is no official SAP support for running SAP NetWeaver on Azure Linux VMs at this point in time.</span></span> <span data-ttu-id="544dd-115">Nicméně zákazníci můžou toodo některé testování nebo zvažte toorun SAP ukázku nebo školení systémů na virtuálních počítačích Azure Linux, dokud není nutné pro kontaktování podpory SAP.</span><span class="sxs-lookup"><span data-stu-id="544dd-115">Nevertheless customers might want toodo some testing or might consider toorun SAP demo or training systems on Azure Linux VMs as long as there is no need for contacting SAP support.</span></span> <span data-ttu-id="544dd-116">Tento článek by měly pomoci nastavení virtuálních počítačích Azure SUSE Linux ke spuštění SAP a poskytuje společné Potenciální nástrahy tooavoid některé základní pomocné parametry v pořadí.</span><span class="sxs-lookup"><span data-stu-id="544dd-116">This article should help setting up Azure SUSE Linux VMs for running SAP and gives some basic hints in order tooavoid common potential pitfalls.</span></span>

<span data-ttu-id="544dd-117">Aktualizované: Prosince 2015</span><span class="sxs-lookup"><span data-stu-id="544dd-117">Updated: December 2015</span></span>

[<span data-ttu-id="544dd-118">V tomto článku naleznete zde</span><span class="sxs-lookup"><span data-stu-id="544dd-118">This article can be found here</span></span>](../../virtual-machines-linux-sap-on-suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

