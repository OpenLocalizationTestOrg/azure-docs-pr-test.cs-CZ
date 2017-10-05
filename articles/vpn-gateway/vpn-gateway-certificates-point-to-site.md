---
title: "Generování a exportování certifikátů pro Point-to-Site: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento článek obsahuje kroky k vytvoření certifikátu podepsaného svým držitelem kořenové, exportujte veřejný klíč a generovat klientské certifikáty pomocí prostředí PowerShell ve Windows 10."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 27b99f7c-50dc-4f88-8a6e-d60080819a43
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: f96b9b212b9322d0677e49ff95184d0feccca2df
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10"></a><span data-ttu-id="20472-103">Generování a exportování certifikátů pro připojení Point-to-Site pomocí prostředí PowerShell ve Windows 10</span><span class="sxs-lookup"><span data-stu-id="20472-103">Generate and export certificates for Point-to-Site connections using PowerShell on Windows 10</span></span>

<span data-ttu-id="20472-104">Připojení point-to-Site používají certifikáty k ověření.</span><span class="sxs-lookup"><span data-stu-id="20472-104">Point-to-Site connections use certificates to authenticate.</span></span> <span data-ttu-id="20472-105">Tento článek ukazuje, jak vytvořit certifikát podepsaný svým držitelem kořenové a generovat klientské certifikáty pomocí prostředí PowerShell ve Windows 10.</span><span class="sxs-lookup"><span data-stu-id="20472-105">This article shows you how to create a self-signed root certificate and generate client certificates using PowerShell on Windows 10.</span></span> <span data-ttu-id="20472-106">Pokud hledáte kroků konfigurace Point-to-Site, například jak nahrát kořenových certifikátů, vyberte jeden z článků ' konfigurace Point-to-Site, z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="20472-106">If you are looking for Point-to-Site configuration steps, such as how to upload root certificates, select one of the 'Configure Point-to-Site' articles from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="20472-107">Vytvořit certifikáty podepsané svým držitelem – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="20472-107">Create self-signed certificates - PowerShell</span></span>](vpn-gateway-certificates-point-to-site.md)
> * [<span data-ttu-id="20472-108">Vytvořit certifikáty podepsané svým držitelem - MakeCert</span><span class="sxs-lookup"><span data-stu-id="20472-108">Create self-signed certificates - MakeCert</span></span>](vpn-gateway-certificates-point-to-site-makecert.md)
> * [<span data-ttu-id="20472-109">Konfigurace Point-to-Site - Resource Manager – portál Azure</span><span class="sxs-lookup"><span data-stu-id="20472-109">Configure Point-to-Site - Resource Manager - Azure portal</span></span>](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="20472-110">Konfigurace Point-to-Site - Resource Manager – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="20472-110">Configure Point-to-Site - Resource Manager - PowerShell</span></span>](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [<span data-ttu-id="20472-111">Konfigurace Point-to-Site - Classic - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="20472-111">Configure Point-to-Site - Classic - Azure portal</span></span>](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


<span data-ttu-id="20472-112">Musíte provést kroky v tomto článku v počítači se systémem Windows 10.</span><span class="sxs-lookup"><span data-stu-id="20472-112">You must perform the steps in this article on a computer running Windows 10.</span></span> <span data-ttu-id="20472-113">Rutiny prostředí PowerShell, které používáte pro generování certifikátů jsou součástí operačního systému Windows 10 a nefungují v jiných verzích Windows.</span><span class="sxs-lookup"><span data-stu-id="20472-113">The PowerShell cmdlets that you use to generate certificates are part of the Windows 10 operating system and do not work on other versions of Windows.</span></span> <span data-ttu-id="20472-114">Počítače Windows 10 je potřeba jenom pro generování certifikátů.</span><span class="sxs-lookup"><span data-stu-id="20472-114">The Windows 10 computer is only needed to generate the certificates.</span></span> <span data-ttu-id="20472-115">Po vygenerování certifikátů jsou můžete odešlete, nebo je nainstalovat pro všechny podporované klientské operační systémy.</span><span class="sxs-lookup"><span data-stu-id="20472-115">Once the certificates are generated, you can upload them, or install them on any supported client operating system.</span></span> 

<span data-ttu-id="20472-116">Pokud nemáte přístup k počítači s Windows 10, můžete použít [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) pro generování certifikátů.</span><span class="sxs-lookup"><span data-stu-id="20472-116">If you do not have access to a Windows 10 computer, you can use [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) to generate certificates.</span></span> <span data-ttu-id="20472-117">Certifikáty, které můžete vygenerovat pomocí obou těchto metod se dá nainstalovat na všechny [podporované](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) klientský operační systém.</span><span class="sxs-lookup"><span data-stu-id="20472-117">The certificates that you generate using either method can be installed on any [supported](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) client operating system.</span></span>

## <span data-ttu-id="20472-118"><a name="rootcert"></a>Vytvořit certifikát podepsaný svým držitelem kořenové</span><span class="sxs-lookup"><span data-stu-id="20472-118"><a name="rootcert"></a>Create a self-signed root certificate</span></span>

<span data-ttu-id="20472-119">Pomocí rutiny New-SelfSignedCertificate vytvořit certifikát podepsaný svým držitelem kořenové.</span><span class="sxs-lookup"><span data-stu-id="20472-119">Use the New-SelfSignedCertificate cmdlet to create a self-signed root certificate.</span></span> <span data-ttu-id="20472-120">Parametr Další informace najdete v tématu [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="20472-120">For additional parameter information, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

1. <span data-ttu-id="20472-121">Z počítače se systémem Windows 10 otevřete konzolu prostředí Windows PowerShell se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="20472-121">From a computer running Windows 10, open a Windows PowerShell console with elevated privileges.</span></span>
2. <span data-ttu-id="20472-122">Použijte následující příklad k vytvoření certifikátu podepsaného svým držitelem kořenové.</span><span class="sxs-lookup"><span data-stu-id="20472-122">Use the following example to create the self-signed root certificate.</span></span> <span data-ttu-id="20472-123">Následující příklad vytvoří certifikát podepsaný svým držitelem kořenové s názvem 'P2SRootCert', který je automaticky nainstalován v 'Certifikáty – aktuální User\Personal\Certificates'.</span><span class="sxs-lookup"><span data-stu-id="20472-123">The following example creates a self-signed root certificate named 'P2SRootCert' that is automatically installed in 'Certificates-Current User\Personal\Certificates'.</span></span> <span data-ttu-id="20472-124">Certifikát můžete zobrazit tak, že otevřete *certmgr.msc*, nebo *Správa uživatelských certifikátů*.</span><span class="sxs-lookup"><span data-stu-id="20472-124">You can view the certificate by opening *certmgr.msc*, or *Manage User Certificates*.</span></span>

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <span data-ttu-id="20472-125"><a name="cer"></a>Export veřejného klíče (.cer)</span><span class="sxs-lookup"><span data-stu-id="20472-125"><a name="cer"></a>Export the public key (.cer)</span></span>

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

<span data-ttu-id="20472-126">Soubor exported.cer musí být nahrán do Azure.</span><span class="sxs-lookup"><span data-stu-id="20472-126">The exported.cer file must be uploaded to Azure.</span></span> <span data-ttu-id="20472-127">Pokyny najdete v tématu [konfigurace připojení typu Point-to-Site](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span><span class="sxs-lookup"><span data-stu-id="20472-127">For instructions, see [Configure a Point-to-Site connection](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span></span> <span data-ttu-id="20472-128">Chcete-li přidat další důvěryhodný kořenový certifikát [v této části](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) článku.</span><span class="sxs-lookup"><span data-stu-id="20472-128">To add an additional trusted root certificate, [this section](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) of the article.</span></span>

### <a name="export-the-self-signed-root-certificate-and-public-key-to-store-it-optional"></a><span data-ttu-id="20472-129">Exportovat certifikát podepsaný svým držitelem kořenové a veřejný klíč uložit (volitelné)</span><span class="sxs-lookup"><span data-stu-id="20472-129">Export the self-signed root certificate and public key to store it (optional)</span></span>

<span data-ttu-id="20472-130">Můžete chtít exportovat certifikát podepsaný svým držitelem kořenové a bezpečně uložit.</span><span class="sxs-lookup"><span data-stu-id="20472-130">You may want to export the self-signed root certificate and store it safely.</span></span> <span data-ttu-id="20472-131">Pokud třeba, můžete později jej nainstalovat na jiný počítač a generovat další klientské certifikáty nebo exportovat jiný soubor .cer.</span><span class="sxs-lookup"><span data-stu-id="20472-131">If need be, you can later install it on another computer and generate more client certificates, or export another .cer file.</span></span> <span data-ttu-id="20472-132">Pokud chcete exportovat certifikát podepsaný svým držitelem kořenové jako .pfx, vyberte kořenový certifikát a použít stejný postup, jak je popsáno v [exportovat certifikát klienta](#clientexport).</span><span class="sxs-lookup"><span data-stu-id="20472-132">To export the self-signed root certificate as a .pfx, select the root certificate and use the same steps as described in [Export a client certificate](#clientexport).</span></span>

## <span data-ttu-id="20472-133"><a name="clientcert"></a>Vygenerování klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="20472-133"><a name="clientcert"></a>Generate a client certificate</span></span>

<span data-ttu-id="20472-134">Každý klientský počítač, který se připojuje k virtuální síti pomocí připojení Point-to-Site, musí mít nainstalovaný klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="20472-134">Each client computer that connects to a VNet using Point-to-Site must have a client certificate installed.</span></span> <span data-ttu-id="20472-135">Vygenerujte certifikát klienta z podepsaného svým držitelem kořenový certifikát a pak je exportovat a nainstalovat certifikát klienta.</span><span class="sxs-lookup"><span data-stu-id="20472-135">You generate a client certificate from the self-signed root certificate, and then export and install the client certificate.</span></span> <span data-ttu-id="20472-136">Pokud není nainstalován klientský certifikát, ověření se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="20472-136">If the client certificate is not installed, authentication fails.</span></span> 

<span data-ttu-id="20472-137">Následující kroky vás provedou vygenerování klientského certifikátu z certifikát podepsaný svým držitelem kořenové.</span><span class="sxs-lookup"><span data-stu-id="20472-137">The following steps walk you through generating a client certificate from a self-signed root certificate.</span></span> <span data-ttu-id="20472-138">Z se stejným kořenovým certifikátem může generovat více klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="20472-138">You may generate multiple client certificates from the same root certificate.</span></span> <span data-ttu-id="20472-139">Při generování klientské certifikáty pomocí následujícího postupu klientský certifikát se automaticky nainstaluje na počítači, který jste použili k vygenerování certifikátu.</span><span class="sxs-lookup"><span data-stu-id="20472-139">When you generate client certificates using the steps below, the client certificate is automatically installed on the computer that you used to generate the certificate.</span></span> <span data-ttu-id="20472-140">Pokud chcete nainstalovat certifikát klienta v jiném počítači klienta, můžete exportovat certifikát.</span><span class="sxs-lookup"><span data-stu-id="20472-140">If you want to install a client certificate on another client computer, you can export the certificate.</span></span>

<span data-ttu-id="20472-141">Příklady pomocí rutiny New-SelfSignedCertificate pro vytvoření klientského certifikátu, který vyprší za jeden rok.</span><span class="sxs-lookup"><span data-stu-id="20472-141">The examples use the New-SelfSignedCertificate cmdlet to generate a client certificate that expires in one year.</span></span> <span data-ttu-id="20472-142">Dalším parametr informace, například nastavení hodnoty různých vypršení platnosti certifikátu klienta najdete v části [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="20472-142">For additional parameter information, such as setting a different expiration value for the client certificate, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

### <a name="example-1"></a><span data-ttu-id="20472-143">Příklad 1</span><span class="sxs-lookup"><span data-stu-id="20472-143">Example 1</span></span>

<span data-ttu-id="20472-144">Tento příklad používá proměnnou deklarované '$cert' z předchozí části.</span><span class="sxs-lookup"><span data-stu-id="20472-144">This example uses the declared '$cert' variable from the previous section.</span></span> <span data-ttu-id="20472-145">Pokud po vytvoření certifikátu podepsaného svým držitelem kořenové uzavřít konzolu PowerShell nebo vytváření dalších klientských certifikátů v nové relaci konzoly prostředí PowerShell, použijte kroky v [příklad 2](#ex2).</span><span class="sxs-lookup"><span data-stu-id="20472-145">If you closed the PowerShell console after creating the self-signed root certificate, or are creating additional client certificates in a new PowerShell console session, use the steps in [Example 2](#ex2).</span></span>

<span data-ttu-id="20472-146">Upravit a spustit v příkladu pro vytvoření klientského certifikátu.</span><span class="sxs-lookup"><span data-stu-id="20472-146">Modify and run the example to generate a client certificate.</span></span> <span data-ttu-id="20472-147">Pokud spustíte následující příklad beze změny jeho, výsledkem je klientský certifikát s názvem 'P2SChildCert'.</span><span class="sxs-lookup"><span data-stu-id="20472-147">If you run the following example without modifying it, the result is a client certificate named 'P2SChildCert'.</span></span>  <span data-ttu-id="20472-148">Pokud chcete název certifikátu podřízené něco jiného, změňte hodnotu CN.</span><span class="sxs-lookup"><span data-stu-id="20472-148">If you want to name the child certificate something else, modify the CN value.</span></span> <span data-ttu-id="20472-149">Neměňte TextExtension při spuštění v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="20472-149">Do not change the TextExtension when running this example.</span></span> <span data-ttu-id="20472-150">Certifikát klienta, který je generovat se automaticky nainstaluje v 'Certifikáty – aktuální User\Personal\Certificates' ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="20472-150">The client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <span data-ttu-id="20472-151"><a name="ex2"></a>Příklad 2</span><span class="sxs-lookup"><span data-stu-id="20472-151"><a name="ex2"></a>Example 2</span></span>

<span data-ttu-id="20472-152">Pokud vytváříte další klientské certifikáty, nebo nejsou pomocí stejné relace prostředí PowerShell, který jste použili k vytvoření vašeho certifikátu podepsaného svým držitelem kořenové, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="20472-152">If you are creating additional client certificates, or are not using the same PowerShell session that you used to create your self-signed root certificate, use the following steps:</span></span>

1. <span data-ttu-id="20472-153">Identifikujte podepsaný svým držitelem kořenový certifikát, který je nainstalován v počítači.</span><span class="sxs-lookup"><span data-stu-id="20472-153">Identify the self-signed root certificate that is installed on the computer.</span></span> <span data-ttu-id="20472-154">Tato rutina vrátí seznam certifikátů, které jsou nainstalovány ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="20472-154">This cmdlet returns a list of certificates that are installed on your computer.</span></span>

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. <span data-ttu-id="20472-155">Vyhledejte název subjektu z vráceném seznamu, a poté zkopírujte kryptografický otisk, který je umístěný vedle ho do textového souboru.</span><span class="sxs-lookup"><span data-stu-id="20472-155">Locate the subject name from the returned list, then copy the thumbprint that is located next to it to a text file.</span></span> <span data-ttu-id="20472-156">V následujícím příkladu jsou dva certifikáty.</span><span class="sxs-lookup"><span data-stu-id="20472-156">In the following example, there are two certificates.</span></span> <span data-ttu-id="20472-157">Název CN je název certifikátu podepsaného svým držitelem kořenové ze kterého chcete generovat certifikát podřízené.</span><span class="sxs-lookup"><span data-stu-id="20472-157">The CN name is the name of the self-signed root certificate from which you want to generate a child certificate.</span></span> <span data-ttu-id="20472-158">V tomto případě 'P2SRootCert'.</span><span class="sxs-lookup"><span data-stu-id="20472-158">In this case, 'P2SRootCert'.</span></span>

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. <span data-ttu-id="20472-159">Deklarujte proměnnou pro kořenový certifikát pomocí kryptografického otisku z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="20472-159">Declare a variable for the root certificate using the thumbprint from the previous step.</span></span> <span data-ttu-id="20472-160">Nahraďte OTISK kryptografický otisk kořenového certifikátu, ze kterého chcete generovat certifikát podřízené.</span><span class="sxs-lookup"><span data-stu-id="20472-160">Replace THUMBPRINT with the thumbprint of the root certificate from which you want to generate a child certificate.</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  <span data-ttu-id="20472-161">Například pomocí kryptografického otisku pro P2SRootCert v předchozím kroku, proměnná vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="20472-161">For example, using the thumbprint for P2SRootCert in the previous step, the variable looks like this:</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  <span data-ttu-id="20472-162">Upravit a spustit v příkladu pro vytvoření klientského certifikátu.</span><span class="sxs-lookup"><span data-stu-id="20472-162">Modify and run the example to generate a client certificate.</span></span> <span data-ttu-id="20472-163">Pokud spustíte následující příklad beze změny jeho, výsledkem je klientský certifikát s názvem 'P2SChildCert'.</span><span class="sxs-lookup"><span data-stu-id="20472-163">If you run the following example without modifying it, the result is a client certificate named 'P2SChildCert'.</span></span> <span data-ttu-id="20472-164">Pokud chcete název certifikátu podřízené něco jiného, změňte hodnotu CN.</span><span class="sxs-lookup"><span data-stu-id="20472-164">If you want to name the child certificate something else, modify the CN value.</span></span> <span data-ttu-id="20472-165">Neměňte TextExtension při spuštění v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="20472-165">Do not change the TextExtension when running this example.</span></span> <span data-ttu-id="20472-166">Certifikát klienta, který je generovat se automaticky nainstaluje v 'Certifikáty – aktuální User\Personal\Certificates' ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="20472-166">The client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <span data-ttu-id="20472-167"><a name="clientexport"></a>Export certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="20472-167"><a name="clientexport"></a>Export a client certificate</span></span>   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

## <span data-ttu-id="20472-168"><a name="install"></a>Nainstalovat certifikát exportovaného klienta</span><span class="sxs-lookup"><span data-stu-id="20472-168"><a name="install"></a>Install an exported client certificate</span></span>

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a><span data-ttu-id="20472-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="20472-169">Next steps</span></span>

<span data-ttu-id="20472-170">V konfiguraci Point-to-Site pokračujte.</span><span class="sxs-lookup"><span data-stu-id="20472-170">Continue with your Point-to-Site configuration.</span></span> 

* <span data-ttu-id="20472-171">Pro **Resource Manager** postup nasazení modelu najdete v tématu [konfigurace připojení typu Point-to-Site k virtuální síti](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="20472-171">For **Resource Manager** deployment model steps, see [Configure a Point-to-Site connection to a VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span></span> 
* <span data-ttu-id="20472-172">Pro **classic** postup nasazení modelu najdete v tématu [konfigurace připojení typu Point-to-Site VPN do virtuální sítě (klasické)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="20472-172">For **classic** deployment model steps, see [Configure a Point-to-Site VPN connection to a VNet (classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span></span>