---
title: "Služba Azure Active Directory Domain Services: Vytvoření nebo výběr virtuální sítě | Dokumentace Microsoftu"
description: "Povolit Azure Active Directory Domain Services pomocí hello portál Azure classic"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.openlocfilehash: 212c741b20e846742d94f70342c4263ce8984153
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a><span data-ttu-id="85b6a-103">Vytvoření nebo výběr virtuální sítě pro Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="85b6a-103">Create or select a virtual network for Azure Active Directory Domain Services</span></span>
## <a name="before-you-begin"></a><span data-ttu-id="85b6a-104">Než začnete</span><span class="sxs-lookup"><span data-stu-id="85b6a-104">Before you begin</span></span>
<span data-ttu-id="85b6a-105">Odkazovat příliš[požadavky sítě pro Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="85b6a-105">Refer too[Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>

## <a name="task-2-create-an-azure-virtual-network"></a><span data-ttu-id="85b6a-106">Úloha 2: Vytvořte virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="85b6a-106">Task 2: Create an Azure virtual network</span></span>
<span data-ttu-id="85b6a-107">Další úlohou konfigurace Hello je toocreate virtuální sítě Azure a podsítě v něm.</span><span class="sxs-lookup"><span data-stu-id="85b6a-107">hello next configuration task is toocreate an Azure virtual network and a subnet within it.</span></span> <span data-ttu-id="85b6a-108">V této podsíti v rámci své virtuální sítě povolíte službu Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="85b6a-108">You enable Azure Active Directory Domain Services in this subnet within your virtual network.</span></span> <span data-ttu-id="85b6a-109">Pokud máte existující virtuální síť, která si přejete toouse, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="85b6a-109">If you have an existing virtual network that you’d prefer toouse, you can skip this step.</span></span>

> [!NOTE]
> <span data-ttu-id="85b6a-110">Ujistěte se, že hello virtuální síť Azure, vytvořte nebo zvolte, toouse s Azure Active Directory Domain Services patří tooan oblast Azure, která je podporována službou Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="85b6a-110">Make sure that hello Azure virtual network you create or choose toouse with Azure Active Directory Domain Services belongs tooan Azure region that's supported by Azure Active Directory Domain Services.</span></span> <span data-ttu-id="85b6a-111">tooascertain hello oblastí Azure, ve kterých je k dispozici Azure Active Directory Domain Services najdete v tématu [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="85b6a-111">tooascertain hello Azure regions in which Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>
>
><span data-ttu-id="85b6a-112">Poznamenejte si název hello tooensure hello virtuální sítě vyberte správnou virtuální síť hello, pokud povolíte Azure Active Directory Domain Services v dalším kroku konfigurace.</span><span class="sxs-lookup"><span data-stu-id="85b6a-112">Note hello name of hello virtual network tooensure that you select hello right virtual network when you enable Azure Active Directory Domain Services in a subsequent configuration step.</span></span>


<span data-ttu-id="85b6a-113">toocreate Azure virtuální sítě, ve kterém chcete tooenable Azure Active Directory Domain Services, postupujte podle těchto pokynů konfigurace:</span><span class="sxs-lookup"><span data-stu-id="85b6a-113">toocreate an Azure virtual network in which you want tooenable Azure Active Directory Domain Services, follow these configuration instructions:</span></span>

1. <span data-ttu-id="85b6a-114">Přejděte toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="85b6a-114">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="85b6a-115">V levém podokně hello vyberte **sítě**.</span><span class="sxs-lookup"><span data-stu-id="85b6a-115">In hello left pane, select **Networks**.</span></span>

    <span data-ttu-id="85b6a-116">![Uzel sítí](./media/active-directory-domain-services-getting-started/networks-node.png)</span><span class="sxs-lookup"><span data-stu-id="85b6a-116">![Networks node](./media/active-directory-domain-services-getting-started/networks-node.png)</span></span>  
    <span data-ttu-id="85b6a-117">Hello **virtuální sítě** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="85b6a-117">hello **Virtual Networks** window opens.</span></span>
3. <span data-ttu-id="85b6a-118">V podokně úloh v dolní části hello okna hello hello, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="85b6a-118">In hello task pane at hello bottom of hello window, click **New**.</span></span>

    ![Okno Virtuální sítě](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. <span data-ttu-id="85b6a-120">Klikněte na **Síťové služby** a vyberte možnost **Virtuální síť**.</span><span class="sxs-lookup"><span data-stu-id="85b6a-120">Click **Network Services**, and then select **Virtual Network**.</span></span>

    ![Virtuální síť – rychle vytvořit](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. <span data-ttu-id="85b6a-122">toocreate virtuální síť, klikněte na tlačítko **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="85b6a-122">toocreate a virtual network, click **Quick Create**.</span></span>

6. <span data-ttu-id="85b6a-123">Zadejte **název** pro svoji virtuální sítě a zvažte provedení hello následující:</span><span class="sxs-lookup"><span data-stu-id="85b6a-123">Specify a **Name** for your virtual network, and consider doing hello following:</span></span>
    * <span data-ttu-id="85b6a-124">Můžete zvolit tooconfigure **adresní prostor** nebo **maximální počet virtuálních počítačů** pro tuto síť.</span><span class="sxs-lookup"><span data-stu-id="85b6a-124">You can choose tooconfigure **Address space** or **Maximum VM count** for this network.</span></span>
    * <span data-ttu-id="85b6a-125">Můžete ponechat hello **DNS server** nastavení jako **žádné** teď.</span><span class="sxs-lookup"><span data-stu-id="85b6a-125">You can leave hello **DNS server** setting as **None** for now.</span></span> <span data-ttu-id="85b6a-126">Po povolení Azure Active Directory Domain Services, můžete aktualizovat nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="85b6a-126">You can update hello setting after you enable Azure Active Directory Domain Services.</span></span>
7. <span data-ttu-id="85b6a-127">V hello **umístění** rozevíracího seznamu vyberte podporovanou oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="85b6a-127">In hello **Location** drop-down list, select a supported Azure region.</span></span>  
    <span data-ttu-id="85b6a-128">tooascertain hello oblastí Azure, ve kterých je k dispozici Azure Active Directory Domain Services najdete v tématu [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="85b6a-128">tooascertain hello Azure regions in which Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>
8. <span data-ttu-id="85b6a-129">toocreate vaší virtuální síti, klikněte na tlačítko **vytvořit virtuální síť**.</span><span class="sxs-lookup"><span data-stu-id="85b6a-129">toocreate your virtual network, click **Create a Virtual Network**.</span></span>

    ![Vytvoření virtuální sítě pro Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. <span data-ttu-id="85b6a-131">Po vytvoření virtuální sítě, vyberte název hello hello virtuální sítě a pak klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="85b6a-131">After you've created a virtual network, select hello name of hello virtual network, and then click hello **Configure** tab.</span></span>

    ![Vytvoření podsítě](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. <span data-ttu-id="85b6a-133">V části **adresní prostory virtuální sítě**, klikněte na tlačítko **přidat podsíť**a pak zadejte podsíť s názvem hello **AaddsSubnet**.</span><span class="sxs-lookup"><span data-stu-id="85b6a-133">Under **virtual network address spaces**, click **add subnet**, and then specify a subnet with hello name **AaddsSubnet**.</span></span>

    ![Vytvoření podsítě pro Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. <span data-ttu-id="85b6a-135">toocreate hello podsítě, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="85b6a-135">toocreate hello subnet, click **Save**.</span></span>


## <a name="next-step"></a><span data-ttu-id="85b6a-136">Další krok</span><span class="sxs-lookup"><span data-stu-id="85b6a-136">Next step</span></span>
[<span data-ttu-id="85b6a-137">Úloha 3: Povolení služby Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="85b6a-137">Task 3: enable Azure Active Directory Domain Services</span></span>](active-directory-ds-getting-started-enableaadds.md)
