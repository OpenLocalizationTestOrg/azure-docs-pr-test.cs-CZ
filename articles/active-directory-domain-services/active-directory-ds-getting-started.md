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
ms.date: 08/28/2017
ms.author: maheshu
ms.openlocfilehash: 47507096a6245d4f1ba57a652ddf5167b3776ae9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a><span data-ttu-id="c65f2-103">Povolit Azure Active Directory Domain Services pomocí portálu Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="c65f2-103">Enable Azure Active Directory Domain Services using the Azure portal (Preview)</span></span>
<span data-ttu-id="c65f2-104">Tento článek ukazuje, jak povolit Azure Active Directory Domain Services (Azure AD DS) pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c65f2-104">This article shows how to enable Azure Active Directory Domain Services (Azure AD DS) using the Azure portal.</span></span>


<span data-ttu-id="c65f2-105">Ke spuštění **povolit Azure AD Domain Services** průvodce proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c65f2-105">To launch the **Enable Azure AD Domain Services** wizard, complete the following steps:</span></span>

1. <span data-ttu-id="c65f2-106">Přejděte na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c65f2-106">Go to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c65f2-107">V levém podokně klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="c65f2-107">In the left pane, click on **New**.</span></span>
3. <span data-ttu-id="c65f2-108">V **nový** okno, zadejte **Domain Services** do panelu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="c65f2-108">In the **New** blade, type **Domain Services** into the search bar.</span></span>

    ![Vyhledání služeb domény](./media/getting-started/search-domain-services.png)

4. <span data-ttu-id="c65f2-110">Kliknutím vyberte **Azure AD Domain Services** ze seznamu návrhy vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="c65f2-110">Click to select **Azure AD Domain Services** from the list of search suggestions.</span></span> <span data-ttu-id="c65f2-111">Na **Azure AD Domain Services** okně klikněte **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c65f2-111">On the **Azure AD Domain Services** blade, click the **Create** button.</span></span>

    ![Okno služby domény](./media/getting-started/domain-services-blade.png)

5. <span data-ttu-id="c65f2-113">**Povolit Azure AD Domain Services** se spustí průvodce.</span><span class="sxs-lookup"><span data-stu-id="c65f2-113">The **Enable Azure AD Domain Services** wizard is launched.</span></span>


## <a name="task-1-configure-basic-settings"></a><span data-ttu-id="c65f2-114">Úloha 1: nakonfigurovali základní nastavení</span><span class="sxs-lookup"><span data-stu-id="c65f2-114">Task 1: configure basic settings</span></span>
<span data-ttu-id="c65f2-115">V **Základy** stránku průvodce, můžete zadat název domény DNS pro spravovanou doménu.</span><span class="sxs-lookup"><span data-stu-id="c65f2-115">In the **Basics** page of the wizard, you can specify the DNS domain name for the managed domain.</span></span> <span data-ttu-id="c65f2-116">Můžete také zvolit skupinu prostředků a umístění Azure, ke které by měly být nasazeny spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="c65f2-116">You can also choose the resource group and Azure location to which the managed domain should be deployed.</span></span>

![Základní informace o konfiguraci](./media/getting-started/domain-services-blade-basics.png)

1. <span data-ttu-id="c65f2-118">Vyberte **název domény DNS** vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="c65f2-118">Choose the **DNS domain name** for your managed domain.</span></span>

   * <span data-ttu-id="c65f2-119">Výchozí název domény adresáře (s **. onmicrosoft.com** příponu) je zadána ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c65f2-119">The default domain name of the directory (with a **.onmicrosoft.com** suffix) is specified by default.</span></span>

   * <span data-ttu-id="c65f2-120">Můžete také zadat v názvu vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="c65f2-120">You can also type in a custom domain name.</span></span> <span data-ttu-id="c65f2-121">V tomto příkladu je použit vlastní název domény *contoso100.com*.</span><span class="sxs-lookup"><span data-stu-id="c65f2-121">In this example, the custom domain name is *contoso100.com*.</span></span>

     > [!WARNING]
     > <span data-ttu-id="c65f2-122">Předpona zadaného názvu domény (například *contoso100* v případě názvu domény *contoso100.com*) musí obsahovat nejvýše 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="c65f2-122">The prefix of your specified domain name (for example, *contoso100* in the *contoso100.com* domain name) must contain 15 or fewer characters.</span></span> <span data-ttu-id="c65f2-123">Nelze vytvořit spravované doméně s předponou delší než 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="c65f2-123">You cannot create a managed domain with a prefix longer than 15 characters.</span></span>
     >
     >

