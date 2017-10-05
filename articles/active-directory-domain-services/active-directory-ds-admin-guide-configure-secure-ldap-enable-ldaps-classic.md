---
title: "Konfigurace zabezpečeného LDAP (LDAPS) ve službě Azure AD Domain Services | Microsoft Docs"
description: "Konfigurace zabezpečení protokolu LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9d563c45-9578-410d-96c8-355af64feae8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 3aafe209aad7383cd0610d147b5fdba673023c93
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="863c5-103">Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="863c5-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="863c5-104">Než začnete</span><span class="sxs-lookup"><span data-stu-id="863c5-104">Before you begin</span></span>
<span data-ttu-id="863c5-105">Ujistěte se, když jste dokončili [úloha 2 - zabezpečený LDAP certifikát, který chcete exportovat. Soubor PFX](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="863c5-105">Ensure you've completed [Task 2 - export the secure LDAP certificate to a .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="863c5-106">Vyberte, zda chcete tuto úlohu dokončit pomocí práci s portálem Azure preview nebo portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="863c5-106">Choose whether to use the preview Azure portal experience or the Azure classic portal to complete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="863c5-107">**Portál Azure (Preview)**: [povolit zabezpečený LDAP pomocí portálu Azure](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="863c5-107">**Azure portal (Preview)**: [Enable secure LDAP using the Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="863c5-108">**Portál Azure classic**: [povolit zabezpečený LDAP pomocí portálu Azure classic](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="863c5-108">**Azure classic portal**: [Enable secure LDAP using the classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal"></a><span data-ttu-id="863c5-109">Úloha 3 – povolení zabezpečeného LDAP pro spravované doméně pomocí portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="863c5-109">Task 3 - enable secure LDAP for the managed domain using the classic Azure portal</span></span>
<span data-ttu-id="863c5-110">Pokud chcete povolit zabezpečený LDAP, proveďte následující kroky konfigurace:</span><span class="sxs-lookup"><span data-stu-id="863c5-110">To enable secure LDAP, perform the following configuration steps:</span></span>

1. <span data-ttu-id="863c5-111">Přejděte na  **[portál Azure classic](https://manage.windowsazure.com)**.</span><span class="sxs-lookup"><span data-stu-id="863c5-111">Navigate to the **[Azure classic portal](https://manage.windowsazure.com)**.</span></span>
2. <span data-ttu-id="863c5-112">V levém podokně vyberte uzel **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="863c5-112">Select the **Active Directory** node on the left pane.</span></span>
3. <span data-ttu-id="863c5-113">Vyberte adresář Azure AD (také označovány jako "klienta"), pro kterou jste povolili službu Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="863c5-113">Select the Azure AD directory (also referred to as 'tenant'), for which you have enabled Azure AD Domain Services.</span></span>

    ![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="863c5-115">Klikněte na kartu **KONFIGUROVAT**.</span><span class="sxs-lookup"><span data-stu-id="863c5-115">Click the **Configure** tab.</span></span>

    ![Karta konfigurace adresáře](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. <span data-ttu-id="863c5-117">Přejděte dolů k části s názvem **služby domény**.</span><span class="sxs-lookup"><span data-stu-id="863c5-117">Scroll down to the section titled **domain services**.</span></span> <span data-ttu-id="863c5-118">Měli byste vidět možnost s názvem **zabezpečení protokolu LDAP (LDAPS)** jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="863c5-118">You should see an option titled **Secure LDAP (LDAPS)** as shown in the following screenshot:</span></span>

    ![Oddíl konfigurace doménových služeb](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. <span data-ttu-id="863c5-120">Klikněte **konfigurovat certifikát...**  tlačítko zprovoznit **konfigurace certifikátu pro zabezpečené LDAP** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="863c5-120">Click the **Configure certificate ...** button to bring up the **Configure Certificate for Secure LDAP** dialog.</span></span>

    ![Konfigurace certifikátu pro zabezpečený LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. <span data-ttu-id="863c5-122">Klikněte na ikonu následující složky **certifikát pomocí souboru PFX** k určení souboru PFX, který obsahuje certifikát, které chcete použít pro zabezpečený přístup protokolu LDAP k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="863c5-122">Click the folder icon following **PFX FILE WITH CERTIFICATE** to specify the PFX file, which contains the certificate you wish to use for secure LDAP access to the managed domain.</span></span> <span data-ttu-id="863c5-123">Také zadejte heslo, které jste zadali při exportu certifikátu do souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="863c5-123">Also enter the password you specified when exporting the certificate to the PFX file.</span></span> <span data-ttu-id="863c5-124">Potom klikněte na tlačítko Hotovo v dolní části.</span><span class="sxs-lookup"><span data-stu-id="863c5-124">Then, click the done button on the bottom.</span></span>

    ![Zadejte soubor zabezpečený LDAP PFX a heslo](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. <span data-ttu-id="863c5-126">**Domény služby** části **konfigurace** by měl získat šedě a je v **čekající na vyřízení...**  stavu pro několik minut.</span><span class="sxs-lookup"><span data-stu-id="863c5-126">The **domain services** section of the **Configure** tab should get grayed out and is in the **Pending...** state for a few minutes.</span></span> <span data-ttu-id="863c5-127">Během této doby je LDAPS certifikát ověřit přesnost a zabezpečený LDAP je nakonfigurována pro vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="863c5-127">During this period, the LDAPS certificate is verified for accuracy and secure LDAP is configured for your managed domain.</span></span>

    ![Zabezpečený LDAP - čekající na vyřízení stavu](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="863c5-129">Chcete-li povolit zabezpečený LDAP vaší spravované domény trvá asi 10 až 15 minut.</span><span class="sxs-lookup"><span data-stu-id="863c5-129">It takes about 10 to 15 minutes to enable secure LDAP for your managed domain.</span></span> <span data-ttu-id="863c5-130">Pokud k zadanému zabezpečený LDAP certifikátu neodpovídá požadované kritéria, zabezpečený LDAP není povoleno pro svůj adresář a zobrazit informace o selhání.</span><span class="sxs-lookup"><span data-stu-id="863c5-130">If the provided secure LDAP certificate does not match the required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="863c5-131">Například je nesprávný název domény, certifikátu již vypršela nebo brzo vyprší.</span><span class="sxs-lookup"><span data-stu-id="863c5-131">For example, the domain name is incorrect, the certificate has already expired or expires soon.</span></span>
   >
   >

9. <span data-ttu-id="863c5-132">Když vaší spravované domény úspěšně povolen zabezpečený LDAP **čekající na vyřízení...**  by zpráva zmizí.</span><span class="sxs-lookup"><span data-stu-id="863c5-132">When secure LDAP is successfully enabled for your managed domain, the **Pending...** message should disappear.</span></span> <span data-ttu-id="863c5-133">Měli byste vidět kryptografický otisk certifikátu, zobrazí.</span><span class="sxs-lookup"><span data-stu-id="863c5-133">You should see the thumbprint of the certificate displayed.</span></span>

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a><span data-ttu-id="863c5-135">Úloha 4 – povolení zabezpečeného přístupu LDAP přes internet</span><span class="sxs-lookup"><span data-stu-id="863c5-135">Task 4 - enable secure LDAP access over the internet</span></span>
<span data-ttu-id="863c5-136">**Nepovinná úloha** – Pokud neplánujete přístup ke spravované doméně pomocí LDAPS přes internet, přeskočte tento úkol konfigurace.</span><span class="sxs-lookup"><span data-stu-id="863c5-136">**Optional task** - If you do not plan to access the managed domain using LDAPS over the internet, skip this configuration task.</span></span>

<span data-ttu-id="863c5-137">Než začnete tuto úlohu, zkontrolujte jste dokončili podle kroků uvedených v [úloha 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="863c5-137">Before you begin this task, ensure you have completed the steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span></span>

1. <span data-ttu-id="863c5-138">Měli byste vidět možnost **povolit ZABEZPEČENÉ LDAP přístup přes INTERNET** v **domény služby** části **konfigurace** stránky.</span><span class="sxs-lookup"><span data-stu-id="863c5-138">You should see an option to **ENABLE SECURE LDAP ACCESS OVER THE INTERNET** in the **domain services** section of the **Configure** page.</span></span> <span data-ttu-id="863c5-139">Tato možnost nastavená na **ne** ve výchozím nastavení, protože je ve výchozím nastavení zakázán přístup k Internetu k spravované doméně přes zabezpečený LDAP.</span><span class="sxs-lookup"><span data-stu-id="863c5-139">This option is set to **NO** by default since internet access to the managed domain over secure LDAP is disabled by default.</span></span>

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. <span data-ttu-id="863c5-141">Přepnutí **UMOŽNIT ZABEZPEČENÝ LDAP přístup přes INTERNET** k **Ano**.</span><span class="sxs-lookup"><span data-stu-id="863c5-141">Toggle **ENABLE SECURE LDAP ACCESS OVER THE INTERNET** to **YES**.</span></span> <span data-ttu-id="863c5-142">Klikněte **Uložit** tlačítko na dolním panelu.</span><span class="sxs-lookup"><span data-stu-id="863c5-142">Click the **SAVE** button on the bottom panel.</span></span>
    <span data-ttu-id="863c5-143">![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span><span class="sxs-lookup"><span data-stu-id="863c5-143">![Secure LDAP enabled](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span></span>
3. <span data-ttu-id="863c5-144">**Domény služby** části **konfigurace** by měl získat šedě a je v **čekající na vyřízení...**  stavu pro několik minut.</span><span class="sxs-lookup"><span data-stu-id="863c5-144">The **domain services** section of the **Configure** tab should get grayed out and is in the **Pending...** state for a few minutes.</span></span> <span data-ttu-id="863c5-145">Po určité době je povolen přístup k Internetu k spravované doméně přes zabezpečený LDAP.</span><span class="sxs-lookup"><span data-stu-id="863c5-145">After some time, internet access to your managed domain over secure LDAP is enabled.</span></span>

    ![Zabezpečený LDAP - čekající na vyřízení stavu](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="863c5-147">Povolit přístup k Internetu přes zabezpečený LDAP vaší spravované domény trvá přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="863c5-147">It takes about 10 minutes to enable internet access over secure LDAP for your managed domain.</span></span>
   >
   >
4. <span data-ttu-id="863c5-148">Při zapnutém úspěšně zabezpečený LDAP přístup k vaší spravované domény přes internet, **čekající na vyřízení...**  by zpráva zmizí.</span><span class="sxs-lookup"><span data-stu-id="863c5-148">When secure LDAP access to your managed domain over the internet is successfully enabled, the **Pending...** message should disappear.</span></span> <span data-ttu-id="863c5-149">Měli byste vidět externí IP adresu, kterou lze použít pro přístup k adresáři přes LDAPS v poli **externí IP adresu pro LDAPS přístup**.</span><span class="sxs-lookup"><span data-stu-id="863c5-149">You should see the external IP address that can be used to access your directory over LDAPS in the field **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a><span data-ttu-id="863c5-151">Úloha 5: Konfigurace DNS pro přístup k spravované doméně z Internetu</span><span class="sxs-lookup"><span data-stu-id="863c5-151">Task 5 - configure DNS to access the managed domain from the internet</span></span>
<span data-ttu-id="863c5-152">**Nepovinná úloha** – Pokud neplánujete přístup ke spravované doméně pomocí LDAPS přes internet, přeskočte tento úkol konfigurace.</span><span class="sxs-lookup"><span data-stu-id="863c5-152">**Optional task** - If you do not plan to access the managed domain using LDAPS over the internet, skip this configuration task.</span></span>

<span data-ttu-id="863c5-153">Než začnete tuto úlohu, zkontrolujte jste dokončili podle kroků uvedených v [úloha 4](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="863c5-153">Before you begin this task, ensure you have completed the steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="863c5-154">Jakmile povolíte zabezpečený LDAP přístup přes internet vaší spravované domény, je potřeba aktualizovat DNS, aby klientské počítače najít této spravované domény.</span><span class="sxs-lookup"><span data-stu-id="863c5-154">Once you have enabled secure LDAP access over the internet for your managed domain, you need to update DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="863c5-155">Na konci úloha 4 externí IP adresa se zobrazí na **konfigurace** kartě v **externí IP adresu pro LDAPS přístup**.</span><span class="sxs-lookup"><span data-stu-id="863c5-155">At the end of task 4, an external IP address is displayed on the **Configure** tab in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="863c5-156">Nakonfigurujte externího poskytovatele DNS, aby název DNS spravované doméně (například "ldaps.contoso100.com') odkazuje na tato externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="863c5-156">Configure your external DNS provider so that the DNS name of the managed domain (for example, 'ldaps.contoso100.com') points to this external IP address.</span></span> <span data-ttu-id="863c5-157">V našem příkladu je potřeba vytvořit následující položku DNS:</span><span class="sxs-lookup"><span data-stu-id="863c5-157">In our example, we need to create the following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="863c5-158">Je to – nyní jste připraveni k připojení k spravované doméně pomocí zabezpečený LDAP přes internet.</span><span class="sxs-lookup"><span data-stu-id="863c5-158">That's it - you are now ready to connect to the managed domain using secure LDAP over the internet.</span></span>

> [!WARNING]
> <span data-ttu-id="863c5-159">Mějte na paměti, že klientské počítače musí důvěřovat vystavitele certifikátu LDAPS moct úspěšně připojit k spravované doméně pomocí LDAPS.</span><span class="sxs-lookup"><span data-stu-id="863c5-159">Remember that client computers must trust the issuer of the LDAPS certificate to be able to connect successfully to the managed domain using LDAPS.</span></span> <span data-ttu-id="863c5-160">Pokud používáte certifikační autoritu organizace nebo veřejně důvěryhodné certifikační autority, není potřeba dělat nic, protože klientské počítače důvěřovat tyto vystavitelů certifikátů.</span><span class="sxs-lookup"><span data-stu-id="863c5-160">If you are using an enterprise certification authority or a publicly trusted certification authority, you do not need to do anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="863c5-161">Pokud používáte certifikát podepsaný svým držitelem, budete muset nainstalovat část veřejný certifikát podepsaný svým držitelem do úložiště důvěryhodných certifikátů v klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="863c5-161">If you are using a self-signed certificate, you need to install the public part of the self-signed certificate into the trusted certificate store on the client computer.</span></span>
>
>


## <a name="lock-down-ldaps-access-to-your-managed-domain-over-the-internet"></a><span data-ttu-id="863c5-162">Uzamčení LDAPS přístup k vaší spravované domény přes internet</span><span class="sxs-lookup"><span data-stu-id="863c5-162">Lock-down LDAPS access to your managed domain over the internet</span></span>
> [!NOTE]
> <span data-ttu-id="863c5-163">**Nepovinná úloha** – Pokud jste nepovolili LDAPS přístup k spravované doméně přes internet, přeskočte tento úkol konfigurace.</span><span class="sxs-lookup"><span data-stu-id="863c5-163">**Optional task** - If you have not enabled LDAPS access to the managed domain over the internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="863c5-164">Než začnete tuto úlohu, zkontrolujte jste dokončili podle kroků uvedených v [úloha 4](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="863c5-164">Before you begin this task, ensure you have completed the steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="863c5-165">Vystavení vaší spravované domény pro LDAPS přístup přes internet představuje bezpečnostní riziko.</span><span class="sxs-lookup"><span data-stu-id="863c5-165">Exposing your managed domain for LDAPS access over the internet represents a security threat.</span></span> <span data-ttu-id="863c5-166">Spravované doméně je dosažitelný z Internetu na port používaný pro zabezpečený LDAP (to znamená, port 636).</span><span class="sxs-lookup"><span data-stu-id="863c5-166">The managed domain is reachable from the internet at the port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="863c5-167">Proto můžete omezit přístup k spravované doméně na konkrétní známé IP adresy.</span><span class="sxs-lookup"><span data-stu-id="863c5-167">Therefore, you can choose to restrict access to the managed domain to specific known IP addresses.</span></span> <span data-ttu-id="863c5-168">Pro lepší zabezpečení vytvořte skupinu zabezpečení sítě (NSG) a přidružte ji k podsíti, kde jste povolili službu Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="863c5-168">For improved security, create a network security group (NSG) and associate it with the subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="863c5-169">Následující tabulka znázorňuje ukázku NSG můžete nakonfigurovat, zamknout zabezpečený LDAP přístup přes internet.</span><span class="sxs-lookup"><span data-stu-id="863c5-169">The following table illustrates a sample NSG you can configure, to lock down secure LDAP access over the internet.</span></span> <span data-ttu-id="863c5-170">NSG obsahuje sadu pravidel, která povolí příchozí LDAPS přístup přes port TCP 636 pouze ze zadané sady IP adres.</span><span class="sxs-lookup"><span data-stu-id="863c5-170">The NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="863c5-171">Výchozí pravidlo, DenyAll, platí pro všechny ostatní příchozí přenosy z Internetu.</span><span class="sxs-lookup"><span data-stu-id="863c5-171">The default 'DenyAll' rule applies to all other inbound traffic from the internet.</span></span> <span data-ttu-id="863c5-172">Pravidla NSG pro povolení LDAPS přístupu přes internet ze zadaných IP adres má vyšší prioritu než skupina NSG DenyAll pravidlo.</span><span class="sxs-lookup"><span data-stu-id="863c5-172">The NSG rule to allow LDAPS access over the internet from specified IP addresses has a higher priority than the DenyAll NSG rule.</span></span>

![Ukázka skupiny NSG k zabezpečení přístupu k LDAPS přes internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="863c5-174">**Další informace** - [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="863c5-174">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="863c5-175">Související obsah</span><span class="sxs-lookup"><span data-stu-id="863c5-175">Related content</span></span>
* [<span data-ttu-id="863c5-176">Azure AD Domain Services – Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="863c5-176">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="863c5-177">Správa spravované domény služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="863c5-177">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="863c5-178">Správa zásad skupiny na spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="863c5-178">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="863c5-179">Skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="863c5-179">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="863c5-180">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="863c5-180">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
