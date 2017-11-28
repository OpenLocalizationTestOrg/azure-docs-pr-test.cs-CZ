---
title: "aaaConfigure zabezpečeného LDAP (LDAPS) v Azure AD Domain Services | Microsoft Docs"
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
ms.openlocfilehash: 8781285cd02d690788056b985b017efd7e4d6b3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="ea209-103">Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="ea209-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ea209-104">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ea209-104">Before you begin</span></span>
<span data-ttu-id="ea209-105">Ujistěte se, když jste dokončili [úloha 2 – export hello zabezpečený LDAP certifikát tooa. Soubor PFX](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="ea209-105">Ensure you've completed [Task 2 - export hello secure LDAP certificate tooa .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="ea209-106">Vyberte, zda toouse hello práci s portálem Azure preview nebo hello Azure classic portálu toocomplete této úlohy.</span><span class="sxs-lookup"><span data-stu-id="ea209-106">Choose whether toouse hello preview Azure portal experience or hello Azure classic portal toocomplete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="ea209-107">**Portál Azure (Preview)**: [povolit zabezpečený LDAP pomocí hello portálu Azure](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="ea209-107">**Azure portal (Preview)**: [Enable secure LDAP using hello Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="ea209-108">**Portál Azure classic**: [povolit zabezpečený LDAP pomocí portálu Azure classic hello](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="ea209-108">**Azure classic portal**: [Enable secure LDAP using hello classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-azure-portal-preview"></a><span data-ttu-id="ea209-109">Úloha 3 – povolení zabezpečeného LDAP pro hello spravované doméně pomocí hello portálu Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="ea209-109">Task 3 - enable secure LDAP for hello managed domain using hello Azure portal (Preview)</span></span>
<span data-ttu-id="ea209-110">tooenable zabezpečený LDAP, proveďte následující kroky konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="ea209-110">tooenable secure LDAP, perform hello following configuration steps:</span></span>

1. <span data-ttu-id="ea209-111">Přejděte toohello  **[portál Azure](https://portal.azure.com)**.</span><span class="sxs-lookup"><span data-stu-id="ea209-111">Navigate toohello **[Azure portal](https://portal.azure.com)**.</span></span>

2. <span data-ttu-id="ea209-112">Vyhledejte domain services v hello **vyhledávání prostředků** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="ea209-112">Search for 'domain services' in hello **Search resources** search box.</span></span> <span data-ttu-id="ea209-113">Vyberte **Azure AD Domain Services** z výsledek hledání hello.</span><span class="sxs-lookup"><span data-stu-id="ea209-113">Select **Azure AD Domain Services** from hello search result.</span></span> <span data-ttu-id="ea209-114">Hello **Azure AD Domain Services** okno uvádí vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="ea209-114">hello **Azure AD Domain Services** blade lists your managed domain.</span></span>

    ![Najít spravované doméně, se zřídí](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="ea209-116">Další podrobnosti o hello domény klikněte na tlačítko hello název toosee hello spravované domény (například "contoso100.com").</span><span class="sxs-lookup"><span data-stu-id="ea209-116">Click hello name of hello managed domain (for example, 'contoso100.com') toosee more details about hello domain.</span></span>

    ![Doménových služeb – Stav zřizování](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="ea209-118">Klikněte na tlačítko **zabezpečení LDAP** v navigačním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="ea209-118">Click **Secure LDAP** on hello navigation pane.</span></span>

    ![Doménových služeb – okno zabezpečený LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. <span data-ttu-id="ea209-120">Zabezpečený LDAP přístup tooyour spravované domény je ve výchozím nastavení zakázaná.</span><span class="sxs-lookup"><span data-stu-id="ea209-120">By default, secure LDAP access tooyour managed domain is disabled.</span></span> <span data-ttu-id="ea209-121">Přepnutí **zabezpečení LDAP** příliš**povolit**.</span><span class="sxs-lookup"><span data-stu-id="ea209-121">Toggle **Secure LDAP** too**Enable**.</span></span>

    ![Povolit zabezpečený LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. <span data-ttu-id="ea209-123">Ve výchozím nastavení zabezpečeného přístupu tooyour LDAP spravované domény přes hello Internetu je zakázáno.</span><span class="sxs-lookup"><span data-stu-id="ea209-123">By default, secure LDAP access tooyour managed domain over hello internet is disabled.</span></span> <span data-ttu-id="ea209-124">Přepnutí **hello povolit zabezpečený LDAP přístup přes internet** příliš**povolit**, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="ea209-124">Toggle **Allow secure LDAP access over hello internet** too**Enable**, if desired.</span></span> 

6. <span data-ttu-id="ea209-125">Klikněte na následující ikona složky hello **. Soubor PFX s certifikátem zabezpečení LDAP**.</span><span class="sxs-lookup"><span data-stu-id="ea209-125">Click hello folder icon following **.PFX file with secure LDAP certificate**.</span></span> <span data-ttu-id="ea209-126">Zadejte soubor PFX toohello cesta hello s certifikátem hello zabezpečený LDAP přístup toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="ea209-126">Specify hello path toohello PFX file with hello certificate for secure LDAP access toohello managed domain.</span></span>

7. <span data-ttu-id="ea209-127">Zadejte hello **toodecrypt heslo. Soubor PFX**.</span><span class="sxs-lookup"><span data-stu-id="ea209-127">Specify hello **Password toodecrypt .PFX file**.</span></span> <span data-ttu-id="ea209-128">Zadejte hello stejné heslo, které jste použili při exportu souboru PFX toohello hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ea209-128">Provide hello same password you used when exporting hello certificate toohello PFX file.</span></span>

8. <span data-ttu-id="ea209-129">Až skončíte, klikněte na tlačítko hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ea209-129">When you are done, click hello **Save** button.</span></span>

9. <span data-ttu-id="ea209-130">Zobrazí oznámení, že uvidíte, že zabezpečený LDAP je konfigurován pro hello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="ea209-130">You see a notification that informs you secure LDAP is being configured for hello managed domain.</span></span> <span data-ttu-id="ea209-131">Až do dokončení této operace, nelze změnit další nastavení pro doménu hello.</span><span class="sxs-lookup"><span data-stu-id="ea209-131">Until this operation is complete, you cannot modify other settings for hello domain.</span></span>

    ![Konfigurace zabezpečeného LDAP pro spravované doméně hello](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> <span data-ttu-id="ea209-133">Jak dlouho trvá přibližně 10 minut too15 tooenable zabezpečený LDAP vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="ea209-133">It takes about 10 too15 minutes tooenable secure LDAP for your managed domain.</span></span> <span data-ttu-id="ea209-134">Pokud hello za předpokladu, že zabezpečený LDAP certifikát neodpovídá hello požadované kritéria, zabezpečený LDAP není povolena pro svůj adresář a zobrazit informace o selhání.</span><span class="sxs-lookup"><span data-stu-id="ea209-134">If hello provided secure LDAP certificate does not match hello required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="ea209-135">Například hello název domény není správný, hello certifikátu již vypršela nebo brzo vyprší.</span><span class="sxs-lookup"><span data-stu-id="ea209-135">For example, hello domain name is incorrect, hello certificate has already expired or expires soon.</span></span> <span data-ttu-id="ea209-136">V takovém případě zkuste to znovu s platným certifikátem.</span><span class="sxs-lookup"><span data-stu-id="ea209-136">In this case, retry with a valid certificate.</span></span>
>
>

<br>

## <a name="task-4---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a><span data-ttu-id="ea209-137">Úloha 4: Konfigurace DNS tooaccess hello spravované doméně z hello Internetu</span><span class="sxs-lookup"><span data-stu-id="ea209-137">Task 4 - configure DNS tooaccess hello managed domain from hello internet</span></span>
> [!NOTE]
> <span data-ttu-id="ea209-138">**Nepovinná úloha** – Pokud neplánujete tooaccess hello spravované doméně pomocí LDAPS přes hello internet, přeskočte tento úkol konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ea209-138">**Optional task** - If you do not plan tooaccess hello managed domain using LDAPS over hello internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="ea209-139">Než začnete tuto úlohu, zkontrolujte jste dokončili hello kroků uvedených v [úloha 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="ea209-139">Before you begin this task, ensure you have completed hello steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="ea209-140">Jakmile povolíte zabezpečený LDAP přístup přes internet hello vaší spravované domény, je nutné tooupdate DNS, aby klientské počítače najít této spravované domény.</span><span class="sxs-lookup"><span data-stu-id="ea209-140">Once you have enabled secure LDAP access over hello internet for your managed domain, you need tooupdate DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="ea209-141">Na konci hello úlohy 3, externí IP adresa se zobrazí na hello **vlastnosti** okno v **externí IP adresu pro LDAPS přístup**.</span><span class="sxs-lookup"><span data-stu-id="ea209-141">At hello end of task 3, an external IP address is displayed on hello **Properties** blade in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="ea209-142">Nakonfigurujte poskytovatele externí DNS tak, aby tento název DNS hello hello spravované domény (například "ldaps.contoso100.com') body toothis externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="ea209-142">Configure your external DNS provider so that hello DNS name of hello managed domain (for example, 'ldaps.contoso100.com') points toothis external IP address.</span></span> <span data-ttu-id="ea209-143">V našem příkladu potřebujeme toocreate hello následující položky DNS:</span><span class="sxs-lookup"><span data-stu-id="ea209-143">In our example, we need toocreate hello following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="ea209-144">Je to – jsou nyní připraven tooconnect toohello spravované doméně pomocí zabezpečený LDAP přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="ea209-144">That's it - you are now ready tooconnect toohello managed domain using secure LDAP over hello internet.</span></span>

> [!WARNING]
> <span data-ttu-id="ea209-145">Mějte na paměti, že klientské počítače musí důvěřovat hello vystavitele hello LDAPS certifikát toobe možné tooconnect úspěšně toohello spravované doméně pomocí LDAPS.</span><span class="sxs-lookup"><span data-stu-id="ea209-145">Remember that client computers must trust hello issuer of hello LDAPS certificate toobe able tooconnect successfully toohello managed domain using LDAPS.</span></span> <span data-ttu-id="ea209-146">Pokud používáte veřejně důvěryhodné certifikační autority, není nutné toodo nic vzhledem k tomu, že klientské počítače důvěřovat tyto vystavitelů certifikátů.</span><span class="sxs-lookup"><span data-stu-id="ea209-146">If you are using a publicly trusted certification authority, you do not need toodo anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="ea209-147">Pokud používáte certifikát podepsaný svým držitelem, nainstalujte hello veřejnou část hello certifikát podepsaný svým držitelem do úložiště důvěryhodných certifikátů hello na klientském počítači hello.</span><span class="sxs-lookup"><span data-stu-id="ea209-147">If you are using a self-signed certificate, install hello public part of hello self-signed certificate into hello trusted certificate store on hello client computer.</span></span>
>
>


## <a name="task-5---lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a><span data-ttu-id="ea209-148">Úloha 5: zamykání LDAPS přístup tooyour spravované hello domény přes internet</span><span class="sxs-lookup"><span data-stu-id="ea209-148">Task 5 - lock-down LDAPS access tooyour managed domain over hello internet</span></span>
> [!NOTE]
> <span data-ttu-id="ea209-149">**Nepovinná úloha** – Pokud jste nepovolili LDAPS spravované doméně toohello přístup přes hello internet, Přeskočit tuto úlohu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ea209-149">**Optional task** - If you have not enabled LDAPS access toohello managed domain over hello internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="ea209-150">Než začnete tuto úlohu, zkontrolujte jste dokončili hello kroků uvedených v [úloha 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="ea209-150">Before you begin this task, ensure you have completed hello steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="ea209-151">Vystavení vaší spravované domény pro LDAPS přístup přes hello internet představuje bezpečnostní riziko.</span><span class="sxs-lookup"><span data-stu-id="ea209-151">Exposing your managed domain for LDAPS access over hello internet represents a security threat.</span></span> <span data-ttu-id="ea209-152">Hello spravované domény je dostupný z hello Internetu v hello port používaný pro zabezpečený LDAP (to znamená, port 636).</span><span class="sxs-lookup"><span data-stu-id="ea209-152">hello managed domain is reachable from hello internet at hello port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="ea209-153">Proto můžete toorestrict přístup toohello spravované domény toospecific známé IP adresy.</span><span class="sxs-lookup"><span data-stu-id="ea209-153">Therefore, you can choose toorestrict access toohello managed domain toospecific known IP addresses.</span></span> <span data-ttu-id="ea209-154">Pro lepší zabezpečení vytvořte skupinu zabezpečení sítě (NSG) a přidružte ji k hello podsíť, kde jste povolili službu Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="ea209-154">For improved security, create a network security group (NSG) and associate it with hello subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="ea209-155">Hello následující tabulka znázorňuje ukázku NSG můžete nakonfigurovat, toolock dolů zabezpečený LDAP přístup přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="ea209-155">hello following table illustrates a sample NSG you can configure, toolock down secure LDAP access over hello internet.</span></span> <span data-ttu-id="ea209-156">Hello NSG obsahuje sadu pravidel, která povolí příchozí LDAPS přístup přes port TCP 636 pouze ze zadané sady IP adres.</span><span class="sxs-lookup"><span data-stu-id="ea209-156">hello NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="ea209-157">Hello výchozí 'DenyAll' pravidlo tooall další příchozí provoz z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="ea209-157">hello default 'DenyAll' rule applies tooall other inbound traffic from hello internet.</span></span> <span data-ttu-id="ea209-158">Hello NSG pravidlo tooallow LDAPS přístup prostřednictvím hello má vyšší prioritu než hello pravidla DenyAll NSG internet ze zadaných IP adres.</span><span class="sxs-lookup"><span data-stu-id="ea209-158">hello NSG rule tooallow LDAPS access over hello internet from specified IP addresses has a higher priority than hello DenyAll NSG rule.</span></span>

![Hello ukázka NSG toosecure LDAPS přístup prostřednictvím Internetu](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="ea209-160">**Další informace** - [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="ea209-160">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="ea209-161">Související obsah</span><span class="sxs-lookup"><span data-stu-id="ea209-161">Related content</span></span>
* [<span data-ttu-id="ea209-162">Azure AD Domain Services – Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="ea209-162">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="ea209-163">Správa spravované domény služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="ea209-163">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="ea209-164">Správa zásad skupiny na spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="ea209-164">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="ea209-165">Skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="ea209-165">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="ea209-166">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="ea209-166">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
