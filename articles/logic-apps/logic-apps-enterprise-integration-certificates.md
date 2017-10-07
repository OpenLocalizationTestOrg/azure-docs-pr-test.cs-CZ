---
title: "aaaUsing certifikáty s Enterprise integračního balíčku | Microsoft Docs"
description: "Zjistěte, jak toouse certifikáty s hello Enterprise integračního balíčku | Azure Logic Apps"
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
ms.openlocfilehash: 7ba9f597a03a852a9ba05d2af08fe4bc2d98aef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a><span data-ttu-id="ea321-103">Další informace o certifikátech a řešení Enterprise Integration Pack</span><span class="sxs-lookup"><span data-stu-id="ea321-103">Learn about certificates and Enterprise Integration Pack</span></span>
## <a name="overview"></a><span data-ttu-id="ea321-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="ea321-104">Overview</span></span>
<span data-ttu-id="ea321-105">Integrace Enterprise používá komunikace B2B toosecure certifikáty.</span><span class="sxs-lookup"><span data-stu-id="ea321-105">Enterprise integration uses certificates toosecure B2B communications.</span></span> <span data-ttu-id="ea321-106">Můžete provádět dva typy certifikátů ve vaší podnikové integrace aplikace:</span><span class="sxs-lookup"><span data-stu-id="ea321-106">You can use two types of certificates in your enterprise integration apps:</span></span>

* <span data-ttu-id="ea321-107">Veřejné certifikáty, které je třeba zakoupit od certifikační autority (CA).</span><span class="sxs-lookup"><span data-stu-id="ea321-107">Public certificates, which must be purchased from a certification authority (CA).</span></span>
* <span data-ttu-id="ea321-108">Privátní certifikáty, které můžete zadat sami.</span><span class="sxs-lookup"><span data-stu-id="ea321-108">Private certificates, which you can issue yourself.</span></span> <span data-ttu-id="ea321-109">Tyto certifikáty jsou někdy označují tooas certifikáty podepsané svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="ea321-109">These certificates are sometimes referred tooas self-signed certificates.</span></span>

## <a name="what-are-certificates"></a><span data-ttu-id="ea321-110">Co jsou certifikáty?</span><span class="sxs-lookup"><span data-stu-id="ea321-110">What are certificates?</span></span>
<span data-ttu-id="ea321-111">Certifikáty jsou digitální dokumenty, ověřit, zda text hello identita hello účastníky v elektronické komunikace a která taky zabezpečené elektronické komunikace.</span><span class="sxs-lookup"><span data-stu-id="ea321-111">Certificates are digital documents that verify hello identity of hello participants in electronic communications and that also secure electronic communications.</span></span>

## <a name="why-use-certificates"></a><span data-ttu-id="ea321-112">Proč používat certifikáty?</span><span class="sxs-lookup"><span data-stu-id="ea321-112">Why use certificates?</span></span>
<span data-ttu-id="ea321-113">Někdy komunikace B2B musí být důvěrné.</span><span class="sxs-lookup"><span data-stu-id="ea321-113">Sometimes B2B communications must be kept confidential.</span></span> <span data-ttu-id="ea321-114">Integrace Enterprise používá certifikáty toosecure tyto komunikace dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="ea321-114">Enterprise integration uses certificates toosecure these communications in two ways:</span></span>

* <span data-ttu-id="ea321-115">Šifrování hello obsah zprávy</span><span class="sxs-lookup"><span data-stu-id="ea321-115">By encrypting hello contents of messages</span></span>
* <span data-ttu-id="ea321-116">Podle digitálnímu podepisování zpráv</span><span class="sxs-lookup"><span data-stu-id="ea321-116">By digitally signing messages</span></span>  

## <a name="upload-a-public-certificate"></a><span data-ttu-id="ea321-117">Nahrát veřejného certifikátu</span><span class="sxs-lookup"><span data-stu-id="ea321-117">Upload a public certificate</span></span>

<span data-ttu-id="ea321-118">toouse *veřejný certifikát* ve vašich logic apps možnosti B2B, musíte nejprve tooupload hello certifikátu do účtu integrace.</span><span class="sxs-lookup"><span data-stu-id="ea321-118">toouse a *public certificate* in your logic apps with B2B capabilities, you first need tooupload hello certificate into your integration account.</span></span>  

<span data-ttu-id="ea321-119">Po odeslání certifikát, je k dispozici toohelp zabezpečení zpráv B2B při definování jejich vlastnosti v hello [smlouvy](logic-apps-enterprise-integration-agreements.md) , kterou vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="ea321-119">After you upload a certificate, it's available toohelp you secure your B2B messages when you define their properties in hello [agreements](logic-apps-enterprise-integration-agreements.md) that you create.</span></span>  

<span data-ttu-id="ea321-120">Tady jsou hello podrobné kroky pro nahrávání veřejné certifikáty do účtu integrace po přihlášení toohello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="ea321-120">Here are hello detailed steps for uploading your public certificates into your integration account after you sign in toohello Azure portal:</span></span>

