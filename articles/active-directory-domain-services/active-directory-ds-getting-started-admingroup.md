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
ms.openlocfilehash: f87bcf33d3b1eb21c7d84814e4c4086f664e293d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a><span data-ttu-id="56af5-103">Povolit Azure Active Directory Domain Services pomocí portálu Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="56af5-103">Enable Azure Active Directory Domain Services using the Azure portal (Preview)</span></span>


## <a name="task-3-configure-administrative-group"></a><span data-ttu-id="56af5-104">Úloha 3: Konfigurace skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="56af5-104">Task 3: configure administrative group</span></span>
<span data-ttu-id="56af5-105">V této úloze konfigurace můžete vytvořit skupiny pro správu v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56af5-105">In this configuration task, you create an administrative group in your Azure AD directory.</span></span> <span data-ttu-id="56af5-106">Tuto speciální skupinu pro správu se nazývá *AAD řadič domény správci*.</span><span class="sxs-lookup"><span data-stu-id="56af5-106">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="56af5-107">Členové této skupiny mají oprávnění správce na počítačích, které jsou připojené k doméně k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="56af5-107">Members of this group are granted administrative permissions on machines that are domain-joined to the managed domain.</span></span> <span data-ttu-id="56af5-108">Na počítačích připojených k doméně je této skupiny přidat do skupiny administrators.</span><span class="sxs-lookup"><span data-stu-id="56af5-108">On domain-joined machines, this group is added to the administrators group.</span></span> <span data-ttu-id="56af5-109">Členové této skupiny navíc můžete použít vzdálené plochy se vzdáleně připojit k doméně počítače.</span><span class="sxs-lookup"><span data-stu-id="56af5-109">Additionally, members of this group can use Remote Desktop to connect remotely to domain-joined machines.</span></span>

> [!NOTE]
> <span data-ttu-id="56af5-110">Nemáte oprávnění správce domény nebo správce podnikové sítě na spravované domény, který jste vytvořili pomocí Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="56af5-110">You do not have Domain Administrator or Enterprise Administrator permissions on the managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="56af5-111">Na spravovaných domén tato oprávnění jsou vyhrazené pomocí služby a nejsou k dispozici uživatelům v rámci klienta.</span><span class="sxs-lookup"><span data-stu-id="56af5-111">On managed domains, these permissions are reserved by the service and are not made available to users within the tenant.</span></span> <span data-ttu-id="56af5-112">Ale můžete vytvořit v této úloze konfigurace speciální skupinu pro správu provádět některé privilegované operace.</span><span class="sxs-lookup"><span data-stu-id="56af5-112">However, you can use the special administrative group created in this configuration task to perform some privileged operations.</span></span> <span data-ttu-id="56af5-113">Tyto operace zahrnují připojení počítače k doméně, které patří do skupiny správy na počítačích připojených k doméně a konfigurace zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="56af5-113">These operations include joining computers to the domain, belonging to the administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="56af5-114">Průvodce automaticky vytvoří skupinu pro správu v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56af5-114">The wizard automatically creates the administrative group in your Azure AD directory.</span></span> <span data-ttu-id="56af5-115">Tato skupina se nazývá 'AAD řadič domény správci'.</span><span class="sxs-lookup"><span data-stu-id="56af5-115">This group is called 'AAD DC Administrators'.</span></span> <span data-ttu-id="56af5-116">Pokud máte existující skupinu s tímto názvem v adresáři služby Azure AD, Průvodce vybírá této skupiny.</span><span class="sxs-lookup"><span data-stu-id="56af5-116">If you have an existing group with this name in your Azure AD directory, the wizard selects this group.</span></span> <span data-ttu-id="56af5-117">Můžete nakonfigurovat skupinu členství pomocí **skupiny správců** stránce průvodce.</span><span class="sxs-lookup"><span data-stu-id="56af5-117">You can configure group membership using the **Administrator group** wizard page.</span></span>

1. <span data-ttu-id="56af5-118">Chcete-li nakonfigurovat členství ve skupině, klikněte na tlačítko **AAD řadič domény správci**.</span><span class="sxs-lookup"><span data-stu-id="56af5-118">To configure group membership, click **AAD DC Administrators**.</span></span>

    ![Nakonfigurujte členství ve skupině](./media/getting-started/domain-services-blade-admingroup.png)

2. <span data-ttu-id="56af5-120">Klikněte **přidat členy** tlačítko Přidat uživatele z adresáře služby Azure AD do skupiny Administrators.</span><span class="sxs-lookup"><span data-stu-id="56af5-120">Click the **Add members** button to add users from your Azure AD directory to the administrator group.</span></span>

3. <span data-ttu-id="56af5-121">Až budete hotovi, klikněte na tlačítko **OK** přesunout na **Souhrn** stránce průvodce.</span><span class="sxs-lookup"><span data-stu-id="56af5-121">When you are done, click **OK** to move on to the **Summary** page of the wizard.</span></span>

