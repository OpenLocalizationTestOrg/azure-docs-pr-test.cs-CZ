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
ms.date: 08/28/2017
ms.author: maheshu
ms.openlocfilehash: 79cbb21c4a50194f5ad8ca1a4a8493ee4a260a9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a><span data-ttu-id="66c33-103">Povolit Azure Active Directory Domain Services pomocí hello portálu Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="66c33-103">Enable Azure Active Directory Domain Services using hello Azure portal (Preview)</span></span>
<span data-ttu-id="66c33-104">Tento článek ukazuje, jak hello tooenable Azure Active Directory Domain Services (Azure AD DS) pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="66c33-104">This article shows how tooenable Azure Active Directory Domain Services (Azure AD DS) using hello Azure portal.</span></span>


<span data-ttu-id="66c33-105">toolaunch hello **povolit Azure AD Domain Services** průvodce, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="66c33-105">toolaunch hello **Enable Azure AD Domain Services** wizard, complete hello following steps:</span></span>

1. <span data-ttu-id="66c33-106">Přejděte toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="66c33-106">Go toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="66c33-107">V levém podokně hello, klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="66c33-107">In hello left pane, click on **New**.</span></span>
3. <span data-ttu-id="66c33-108">V hello **nový** okno, zadejte **Domain Services** do panelu Hledat hello.</span><span class="sxs-lookup"><span data-stu-id="66c33-108">In hello **New** blade, type **Domain Services** into hello search bar.</span></span>

    ![Vyhledání služeb domény](./media/getting-started/search-domain-services.png)

4. <span data-ttu-id="66c33-110">Klikněte na tlačítko tooselect **Azure AD Domain Services** ze seznamu hello návrhy vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="66c33-110">Click tooselect **Azure AD Domain Services** from hello list of search suggestions.</span></span> <span data-ttu-id="66c33-111">Na hello **Azure AD Domain Services** okně klikněte na tlačítko hello **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="66c33-111">On hello **Azure AD Domain Services** blade, click hello **Create** button.</span></span>

    ![Okno služby domény](./media/getting-started/domain-services-blade.png)

5. <span data-ttu-id="66c33-113">Hello **povolit Azure AD Domain Services** se spustí průvodce.</span><span class="sxs-lookup"><span data-stu-id="66c33-113">hello **Enable Azure AD Domain Services** wizard is launched.</span></span>


## <a name="task-1-configure-basic-settings"></a><span data-ttu-id="66c33-114">Úloha 1: nakonfigurovali základní nastavení</span><span class="sxs-lookup"><span data-stu-id="66c33-114">Task 1: configure basic settings</span></span>
<span data-ttu-id="66c33-115">V hello **Základy** stránku hello průvodce, můžete zadat název domény DNS hello hello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="66c33-115">In hello **Basics** page of hello wizard, you can specify hello DNS domain name for hello managed domain.</span></span> <span data-ttu-id="66c33-116">Můžete také vybrat skupinu prostředků hello a umístění Azure toowhich hello spravované domény by měl být nasazena.</span><span class="sxs-lookup"><span data-stu-id="66c33-116">You can also choose hello resource group and Azure location toowhich hello managed domain should be deployed.</span></span>

![Základní informace o konfiguraci](./media/getting-started/domain-services-blade-basics.png)

1. <span data-ttu-id="66c33-118">Zvolte hello **název domény DNS** vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="66c33-118">Choose hello **DNS domain name** for your managed domain.</span></span>

   * <span data-ttu-id="66c33-119">Hello výchozí název domény hello adresáře (s **. onmicrosoft.com** příponu) je zadána ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="66c33-119">hello default domain name of hello directory (with a **.onmicrosoft.com** suffix) is specified by default.</span></span>

   * <span data-ttu-id="66c33-120">Můžete také zadat v názvu vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="66c33-120">You can also type in a custom domain name.</span></span> <span data-ttu-id="66c33-121">V tomto příkladu je název vlastní domény hello *contoso100.com*.</span><span class="sxs-lookup"><span data-stu-id="66c33-121">In this example, hello custom domain name is *contoso100.com*.</span></span>

     > [!WARNING]
     > <span data-ttu-id="66c33-122">Předpona Hello vaší zadaného názvu domény (například *contoso100* v hello *contoso100.com* název domény) musí obsahovat nejvýše 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="66c33-122">hello prefix of your specified domain name (for example, *contoso100* in hello *contoso100.com* domain name) must contain 15 or fewer characters.</span></span> <span data-ttu-id="66c33-123">Nelze vytvořit spravované doméně s předponou delší než 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="66c33-123">You cannot create a managed domain with a prefix longer than 15 characters.</span></span>
     >
     >

