---
title: "Azure Active Directory Domain Services: Povolení služby Azure Active Directory Domain Services | Dokumentace Microsoftu"
description: "Povolit Azure Active Directory Domain Services pomocí hello portál Azure classic"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.openlocfilehash: 6263eb1849808a7c85e572e1046bc9039362dd9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a><span data-ttu-id="e237e-103">Povolit Azure Active Directory Domain Services pomocí hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="e237e-103">Enable Azure Active Directory Domain Services using hello Azure classic portal</span></span>

## <a name="task-3-enable-azure-active-directory-domain-services"></a><span data-ttu-id="e237e-104">Úloha 3: Povolení služby Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="e237e-104">Task 3: enable Azure Active Directory Domain Services</span></span>
<span data-ttu-id="e237e-105">V této úloze povolíte Azure Active Directory Domain Services (Azure AD DS) pro váš adresář díky hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e237e-105">In this task, you enable Azure Active Directory Domain Services (Azure AD DS) for your directory by doing hello following steps:</span></span>

1. <span data-ttu-id="e237e-106">Přejděte toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="e237e-106">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="e237e-107">V levém podokně hello vyberte hello **služby Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e237e-107">In hello left pane, select hello **Active Directory** button.</span></span>
3. <span data-ttu-id="e237e-108">Vyberte klienta Azure Active Directory (Azure AD) hello (adresář) pro které chcete tooenable služby Azure AD DS.</span><span class="sxs-lookup"><span data-stu-id="e237e-108">Select hello Azure Active Directory (Azure AD) tenant (directory) for which you want tooenable Azure AD DS.</span></span>

    ![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="e237e-110">Na hello **preview directory** klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="e237e-110">On hello **preview directory** page, click hello **Configure** tab.</span></span>

    ![Karta konfigurace adresáře](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. <span data-ttu-id="e237e-112">V části **služby domény**, změňte hello **povolit doménové služby pro tento adresář** možnost příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="e237e-112">Under **domain services**, change hello **Enable domain services for this directory** option too**Yes**.</span></span>  
    <span data-ttu-id="e237e-113">Na stránce hello se zobrazí další možnosti konfigurace Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="e237e-113">Additional Azure Active Directory Domain Services configuration options appear on hello page.</span></span>

    ![Povolení doménových služeb](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > <span data-ttu-id="e237e-115">Když povolíte Azure Active Directory Domain Services pro vašeho klienta, Azure AD generuje a uloží hello protokolu Kerberos a NTLM hodnoty hash přihlašovacích údajů vyžadované pro ověřování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e237e-115">When you enable Azure Active Directory Domain Services for your tenant, Azure AD generates and stores hello Kerberos and NTLM credential hashes that are required for authenticating users.</span></span>
   >
   >
6. <span data-ttu-id="e237e-116">Zadejte hello **název domény DNS služby domain services**.</span><span class="sxs-lookup"><span data-stu-id="e237e-116">Specify hello **DNS domain name of domain services**.</span></span>

   * <span data-ttu-id="e237e-117">Hello výchozí název domény hello adresáře (s **. onmicrosoft.com** příponu) je standardně vybraná.</span><span class="sxs-lookup"><span data-stu-id="e237e-117">hello default domain name of hello directory (with a **.onmicrosoft.com** suffix) is selected by default.</span></span>

   * <span data-ttu-id="e237e-118">Hello seznam obsahuje všechny domény, které byly nakonfigurovány pro váš adresář Azure AD, včetně obě ověřit a neověřených domén, které nakonfigurujete na hello **domén** kartě.</span><span class="sxs-lookup"><span data-stu-id="e237e-118">hello list contains all domains that have been configured for your Azure AD directory, including both verified and unverified domains that you configure on hello **Domains** tab.</span></span>

   * <span data-ttu-id="e237e-119">Můžete zadat i vlastní název domény.</span><span class="sxs-lookup"><span data-stu-id="e237e-119">You can also enter a custom domain name.</span></span> <span data-ttu-id="e237e-120">V tomto příkladu je název vlastní domény hello *contoso100.com*.</span><span class="sxs-lookup"><span data-stu-id="e237e-120">In this example, hello custom domain name is *contoso100.com*.</span></span>

     > [!WARNING]
     > <span data-ttu-id="e237e-121">Předpona Hello vaší zadaného názvu domény (například *contoso100* v hello *contoso100.com* název domény) musí obsahovat nejvýše 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="e237e-121">hello prefix of your specified domain name (for example, *contoso100* in hello *contoso100.com* domain name) must contain 15 or fewer characters.</span></span> <span data-ttu-id="e237e-122">Nelze vytvořit domény služby Azure Active Directory Domain Services s předponou obsahující více než 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="e237e-122">You cannot create an Azure Active Directory Domain Services domain with a prefix containing more than 15 characters.</span></span>
     >
     >
7. <span data-ttu-id="e237e-123">Zkontrolujte název domény DNS hello, které jste vybrali pro hello spravované domény ve virtuální síti hello již neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e237e-123">Ensure that hello DNS domain name you have chosen for hello managed domain does not already exist in hello virtual network.</span></span> <span data-ttu-id="e237e-124">Konkrétně zkontrolujte zda toosee:</span><span class="sxs-lookup"><span data-stu-id="e237e-124">Specifically, check toosee whether:</span></span>

   * <span data-ttu-id="e237e-125">Už máte doménu hello stejným názvem domény DNS ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="e237e-125">You already have a domain with hello same DNS domain name on hello virtual network.</span></span>

   * <span data-ttu-id="e237e-126">Hello virtuální sítě, které jste vybrali má připojení VPN s vaší místní síti a máte doménu s hello stejným názvem domény DNS ve vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="e237e-126">hello virtual network you've selected has a VPN connection with your on-premises network, and you have a domain with hello same DNS domain name on your on-premises network.</span></span>

   * <span data-ttu-id="e237e-127">Máte stávající cloudovou službu s tímto názvem ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="e237e-127">You have an existing cloud service with that name on hello virtual network.</span></span>
8. <span data-ttu-id="e237e-128">Vyberte virtuální síť, na kterém chcete Azure Active Directory Domain Services toobe k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e237e-128">Select a virtual network on which you want Azure Active Directory Domain Services toobe available.</span></span> <span data-ttu-id="e237e-129">Vyberte virtuální síť hello a vyhrazené podsíť, které jste vytvořili v hello **služeb domény připojit virtuální síť toothis** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="e237e-129">Select hello virtual network and dedicated subnet you created in hello **Connect domain services toothis virtual network** drop-down list.</span></span> <span data-ttu-id="e237e-130">Zvažte také následující hello:</span><span class="sxs-lookup"><span data-stu-id="e237e-130">Also consider hello following:</span></span>

   * <span data-ttu-id="e237e-131">Zkontrolujte, zda text hello virtuální síti, kterou jste určili patří tooan oblast Azure, která je podporována službou Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="e237e-131">Ensure that hello virtual network that you have specified belongs tooan Azure region that's supported by Azure Active Directory Domain Services.</span></span> <span data-ttu-id="e237e-132">tooascertain hello oblastí Azure, kde je k dispozici Azure Active Directory Domain Services najdete v tématu [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="e237e-132">tooascertain hello Azure regions where Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>

   * <span data-ttu-id="e237e-133">V rozevíracím seznamu hello se nezobrazí virtuální sítě, které patří tooa oblasti, kde se nepodporuje Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="e237e-133">Virtual networks that belong tooa region where Azure Active Directory Domain Services is not supported do not show up in hello drop-down list.</span></span>

   * <span data-ttu-id="e237e-134">Použijte vyhrazenou podsítí v rámci hello virtuální sítě pro Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="e237e-134">Use a dedicated subnet within hello virtual network for Azure Active Directory Domain Services.</span></span> <span data-ttu-id="e237e-135">Proveďte *není* vyberte hello podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="e237e-135">Do *not* select hello gateway subnet.</span></span> <span data-ttu-id="e237e-136">Viz téma o [aspektech sítí](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="e237e-136">See [networking considerations](active-directory-ds-networking.md).</span></span>

   * <span data-ttu-id="e237e-137">Podobně virtuální sítě, které byly vytvořeny pomocí Azure Resource Manager nejsou uvedena v rozevíracím seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="e237e-137">Similarly, virtual networks that were created by using Azure Resource Manager do not appear in hello drop-down list.</span></span> <span data-ttu-id="e237e-138">Virtuální sítě na bázi Resource Manageru nejsou v současné době službou Azure Active Directory Domain Services podporované.</span><span class="sxs-lookup"><span data-stu-id="e237e-138">Resource Manager-based virtual networks are not currently supported by Azure Active Directory Domain Services.</span></span>
9. <span data-ttu-id="e237e-139">Klikněte na tlačítko tooenable Azure Active Directory Domain Services, v podokně úloh hello v hello dolní části stránky hello **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e237e-139">tooenable Azure Active Directory Domain Services, in hello task pane at hello bottom of hello page, click **Save**.</span></span>
    * <span data-ttu-id="e237e-140">Zatímco Azure Active Directory Domain Services je povolený pro váš adresář, stránku hello zobrazuje stav *čekající*.</span><span class="sxs-lookup"><span data-stu-id="e237e-140">While Azure Active Directory Domain Services is being enabled for your directory, hello page displays a status of *Pending*.</span></span>

        ![Povolení okna Domain Services](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > <span data-ttu-id="e237e-142">Služba Azure Active Directory Domain Services poskytuje vysokou dostupnost vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="e237e-142">Azure Active Directory Domain Services provides high availability for your managed domain.</span></span> <span data-ttu-id="e237e-143">Jakmile povolíte Azure Active Directory Domain Services, jsou hello IP adresy, které služby jsou dostupné ve virtuální síti hello zobrazených současně.</span><span class="sxs-lookup"><span data-stu-id="e237e-143">After you enable Azure Active Directory Domain Services, hello IP addresses at which domain services are available on hello virtual network are displayed one at a time.</span></span> <span data-ttu-id="e237e-144">Hello druhou IP adresu zobrazí krátce po hello nejprve jako brzy hello služba povolí vysokou dostupnost vaší domény.</span><span class="sxs-lookup"><span data-stu-id="e237e-144">hello second IP address is displayed shortly after hello first, as soon hello service enables high availability for your domain.</span></span> <span data-ttu-id="e237e-145">Při vysoké dostupnosti je nakonfigurovaná a aktivní pro vaši doménu, jste měli vidět dvě IP adresy v hello **služby domény** části hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="e237e-145">When high availability is configured and active for your domain, you should see two IP addresses in hello **domain services** section of hello **Configure** tab.</span></span>
        >
        >
    * <span data-ttu-id="e237e-146">Po přibližně 20 minut too30, hello první IP adresy ve které doméně služby jsou dostupné ve vaší virtuální síti v hello **IP adresu** na hello **konfigurace** stránky.</span><span class="sxs-lookup"><span data-stu-id="e237e-146">After about 20 too30 minutes, hello first IP address at which domain services are available on your virtual network in hello **IP address** field on hello **Configure** page.</span></span>

        ![Okno Domain Services se zobrazenou první zřízenou IP adresou](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * <span data-ttu-id="e237e-148">Když je vysoká dostupnost provozní vaší domény, zobrazí se na stránce hello dvě IP adresy.</span><span class="sxs-lookup"><span data-stu-id="e237e-148">When high availability is operational for your domain, two IP addresses are displayed on hello page.</span></span> <span data-ttu-id="e237e-149">Na těchto dvou IP adresách je k dispozici vaše spravovaná doména ve vybrané virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="e237e-149">Your managed domain is available on your selected virtual network at these two IP addresses.</span></span>

10. <span data-ttu-id="e237e-150">Poznamenejte si hello dvě IP adresy, abyste aktualizujete hello nastavení DNS pro vaši virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="e237e-150">Note hello two IP addresses so that you can update hello DNS settings for your virtual network.</span></span> <span data-ttu-id="e237e-151">Díky tomu virtuální počítače na hello virtuální sítě tooconnect toohello domény pro operace, jako je například připojení k doméně.</span><span class="sxs-lookup"><span data-stu-id="e237e-151">Doing so enables virtual machines on hello virtual network tooconnect toohello domain for operations such as domain join.</span></span>

    ![Okno služby Domain Service se zobrazením obou zřízených IP adres](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> <span data-ttu-id="e237e-153">Spravované domény tooyour synchronizace v závislosti na velikosti hello klienta služby Azure AD (například hello počet uživatele nebo skupiny), trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="e237e-153">Depending on hello size of your Azure AD tenant (for example, hello number of users or groups), synchronization tooyour managed domain takes a while.</span></span> <span data-ttu-id="e237e-154">Tento proces synchronizace se odehrává na pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="e237e-154">This synchronization process happens in hello background.</span></span> <span data-ttu-id="e237e-155">U velkých klientů s desítkami tisíc objektů může trvat i několik dní pro všechny uživatele, členství ve skupinách a přihlašovacích údajů toobe synchronizované.</span><span class="sxs-lookup"><span data-stu-id="e237e-155">For large tenants with tens of thousands of objects, it might take a day or two for all users, group memberships, and credentials toobe synchronized.</span></span>
>
>

## <a name="next-step"></a><span data-ttu-id="e237e-156">Další krok</span><span class="sxs-lookup"><span data-stu-id="e237e-156">Next step</span></span>
[<span data-ttu-id="e237e-157">Úloha 4: aktualizace nastavení DNS hello hello virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="e237e-157">Task 4: update hello DNS settings for hello Azure virtual network</span></span>](active-directory-ds-getting-started-update-dns.md)
