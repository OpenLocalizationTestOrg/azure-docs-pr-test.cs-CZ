---
title: "aaaManage profilů Azure Traffic Manageru | Microsoft Docs"
description: "Tento článek vám pomůže vytvořit, zakázat, povolit nebo odstranit profil Azure Traffic Manageru."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f06e0365-0a20-4d08-b7e1-e56025e64f66
ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: kumud
ms.openlocfilehash: 0c6ab0c451581d039514a9de0b525b3937e45a85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-traffic-manager-profile"></a><span data-ttu-id="44c35-103">Správa profilu Azure Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="44c35-103">Manage an Azure Traffic Manager profile</span></span>

<span data-ttu-id="44c35-104">Profily Traffic Manageru pomocí směrování provozu metody toocontrol hello distribuce přenosů tooyour cloudové služby nebo koncových bodů webu.</span><span class="sxs-lookup"><span data-stu-id="44c35-104">Traffic Manager profiles use traffic-routing methods toocontrol hello distribution of traffic tooyour cloud services or website endpoints.</span></span> <span data-ttu-id="44c35-105">Tento článek vysvětluje, jak toocreate a správy těchto profilů.</span><span class="sxs-lookup"><span data-stu-id="44c35-105">This article explains how toocreate and manage these profiles.</span></span>

## <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="44c35-106">Vytvoření profilu Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="44c35-106">Create a Traffic Manager profile</span></span>

<span data-ttu-id="44c35-107">Profil Traffic Manageru můžete vytvořit pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="44c35-107">You can create a Traffic Manager profile by using hello Azure portal.</span></span> <span data-ttu-id="44c35-108">Po vytvoření profilu, můžete nakonfigurovat koncové body, monitorování a další nastavení v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="44c35-108">After creating your profile, you can configure endpoints, monitoring, and other settings in hello Azure portal.</span></span> <span data-ttu-id="44c35-109">Traffic Manager podporuje až too200 koncových bodů na jeden profil.</span><span class="sxs-lookup"><span data-stu-id="44c35-109">Traffic Manager supports up too200 endpoints per profile.</span></span> <span data-ttu-id="44c35-110">Většina scénářů použití však vyžaduje jen několik koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="44c35-110">However, most usage scenarios require only a few of endpoints.</span></span>

### <a name="toocreate-a-traffic-manager-profile"></a><span data-ttu-id="44c35-111">toocreate profil Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="44c35-111">toocreate a Traffic Manager profile</span></span>

