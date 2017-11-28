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
ms.openlocfilehash: 8bde872a13bc9960d1e62c74017ff78a8953a0a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a><span data-ttu-id="da897-103">Povolit Azure Active Directory Domain Services pomocí hello portálu Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="da897-103">Enable Azure Active Directory Domain Services using hello Azure portal (Preview)</span></span>


## <a name="task-3-configure-administrative-group"></a><span data-ttu-id="da897-104">Úloha 3: Konfigurace skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="da897-104">Task 3: configure administrative group</span></span>
<span data-ttu-id="da897-105">V této úloze konfigurace můžete vytvořit skupiny pro správu v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="da897-105">In this configuration task, you create an administrative group in your Azure AD directory.</span></span> <span data-ttu-id="da897-106">Tuto speciální skupinu pro správu se nazývá *AAD řadič domény správci*.</span><span class="sxs-lookup"><span data-stu-id="da897-106">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="da897-107">Členové této skupiny mají oprávnění správce na počítačích, které jsou připojené k doméně toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="da897-107">Members of this group are granted administrative permissions on machines that are domain-joined toohello managed domain.</span></span> <span data-ttu-id="da897-108">Na počítačích připojených k doméně je této skupiny přidat toohello skupiny administrators.</span><span class="sxs-lookup"><span data-stu-id="da897-108">On domain-joined machines, this group is added toohello administrators group.</span></span> <span data-ttu-id="da897-109">Členové této skupiny můžou navíc použít počítače vzdáleně připojené k toodomain tooconnect vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="da897-109">Additionally, members of this group can use Remote Desktop tooconnect remotely toodomain-joined machines.</span></span>

> [!NOTE]
> <span data-ttu-id="da897-110">Nemáte oprávnění správce domény nebo správce podnikové sítě na hello spravované domény, který jste vytvořili pomocí Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="da897-110">You do not have Domain Administrator or Enterprise Administrator permissions on hello managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="da897-111">Na spravovaných domén tato oprávnění jsou vyhrazené hello služby a nejsou k dispozici toousers v rámci klienta hello provedeny.</span><span class="sxs-lookup"><span data-stu-id="da897-111">On managed domains, these permissions are reserved by hello service and are not made available toousers within hello tenant.</span></span> <span data-ttu-id="da897-112">Můžete však použít hello speciální skupinu pro správu vytvořit v této konfigurace úloh tooperform některé privilegované operace.</span><span class="sxs-lookup"><span data-stu-id="da897-112">However, you can use hello special administrative group created in this configuration task tooperform some privileged operations.</span></span> <span data-ttu-id="da897-113">Tyto operace zahrnují připojení počítače toohello doméně, patřící toohello skupiny správy na počítačích připojených k doméně a konfigurace zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="da897-113">These operations include joining computers toohello domain, belonging toohello administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="da897-114">Skupina pro správu hello Hello průvodce automaticky vytvoří v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="da897-114">hello wizard automatically creates hello administrative group in your Azure AD directory.</span></span> <span data-ttu-id="da897-115">Tato skupina se nazývá 'AAD řadič domény správci'.</span><span class="sxs-lookup"><span data-stu-id="da897-115">This group is called 'AAD DC Administrators'.</span></span> <span data-ttu-id="da897-116">Pokud máte existující skupinu s tímto názvem v adresáři služby Azure AD, Průvodce hello vybírá této skupiny.</span><span class="sxs-lookup"><span data-stu-id="da897-116">If you have an existing group with this name in your Azure AD directory, hello wizard selects this group.</span></span> <span data-ttu-id="da897-117">Můžete nakonfigurovat pomocí hello členství ve skupině **skupiny správců** stránce průvodce.</span><span class="sxs-lookup"><span data-stu-id="da897-117">You can configure group membership using hello **Administrator group** wizard page.</span></span>

1. <span data-ttu-id="da897-118">členství ve skupině tooconfigure, klikněte na tlačítko **AAD řadič domény správci**.</span><span class="sxs-lookup"><span data-stu-id="da897-118">tooconfigure group membership, click **AAD DC Administrators**.</span></span>

    ![Nakonfigurujte členství ve skupině](./media/getting-started/domain-services-blade-admingroup.png)

2. <span data-ttu-id="da897-120">Klikněte na tlačítko hello **přidat členy** tlačítko tooadd uživatelé ze skupiny správce toohello adresář Azure AD.</span><span class="sxs-lookup"><span data-stu-id="da897-120">Click hello **Add members** button tooadd users from your Azure AD directory toohello administrator group.</span></span>

3. <span data-ttu-id="da897-121">Až budete hotovi, klikněte na tlačítko **OK** toomove na toohello **Souhrn** hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="da897-121">When you are done, click **OK** toomove on toohello **Summary** page of hello wizard.</span></span>

