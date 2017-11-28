---
title: "aaaNamed umístění v Azure Active Directory | Microsoft Docs"
description: "Konfigurace s názvem umístění, nemusíte mít IP adresy, které jsou vlastněny organizaci generovat falešně pozitivních pro umístění tooatypical Neuskutečnitelná cesta hello riziko typ události."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 591e4b94b2ec9d45e20c01711e922f9972e047e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="named-locations-in-azure-active-directory"></a><span data-ttu-id="429ee-103">Pojmenované umístění v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="429ee-103">Named locations in Azure Active Directory</span></span>

<span data-ttu-id="429ee-104">S hello s názvem umístění funkce služby Azure Active Directory můžete označit důvěryhodné rozsahy IP adres ve vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="429ee-104">With hello named locations feature of Azure Active Directory, you can label trusted IP address ranges in your organizations.</span></span> <span data-ttu-id="429ee-105">Ve vašem prostředí, můžete použít s názvem umístění v kontextu hello hello zjišťování [rizik události](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="429ee-105">In your environment, you can use named locations in hello context of hello detection of [risk events](active-directory-reporting-risk-events.md).</span></span> <span data-ttu-id="429ee-106">Funkce Hello pomáhá snižovat hello počet hlášených falešně pozitivních pro hello *Neuskutečnitelná cesta tooatypical umístění* rizik typ události.</span><span class="sxs-lookup"><span data-stu-id="429ee-106">hello feature helps reduce hello number of reported false positives for hello *Impossible travel tooatypical locations* risk event type.</span></span> 

## <a name="configuration"></a><span data-ttu-id="429ee-107">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="429ee-107">Configuration</span></span>

<span data-ttu-id="429ee-108">tooconfigure s názvem umístění:</span><span class="sxs-lookup"><span data-stu-id="429ee-108">tooconfigure a named location:</span></span>

1. <span data-ttu-id="429ee-109">Přihlaste se toohello [portál Azure](https://portal.azure.com) jako globální správce.</span><span class="sxs-lookup"><span data-stu-id="429ee-109">Sign in toohello [Azure portal](https://portal.azure.com) as global administrator.</span></span>

2. <span data-ttu-id="429ee-110">V levém podokně hello, klikněte na **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="429ee-110">In hello left pane, click **Azure Active Directory**.</span></span>

    ![v levém podokně hello Hello odkaz Azure Active Directory](./media/active-directory-named-locations/01.png)

3. <span data-ttu-id="429ee-112">Na hello **Azure Active Directory** okno, v hello **zabezpečení** klikněte na tlačítko **podmíněného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="429ee-112">On hello **Azure Active Directory** blade, in hello **Security** section, click **Conditional access**.</span></span>

    ![Hello příkaz podmíněného přístupu](./media/active-directory-named-locations/05.png)


4. <span data-ttu-id="429ee-114">Na hello **podmíněného přístupu** okno, v hello **spravovat** klikněte na tlačítko **s názvem umístění**.</span><span class="sxs-lookup"><span data-stu-id="429ee-114">On hello **Conditional Access** blade, in hello **Manage** section, click **Named locations**.</span></span>

    ![Hello pojmenované umístění příkaz](./media/active-directory-named-locations/06.png)


5. <span data-ttu-id="429ee-116">Na hello **s názvem umístění** okně klikněte na tlačítko **nové umístění**.</span><span class="sxs-lookup"><span data-stu-id="429ee-116">On hello **Named locations** blade, click **New location**.</span></span>

    ![Hello nový příkaz umístění](./media/active-directory-named-locations/07.png)


6. <span data-ttu-id="429ee-118">Na hello **nový** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="429ee-118">On hello **New** blade, do hello following:</span></span>

    ![Hello nové okno](./media/active-directory-named-locations/08.png)

    <span data-ttu-id="429ee-120">a.</span><span class="sxs-lookup"><span data-stu-id="429ee-120">a.</span></span> <span data-ttu-id="429ee-121">V hello **název** zadejte název pro vaše s názvem umístění.</span><span class="sxs-lookup"><span data-stu-id="429ee-121">In hello **Name** box, type a name for your named location.</span></span>

    <span data-ttu-id="429ee-122">b.</span><span class="sxs-lookup"><span data-stu-id="429ee-122">b.</span></span> <span data-ttu-id="429ee-123">V hello **rozsahy IP adres** zadejte rozsah adres IP.</span><span class="sxs-lookup"><span data-stu-id="429ee-123">In hello **IP ranges** box, type an IP range.</span></span> <span data-ttu-id="429ee-124">rozsah IP Hello musí toobe v hello *směrování mezi doménami* formátu (CIDR).</span><span class="sxs-lookup"><span data-stu-id="429ee-124">hello IP range needs toobe in hello *Classless Inter-Domain Routing* (CIDR) format.</span></span>  

    <span data-ttu-id="429ee-125">c.</span><span class="sxs-lookup"><span data-stu-id="429ee-125">c.</span></span> <span data-ttu-id="429ee-126">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="429ee-126">Click **Create**.</span></span>



## <a name="what-you-should-know"></a><span data-ttu-id="429ee-127">Důležité informace</span><span class="sxs-lookup"><span data-stu-id="429ee-127">What you should know</span></span>

<span data-ttu-id="429ee-128">**Hromadné aktualizace**: při vytváření nebo aktualizaci s názvem umístění pro hromadné aktualizace, můžete nahrát nebo stáhnout soubor CSV s rozsahy IP hello.</span><span class="sxs-lookup"><span data-stu-id="429ee-128">**Bulk updates**: When you create or update named locations, for bulk updates, you can upload or download a CSV file with hello IP ranges.</span></span> <span data-ttu-id="429ee-129">Nahrávaný přidá hello rozsahy IP adres v seznamu toohello hello souboru místo přepsání hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="429ee-129">An upload adds hello IP ranges in hello file toohello list instead of overwriting hello list.</span></span>

![Hello nahrávání a stahování odkazy](./media/active-directory-named-locations/09.png)


<span data-ttu-id="429ee-131">**Omezení**: můžete definovat nesmí být delší než 60 s názvem umístění, s jeden rozsah přiřazené tooeach IP z nich.</span><span class="sxs-lookup"><span data-stu-id="429ee-131">**Limitations**: You can define a maximum of 60 named locations, with one IP range assigned tooeach of them.</span></span> <span data-ttu-id="429ee-132">Pokud máte pouze jednu s názvem umístění nakonfigurovaný, můžete definovat až too500 rozsahy IP adres pro ni.</span><span class="sxs-lookup"><span data-stu-id="429ee-132">If you have just one named location configured, you can define up too500 IP ranges for it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="429ee-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="429ee-133">Next steps</span></span>

<span data-ttu-id="429ee-134">toolearn Další informace o rizikových událostech, najdete v části [Azure Active Directory rizikových událostech](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="429ee-134">toolearn more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>