1. <span data-ttu-id="44c35-112">V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="44c35-112">From a browser, sign in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="44c35-113">Pokud ještě účet nemáte, můžete si zaregistrovat [zkušební verzi na měsíc zdarma](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="44c35-113">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="44c35-114">Na hello **rozbočovače** nabídky, klikněte na tlačítko **nový** > **sítě** > **zobrazit všechny**, klikněte na tlačítko **provoz Správce** profil tooopen hello **profil služby Traffic Manager vytvořit** okno, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="44c35-114">On hello **Hub** menu, click **New** > **Networking** > **See all**, click **Traffic Manager** profile tooopen hello **Create Traffic Manager profile** blade, then click **Create**.</span></span>
3. <span data-ttu-id="44c35-115">Na hello **profil služby Traffic Manager vytvořit** okně dokončení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="44c35-115">On hello **Create Traffic Manager profile** blade, complete as follows:</span></span>
    1. <span data-ttu-id="44c35-116">Do pole **Název** zadejte název profilu.</span><span class="sxs-lookup"><span data-stu-id="44c35-116">In **Name**, provide a name for your profile.</span></span> <span data-ttu-id="44c35-117">Tento název musí toobe jedinečné v rámci zóny trafficmanager.net hello a výsledkem název DNS hello <name>, trafficmanager.net, který je použité tooaccess vašeho profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="44c35-117">This name needs toobe unique within hello trafficmanager.net zone and results in hello DNS name <name>, trafficmanager.net, that is used tooaccess your Traffic Manager profile.</span></span>
    2. <span data-ttu-id="44c35-118">V **metoda směrování**, vyberte hello **s prioritou** metody směrování.</span><span class="sxs-lookup"><span data-stu-id="44c35-118">In **Routing method**, select hello **Priority** routing method.</span></span>
    3. <span data-ttu-id="44c35-119">V **předplatné**, vyberte předplatné hello toocreate chcete tento profil v části</span><span class="sxs-lookup"><span data-stu-id="44c35-119">In **Subscription**, select hello subscription you want toocreate this profile under</span></span>
    4. <span data-ttu-id="44c35-120">V **skupiny prostředků**, vytvořit nové tooplace skupiny prostředků v tomto profilu.</span><span class="sxs-lookup"><span data-stu-id="44c35-120">In **Resource Group**, create a new resource group tooplace this profile under.</span></span>
    5. <span data-ttu-id="44c35-121">V **umístění skupiny prostředků**, vyberte umístění hello hello skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="44c35-121">In **Resource group location**, select hello location of hello resource group.</span></span> <span data-ttu-id="44c35-122">Toto nastavení odkazuje toohello umístění skupiny prostředků hello a nemá žádný vliv na hello profil Traffic Manageru, která bude nasazena globálně.</span><span class="sxs-lookup"><span data-stu-id="44c35-122">This setting refers toohello location of hello resource group, and has no impact on hello Traffic Manager profile that will be deployed globally.</span></span>
    6. <span data-ttu-id="44c35-123">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="44c35-123">Click **Create**.</span></span>
    7. <span data-ttu-id="44c35-124">Po dokončení nasazení globální hello vašeho profilu Traffic Manager je uveden ve skupině příslušných prostředků jako jeden z prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="44c35-124">When hello global deployment of your Traffic Manager profile is complete, it is listed in respective resource group as one of hello resources.</span></span>

## <a name="disable-enable-or-delete-a-profile"></a><span data-ttu-id="44c35-125">Zakázání, povolení nebo odstranění profilu</span><span class="sxs-lookup"><span data-stu-id="44c35-125">Disable, enable, or delete a profile</span></span>

<span data-ttu-id="44c35-126">Existující profil můžete zakázat tak, že Traffic Manager neodkazuje uživatelské požadavky toohello nakonfigurované koncové body.</span><span class="sxs-lookup"><span data-stu-id="44c35-126">You can disable an existing profile so that Traffic Manager does not refer user requests toohello configured endpoints.</span></span> <span data-ttu-id="44c35-127">Pokud profil Traffic Manageru zakážete, hello profil a hello informace obsažené v profilu hello zůstanou beze změn a lze ho upravovat v rozhraní Traffic Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="44c35-127">When you disable a Traffic Manager profile, hello profile and hello information contained in hello profile remain intact and can be edited in hello Traffic Manager interface.</span></span>  <span data-ttu-id="44c35-128">Referenční seznamy pokračovat, až bude znovu povolíte hello profilu.</span><span class="sxs-lookup"><span data-stu-id="44c35-128">Referrals resume when you re-enable hello profile.</span></span> <span data-ttu-id="44c35-129">Když vytvoříte profil Traffic Manageru v hello portálu Azure, je automaticky povolené.</span><span class="sxs-lookup"><span data-stu-id="44c35-129">When you create a Traffic Manager profile in hello Azure portal, it's automatically enabled.</span></span> <span data-ttu-id="44c35-130">Pokud se rozhodnete, že profil již nebude potřebný, můžete ho odstranit.</span><span class="sxs-lookup"><span data-stu-id="44c35-130">If you decide a profile is no longer necessary, you can delete it.</span></span>

### <a name="toodisable-a-profile"></a><span data-ttu-id="44c35-131">toodisable profil</span><span class="sxs-lookup"><span data-stu-id="44c35-131">toodisable a profile</span></span>

1. <span data-ttu-id="44c35-132">Pokud používáte vlastní název domény, změňte záznam CNAME hello na serveru DNS pro Internet tak, aby už odkazuje tooyour profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="44c35-132">If you are using a custom domain name, change hello CNAME record on your Internet DNS server so that it no longer points tooyour Traffic Manager profile.</span></span>
2. <span data-ttu-id="44c35-133">Zastaví provoz vrácení směrované toohello koncových bodů prostřednictvím nastavení profilu Traffic Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="44c35-133">Traffic stops being directed toohello endpoints through hello Traffic Manager profile settings.</span></span>
3. <span data-ttu-id="44c35-134">V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="44c35-134">From a browser, sign in toohello [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="44c35-135">V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** jméno, které chcete toomodify a pak klikněte na profil služby Traffic Manager hello v hello výsledky této hello zobrazí.</span><span class="sxs-lookup"><span data-stu-id="44c35-135">In hello portal’s search bar, search for hello **Traffic Manager profile** name that you want toomodify, and then click hello Traffic Manager profile in hello results that hello displayed.</span></span>
3. <span data-ttu-id="44c35-136">V hello **profil služby Traffic Manager** okně klikněte na tlačítko **přehled**, v okně Přehled hello klikněte na tlačítko **zakázat**a pak potvrďte profil služby Traffic Manager toodisable hello.</span><span class="sxs-lookup"><span data-stu-id="44c35-136">In hello **Traffic Manager profile** blade, click **Overview**, in hello Overview blade click **Disable**, and then confirm toodisable hello Traffic Manager profile.</span></span>

### <a name="tooenable-a-profile"></a><span data-ttu-id="44c35-137">tooenable profil</span><span class="sxs-lookup"><span data-stu-id="44c35-137">tooenable a profile</span></span>

1. <span data-ttu-id="44c35-138">V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="44c35-138">From a browser, sign in toohello [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="44c35-139">V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** jméno, které chcete toomodify a pak klikněte na profil služby Traffic Manager hello v hello výsledky této hello zobrazí.</span><span class="sxs-lookup"><span data-stu-id="44c35-139">In hello portal’s search bar, search for hello **Traffic Manager profile** name that you want toomodify, and then click hello Traffic Manager profile in hello results that hello displayed.</span></span>
3. <span data-ttu-id="44c35-140">V hello **profil služby Traffic Manager** okně klikněte na tlačítko **přehled**a potom v okně Přehled hello klikněte na tlačítko **povolit**.</span><span class="sxs-lookup"><span data-stu-id="44c35-140">In hello **Traffic Manager profile** blade, click **Overview**, and then in hello Overview blade click **Enable**.</span></span>
5. <span data-ttu-id="44c35-141">Pokud používáte vlastní název domény, vytvořte záznam prostředku CNAME na Internetu název serveru DNS toopoint toohello domény vašeho profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="44c35-141">If you are using a custom domain name, create a CNAME resource record on your Internet DNS server toopoint toohello domain name of your Traffic Manager profile.</span></span>
6. <span data-ttu-id="44c35-142">Přenosy jsou koncové body směrovanou toohello znovu.</span><span class="sxs-lookup"><span data-stu-id="44c35-142">Traffic is directed toohello endpoints again.</span></span>

### <a name="toodelete-a-profile"></a><span data-ttu-id="44c35-143">toodelete profil</span><span class="sxs-lookup"><span data-stu-id="44c35-143">toodelete a profile</span></span>

1. <span data-ttu-id="44c35-144">Ujistěte se, že hello záznam prostředku DNS na serveru DNS pro Internet již nepoužívá záznam prostředku CNAME, který odkazuje toohello název domény vašeho profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="44c35-144">Ensure that hello DNS resource record on your Internet DNS server no longer uses a CNAME resource record that points toohello domain name of your Traffic Manager profile.</span></span>
2. <span data-ttu-id="44c35-145">V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** jméno, které chcete toomodify a pak klikněte na profil služby Traffic Manager hello v hello výsledky této hello zobrazí.</span><span class="sxs-lookup"><span data-stu-id="44c35-145">In hello portal’s search bar, search for hello **Traffic Manager profile** name that you want toomodify, and then click hello Traffic Manager profile in hello results that hello displayed.</span></span>
3. <span data-ttu-id="44c35-146">V hello **profil služby Traffic Manager** okně klikněte na tlačítko **přehled**, v okně Přehled hello klikněte na tlačítko **odstranit**a pak potvrďte profil služby Traffic Manager toodelete hello.</span><span class="sxs-lookup"><span data-stu-id="44c35-146">In hello **Traffic Manager profile** blade, click **Overview**, in hello Overview blade click **Delete**, and then confirm toodelete hello Traffic Manager profile.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44c35-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44c35-147">Next steps</span></span>

* [<span data-ttu-id="44c35-148">Přidání koncového bodu</span><span class="sxs-lookup"><span data-stu-id="44c35-148">Add an endpoint</span></span>](traffic-manager-endpoints.md)
* [<span data-ttu-id="44c35-149">Konfigurace metody prioritního směrování</span><span class="sxs-lookup"><span data-stu-id="44c35-149">Configure Priority routing method</span></span>](traffic-manager-configure-priority-routing-method.md)
* [<span data-ttu-id="44c35-150">Konfigurace metody geografického směrování</span><span class="sxs-lookup"><span data-stu-id="44c35-150">Configure Geographic routing method</span></span>](traffic-manager-configure-geographic-routing-method.md) 
* [<span data-ttu-id="44c35-151">Konfigurace metody váženého směrování</span><span class="sxs-lookup"><span data-stu-id="44c35-151">Configure Weighted routing method</span></span>](traffic-manager-configure-weighted-routing-method.md)
* [<span data-ttu-id="44c35-152">Konfigurace metody směrování podle výkonu</span><span class="sxs-lookup"><span data-stu-id="44c35-152">Configure Performance routing method</span></span>](traffic-manager-configure-performance-routing-method.md)