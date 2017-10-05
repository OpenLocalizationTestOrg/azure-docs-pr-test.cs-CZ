---
title: "Služba Azure Active Directory Domain Services: Vytvoření nebo výběr virtuální sítě | Dokumentace Microsoftu"
description: "Povolení služby Azure Active Directory Domain Services pomocí portálu Azure Classic"
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
ms.openlocfilehash: 457519b00b65b0157effe2d4aba033a1c99852e8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a><span data-ttu-id="11b54-103">Vytvoření nebo výběr virtuální sítě pro Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="11b54-103">Create or select a virtual network for Azure Active Directory Domain Services</span></span>
## <a name="before-you-begin"></a><span data-ttu-id="11b54-104">Než začnete</span><span class="sxs-lookup"><span data-stu-id="11b54-104">Before you begin</span></span>
<span data-ttu-id="11b54-105">Přečtěte si článek [Důležité informace o sítích pro Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="11b54-105">Refer to [Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>

## <a name="task-2-create-an-azure-virtual-network"></a><span data-ttu-id="11b54-106">Úloha 2: Vytvořte virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="11b54-106">Task 2: Create an Azure virtual network</span></span>
<span data-ttu-id="11b54-107">Další úlohou konfigurace je vytvoření virtuální sítě Azure a podsítě v ní.</span><span class="sxs-lookup"><span data-stu-id="11b54-107">The next configuration task is to create an Azure virtual network and a subnet within it.</span></span> <span data-ttu-id="11b54-108">V této podsíti v rámci své virtuální sítě povolíte službu Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="11b54-108">You enable Azure Active Directory Domain Services in this subnet within your virtual network.</span></span> <span data-ttu-id="11b54-109">Pokud máte existující virtuální síť, kterou chcete použít, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="11b54-109">If you have an existing virtual network that you’d prefer to use, you can skip this step.</span></span>

> [!NOTE]
> <span data-ttu-id="11b54-110">Ujistěte se, že virtuální síť Azure, kterou vytváříte nebo si vyberete pro použití se službou Azure Active Directory Domain Services, patří do oblasti Azure podporované službou Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="11b54-110">Make sure that the Azure virtual network you create or choose to use with Azure Active Directory Domain Services belongs to an Azure region that's supported by Azure Active Directory Domain Services.</span></span> <span data-ttu-id="11b54-111">Oblasti Azure, ve kterých je služba Azure Active Directory Domain Services k dispozici, pro ověření najdete v článku [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="11b54-111">To ascertain the Azure regions in which Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>
>
><span data-ttu-id="11b54-112">Poznamenejte si název virtuální sítě, abyste při povolování služby Azure Active Directory Domain Services v dalším kroku konfigurace zajistili výběr správné virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="11b54-112">Note the name of the virtual network to ensure that you select the right virtual network when you enable Azure Active Directory Domain Services in a subsequent configuration step.</span></span>


<span data-ttu-id="11b54-113">Chcete-li vytvořit virtuální síť Azure, ve které chcete povolit Azure Active Directory Domain Services, postupujte podle těchto konfiguračních pokynů:</span><span class="sxs-lookup"><span data-stu-id="11b54-113">To create an Azure virtual network in which you want to enable Azure Active Directory Domain Services, follow these configuration instructions:</span></span>

1. <span data-ttu-id="11b54-114">Přejděte na [portál Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="11b54-114">Go to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="11b54-115">V levém podokně vyberte **Sítě**.</span><span class="sxs-lookup"><span data-stu-id="11b54-115">In the left pane, select **Networks**.</span></span>

    <span data-ttu-id="11b54-116">![Uzel sítí](./media/active-directory-domain-services-getting-started/networks-node.png)</span><span class="sxs-lookup"><span data-stu-id="11b54-116">![Networks node](./media/active-directory-domain-services-getting-started/networks-node.png)</span></span>  
    <span data-ttu-id="11b54-117">Otevře se okno **Virtuální sítě**.</span><span class="sxs-lookup"><span data-stu-id="11b54-117">The **Virtual Networks** window opens.</span></span>
3. <span data-ttu-id="11b54-118">V podokně úloh ve spodní části okna klikněte na **Nová**.</span><span class="sxs-lookup"><span data-stu-id="11b54-118">In the task pane at the bottom of the window, click **New**.</span></span>

    ![Okno Virtuální sítě](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. <span data-ttu-id="11b54-120">Klikněte na **Síťové služby** a vyberte možnost **Virtuální síť**.</span><span class="sxs-lookup"><span data-stu-id="11b54-120">Click **Network Services**, and then select **Virtual Network**.</span></span>

    ![Virtuální síť – rychle vytvořit](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. <span data-ttu-id="11b54-122">Vytvořte virtuální síť kliknutím na **Rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="11b54-122">To create a virtual network, click **Quick Create**.</span></span>

6. <span data-ttu-id="11b54-123">Zadejte **Název** své virtuální sítě a zvažte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="11b54-123">Specify a **Name** for your virtual network, and consider doing the following:</span></span>
    * <span data-ttu-id="11b54-124">Pro tuto síť můžete nakonfigurovat **Adresní prostor** nebo **Maximální počet virtuálních počítačů**.</span><span class="sxs-lookup"><span data-stu-id="11b54-124">You can choose to configure **Address space** or **Maximum VM count** for this network.</span></span>
    * <span data-ttu-id="11b54-125">Prozatím můžete ponechat možnost **Server DNS** nastavenou na hodnotu **Žádný**.</span><span class="sxs-lookup"><span data-stu-id="11b54-125">You can leave the **DNS server** setting as **None** for now.</span></span> <span data-ttu-id="11b54-126">Nastavení můžete aktualizovat poté, co povolíte službu Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="11b54-126">You can update the setting after you enable Azure Active Directory Domain Services.</span></span>
7. <span data-ttu-id="11b54-127">Z rozevíracího seznamu **Umístění** vyberte podporovanou oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="11b54-127">In the **Location** drop-down list, select a supported Azure region.</span></span>  
    <span data-ttu-id="11b54-128">Oblasti Azure, ve kterých je služba Azure Active Directory Domain Services k dispozici, pro ověření najdete v článku [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="11b54-128">To ascertain the Azure regions in which Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>
8. <span data-ttu-id="11b54-129">Vytvořte virtuální síť kliknutím na **Vytvořit virtuální síť**.</span><span class="sxs-lookup"><span data-stu-id="11b54-129">To create your virtual network, click **Create a Virtual Network**.</span></span>

    ![Vytvoření virtuální sítě pro Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. <span data-ttu-id="11b54-131">Po vytvoření virtuální sítě vyberte název virtuální sítě a pak klikněte na kartu **Konfigurovat**.</span><span class="sxs-lookup"><span data-stu-id="11b54-131">After you've created a virtual network, select the name of the virtual network, and then click the **Configure** tab.</span></span>

    ![Vytvoření podsítě](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. <span data-ttu-id="11b54-133">V části **adresních prostorů virtuální sítě** klikněte na **přidat podsíť**a pak zadejte podsíť s názvem **AaddsSubnet**.</span><span class="sxs-lookup"><span data-stu-id="11b54-133">Under **virtual network address spaces**, click **add subnet**, and then specify a subnet with the name **AaddsSubnet**.</span></span>

    ![Vytvoření podsítě pro Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. <span data-ttu-id="11b54-135">Kliknutím na **Uložit** vytvořte podsíť.</span><span class="sxs-lookup"><span data-stu-id="11b54-135">To create the subnet, click **Save**.</span></span>


## <a name="next-step"></a><span data-ttu-id="11b54-136">Další krok</span><span class="sxs-lookup"><span data-stu-id="11b54-136">Next step</span></span>
[<span data-ttu-id="11b54-137">Úloha 3: Povolení služby Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="11b54-137">Task 3: enable Azure Active Directory Domain Services</span></span>](active-directory-ds-getting-started-enableaadds.md)