4. <span data-ttu-id="da897-122">Na hello **Souhrn** hello průvodce, zkontrolujte nastavení konfigurace hello hello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="da897-122">On hello **Summary** page of hello wizard, review hello configuration settings for hello managed domain.</span></span> <span data-ttu-id="da897-123">Krok tooany hello Průvodce toomake změn, můžete přejít zpět, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="da897-123">You can go back tooany step of hello wizard toomake changes, if necessary.</span></span> <span data-ttu-id="da897-124">Až budete hotovi, klikněte na tlačítko **OK** toocreate hello nové spravované domény.</span><span class="sxs-lookup"><span data-stu-id="da897-124">When you are done, click **OK** toocreate hello new managed domain.</span></span>

    ![Souhrn](./media/getting-started/domain-services-blade-summary.png)

5. <span data-ttu-id="da897-126">Zobrazí oznámení, že zobrazuje průběh hello vašeho nasazení služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="da897-126">You see a notification that shows hello progress of your Azure AD Domain Services deployment.</span></span> <span data-ttu-id="da897-127">Klikněte na tlačítko hello oznámení toosee podrobný průběh nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="da897-127">Click hello notification toosee detailed progress for hello deployment.</span></span>

    ![Oznámení – v průběhu nasazení](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="provision-your-managed-domain"></a><span data-ttu-id="da897-129">Zřídit vaší spravované domény</span><span class="sxs-lookup"><span data-stu-id="da897-129">Provision your managed domain</span></span>
<span data-ttu-id="da897-130">Hello proces zřizování vaší spravované domény může trvat až hodinu tooan.</span><span class="sxs-lookup"><span data-stu-id="da897-130">hello process of provisioning your managed domain can take up tooan hour.</span></span>

1. <span data-ttu-id="da897-131">Během nasazení, můžete vyhledat domain services v hello **vyhledávání prostředků** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="da897-131">While your deployment is in progress, you can search for 'domain services' in hello **Search resources** search box.</span></span> <span data-ttu-id="da897-132">Vyberte **Azure AD Domain Services** z výsledek hledání hello.</span><span class="sxs-lookup"><span data-stu-id="da897-132">Select **Azure AD Domain Services** from hello search result.</span></span> <span data-ttu-id="da897-133">Hello **Azure AD Domain Services** okno uvádí hello spravované domény, který se připravuje.</span><span class="sxs-lookup"><span data-stu-id="da897-133">hello **Azure AD Domain Services** blade lists hello managed domain that is being provisioned.</span></span>

    ![Najít spravované doméně, se zřídí](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="da897-135">Další podrobnosti o hello domény klikněte na tlačítko hello název toosee hello spravované domény (například "contoso100.com").</span><span class="sxs-lookup"><span data-stu-id="da897-135">Click hello name of hello managed domain (for example, 'contoso100.com') toosee more details about hello domain.</span></span>

    ![Doménových služeb – Stav zřizování](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="da897-137">Hello **přehled** kartě se zobrazují se právě zřizuje tuto doménu hello.</span><span class="sxs-lookup"><span data-stu-id="da897-137">hello **Overview** tab shows that hello domain is currently being provisioned.</span></span> <span data-ttu-id="da897-138">Spravované domény hello nelze nakonfigurovat, dokud je plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="da897-138">You cannot configure hello managed domain until it is fully provisioned.</span></span> <span data-ttu-id="da897-139">To může trvat až hodinu tooan vaší spravované domény toobe plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="da897-139">It may take up tooan hour for your managed domain toobe fully provisioned.</span></span>

    ![<span data-ttu-id="da897-140">Doménových služeb – karta Přehled během hello Stav zřizování</span><span class="sxs-lookup"><span data-stu-id="da897-140">Domain Services - Overview tab during hello provisioning state</span></span> ](./media/getting-started/domain-services-provisioning-state-details.png)

4. <span data-ttu-id="da897-141">Pokud spravované doméně hello plně zřízený, hello **přehled** kartě se zobrazují stav domén hello jako **systémem**.</span><span class="sxs-lookup"><span data-stu-id="da897-141">When hello managed domain is fully provisioned, hello **Overview** tab shows hello domain status as **Running**.</span></span>

    ![Domain Services – Karta Přehled po úplném zřízení](./media/getting-started/domain-services-provisioned.png)

5. <span data-ttu-id="da897-143">Na hello **vlastnosti** kartě uvidíte dvě IP adresy, které jsou k dispozici pro virtuální síť hello řadiče.</span><span class="sxs-lookup"><span data-stu-id="da897-143">On hello **Properties** tab, you see two IP addresses at which domain controllers are available for hello virtual network.</span></span>

    ![Doménových služeb – karta Vlastnosti po plně zřízený.](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="next-step"></a><span data-ttu-id="da897-145">Další krok</span><span class="sxs-lookup"><span data-stu-id="da897-145">Next step</span></span>
[<span data-ttu-id="da897-146">Úloha 4: aktualizace nastavení DNS hello hello virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="da897-146">Task 4: update hello DNS settings for hello Azure virtual network</span></span>](active-directory-ds-getting-started-dns.md)
