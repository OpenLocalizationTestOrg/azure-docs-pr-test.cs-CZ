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
ms.openlocfilehash: 99e44d917b115d7f7a67ff0a5e22c5c9165287e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="21fc4-103">Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="21fc4-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="21fc4-104">Tento článek ukazuje, jak můžete povolit zabezpečené Lightweight Directory přístup protokolu (LDAPS) vaší spravované domény služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="21fc4-104">This article shows how you can enable Secure Lightweight Directory Access Protocol (LDAPS) for your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="21fc4-105">Zabezpečený LDAP se také označuje jako "přístup protokolu LDAP (Lightweight Directory) přes vrstvy SSL (Secure Sockets) nebo zabezpečení TLS (Transport Layer)'.</span><span class="sxs-lookup"><span data-stu-id="21fc4-105">Secure LDAP is also known as 'Lightweight Directory Access Protocol (LDAP) over Secure Sockets Layer (SSL) / Transport Layer Security (TLS)'.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="21fc4-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="21fc4-106">Before you begin</span></span>
<span data-ttu-id="21fc4-107">úlohy hello tooperform uvedené v tomto článku, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="21fc4-107">tooperform hello tasks listed in this article, you need:</span></span>

1. <span data-ttu-id="21fc4-108">Platná **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="21fc4-108">A valid **Azure subscription**.</span></span>
2. <span data-ttu-id="21fc4-109">**Adresář Azure AD** – buď synchronizovány s místní adresář nebo výhradně cloudový adresář.</span><span class="sxs-lookup"><span data-stu-id="21fc4-109">An **Azure AD directory** - either synchronized with an on-premises directory or a cloud-only directory.</span></span>
3. <span data-ttu-id="21fc4-110">**Azure AD Domain Services** musí být povolen pro adresář hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="21fc4-110">**Azure AD Domain Services** must be enabled for hello Azure AD directory.</span></span> <span data-ttu-id="21fc4-111">Pokud jste tak dosud neučinili, postupujte podle kroků uvedených v hello všechny hello úlohy [příručce Začínáme](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="21fc4-111">If you haven't done so, follow all hello tasks outlined in hello [Getting Started guide](active-directory-ds-getting-started.md).</span></span>
4. <span data-ttu-id="21fc4-112">A **tooenable toobe používá certifikát zabezpečený LDAP**.</span><span class="sxs-lookup"><span data-stu-id="21fc4-112">A **certificate toobe used tooenable secure LDAP**.</span></span>

   * <span data-ttu-id="21fc4-113">**Doporučená** – Získejte certifikát od důvěryhodné veřejné certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="21fc4-113">**Recommended** - Obtain a certificate from a trusted public certification authority.</span></span> <span data-ttu-id="21fc4-114">Tato možnost konfigurace je bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="21fc4-114">This configuration option is more secure.</span></span>
   * <span data-ttu-id="21fc4-115">Alternativně můžete také příliš[vytvořit certifikát podepsaný svým držitelem](#task-1---obtain-a-certificate-for-secure-ldap) jak uvidíte později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="21fc4-115">Alternately, you may also choose too[create a self-signed certificate](#task-1---obtain-a-certificate-for-secure-ldap) as shown later in this article.</span></span>

<br>

### <a name="requirements-for-hello-secure-ldap-certificate"></a><span data-ttu-id="21fc4-116">Požadavky pro hello zabezpečený LDAP certifikát</span><span class="sxs-lookup"><span data-stu-id="21fc4-116">Requirements for hello secure LDAP certificate</span></span>
<span data-ttu-id="21fc4-117">Získejte platný certifikát na hello následující pokyny, dříve než povolíte zabezpečený LDAP.</span><span class="sxs-lookup"><span data-stu-id="21fc4-117">Acquire a valid certificate per hello following guidelines, before you enable secure LDAP.</span></span> <span data-ttu-id="21fc4-118">Pokud se pokusíte tooenable dojde k selhání zabezpečený LDAP vaší spravované domény s neplatný nebo nesprávný certifikát.</span><span class="sxs-lookup"><span data-stu-id="21fc4-118">You encounter failures if you try tooenable secure LDAP for your managed domain with an invalid/incorrect certificate.</span></span>

1. <span data-ttu-id="21fc4-119">**Důvěryhodného vystavitele** -hello certifikát musí být vydaný autoritu důvěřují počítače připojující se toohello spravované doméně pomocí zabezpečený LDAP.</span><span class="sxs-lookup"><span data-stu-id="21fc4-119">**Trusted issuer** - hello certificate must be issued by an authority trusted by computers connecting toohello managed domain using secure LDAP.</span></span> <span data-ttu-id="21fc4-120">Tato autorita může být veřejné certifikační autority tyto počítače důvěřují.</span><span class="sxs-lookup"><span data-stu-id="21fc4-120">This authority may be a public certification authority trusted by these computers.</span></span>
2. <span data-ttu-id="21fc4-121">**Doba platnosti** -hello certifikát musí být platný pro alespoň hello další 3 až 6 měsíců.</span><span class="sxs-lookup"><span data-stu-id="21fc4-121">**Lifetime** - hello certificate must be valid for at least hello next 3-6 months.</span></span> <span data-ttu-id="21fc4-122">Zabezpečený LDAP tooyour spravované domény přístupu dojde k narušení když vyprší platnost certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="21fc4-122">Secure LDAP access tooyour managed domain is disrupted when hello certificate expires.</span></span>
3. <span data-ttu-id="21fc4-123">**Název subjektu** -hello název předmětu na certifikátu hello musí být zástupný znak vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="21fc4-123">**Subject name** - hello subject name on hello certificate must be a wildcard for your managed domain.</span></span> <span data-ttu-id="21fc4-124">Například pokud název domény "contoso100.com", název subjektu certifikátu hello musí být: *. contoso100.com ".</span><span class="sxs-lookup"><span data-stu-id="21fc4-124">For instance, if your domain is named 'contoso100.com', hello certificate's subject name must be '*.contoso100.com'.</span></span> <span data-ttu-id="21fc4-125">Nastavit hello DNS (alternativní název subjektu) toothis zástupný název.</span><span class="sxs-lookup"><span data-stu-id="21fc4-125">Set hello DNS name (subject alternate name) toothis wildcard name.</span></span>
4. <span data-ttu-id="21fc4-126">**Použití klíče** - hello certifikát musí být nakonfigurovaný pro následující hello používá – digitální podpisy i šifrování klíče.</span><span class="sxs-lookup"><span data-stu-id="21fc4-126">**Key usage** - hello certificate must be configured for hello following uses - Digital signatures and key encipherment.</span></span>
5. <span data-ttu-id="21fc4-127">**Účel certifikátu** -hello certifikát musí být platný pro ověření serveru SSL.</span><span class="sxs-lookup"><span data-stu-id="21fc4-127">**Certificate purpose** - hello certificate must be valid for SSL server authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="21fc4-128">**Podnikové certifikační autority:** Azure AD Domain Services nepodporuje použití zabezpečeného LDAP certifikátů vystavených certifikační autoritou rozlehlé sítě vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="21fc4-128">**Enterprise Certification Authorities:** Azure AD Domain Services does not support using secure LDAP certificates issued by your organization's enterprise certification authority.</span></span> <span data-ttu-id="21fc4-129">Toto omezení se totiž hello služby nedůvěřuje certifikační Autority jako certifikát kořenové certifikační autority rozlehlé sítě.</span><span class="sxs-lookup"><span data-stu-id="21fc4-129">This restriction is because hello service does not trust your enterprise CA as a root certification authority.</span></span> 
>
>

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a><span data-ttu-id="21fc4-130">Úloha 1 – získání certifikátu pro zabezpečený LDAP</span><span class="sxs-lookup"><span data-stu-id="21fc4-130">Task 1 - obtain a certificate for secure LDAP</span></span>
<span data-ttu-id="21fc4-131">první úlohou Hello zahrnuje získání certifikátu pro zabezpečený LDAP přístup toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="21fc4-131">hello first task involves obtaining a certificate used for secure LDAP access toohello managed domain.</span></span> <span data-ttu-id="21fc4-132">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="21fc4-132">You have two options:</span></span>

* <span data-ttu-id="21fc4-133">Získejte certifikát od certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="21fc4-133">Obtain a certificate from a certification authority.</span></span> <span data-ttu-id="21fc4-134">autorita Hello může být veřejné certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="21fc4-134">hello authority may be a public certification authority.</span></span>
* <span data-ttu-id="21fc4-135">Vytvořte certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="21fc4-135">Create a self-signed certificate.</span></span>

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a><span data-ttu-id="21fc4-136">Možnost (doporučeno) - získat zabezpečený LDAP certifikát od certifikační autority</span><span class="sxs-lookup"><span data-stu-id="21fc4-136">Option A (Recommended) - Obtain a secure LDAP certificate from a certification authority</span></span>
<span data-ttu-id="21fc4-137">Pokud vaše organizace obdrží svoje certifikáty od veřejné certifikační autority, budete potřebovat tooobtain hello zabezpečený LDAP certifikát od této veřejné certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="21fc4-137">If your organization obtains its certificates from a public certification authority, you need tooobtain hello secure LDAP certificate from that public certification authority.</span></span>

<span data-ttu-id="21fc4-138">Při žádosti o certifikát, ujistěte se, že jste splňují všechny požadavky hello uvedených v [požadavky hello zabezpečený LDAP certifikát](#requirements-for-the-secure-ldap-certificate).</span><span class="sxs-lookup"><span data-stu-id="21fc4-138">When requesting a certificate, ensure that you satisfy all hello requirements outlined in [Requirements for hello secure LDAP certificate](#requirements-for-the-secure-ldap-certificate).</span></span>

> [!NOTE]
> <span data-ttu-id="21fc4-139">Klientské počítače, které je třeba tooconnect toohello spravované doméně pomocí zabezpečený LDAP musí důvěřovat hello vystavitele certifikátu zabezpečeného LDAP hello.</span><span class="sxs-lookup"><span data-stu-id="21fc4-139">Client computers that need tooconnect toohello managed domain using secure LDAP must trust hello issuer of hello secure LDAP certificate.</span></span>
>
>

### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a><span data-ttu-id="21fc4-140">Možnost B - vytvořit certifikát podepsaný svým držitelem pro zabezpečený LDAP</span><span class="sxs-lookup"><span data-stu-id="21fc4-140">Option B - Create a self-signed certificate for secure LDAP</span></span>
<span data-ttu-id="21fc4-141">Pokud neočekáváte toouse certifikát od veřejné certifikační autority, můžete se rozhodnout toocreate certifikát podepsaný svým držitelem pro zabezpečený LDAP.</span><span class="sxs-lookup"><span data-stu-id="21fc4-141">If you do not expect toouse a certificate from a public certification authority, you may choose toocreate a self-signed certificate for secure LDAP.</span></span>

<span data-ttu-id="21fc4-142">**Vytvořit certifikát podepsaný svým držitelem pomocí prostředí PowerShell**</span><span class="sxs-lookup"><span data-stu-id="21fc4-142">**Create a self-signed certificate using PowerShell**</span></span>

<span data-ttu-id="21fc4-143">Na počítači s Windows, otevřete nové okno prostředí PowerShell jako **správce** a typ hello následující příkazy, toocreate nový certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="21fc4-143">On your Windows computer, open a new PowerShell window as **Administrator** and type hello following commands, toocreate a new self-signed certificate.</span></span>

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

<span data-ttu-id="21fc4-144">V předchozích ukázkových hello, nahraďte '*. contoso100.com "hello název domény DNS s vaší spravované domény. For example, pokud jste vytvořili spravované doméně nazvané 'contoso100.onmicrosoft.com', nahraďte '*. contoso100.com "v hello výše skriptu s ' *. contoso100.onmicrosoft.com').</span><span class="sxs-lookup"><span data-stu-id="21fc4-144">In hello preceding sample, replace '*.contoso100.com' with hello DNS domain name of your managed domain. For example, if you created a managed domain called 'contoso100.onmicrosoft.com', replace '*.contoso100.com' in hello above script with '*.contoso100.onmicrosoft.com').</span></span>

![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

<span data-ttu-id="21fc4-146">Hello nově vytvořený certifikát podepsaný svým držitelem se umístí do úložiště certifikátů hello místního počítače.</span><span class="sxs-lookup"><span data-stu-id="21fc4-146">hello newly created self-signed certificate is placed in hello local machine's certificate store.</span></span>


## <a name="next-step"></a><span data-ttu-id="21fc4-147">Další krok</span><span class="sxs-lookup"><span data-stu-id="21fc4-147">Next step</span></span>
[<span data-ttu-id="21fc4-148">Úloha 2 – export hello tooa certifikát zabezpečený LDAP. Soubor PFX</span><span class="sxs-lookup"><span data-stu-id="21fc4-148">Task 2 - export hello secure LDAP certificate tooa .PFX file</span></span>](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
