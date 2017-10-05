---
title: "<span data-ttu-id=\"0394f-101\">Použití certifikátů s Enterprise integračního balíčku | Microsoft Docs</span><span class=\"sxs-lookup\"><span data-stu-id=\"0394f-101\">Using certificates with Enterprise Integration Pack | Microsoft Docs</span></span>"
description: "<span data-ttu-id=\"0394f-102\">Další informace o použití certifikátů s Enterprise integračního balíčku | Azure Logic Apps</span><span class=\"sxs-lookup\"><span data-stu-id=\"0394f-102\">Learn how to use certificates with the Enterprise Integration Pack | Azure Logic Apps</span></span>"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 0570aab14283b38f9efcc50636f0c0c1c8e3ed13
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a><span data-ttu-id="0394f-103">Další informace o certifikátech a řešení Enterprise Integration Pack</span><span class="sxs-lookup"><span data-stu-id="0394f-103">Learn about certificates and Enterprise Integration Pack</span></span>
## <a name="overview"></a><span data-ttu-id="0394f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="0394f-104">Overview</span></span>
<span data-ttu-id="0394f-105">Integrace Enterprise používá certifikáty k zabezpečení komunikace B2B.</span><span class="sxs-lookup"><span data-stu-id="0394f-105">Enterprise integration uses certificates to secure B2B communications.</span></span> <span data-ttu-id="0394f-106">Můžete provádět dva typy certifikátů ve vaší podnikové integrace aplikace:</span><span class="sxs-lookup"><span data-stu-id="0394f-106">You can use two types of certificates in your enterprise integration apps:</span></span>

* <span data-ttu-id="0394f-107">Veřejné certifikáty, které je třeba zakoupit od certifikační autority (CA).</span><span class="sxs-lookup"><span data-stu-id="0394f-107">Public certificates, which must be purchased from a certification authority (CA).</span></span>
* <span data-ttu-id="0394f-108">Privátní certifikáty, které můžete zadat sami.</span><span class="sxs-lookup"><span data-stu-id="0394f-108">Private certificates, which you can issue yourself.</span></span> <span data-ttu-id="0394f-109">Tyto certifikáty se někdy označují jako certifikáty podepsané svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="0394f-109">These certificates are sometimes referred to as self-signed certificates.</span></span>

## <a name="what-are-certificates"></a><span data-ttu-id="0394f-110">Co jsou certifikáty?</span><span class="sxs-lookup"><span data-stu-id="0394f-110">What are certificates?</span></span>
<span data-ttu-id="0394f-111">Certifikáty jsou digitální dokumenty, ověření identity účastníky v elektronické komunikace a která taky zabezpečené elektronické komunikace.</span><span class="sxs-lookup"><span data-stu-id="0394f-111">Certificates are digital documents that verify the identity of the participants in electronic communications and that also secure electronic communications.</span></span>

## <a name="why-use-certificates"></a><span data-ttu-id="0394f-112">Proč používat certifikáty?</span><span class="sxs-lookup"><span data-stu-id="0394f-112">Why use certificates?</span></span>
<span data-ttu-id="0394f-113">Někdy komunikace B2B musí být důvěrné.</span><span class="sxs-lookup"><span data-stu-id="0394f-113">Sometimes B2B communications must be kept confidential.</span></span> <span data-ttu-id="0394f-114">Integrace Enterprise používá certifikáty k zabezpečení této komunikace dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="0394f-114">Enterprise integration uses certificates to secure these communications in two ways:</span></span>

* <span data-ttu-id="0394f-115">Šifrování obsahu zpráv</span><span class="sxs-lookup"><span data-stu-id="0394f-115">By encrypting the contents of messages</span></span>
* <span data-ttu-id="0394f-116">Podle digitálnímu podepisování zpráv</span><span class="sxs-lookup"><span data-stu-id="0394f-116">By digitally signing messages</span></span>  

## <a name="upload-a-public-certificate"></a><span data-ttu-id="0394f-117">Nahrát veřejného certifikátu</span><span class="sxs-lookup"><span data-stu-id="0394f-117">Upload a public certificate</span></span>

<span data-ttu-id="0394f-118">Použít *veřejný certifikát* ve vašich logic apps možnosti B2B, musíte nejprve nahrát na server certifikát ke svému účtu integrace.</span><span class="sxs-lookup"><span data-stu-id="0394f-118">To use a *public certificate* in your logic apps with B2B capabilities, you first need to upload the certificate into your integration account.</span></span>  

<span data-ttu-id="0394f-119">Po odeslání certifikát, je k dispozici a pomáhá vám zabezpečit zpráv B2B, když definujete jejich vlastnosti v [smlouvy](logic-apps-enterprise-integration-agreements.md) , kterou vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="0394f-119">After you upload a certificate, it's available to help you secure your B2B messages when you define their properties in the [agreements](logic-apps-enterprise-integration-agreements.md) that you create.</span></span>  

