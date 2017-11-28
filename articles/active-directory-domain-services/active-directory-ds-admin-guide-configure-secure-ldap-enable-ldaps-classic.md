---
title: "aaaConfigure zabezpečeného LDAP (LDAPS) v Azure AD Domain Services | Microsoft Docs"
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
ms.openlocfilehash: a0d6e2faf474b1f0cbe157fb4ae2754b1d521ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="87913-103">Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="87913-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="87913-104">Než začnete</span><span class="sxs-lookup"><span data-stu-id="87913-104">Before you begin</span></span>
<span data-ttu-id="87913-105">Ujistěte se, když jste dokončili [úloha 2 – export hello zabezpečený LDAP certifikát tooa. Soubor PFX](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="87913-105">Ensure you've completed [Task 2 - export hello secure LDAP certificate tooa .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="87913-106">Vyberte, zda toouse hello práci s portálem Azure preview nebo hello Azure classic portálu toocomplete této úlohy.</span><span class="sxs-lookup"><span data-stu-id="87913-106">Choose whether toouse hello preview Azure portal experience or hello Azure classic portal toocomplete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="87913-107">**Portál Azure (Preview)**: [povolit zabezpečený LDAP pomocí hello portálu Azure](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="87913-107">**Azure portal (Preview)**: [Enable secure LDAP using hello Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="87913-108">**Portál Azure classic**: [povolit zabezpečený LDAP pomocí portálu Azure classic hello](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="87913-108">**Azure classic portal**: [Enable secure LDAP using hello classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-classic-azure-portal"></a><span data-ttu-id="87913-109">Úloha 3 – povolení zabezpečeného LDAP pro hello spravované doméně pomocí portálu Azure classic hello</span><span class="sxs-lookup"><span data-stu-id="87913-109">Task 3 - enable secure LDAP for hello managed domain using hello classic Azure portal</span></span>
<span data-ttu-id="87913-110">tooenable zabezpečený LDAP, proveďte následující kroky konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="87913-110">tooenable secure LDAP, perform hello following configuration steps:</span></span>

1. <span data-ttu-id="87913-111">Přejděte toohello  **[portál Azure classic](https://manage.windowsazure.com)**.</span><span class="sxs-lookup"><span data-stu-id="87913-111">Navigate toohello **[Azure classic portal](https://manage.windowsazure.com)**.</span></span>
2. <span data-ttu-id="87913-112">Vyberte hello **služby Active Directory** uzlu v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="87913-112">Select hello **Active Directory** node on hello left pane.</span></span>
3. <span data-ttu-id="87913-113">Vyberte adresář hello Azure AD (také odkazované tooas "klienta"), pro kterou jste povolili službu Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="87913-113">Select hello Azure AD directory (also referred tooas 'tenant'), for which you have enabled Azure AD Domain Services.</span></span>

    ![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="87913-115">Klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="87913-115">Click hello **Configure** tab.</span></span>

    ![Karta konfigurace adresáře](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. <span data-ttu-id="87913-117">Projděte dolů toohello části s názvem **služby domény**.</span><span class="sxs-lookup"><span data-stu-id="87913-117">Scroll down toohello section titled **domain services**.</span></span> <span data-ttu-id="87913-118">Měli byste vidět možnost s názvem **zabezpečení protokolu LDAP (LDAPS)** jak ukazuje následující snímek obrazovky hello:</span><span class="sxs-lookup"><span data-stu-id="87913-118">You should see an option titled **Secure LDAP (LDAPS)** as shown in hello following screenshot:</span></span>

    ![Oddíl konfigurace doménových služeb](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. <span data-ttu-id="87913-120">Klikněte na tlačítko hello **konfigurovat certifikát...**  toobring tlačítko nahoru hello **konfigurace certifikátu pro zabezpečené LDAP** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87913-120">Click hello **Configure certificate ...** button toobring up hello **Configure Certificate for Secure LDAP** dialog.</span></span>

    ![Konfigurace certifikátu pro zabezpečený LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. <span data-ttu-id="87913-122">Klikněte na následující ikona složky hello **certifikát pomocí souboru PFX** toospecify hello PFX souboru, který obsahuje certifikát hello chcete toouse pro zabezpečený LDAP přístup toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="87913-122">Click hello folder icon following **PFX FILE WITH CERTIFICATE** toospecify hello PFX file, which contains hello certificate you wish toouse for secure LDAP access toohello managed domain.</span></span> <span data-ttu-id="87913-123">Zadejte také hello heslo, které jste zadali při exportu souboru PFX toohello hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="87913-123">Also enter hello password you specified when exporting hello certificate toohello PFX file.</span></span> <span data-ttu-id="87913-124">Potom klikněte na provést na dolní hello tlačítko hello.</span><span class="sxs-lookup"><span data-stu-id="87913-124">Then, click hello done button on hello bottom.</span></span>

    ![Zadejte soubor zabezpečený LDAP PFX a heslo](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. <span data-ttu-id="87913-126">Hello **domény služby** části hello **konfigurace** by měl získat šedě a je v hello **čekající na vyřízení...**  stavu pro několik minut.</span><span class="sxs-lookup"><span data-stu-id="87913-126">hello **domain services** section of hello **Configure** tab should get grayed out and is in hello **Pending...** state for a few minutes.</span></span> <span data-ttu-id="87913-127">Během této doby je hello LDAPS certifikát ověřit přesnost a zabezpečený LDAP je nakonfigurována pro vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="87913-127">During this period, hello LDAPS certificate is verified for accuracy and secure LDAP is configured for your managed domain.</span></span>

    ![Zabezpečený LDAP - čekající na vyřízení stavu](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="87913-129">Jak dlouho trvá přibližně 10 minut too15 tooenable zabezpečený LDAP vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="87913-129">It takes about 10 too15 minutes tooenable secure LDAP for your managed domain.</span></span> <span data-ttu-id="87913-130">Pokud hello za předpokladu, že zabezpečený LDAP certifikát neodpovídá hello požadované kritéria, zabezpečený LDAP není povolena pro svůj adresář a zobrazit informace o selhání.</span><span class="sxs-lookup"><span data-stu-id="87913-130">If hello provided secure LDAP certificate does not match hello required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="87913-131">Například hello název domény není správný, hello certifikátu již vypršela nebo brzo vyprší.</span><span class="sxs-lookup"><span data-stu-id="87913-131">For example, hello domain name is incorrect, hello certificate has already expired or expires soon.</span></span>
   >
   >

9. <span data-ttu-id="87913-132">Když zabezpečený LDAP úspěšně povolen vaší spravované domény, hello **čekající na vyřízení...**  by zpráva zmizí.</span><span class="sxs-lookup"><span data-stu-id="87913-132">When secure LDAP is successfully enabled for your managed domain, hello **Pending...** message should disappear.</span></span> <span data-ttu-id="87913-133">Měli byste vidět hello kryptografický otisk certifikátu hello zobrazí.</span><span class="sxs-lookup"><span data-stu-id="87913-133">You should see hello thumbprint of hello certificate displayed.</span></span>

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-hello-internet"></a><span data-ttu-id="87913-135">Úloha 4 – povolení zabezpečeného přístupu LDAP přes hello Internetu</span><span class="sxs-lookup"><span data-stu-id="87913-135">Task 4 - enable secure LDAP access over hello internet</span></span>
<span data-ttu-id="87913-136">**Nepovinná úloha** – Pokud neplánujete tooaccess hello spravované doméně pomocí LDAPS přes hello internet, přeskočte tento úkol konfigurace.</span><span class="sxs-lookup"><span data-stu-id="87913-136">**Optional task** - If you do not plan tooaccess hello managed domain using LDAPS over hello internet, skip this configuration task.</span></span>

<span data-ttu-id="87913-137">Než začnete tuto úlohu, zkontrolujte jste dokončili hello kroků uvedených v [úloha 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="87913-137">Before you begin this task, ensure you have completed hello steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span></span>

1. <span data-ttu-id="87913-138">Měli byste vidět možnost příliš**hello povolit ZABEZPEČENÉ LDAP přístup přes INTERNET** v hello **domény služby** části hello **konfigurace** stránky.</span><span class="sxs-lookup"><span data-stu-id="87913-138">You should see an option too**ENABLE SECURE LDAP ACCESS OVER hello INTERNET** in hello **domain services** section of hello **Configure** page.</span></span> <span data-ttu-id="87913-139">Tato možnost nastavená příliš**ne** ve výchozím nastavení vzhledem k tomu, že toohello přístup k Internetu spravované domény přes zabezpečený LDAP ve výchozím nastavení vypnutá.</span><span class="sxs-lookup"><span data-stu-id="87913-139">This option is set too**NO** by default since internet access toohello managed domain over secure LDAP is disabled by default.</span></span>

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. <span data-ttu-id="87913-141">Přepnutí **hello povolit ZABEZPEČENÉ LDAP přístup přes INTERNET** příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="87913-141">Toggle **ENABLE SECURE LDAP ACCESS OVER hello INTERNET** too**YES**.</span></span> <span data-ttu-id="87913-142">Klikněte na tlačítko hello **Uložit** tlačítko na dolním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="87913-142">Click hello **SAVE** button on hello bottom panel.</span></span>
    <span data-ttu-id="87913-143">![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span><span class="sxs-lookup"><span data-stu-id="87913-143">![Secure LDAP enabled](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span></span>
3. <span data-ttu-id="87913-144">Hello **domény služby** části hello **konfigurace** by měl získat šedě a je v hello **čekající na vyřízení...**  stavu pro několik minut.</span><span class="sxs-lookup"><span data-stu-id="87913-144">hello **domain services** section of hello **Configure** tab should get grayed out and is in hello **Pending...** state for a few minutes.</span></span> <span data-ttu-id="87913-145">Po určité době je povoleno spravované domény tooyour internet přístup přes zabezpečený LDAP.</span><span class="sxs-lookup"><span data-stu-id="87913-145">After some time, internet access tooyour managed domain over secure LDAP is enabled.</span></span>

    ![Zabezpečený LDAP - čekající na vyřízení stavu](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="87913-147">Jak dlouho trvá přibližně 10 minut tooenable přístup k Internetu přes zabezpečený LDAP vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="87913-147">It takes about 10 minutes tooenable internet access over secure LDAP for your managed domain.</span></span>
   >
   >
4. <span data-ttu-id="87913-148">Pokud zabezpečeného přístupu tooyour LDAP spravované domény přes hello Internetu byl úspěšně povolen, hello **čekající na vyřízení...**  by zpráva zmizí.</span><span class="sxs-lookup"><span data-stu-id="87913-148">When secure LDAP access tooyour managed domain over hello internet is successfully enabled, hello **Pending...** message should disappear.</span></span> <span data-ttu-id="87913-149">Měli byste vidět hello externí IP adresu, kterou se dá použít tooaccess adresáře prostřednictvím LDAPS v poli hello **externí IP adresu pro LDAPS přístup**.</span><span class="sxs-lookup"><span data-stu-id="87913-149">You should see hello external IP address that can be used tooaccess your directory over LDAPS in hello field **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a><span data-ttu-id="87913-151">Úloha 5: Konfigurace DNS tooaccess hello spravované doméně z hello Internetu</span><span class="sxs-lookup"><span data-stu-id="87913-151">Task 5 - configure DNS tooaccess hello managed domain from hello internet</span></span>
<span data-ttu-id="87913-152">**Nepovinná úloha** – Pokud neplánujete tooaccess hello spravované doméně pomocí LDAPS přes hello internet, přeskočte tento úkol konfigurace.</span><span class="sxs-lookup"><span data-stu-id="87913-152">**Optional task** - If you do not plan tooaccess hello managed domain using LDAPS over hello internet, skip this configuration task.</span></span>

<span data-ttu-id="87913-153">Než začnete tuto úlohu, zkontrolujte jste dokončili hello kroků uvedených v [úloha 4](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="87913-153">Before you begin this task, ensure you have completed hello steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="87913-154">Jakmile povolíte zabezpečený LDAP přístup přes internet hello vaší spravované domény, je nutné tooupdate DNS, aby klientské počítače najít této spravované domény.</span><span class="sxs-lookup"><span data-stu-id="87913-154">Once you have enabled secure LDAP access over hello internet for your managed domain, you need tooupdate DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="87913-155">Na konci hello úlohy 4, externí IP adresa se zobrazí na hello **konfigurace** kartě v **externí IP adresu pro LDAPS přístup**.</span><span class="sxs-lookup"><span data-stu-id="87913-155">At hello end of task 4, an external IP address is displayed on hello **Configure** tab in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="87913-156">Nakonfigurujte poskytovatele externí DNS tak, aby tento název DNS hello hello spravované domény (například "ldaps.contoso100.com') body toothis externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="87913-156">Configure your external DNS provider so that hello DNS name of hello managed domain (for example, 'ldaps.contoso100.com') points toothis external IP address.</span></span> <span data-ttu-id="87913-157">V našem příkladu potřebujeme toocreate hello následující položky DNS:</span><span class="sxs-lookup"><span data-stu-id="87913-157">In our example, we need toocreate hello following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="87913-158">Je to – jsou nyní připraven tooconnect toohello spravované doméně pomocí zabezpečený LDAP přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="87913-158">That's it - you are now ready tooconnect toohello managed domain using secure LDAP over hello internet.</span></span>

> [!WARNING]
> <span data-ttu-id="87913-159">Mějte na paměti, že klientské počítače musí důvěřovat hello vystavitele hello LDAPS certifikát toobe možné tooconnect úspěšně toohello spravované doméně pomocí LDAPS.</span><span class="sxs-lookup"><span data-stu-id="87913-159">Remember that client computers must trust hello issuer of hello LDAPS certificate toobe able tooconnect successfully toohello managed domain using LDAPS.</span></span> <span data-ttu-id="87913-160">Pokud používáte certifikační autoritu organizace nebo veřejně důvěryhodné certifikační autority, není nutné toodo nic vzhledem k tomu, že klientské počítače důvěřovat tyto vystavitelů certifikátů.</span><span class="sxs-lookup"><span data-stu-id="87913-160">If you are using an enterprise certification authority or a publicly trusted certification authority, you do not need toodo anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="87913-161">Pokud používáte certifikát podepsaný svým držitelem, je třeba tooinstall hello veřejnou část hello certifikát podepsaný svým držitelem do úložiště důvěryhodných certifikátů hello na klientském počítači hello.</span><span class="sxs-lookup"><span data-stu-id="87913-161">If you are using a self-signed certificate, you need tooinstall hello public part of hello self-signed certificate into hello trusted certificate store on hello client computer.</span></span>
>
>


## <a name="lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a><span data-ttu-id="87913-162">Uzamčení LDAPS přístup ke spravované doméně tooyour přes hello Internetu</span><span class="sxs-lookup"><span data-stu-id="87913-162">Lock-down LDAPS access tooyour managed domain over hello internet</span></span>
> [!NOTE]
> <span data-ttu-id="87913-163">**Nepovinná úloha** – Pokud jste nepovolili LDAPS spravované doméně toohello přístup přes hello internet, Přeskočit tuto úlohu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="87913-163">**Optional task** - If you have not enabled LDAPS access toohello managed domain over hello internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="87913-164">Než začnete tuto úlohu, zkontrolujte jste dokončili hello kroků uvedených v [úloha 4](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="87913-164">Before you begin this task, ensure you have completed hello steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="87913-165">Vystavení vaší spravované domény pro LDAPS přístup přes hello internet představuje bezpečnostní riziko.</span><span class="sxs-lookup"><span data-stu-id="87913-165">Exposing your managed domain for LDAPS access over hello internet represents a security threat.</span></span> <span data-ttu-id="87913-166">Hello spravované domény je dostupný z hello Internetu v hello port používaný pro zabezpečený LDAP (to znamená, port 636).</span><span class="sxs-lookup"><span data-stu-id="87913-166">hello managed domain is reachable from hello internet at hello port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="87913-167">Proto můžete toorestrict přístup toohello spravované domény toospecific známé IP adresy.</span><span class="sxs-lookup"><span data-stu-id="87913-167">Therefore, you can choose toorestrict access toohello managed domain toospecific known IP addresses.</span></span> <span data-ttu-id="87913-168">Pro lepší zabezpečení vytvořte skupinu zabezpečení sítě (NSG) a přidružte ji k hello podsíť, kde jste povolili službu Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="87913-168">For improved security, create a network security group (NSG) and associate it with hello subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="87913-169">Hello následující tabulka znázorňuje ukázku NSG můžete nakonfigurovat, toolock dolů zabezpečený LDAP přístup přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="87913-169">hello following table illustrates a sample NSG you can configure, toolock down secure LDAP access over hello internet.</span></span> <span data-ttu-id="87913-170">Hello NSG obsahuje sadu pravidel, která povolí příchozí LDAPS přístup přes port TCP 636 pouze ze zadané sady IP adres.</span><span class="sxs-lookup"><span data-stu-id="87913-170">hello NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="87913-171">Hello výchozí 'DenyAll' pravidlo tooall další příchozí provoz z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="87913-171">hello default 'DenyAll' rule applies tooall other inbound traffic from hello internet.</span></span> <span data-ttu-id="87913-172">Hello NSG pravidlo tooallow LDAPS přístup prostřednictvím hello má vyšší prioritu než hello pravidla DenyAll NSG internet ze zadaných IP adres.</span><span class="sxs-lookup"><span data-stu-id="87913-172">hello NSG rule tooallow LDAPS access over hello internet from specified IP addresses has a higher priority than hello DenyAll NSG rule.</span></span>

![Hello ukázka NSG toosecure LDAPS přístup prostřednictvím Internetu](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="87913-174">**Další informace** - [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="87913-174">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="87913-175">Související obsah</span><span class="sxs-lookup"><span data-stu-id="87913-175">Related content</span></span>
* [<span data-ttu-id="87913-176">Azure AD Domain Services – Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="87913-176">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="87913-177">Správa spravované domény služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="87913-177">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="87913-178">Správa zásad skupiny na spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="87913-178">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="87913-179">Skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="87913-179">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="87913-180">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="87913-180">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