1. <span data-ttu-id="ea321-121">Vyberte **další služby** a zadejte **integrace** hello filtru vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="ea321-121">Select **More services** and enter **integration** in hello filter search box.</span></span> <span data-ttu-id="ea321-122">Vyberte **účty pro integraci** ze seznamu výsledků hello</span><span class="sxs-lookup"><span data-stu-id="ea321-122">Select **Integration Accounts** from hello results list</span></span>     
<span data-ttu-id="ea321-123">![Vyberte procházení](media/logic-apps-enterprise-integration-certificates/overview-1.png)</span><span class="sxs-lookup"><span data-stu-id="ea321-123">![Select Browse](media/logic-apps-enterprise-integration-certificates/overview-1.png)</span></span>  
2. <span data-ttu-id="ea321-124">Vyberte hello integrace účet toowhich chcete tooadd hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ea321-124">Select hello integration account toowhich you want tooadd hello certificate.</span></span>  
![Vyberte hello integrace účet toowhich chcete tooadd hello certifikátu](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. <span data-ttu-id="ea321-126">Vyberte hello **certifikáty** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="ea321-126">Select hello **Certificates** tile.</span></span>  
<span data-ttu-id="ea321-127">![Dlaždice certifikáty vyberte hello](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="ea321-127">![Select hello Certificates tile](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span></span>
4. <span data-ttu-id="ea321-128">V hello **certifikáty** okno, které se otevře, vyberte hello **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ea321-128">In hello **Certificates** blade that opens, select hello **Add** button.</span></span>   
<span data-ttu-id="ea321-129">![Kliknutím na tlačítko Přidat hello](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="ea321-129">![Select hello Add button](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span></span>
5. <span data-ttu-id="ea321-130">Zadejte **název** certifikát, a pak vyberte hello typ certifikátu jako **veřejné** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="ea321-130">Enter a **Name** for your certificate, and then select hello certificate type as **public** from hello dropdown.</span></span>  
6. <span data-ttu-id="ea321-131">Vyberte hello ikonou složky na pravé straně hello hello **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ea321-131">Select hello folder icon on hello right side of hello **Certificate** text box.</span></span> <span data-ttu-id="ea321-132">Když se otevře okno pro výběr souboru hello, najděte a vyberte soubor certifikátu hello, které chcete tooupload tooyour integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="ea321-132">When hello file picker opens, find and select hello certificate file that you want tooupload tooyour integration account.</span></span>
7. <span data-ttu-id="ea321-133">Vyberte certifikát hello a pak vyberte **OK** v hello pro výběr souborů.</span><span class="sxs-lookup"><span data-stu-id="ea321-133">Select hello certificate, and then select **OK** in hello file picker.</span></span> <span data-ttu-id="ea321-134">To ověří a odesílá je účet integrace tooyour hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ea321-134">This validates and uploads hello certificate tooyour integration account.</span></span>
8. <span data-ttu-id="ea321-135">Nakonec zpět na hello **přidat certifikát** okně, vyberte hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ea321-135">Finally, back on hello **Add certificate** blade, select hello **OK** button.</span></span>  
<span data-ttu-id="ea321-136">![Kliknutím na tlačítko OK hello](media/logic-apps-enterprise-integration-certificates/certificate-3.png)</span><span class="sxs-lookup"><span data-stu-id="ea321-136">![Select hello OK button](media/logic-apps-enterprise-integration-certificates/certificate-3.png)</span></span>  
9. <span data-ttu-id="ea321-137">Vyberte hello **certifikáty** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="ea321-137">Select hello **Certificates** tile.</span></span> <span data-ttu-id="ea321-138">Měli byste vidět, že hello nově přidaná certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ea321-138">You should see hello newly added certificate.</span></span>  
<span data-ttu-id="ea321-139">![V tématu hello nový certifikát](media/logic-apps-enterprise-integration-certificates/certificate-4.png)</span><span class="sxs-lookup"><span data-stu-id="ea321-139">![See hello new certificate](media/logic-apps-enterprise-integration-certificates/certificate-4.png)</span></span>  

## <a name="upload-a-private-certificate"></a><span data-ttu-id="ea321-140">Nahrát privátní certifikát</span><span class="sxs-lookup"><span data-stu-id="ea321-140">Upload a private certificate</span></span>

<span data-ttu-id="ea321-141">toouse *privátní certifikát* ve vašich logic apps s B2B možnosti, můžete nahrát účet privátní certifikát tooyour integrace podle trvá hello následující kroky</span><span class="sxs-lookup"><span data-stu-id="ea321-141">toouse a *private certificate* in your logic apps with B2B capabilities, You can upload a private certificate tooyour integration account by taking hello following steps</span></span>

1. <span data-ttu-id="ea321-142">[Nahrát vaší privátní klíče tooKey trezoru](../key-vault/key-vault-get-started.md "Další informace o Key Vault") a poskytují **název klíče**</span><span class="sxs-lookup"><span data-stu-id="ea321-142">[Upload your private key tooKey Vault](../key-vault/key-vault-get-started.md "Learn about Key Vault") and provide a **Key Name**</span></span> 
   
   > [!TIP]
   > <span data-ttu-id="ea321-143">Je nutné autorizovat Logic Apps tooperform operace v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="ea321-143">You must authorize Logic Apps tooperform operations on Key Vault.</span></span> <span data-ttu-id="ea321-144">Můžete udělit přístup toohello Logic Apps instanční objekt pomocí hello následující příkaz prostředí PowerShell:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`</span><span class="sxs-lookup"><span data-stu-id="ea321-144">You can grant access toohello Logic Apps service principal by using hello following PowerShell command: `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`</span></span>  
   > 
   > 

<span data-ttu-id="ea321-145">Poté, co jste prováděné hello předchozím kroku, přidáte účet toointegration privátní certifikát.</span><span class="sxs-lookup"><span data-stu-id="ea321-145">After you've taken hello previous step, add a private certificate toointegration account.</span></span>

<span data-ttu-id="ea321-146">Podrobné kroky pro nahrávání privátní certifikáty do účtu integrace po přihlášení toohello portálu Azure jsou hello následující:</span><span class="sxs-lookup"><span data-stu-id="ea321-146">Following are hello detailed steps for uploading your private certificates into your integration account after you sign in toohello Azure portal:</span></span>  
 
1. <span data-ttu-id="ea321-147">Vyberte hello integrace účet toowhich tooadd hello certifikátu a zaškrtněte hello **certifikáty** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="ea321-147">Select hello integration account toowhich you want tooadd hello certificate and select hello **Certificates** tile.</span></span>  
<span data-ttu-id="ea321-148">![Dlaždice certifikáty vyberte hello](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="ea321-148">![Select hello Certificates tile](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span></span>  
2. <span data-ttu-id="ea321-149">V hello **certifikáty** okno, které se otevře, vyberte hello **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ea321-149">In hello **Certificates** blade that opens, select hello **Add** button.</span></span>   
<span data-ttu-id="ea321-150">![Kliknutím na tlačítko Přidat hello](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="ea321-150">![Select hello Add button](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span></span>
3. <span data-ttu-id="ea321-151">Zadejte **název** certifikát, a typ certifikátu vyberte hello jako **privátní** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="ea321-151">Enter a **Name** for your certificate, and select hello certificate type as **private** from hello dropdown.</span></span>   
4. <span data-ttu-id="ea321-152">Vyberte ikonu složky hello na pravé straně hello hello **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ea321-152">select hello folder icon on hello right side of hello **Certificate** text box.</span></span> <span data-ttu-id="ea321-153">Když se otevře okno pro výběr souboru hello, najdete hello odpovídající veřejný certifikát, který má tooupload tooyour integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="ea321-153">When hello file picker opens, find hello corresponding public certificate that you want tooupload tooyour integration account.</span></span>   
   
   > [!Note]
   > <span data-ttu-id="ea321-154">Při přidávání privátního certifikátu je důležité tooadd odpovídající veřejný certifikát tooshow v [smlouvy AS2](logic-apps-enterprise-integration-as2.md) příjem a odesílání nastavení pro podepisování a šifrování zpráv hello.</span><span class="sxs-lookup"><span data-stu-id="ea321-154">While adding a private certificate it is important tooadd corresponding public certificate tooshow in [AS2 agreement](logic-apps-enterprise-integration-as2.md) receive and send settings for signing and encrypting hello messages.</span></span>
   > 
   >   

5. <span data-ttu-id="ea321-155">Vyberte hello **skupiny prostředků**, **Key Vault**, **název klíče** a vyberte hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ea321-155">Select hello **Resource Group**, **Key Vault**, **Key Name** and select hello **OK** button.</span></span>  
<span data-ttu-id="ea321-156">![Přidání certifikátu](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="ea321-156">![Add certificate](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)</span></span>  
6. <span data-ttu-id="ea321-157">Vyberte hello **certifikáty** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="ea321-157">Select hello **Certificates** tile.</span></span> <span data-ttu-id="ea321-158">Měli byste vidět, že hello nově přidaná certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ea321-158">You should see hello newly added certificate.</span></span>
<span data-ttu-id="ea321-159">![V tématu hello nový certifikát](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="ea321-159">![See hello new certificate](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)</span></span>  



* [<span data-ttu-id="ea321-160">Vytvoření smlouvy s B2B</span><span class="sxs-lookup"><span data-stu-id="ea321-160">Create a B2B agreement</span></span>](logic-apps-enterprise-integration-agreements.md)  
* [<span data-ttu-id="ea321-161">Další informace o Key Vault</span><span class="sxs-lookup"><span data-stu-id="ea321-161">Learn more about Key Vault</span></span>](../key-vault/key-vault-get-started.md "Další informace o Key Vault")  