<span data-ttu-id="0394f-120">Tady jsou podrobné kroky pro nahrávání veřejné certifikáty do účtu integrace po přihlášení k portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="0394f-120">Here are the detailed steps for uploading your public certificates into your integration account after you sign in to the Azure portal:</span></span>

1. <span data-ttu-id="0394f-121">Vyberte **další služby** a zadejte **integrace** do vyhledávacího pole filtru.</span><span class="sxs-lookup"><span data-stu-id="0394f-121">Select **More services** and enter **integration** in the filter search box.</span></span> <span data-ttu-id="0394f-122">Vyberte **účty pro integraci** ze seznamu výsledků</span><span class="sxs-lookup"><span data-stu-id="0394f-122">Select **Integration Accounts** from the results list</span></span>     
<span data-ttu-id="0394f-123">![Vyberte procházení](media/logic-apps-enterprise-integration-certificates/overview-1.png)</span><span class="sxs-lookup"><span data-stu-id="0394f-123">![Select Browse](media/logic-apps-enterprise-integration-certificates/overview-1.png)</span></span>  
2. <span data-ttu-id="0394f-124">Vyberte účet integrace, do které chcete přidat certifikát.</span><span class="sxs-lookup"><span data-stu-id="0394f-124">Select the integration account to which you want to add the certificate.</span></span>  
![Vyberte účet integrace, do které chcete přidat certifikát](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. <span data-ttu-id="0394f-126">Vyberte **certifikáty** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="0394f-126">Select the **Certificates** tile.</span></span>  
<span data-ttu-id="0394f-127">![Vyberte dlaždici certifikáty](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="0394f-127">![Select the Certificates tile](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span></span>
4. <span data-ttu-id="0394f-128">V **certifikáty** okno, které se otevře, vyberte **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0394f-128">In the **Certificates** blade that opens, select the **Add** button.</span></span>   
<span data-ttu-id="0394f-129">![Kliknutím na tlačítko Přidat](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="0394f-129">![Select the Add button](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span></span>
5. <span data-ttu-id="0394f-130">Zadejte **název** certifikát, a pak vyberte certifikát, zadejte jako **veřejné** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0394f-130">Enter a **Name** for your certificate, and then select the certificate type as **public** from the dropdown.</span></span>  
6. <span data-ttu-id="0394f-131">Vyberte ikonu složky na pravé straně **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0394f-131">Select the folder icon on the right side of the **Certificate** text box.</span></span> <span data-ttu-id="0394f-132">Po otevření nástroje pro výběr souborů, najděte a vyberte soubor certifikátu, který chcete odeslat do svého účtu integrace.</span><span class="sxs-lookup"><span data-stu-id="0394f-132">When the file picker opens, find and select the certificate file that you want to upload to your integration account.</span></span>
7. <span data-ttu-id="0394f-133">Vyberte certifikát a pak vyberte **OK** v nástroje pro výběr souborů.</span><span class="sxs-lookup"><span data-stu-id="0394f-133">Select the certificate, and then select **OK** in the file picker.</span></span> <span data-ttu-id="0394f-134">To se ověří a odešle certifikát ke svému účtu integrace.</span><span class="sxs-lookup"><span data-stu-id="0394f-134">This validates and uploads the certificate to your integration account.</span></span>
8. <span data-ttu-id="0394f-135">Nakonec zpět na **přidat certifikát** okně, vyberte **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0394f-135">Finally, back on the **Add certificate** blade, select the **OK** button.</span></span>  
<span data-ttu-id="0394f-136">![Vyberte tlačítko OK](media/logic-apps-enterprise-integration-certificates/certificate-3.png)</span><span class="sxs-lookup"><span data-stu-id="0394f-136">![Select the OK button](media/logic-apps-enterprise-integration-certificates/certificate-3.png)</span></span>  
9. <span data-ttu-id="0394f-137">Vyberte **certifikáty** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="0394f-137">Select the **Certificates** tile.</span></span> <span data-ttu-id="0394f-138">Měli byste vidět nově přidané certifikát.</span><span class="sxs-lookup"><span data-stu-id="0394f-138">You should see the newly added certificate.</span></span>  
<span data-ttu-id="0394f-139">![V tématu nový certifikát](media/logic-apps-enterprise-integration-certificates/certificate-4.png)</span><span class="sxs-lookup"><span data-stu-id="0394f-139">![See the new certificate](media/logic-apps-enterprise-integration-certificates/certificate-4.png)</span></span>  

## <a name="upload-a-private-certificate"></a><span data-ttu-id="0394f-140">Nahrát privátní certifikát</span><span class="sxs-lookup"><span data-stu-id="0394f-140">Upload a private certificate</span></span>

<span data-ttu-id="0394f-141">Použít *privátní certifikát* ve vašich logic apps s B2B možnosti, můžete odeslat privátní certifikát do svého účtu integrace provedením následujících kroků</span><span class="sxs-lookup"><span data-stu-id="0394f-141">To use a *private certificate* in your logic apps with B2B capabilities, You can upload a private certificate to your integration account by taking the following steps</span></span>

1. <span data-ttu-id="0394f-142">[Nahrát svůj privátní klíč do Key Vault](../key-vault/key-vault-get-started.md "Další informace o Key Vault") a poskytují **název klíče**</span><span class="sxs-lookup"><span data-stu-id="0394f-142">[Upload your private key to Key Vault](../key-vault/key-vault-get-started.md "Learn about Key Vault") and provide a **Key Name**</span></span> 
   
   > [!TIP]
   > <span data-ttu-id="0394f-143">Je nutné autorizovat Logic Apps k provádění operací v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0394f-143">You must authorize Logic Apps to perform operations on Key Vault.</span></span> <span data-ttu-id="0394f-144">Objekt služby Logic Apps můžete poskytnout přístup pomocí následujícího příkazu Powershellu:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`</span><span class="sxs-lookup"><span data-stu-id="0394f-144">You can grant access to the Logic Apps service principal by using the following PowerShell command: `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`</span></span>  
   > 
   > 

<span data-ttu-id="0394f-145">Poté, co jste jste udělali v předchozím kroku, přidejte privátní certifikát k integraci účtu.</span><span class="sxs-lookup"><span data-stu-id="0394f-145">After you've taken the previous step, add a private certificate to integration account.</span></span>

<span data-ttu-id="0394f-146">Následující části jsou podrobné kroky pro nahrávání privátní certifikáty do účtu integrace po přihlášení k portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="0394f-146">Following are the detailed steps for uploading your private certificates into your integration account after you sign in to the Azure portal:</span></span>  
 
1. <span data-ttu-id="0394f-147">Vyberte účet integrace, do které chcete přidat certifikát a vyberte **certifikáty** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="0394f-147">Select the integration account to which you want to add the certificate and select the **Certificates** tile.</span></span>  
<span data-ttu-id="0394f-148">![Vyberte dlaždici certifikáty](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="0394f-148">![Select the Certificates tile](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span></span>  
2. <span data-ttu-id="0394f-149">V **certifikáty** okno, které se otevře, vyberte **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0394f-149">In the **Certificates** blade that opens, select the **Add** button.</span></span>   
<span data-ttu-id="0394f-150">![Kliknutím na tlačítko Přidat](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="0394f-150">![Select the Add button](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span></span>
3. <span data-ttu-id="0394f-151">Zadejte **název** certifikát, a vyberte certifikát, zadejte jako **privátní** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0394f-151">Enter a **Name** for your certificate, and select the certificate type as **private** from the dropdown.</span></span>   
4. <span data-ttu-id="0394f-152">Vyberte ikonu složky na pravé straně **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0394f-152">select the folder icon on the right side of the **Certificate** text box.</span></span> <span data-ttu-id="0394f-153">Po otevření nástroje pro výběr souborů, najděte odpovídající veřejný certifikát, který chcete odeslat do svého účtu integrace.</span><span class="sxs-lookup"><span data-stu-id="0394f-153">When the file picker opens, find the corresponding public certificate that you want to upload to your integration account.</span></span>   
   
   > [!Note]
   > <span data-ttu-id="0394f-154">Při přidávání privátního certifikátu je potřeba přidat odpovídající veřejný certifikát, které se zobrazí v [smlouvy AS2](logic-apps-enterprise-integration-as2.md) příjem a odesílání nastavení pro podepisování a šifrování zpráv.</span><span class="sxs-lookup"><span data-stu-id="0394f-154">While adding a private certificate it is important to add corresponding public certificate to show in [AS2 agreement](logic-apps-enterprise-integration-as2.md) receive and send settings for signing and encrypting the messages.</span></span>
   > 
   >   

5. <span data-ttu-id="0394f-155">Vyberte **skupiny prostředků**, **Key Vault**, **název klíče** a vyberte **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0394f-155">Select the **Resource Group**, **Key Vault**, **Key Name** and select the **OK** button.</span></span>  
<span data-ttu-id="0394f-156">![Přidání certifikátu](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="0394f-156">![Add certificate](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)</span></span>  
6. <span data-ttu-id="0394f-157">Vyberte **certifikáty** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="0394f-157">Select the **Certificates** tile.</span></span> <span data-ttu-id="0394f-158">Měli byste vidět nově přidané certifikát.</span><span class="sxs-lookup"><span data-stu-id="0394f-158">You should see the newly added certificate.</span></span>
<span data-ttu-id="0394f-159">![V tématu nový certifikát](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="0394f-159">![See the new certificate](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)</span></span>  



* [<span data-ttu-id="0394f-160">Vytvoření smlouvy s B2B</span><span class="sxs-lookup"><span data-stu-id="0394f-160">Create a B2B agreement</span></span>](logic-apps-enterprise-integration-agreements.md)  
* [<span data-ttu-id="0394f-161">Další informace o Key Vault</span><span class="sxs-lookup"><span data-stu-id="0394f-161">Learn more about Key Vault</span></span>](../key-vault/key-vault-get-started.md "Další informace o Key Vault")  

