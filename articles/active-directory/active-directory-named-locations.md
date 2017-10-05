---
title: "Umístění ve službě Azure Active Directory s názvem | Microsoft Docs"
description: "Konfigurace s názvem umístění, nemusíte mít IP adresy, které jsou vlastněny organizaci generovat falešně pozitivních pro Impossible dostavit do netypických míst typ události riziko."
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
ms.openlocfilehash: ff31ded1d9d60e47e0ae5f01119de78cd7f2df38
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="named-locations-in-azure-active-directory"></a><span data-ttu-id="05fe8-103">Pojmenované umístění v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="05fe8-103">Named locations in Azure Active Directory</span></span>

<span data-ttu-id="05fe8-104">Pomocí funkce s názvem umístění služby Azure Active Directory můžete označit důvěryhodné rozsahy IP adres ve vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="05fe8-104">With the named locations feature of Azure Active Directory, you can label trusted IP address ranges in your organizations.</span></span> <span data-ttu-id="05fe8-105">Ve vašem prostředí, můžete použít s názvem umístění v kontextu detekce [rizik události](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="05fe8-105">In your environment, you can use named locations in the context of the detection of [risk events](active-directory-reporting-risk-events.md).</span></span> <span data-ttu-id="05fe8-106">Tato funkce pomáhá snížit počet hlášených falešně pozitivních pro *Impossible dostavit do netypických míst* rizik typ události.</span><span class="sxs-lookup"><span data-stu-id="05fe8-106">The feature helps reduce the number of reported false positives for the *Impossible travel to atypical locations* risk event type.</span></span> 

## <a name="configuration"></a><span data-ttu-id="05fe8-107">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="05fe8-107">Configuration</span></span>

<span data-ttu-id="05fe8-108">Konfigurace s názvem umístění:</span><span class="sxs-lookup"><span data-stu-id="05fe8-108">To configure a named location:</span></span>

1. <span data-ttu-id="05fe8-109">Přihlaste se k [portál Azure](https://portal.azure.com) jako globální správce.</span><span class="sxs-lookup"><span data-stu-id="05fe8-109">Sign in to the [Azure portal](https://portal.azure.com) as global administrator.</span></span>

2. <span data-ttu-id="05fe8-110">V levém podokně klikněte na **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="05fe8-110">In the left pane, click **Azure Active Directory**.</span></span>

    ![V levém podokně na odkaz Azure Active Directory](./media/active-directory-named-locations/01.png)

3. <span data-ttu-id="05fe8-112">Na **Azure Active Directory** okno v **zabezpečení** klikněte na tlačítko **podmíněného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="05fe8-112">On the **Azure Active Directory** blade, in the **Security** section, click **Conditional access**.</span></span>

    ![Příkaz podmíněného přístupu](./media/active-directory-named-locations/05.png)


4. <span data-ttu-id="05fe8-114">Na **podmíněného přístupu** okno v **spravovat** klikněte na tlačítko **s názvem umístění**.</span><span class="sxs-lookup"><span data-stu-id="05fe8-114">On the **Conditional Access** blade, in the **Manage** section, click **Named locations**.</span></span>

    ![Příkaz pojmenované umístění](./media/active-directory-named-locations/06.png)


5. <span data-ttu-id="05fe8-116">Na **s názvem umístění** okně klikněte na tlačítko **nové umístění**.</span><span class="sxs-lookup"><span data-stu-id="05fe8-116">On the **Named locations** blade, click **New location**.</span></span>

    ![Příkaz nové umístění](./media/active-directory-named-locations/07.png)


6. <span data-ttu-id="05fe8-118">Na **nový** okno, proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="05fe8-118">On the **New** blade, do the following:</span></span>

    ![Nové okno](./media/active-directory-named-locations/08.png)

    <span data-ttu-id="05fe8-120">a.</span><span class="sxs-lookup"><span data-stu-id="05fe8-120">a.</span></span> <span data-ttu-id="05fe8-121">V **název** zadejte název pro vaše s názvem umístění.</span><span class="sxs-lookup"><span data-stu-id="05fe8-121">In the **Name** box, type a name for your named location.</span></span>

    <span data-ttu-id="05fe8-122">b.</span><span class="sxs-lookup"><span data-stu-id="05fe8-122">b.</span></span> <span data-ttu-id="05fe8-123">V **rozsahy IP adres** zadejte rozsah adres IP.</span><span class="sxs-lookup"><span data-stu-id="05fe8-123">In the **IP ranges** box, type an IP range.</span></span> <span data-ttu-id="05fe8-124">Rozsah IP adres musí být ve *směrování mezi doménami* formátu (CIDR).</span><span class="sxs-lookup"><span data-stu-id="05fe8-124">The IP range needs to be in the *Classless Inter-Domain Routing* (CIDR) format.</span></span>  

    <span data-ttu-id="05fe8-125">c.</span><span class="sxs-lookup"><span data-stu-id="05fe8-125">c.</span></span> <span data-ttu-id="05fe8-126">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="05fe8-126">Click **Create**.</span></span>



## <a name="what-you-should-know"></a><span data-ttu-id="05fe8-127">Důležité informace</span><span class="sxs-lookup"><span data-stu-id="05fe8-127">What you should know</span></span>

<span data-ttu-id="05fe8-128">**Hromadné aktualizace**: při vytváření nebo aktualizaci s názvem umístění pro hromadné aktualizace, můžete nahrát nebo stáhnout soubor CSV s rozsahy IP.</span><span class="sxs-lookup"><span data-stu-id="05fe8-128">**Bulk updates**: When you create or update named locations, for bulk updates, you can upload or download a CSV file with the IP ranges.</span></span> <span data-ttu-id="05fe8-129">Nahrávaný přidá rozsahy IP v souboru do seznamu místo přepsání seznamu.</span><span class="sxs-lookup"><span data-stu-id="05fe8-129">An upload adds the IP ranges in the file to the list instead of overwriting the list.</span></span>

![Nahrávání a stahování odkazy](./media/active-directory-named-locations/09.png)


<span data-ttu-id="05fe8-131">**Omezení**: nesmí být delší než 60 s názvem umístění, můžete definovat s jeden rozsah IP adres přiřazené ke každému z nich.</span><span class="sxs-lookup"><span data-stu-id="05fe8-131">**Limitations**: You can define a maximum of 60 named locations, with one IP range assigned to each of them.</span></span> <span data-ttu-id="05fe8-132">Pokud máte pouze jednu s názvem umístění nakonfigurovaný, můžete definovat maximálně 500 rozsahy IP adres pro ni.</span><span class="sxs-lookup"><span data-stu-id="05fe8-132">If you have just one named location configured, you can define up to 500 IP ranges for it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="05fe8-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05fe8-133">Next steps</span></span>

<span data-ttu-id="05fe8-134">Další informace o rizikových událostech najdete v tématu [Azure Active Directory rizikových událostech](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="05fe8-134">To learn more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>