4. <span data-ttu-id="56af5-122">Na **Souhrn** stránky v průvodci zkontrolujte nastavení konfigurace pro spravované domény.</span><span class="sxs-lookup"><span data-stu-id="56af5-122">On the **Summary** page of the wizard, review the configuration settings for the managed domain.</span></span> <span data-ttu-id="56af5-123">Můžete přejít zpět do jakéhokoli kroku průvodce provést změny, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="56af5-123">You can go back to any step of the wizard to make changes, if necessary.</span></span> <span data-ttu-id="56af5-124">Až budete hotovi, klikněte na tlačítko **OK** k vytvoření nové spravované domény.</span><span class="sxs-lookup"><span data-stu-id="56af5-124">When you are done, click **OK** to create the new managed domain.</span></span>

    ![Souhrn](./media/getting-started/domain-services-blade-summary.png)

5. <span data-ttu-id="56af5-126">Zobrazí oznámení, že zobrazuje průběh nasazení služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="56af5-126">You see a notification that shows the progress of your Azure AD Domain Services deployment.</span></span> <span data-ttu-id="56af5-127">Kliknutím na oznámení najdete podrobný průběh nasazení.</span><span class="sxs-lookup"><span data-stu-id="56af5-127">Click the notification to see detailed progress for the deployment.</span></span>

    ![Oznámení – v průběhu nasazení](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="provision-your-managed-domain"></a><span data-ttu-id="56af5-129">Zřídit vaší spravované domény</span><span class="sxs-lookup"><span data-stu-id="56af5-129">Provision your managed domain</span></span>
<span data-ttu-id="56af5-130">Proces zřizování vaší spravované domény může trvat až jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="56af5-130">The process of provisioning your managed domain can take up to an hour.</span></span>

1. <span data-ttu-id="56af5-131">Během nasazení, můžete vyhledat 'domain services, na **vyhledávání prostředků** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="56af5-131">While your deployment is in progress, you can search for 'domain services' in the **Search resources** search box.</span></span> <span data-ttu-id="56af5-132">Vyberte **Azure AD Domain Services** z výsledku hledání.</span><span class="sxs-lookup"><span data-stu-id="56af5-132">Select **Azure AD Domain Services** from the search result.</span></span> <span data-ttu-id="56af5-133">**Azure AD Domain Services** okno uvádí spravované domény, který se připravuje.</span><span class="sxs-lookup"><span data-stu-id="56af5-133">The **Azure AD Domain Services** blade lists the managed domain that is being provisioned.</span></span>

    ![Najít spravované doméně, se zřídí](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="56af5-135">Klikněte na název spravované domény (například "contoso100.com") můžete zjistit podrobnosti o doméně.</span><span class="sxs-lookup"><span data-stu-id="56af5-135">Click the name of the managed domain (for example, 'contoso100.com') to see more details about the domain.</span></span>

    ![Doménových služeb – Stav zřizování](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="56af5-137">**Přehled** kartě se zobrazují, doménu se právě zřizuje.</span><span class="sxs-lookup"><span data-stu-id="56af5-137">The **Overview** tab shows that the domain is currently being provisioned.</span></span> <span data-ttu-id="56af5-138">Spravované doméně nelze nakonfigurovat, dokud je plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="56af5-138">You cannot configure the managed domain until it is fully provisioned.</span></span> <span data-ttu-id="56af5-139">To může trvat až jednu hodinu vaší spravované domény kompletní zřízení.</span><span class="sxs-lookup"><span data-stu-id="56af5-139">It may take up to an hour for your managed domain to be fully provisioned.</span></span>

    ![<span data-ttu-id="56af5-140">Doménových služeb – karta Přehled během Stav zřizování</span><span class="sxs-lookup"><span data-stu-id="56af5-140">Domain Services - Overview tab during the provisioning state</span></span> ](./media/getting-started/domain-services-provisioning-state-details.png)

4. <span data-ttu-id="56af5-141">Pokud spravované doméně plně zřízený, **přehled** kartě se zobrazují stav domény jako **systémem**.</span><span class="sxs-lookup"><span data-stu-id="56af5-141">When the managed domain is fully provisioned, the **Overview** tab shows the domain status as **Running**.</span></span>

    ![Domain Services – Karta Přehled po úplném zřízení](./media/getting-started/domain-services-provisioned.png)

5. <span data-ttu-id="56af5-143">Na **vlastnosti** kartě uvidíte dvě IP adresy, které jsou k dispozici pro virtuální síť řadiče.</span><span class="sxs-lookup"><span data-stu-id="56af5-143">On the **Properties** tab, you see two IP addresses at which domain controllers are available for the virtual network.</span></span>

    ![Doménových služeb – karta Vlastnosti po plně zřízený.](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="next-step"></a><span data-ttu-id="56af5-145">Další krok</span><span class="sxs-lookup"><span data-stu-id="56af5-145">Next step</span></span>
[<span data-ttu-id="56af5-146">Úloha 4: Aktualizace nastavení DNS pro virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="56af5-146">Task 4: update the DNS settings for the Azure virtual network</span></span>](active-directory-ds-getting-started-dns.md)
