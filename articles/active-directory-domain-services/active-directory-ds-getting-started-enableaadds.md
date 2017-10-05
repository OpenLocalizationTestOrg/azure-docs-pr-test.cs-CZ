---
title: "Azure Active Directory Domain Services: Povolení služby Azure Active Directory Domain Services | Dokumentace Microsoftu"
description: "Povolení služby Azure Active Directory Domain Services pomocí portálu Azure Classic"
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
ms.openlocfilehash: ed72325ca9db99405c6173eb882a92f80cd77f47
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-classic-portal"></a><span data-ttu-id="674aa-103">Povolení služby Azure Active Directory Domain Services pomocí portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="674aa-103">Enable Azure Active Directory Domain Services using the Azure classic portal</span></span>

## <a name="task-3-enable-azure-active-directory-domain-services"></a><span data-ttu-id="674aa-104">Úloha 3: Povolení služby Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="674aa-104">Task 3: enable Azure Active Directory Domain Services</span></span>
<span data-ttu-id="674aa-105">V této úloze povolíte službu Azure Active Directory Domain Services (Azure AD DS) pro svůj adresář pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="674aa-105">In this task, you enable Azure Active Directory Domain Services (Azure AD DS) for your directory by doing the following steps:</span></span>

1. <span data-ttu-id="674aa-106">Přejděte na [portál Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="674aa-106">Go to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="674aa-107">V levém podokně vyberte tlačítko **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="674aa-107">In the left pane, select the **Active Directory** button.</span></span>
3. <span data-ttu-id="674aa-108">Vyberte klienta (adresář) Azure Active Directory (Azure DS), pro kterého chcete povolit službu Azure AD DS.</span><span class="sxs-lookup"><span data-stu-id="674aa-108">Select the Azure Active Directory (Azure AD) tenant (directory) for which you want to enable Azure AD DS.</span></span>

    ![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="674aa-110">Na stránce **Náhled adresáře** klikněte na kartu **Konfigurovat**.</span><span class="sxs-lookup"><span data-stu-id="674aa-110">On the **preview directory** page, click the **Configure** tab.</span></span>

    ![Karta konfigurace adresáře](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. <span data-ttu-id="674aa-112">V části **Domain Services** změňte možnost **Povolit službu Domain Services pro tento adresář** na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="674aa-112">Under **domain services**, change the **Enable domain services for this directory** option to **Yes**.</span></span>  
    <span data-ttu-id="674aa-113">Na stránce se zobrazí další možnosti konfigurace služby Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="674aa-113">Additional Azure Active Directory Domain Services configuration options appear on the page.</span></span>

    ![Povolení doménových služeb](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > <span data-ttu-id="674aa-115">Když pro svého klienta povolíte službu Azure Active Directory Domain Services, Azure AD bude generovat a ukládat hodnoty hash přihlašovacích údajů protokolů Kerberos a NTLM potřebné pro ověřování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="674aa-115">When you enable Azure Active Directory Domain Services for your tenant, Azure AD generates and stores the Kerberos and NTLM credential hashes that are required for authenticating users.</span></span>
   >
   >
6. <span data-ttu-id="674aa-116">Zadejte **název domény DNS doménových služeb**.</span><span class="sxs-lookup"><span data-stu-id="674aa-116">Specify the **DNS domain name of domain services**.</span></span>

   * <span data-ttu-id="674aa-117">Ve výchozím nastavení je vybrán výchozí název domény adresáře (s příponou **.onmicrosoft.com**).</span><span class="sxs-lookup"><span data-stu-id="674aa-117">The default domain name of the directory (with a **.onmicrosoft.com** suffix) is selected by default.</span></span>

   * <span data-ttu-id="674aa-118">Seznam obsahuje všechny domény, které byly nakonfigurované pro váš adresář služby Azure AD – včetně ověřených i neověřených domén, které konfigurujete na kartě **Domény**.</span><span class="sxs-lookup"><span data-stu-id="674aa-118">The list contains all domains that have been configured for your Azure AD directory, including both verified and unverified domains that you configure on the **Domains** tab.</span></span>

   * <span data-ttu-id="674aa-119">Můžete zadat i vlastní název domény.</span><span class="sxs-lookup"><span data-stu-id="674aa-119">You can also enter a custom domain name.</span></span> <span data-ttu-id="674aa-120">V tomto příkladu je použit vlastní název domény *contoso100.com*.</span><span class="sxs-lookup"><span data-stu-id="674aa-120">In this example, the custom domain name is *contoso100.com*.</span></span>

     > [!WARNING]
     > <span data-ttu-id="674aa-121">Předpona zadaného názvu domény (například *contoso100* v případě názvu domény *contoso100.com*) musí obsahovat nejvýše 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="674aa-121">The prefix of your specified domain name (for example, *contoso100* in the *contoso100.com* domain name) must contain 15 or fewer characters.</span></span> <span data-ttu-id="674aa-122">Nelze vytvořit domény služby Azure Active Directory Domain Services s předponou obsahující více než 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="674aa-122">You cannot create an Azure Active Directory Domain Services domain with a prefix containing more than 15 characters.</span></span>
     >
     >
7. <span data-ttu-id="674aa-123">Ujistěte se, že vámi zvolený název domény DNS pro spravovanou doménu ještě ve virtuální síti neexistuje.</span><span class="sxs-lookup"><span data-stu-id="674aa-123">Ensure that the DNS domain name you have chosen for the managed domain does not already exist in the virtual network.</span></span> <span data-ttu-id="674aa-124">Konkrétně zkontrolujte následující body:</span><span class="sxs-lookup"><span data-stu-id="674aa-124">Specifically, check to see whether:</span></span>

   * <span data-ttu-id="674aa-125">Ve virtuální síti již existuje doména se stejným názvem domény DNS.</span><span class="sxs-lookup"><span data-stu-id="674aa-125">You already have a domain with the same DNS domain name on the virtual network.</span></span>

   * <span data-ttu-id="674aa-126">Vybraná virtuální síť má připojení VPN k vaší místní síti, ve které máte doménu se stejným názvem domény DNS.</span><span class="sxs-lookup"><span data-stu-id="674aa-126">The virtual network you've selected has a VPN connection with your on-premises network, and you have a domain with the same DNS domain name on your on-premises network.</span></span>

   * <span data-ttu-id="674aa-127">Ve virtuální síti existuje cloudová služba s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="674aa-127">You have an existing cloud service with that name on the virtual network.</span></span>
8. <span data-ttu-id="674aa-128">Vyberte virtuální síť, ve které má být k dispozici služba Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="674aa-128">Select a virtual network on which you want Azure Active Directory Domain Services to be available.</span></span> <span data-ttu-id="674aa-129">Z rozevíracího seznamu **Připojit službu Domain Services k této virtuální síti** vyberte vytvořenou virtuální síť a vyhrazenou podsíť.</span><span class="sxs-lookup"><span data-stu-id="674aa-129">Select the virtual network and dedicated subnet you created in the **Connect domain services to this virtual network** drop-down list.</span></span> <span data-ttu-id="674aa-130">Zvažte také následující body:</span><span class="sxs-lookup"><span data-stu-id="674aa-130">Also consider the following:</span></span>

   * <span data-ttu-id="674aa-131">Ujistěte se, že určená virtuální síť patří do oblasti Azure podporované službou Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="674aa-131">Ensure that the virtual network that you have specified belongs to an Azure region that's supported by Azure Active Directory Domain Services.</span></span> <span data-ttu-id="674aa-132">Oblasti Azure, ve kterých je služba Azure Active Directory Domain Services k dispozici, pro ověření najdete v článku [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="674aa-132">To ascertain the Azure regions where Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>

   * <span data-ttu-id="674aa-133">Virtuální sítě patřící do oblasti, ve které není služba Azure Active Directory Domain Services podporovaná, se v rozevíracím seznamu nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="674aa-133">Virtual networks that belong to a region where Azure Active Directory Domain Services is not supported do not show up in the drop-down list.</span></span>

   * <span data-ttu-id="674aa-134">Pro službu Azure Active Directory Domain Services použijte vyhrazenou podsíť ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="674aa-134">Use a dedicated subnet within the virtual network for Azure Active Directory Domain Services.</span></span> <span data-ttu-id="674aa-135">*Nevybírejte* podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="674aa-135">Do *not* select the gateway subnet.</span></span> <span data-ttu-id="674aa-136">Viz téma o [aspektech sítí](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="674aa-136">See [networking considerations](active-directory-ds-networking.md).</span></span>

   * <span data-ttu-id="674aa-137">Podobně se v rozevíracím seznamu nezobrazí ani virtuální sítě vytvořené pomocí Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="674aa-137">Similarly, virtual networks that were created by using Azure Resource Manager do not appear in the drop-down list.</span></span> <span data-ttu-id="674aa-138">Virtuální sítě na bázi Resource Manageru nejsou v současné době službou Azure Active Directory Domain Services podporované.</span><span class="sxs-lookup"><span data-stu-id="674aa-138">Resource Manager-based virtual networks are not currently supported by Azure Active Directory Domain Services.</span></span>
9. <span data-ttu-id="674aa-139">Službu Azure Active Directory Domain Services povolíte kliknutím na **Uložit** v podokně úloh v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="674aa-139">To enable Azure Active Directory Domain Services, in the task pane at the bottom of the page, click **Save**.</span></span>
    * <span data-ttu-id="674aa-140">Zatímco probíhá povolování služby Azure Active Directory Domain Services pro váš adresář, na stránce se zobrazuje stav *Čeká na vyřízení*.</span><span class="sxs-lookup"><span data-stu-id="674aa-140">While Azure Active Directory Domain Services is being enabled for your directory, the page displays a status of *Pending*.</span></span>

        ![Povolení okna Domain Services](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > <span data-ttu-id="674aa-142">Služba Azure Active Directory Domain Services poskytuje vysokou dostupnost vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="674aa-142">Azure Active Directory Domain Services provides high availability for your managed domain.</span></span> <span data-ttu-id="674aa-143">Když službu Azure Active Directory Domain Services povolíte, postupně se zobrazují IP adresy, na kterých je služba Domain Services ve virtuální síti k dispozici.</span><span class="sxs-lookup"><span data-stu-id="674aa-143">After you enable Azure Active Directory Domain Services, the IP addresses at which domain services are available on the virtual network are displayed one at a time.</span></span> <span data-ttu-id="674aa-144">Druhá IP adresa se zobrazí krátce po první adrese, jakmile služba povolí vysokou dostupnost vaší domény.</span><span class="sxs-lookup"><span data-stu-id="674aa-144">The second IP address is displayed shortly after the first, as soon the service enables high availability for your domain.</span></span> <span data-ttu-id="674aa-145">Když je vysoká dostupnost vaší domény nakonfigurovaná a aktivní, měli byste vidět dvě IP adresy v oddílu **doménové služby** na kartě **Konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="674aa-145">When high availability is configured and active for your domain, you should see two IP addresses in the **domain services** section of the **Configure** tab.</span></span>
        >
        >
    * <span data-ttu-id="674aa-146">Přibližně po 20 až 30 minutách se první IP adresa, na které je k dispozici služba Domain Services ve virtuální síti, zobrazí v poli **IP adresa** na stránce **Konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="674aa-146">After about 20 to 30 minutes, the first IP address at which domain services are available on your virtual network in the **IP address** field on the **Configure** page.</span></span>

        ![Okno Domain Services se zobrazenou první zřízenou IP adresou](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * <span data-ttu-id="674aa-148">Když je vysoká dostupnost vaší domény funkční, zobrazí se na stránce dvě IP adresy.</span><span class="sxs-lookup"><span data-stu-id="674aa-148">When high availability is operational for your domain, two IP addresses are displayed on the page.</span></span> <span data-ttu-id="674aa-149">Na těchto dvou IP adresách je k dispozici vaše spravovaná doména ve vybrané virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="674aa-149">Your managed domain is available on your selected virtual network at these two IP addresses.</span></span>

10. <span data-ttu-id="674aa-150">Poznamenejte si obě tyto IP adresy, abyste mohli aktualizovat nastavení DNS pro svou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="674aa-150">Note the two IP addresses so that you can update the DNS settings for your virtual network.</span></span> <span data-ttu-id="674aa-151">Tento postup umožňuje virtuálním počítačům ve virtuální síti připojit se k doméně kvůli operacím, jako je například připojení k doméně.</span><span class="sxs-lookup"><span data-stu-id="674aa-151">Doing so enables virtual machines on the virtual network to connect to the domain for operations such as domain join.</span></span>

    ![Okno služby Domain Service se zobrazením obou zřízených IP adres](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> <span data-ttu-id="674aa-153">Synchronizace se spravovanou doménou bude v závislosti na velikosti klienta Azure AD (například na počtu uživatelů nebo skupin) chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="674aa-153">Depending on the size of your Azure AD tenant (for example, the number of users or groups), synchronization to your managed domain takes a while.</span></span> <span data-ttu-id="674aa-154">Proces synchronizace se odehrává na pozadí.</span><span class="sxs-lookup"><span data-stu-id="674aa-154">This synchronization process happens in the background.</span></span> <span data-ttu-id="674aa-155">U velkých klientů s desítkami tisíc objektů může trvat i několik dní, než se synchronizují všichni uživatelé, členství ve skupinách a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="674aa-155">For large tenants with tens of thousands of objects, it might take a day or two for all users, group memberships, and credentials to be synchronized.</span></span>
>
>

## <a name="next-step"></a><span data-ttu-id="674aa-156">Další krok</span><span class="sxs-lookup"><span data-stu-id="674aa-156">Next step</span></span>
[<span data-ttu-id="674aa-157">Úloha 4: Aktualizace nastavení DNS pro virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="674aa-157">Task 4: update the DNS settings for the Azure virtual network</span></span>](active-directory-ds-getting-started-update-dns.md)
