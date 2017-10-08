---
title: "Generování a exportování certifikátů pro Point-to-Site: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento článek obsahuje kroky toocreate certifikát podepsaný svým držitelem kořenové, export veřejného klíče hello a generovat klientské certifikáty pomocí prostředí PowerShell ve Windows 10."
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
ms.openlocfilehash: 11dda015368cda5ce9799fcc4f01d7c542b84fe8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10"></a><span data-ttu-id="1e2d8-103">Generování a exportování certifikátů pro připojení Point-to-Site pomocí prostředí PowerShell ve Windows 10</span><span class="sxs-lookup"><span data-stu-id="1e2d8-103">Generate and export certificates for Point-to-Site connections using PowerShell on Windows 10</span></span>

<span data-ttu-id="1e2d8-104">Připojení point-to-Site pomocí tooauthenticate certifikáty.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-104">Point-to-Site connections use certificates tooauthenticate.</span></span> <span data-ttu-id="1e2d8-105">Tento článek ukazuje, jak toocreate a podepsaný svým držitelem kořenový certifikát a generovat klientské certifikáty pomocí prostředí PowerShell ve Windows 10.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-105">This article shows you how toocreate a self-signed root certificate and generate client certificates using PowerShell on Windows 10.</span></span> <span data-ttu-id="1e2d8-106">Pokud hledáte kroků konfigurace Point-to-Site, například jak tooupload kořenové certifikáty, vyberte jeden z článků hello ' konfigurace Point-to-Site, z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="1e2d8-106">If you are looking for Point-to-Site configuration steps, such as how tooupload root certificates, select one of hello 'Configure Point-to-Site' articles from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1e2d8-107">Vytvořit certifikáty podepsané svým držitelem – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1e2d8-107">Create self-signed certificates - PowerShell</span></span>](vpn-gateway-certificates-point-to-site.md)
> * [<span data-ttu-id="1e2d8-108">Vytvořit certifikáty podepsané svým držitelem - MakeCert</span><span class="sxs-lookup"><span data-stu-id="1e2d8-108">Create self-signed certificates - MakeCert</span></span>](vpn-gateway-certificates-point-to-site-makecert.md)
> * [<span data-ttu-id="1e2d8-109">Konfigurace Point-to-Site - Resource Manager – portál Azure</span><span class="sxs-lookup"><span data-stu-id="1e2d8-109">Configure Point-to-Site - Resource Manager - Azure portal</span></span>](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="1e2d8-110">Konfigurace Point-to-Site - Resource Manager – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1e2d8-110">Configure Point-to-Site - Resource Manager - PowerShell</span></span>](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [<span data-ttu-id="1e2d8-111">Konfigurace Point-to-Site - Classic - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1e2d8-111">Configure Point-to-Site - Classic - Azure portal</span></span>](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


<span data-ttu-id="1e2d8-112">Je třeba provést hello kroky v tomto článku v počítači se systémem Windows 10.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-112">You must perform hello steps in this article on a computer running Windows 10.</span></span> <span data-ttu-id="1e2d8-113">rutiny prostředí PowerShell Hello použít toogenerate certifikáty jsou součástí operačního systému hello Windows 10 a nefungují v jiných verzích Windows.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-113">hello PowerShell cmdlets that you use toogenerate certificates are part of hello Windows 10 operating system and do not work on other versions of Windows.</span></span> <span data-ttu-id="1e2d8-114">počítač Hello Windows 10 je jenom certifikáty potřebné toogenerate hello.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-114">hello Windows 10 computer is only needed toogenerate hello certificates.</span></span> <span data-ttu-id="1e2d8-115">Po vygenerování hello certifikáty jsou můžete odešlete, nebo je nainstalovat pro všechny podporované klientské operační systémy.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-115">Once hello certificates are generated, you can upload them, or install them on any supported client operating system.</span></span> 

<span data-ttu-id="1e2d8-116">Pokud nemáte přístup k počítači tooa Windows 10, můžete použít [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) toogenerate certifikáty.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-116">If you do not have access tooa Windows 10 computer, you can use [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) toogenerate certificates.</span></span> <span data-ttu-id="1e2d8-117">Hello certifikáty, které generují pomocí obou těchto metod se dá nainstalovat na všechny [podporované](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) klientský operační systém.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-117">hello certificates that you generate using either method can be installed on any [supported](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) client operating system.</span></span>

## <span data-ttu-id="1e2d8-118"><a name="rootcert"></a>Vytvořit certifikát podepsaný svým držitelem kořenové</span><span class="sxs-lookup"><span data-stu-id="1e2d8-118"><a name="rootcert"></a>Create a self-signed root certificate</span></span>

<span data-ttu-id="1e2d8-119">Použijte toocreate rutiny New-SelfSignedCertificate hello certifikát podepsaný svým držitelem kořenové.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-119">Use hello New-SelfSignedCertificate cmdlet toocreate a self-signed root certificate.</span></span> <span data-ttu-id="1e2d8-120">Parametr Další informace najdete v tématu [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="1e2d8-120">For additional parameter information, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

1. <span data-ttu-id="1e2d8-121">Z počítače se systémem Windows 10 otevřete konzolu prostředí Windows PowerShell se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-121">From a computer running Windows 10, open a Windows PowerShell console with elevated privileges.</span></span>
2. <span data-ttu-id="1e2d8-122">Použijte následující příklad toocreate hello podepsaný svým držitelem kořenový certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-122">Use hello following example toocreate hello self-signed root certificate.</span></span> <span data-ttu-id="1e2d8-123">Hello následující příklad vytvoří certifikát podepsaný svým držitelem kořenové s názvem 'P2SRootCert', který je automaticky nainstalován v 'Certifikáty – aktuální User\Personal\Certificates'.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-123">hello following example creates a self-signed root certificate named 'P2SRootCert' that is automatically installed in 'Certificates-Current User\Personal\Certificates'.</span></span> <span data-ttu-id="1e2d8-124">Můžete zobrazit certifikát hello otevřením *certmgr.msc*, nebo *Správa uživatelských certifikátů*.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-124">You can view hello certificate by opening *certmgr.msc*, or *Manage User Certificates*.</span></span>

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <span data-ttu-id="1e2d8-125"><a name="cer"></a>Export hello veřejného klíče (.cer)</span><span class="sxs-lookup"><span data-stu-id="1e2d8-125"><a name="cer"></a>Export hello public key (.cer)</span></span>

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

<span data-ttu-id="1e2d8-126">nahrané tooAzure musí být soubor exported.cer Hello.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-126">hello exported.cer file must be uploaded tooAzure.</span></span> <span data-ttu-id="1e2d8-127">Pokyny najdete v tématu [konfigurace připojení typu Point-to-Site](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span><span class="sxs-lookup"><span data-stu-id="1e2d8-127">For instructions, see [Configure a Point-to-Site connection](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span></span> <span data-ttu-id="1e2d8-128">tooadd další důvěryhodný kořenový certifikát [v této části](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) hello článku.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-128">tooadd an additional trusted root certificate, [this section](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) of hello article.</span></span>

### <a name="export-hello-self-signed-root-certificate-and-public-key-toostore-it-optional"></a><span data-ttu-id="1e2d8-129">Exportovat certifikát podepsaný svým držitelem kořenové hello a veřejného klíče toostore ho (volitelné)</span><span class="sxs-lookup"><span data-stu-id="1e2d8-129">Export hello self-signed root certificate and public key toostore it (optional)</span></span>

<span data-ttu-id="1e2d8-130">Můžete chtít tooexport hello kořenového certifikátu podepsaného svým držitelem a bezpečně uložit.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-130">You may want tooexport hello self-signed root certificate and store it safely.</span></span> <span data-ttu-id="1e2d8-131">Pokud třeba, můžete později jej nainstalovat na jiný počítač a generovat další klientské certifikáty nebo exportovat jiný soubor .cer.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-131">If need be, you can later install it on another computer and generate more client certificates, or export another .cer file.</span></span> <span data-ttu-id="1e2d8-132">tooexport hello certifikátu podepsaného svým držitelem kořenové jako PFX, vyberte hello kořenový certifikát a použití hello stejný postup, jak je popsáno v [exportovat certifikát klienta](#clientexport).</span><span class="sxs-lookup"><span data-stu-id="1e2d8-132">tooexport hello self-signed root certificate as a .pfx, select hello root certificate and use hello same steps as described in [Export a client certificate](#clientexport).</span></span>

## <span data-ttu-id="1e2d8-133"><a name="clientcert"></a>Vygenerování klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="1e2d8-133"><a name="clientcert"></a>Generate a client certificate</span></span>

<span data-ttu-id="1e2d8-134">Každý klientský počítač, který se připojuje tooa Point-to-Site pomocí virtuální síť musí mít nainstalovaný certifikát klienta.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-134">Each client computer that connects tooa VNet using Point-to-Site must have a client certificate installed.</span></span> <span data-ttu-id="1e2d8-135">Vygenerujte certifikát klienta z hello podepsaný svým držitelem kořenový certifikát a pak je exportovat a nainstalovat certifikát klienta hello.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-135">You generate a client certificate from hello self-signed root certificate, and then export and install hello client certificate.</span></span> <span data-ttu-id="1e2d8-136">Pokud hello klientský certifikát není nainstalovaná, ověření se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-136">If hello client certificate is not installed, authentication fails.</span></span> 

<span data-ttu-id="1e2d8-137">Hello následující kroky vás provedou vygenerování klientského certifikátu z certifikát podepsaný svým držitelem kořenové.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-137">hello following steps walk you through generating a client certificate from a self-signed root certificate.</span></span> <span data-ttu-id="1e2d8-138">Více klientských certifikátů může vygenerovat z hello stejným kořenovým certifikátem.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-138">You may generate multiple client certificates from hello same root certificate.</span></span> <span data-ttu-id="1e2d8-139">Při generování klientských certifikátů pomocí následujících kroků hello hello klientský certifikát je automaticky nainstaluje na počítač hello použitá toogenerate hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-139">When you generate client certificates using hello steps below, hello client certificate is automatically installed on hello computer that you used toogenerate hello certificate.</span></span> <span data-ttu-id="1e2d8-140">Pokud chcete tooinstall klientský certifikát na jiném počítači klienta, můžete exportovat certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-140">If you want tooinstall a client certificate on another client computer, you can export hello certificate.</span></span>

<span data-ttu-id="1e2d8-141">Příklady Hello pomocí toogenerate rutiny New-SelfSignedCertificate hello klientský certifikát, který vyprší za jeden rok.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-141">hello examples use hello New-SelfSignedCertificate cmdlet toogenerate a client certificate that expires in one year.</span></span> <span data-ttu-id="1e2d8-142">Dalším parametr informace, například nastavení hodnoty různých vypršení platnosti certifikátu klienta se hello, najdete v části [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="1e2d8-142">For additional parameter information, such as setting a different expiration value for hello client certificate, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

### <a name="example-1"></a><span data-ttu-id="1e2d8-143">Příklad 1</span><span class="sxs-lookup"><span data-stu-id="1e2d8-143">Example 1</span></span>

<span data-ttu-id="1e2d8-144">Tento příklad používá hello deklarovat proměnnou '$cert' z předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-144">This example uses hello declared '$cert' variable from hello previous section.</span></span> <span data-ttu-id="1e2d8-145">Pokud jste zavřeli konzole PowerShell hello po vytvoření hello certifikát podepsaný svým držitelem kořenové, nebo vytváříte další klientské certifikáty v nové relaci konzoly prostředí PowerShell, použijte kroky hello v [příklad 2](#ex2).</span><span class="sxs-lookup"><span data-stu-id="1e2d8-145">If you closed hello PowerShell console after creating hello self-signed root certificate, or are creating additional client certificates in a new PowerShell console session, use hello steps in [Example 2](#ex2).</span></span>

<span data-ttu-id="1e2d8-146">Upravit a spustit toogenerate příklad hello klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-146">Modify and run hello example toogenerate a client certificate.</span></span> <span data-ttu-id="1e2d8-147">Pokud spustíte hello následující příklad beze změny jeho, výsledkem hello je klientský certifikát s názvem 'P2SChildCert'.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-147">If you run hello following example without modifying it, hello result is a client certificate named 'P2SChildCert'.</span></span>  <span data-ttu-id="1e2d8-148">Pokud chcete certifikát podřízené hello tooname něco jiného, upravte hodnotu CN hello.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-148">If you want tooname hello child certificate something else, modify hello CN value.</span></span> <span data-ttu-id="1e2d8-149">Neměňte hello TextExtension při spuštění v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-149">Do not change hello TextExtension when running this example.</span></span> <span data-ttu-id="1e2d8-150">Hello klientský certifikát, který je generovat se automaticky nainstaluje v 'Certifikáty – aktuální User\Personal\Certificates' ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-150">hello client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <span data-ttu-id="1e2d8-151"><a name="ex2"></a>Příklad 2</span><span class="sxs-lookup"><span data-stu-id="1e2d8-151"><a name="ex2"></a>Example 2</span></span>

<span data-ttu-id="1e2d8-152">Pokud vytváříte další klientské certifikáty, nebo jsou nepoužíváte hello stejné relace prostředí PowerShell, kterou jste použili toocreate certifikát podepsaný svým držitelem kořenové, hello použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1e2d8-152">If you are creating additional client certificates, or are not using hello same PowerShell session that you used toocreate your self-signed root certificate, use hello following steps:</span></span>

1. <span data-ttu-id="1e2d8-153">Identifikujte hello podepsaný svým držitelem kořenový certifikát, který je nainstalován na počítači hello.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-153">Identify hello self-signed root certificate that is installed on hello computer.</span></span> <span data-ttu-id="1e2d8-154">Tato rutina vrátí seznam certifikátů, které jsou nainstalovány ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-154">This cmdlet returns a list of certificates that are installed on your computer.</span></span>

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. <span data-ttu-id="1e2d8-155">Najděte název subjektu hello z hello vrátí seznam, pak kryptografický otisk hello kopie, který je umístěn další text tooa tooit souboru.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-155">Locate hello subject name from hello returned list, then copy hello thumbprint that is located next tooit tooa text file.</span></span> <span data-ttu-id="1e2d8-156">V následujícím příkladu hello existují dva certifikáty.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-156">In hello following example, there are two certificates.</span></span> <span data-ttu-id="1e2d8-157">Název CN Hello je název hello hello podepsaný svým držitelem kořenového certifikátu, ze kterého mají být toogenerate certifikát podřízené.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-157">hello CN name is hello name of hello self-signed root certificate from which you want toogenerate a child certificate.</span></span> <span data-ttu-id="1e2d8-158">V tomto případě 'P2SRootCert'.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-158">In this case, 'P2SRootCert'.</span></span>

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. <span data-ttu-id="1e2d8-159">Deklarujte proměnnou pro hello kořenový certifikát pomocí kryptografického otisku hello hello v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-159">Declare a variable for hello root certificate using hello thumbprint from hello previous step.</span></span> <span data-ttu-id="1e2d8-160">Nahraďte OTISK hello kryptografický otisk hello kořenový certifikát, ze kterého mají být toogenerate certifikát podřízené.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-160">Replace THUMBPRINT with hello thumbprint of hello root certificate from which you want toogenerate a child certificate.</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  <span data-ttu-id="1e2d8-161">Například pomocí hello kryptografický otisk pro P2SRootCert v předchozím kroku hello, proměnná hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1e2d8-161">For example, using hello thumbprint for P2SRootCert in hello previous step, hello variable looks like this:</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  <span data-ttu-id="1e2d8-162">Upravit a spustit toogenerate příklad hello klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-162">Modify and run hello example toogenerate a client certificate.</span></span> <span data-ttu-id="1e2d8-163">Pokud spustíte hello následující příklad beze změny jeho, výsledkem hello je klientský certifikát s názvem 'P2SChildCert'.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-163">If you run hello following example without modifying it, hello result is a client certificate named 'P2SChildCert'.</span></span> <span data-ttu-id="1e2d8-164">Pokud chcete certifikát podřízené hello tooname něco jiného, upravte hodnotu CN hello.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-164">If you want tooname hello child certificate something else, modify hello CN value.</span></span> <span data-ttu-id="1e2d8-165">Neměňte hello TextExtension při spuštění v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-165">Do not change hello TextExtension when running this example.</span></span> <span data-ttu-id="1e2d8-166">Hello klientský certifikát, který je generovat se automaticky nainstaluje v 'Certifikáty – aktuální User\Personal\Certificates' ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-166">hello client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <span data-ttu-id="1e2d8-167"><a name="clientexport"></a>Export certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="1e2d8-167"><a name="clientexport"></a>Export a client certificate</span></span>   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

## <span data-ttu-id="1e2d8-168"><a name="install"></a>Nainstalovat certifikát exportovaného klienta</span><span class="sxs-lookup"><span data-stu-id="1e2d8-168"><a name="install"></a>Install an exported client certificate</span></span>

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a><span data-ttu-id="1e2d8-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e2d8-169">Next steps</span></span>

<span data-ttu-id="1e2d8-170">V konfiguraci Point-to-Site pokračujte.</span><span class="sxs-lookup"><span data-stu-id="1e2d8-170">Continue with your Point-to-Site configuration.</span></span> 

* <span data-ttu-id="1e2d8-171">Pro **Resource Manager** postup nasazení modelu najdete v tématu [konfigurace tooa připojení Point-to-Site VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1e2d8-171">For **Resource Manager** deployment model steps, see [Configure a Point-to-Site connection tooa VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span></span> 
* <span data-ttu-id="1e2d8-172">Pro **classic** postup nasazení modelu najdete v tématu [konfigurace tooa připojení Point-to-Site VPN virtuální sítě (klasické)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1e2d8-172">For **classic** deployment model steps, see [Configure a Point-to-Site VPN connection tooa VNet (classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span></span>
