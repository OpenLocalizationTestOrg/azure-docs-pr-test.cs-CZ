---
title: "Azure Active Directory Domain Services: Začínáme | Microsoft Docs"
description: "Povolit Azure Active Directory Domain Services pomocí hello portálu Azure (Preview)"
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
ms.openlocfilehash: 7695dabb11df8d9dcfdac24996edf021af2e7f52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a><span data-ttu-id="13270-103">Povolit Azure Active Directory Domain Services pomocí hello portálu Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="13270-103">Enable Azure Active Directory Domain Services using hello Azure portal (Preview)</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="13270-104">Než začnete</span><span class="sxs-lookup"><span data-stu-id="13270-104">Before you begin</span></span>
<span data-ttu-id="13270-105">Odkazovat příliš[požadavky sítě pro Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="13270-105">Refer too[Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>


## <a name="task-2-configure-network-settings"></a><span data-ttu-id="13270-106">Úloha 2: Konfigurace nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="13270-106">Task 2: configure network settings</span></span>
<span data-ttu-id="13270-107">Další úlohou konfigurace Hello je toocreate virtuální sítě Azure a vyhrazené podsítě v něm.</span><span class="sxs-lookup"><span data-stu-id="13270-107">hello next configuration task is toocreate an Azure virtual network and a dedicated subnet within it.</span></span> <span data-ttu-id="13270-108">V této podsíti v rámci své virtuální sítě povolíte službu Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="13270-108">You enable Azure Active Directory Domain Services in this subnet within your virtual network.</span></span> <span data-ttu-id="13270-109">Vyberte existující virtuální síť, kde můžete vytvořit hello vyhrazené podsítě v něm.</span><span class="sxs-lookup"><span data-stu-id="13270-109">You may also pick an existing virtual network and create hello dedicated subnet within it.</span></span>

1. <span data-ttu-id="13270-110">Klikněte na tlačítko **virtuální síť** tooselect virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="13270-110">Click **Virtual network** tooselect a virtual network.</span></span>
2. <span data-ttu-id="13270-111">Na hello **zvolte virtuální síť** okně se zobrazí všechny existující virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="13270-111">On hello **Choose virtual network** blade, you see all existing virtual networks.</span></span> <span data-ttu-id="13270-112">Zobrazí pouze hello virtuální sítě, které patří toohello skupinu prostředků a umístění Azure, které jste vybrali na hello **Základy** stránce průvodce.</span><span class="sxs-lookup"><span data-stu-id="13270-112">You see only hello virtual networks that belong toohello resource group and Azure location you have selected on hello **Basics** wizard page.</span></span>

3. <span data-ttu-id="13270-113">Vyberte hello virtuální síť, ve kterém by měl povolit Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="13270-113">Choose hello virtual network in which Azure AD Domain Services should be enabled.</span></span> <span data-ttu-id="13270-114">Klikněte na tlačítko **vytvořit nový**, pokud dáváte přednost toocreate nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="13270-114">Click **Create new**, if you prefer toocreate a new virtual network.</span></span> <span data-ttu-id="13270-115">Důrazně doporučujeme používat vyhrazené podsíť pro Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="13270-115">We highly recommend using a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="13270-116">Pokud vyberete existující virtuální síť, [vytvořit vyhrazený podsíť pomocí rozšíření virtuální sítě hello](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) a pak vyberte této podsíti.</span><span class="sxs-lookup"><span data-stu-id="13270-116">If you pick an existing virtual network, [create a dedicated subnet using hello virtual networks extension](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) and then pick that subnet.</span></span> 

    ![Vyberte virtuální síť](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. <span data-ttu-id="13270-118">Klikněte na tlačítko **podsíť** toopick hello vyhrazené podsítě v této virtuální síti, v rámci které tooenable nové spravované domény.</span><span class="sxs-lookup"><span data-stu-id="13270-118">Click **Subnet** toopick hello dedicated subnet in this virtual network, within which tooenable your new managed domain.</span></span> <span data-ttu-id="13270-119">V hello **vytvořit podsíť** okno, zadejte název pro podsíť hello a klikněte na tlačítko **OK** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="13270-119">In hello **Create subnet** blade, specify a name for hello subnet, and click **OK** when you're done.</span></span> <span data-ttu-id="13270-120">Například vytvořte podsíť s názvem hello 'DomainServices', a usnadňuje pro ostatní správci toounderstand co je nasazen v rámci podsítě hello.</span><span class="sxs-lookup"><span data-stu-id="13270-120">For example, create a subnet with hello name 'DomainServices', making it easy for other administrators toounderstand what is deployed within hello subnet.</span></span>

    ![Vyberte podsíť v rámci virtuální sítě hello](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > <span data-ttu-id="13270-122">**Pokyny pro výběr podsíť**</span><span class="sxs-lookup"><span data-stu-id="13270-122">**Guidelines for selecting a subnet**</span></span>
  > 1. <span data-ttu-id="13270-123">Použijte vyhrazenou podsíť pro Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="13270-123">Use a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="13270-124">Nenasazujte žádné jiné podsíti toothis virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="13270-124">Do not deploy any other virtual machines toothis subnet.</span></span> <span data-ttu-id="13270-125">Tuto konfiguraci můžete tooconfigure skupin zabezpečení sítě (Nsg) pro zatížení nebo virtuální počítače bez nutnosti přerušení vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="13270-125">This configuration enables you tooconfigure network security groups (NSGs) for your workloads/virtual machines without disrupting your managed domain.</span></span> <span data-ttu-id="13270-126">Podrobnosti najdete v tématu [sítě důležité informace týkající se Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="13270-126">For details, see [networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>
  2. <span data-ttu-id="13270-127">Nevybírejte hello podsíť brány pro nasazení služby Azure AD Domain Services, protože se nejedná o podporovanou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="13270-127">Do not select hello Gateway subnet for deploying Azure AD Domain Services, because it is not a supported configuration.</span></span>
  3. <span data-ttu-id="13270-128">Zajistěte, aby byl dostatek prostoru k dispozici adres - IP adres k dispozici minimálně 3 až 5 této podsíti hello, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="13270-128">Ensure that hello subnet you've selected has sufficient available address space - at least 3-5 available IP addresses.</span></span>
  >

5. <span data-ttu-id="13270-129">Až budete hotovi, klikněte na tlačítko **OK** toomove na toohello **skupiny správců** hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="13270-129">When you are done, click **OK** toomove on toohello **Administrator group** page of hello wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="13270-130">Další krok</span><span class="sxs-lookup"><span data-stu-id="13270-130">Next step</span></span>
[<span data-ttu-id="13270-131">Úloha 3: Konfigurace skupiny pro správu a povolení služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="13270-131">Task 3: configure administrative group and enable Azure AD Domain Services</span></span>](active-directory-ds-getting-started-admingroup.md)
