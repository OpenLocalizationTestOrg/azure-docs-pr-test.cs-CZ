---
title: "Azure Active Directory Domain Services: Začínáme | Microsoft Docs"
description: "Povolit Azure Active Directory Domain Services pomocí portálu Azure (Preview)"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: maheshu
ms.openlocfilehash: 7f420d60862adf61e4f21e5abac2932a742bd55d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a><span data-ttu-id="a6348-103">Povolit Azure Active Directory Domain Services pomocí portálu Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="a6348-103">Enable Azure Active Directory Domain Services using the Azure portal (Preview)</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="a6348-104">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a6348-104">Before you begin</span></span>
<span data-ttu-id="a6348-105">Přečtěte si článek [Důležité informace o sítích pro Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="a6348-105">Refer to [Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>


## <a name="task-2-configure-network-settings"></a><span data-ttu-id="a6348-106">Úloha 2: Konfigurace nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="a6348-106">Task 2: configure network settings</span></span>
<span data-ttu-id="a6348-107">Další úlohou konfigurace je vytvoření virtuální sítě Azure a vyhrazené podsítě v něm.</span><span class="sxs-lookup"><span data-stu-id="a6348-107">The next configuration task is to create an Azure virtual network and a dedicated subnet within it.</span></span> <span data-ttu-id="a6348-108">V této podsíti v rámci své virtuální sítě povolíte službu Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="a6348-108">You enable Azure Active Directory Domain Services in this subnet within your virtual network.</span></span> <span data-ttu-id="a6348-109">Také můžete vybrat existující virtuální síť a vytvořit vyhrazený podsítě v něm.</span><span class="sxs-lookup"><span data-stu-id="a6348-109">You may also pick an existing virtual network and create the dedicated subnet within it.</span></span>

1. <span data-ttu-id="a6348-110">Klikněte na tlačítko **virtuální síť** vyberte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="a6348-110">Click **Virtual network** to select a virtual network.</span></span>
2. <span data-ttu-id="a6348-111">Na **zvolte virtuální síť** okně se zobrazí všechny existující virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a6348-111">On the **Choose virtual network** blade, you see all existing virtual networks.</span></span> <span data-ttu-id="a6348-112">Zobrazí pouze virtuální sítě, které patří do skupiny prostředků a umístění Azure, které jste vybrali na **Základy** stránce průvodce.</span><span class="sxs-lookup"><span data-stu-id="a6348-112">You see only the virtual networks that belong to the resource group and Azure location you have selected on the **Basics** wizard page.</span></span>

3. <span data-ttu-id="a6348-113">Vyberte virtuální síť, ve kterém by měl být povolili Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="a6348-113">Choose the virtual network in which Azure AD Domain Services should be enabled.</span></span> <span data-ttu-id="a6348-114">Klikněte na tlačítko **vytvořit nový**, pokud chcete vytvořit novou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="a6348-114">Click **Create new**, if you prefer to create a new virtual network.</span></span> <span data-ttu-id="a6348-115">Důrazně doporučujeme používat vyhrazené podsíť pro Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="a6348-115">We highly recommend using a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="a6348-116">Pokud vyberete existující virtuální síť, [vytvořit vyhrazený podsíť pomocí virtuální sítě rozšíření](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) a pak vyberte této podsíti.</span><span class="sxs-lookup"><span data-stu-id="a6348-116">If you pick an existing virtual network, [create a dedicated subnet using the virtual networks extension](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) and then pick that subnet.</span></span> 

    ![Vyberte virtuální síť](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. <span data-ttu-id="a6348-118">Klikněte na tlačítko **podsíť** vybrat vyhrazené podsítě v této virtuální síti, ve které chcete povolit vaší nové spravované domény.</span><span class="sxs-lookup"><span data-stu-id="a6348-118">Click **Subnet** to pick the dedicated subnet in this virtual network, within which to enable your new managed domain.</span></span> <span data-ttu-id="a6348-119">V **vytvořit podsíť** okno, zadejte název pro podsíť a klikněte na tlačítko **OK** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="a6348-119">In the **Create subnet** blade, specify a name for the subnet, and click **OK** when you're done.</span></span> <span data-ttu-id="a6348-120">Můžete například vytvořte podsíť s názvem "DomainServices", a usnadňuje pro ostatní správci zjistit, co je nasazen v rámci podsítě.</span><span class="sxs-lookup"><span data-stu-id="a6348-120">For example, create a subnet with the name 'DomainServices', making it easy for other administrators to understand what is deployed within the subnet.</span></span>

    ![Vyberte podsítí v rámci virtuální sítě](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > <span data-ttu-id="a6348-122">**Pokyny pro výběr podsíť**</span><span class="sxs-lookup"><span data-stu-id="a6348-122">**Guidelines for selecting a subnet**</span></span>
  > 1. <span data-ttu-id="a6348-123">Použijte vyhrazenou podsíť pro Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="a6348-123">Use a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="a6348-124">Nenasazujte případných dalších virtuálních počítačů na této podsíti.</span><span class="sxs-lookup"><span data-stu-id="a6348-124">Do not deploy any other virtual machines to this subnet.</span></span> <span data-ttu-id="a6348-125">Tato konfigurace umožňuje nakonfigurovat skupiny zabezpečení sítě (Nsg) pro zatížení nebo virtuální počítače bez nutnosti přerušení vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="a6348-125">This configuration enables you to configure network security groups (NSGs) for your workloads/virtual machines without disrupting your managed domain.</span></span> <span data-ttu-id="a6348-126">Podrobnosti najdete v tématu [sítě důležité informace týkající se Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="a6348-126">For details, see [networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>
  2. <span data-ttu-id="a6348-127">Nevybírejte podsíť brány pro nasazení služby Azure AD Domain Services, protože se nejedná o podporovanou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a6348-127">Do not select the Gateway subnet for deploying Azure AD Domain Services, because it is not a supported configuration.</span></span>
  3. <span data-ttu-id="a6348-128">Zajistěte, aby podsíť, kterou jste vybrali dostatek prostoru k dispozici adres - aspoň 3 až 5 dostupných IP adres.</span><span class="sxs-lookup"><span data-stu-id="a6348-128">Ensure that the subnet you've selected has sufficient available address space - at least 3-5 available IP addresses.</span></span>
  >

5. <span data-ttu-id="a6348-129">Až budete hotovi, klikněte na tlačítko **OK** přesunout na **skupiny správců** stránce průvodce.</span><span class="sxs-lookup"><span data-stu-id="a6348-129">When you are done, click **OK** to move on to the **Administrator group** page of the wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="a6348-130">Další krok</span><span class="sxs-lookup"><span data-stu-id="a6348-130">Next step</span></span>
[<span data-ttu-id="a6348-131">Úloha 3: Konfigurace skupiny pro správu a povolení služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="a6348-131">Task 3: configure administrative group and enable Azure AD Domain Services</span></span>](active-directory-ds-getting-started-admingroup.md)
