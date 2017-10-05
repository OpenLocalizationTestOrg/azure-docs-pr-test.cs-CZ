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
ms.openlocfilehash: 93afa49166c5b31d23237c308b9d34f6d6f3507d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="961f4-103">Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="961f4-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="961f4-104">Tento článek ukazuje, jak můžete povolit zabezpečené Lightweight Directory přístup protokolu (LDAPS) vaší spravované domény služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="961f4-104">This article shows how you can enable Secure Lightweight Directory Access Protocol (LDAPS) for your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="961f4-105">Zabezpečený LDAP se také označuje jako "přístup protokolu LDAP (Lightweight Directory) přes vrstvy SSL (Secure Sockets) nebo zabezpečení TLS (Transport Layer)'.</span><span class="sxs-lookup"><span data-stu-id="961f4-105">Secure LDAP is also known as 'Lightweight Directory Access Protocol (LDAP) over Secure Sockets Layer (SSL) / Transport Layer Security (TLS)'.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="961f4-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="961f4-106">Before you begin</span></span>
<span data-ttu-id="961f4-107">Chcete-li provést úkoly vypsané v tomto článku, je třeba:</span><span class="sxs-lookup"><span data-stu-id="961f4-107">To perform the tasks listed in this article, you need:</span></span>

1. <span data-ttu-id="961f4-108">Platná **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="961f4-108">A valid **Azure subscription**.</span></span>
2. <span data-ttu-id="961f4-109">**Adresář Azure AD** – buď synchronizovány s místní adresář nebo výhradně cloudový adresář.</span><span class="sxs-lookup"><span data-stu-id="961f4-109">An **Azure AD directory** - either synchronized with an on-premises directory or a cloud-only directory.</span></span>
3. <span data-ttu-id="961f4-110">**Azure AD Domain Services** musí být povolen pro adresář Azure AD.</span><span class="sxs-lookup"><span data-stu-id="961f4-110">**Azure AD Domain Services** must be enabled for the Azure AD directory.</span></span> <span data-ttu-id="961f4-111">Pokud jste tak dosud neučinili, postupujte podle všechny úkoly popsané v [příručce Začínáme](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="961f4-111">If you haven't done so, follow all the tasks outlined in the [Getting Started guide](active-directory-ds-getting-started.md).</span></span>
4. <span data-ttu-id="961f4-112">A **certifikát, který se použije k povolení zabezpečeného LDAP**.</span><span class="sxs-lookup"><span data-stu-id="961f4-112">A **certificate to be used to enable secure LDAP**.</span></span>

   * <span data-ttu-id="961f4-113">**Doporučená** – Získejte certifikát od důvěryhodné veřejné certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="961f4-113">**Recommended** - Obtain a certificate from a trusted public certification authority.</span></span> <span data-ttu-id="961f4-114">Tato možnost konfigurace je bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="961f4-114">This configuration option is more secure.</span></span>
   * <span data-ttu-id="961f4-115">Alternativně můžete taky rozhodnout k [vytvořit certifikát podepsaný svým držitelem](#task-1---obtain-a-certificate-for-secure-ldap) jak uvidíte později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="961f4-115">Alternately, you may also choose to [create a self-signed certificate](#task-1---obtain-a-certificate-for-secure-ldap) as shown later in this article.</span></span>

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a><span data-ttu-id="961f4-116">Požadavky pro zabezpečené certifikát protokolu LDAP</span><span class="sxs-lookup"><span data-stu-id="961f4-116">Requirements for the secure LDAP certificate</span></span>
<span data-ttu-id="961f4-117">Před povolením zabezpečený LDAP, získejte platný certifikát podle následujících pokynů.</span><span class="sxs-lookup"><span data-stu-id="961f4-117">Acquire a valid certificate per the following guidelines, before you enable secure LDAP.</span></span> <span data-ttu-id="961f4-118">Chyb narazíte, pokud se pokusíte povolit zabezpečený LDAP vaší spravované domény s neplatný nebo nesprávný certifikát.</span><span class="sxs-lookup"><span data-stu-id="961f4-118">You encounter failures if you try to enable secure LDAP for your managed domain with an invalid/incorrect certificate.</span></span>

1. <span data-ttu-id="961f4-119">**Důvěryhodného vystavitele** -certifikát musí být vydaný autoritu důvěřují počítače připojující se k spravované doméně pomocí zabezpečený LDAP.</span><span class="sxs-lookup"><span data-stu-id="961f4-119">**Trusted issuer** - The certificate must be issued by an authority trusted by computers connecting to the managed domain using secure LDAP.</span></span> <span data-ttu-id="961f4-120">Tato autorita může být veřejné certifikační autority tyto počítače důvěřují.</span><span class="sxs-lookup"><span data-stu-id="961f4-120">This authority may be a public certification authority trusted by these computers.</span></span>
2. <span data-ttu-id="961f4-121">**Doba platnosti** -certifikát musí být platný pro další 3 až 6 měsíců.</span><span class="sxs-lookup"><span data-stu-id="961f4-121">**Lifetime** - The certificate must be valid for at least the next 3-6 months.</span></span> <span data-ttu-id="961f4-122">Zabezpečený LDAP přístup k vaší spravované domény dojde k narušení při vypršení platnosti certifikátu.</span><span class="sxs-lookup"><span data-stu-id="961f4-122">Secure LDAP access to your managed domain is disrupted when the certificate expires.</span></span>
3. <span data-ttu-id="961f4-123">**Název subjektu** -název předmětu na certifikátu musí být zástupný znak vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="961f4-123">**Subject name** - The subject name on the certificate must be a wildcard for your managed domain.</span></span> <span data-ttu-id="961f4-124">Například pokud název domény "contoso100.com", název subjektu certifikátu musí být: *. contoso100.com ".</span><span class="sxs-lookup"><span data-stu-id="961f4-124">For instance, if your domain is named 'contoso100.com', the certificate's subject name must be '*.contoso100.com'.</span></span> <span data-ttu-id="961f4-125">Nastavte název DNS (alternativní název subjektu) na tento název zástupný znak.</span><span class="sxs-lookup"><span data-stu-id="961f4-125">Set the DNS name (subject alternate name) to this wildcard name.</span></span>
4. <span data-ttu-id="961f4-126">**Použití klíče** -certifikát musí být nakonfigurované pro následující používá – digitální podpisy i šifrování klíče.</span><span class="sxs-lookup"><span data-stu-id="961f4-126">**Key usage** - The certificate must be configured for the following uses - Digital signatures and key encipherment.</span></span>
5. <span data-ttu-id="961f4-127">**Účel certifikátu** -certifikát musí být platný pro ověření serveru SSL.</span><span class="sxs-lookup"><span data-stu-id="961f4-127">**Certificate purpose** - The certificate must be valid for SSL server authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="961f4-128">**Podnikové certifikační autority:** Azure AD Domain Services nepodporuje použití zabezpečeného LDAP certifikátů vystavených certifikační autoritou rozlehlé sítě vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="961f4-128">**Enterprise Certification Authorities:** Azure AD Domain Services does not support using secure LDAP certificates issued by your organization's enterprise certification authority.</span></span> <span data-ttu-id="961f4-129">Toto omezení je, protože služba nedůvěřuje certifikační Autority jako certifikát kořenové certifikační autority rozlehlé sítě.</span><span class="sxs-lookup"><span data-stu-id="961f4-129">This restriction is because the service does not trust your enterprise CA as a root certification authority.</span></span> 
>
>

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a><span data-ttu-id="961f4-130">Úloha 1 – získání certifikátu pro zabezpečený LDAP</span><span class="sxs-lookup"><span data-stu-id="961f4-130">Task 1 - obtain a certificate for secure LDAP</span></span>
<span data-ttu-id="961f4-131">První úlohou zahrnuje získání certifikátu pro zabezpečený přístup protokolu LDAP k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="961f4-131">The first task involves obtaining a certificate used for secure LDAP access to the managed domain.</span></span> <span data-ttu-id="961f4-132">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="961f4-132">You have two options:</span></span>

* <span data-ttu-id="961f4-133">Získejte certifikát od certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="961f4-133">Obtain a certificate from a certification authority.</span></span> <span data-ttu-id="961f4-134">Oprávnění může být veřejné certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="961f4-134">The authority may be a public certification authority.</span></span>
* <span data-ttu-id="961f4-135">Vytvořte certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="961f4-135">Create a self-signed certificate.</span></span>

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a><span data-ttu-id="961f4-136">Možnost (doporučeno) - získat zabezpečený LDAP certifikát od certifikační autority</span><span class="sxs-lookup"><span data-stu-id="961f4-136">Option A (Recommended) - Obtain a secure LDAP certificate from a certification authority</span></span>
<span data-ttu-id="961f4-137">Pokud vaše organizace obdrží svoje certifikáty od veřejné certifikační autority, musíte získat certifikát zabezpečený LDAP od této veřejné certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="961f4-137">If your organization obtains its certificates from a public certification authority, you need to obtain the secure LDAP certificate from that public certification authority.</span></span>

<span data-ttu-id="961f4-138">Při žádosti o certifikát, ujistěte se, že jste splňují všechny požadavky uvedené v [zabezpečený LDAP požadavky](#requirements-for-the-secure-ldap-certificate).</span><span class="sxs-lookup"><span data-stu-id="961f4-138">When requesting a certificate, ensure that you satisfy all the requirements outlined in [Requirements for the secure LDAP certificate](#requirements-for-the-secure-ldap-certificate).</span></span>

> [!NOTE]
> <span data-ttu-id="961f4-139">Klientské počítače, které je třeba se připojit k spravované doméně pomocí zabezpečený LDAP musí důvěřovat Vystavitel certifikátu zabezpečení protokolu LDAP.</span><span class="sxs-lookup"><span data-stu-id="961f4-139">Client computers that need to connect to the managed domain using secure LDAP must trust the issuer of the secure LDAP certificate.</span></span>
>
>

### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a><span data-ttu-id="961f4-140">Možnost B - vytvořit certifikát podepsaný svým držitelem pro zabezpečený LDAP</span><span class="sxs-lookup"><span data-stu-id="961f4-140">Option B - Create a self-signed certificate for secure LDAP</span></span>
<span data-ttu-id="961f4-141">Pokud neočekáváte používat certifikát od veřejné certifikační autority, můžete se rozhodnout vytvořit certifikát podepsaný svým držitelem pro zabezpečený LDAP.</span><span class="sxs-lookup"><span data-stu-id="961f4-141">If you do not expect to use a certificate from a public certification authority, you may choose to create a self-signed certificate for secure LDAP.</span></span>

<span data-ttu-id="961f4-142">**Vytvořit certifikát podepsaný svým držitelem pomocí prostředí PowerShell**</span><span class="sxs-lookup"><span data-stu-id="961f4-142">**Create a self-signed certificate using PowerShell**</span></span>

<span data-ttu-id="961f4-143">Na počítači s Windows, otevřete nové okno prostředí PowerShell jako **správce** a zadejte následující příkazy, chcete-li vytvořit nový certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="961f4-143">On your Windows computer, open a new PowerShell window as **Administrator** and type the following commands, to create a new self-signed certificate.</span></span>

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

<span data-ttu-id="961f4-144">V předchozím příkladu nahraďte '*. contoso100.com "s název domény DNS vaší spravované domény. For example, pokud jste vytvořili spravované doméně nazvané 'contoso100.onmicrosoft.com', nahraďte '*. contoso100.com "ve výše uvedené skriptu s ' *. contoso100.onmicrosoft.com').</span><span class="sxs-lookup"><span data-stu-id="961f4-144">In the preceding sample, replace '*.contoso100.com' with the DNS domain name of your managed domain. For example, if you created a managed domain called 'contoso100.onmicrosoft.com', replace '*.contoso100.com' in the above script with '*.contoso100.onmicrosoft.com').</span></span>

![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

<span data-ttu-id="961f4-146">Nově vytvořený certifikát podepsaný svým držitelem se umístí do úložiště certifikátů místního počítače.</span><span class="sxs-lookup"><span data-stu-id="961f4-146">The newly created self-signed certificate is placed in the local machine's certificate store.</span></span>


## <a name="next-step"></a><span data-ttu-id="961f4-147">Další krok</span><span class="sxs-lookup"><span data-stu-id="961f4-147">Next step</span></span>
[<span data-ttu-id="961f4-148">Úloha 2 – zabezpečený LDAP certifikát, který chcete exportovat. Soubor PFX</span><span class="sxs-lookup"><span data-stu-id="961f4-148">Task 2 - export the secure LDAP certificate to a .PFX file</span></span>](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
