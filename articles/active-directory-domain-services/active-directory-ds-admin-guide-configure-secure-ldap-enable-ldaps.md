---
title: "Konfigurace zabezpečeného LDAP (LDAPS) ve službě Azure AD Domain Services | Microsoft Docs"
description: "Konfigurace zabezpečení protokolu LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 3b19f078b0d6dc3e02d951014056406fd1b099a8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="826d5-103">Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="826d5-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="826d5-104">Než začnete</span><span class="sxs-lookup"><span data-stu-id="826d5-104">Before you begin</span></span>
<span data-ttu-id="826d5-105">Ujistěte se, když jste dokončili [úloha 2 - zabezpečený LDAP certifikát, který chcete exportovat. Soubor PFX](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="826d5-105">Ensure you've completed [Task 2 - export the secure LDAP certificate to a .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="826d5-106">Vyberte, zda chcete tuto úlohu dokončit pomocí práci s portálem Azure preview nebo portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="826d5-106">Choose whether to use the preview Azure portal experience or the Azure classic portal to complete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="826d5-107">**Portál Azure (Preview)**: [povolit zabezpečený LDAP pomocí portálu Azure](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="826d5-107">**Azure portal (Preview)**: [Enable secure LDAP using the Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="826d5-108">**Portál Azure classic**: [povolit zabezpečený LDAP pomocí portálu Azure classic](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="826d5-108">**Azure classic portal**: [Enable secure LDAP using the classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview"></a><span data-ttu-id="826d5-109">Úloha 3 – povolení zabezpečeného LDAP pro spravované doméně pomocí portálu Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="826d5-109">Task 3 - enable secure LDAP for the managed domain using the Azure portal (Preview)</span></span>
<span data-ttu-id="826d5-110">Pokud chcete povolit zabezpečený LDAP, proveďte následující kroky konfigurace:</span><span class="sxs-lookup"><span data-stu-id="826d5-110">To enable secure LDAP, perform the following configuration steps:</span></span>

1. <span data-ttu-id="826d5-111">Přejděte na  **[portál Azure](https://portal.azure.com)**.</span><span class="sxs-lookup"><span data-stu-id="826d5-111">Navigate to the **[Azure portal](https://portal.azure.com)**.</span></span>

2. <span data-ttu-id="826d5-112">Vyhledejte 'domain services' v **vyhledávání prostředků** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="826d5-112">Search for 'domain services' in the **Search resources** search box.</span></span> <span data-ttu-id="826d5-113">Vyberte **Azure AD Domain Services** z výsledku hledání.</span><span class="sxs-lookup"><span data-stu-id="826d5-113">Select **Azure AD Domain Services** from the search result.</span></span> <span data-ttu-id="826d5-114">**Azure AD Domain Services** okno uvádí vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="826d5-114">The **Azure AD Domain Services** blade lists your managed domain.</span></span>

    ![Najít spravované doméně, se zřídí](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="826d5-116">Klikněte na název spravované domény (například "contoso100.com") můžete zjistit podrobnosti o doméně.</span><span class="sxs-lookup"><span data-stu-id="826d5-116">Click the name of the managed domain (for example, 'contoso100.com') to see more details about the domain.</span></span>

    ![Doménových služeb – Stav zřizování](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="826d5-118">Klikněte na tlačítko **zabezpečení LDAP** v navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="826d5-118">Click **Secure LDAP** on the navigation pane.</span></span>

    ![Doménových služeb – okno zabezpečený LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. <span data-ttu-id="826d5-120">Zabezpečený LDAP přístup k vaší spravované domény je ve výchozím nastavení zakázaná.</span><span class="sxs-lookup"><span data-stu-id="826d5-120">By default, secure LDAP access to your managed domain is disabled.</span></span> <span data-ttu-id="826d5-121">Přepnutí **zabezpečený LDAP** k **povolit**.</span><span class="sxs-lookup"><span data-stu-id="826d5-121">Toggle **Secure LDAP** to **Enable**.</span></span>

    ![Povolit zabezpečený LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. <span data-ttu-id="826d5-123">Zabezpečený LDAP přístup k vaší spravované domény přes internet je ve výchozím nastavení zakázaná.</span><span class="sxs-lookup"><span data-stu-id="826d5-123">By default, secure LDAP access to your managed domain over the internet is disabled.</span></span> <span data-ttu-id="826d5-124">Přepnutí **povolit zabezpečený LDAP přístup přes internet** k **povolit**, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="826d5-124">Toggle **Allow secure LDAP access over the internet** to **Enable**, if desired.</span></span> 

6. <span data-ttu-id="826d5-125">Klikněte na ikonu následující složky **. Soubor PFX s certifikátem zabezpečení LDAP**.</span><span class="sxs-lookup"><span data-stu-id="826d5-125">Click the folder icon following **.PFX file with secure LDAP certificate**.</span></span> <span data-ttu-id="826d5-126">Zadejte cestu k souboru PFX pomocí certifikátu pro zabezpečený přístup protokolu LDAP k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="826d5-126">Specify the path to the PFX file with the certificate for secure LDAP access to the managed domain.</span></span>

7. <span data-ttu-id="826d5-127">Zadejte **heslo pro dešifrování. Soubor PFX**.</span><span class="sxs-lookup"><span data-stu-id="826d5-127">Specify the **Password to decrypt .PFX file**.</span></span> <span data-ttu-id="826d5-128">Zadejte stejné heslo, které jste použili při exportu certifikátu do souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="826d5-128">Provide the same password you used when exporting the certificate to the PFX file.</span></span>

8. <span data-ttu-id="826d5-129">Až budete hotovi, klikněte na **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="826d5-129">When you are done, click the **Save** button.</span></span>

9. <span data-ttu-id="826d5-130">Uvidíte, že oznámení, že informuje o tom, že zabezpečený LDAP je konfigurován pro spravovanou doménu.</span><span class="sxs-lookup"><span data-stu-id="826d5-130">You see a notification that informs you secure LDAP is being configured for the managed domain.</span></span> <span data-ttu-id="826d5-131">Až do dokončení této operace, nelze změnit další nastavení pro doménu.</span><span class="sxs-lookup"><span data-stu-id="826d5-131">Until this operation is complete, you cannot modify other settings for the domain.</span></span>

    ![Konfigurace zabezpečeného LDAP pro spravované doméně](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> <span data-ttu-id="826d5-133">Chcete-li povolit zabezpečený LDAP vaší spravované domény trvá asi 10 až 15 minut.</span><span class="sxs-lookup"><span data-stu-id="826d5-133">It takes about 10 to 15 minutes to enable secure LDAP for your managed domain.</span></span> <span data-ttu-id="826d5-134">Pokud k zadanému zabezpečený LDAP certifikátu neodpovídá požadované kritéria, zabezpečený LDAP není povoleno pro svůj adresář a zobrazit informace o selhání.</span><span class="sxs-lookup"><span data-stu-id="826d5-134">If the provided secure LDAP certificate does not match the required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="826d5-135">Například je nesprávný název domény, certifikátu již vypršela nebo brzo vyprší.</span><span class="sxs-lookup"><span data-stu-id="826d5-135">For example, the domain name is incorrect, the certificate has already expired or expires soon.</span></span> <span data-ttu-id="826d5-136">V takovém případě zkuste to znovu s platným certifikátem.</span><span class="sxs-lookup"><span data-stu-id="826d5-136">In this case, retry with a valid certificate.</span></span>
>
>

<br>

## <a name="task-4---configure-dns-to-access-the-managed-domain-from-the-internet"></a><span data-ttu-id="826d5-137">Úloha 4: Konfigurace DNS pro přístup k spravované doméně z Internetu</span><span class="sxs-lookup"><span data-stu-id="826d5-137">Task 4 - configure DNS to access the managed domain from the internet</span></span>
> [!NOTE]
> <span data-ttu-id="826d5-138">**Nepovinná úloha** – Pokud neplánujete přístup ke spravované doméně pomocí LDAPS přes internet, přeskočte tento úkol konfigurace.</span><span class="sxs-lookup"><span data-stu-id="826d5-138">**Optional task** - If you do not plan to access the managed domain using LDAPS over the internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="826d5-139">Než začnete tuto úlohu, zkontrolujte jste dokončili podle kroků uvedených v [úloha 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="826d5-139">Before you begin this task, ensure you have completed the steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="826d5-140">Jakmile povolíte zabezpečený LDAP přístup přes internet vaší spravované domény, je potřeba aktualizovat DNS, aby klientské počítače najít této spravované domény.</span><span class="sxs-lookup"><span data-stu-id="826d5-140">Once you have enabled secure LDAP access over the internet for your managed domain, you need to update DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="826d5-141">Na konci úloha 3 externí IP adresa se zobrazí na **vlastnosti** okno v **externí IP adresu pro LDAPS přístup**.</span><span class="sxs-lookup"><span data-stu-id="826d5-141">At the end of task 3, an external IP address is displayed on the **Properties** blade in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="826d5-142">Nakonfigurujte externího poskytovatele DNS, aby název DNS spravované doméně (například "ldaps.contoso100.com') odkazuje na tato externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="826d5-142">Configure your external DNS provider so that the DNS name of the managed domain (for example, 'ldaps.contoso100.com') points to this external IP address.</span></span> <span data-ttu-id="826d5-143">V našem příkladu je potřeba vytvořit následující položku DNS:</span><span class="sxs-lookup"><span data-stu-id="826d5-143">In our example, we need to create the following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="826d5-144">Je to – nyní jste připraveni k připojení k spravované doméně pomocí zabezpečený LDAP přes internet.</span><span class="sxs-lookup"><span data-stu-id="826d5-144">That's it - you are now ready to connect to the managed domain using secure LDAP over the internet.</span></span>

> [!WARNING]
> <span data-ttu-id="826d5-145">Mějte na paměti, že klientské počítače musí důvěřovat vystavitele certifikátu LDAPS moct úspěšně připojit k spravované doméně pomocí LDAPS.</span><span class="sxs-lookup"><span data-stu-id="826d5-145">Remember that client computers must trust the issuer of the LDAPS certificate to be able to connect successfully to the managed domain using LDAPS.</span></span> <span data-ttu-id="826d5-146">Pokud používáte veřejně důvěryhodné certifikační autority, není potřeba dělat nic, protože klientské počítače důvěřovat tyto vystavitelů certifikátů.</span><span class="sxs-lookup"><span data-stu-id="826d5-146">If you are using a publicly trusted certification authority, you do not need to do anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="826d5-147">Pokud používáte certifikát podepsaný svým držitelem, nainstalujte část veřejný certifikát podepsaný svým držitelem do úložiště důvěryhodných certifikátů v klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="826d5-147">If you are using a self-signed certificate, install the public part of the self-signed certificate into the trusted certificate store on the client computer.</span></span>
>
>


## <a name="task-5---lock-down-ldaps-access-to-your-managed-domain-over-the-internet"></a><span data-ttu-id="826d5-148">Úloha 5: zamykání LDAPS přístup k vaší spravované domény přes internet</span><span class="sxs-lookup"><span data-stu-id="826d5-148">Task 5 - lock-down LDAPS access to your managed domain over the internet</span></span>
> [!NOTE]
> <span data-ttu-id="826d5-149">**Nepovinná úloha** – Pokud jste nepovolili LDAPS přístup k spravované doméně přes internet, přeskočte tento úkol konfigurace.</span><span class="sxs-lookup"><span data-stu-id="826d5-149">**Optional task** - If you have not enabled LDAPS access to the managed domain over the internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="826d5-150">Než začnete tuto úlohu, zkontrolujte jste dokončili podle kroků uvedených v [úloha 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="826d5-150">Before you begin this task, ensure you have completed the steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="826d5-151">Vystavení vaší spravované domény pro LDAPS přístup přes internet představuje bezpečnostní riziko.</span><span class="sxs-lookup"><span data-stu-id="826d5-151">Exposing your managed domain for LDAPS access over the internet represents a security threat.</span></span> <span data-ttu-id="826d5-152">Spravované doméně je dosažitelný z Internetu na port používaný pro zabezpečený LDAP (to znamená, port 636).</span><span class="sxs-lookup"><span data-stu-id="826d5-152">The managed domain is reachable from the internet at the port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="826d5-153">Proto můžete omezit přístup k spravované doméně na konkrétní známé IP adresy.</span><span class="sxs-lookup"><span data-stu-id="826d5-153">Therefore, you can choose to restrict access to the managed domain to specific known IP addresses.</span></span> <span data-ttu-id="826d5-154">Pro lepší zabezpečení vytvořte skupinu zabezpečení sítě (NSG) a přidružte ji k podsíti, kde jste povolili službu Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="826d5-154">For improved security, create a network security group (NSG) and associate it with the subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="826d5-155">Následující tabulka znázorňuje ukázku NSG můžete nakonfigurovat, zamknout zabezpečený LDAP přístup přes internet.</span><span class="sxs-lookup"><span data-stu-id="826d5-155">The following table illustrates a sample NSG you can configure, to lock down secure LDAP access over the internet.</span></span> <span data-ttu-id="826d5-156">NSG obsahuje sadu pravidel, která povolí příchozí LDAPS přístup přes port TCP 636 pouze ze zadané sady IP adres.</span><span class="sxs-lookup"><span data-stu-id="826d5-156">The NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="826d5-157">Výchozí pravidlo, DenyAll, platí pro všechny ostatní příchozí přenosy z Internetu.</span><span class="sxs-lookup"><span data-stu-id="826d5-157">The default 'DenyAll' rule applies to all other inbound traffic from the internet.</span></span> <span data-ttu-id="826d5-158">Pravidla NSG pro povolení LDAPS přístupu přes internet ze zadaných IP adres má vyšší prioritu než skupina NSG DenyAll pravidlo.</span><span class="sxs-lookup"><span data-stu-id="826d5-158">The NSG rule to allow LDAPS access over the internet from specified IP addresses has a higher priority than the DenyAll NSG rule.</span></span>

![Ukázka skupiny NSG k zabezpečení přístupu k LDAPS přes internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="826d5-160">**Další informace** - [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="826d5-160">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="826d5-161">Související obsah</span><span class="sxs-lookup"><span data-stu-id="826d5-161">Related content</span></span>
* [<span data-ttu-id="826d5-162">Azure AD Domain Services – Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="826d5-162">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="826d5-163">Správa spravované domény služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="826d5-163">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="826d5-164">Správa zásad skupiny na spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="826d5-164">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="826d5-165">Skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="826d5-165">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="826d5-166">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="826d5-166">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