2. <span data-ttu-id="c65f2-124">Ujistěte se, že vámi zvolený název domény DNS pro spravovanou doménu ještě ve virtuální síti neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c65f2-124">Ensure that the DNS domain name you have chosen for the managed domain does not already exist in the virtual network.</span></span> <span data-ttu-id="c65f2-125">Konkrétně, zkontrolujte, zda:</span><span class="sxs-lookup"><span data-stu-id="c65f2-125">Specifically, check whether:</span></span>

   * <span data-ttu-id="c65f2-126">Ve virtuální síti již existuje doména se stejným názvem domény DNS.</span><span class="sxs-lookup"><span data-stu-id="c65f2-126">You already have a domain with the same DNS domain name on the virtual network.</span></span>

   * <span data-ttu-id="c65f2-127">Virtuální síť, kde budete chtít povolit spravované domény nemá připojení k síti VPN s vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="c65f2-127">The virtual network where you plan to enable the managed domain has a VPN connection with your on-premises network.</span></span> <span data-ttu-id="c65f2-128">V tomto scénáři Ujistěte se, že nemáte doménu se stejným názvem domény DNS ve vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="c65f2-128">In this scenario, ensure you don't have a domain with the same DNS domain name on your on-premises network.</span></span>

   * <span data-ttu-id="c65f2-129">Ve virtuální síti existuje cloudová služba s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="c65f2-129">You have an existing cloud service with that name on the virtual network.</span></span>

3. <span data-ttu-id="c65f2-130">Vyberte **typ virtuální sítě**.</span><span class="sxs-lookup"><span data-stu-id="c65f2-130">Choose the **type of virtual network**.</span></span> <span data-ttu-id="c65f2-131">Ve výchozím nastavení **Resource Manager** je vybrán virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c65f2-131">By default, the **Resource Manager** virtual network type is selected.</span></span> <span data-ttu-id="c65f2-132">Doporučujeme použít tento typ virtuální sítě pro všechny nově vytvořený spravované domény.</span><span class="sxs-lookup"><span data-stu-id="c65f2-132">We recommend using this type of virtual network for all newly created managed domains.</span></span>

4. <span data-ttu-id="c65f2-133">Vyberte Azure **předplatné** ve kterém chcete vytvořit spravované domény.</span><span class="sxs-lookup"><span data-stu-id="c65f2-133">Select the Azure **Subscription** in which you would like to create the managed domain.</span></span>

5. <span data-ttu-id="c65f2-134">Vyberte **skupiny prostředků** k spravované doméně by měly patřit.</span><span class="sxs-lookup"><span data-stu-id="c65f2-134">Select the **Resource group** to which the managed domain should belong.</span></span> <span data-ttu-id="c65f2-135">Můžete buď **vytvořit nový** nebo **použít existující** možnosti vybrat skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c65f2-135">You can choose either the **Create new** or **Use existing** options to select the resource group.</span></span>

6. <span data-ttu-id="c65f2-136">Zvolte Azure **umístění** ve které má být vytvořena spravované domény.</span><span class="sxs-lookup"><span data-stu-id="c65f2-136">Choose the Azure **Location** in which the managed domain should be created.</span></span> <span data-ttu-id="c65f2-137">Na **sítě** stránky v průvodci zobrazí pouze virtuální sítě, které patří do umístění, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="c65f2-137">On the **Network** page of the wizard, you see only virtual networks that belong to the location you have selected.</span></span>

7. <span data-ttu-id="c65f2-138">Až budete hotovi, klikněte na tlačítko **OK** přesunout na **sítě** stránce průvodce.</span><span class="sxs-lookup"><span data-stu-id="c65f2-138">When you are done, click **OK** to move on to the **Network** page of the wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="c65f2-139">Další krok</span><span class="sxs-lookup"><span data-stu-id="c65f2-139">Next step</span></span>
[<span data-ttu-id="c65f2-140">Úloha 2: Konfigurace nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="c65f2-140">Task 2: configure network settings</span></span>](active-directory-ds-getting-started-network.md)