2. <span data-ttu-id="66c33-124">Zkontrolujte název domény DNS hello, které jste vybrali pro hello spravované domény ve virtuální síti hello již neexistuje.</span><span class="sxs-lookup"><span data-stu-id="66c33-124">Ensure that hello DNS domain name you have chosen for hello managed domain does not already exist in hello virtual network.</span></span> <span data-ttu-id="66c33-125">Konkrétně, zkontrolujte, zda:</span><span class="sxs-lookup"><span data-stu-id="66c33-125">Specifically, check whether:</span></span>

   * <span data-ttu-id="66c33-126">Už máte doménu hello stejným názvem domény DNS ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="66c33-126">You already have a domain with hello same DNS domain name on hello virtual network.</span></span>

   * <span data-ttu-id="66c33-127">Hello virtuální síť, kde plánujete tooenable hello spravované domény nemá připojení k síti VPN s vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="66c33-127">hello virtual network where you plan tooenable hello managed domain has a VPN connection with your on-premises network.</span></span> <span data-ttu-id="66c33-128">V tomto scénáři, ujistěte se, nemáte doménu hello stejným názvem domény DNS ve vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="66c33-128">In this scenario, ensure you don't have a domain with hello same DNS domain name on your on-premises network.</span></span>

   * <span data-ttu-id="66c33-129">Máte stávající cloudovou službu s tímto názvem ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="66c33-129">You have an existing cloud service with that name on hello virtual network.</span></span>

3. <span data-ttu-id="66c33-130">Zvolte hello **typ virtuální sítě**.</span><span class="sxs-lookup"><span data-stu-id="66c33-130">Choose hello **type of virtual network**.</span></span> <span data-ttu-id="66c33-131">Ve výchozím nastavení, hello **Resource Manager** je vybrán virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="66c33-131">By default, hello **Resource Manager** virtual network type is selected.</span></span> <span data-ttu-id="66c33-132">Doporučujeme použít tento typ virtuální sítě pro všechny nově vytvořený spravované domény.</span><span class="sxs-lookup"><span data-stu-id="66c33-132">We recommend using this type of virtual network for all newly created managed domains.</span></span>

4. <span data-ttu-id="66c33-133">Vyberte hello Azure **předplatné** ve kterém chcete toocreate hello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="66c33-133">Select hello Azure **Subscription** in which you would like toocreate hello managed domain.</span></span>

5. <span data-ttu-id="66c33-134">Vyberte hello **skupiny prostředků** by měly patřit toowhich hello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="66c33-134">Select hello **Resource group** toowhich hello managed domain should belong.</span></span> <span data-ttu-id="66c33-135">Můžete buď hello **vytvořit nový** nebo **použít existující** skupiny prostředků hello tooselect možnosti.</span><span class="sxs-lookup"><span data-stu-id="66c33-135">You can choose either hello **Create new** or **Use existing** options tooselect hello resource group.</span></span>

6. <span data-ttu-id="66c33-136">Zvolte hello Azure **umístění** v které hello je potřeba vytvořit spravované domény.</span><span class="sxs-lookup"><span data-stu-id="66c33-136">Choose hello Azure **Location** in which hello managed domain should be created.</span></span> <span data-ttu-id="66c33-137">Na hello **sítě** stránku hello Průvodce zobrazí pouze virtuální sítě, které patří toohello umístění, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="66c33-137">On hello **Network** page of hello wizard, you see only virtual networks that belong toohello location you have selected.</span></span>

7. <span data-ttu-id="66c33-138">Až budete hotovi, klikněte na tlačítko **OK** toomove na toohello **sítě** hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="66c33-138">When you are done, click **OK** toomove on toohello **Network** page of hello wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="66c33-139">Další krok</span><span class="sxs-lookup"><span data-stu-id="66c33-139">Next step</span></span>
[<span data-ttu-id="66c33-140">Úloha 2: Konfigurace nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="66c33-140">Task 2: configure network settings</span></span>](active-directory-ds-getting-started-network.md)
