---
title: "Konfigurace zabezpečení aaaSplit sloučení | Microsoft Docs"
description: "Nastavit x409 certifikáty pro šifrování"
metakeywords: Elastic Database certificates security
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: f9e89c57-61a0-484f-b787-82dae2349cb6
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 511c04be0598d8a0889aa3e3fcf02be0bf0e96cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="split-merge-security-configuration"></a><span data-ttu-id="209fb-103">Konfigurace zabezpečení rozdělení sloučení</span><span class="sxs-lookup"><span data-stu-id="209fb-103">Split-merge security configuration</span></span>
<span data-ttu-id="209fb-104">toouse hello rozdělení či sloučení služby, je nutné správně nakonfigurovat zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="209fb-104">toouse hello Split/Merge service, you must correctly configure security.</span></span> <span data-ttu-id="209fb-105">Služba Hello je součástí funkce hello elastické škálování sady Microsoft Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="209fb-105">hello service is part of hello Elastic Scale feature of Microsoft Azure SQL Database.</span></span> <span data-ttu-id="209fb-106">Další informace najdete v tématu [elastické škálování rozdělení a sloučení kurz služby](sql-database-elastic-scale-configure-deploy-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="209fb-106">For more information, see [Elastic Scale Split and Merge Service Tutorial](sql-database-elastic-scale-configure-deploy-split-and-merge.md).</span></span>

## <a name="configuring-certificates"></a><span data-ttu-id="209fb-107">Konfigurace certifikátů</span><span class="sxs-lookup"><span data-stu-id="209fb-107">Configuring certificates</span></span>
<span data-ttu-id="209fb-108">Certifikáty jsou nakonfigurovat dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="209fb-108">Certificates are configured in two ways.</span></span> 

1. [<span data-ttu-id="209fb-109">tooConfigure hello certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="209fb-109">tooConfigure hello SSL Certificate</span></span>](#to-configure-the-ssl-certificate)
2. [<span data-ttu-id="209fb-110">tooConfigure klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="209fb-110">tooConfigure Client Certificates</span></span>](#to-configure-client-certificates) 

## <a name="tooobtain-certificates"></a><span data-ttu-id="209fb-111">tooobtain certifikáty</span><span class="sxs-lookup"><span data-stu-id="209fb-111">tooobtain certificates</span></span>
<span data-ttu-id="209fb-112">Certifikáty nelze získat z veřejné certifikační autority (CA) nebo z hello [certifikát služby systému Windows](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx).</span><span class="sxs-lookup"><span data-stu-id="209fb-112">Certificates can be obtained from public Certificate Authorities (CAs) or from hello [Windows Certificate Service](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx).</span></span> <span data-ttu-id="209fb-113">Jedná se o upřednostňovaný hello metody tooobtain certifikáty.</span><span class="sxs-lookup"><span data-stu-id="209fb-113">These are hello preferred methods tooobtain certificates.</span></span>

<span data-ttu-id="209fb-114">Pokud nejsou k dispozici tyto možnosti, můžete vygenerovat **certifikáty podepsané svým držitelem**.</span><span class="sxs-lookup"><span data-stu-id="209fb-114">If those options are not available, you can generate **self-signed certificates**.</span></span>

## <a name="tools-toogenerate-certificates"></a><span data-ttu-id="209fb-115">Nástroje pro certifikáty toogenerate</span><span class="sxs-lookup"><span data-stu-id="209fb-115">Tools toogenerate certificates</span></span>
* [<span data-ttu-id="209fb-116">MakeCert.exe</span><span class="sxs-lookup"><span data-stu-id="209fb-116">makecert.exe</span></span>](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [<span data-ttu-id="209fb-117">Pvk2pfx.exe</span><span class="sxs-lookup"><span data-stu-id="209fb-117">pvk2pfx.exe</span></span>](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="toorun-hello-tools"></a><span data-ttu-id="209fb-118">toorun hello nástroje</span><span class="sxs-lookup"><span data-stu-id="209fb-118">toorun hello tools</span></span>
* <span data-ttu-id="209fb-119">Z příkazového řádku pro vývojáře Visual Studia, najdete v části [Visual Studio – příkazový řádek](http://msdn.microsoft.com/library/ms229859.aspx)</span><span class="sxs-lookup"><span data-stu-id="209fb-119">From a Developer Command Prompt for Visual Studios, see [Visual Studio Command Prompt](http://msdn.microsoft.com/library/ms229859.aspx)</span></span> 
  
    <span data-ttu-id="209fb-120">Pokud nainstalovaná, přejděte na:</span><span class="sxs-lookup"><span data-stu-id="209fb-120">If installed, go to:</span></span>
  
        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 
* <span data-ttu-id="209fb-121">Získat hello WDK z [Windows 8.1: stažení sad a nástrojů](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)</span><span class="sxs-lookup"><span data-stu-id="209fb-121">Get hello WDK from [Windows 8.1: Download kits and tools](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)</span></span>

## <a name="tooconfigure-hello-ssl-certificate"></a><span data-ttu-id="209fb-122">certifikát SSL tooconfigure hello</span><span class="sxs-lookup"><span data-stu-id="209fb-122">tooconfigure hello SSL certificate</span></span>
<span data-ttu-id="209fb-123">Certifikát SSL požadovaný tooencrypt hello komunikace a ověření serveru hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-123">A SSL certificate is required tooencrypt hello communication and authenticate hello server.</span></span> <span data-ttu-id="209fb-124">Zvolte hello nejvhodnějšímu hello tři scénáře a spusťte všechny jeho kroky:</span><span class="sxs-lookup"><span data-stu-id="209fb-124">Choose hello most applicable of hello three scenarios below, and execute all its steps:</span></span>

### <a name="create-a-new-self-signed-certificate"></a><span data-ttu-id="209fb-125">Vytvořit nový certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="209fb-125">Create a new self-signed certificate</span></span>
1. [<span data-ttu-id="209fb-126">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="209fb-126">Create a Self-Signed Certificate</span></span>](#create-a-self-signed-certificate)
2. [<span data-ttu-id="209fb-127">Vytvoření souboru PFX pro certifikát SSL podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="209fb-127">Create PFX file for Self-Signed SSL Certificate</span></span>](#create-pfx-file-for-self-signed-ssl-certificate)
3. [<span data-ttu-id="209fb-128">Nahrajte certifikát SSL tooCloud služby</span><span class="sxs-lookup"><span data-stu-id="209fb-128">Upload SSL Certificate tooCloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
4. [<span data-ttu-id="209fb-129">Aktualizovat certifikát SSL v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="209fb-129">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)
5. [<span data-ttu-id="209fb-130">Import certifikační autorita protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="209fb-130">Import SSL Certification Authority</span></span>](#import-ssl-certification-authority)

### <a name="toouse-an-existing-certificate-from-hello-certificate-store"></a><span data-ttu-id="209fb-131">uložení toouse existující certifikát od hello certifikátu</span><span class="sxs-lookup"><span data-stu-id="209fb-131">toouse an existing certificate from hello certificate store</span></span>
1. [<span data-ttu-id="209fb-132">Exportovat certifikát SSL z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="209fb-132">Export SSL Certificate From Certificate Store</span></span>](#export-ssl-certificate-from-certificate-store)
2. [<span data-ttu-id="209fb-133">Nahrajte certifikát SSL tooCloud služby</span><span class="sxs-lookup"><span data-stu-id="209fb-133">Upload SSL Certificate tooCloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
3. [<span data-ttu-id="209fb-134">Aktualizovat certifikát SSL v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="209fb-134">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)

### <a name="toouse-an-existing-certificate-in-a-pfx-file"></a><span data-ttu-id="209fb-135">toouse existující certifikát v souboru PFX</span><span class="sxs-lookup"><span data-stu-id="209fb-135">toouse an existing certificate in a PFX file</span></span>
1. [<span data-ttu-id="209fb-136">Nahrajte certifikát SSL tooCloud služby</span><span class="sxs-lookup"><span data-stu-id="209fb-136">Upload SSL Certificate tooCloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
2. [<span data-ttu-id="209fb-137">Aktualizovat certifikát SSL v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="209fb-137">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)

## <a name="tooconfigure-client-certificates"></a><span data-ttu-id="209fb-138">tooconfigure klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="209fb-138">tooconfigure client certificates</span></span>
<span data-ttu-id="209fb-139">Klientské certifikáty se vyžadují v pořadí tooauthenticate požadavky toohello služby.</span><span class="sxs-lookup"><span data-stu-id="209fb-139">Client certificates are required in order tooauthenticate requests toohello service.</span></span> <span data-ttu-id="209fb-140">Zvolte hello nejvhodnějšímu hello tři scénáře a spusťte všechny jeho kroky:</span><span class="sxs-lookup"><span data-stu-id="209fb-140">Choose hello most applicable of hello three scenarios below, and execute all its steps:</span></span>

### <a name="turn-off-client-certificates"></a><span data-ttu-id="209fb-141">Vypnout klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="209fb-141">Turn off client certificates</span></span>
1. [<span data-ttu-id="209fb-142">Vypněte ověřování na základě certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-142">Turn Off Client Certificate-Based Authentication</span></span>](#turn-off-client-certificate-based-authentication)

### <a name="issue-new-self-signed-client-certificates"></a><span data-ttu-id="209fb-143">Vydat nové certifikáty podepsané svým držitelem klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-143">Issue new self-signed client certificates</span></span>
1. [<span data-ttu-id="209fb-144">Vytvořit podepsaný certifikační autoritou</span><span class="sxs-lookup"><span data-stu-id="209fb-144">Create a Self-Signed Certification Authority</span></span>](#create-a-self-signed-certification-authority)
2. [<span data-ttu-id="209fb-145">Nahrajte certifikát certifikační Autority tooCloud služby</span><span class="sxs-lookup"><span data-stu-id="209fb-145">Upload CA Certificate tooCloud Service</span></span>](#upload-ca-certificate-to-cloud-service)
3. [<span data-ttu-id="209fb-146">Aktualizovat certifikát certifikační Autority v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="209fb-146">Update CA Certificate in Service Configuration File</span></span>](#update-ca-certificate-in-service-configuration-file)
4. [<span data-ttu-id="209fb-147">Problém klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="209fb-147">Issue Client Certificates</span></span>](#issue-client-certificates)
5. [<span data-ttu-id="209fb-148">Vytvářet soubory PFX pro certifikáty klientů</span><span class="sxs-lookup"><span data-stu-id="209fb-148">Create PFX files for Client Certificates</span></span>](#create-pfx-files-for-client-certificates)
6. [<span data-ttu-id="209fb-149">Import certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-149">Import Client Certificate</span></span>](#Import-Client-Certificate)
7. [<span data-ttu-id="209fb-150">Zkopírujte kryptografické otisky certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-150">Copy Client Certificate Thumbprints</span></span>](#copy-client-certificate-thumbprints)
8. [<span data-ttu-id="209fb-151">Konfigurace klientů povolené v hello konfigurační soubor služby</span><span class="sxs-lookup"><span data-stu-id="209fb-151">Configure Allowed Clients in hello Service Configuration File</span></span>](#configure-allowed-clients-in-the-service-configuration-file)

### <a name="use-existing-client-certificates"></a><span data-ttu-id="209fb-152">Použít existující klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="209fb-152">Use existing client certificates</span></span>
1. [<span data-ttu-id="209fb-153">Najít veřejného klíče certifikační Autority</span><span class="sxs-lookup"><span data-stu-id="209fb-153">Find CA Public Key</span></span>](#find-ca-public-key)
2. [<span data-ttu-id="209fb-154">Nahrajte certifikát certifikační Autority tooCloud služby</span><span class="sxs-lookup"><span data-stu-id="209fb-154">Upload CA Certificate tooCloud Service</span></span>](#Upload-CA-certificate-to-cloud-service)
3. [<span data-ttu-id="209fb-155">Aktualizovat certifikát certifikační Autority v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="209fb-155">Update CA Certificate in Service Configuration File</span></span>](#Update-CA-Certificate-in-Service-Configuration-File)
4. [<span data-ttu-id="209fb-156">Zkopírujte kryptografické otisky certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-156">Copy Client Certificate Thumbprints</span></span>](#Copy-Client-Certificate-Thumbprints)
5. [<span data-ttu-id="209fb-157">Konfigurace klientů povolené v hello konfigurační soubor služby</span><span class="sxs-lookup"><span data-stu-id="209fb-157">Configure Allowed Clients in hello Service Configuration File</span></span>](#configure-allowed-clients-in-the-service-configuration-file)
6. [<span data-ttu-id="209fb-158">Konfigurace Kontrola odvolání certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-158">Configure Client Certificate Revocation Check</span></span>](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a><span data-ttu-id="209fb-159">Povolené IP adresy</span><span class="sxs-lookup"><span data-stu-id="209fb-159">Allowed IP addresses</span></span>
<span data-ttu-id="209fb-160">Koncové body služby toohello přístup může být omezená toospecific rozsahy IP adres.</span><span class="sxs-lookup"><span data-stu-id="209fb-160">Access toohello service endpoints can be restricted toospecific ranges of IP addresses.</span></span>

## <a name="tooconfigure-encryption-for-hello-store"></a><span data-ttu-id="209fb-161">tooconfigure šifrování pro úložiště hello</span><span class="sxs-lookup"><span data-stu-id="209fb-161">tooconfigure encryption for hello store</span></span>
<span data-ttu-id="209fb-162">Certifikát je požadovaná tooencrypt hello pověření, které jsou uloženy v úložišti metadat hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-162">A certificate is required tooencrypt hello credentials that are stored in hello metadata store.</span></span> <span data-ttu-id="209fb-163">Zvolte hello nejvhodnějšímu hello tři scénáře a spusťte všechny jeho kroky:</span><span class="sxs-lookup"><span data-stu-id="209fb-163">Choose hello most applicable of hello three scenarios below, and execute all its steps:</span></span>

### <a name="use-a-new-self-signed-certificate"></a><span data-ttu-id="209fb-164">Použití nového certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="209fb-164">Use a new self-signed certificate</span></span>
1. [<span data-ttu-id="209fb-165">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="209fb-165">Create a Self-Signed Certificate</span></span>](#create-a-self-signed-certificate)
2. [<span data-ttu-id="209fb-166">Vytvořit soubor PFX pro šifrovací certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="209fb-166">Create PFX file for Self-Signed Encryption Certificate</span></span>](#create-pfx-file-for-self-signed-ssl-certificate)
3. [<span data-ttu-id="209fb-167">Nahrajte certifikát pro šifrování tooCloud služby</span><span class="sxs-lookup"><span data-stu-id="209fb-167">Upload Encryption Certificate tooCloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
4. [<span data-ttu-id="209fb-168">Aktualizovat certifikát šifrování v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="209fb-168">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-from-hello-certificate-store"></a><span data-ttu-id="209fb-169">Použít stávající certifikát z úložiště certifikátů hello</span><span class="sxs-lookup"><span data-stu-id="209fb-169">Use an existing certificate from hello certificate store</span></span>
1. [<span data-ttu-id="209fb-170">Exportovat šifrovací certifikát z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="209fb-170">Export Encryption Certificate From Certificate Store</span></span>](#export-encryption-certificate-from-certificate-store)
2. [<span data-ttu-id="209fb-171">Nahrajte certifikát pro šifrování tooCloud služby</span><span class="sxs-lookup"><span data-stu-id="209fb-171">Upload Encryption Certificate tooCloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
3. [<span data-ttu-id="209fb-172">Aktualizovat certifikát šifrování v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="209fb-172">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a><span data-ttu-id="209fb-173">Použít stávající certifikát v souboru PFX</span><span class="sxs-lookup"><span data-stu-id="209fb-173">Use an existing certificate in a PFX file</span></span>
1. [<span data-ttu-id="209fb-174">Nahrajte certifikát pro šifrování tooCloud služby</span><span class="sxs-lookup"><span data-stu-id="209fb-174">Upload Encryption Certificate tooCloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
2. [<span data-ttu-id="209fb-175">Aktualizovat certifikát šifrování v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="209fb-175">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

## <a name="hello-default-configuration"></a><span data-ttu-id="209fb-176">Hello výchozí konfigurace</span><span class="sxs-lookup"><span data-stu-id="209fb-176">hello default configuration</span></span>
<span data-ttu-id="209fb-177">Výchozí konfigurace Hello odmítne koncový bod HTTP toohello všechny přístup.</span><span class="sxs-lookup"><span data-stu-id="209fb-177">hello default configuration denies all access toohello HTTP endpoint.</span></span> <span data-ttu-id="209fb-178">Toto je doporučená nastavení, protože hello požadavky toothese koncové body mohou provádět citlivé údaje, jako jsou přihlašovací údaje databáze hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-178">This is hello recommended setting, since hello requests toothese endpoints may carry sensitive information like database credentials.</span></span>
<span data-ttu-id="209fb-179">Výchozí konfigurace Hello umožňuje koncový bod HTTPS toohello všechny přístup.</span><span class="sxs-lookup"><span data-stu-id="209fb-179">hello default configuration allows all access toohello HTTPS endpoint.</span></span> <span data-ttu-id="209fb-180">Toto nastavení může být další s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="209fb-180">This setting may be restricted further.</span></span>

### <a name="changing-hello-configuration"></a><span data-ttu-id="209fb-181">Změna hello konfigurace</span><span class="sxs-lookup"><span data-stu-id="209fb-181">Changing hello Configuration</span></span>
<span data-ttu-id="209fb-182">Hello skupiny pravidly řízení přístupu, které se vztahují tooand koncový bod se konfigurují v hello  **<EndpointAcls>**  část v hello **konfigurační soubor služby**.</span><span class="sxs-lookup"><span data-stu-id="209fb-182">hello group of access control rules that apply tooand endpoint are configured in hello **<EndpointAcls>** section in hello **service configuration file**.</span></span>

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

<span data-ttu-id="209fb-183">Hello pravidel ve skupině řízení přístupu, které jsou nakonfigurované v <AccessControl name=""> části konfigurační soubor služby hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-183">hello rules in an access control group are configured in a <AccessControl name=""> section of hello service configuration file.</span></span> 

<span data-ttu-id="209fb-184">Formát Hello je vysvětleno v dokumentaci seznamů řízení přístupu k síti.</span><span class="sxs-lookup"><span data-stu-id="209fb-184">hello format is explained in Network Access Control Lists documentation.</span></span>
<span data-ttu-id="209fb-185">Například tooallow pouze IP adresy v hello rozsah 100.100.0.0 too100.100.255.255 tooaccess hello koncový bod HTTPS, pravidla hello bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="209fb-185">For example, tooallow only IPs in hello range 100.100.0.0 too100.100.255.255 tooaccess hello HTTPS endpoint, hello rules would look like this:</span></span>

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a><span data-ttu-id="209fb-186">Odmítnutí služby prevence</span><span class="sxs-lookup"><span data-stu-id="209fb-186">Denial of service prevention</span></span>
<span data-ttu-id="209fb-187">Existují dva různé mechanismy podporované toodetect a zabránit útokům odmítnutí služby:</span><span class="sxs-lookup"><span data-stu-id="209fb-187">There are two different mechanisms supported toodetect and prevent Denial of Service attacks:</span></span>

* <span data-ttu-id="209fb-188">Omezit počet souběžných požadavků na vzdáleného hostitele (ve výchozím nastavení vypnuté)</span><span class="sxs-lookup"><span data-stu-id="209fb-188">Restrict number of concurrent requests per remote host (off by default)</span></span>
* <span data-ttu-id="209fb-189">Omezit rychlost přístupu na vzdáleného hostitele (na ve výchozím nastavení)</span><span class="sxs-lookup"><span data-stu-id="209fb-189">Restrict rate of access per remote host (on by default)</span></span>

<span data-ttu-id="209fb-190">Tyto jsou založené na hello funkce, které jsou popsána v dynamické IP zabezpečení ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="209fb-190">These are based on hello features further documented in Dynamic IP Security in IIS.</span></span> <span data-ttu-id="209fb-191">Při změně této konfigurace pozor hello následující faktory:</span><span class="sxs-lookup"><span data-stu-id="209fb-191">When changing this configuration beware of hello following factors:</span></span>

* <span data-ttu-id="209fb-192">chování Hello proxy servery a zařízení překlad síťových adres přes hello vzdáleného hostitele informace</span><span class="sxs-lookup"><span data-stu-id="209fb-192">hello behavior of proxies and Network Address Translation devices over hello remote host information</span></span>
* <span data-ttu-id="209fb-193">Každý prostředek tooany žádosti v hello webovou roli považuje za (například načítání skripty, obrázky atd.)</span><span class="sxs-lookup"><span data-stu-id="209fb-193">Each request tooany resource in hello web role is considered (e.g. loading scripts, images, etc)</span></span>

## <a name="restricting-number-of-concurrent-accesses"></a><span data-ttu-id="209fb-194">Omezuje počet souběžných přístupů</span><span class="sxs-lookup"><span data-stu-id="209fb-194">Restricting number of concurrent accesses</span></span>
<span data-ttu-id="209fb-195">jsou Hello nastavení, které konfiguraci tohoto chování:</span><span class="sxs-lookup"><span data-stu-id="209fb-195">hello settings that configure this behavior are:</span></span>

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

<span data-ttu-id="209fb-196">Změňte DynamicIpRestrictionDenyByConcurrentRequests tootrue tooenable tuto ochranu.</span><span class="sxs-lookup"><span data-stu-id="209fb-196">Change DynamicIpRestrictionDenyByConcurrentRequests tootrue tooenable this protection.</span></span>

## <a name="restricting-rate-of-access"></a><span data-ttu-id="209fb-197">Omezení rychlost přístupu</span><span class="sxs-lookup"><span data-stu-id="209fb-197">Restricting rate of access</span></span>
<span data-ttu-id="209fb-198">jsou Hello nastavení, které konfiguraci tohoto chování:</span><span class="sxs-lookup"><span data-stu-id="209fb-198">hello settings that configure this behavior are:</span></span>

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-hello-response-tooa-denied-request"></a><span data-ttu-id="209fb-199">Konfigurace hello odpovědi tooa odmítla žádost</span><span class="sxs-lookup"><span data-stu-id="209fb-199">Configuring hello response tooa denied request</span></span>
<span data-ttu-id="209fb-200">Hello následující nastavení konfiguruje tooa odpovědi hello odmítla žádost:</span><span class="sxs-lookup"><span data-stu-id="209fb-200">hello following setting configures hello response tooa denied request:</span></span>

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
<span data-ttu-id="209fb-201">Pro ostatní podporované hodnoty naleznete v dokumentaci toohello pro dynamické IP zabezpečení ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="209fb-201">Refer toohello documentation for Dynamic IP Security in IIS for other supported values.</span></span>

## <a name="operations-for-configuring-service-certificates"></a><span data-ttu-id="209fb-202">Operace pro certifikáty služeb konfigurace</span><span class="sxs-lookup"><span data-stu-id="209fb-202">Operations for configuring service certificates</span></span>
<span data-ttu-id="209fb-203">Toto téma se týká jenom pro referenci.</span><span class="sxs-lookup"><span data-stu-id="209fb-203">This topic is for reference only.</span></span> <span data-ttu-id="209fb-204">Postupujte podle hello konfiguračních kroků uvedených v:</span><span class="sxs-lookup"><span data-stu-id="209fb-204">Please follow hello configuration steps outlined in:</span></span>

* <span data-ttu-id="209fb-205">Nakonfigurujte certifikát protokolu SSL hello</span><span class="sxs-lookup"><span data-stu-id="209fb-205">Configure hello SSL certificate</span></span>
* <span data-ttu-id="209fb-206">Nakonfigurujte klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="209fb-206">Configure client certificates</span></span>

## <a name="create-a-self-signed-certificate"></a><span data-ttu-id="209fb-207">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="209fb-207">Create a self-signed certificate</span></span>
<span data-ttu-id="209fb-208">Spusťte:</span><span class="sxs-lookup"><span data-stu-id="209fb-208">Execute:</span></span>

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

<span data-ttu-id="209fb-209">toocustomize:</span><span class="sxs-lookup"><span data-stu-id="209fb-209">toocustomize:</span></span>

* <span data-ttu-id="209fb-210">-n se adresa URL služby hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-210">-n with hello service URL.</span></span> <span data-ttu-id="209fb-211">Zástupné znaky ("CN = * .cloudapp .net") a jsou podporovány alternativní názvy (CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net").</span><span class="sxs-lookup"><span data-stu-id="209fb-211">Wildcards ("CN=*.cloudapp.net") and alternative names ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") are supported.</span></span>
* <span data-ttu-id="209fb-212">-e s datem vypršení platnosti certifikátu hello vytvořit silné heslo a zadejte jej po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="209fb-212">-e with hello certificate expiration date Create a strong password and specify it when prompted.</span></span>

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a><span data-ttu-id="209fb-213">Vytvořit soubor PFX pro certifikát SSL podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="209fb-213">Create PFX file for self-signed SSL certificate</span></span>
<span data-ttu-id="209fb-214">Spusťte:</span><span class="sxs-lookup"><span data-stu-id="209fb-214">Execute:</span></span>

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

<span data-ttu-id="209fb-215">Zadejte heslo a poté exportujte certifikát pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="209fb-215">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="209fb-216">Ano, exportovat soukromý klíč hello</span><span class="sxs-lookup"><span data-stu-id="209fb-216">Yes, export hello private key</span></span>
* <span data-ttu-id="209fb-217">Exportovat všechny rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="209fb-217">Export all extended properties</span></span>

## <a name="export-ssl-certificate-from-certificate-store"></a><span data-ttu-id="209fb-218">Exportovat certifikát SSL z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="209fb-218">Export SSL certificate from certificate store</span></span>
* <span data-ttu-id="209fb-219">Najít certifikát</span><span class="sxs-lookup"><span data-stu-id="209fb-219">Find certificate</span></span>
* <span data-ttu-id="209fb-220">Klikněte na tlačítko Akce -> všechny úlohy -> Export...</span><span class="sxs-lookup"><span data-stu-id="209fb-220">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="209fb-221">Export certifikátu do. Soubor PFX pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="209fb-221">Export certificate into a .PFX file with these options:</span></span>
  * <span data-ttu-id="209fb-222">Ano, exportovat soukromý klíč hello</span><span class="sxs-lookup"><span data-stu-id="209fb-222">Yes, export hello private key</span></span>
  * <span data-ttu-id="209fb-223">Pokud je to možné zahrnout všechny certifikáty na cestě k certifikátu hello * exportovat všechny rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="209fb-223">Include all certificates in hello certification path if possible *Export all extended properties</span></span>

## <a name="upload-ssl-certificate-toocloud-service"></a><span data-ttu-id="209fb-224">Nahrát služba toocloud certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="209fb-224">Upload SSL certificate toocloud service</span></span>
<span data-ttu-id="209fb-225">Nahrajte certifikát s hello existující nebo vygenerovat. Soubor PFX s hello dvojici klíčů SSL:</span><span class="sxs-lookup"><span data-stu-id="209fb-225">Upload certificate with hello existing or generated .PFX file with hello SSL key pair:</span></span>

* <span data-ttu-id="209fb-226">Zadejte heslo hello ochrana hello privátního klíče</span><span class="sxs-lookup"><span data-stu-id="209fb-226">Enter hello password protecting hello private key information</span></span>

## <a name="update-ssl-certificate-in-service-configuration-file"></a><span data-ttu-id="209fb-227">Aktualizovat certifikát SSL v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="209fb-227">Update SSL certificate in service configuration file</span></span>
<span data-ttu-id="209fb-228">Aktualizace hodnoty kryptografického otisku hello hello následující nastavení v konfiguračním souboru služby hello s hello kryptografický otisk certifikátu hello nahrán toohello cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="209fb-228">Update hello thumbprint value of hello following setting in hello service configuration file with hello thumbprint of hello certificate uploaded toohello cloud service:</span></span>

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a><span data-ttu-id="209fb-229">Import certifikační autorita protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="209fb-229">Import SSL certification authority</span></span>
<span data-ttu-id="209fb-230">Postupujte podle těchto kroků v všechny účet nebo počítač, který bude komunikovat se službou hello:</span><span class="sxs-lookup"><span data-stu-id="209fb-230">Follow these steps in all account/machine that will communicate with hello service:</span></span>

* <span data-ttu-id="209fb-231">Dvakrát klikněte na hello. Soubor CER v Průzkumníku Windows</span><span class="sxs-lookup"><span data-stu-id="209fb-231">Double-click hello .CER file in Windows Explorer</span></span>
* <span data-ttu-id="209fb-232">V dialogovém okně hello certifikátu klikněte na tlačítko Nainstalovat certifikát...</span><span class="sxs-lookup"><span data-stu-id="209fb-232">In hello Certificate dialog, click Install Certificate…</span></span>
* <span data-ttu-id="209fb-233">Import certifikátu do úložiště důvěryhodných kořenových certifikačních autorit hello</span><span class="sxs-lookup"><span data-stu-id="209fb-233">Import certificate into hello Trusted Root Certification Authorities store</span></span>

## <a name="turn-off-client-certificate-based-authentication"></a><span data-ttu-id="209fb-234">Vypněte ověřování na základě certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-234">Turn off client certificate-based authentication</span></span>
<span data-ttu-id="209fb-235">Je podporován pouze na základě certifikátu ověření klienta a jejím zakázáním umožní pro koncové body služby toohello veřejný přístup, pokud ostatní mechanismy jsou na místě (například Microsoft Azure Virtual Network).</span><span class="sxs-lookup"><span data-stu-id="209fb-235">Only client certificate-based authentication is supported and disabling it will allow for public access toohello service endpoints, unless other mechanisms are in place (e.g. Microsoft Azure Virtual Network).</span></span>

<span data-ttu-id="209fb-236">Změna těchto nastavení toofalse v hello služby konfigurační soubor tooturn hello funkce:</span><span class="sxs-lookup"><span data-stu-id="209fb-236">Change these settings toofalse in hello service configuration file tooturn hello feature off:</span></span>

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

<span data-ttu-id="209fb-237">V nastavení certifikátu hello certifikační Autority, zkopírujte hello certifikátu se stejným kryptografickým otiskem jako hello SSL:</span><span class="sxs-lookup"><span data-stu-id="209fb-237">Then, copy hello same thumbprint as hello SSL certificate in hello CA certificate setting:</span></span>

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a><span data-ttu-id="209fb-238">Vytvořit podepsaný certifikační autoritou</span><span class="sxs-lookup"><span data-stu-id="209fb-238">Create a self-signed certification authority</span></span>
<span data-ttu-id="209fb-239">Provést následující kroky toocreate tooact certifikát podepsaný svým držitelem jako certifikační autorita hello:</span><span class="sxs-lookup"><span data-stu-id="209fb-239">Execute hello following steps toocreate a self-signed certificate tooact as a Certification Authority:</span></span>

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

<span data-ttu-id="209fb-240">toocustomize ho</span><span class="sxs-lookup"><span data-stu-id="209fb-240">toocustomize it</span></span>

* <span data-ttu-id="209fb-241">-e s datem vypršení platnosti certifikační hello</span><span class="sxs-lookup"><span data-stu-id="209fb-241">-e with hello certification expiration date</span></span>

## <a name="find-ca-public-key"></a><span data-ttu-id="209fb-242">Najít veřejného klíče certifikační Autority</span><span class="sxs-lookup"><span data-stu-id="209fb-242">Find CA public key</span></span>
<span data-ttu-id="209fb-243">Všechny certifikáty klienta musí být vydány certifikační autoritou důvěryhodné službou hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-243">All client certificates must have been issued by a Certification Authority trusted by hello service.</span></span> <span data-ttu-id="209fb-244">Najít hello veřejného klíče toohello certifikační autorita, která vydala hello klientské certifikáty, které budou toobe pro ověřování v pořadí tooupload ho používají toohello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="209fb-244">Find hello public key toohello Certification Authority that issued hello client certificates that are going toobe used for authentication in order tooupload it toohello cloud service.</span></span>

<span data-ttu-id="209fb-245">Pokud není k dispozici soubor hello hello veřejným klíčem, můžete ho exportujte z úložiště certifikátů hello:</span><span class="sxs-lookup"><span data-stu-id="209fb-245">If hello file with hello public key is not available, export it from hello certificate store:</span></span>

* <span data-ttu-id="209fb-246">Najít certifikát</span><span class="sxs-lookup"><span data-stu-id="209fb-246">Find certificate</span></span>
  * <span data-ttu-id="209fb-247">Vyhledejte klientský certifikát vydaný hello stejné certifikační autority</span><span class="sxs-lookup"><span data-stu-id="209fb-247">Search for a client certificate issued by hello same Certification Authority</span></span>
* <span data-ttu-id="209fb-248">Dvakrát klikněte na certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-248">Double-click hello certificate.</span></span>
* <span data-ttu-id="209fb-249">Vyberte kartu hello cestě k certifikátu v dialogovém okně pro certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-249">Select hello Certification Path tab in hello Certificate dialog.</span></span>
* <span data-ttu-id="209fb-250">Dvakrát klikněte na položku hello certifikační Autority v cestě hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-250">Double-click hello CA entry in hello path.</span></span>
* <span data-ttu-id="209fb-251">Poznamenejte hello vlastnosti certifikátu.</span><span class="sxs-lookup"><span data-stu-id="209fb-251">Take notes of hello certificate properties.</span></span>
* <span data-ttu-id="209fb-252">Zavřít hello **certifikát** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="209fb-252">Close hello **Certificate** dialog.</span></span>
* <span data-ttu-id="209fb-253">Najít certifikát</span><span class="sxs-lookup"><span data-stu-id="209fb-253">Find certificate</span></span>
  * <span data-ttu-id="209fb-254">Vyhledejte hello certifikační Autority uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="209fb-254">Search for hello CA noted above.</span></span>
* <span data-ttu-id="209fb-255">Klikněte na tlačítko Akce -> všechny úlohy -> Export...</span><span class="sxs-lookup"><span data-stu-id="209fb-255">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="209fb-256">Export certifikátu do. CER pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="209fb-256">Export certificate into a .CER with these options:</span></span>
  * <span data-ttu-id="209fb-257">**Ne, neexportovat privátní klíč hello**</span><span class="sxs-lookup"><span data-stu-id="209fb-257">**No, do not export hello private key**</span></span>
  * <span data-ttu-id="209fb-258">Pokud je to možné zahrnout všechny certifikáty na cestě k certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-258">Include all certificates in hello certification path if possible.</span></span>
  * <span data-ttu-id="209fb-259">Exportujte všechny rozšířené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="209fb-259">Export all extended properties.</span></span>

## <a name="upload-ca-certificate-toocloud-service"></a><span data-ttu-id="209fb-260">Nahrát služby toocloud certifikát certifikační Autority</span><span class="sxs-lookup"><span data-stu-id="209fb-260">Upload CA certificate toocloud service</span></span>
<span data-ttu-id="209fb-261">Nahrajte certifikát s hello existující nebo vygenerovat. Soubor CER s veřejným klíčem hello certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="209fb-261">Upload certificate with hello existing or generated .CER file with hello CA public key.</span></span>

## <a name="update-ca-certificate-in-service-configuration-file"></a><span data-ttu-id="209fb-262">Certifikát certifikační Autority aktualizace v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="209fb-262">Update CA certificate in service configuration file</span></span>
<span data-ttu-id="209fb-263">Aktualizace hodnoty kryptografického otisku hello hello následující nastavení v konfiguračním souboru služby hello s hello kryptografický otisk certifikátu hello nahrán toohello cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="209fb-263">Update hello thumbprint value of hello following setting in hello service configuration file with hello thumbprint of hello certificate uploaded toohello cloud service:</span></span>

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

<span data-ttu-id="209fb-264">Aktualizujte hodnotu hello hello následující nastavení se hello stejné kryptografický otisk:</span><span class="sxs-lookup"><span data-stu-id="209fb-264">Update hello value of hello following setting with hello same thumbprint:</span></span>

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a><span data-ttu-id="209fb-265">Vystavovat certifikáty klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-265">Issue client certificates</span></span>
<span data-ttu-id="209fb-266">Každé jednotlivé autorizovaný tooaccess hello služby by měl mít klientský certifikát vydaný pro his/hers výhradní použití a by si měli vybrat, že his/hers vlastní silné heslo tooprotect jeho privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="209fb-266">Each individual authorized tooaccess hello service should have a client certificate issued for his/hers exclusive use and should choose his/hers own strong password tooprotect its private key.</span></span> 

<span data-ttu-id="209fb-267">Následující kroky Hello musí být spuštěn v hello byl vygenerované a uložené stejný počítač, kde hello certifikátu podepsaného svým držitelem certifikační Autority:</span><span class="sxs-lookup"><span data-stu-id="209fb-267">hello following steps must be executed in hello same machine where hello self-signed CA certificate was generated and stored:</span></span>

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

<span data-ttu-id="209fb-268">Přizpůsobení:</span><span class="sxs-lookup"><span data-stu-id="209fb-268">Customizing:</span></span>

* <span data-ttu-id="209fb-269">-n s ID pro toohello klienta, která bude ověřena pomocí tohoto certifikátu</span><span class="sxs-lookup"><span data-stu-id="209fb-269">-n with an ID for toohello client that will be authenticated with this certificate</span></span>
* <span data-ttu-id="209fb-270">-e s datem vypršení platnosti certifikátu hello</span><span class="sxs-lookup"><span data-stu-id="209fb-270">-e with hello certificate expiration date</span></span>
* <span data-ttu-id="209fb-271">MyID.pvk a MyID.cer s jedinečné názvy souborů pro tento certifikát klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-271">MyID.pvk and MyID.cer with unique filenames for this client certificate</span></span>

<span data-ttu-id="209fb-272">Tento příkaz zobrazí výzvu pro heslo toobe vytvořen a pak se použije jednou.</span><span class="sxs-lookup"><span data-stu-id="209fb-272">This command will prompt for a password toobe created and then used once.</span></span> <span data-ttu-id="209fb-273">Použijte silné heslo.</span><span class="sxs-lookup"><span data-stu-id="209fb-273">Use a strong password.</span></span>

## <a name="create-pfx-files-for-client-certificates"></a><span data-ttu-id="209fb-274">Vytvářet soubory PFX pro klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="209fb-274">Create PFX files for client certificates</span></span>
<span data-ttu-id="209fb-275">Pro každý certifikát generovaného klienta spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="209fb-275">For each generated client certificate, execute:</span></span>

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

<span data-ttu-id="209fb-276">Přizpůsobení:</span><span class="sxs-lookup"><span data-stu-id="209fb-276">Customizing:</span></span>

    MyID.pvk and MyID.cer with hello filename for hello client certificate

<span data-ttu-id="209fb-277">Zadejte heslo a poté exportujte certifikát pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="209fb-277">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="209fb-278">Ano, exportovat soukromý klíč hello</span><span class="sxs-lookup"><span data-stu-id="209fb-278">Yes, export hello private key</span></span>
* <span data-ttu-id="209fb-279">Exportovat všechny rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="209fb-279">Export all extended properties</span></span>
* <span data-ttu-id="209fb-280">jednotlivé toowhom Hello, kterou se vydává tento certifikát by měl vybrat heslo export hello</span><span class="sxs-lookup"><span data-stu-id="209fb-280">hello individual toowhom this certificate is being issued should choose hello export password</span></span>

## <a name="import-client-certificate"></a><span data-ttu-id="209fb-281">Import certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-281">Import client certificate</span></span>
<span data-ttu-id="209fb-282">Každého uživatele, pro kterého klientský certifikát vystavil naimportovat pár klíčů hello v hello počítače si pomocí toocommunicate službou hello:</span><span class="sxs-lookup"><span data-stu-id="209fb-282">Each individual for whom a client certificate has been issued should import hello key pair in hello machines he/she will use toocommunicate with hello service:</span></span>

* <span data-ttu-id="209fb-283">Dvakrát klikněte na hello. Soubor PFX v Průzkumníku Windows</span><span class="sxs-lookup"><span data-stu-id="209fb-283">Double-click hello .PFX file in Windows Explorer</span></span>
* <span data-ttu-id="209fb-284">Import certifikátu do hello osobní úložiště s alespoň tuto možnost:</span><span class="sxs-lookup"><span data-stu-id="209fb-284">Import certificate into hello Personal store with at least this option:</span></span>
  * <span data-ttu-id="209fb-285">Zahrnout všechny rozšířené vlastnosti zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="209fb-285">Include all extended properties checked</span></span>

## <a name="copy-client-certificate-thumbprints"></a><span data-ttu-id="209fb-286">Zkopírujte kryptografické otisky certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-286">Copy client certificate thumbprints</span></span>
<span data-ttu-id="209fb-287">Každého uživatele, pro kterého klientský certifikát vystavil musí postupujte podle těchto kroků v pořadí tooobtain hello kryptografický otisk his/hers certifikát, který bude přidán konfigurační soubor služby toohello:</span><span class="sxs-lookup"><span data-stu-id="209fb-287">Each individual for whom a client certificate has been issued must follow these steps in order tooobtain hello thumbprint of his/hers certificate which will be added toohello service configuration file:</span></span>

* <span data-ttu-id="209fb-288">Spustit certmgr.exe</span><span class="sxs-lookup"><span data-stu-id="209fb-288">Run certmgr.exe</span></span>
* <span data-ttu-id="209fb-289">Vyberte kartu Osobní hello</span><span class="sxs-lookup"><span data-stu-id="209fb-289">Select hello Personal tab</span></span>
* <span data-ttu-id="209fb-290">Klikněte dvakrát na certifikátu klienta hello toobe používaného pro ověřování</span><span class="sxs-lookup"><span data-stu-id="209fb-290">Double-click hello client certificate toobe used for authentication</span></span>
* <span data-ttu-id="209fb-291">V dialogu hello certifikátu, které se otevře vyberte kartu s podrobnostmi hello</span><span class="sxs-lookup"><span data-stu-id="209fb-291">In hello Certificate dialog that opens, select hello Details tab</span></span>
* <span data-ttu-id="209fb-292">Ujistěte se, že všechny zobrazit zobrazení</span><span class="sxs-lookup"><span data-stu-id="209fb-292">Make sure Show is displaying All</span></span>
* <span data-ttu-id="209fb-293">Vyberte hello pole s názvem kryptografický otisk v seznamu hello</span><span class="sxs-lookup"><span data-stu-id="209fb-293">Select hello field named Thumbprint in hello list</span></span>
* <span data-ttu-id="209fb-294">Zkopírujte hodnotu hello kryptografický otisk hello ** odstranit neviditelné znaky znakové sady Unicode před první číslice hello ** odstranit všechny mezery</span><span class="sxs-lookup"><span data-stu-id="209fb-294">Copy hello value of hello thumbprint ** Delete non-visible Unicode characters in front of hello first digit ** Delete all spaces</span></span>

## <a name="configure-allowed-clients-in-hello-service-configuration-file"></a><span data-ttu-id="209fb-295">Konfigurace klientů povolené v konfiguračním souboru služby hello</span><span class="sxs-lookup"><span data-stu-id="209fb-295">Configure Allowed clients in hello service configuration file</span></span>
<span data-ttu-id="209fb-296">Aktualizujte hodnotu hello hello následující nastavení v konfiguračním souboru služby hello čárkami oddělený seznam hello kryptografické otisky certifikátů klienta hello povolená služba toohello access:</span><span class="sxs-lookup"><span data-stu-id="209fb-296">Update hello value of hello following setting in hello service configuration file with a comma-separated list of hello thumbprints of hello client certificates allowed access toohello service:</span></span>

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a><span data-ttu-id="209fb-297">Konfigurace Kontrola odvolání certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="209fb-297">Configure client certificate revocation check</span></span>
<span data-ttu-id="209fb-298">Výchozí nastavení Hello nekontroluje s hello certifikační autority pro stav odvolání certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="209fb-298">hello default setting does not check with hello Certification Authority for client certificate revocation status.</span></span> <span data-ttu-id="209fb-299">kontroluje tooturn na hello, pokud hello certifikační autority, který vydal hello klientské certifikáty podporuje tyto kontroly, změnit následující nastavení na jednu z hodnot hello definované v hello X509RevocationMode výčtu hello:</span><span class="sxs-lookup"><span data-stu-id="209fb-299">tooturn on hello checks, if hello Certification Authority which issued hello client certificates supports such checks, change hello following setting with one of hello values defined in hello X509RevocationMode Enumeration:</span></span>

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a><span data-ttu-id="209fb-300">Vytvořit soubor PFX pro certifikáty podepsané svým držitelem šifrování</span><span class="sxs-lookup"><span data-stu-id="209fb-300">Create PFX file for self-signed encryption certificates</span></span>
<span data-ttu-id="209fb-301">Šifrovací certifikát spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="209fb-301">For an encryption certificate, execute:</span></span>

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

<span data-ttu-id="209fb-302">Přizpůsobení:</span><span class="sxs-lookup"><span data-stu-id="209fb-302">Customizing:</span></span>

    MyID.pvk and MyID.cer with hello filename for hello encryption certificate

<span data-ttu-id="209fb-303">Zadejte heslo a poté exportujte certifikát pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="209fb-303">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="209fb-304">Ano, exportovat soukromý klíč hello</span><span class="sxs-lookup"><span data-stu-id="209fb-304">Yes, export hello private key</span></span>
* <span data-ttu-id="209fb-305">Exportovat všechny rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="209fb-305">Export all extended properties</span></span>
* <span data-ttu-id="209fb-306">Hello heslo budete potřebovat při nahrávání hello certifikát toohello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="209fb-306">You will need hello password when uploading hello certificate toohello cloud service.</span></span>

## <a name="export-encryption-certificate-from-certificate-store"></a><span data-ttu-id="209fb-307">Exportovat šifrovací certifikát z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="209fb-307">Export encryption certificate from certificate store</span></span>
* <span data-ttu-id="209fb-308">Najít certifikát</span><span class="sxs-lookup"><span data-stu-id="209fb-308">Find certificate</span></span>
* <span data-ttu-id="209fb-309">Klikněte na tlačítko Akce -> všechny úlohy -> Export...</span><span class="sxs-lookup"><span data-stu-id="209fb-309">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="209fb-310">Export certifikátu do. Soubor PFX pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="209fb-310">Export certificate into a .PFX file with these options:</span></span> 
  * <span data-ttu-id="209fb-311">Ano, exportovat soukromý klíč hello</span><span class="sxs-lookup"><span data-stu-id="209fb-311">Yes, export hello private key</span></span>
  * <span data-ttu-id="209fb-312">Pokud je to možné zahrnout všechny certifikáty na cestě k certifikátu hello</span><span class="sxs-lookup"><span data-stu-id="209fb-312">Include all certificates in hello certification path if possible</span></span> 
* <span data-ttu-id="209fb-313">Exportovat všechny rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="209fb-313">Export all extended properties</span></span>

## <a name="upload-encryption-certificate-toocloud-service"></a><span data-ttu-id="209fb-314">Nahrát šifrovací certifikát toocloud služby</span><span class="sxs-lookup"><span data-stu-id="209fb-314">Upload encryption certificate toocloud service</span></span>
<span data-ttu-id="209fb-315">Nahrajte certifikát s hello existující nebo vygenerovat. Soubor PFX s pár klíčů hello šifrování:</span><span class="sxs-lookup"><span data-stu-id="209fb-315">Upload certificate with hello existing or generated .PFX file with hello encryption key pair:</span></span>

* <span data-ttu-id="209fb-316">Zadejte heslo hello ochrana hello privátního klíče</span><span class="sxs-lookup"><span data-stu-id="209fb-316">Enter hello password protecting hello private key information</span></span>

## <a name="update-encryption-certificate-in-service-configuration-file"></a><span data-ttu-id="209fb-317">Aktualizovat certifikát šifrování v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="209fb-317">Update encryption certificate in service configuration file</span></span>
<span data-ttu-id="209fb-318">Aktualizace hodnoty kryptografického otisku hello hello následující nastavení v konfiguračním souboru služby hello s hello kryptografický otisk certifikátu hello nahrán toohello cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="209fb-318">Update hello thumbprint value of hello following settings in hello service configuration file with hello thumbprint of hello certificate uploaded toohello cloud service:</span></span>

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a><span data-ttu-id="209fb-319">Běžné operace certifikátu</span><span class="sxs-lookup"><span data-stu-id="209fb-319">Common certificate operations</span></span>
* <span data-ttu-id="209fb-320">Nakonfigurujte certifikát protokolu SSL hello</span><span class="sxs-lookup"><span data-stu-id="209fb-320">Configure hello SSL certificate</span></span>
* <span data-ttu-id="209fb-321">Nakonfigurujte klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="209fb-321">Configure client certificates</span></span>

## <a name="find-certificate"></a><span data-ttu-id="209fb-322">Najít certifikát</span><span class="sxs-lookup"><span data-stu-id="209fb-322">Find certificate</span></span>
<span data-ttu-id="209fb-323">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="209fb-323">Follow these steps:</span></span>

1. <span data-ttu-id="209fb-324">Spusťte mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="209fb-324">Run mmc.exe.</span></span>
2. <span data-ttu-id="209fb-325">Soubor -> Přidat nebo odebrat modul Snap-in...</span><span class="sxs-lookup"><span data-stu-id="209fb-325">File -> Add/Remove Snap-in…</span></span>
3. <span data-ttu-id="209fb-326">Vyberte **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="209fb-326">Select **Certificates**.</span></span>
4. <span data-ttu-id="209fb-327">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="209fb-327">Click **Add**.</span></span>
5. <span data-ttu-id="209fb-328">Vyberte umístění úložiště certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-328">Choose hello certificate store location.</span></span>
6. <span data-ttu-id="209fb-329">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="209fb-329">Click **Finish**.</span></span>
7. <span data-ttu-id="209fb-330">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="209fb-330">Click **OK**.</span></span>
8. <span data-ttu-id="209fb-331">Rozbalte položku **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="209fb-331">Expand **Certificates**.</span></span>
9. <span data-ttu-id="209fb-332">Rozbalte uzel úložiště certifikátů hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-332">Expand hello certificate store node.</span></span>
10. <span data-ttu-id="209fb-333">Rozbalte hello certifikát podřízený uzel.</span><span class="sxs-lookup"><span data-stu-id="209fb-333">Expand hello Certificate child node.</span></span>
11. <span data-ttu-id="209fb-334">Vyberte certifikát v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-334">Select a certificate in hello list.</span></span>

## <a name="export-certificate"></a><span data-ttu-id="209fb-335">Export certifikátu</span><span class="sxs-lookup"><span data-stu-id="209fb-335">Export certificate</span></span>
<span data-ttu-id="209fb-336">V hello **Průvodce exportem certifikátu**:</span><span class="sxs-lookup"><span data-stu-id="209fb-336">In hello **Certificate Export Wizard**:</span></span>

1. <span data-ttu-id="209fb-337">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="209fb-337">Click **Next**.</span></span>
2. <span data-ttu-id="209fb-338">Vyberte **Ano**, pak **Export hello privátní klíč**.</span><span class="sxs-lookup"><span data-stu-id="209fb-338">Select **Yes**, then **Export hello private key**.</span></span>
3. <span data-ttu-id="209fb-339">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="209fb-339">Click **Next**.</span></span>
4. <span data-ttu-id="209fb-340">Vyberte formát souboru požadovaného výstup hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-340">Select hello desired output file format.</span></span>
5. <span data-ttu-id="209fb-341">Kontrola hello požadované možnosti.</span><span class="sxs-lookup"><span data-stu-id="209fb-341">Check hello desired options.</span></span>
6. <span data-ttu-id="209fb-342">Zkontrolujte **heslo**.</span><span class="sxs-lookup"><span data-stu-id="209fb-342">Check **Password**.</span></span>
7. <span data-ttu-id="209fb-343">Zadejte silné heslo a potvrďte ho.</span><span class="sxs-lookup"><span data-stu-id="209fb-343">Enter a strong password and confirm it.</span></span>
8. <span data-ttu-id="209fb-344">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="209fb-344">Click **Next**.</span></span>
9. <span data-ttu-id="209fb-345">Zadejte nebo vyhledejte název souboru, kde toostore hello certifikátu (použijte. Příponu PFX).</span><span class="sxs-lookup"><span data-stu-id="209fb-345">Type or browse a filename where toostore hello certificate (use a .PFX extension).</span></span>
10. <span data-ttu-id="209fb-346">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="209fb-346">Click **Next**.</span></span>
11. <span data-ttu-id="209fb-347">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="209fb-347">Click **Finish**.</span></span>
12. <span data-ttu-id="209fb-348">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="209fb-348">Click **OK**.</span></span>

## <a name="import-certificate"></a><span data-ttu-id="209fb-349">Importovat certifikát</span><span class="sxs-lookup"><span data-stu-id="209fb-349">Import certificate</span></span>
<span data-ttu-id="209fb-350">V Průvodci importem certifikátu hello:</span><span class="sxs-lookup"><span data-stu-id="209fb-350">In hello Certificate Import Wizard:</span></span>

1. <span data-ttu-id="209fb-351">Vyberte umístění úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-351">Select hello store location.</span></span>
   
   * <span data-ttu-id="209fb-352">Vyberte **aktuální uživatel** -li jen procesů spuštěných v rámci aktuální uživatel přistupuje službě hello</span><span class="sxs-lookup"><span data-stu-id="209fb-352">Select **Current User** if only processes running under current user will access hello service</span></span>
   * <span data-ttu-id="209fb-353">Vyberte **místního počítače** Pokud jiné procesy v tomto počítači bude mít přístup hello služby</span><span class="sxs-lookup"><span data-stu-id="209fb-353">Select **Local Machine** if other processes in this computer will access hello service</span></span>
2. <span data-ttu-id="209fb-354">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="209fb-354">Click **Next**.</span></span>
3. <span data-ttu-id="209fb-355">Pokud import ze souboru, zkontrolujte cestu k souboru hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-355">If importing from a file, confirm hello file path.</span></span>
4. <span data-ttu-id="209fb-356">Pokud import. Soubor PFX:</span><span class="sxs-lookup"><span data-stu-id="209fb-356">If importing a .PFX file:</span></span>
   1. <span data-ttu-id="209fb-357">Zadejte heslo hello ochranu privátního klíče hello</span><span class="sxs-lookup"><span data-stu-id="209fb-357">Enter hello password protecting hello private key</span></span>
   2. <span data-ttu-id="209fb-358">Vyberte možnosti importu</span><span class="sxs-lookup"><span data-stu-id="209fb-358">Select import options</span></span>
5. <span data-ttu-id="209fb-359">Vyberte certifikáty "Místo" v hello následující úložiště</span><span class="sxs-lookup"><span data-stu-id="209fb-359">Select "Place" certificates in hello following store</span></span>
6. <span data-ttu-id="209fb-360">Klikněte na **Browse** (Procházet).</span><span class="sxs-lookup"><span data-stu-id="209fb-360">Click **Browse**.</span></span>
7. <span data-ttu-id="209fb-361">Vyberte požadované úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-361">Select hello desired store.</span></span>
8. <span data-ttu-id="209fb-362">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="209fb-362">Click **Finish**.</span></span>
   
   * <span data-ttu-id="209fb-363">Pokud jste vybrali hello úložiště Důvěryhodné kořenové certifikační autority, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="209fb-363">If hello Trusted Root Certification Authority store was chosen, click **Yes**.</span></span>
9. <span data-ttu-id="209fb-364">Klikněte na tlačítko **OK** na všechny systémy windows dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="209fb-364">Click **OK** on all dialog windows.</span></span>

## <a name="upload-certificate"></a><span data-ttu-id="209fb-365">Nahrání certifikátu</span><span class="sxs-lookup"><span data-stu-id="209fb-365">Upload certificate</span></span>
<span data-ttu-id="209fb-366">V hello [portálu Azure](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="209fb-366">In hello [Azure Portal](https://portal.azure.com/)</span></span>

1. <span data-ttu-id="209fb-367">Vyberte **cloudových služeb**.</span><span class="sxs-lookup"><span data-stu-id="209fb-367">Select **Cloud Services**.</span></span>
2. <span data-ttu-id="209fb-368">Vyberte hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="209fb-368">Select hello cloud service.</span></span>
3. <span data-ttu-id="209fb-369">V horní nabídce hello, klikněte na tlačítko **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="209fb-369">On hello top menu, click **Certificates**.</span></span>
4. <span data-ttu-id="209fb-370">Na dolním panelu hello, klikněte na tlačítko **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="209fb-370">On hello bottom bar, click **Upload**.</span></span>
5. <span data-ttu-id="209fb-371">Vyberte soubor certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-371">Select hello certificate file.</span></span>
6. <span data-ttu-id="209fb-372">Pokud je. Soubor PFX souboru, zadejte heslo hello hello privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="209fb-372">If it is a .PFX file, enter hello password for hello private key.</span></span>
7. <span data-ttu-id="209fb-373">Po dokončení kopírování hello kryptografický otisk certifikátu z hello novou položku v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-373">Once completed, copy hello certificate thumbprint from hello new entry in hello list.</span></span>

## <a name="other-security-considerations"></a><span data-ttu-id="209fb-374">Další aspekty zabezpečení</span><span class="sxs-lookup"><span data-stu-id="209fb-374">Other security considerations</span></span>
<span data-ttu-id="209fb-375">nastavení SSL Hello popsané v tomto dokumentu zašifrování komunikace mezi hello služby a jeho klienty, pokud se používá koncový bod HTTPS hello.</span><span class="sxs-lookup"><span data-stu-id="209fb-375">hello SSL settings described in this document encrypt communication between hello service and its clients when hello HTTPS endpoint is used.</span></span> <span data-ttu-id="209fb-376">To je důležité, protože přihlašovací údaje pro přístup k databázi a další potenciálně citlivé informace, které jsou obsaženy v hello komunikace.</span><span class="sxs-lookup"><span data-stu-id="209fb-376">This is important since credentials for database access and potentially other sensitive information are contained in hello communication.</span></span> <span data-ttu-id="209fb-377">Upozorňujeme však, že služba hello ukládá vnitřní stav, včetně přihlašovacích údajů, v jeho interní tabulky v hello Microsoft Azure SQL database, která jste zadali pro metadata úložiště v rámci vašeho předplatného Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="209fb-377">Note, however, that hello service persists internal status, including credentials, in its internal tables in hello Microsoft Azure SQL database that you have provided for metadata storage in your Microsoft Azure subscription.</span></span> <span data-ttu-id="209fb-378">Tuto databázi byl definován jako součást hello následující nastavení v konfiguračním souboru služby (. Soubor .CSCFG):</span><span class="sxs-lookup"><span data-stu-id="209fb-378">That database was defined as part of hello following setting in your service configuration file (.CSCFG file):</span></span> 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

<span data-ttu-id="209fb-379">Přihlašovací údaje uložené v této databázi se šifrují.</span><span class="sxs-lookup"><span data-stu-id="209fb-379">Credentials stored in this database are encrypted.</span></span> <span data-ttu-id="209fb-380">Však jako osvědčený postup, zajistěte webové a pracovní role vaše nasazení služby jsou uchovány až toodate a co se mají přístup toohello metadata databáze a hello certifikátu se používá k šifrování a dešifrování uložené přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="209fb-380">However, as a best practice, ensure that both web and worker roles of your service deployments are kept up toodate and secure as they both have access toohello metadata database and hello certificate used for encryption and decryption of stored credentials.</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

