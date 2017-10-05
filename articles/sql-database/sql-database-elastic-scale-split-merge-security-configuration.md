---
title: "Konfigurace zabezpečení rozdělení sloučení | Microsoft Docs"
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
ms.openlocfilehash: 7e6ccf51a4b75eef16a7df5c1a1018954af8e5dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="split-merge-security-configuration"></a><span data-ttu-id="7ddf1-103">Konfigurace zabezpečení rozdělení sloučení</span><span class="sxs-lookup"><span data-stu-id="7ddf1-103">Split-merge security configuration</span></span>
<span data-ttu-id="7ddf1-104">Pokud chcete používat službu rozdělení či sloučení, musíte nakonfigurovat správně zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-104">To use the Split/Merge service, you must correctly configure security.</span></span> <span data-ttu-id="7ddf1-105">Služba je součástí funkce elastické škálování sady Microsoft Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-105">The service is part of the Elastic Scale feature of Microsoft Azure SQL Database.</span></span> <span data-ttu-id="7ddf1-106">Další informace najdete v tématu [elastické škálování rozdělení a sloučení kurz služby](sql-database-elastic-scale-configure-deploy-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="7ddf1-106">For more information, see [Elastic Scale Split and Merge Service Tutorial](sql-database-elastic-scale-configure-deploy-split-and-merge.md).</span></span>

## <a name="configuring-certificates"></a><span data-ttu-id="7ddf1-107">Konfigurace certifikátů</span><span class="sxs-lookup"><span data-stu-id="7ddf1-107">Configuring certificates</span></span>
<span data-ttu-id="7ddf1-108">Certifikáty jsou nakonfigurovat dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-108">Certificates are configured in two ways.</span></span> 

1. [<span data-ttu-id="7ddf1-109">Konfigurovat certifikát protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="7ddf1-109">To Configure the SSL Certificate</span></span>](#to-configure-the-ssl-certificate)
2. [<span data-ttu-id="7ddf1-110">Ke konfiguraci klientských certifikátů</span><span class="sxs-lookup"><span data-stu-id="7ddf1-110">To Configure Client Certificates</span></span>](#to-configure-client-certificates) 

## <a name="to-obtain-certificates"></a><span data-ttu-id="7ddf1-111">Chcete-li získat certifikáty</span><span class="sxs-lookup"><span data-stu-id="7ddf1-111">To obtain certificates</span></span>
<span data-ttu-id="7ddf1-112">Certifikáty nelze získat z veřejné certifikační autority (CA) nebo [certifikát služby systému Windows](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx).</span><span class="sxs-lookup"><span data-stu-id="7ddf1-112">Certificates can be obtained from public Certificate Authorities (CAs) or from the [Windows Certificate Service](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx).</span></span> <span data-ttu-id="7ddf1-113">Tyto jsou upřednostňované metody k získání certifikátů.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-113">These are the preferred methods to obtain certificates.</span></span>

<span data-ttu-id="7ddf1-114">Pokud nejsou k dispozici tyto možnosti, můžete vygenerovat **certifikáty podepsané svým držitelem**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-114">If those options are not available, you can generate **self-signed certificates**.</span></span>

## <a name="tools-to-generate-certificates"></a><span data-ttu-id="7ddf1-115">Nástroje pro generování certifikátů</span><span class="sxs-lookup"><span data-stu-id="7ddf1-115">Tools to generate certificates</span></span>
* [<span data-ttu-id="7ddf1-116">MakeCert.exe</span><span class="sxs-lookup"><span data-stu-id="7ddf1-116">makecert.exe</span></span>](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [<span data-ttu-id="7ddf1-117">Pvk2pfx.exe</span><span class="sxs-lookup"><span data-stu-id="7ddf1-117">pvk2pfx.exe</span></span>](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a><span data-ttu-id="7ddf1-118">Ke spuštění nástroje</span><span class="sxs-lookup"><span data-stu-id="7ddf1-118">To run the tools</span></span>
* <span data-ttu-id="7ddf1-119">Z příkazového řádku pro vývojáře Visual Studia, najdete v části [Visual Studio – příkazový řádek](http://msdn.microsoft.com/library/ms229859.aspx)</span><span class="sxs-lookup"><span data-stu-id="7ddf1-119">From a Developer Command Prompt for Visual Studios, see [Visual Studio Command Prompt](http://msdn.microsoft.com/library/ms229859.aspx)</span></span> 
  
    <span data-ttu-id="7ddf1-120">Pokud nainstalovaná, přejděte na:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-120">If installed, go to:</span></span>
  
        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 
* <span data-ttu-id="7ddf1-121">Získat WDK z [Windows 8.1: stažení sad a nástrojů](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)</span><span class="sxs-lookup"><span data-stu-id="7ddf1-121">Get the WDK from [Windows 8.1: Download kits and tools](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)</span></span>

## <a name="to-configure-the-ssl-certificate"></a><span data-ttu-id="7ddf1-122">Konfigurovat certifikát protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="7ddf1-122">To configure the SSL certificate</span></span>
<span data-ttu-id="7ddf1-123">Je vyžadován certifikát SSL k zašifrování komunikace a ověřování serveru.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-123">A SSL certificate is required to encrypt the communication and authenticate the server.</span></span> <span data-ttu-id="7ddf1-124">Zvolte nejvíce použít následující tři scénáře a spusťte všechny jeho kroky:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-124">Choose the most applicable of the three scenarios below, and execute all its steps:</span></span>

### <a name="create-a-new-self-signed-certificate"></a><span data-ttu-id="7ddf1-125">Vytvořit nový certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="7ddf1-125">Create a new self-signed certificate</span></span>
1. [<span data-ttu-id="7ddf1-126">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="7ddf1-126">Create a Self-Signed Certificate</span></span>](#create-a-self-signed-certificate)
2. [<span data-ttu-id="7ddf1-127">Vytvoření souboru PFX pro certifikát SSL podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="7ddf1-127">Create PFX file for Self-Signed SSL Certificate</span></span>](#create-pfx-file-for-self-signed-ssl-certificate)
3. [<span data-ttu-id="7ddf1-128">Nahrajte certifikát SSL pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-128">Upload SSL Certificate to Cloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
4. [<span data-ttu-id="7ddf1-129">Aktualizovat certifikát SSL v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-129">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)
5. [<span data-ttu-id="7ddf1-130">Import certifikační autorita protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="7ddf1-130">Import SSL Certification Authority</span></span>](#import-ssl-certification-authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a><span data-ttu-id="7ddf1-131">Chcete-li použít stávající certifikát z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-131">To use an existing certificate from the certificate store</span></span>
1. [<span data-ttu-id="7ddf1-132">Exportovat certifikát SSL z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-132">Export SSL Certificate From Certificate Store</span></span>](#export-ssl-certificate-from-certificate-store)
2. [<span data-ttu-id="7ddf1-133">Nahrajte certifikát SSL pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-133">Upload SSL Certificate to Cloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
3. [<span data-ttu-id="7ddf1-134">Aktualizovat certifikát SSL v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-134">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a><span data-ttu-id="7ddf1-135">Chcete-li použít existující certifikát v souboru PFX</span><span class="sxs-lookup"><span data-stu-id="7ddf1-135">To use an existing certificate in a PFX file</span></span>
1. [<span data-ttu-id="7ddf1-136">Nahrajte certifikát SSL pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-136">Upload SSL Certificate to Cloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
2. [<span data-ttu-id="7ddf1-137">Aktualizovat certifikát SSL v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-137">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)

## <a name="to-configure-client-certificates"></a><span data-ttu-id="7ddf1-138">Ke konfiguraci klientských certifikátů</span><span class="sxs-lookup"><span data-stu-id="7ddf1-138">To configure client certificates</span></span>
<span data-ttu-id="7ddf1-139">Klientské certifikáty jsou požadovány k ověřování žádostí ve službě.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-139">Client certificates are required in order to authenticate requests to the service.</span></span> <span data-ttu-id="7ddf1-140">Zvolte nejvíce použít následující tři scénáře a spusťte všechny jeho kroky:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-140">Choose the most applicable of the three scenarios below, and execute all its steps:</span></span>

### <a name="turn-off-client-certificates"></a><span data-ttu-id="7ddf1-141">Vypnout klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="7ddf1-141">Turn off client certificates</span></span>
1. [<span data-ttu-id="7ddf1-142">Vypněte ověřování na základě certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-142">Turn Off Client Certificate-Based Authentication</span></span>](#turn-off-client-certificate-based-authentication)

### <a name="issue-new-self-signed-client-certificates"></a><span data-ttu-id="7ddf1-143">Vydat nové certifikáty podepsané svým držitelem klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-143">Issue new self-signed client certificates</span></span>
1. [<span data-ttu-id="7ddf1-144">Vytvořit podepsaný certifikační autoritou</span><span class="sxs-lookup"><span data-stu-id="7ddf1-144">Create a Self-Signed Certification Authority</span></span>](#create-a-self-signed-certification-authority)
2. [<span data-ttu-id="7ddf1-145">Nahrajte certifikát certifikační Autority do cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-145">Upload CA Certificate to Cloud Service</span></span>](#upload-ca-certificate-to-cloud-service)
3. [<span data-ttu-id="7ddf1-146">Aktualizovat certifikát certifikační Autority v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-146">Update CA Certificate in Service Configuration File</span></span>](#update-ca-certificate-in-service-configuration-file)
4. [<span data-ttu-id="7ddf1-147">Problém klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="7ddf1-147">Issue Client Certificates</span></span>](#issue-client-certificates)
5. [<span data-ttu-id="7ddf1-148">Vytvářet soubory PFX pro certifikáty klientů</span><span class="sxs-lookup"><span data-stu-id="7ddf1-148">Create PFX files for Client Certificates</span></span>](#create-pfx-files-for-client-certificates)
6. [<span data-ttu-id="7ddf1-149">Import certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-149">Import Client Certificate</span></span>](#Import-Client-Certificate)
7. [<span data-ttu-id="7ddf1-150">Zkopírujte kryptografické otisky certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-150">Copy Client Certificate Thumbprints</span></span>](#copy-client-certificate-thumbprints)
8. [<span data-ttu-id="7ddf1-151">Konfigurace klientů povolených v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-151">Configure Allowed Clients in the Service Configuration File</span></span>](#configure-allowed-clients-in-the-service-configuration-file)

### <a name="use-existing-client-certificates"></a><span data-ttu-id="7ddf1-152">Použít existující klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="7ddf1-152">Use existing client certificates</span></span>
1. [<span data-ttu-id="7ddf1-153">Najít veřejného klíče certifikační Autority</span><span class="sxs-lookup"><span data-stu-id="7ddf1-153">Find CA Public Key</span></span>](#find-ca-public-key)
2. [<span data-ttu-id="7ddf1-154">Nahrajte certifikát certifikační Autority do cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-154">Upload CA Certificate to Cloud Service</span></span>](#Upload-CA-certificate-to-cloud-service)
3. [<span data-ttu-id="7ddf1-155">Aktualizovat certifikát certifikační Autority v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-155">Update CA Certificate in Service Configuration File</span></span>](#Update-CA-Certificate-in-Service-Configuration-File)
4. [<span data-ttu-id="7ddf1-156">Zkopírujte kryptografické otisky certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-156">Copy Client Certificate Thumbprints</span></span>](#Copy-Client-Certificate-Thumbprints)
5. [<span data-ttu-id="7ddf1-157">Konfigurace klientů povolených v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-157">Configure Allowed Clients in the Service Configuration File</span></span>](#configure-allowed-clients-in-the-service-configuration-file)
6. [<span data-ttu-id="7ddf1-158">Konfigurace Kontrola odvolání certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-158">Configure Client Certificate Revocation Check</span></span>](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a><span data-ttu-id="7ddf1-159">Povolené IP adresy</span><span class="sxs-lookup"><span data-stu-id="7ddf1-159">Allowed IP addresses</span></span>
<span data-ttu-id="7ddf1-160">Přístup ke koncovým bodům služby lze omezit na konkrétní rozsahy IP adres.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-160">Access to the service endpoints can be restricted to specific ranges of IP addresses.</span></span>

## <a name="to-configure-encryption-for-the-store"></a><span data-ttu-id="7ddf1-161">Konfigurace šifrování pro úložiště</span><span class="sxs-lookup"><span data-stu-id="7ddf1-161">To configure encryption for the store</span></span>
<span data-ttu-id="7ddf1-162">K šifrování přihlašovacích údajů, které jsou uložené v úložišti metadat je vyžadován certifikát.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-162">A certificate is required to encrypt the credentials that are stored in the metadata store.</span></span> <span data-ttu-id="7ddf1-163">Zvolte nejvíce použít následující tři scénáře a spusťte všechny jeho kroky:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-163">Choose the most applicable of the three scenarios below, and execute all its steps:</span></span>

### <a name="use-a-new-self-signed-certificate"></a><span data-ttu-id="7ddf1-164">Použití nového certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="7ddf1-164">Use a new self-signed certificate</span></span>
1. [<span data-ttu-id="7ddf1-165">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="7ddf1-165">Create a Self-Signed Certificate</span></span>](#create-a-self-signed-certificate)
2. [<span data-ttu-id="7ddf1-166">Vytvořit soubor PFX pro šifrovací certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="7ddf1-166">Create PFX file for Self-Signed Encryption Certificate</span></span>](#create-pfx-file-for-self-signed-ssl-certificate)
3. [<span data-ttu-id="7ddf1-167">Nahrát šifrovací certifikát do cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-167">Upload Encryption Certificate to Cloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
4. [<span data-ttu-id="7ddf1-168">Aktualizovat certifikát šifrování v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-168">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a><span data-ttu-id="7ddf1-169">Použít stávající certifikát z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-169">Use an existing certificate from the certificate store</span></span>
1. [<span data-ttu-id="7ddf1-170">Exportovat šifrovací certifikát z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-170">Export Encryption Certificate From Certificate Store</span></span>](#export-encryption-certificate-from-certificate-store)
2. [<span data-ttu-id="7ddf1-171">Nahrát šifrovací certifikát do cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-171">Upload Encryption Certificate to Cloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
3. [<span data-ttu-id="7ddf1-172">Aktualizovat certifikát šifrování v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-172">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a><span data-ttu-id="7ddf1-173">Použít stávající certifikát v souboru PFX</span><span class="sxs-lookup"><span data-stu-id="7ddf1-173">Use an existing certificate in a PFX file</span></span>
1. [<span data-ttu-id="7ddf1-174">Nahrát šifrovací certifikát do cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-174">Upload Encryption Certificate to Cloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
2. [<span data-ttu-id="7ddf1-175">Aktualizovat certifikát šifrování v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-175">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

## <a name="the-default-configuration"></a><span data-ttu-id="7ddf1-176">Výchozí konfigurace</span><span class="sxs-lookup"><span data-stu-id="7ddf1-176">The default configuration</span></span>
<span data-ttu-id="7ddf1-177">Výchozí konfigurace odmítne všechny přístup ke koncovému bodu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-177">The default configuration denies all access to the HTTP endpoint.</span></span> <span data-ttu-id="7ddf1-178">Toto je doporučená nastavení, protože žádosti, které chcete tyto koncové body mohou provádět citlivé údaje, jako jsou přihlašovací údaje databáze.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-178">This is the recommended setting, since the requests to these endpoints may carry sensitive information like database credentials.</span></span>
<span data-ttu-id="7ddf1-179">Výchozí konfigurace umožňuje veškerý přístup ke koncovému bodu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-179">The default configuration allows all access to the HTTPS endpoint.</span></span> <span data-ttu-id="7ddf1-180">Toto nastavení může být další s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-180">This setting may be restricted further.</span></span>

### <a name="changing-the-configuration"></a><span data-ttu-id="7ddf1-181">Změna konfigurace</span><span class="sxs-lookup"><span data-stu-id="7ddf1-181">Changing the Configuration</span></span>
<span data-ttu-id="7ddf1-182">Skupiny pravidly řízení přístupu, která se týkají a koncového bodu se konfigurují v  **<EndpointAcls>**  tématu **konfigurační soubor služby**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-182">The group of access control rules that apply to and endpoint are configured in the **<EndpointAcls>** section in the **service configuration file**.</span></span>

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

<span data-ttu-id="7ddf1-183">Pravidla ve skupině řízení přístupu jsou nakonfigurována v <AccessControl name=""> oddíl konfiguračního souboru služby.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-183">The rules in an access control group are configured in a <AccessControl name=""> section of the service configuration file.</span></span> 

<span data-ttu-id="7ddf1-184">Formát je vysvětleno v dokumentaci seznamů řízení přístupu k síti.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-184">The format is explained in Network Access Control Lists documentation.</span></span>
<span data-ttu-id="7ddf1-185">Například povolit jenom IP adresy v rozsahu 100.100.0.0 k 100.100.255.255 pro přístup k koncový bod HTTPS, pravidla bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-185">For example, to allow only IPs in the range 100.100.0.0 to 100.100.255.255 to access the HTTPS endpoint, the rules would look like this:</span></span>

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a><span data-ttu-id="7ddf1-186">Odmítnutí služby prevence</span><span class="sxs-lookup"><span data-stu-id="7ddf1-186">Denial of service prevention</span></span>
<span data-ttu-id="7ddf1-187">Existují dva různé mechanismy, které jsou podporovány rozpoznat a zabránit útokům odmítnutí služby:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-187">There are two different mechanisms supported to detect and prevent Denial of Service attacks:</span></span>

* <span data-ttu-id="7ddf1-188">Omezit počet souběžných požadavků na vzdáleného hostitele (ve výchozím nastavení vypnuté)</span><span class="sxs-lookup"><span data-stu-id="7ddf1-188">Restrict number of concurrent requests per remote host (off by default)</span></span>
* <span data-ttu-id="7ddf1-189">Omezit rychlost přístupu na vzdáleného hostitele (na ve výchozím nastavení)</span><span class="sxs-lookup"><span data-stu-id="7ddf1-189">Restrict rate of access per remote host (on by default)</span></span>

<span data-ttu-id="7ddf1-190">Tyto jsou založené na funkce, které jsou popsána v dynamické IP zabezpečení ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-190">These are based on the features further documented in Dynamic IP Security in IIS.</span></span> <span data-ttu-id="7ddf1-191">Při změně této konfigurace mějte na paměti následující faktory:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-191">When changing this configuration beware of the following factors:</span></span>

* <span data-ttu-id="7ddf1-192">Chování zařízení překlad síťových adres přes informace o vzdáleného hostitele a proxy servery</span><span class="sxs-lookup"><span data-stu-id="7ddf1-192">The behavior of proxies and Network Address Translation devices over the remote host information</span></span>
* <span data-ttu-id="7ddf1-193">Každý k jakémukoli prostředku ve webové roli, je žádost považována za (například načítání skripty, obrázky atd.)</span><span class="sxs-lookup"><span data-stu-id="7ddf1-193">Each request to any resource in the web role is considered (e.g. loading scripts, images, etc)</span></span>

## <a name="restricting-number-of-concurrent-accesses"></a><span data-ttu-id="7ddf1-194">Omezuje počet souběžných přístupů</span><span class="sxs-lookup"><span data-stu-id="7ddf1-194">Restricting number of concurrent accesses</span></span>
<span data-ttu-id="7ddf1-195">Jsou nastavení, které konfiguraci tohoto chování:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-195">The settings that configure this behavior are:</span></span>

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

<span data-ttu-id="7ddf1-196">Změnit DynamicIpRestrictionDenyByConcurrentRequests na hodnotu true, chcete-li povolit tuto ochranu.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-196">Change DynamicIpRestrictionDenyByConcurrentRequests to true to enable this protection.</span></span>

## <a name="restricting-rate-of-access"></a><span data-ttu-id="7ddf1-197">Omezení rychlost přístupu</span><span class="sxs-lookup"><span data-stu-id="7ddf1-197">Restricting rate of access</span></span>
<span data-ttu-id="7ddf1-198">Jsou nastavení, které konfiguraci tohoto chování:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-198">The settings that configure this behavior are:</span></span>

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a><span data-ttu-id="7ddf1-199">Konfigurace odpověď na žádost o odepření</span><span class="sxs-lookup"><span data-stu-id="7ddf1-199">Configuring the response to a denied request</span></span>
<span data-ttu-id="7ddf1-200">Toto nastavení konfiguruje odpověď na žádost o odepření:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-200">The following setting configures the response to a denied request:</span></span>

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
<span data-ttu-id="7ddf1-201">Naleznete v dokumentaci k zabezpečení dynamické IP ve službě IIS pro ostatní podporované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-201">Refer to the documentation for Dynamic IP Security in IIS for other supported values.</span></span>

## <a name="operations-for-configuring-service-certificates"></a><span data-ttu-id="7ddf1-202">Operace pro certifikáty služeb konfigurace</span><span class="sxs-lookup"><span data-stu-id="7ddf1-202">Operations for configuring service certificates</span></span>
<span data-ttu-id="7ddf1-203">Toto téma se týká jenom pro referenci.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-203">This topic is for reference only.</span></span> <span data-ttu-id="7ddf1-204">Postupujte podle kroků konfigurace uvedených v:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-204">Please follow the configuration steps outlined in:</span></span>

* <span data-ttu-id="7ddf1-205">Konfigurovat certifikát protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="7ddf1-205">Configure the SSL certificate</span></span>
* <span data-ttu-id="7ddf1-206">Nakonfigurujte klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="7ddf1-206">Configure client certificates</span></span>

## <a name="create-a-self-signed-certificate"></a><span data-ttu-id="7ddf1-207">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="7ddf1-207">Create a self-signed certificate</span></span>
<span data-ttu-id="7ddf1-208">Spusťte:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-208">Execute:</span></span>

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

<span data-ttu-id="7ddf1-209">Chcete-li přizpůsobit:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-209">To customize:</span></span>

* <span data-ttu-id="7ddf1-210">-n se adresa URL služby.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-210">-n with the service URL.</span></span> <span data-ttu-id="7ddf1-211">Zástupné znaky ("CN = * .cloudapp .net") a jsou podporovány alternativní názvy (CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net").</span><span class="sxs-lookup"><span data-stu-id="7ddf1-211">Wildcards ("CN=*.cloudapp.net") and alternative names ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") are supported.</span></span>
* <span data-ttu-id="7ddf1-212">-e s datem vypršení platnosti certifikátu vytvořit silné heslo a zadejte jej po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-212">-e with the certificate expiration date Create a strong password and specify it when prompted.</span></span>

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a><span data-ttu-id="7ddf1-213">Vytvořit soubor PFX pro certifikát SSL podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="7ddf1-213">Create PFX file for self-signed SSL certificate</span></span>
<span data-ttu-id="7ddf1-214">Spusťte:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-214">Execute:</span></span>

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

<span data-ttu-id="7ddf1-215">Zadejte heslo a poté exportujte certifikát pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-215">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="7ddf1-216">Ano, exportovat soukromý klíč</span><span class="sxs-lookup"><span data-stu-id="7ddf1-216">Yes, export the private key</span></span>
* <span data-ttu-id="7ddf1-217">Exportovat všechny rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7ddf1-217">Export all extended properties</span></span>

## <a name="export-ssl-certificate-from-certificate-store"></a><span data-ttu-id="7ddf1-218">Exportovat certifikát SSL z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-218">Export SSL certificate from certificate store</span></span>
* <span data-ttu-id="7ddf1-219">Najít certifikát</span><span class="sxs-lookup"><span data-stu-id="7ddf1-219">Find certificate</span></span>
* <span data-ttu-id="7ddf1-220">Klikněte na tlačítko Akce -> všechny úlohy -> Export...</span><span class="sxs-lookup"><span data-stu-id="7ddf1-220">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="7ddf1-221">Export certifikátu do. Soubor PFX pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-221">Export certificate into a .PFX file with these options:</span></span>
  * <span data-ttu-id="7ddf1-222">Ano, exportovat soukromý klíč</span><span class="sxs-lookup"><span data-stu-id="7ddf1-222">Yes, export the private key</span></span>
  * <span data-ttu-id="7ddf1-223">Pokud je to možné zahrnout všechny certifikáty na cestě k certifikátu * exportovat všechny rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7ddf1-223">Include all certificates in the certification path if possible *Export all extended properties</span></span>

## <a name="upload-ssl-certificate-to-cloud-service"></a><span data-ttu-id="7ddf1-224">Nahrajte certifikát SSL do cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-224">Upload SSL certificate to cloud service</span></span>
<span data-ttu-id="7ddf1-225">Nahrání certifikátu s existujícím nebo vygenerovat. Soubor PFX s dvojici klíčů SSL:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-225">Upload certificate with the existing or generated .PFX file with the SSL key pair:</span></span>

* <span data-ttu-id="7ddf1-226">Zadejte heslo ochranu privátního klíče</span><span class="sxs-lookup"><span data-stu-id="7ddf1-226">Enter the password protecting the private key information</span></span>

## <a name="update-ssl-certificate-in-service-configuration-file"></a><span data-ttu-id="7ddf1-227">Aktualizovat certifikát SSL v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-227">Update SSL certificate in service configuration file</span></span>
<span data-ttu-id="7ddf1-228">Aktualizujte hodnotu kryptografického otisku následující nastavení v konfiguračním souboru služby kryptografický otisk certifikátu nahrán do cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-228">Update the thumbprint value of the following setting in the service configuration file with the thumbprint of the certificate uploaded to the cloud service:</span></span>

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a><span data-ttu-id="7ddf1-229">Import certifikační autorita protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="7ddf1-229">Import SSL certification authority</span></span>
<span data-ttu-id="7ddf1-230">Postupujte podle těchto kroků v všechny účet nebo počítač, který bude komunikovat se službou:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-230">Follow these steps in all account/machine that will communicate with the service:</span></span>

* <span data-ttu-id="7ddf1-231">Dvakrát klikněte na. Soubor CER v Průzkumníku Windows</span><span class="sxs-lookup"><span data-stu-id="7ddf1-231">Double-click the .CER file in Windows Explorer</span></span>
* <span data-ttu-id="7ddf1-232">V dialogovém okně Certifikát klikněte na tlačítko Nainstalovat certifikát...</span><span class="sxs-lookup"><span data-stu-id="7ddf1-232">In the Certificate dialog, click Install Certificate…</span></span>
* <span data-ttu-id="7ddf1-233">Import certifikátu do úložiště důvěryhodných kořenových certifikačních autorit</span><span class="sxs-lookup"><span data-stu-id="7ddf1-233">Import certificate into the Trusted Root Certification Authorities store</span></span>

## <a name="turn-off-client-certificate-based-authentication"></a><span data-ttu-id="7ddf1-234">Vypněte ověřování na základě certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-234">Turn off client certificate-based authentication</span></span>
<span data-ttu-id="7ddf1-235">Je podporován pouze na základě certifikátu ověření klienta a jejím zakázáním umožní pro veřejný přístup ke koncovým bodům služby, pokud ostatní mechanismy jsou na místě (například Microsoft Azure Virtual Network).</span><span class="sxs-lookup"><span data-stu-id="7ddf1-235">Only client certificate-based authentication is supported and disabling it will allow for public access to the service endpoints, unless other mechanisms are in place (e.g. Microsoft Azure Virtual Network).</span></span>

<span data-ttu-id="7ddf1-236">Změna těchto nastavení na hodnotu false v konfiguračním souboru služby, chcete-li tuto funkci vypnout:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-236">Change these settings to false in the service configuration file to turn the feature off:</span></span>

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

<span data-ttu-id="7ddf1-237">V nastavení certifikátu certifikační Autority, zkopírujte stejným kryptografickým otiskem jako certifikát SSL:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-237">Then, copy the same thumbprint as the SSL certificate in the CA certificate setting:</span></span>

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a><span data-ttu-id="7ddf1-238">Vytvořit podepsaný certifikační autoritou</span><span class="sxs-lookup"><span data-stu-id="7ddf1-238">Create a self-signed certification authority</span></span>
<span data-ttu-id="7ddf1-239">Provést následující kroky, chcete-li vytvořit certifikát podepsaný svým držitelem tak, aby fungoval jako certifikační autority:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-239">Execute the following steps to create a self-signed certificate to act as a Certification Authority:</span></span>

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

<span data-ttu-id="7ddf1-240">Chcete-li přizpůsobit ho</span><span class="sxs-lookup"><span data-stu-id="7ddf1-240">To customize it</span></span>

* <span data-ttu-id="7ddf1-241">-e s datem vypršení platnosti certifikační</span><span class="sxs-lookup"><span data-stu-id="7ddf1-241">-e with the certification expiration date</span></span>

## <a name="find-ca-public-key"></a><span data-ttu-id="7ddf1-242">Najít veřejného klíče certifikační Autority</span><span class="sxs-lookup"><span data-stu-id="7ddf1-242">Find CA public key</span></span>
<span data-ttu-id="7ddf1-243">Všechny certifikáty klienta musí být vydány certifikační autoritou důvěryhodné službou.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-243">All client certificates must have been issued by a Certification Authority trusted by the service.</span></span> <span data-ttu-id="7ddf1-244">Nalezen veřejný klíč pro certifikační autoritu, která vydala certifikáty, které se chystáte použít k ověření, aby bylo možné nahrát do cloudové služby klienta.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-244">Find the public key to the Certification Authority that issued the client certificates that are going to be used for authentication in order to upload it to the cloud service.</span></span>

<span data-ttu-id="7ddf1-245">Pokud je soubor s veřejný klíč není k dispozici, můžete ho exportujte z úložiště certifikátů:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-245">If the file with the public key is not available, export it from the certificate store:</span></span>

* <span data-ttu-id="7ddf1-246">Najít certifikát</span><span class="sxs-lookup"><span data-stu-id="7ddf1-246">Find certificate</span></span>
  * <span data-ttu-id="7ddf1-247">Vyhledejte klientský certifikát vydaný certifikační autoritou stejné</span><span class="sxs-lookup"><span data-stu-id="7ddf1-247">Search for a client certificate issued by the same Certification Authority</span></span>
* <span data-ttu-id="7ddf1-248">Dvakrát klikněte na certifikát.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-248">Double-click the certificate.</span></span>
* <span data-ttu-id="7ddf1-249">Vyberte kartu cestě k certifikátu v dialogovém okně certifikát.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-249">Select the Certification Path tab in the Certificate dialog.</span></span>
* <span data-ttu-id="7ddf1-250">Dvakrát klikněte na položku certifikační Autority v cestě.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-250">Double-click the CA entry in the path.</span></span>
* <span data-ttu-id="7ddf1-251">Poznamenejte ve vlastnostech certifikátu.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-251">Take notes of the certificate properties.</span></span>
* <span data-ttu-id="7ddf1-252">Zavřít **certifikát** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-252">Close the **Certificate** dialog.</span></span>
* <span data-ttu-id="7ddf1-253">Najít certifikát</span><span class="sxs-lookup"><span data-stu-id="7ddf1-253">Find certificate</span></span>
  * <span data-ttu-id="7ddf1-254">Vyhledávání pro certifikační Autoritu uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-254">Search for the CA noted above.</span></span>
* <span data-ttu-id="7ddf1-255">Klikněte na tlačítko Akce -> všechny úlohy -> Export...</span><span class="sxs-lookup"><span data-stu-id="7ddf1-255">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="7ddf1-256">Export certifikátu do. CER pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-256">Export certificate into a .CER with these options:</span></span>
  * <span data-ttu-id="7ddf1-257">**Ne, neexportovat privátní klíč**</span><span class="sxs-lookup"><span data-stu-id="7ddf1-257">**No, do not export the private key**</span></span>
  * <span data-ttu-id="7ddf1-258">Pokud je to možné zahrnout všechny certifikáty na cestě k certifikátu.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-258">Include all certificates in the certification path if possible.</span></span>
  * <span data-ttu-id="7ddf1-259">Exportujte všechny rozšířené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-259">Export all extended properties.</span></span>

## <a name="upload-ca-certificate-to-cloud-service"></a><span data-ttu-id="7ddf1-260">Nahrajte certifikát certifikační Autority do cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-260">Upload CA certificate to cloud service</span></span>
<span data-ttu-id="7ddf1-261">Nahrání certifikátu s existujícím nebo vygenerovat. Soubor CER pomocí veřejného klíče certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-261">Upload certificate with the existing or generated .CER file with the CA public key.</span></span>

## <a name="update-ca-certificate-in-service-configuration-file"></a><span data-ttu-id="7ddf1-262">Certifikát certifikační Autority aktualizace v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-262">Update CA certificate in service configuration file</span></span>
<span data-ttu-id="7ddf1-263">Aktualizujte hodnotu kryptografického otisku následující nastavení v konfiguračním souboru služby kryptografický otisk certifikátu nahrán do cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-263">Update the thumbprint value of the following setting in the service configuration file with the thumbprint of the certificate uploaded to the cloud service:</span></span>

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

<span data-ttu-id="7ddf1-264">Aktualizujte hodnotu následující nastavení se stejným kryptografickým otiskem:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-264">Update the value of the following setting with the same thumbprint:</span></span>

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a><span data-ttu-id="7ddf1-265">Vystavovat certifikáty klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-265">Issue client certificates</span></span>
<span data-ttu-id="7ddf1-266">Každé jednotlivé oprávnění k přístupu ke službě by měl mít klientský certifikát vydaný pro his/hers výhradní použití a by si měli vybrat, že his/hers vlastní silné heslo k ochraně jeho privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-266">Each individual authorized to access the service should have a client certificate issued for his/hers exclusive use and should choose his/hers own strong password to protect its private key.</span></span> 

<span data-ttu-id="7ddf1-267">Ve stejném počítači, kde se certifikát podepsaný svým držitelem certifikační Autority vygenerované a uložené musí provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-267">The following steps must be executed in the same machine where the self-signed CA certificate was generated and stored:</span></span>

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

<span data-ttu-id="7ddf1-268">Přizpůsobení:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-268">Customizing:</span></span>

* <span data-ttu-id="7ddf1-269">-n s ID pro klientovi, který bude ověřen s tímto certifikátem</span><span class="sxs-lookup"><span data-stu-id="7ddf1-269">-n with an ID for to the client that will be authenticated with this certificate</span></span>
* <span data-ttu-id="7ddf1-270">-e s datem vypršení platnosti certifikátu</span><span class="sxs-lookup"><span data-stu-id="7ddf1-270">-e with the certificate expiration date</span></span>
* <span data-ttu-id="7ddf1-271">MyID.pvk a MyID.cer s jedinečné názvy souborů pro tento certifikát klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-271">MyID.pvk and MyID.cer with unique filenames for this client certificate</span></span>

<span data-ttu-id="7ddf1-272">Tento příkaz zobrazí výzvu k zadání hesla pro vytvořen a pak použít jednou.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-272">This command will prompt for a password to be created and then used once.</span></span> <span data-ttu-id="7ddf1-273">Použijte silné heslo.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-273">Use a strong password.</span></span>

## <a name="create-pfx-files-for-client-certificates"></a><span data-ttu-id="7ddf1-274">Vytvářet soubory PFX pro klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="7ddf1-274">Create PFX files for client certificates</span></span>
<span data-ttu-id="7ddf1-275">Pro každý certifikát generovaného klienta spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-275">For each generated client certificate, execute:</span></span>

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

<span data-ttu-id="7ddf1-276">Přizpůsobení:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-276">Customizing:</span></span>

    MyID.pvk and MyID.cer with the filename for the client certificate

<span data-ttu-id="7ddf1-277">Zadejte heslo a poté exportujte certifikát pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-277">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="7ddf1-278">Ano, exportovat soukromý klíč</span><span class="sxs-lookup"><span data-stu-id="7ddf1-278">Yes, export the private key</span></span>
* <span data-ttu-id="7ddf1-279">Exportovat všechny rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7ddf1-279">Export all extended properties</span></span>
* <span data-ttu-id="7ddf1-280">Jednotlivé, kterým se vydává tento certifikát by měl vybrat heslo export</span><span class="sxs-lookup"><span data-stu-id="7ddf1-280">The individual to whom this certificate is being issued should choose the export password</span></span>

## <a name="import-client-certificate"></a><span data-ttu-id="7ddf1-281">Import certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-281">Import client certificate</span></span>
<span data-ttu-id="7ddf1-282">Každého uživatele, pro kterého klientský certifikát vystavil naimportovat páru klíčů v počítačích, které si bude používat pro komunikaci se službou:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-282">Each individual for whom a client certificate has been issued should import the key pair in the machines he/she will use to communicate with the service:</span></span>

* <span data-ttu-id="7ddf1-283">Dvakrát klikněte na. Soubor PFX v Průzkumníku Windows</span><span class="sxs-lookup"><span data-stu-id="7ddf1-283">Double-click the .PFX file in Windows Explorer</span></span>
* <span data-ttu-id="7ddf1-284">Import certifikátu do osobní úložiště s alespoň tuto možnost:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-284">Import certificate into the Personal store with at least this option:</span></span>
  * <span data-ttu-id="7ddf1-285">Zahrnout všechny rozšířené vlastnosti zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="7ddf1-285">Include all extended properties checked</span></span>

## <a name="copy-client-certificate-thumbprints"></a><span data-ttu-id="7ddf1-286">Zkopírujte kryptografické otisky certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-286">Copy client certificate thumbprints</span></span>
<span data-ttu-id="7ddf1-287">Každého uživatele, pro kterého klientský certifikát vystavil nutné provést následující kroky, aby bylo možné získat kryptografický otisk his/hers certifikát, který přidá do konfiguračního souboru služby:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-287">Each individual for whom a client certificate has been issued must follow these steps in order to obtain the thumbprint of his/hers certificate which will be added to the service configuration file:</span></span>

* <span data-ttu-id="7ddf1-288">Spustit certmgr.exe</span><span class="sxs-lookup"><span data-stu-id="7ddf1-288">Run certmgr.exe</span></span>
* <span data-ttu-id="7ddf1-289">Vyberte kartu Osobní</span><span class="sxs-lookup"><span data-stu-id="7ddf1-289">Select the Personal tab</span></span>
* <span data-ttu-id="7ddf1-290">Dvakrát klikněte na certifikát klienta, který se má použít pro ověřování</span><span class="sxs-lookup"><span data-stu-id="7ddf1-290">Double-click the client certificate to be used for authentication</span></span>
* <span data-ttu-id="7ddf1-291">V dialogovém okně certifikátů, které se otevře vyberte kartu s podrobnostmi</span><span class="sxs-lookup"><span data-stu-id="7ddf1-291">In the Certificate dialog that opens, select the Details tab</span></span>
* <span data-ttu-id="7ddf1-292">Ujistěte se, že všechny zobrazit zobrazení</span><span class="sxs-lookup"><span data-stu-id="7ddf1-292">Make sure Show is displaying All</span></span>
* <span data-ttu-id="7ddf1-293">Vyberte pole v seznamu s názvem kryptografický otisk</span><span class="sxs-lookup"><span data-stu-id="7ddf1-293">Select the field named Thumbprint in the list</span></span>
* <span data-ttu-id="7ddf1-294">Zkopírujte hodnotu kryptografického otisku ** odstranit neviditelné znaky znakové sady Unicode před první číslice ** odstranit všechny mezery</span><span class="sxs-lookup"><span data-stu-id="7ddf1-294">Copy the value of the thumbprint ** Delete non-visible Unicode characters in front of the first digit ** Delete all spaces</span></span>

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a><span data-ttu-id="7ddf1-295">Konfigurace klientů povolené v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-295">Configure Allowed clients in the service configuration file</span></span>
<span data-ttu-id="7ddf1-296">Čárkami oddělený seznam kryptografické otisky certifikátů klienta povolený přístup ke službě aktualizujte hodnotu následující nastavení v konfiguračním souboru služby:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-296">Update the value of the following setting in the service configuration file with a comma-separated list of the thumbprints of the client certificates allowed access to the service:</span></span>

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a><span data-ttu-id="7ddf1-297">Konfigurace Kontrola odvolání certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="7ddf1-297">Configure client certificate revocation check</span></span>
<span data-ttu-id="7ddf1-298">Ve výchozím nastavení nekontroluje s certifikační autoritou pro stav odvolání certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-298">The default setting does not check with the Certification Authority for client certificate revocation status.</span></span> <span data-ttu-id="7ddf1-299">Chcete-li zapnout kontrol, pokud certifikační autorita, která vydala certifikáty klienta podporuje tyto kontroly, změňte následující nastavení jedna z hodnot fronty definovaných ve výčtu X509RevocationMode:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-299">To turn on the checks, if the Certification Authority which issued the client certificates supports such checks, change the following setting with one of the values defined in the X509RevocationMode Enumeration:</span></span>

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a><span data-ttu-id="7ddf1-300">Vytvořit soubor PFX pro certifikáty podepsané svým držitelem šifrování</span><span class="sxs-lookup"><span data-stu-id="7ddf1-300">Create PFX file for self-signed encryption certificates</span></span>
<span data-ttu-id="7ddf1-301">Šifrovací certifikát spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-301">For an encryption certificate, execute:</span></span>

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

<span data-ttu-id="7ddf1-302">Přizpůsobení:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-302">Customizing:</span></span>

    MyID.pvk and MyID.cer with the filename for the encryption certificate

<span data-ttu-id="7ddf1-303">Zadejte heslo a poté exportujte certifikát pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-303">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="7ddf1-304">Ano, exportovat soukromý klíč</span><span class="sxs-lookup"><span data-stu-id="7ddf1-304">Yes, export the private key</span></span>
* <span data-ttu-id="7ddf1-305">Exportovat všechny rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7ddf1-305">Export all extended properties</span></span>
* <span data-ttu-id="7ddf1-306">Heslo budete potřebovat při nahrávání certifikát do cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-306">You will need the password when uploading the certificate to the cloud service.</span></span>

## <a name="export-encryption-certificate-from-certificate-store"></a><span data-ttu-id="7ddf1-307">Exportovat šifrovací certifikát z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-307">Export encryption certificate from certificate store</span></span>
* <span data-ttu-id="7ddf1-308">Najít certifikát</span><span class="sxs-lookup"><span data-stu-id="7ddf1-308">Find certificate</span></span>
* <span data-ttu-id="7ddf1-309">Klikněte na tlačítko Akce -> všechny úlohy -> Export...</span><span class="sxs-lookup"><span data-stu-id="7ddf1-309">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="7ddf1-310">Export certifikátu do. Soubor PFX pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-310">Export certificate into a .PFX file with these options:</span></span> 
  * <span data-ttu-id="7ddf1-311">Ano, exportovat soukromý klíč</span><span class="sxs-lookup"><span data-stu-id="7ddf1-311">Yes, export the private key</span></span>
  * <span data-ttu-id="7ddf1-312">Pokud je to možné zahrnout všechny certifikáty na cestě k certifikátu</span><span class="sxs-lookup"><span data-stu-id="7ddf1-312">Include all certificates in the certification path if possible</span></span> 
* <span data-ttu-id="7ddf1-313">Exportovat všechny rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7ddf1-313">Export all extended properties</span></span>

## <a name="upload-encryption-certificate-to-cloud-service"></a><span data-ttu-id="7ddf1-314">Nahrát šifrovací certifikát do cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-314">Upload encryption certificate to cloud service</span></span>
<span data-ttu-id="7ddf1-315">Nahrání certifikátu s existujícím nebo vygenerovat. Soubor PFX s páru klíčů šifrování:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-315">Upload certificate with the existing or generated .PFX file with the encryption key pair:</span></span>

* <span data-ttu-id="7ddf1-316">Zadejte heslo ochranu privátního klíče</span><span class="sxs-lookup"><span data-stu-id="7ddf1-316">Enter the password protecting the private key information</span></span>

## <a name="update-encryption-certificate-in-service-configuration-file"></a><span data-ttu-id="7ddf1-317">Aktualizovat certifikát šifrování v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="7ddf1-317">Update encryption certificate in service configuration file</span></span>
<span data-ttu-id="7ddf1-318">Aktualizujte hodnotu kryptografického otisku následující nastavení v konfiguračním souboru služby kryptografický otisk certifikátu nahrán do cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-318">Update the thumbprint value of the following settings in the service configuration file with the thumbprint of the certificate uploaded to the cloud service:</span></span>

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a><span data-ttu-id="7ddf1-319">Běžné operace certifikátu</span><span class="sxs-lookup"><span data-stu-id="7ddf1-319">Common certificate operations</span></span>
* <span data-ttu-id="7ddf1-320">Konfigurovat certifikát protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="7ddf1-320">Configure the SSL certificate</span></span>
* <span data-ttu-id="7ddf1-321">Nakonfigurujte klientské certifikáty</span><span class="sxs-lookup"><span data-stu-id="7ddf1-321">Configure client certificates</span></span>

## <a name="find-certificate"></a><span data-ttu-id="7ddf1-322">Najít certifikát</span><span class="sxs-lookup"><span data-stu-id="7ddf1-322">Find certificate</span></span>
<span data-ttu-id="7ddf1-323">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-323">Follow these steps:</span></span>

1. <span data-ttu-id="7ddf1-324">Spusťte mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-324">Run mmc.exe.</span></span>
2. <span data-ttu-id="7ddf1-325">Soubor -> Přidat nebo odebrat modul Snap-in...</span><span class="sxs-lookup"><span data-stu-id="7ddf1-325">File -> Add/Remove Snap-in…</span></span>
3. <span data-ttu-id="7ddf1-326">Vyberte **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-326">Select **Certificates**.</span></span>
4. <span data-ttu-id="7ddf1-327">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-327">Click **Add**.</span></span>
5. <span data-ttu-id="7ddf1-328">Vyberte umístění úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-328">Choose the certificate store location.</span></span>
6. <span data-ttu-id="7ddf1-329">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-329">Click **Finish**.</span></span>
7. <span data-ttu-id="7ddf1-330">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-330">Click **OK**.</span></span>
8. <span data-ttu-id="7ddf1-331">Rozbalte položku **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-331">Expand **Certificates**.</span></span>
9. <span data-ttu-id="7ddf1-332">Rozbalte uzel úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-332">Expand the certificate store node.</span></span>
10. <span data-ttu-id="7ddf1-333">Rozbalte uzel podřízené certifikátu.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-333">Expand the Certificate child node.</span></span>
11. <span data-ttu-id="7ddf1-334">Vyberte certifikát v seznamu.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-334">Select a certificate in the list.</span></span>

## <a name="export-certificate"></a><span data-ttu-id="7ddf1-335">Export certifikátu</span><span class="sxs-lookup"><span data-stu-id="7ddf1-335">Export certificate</span></span>
<span data-ttu-id="7ddf1-336">V **Průvodce exportem certifikátu**:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-336">In the **Certificate Export Wizard**:</span></span>

1. <span data-ttu-id="7ddf1-337">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-337">Click **Next**.</span></span>
2. <span data-ttu-id="7ddf1-338">Vyberte **Ano**, pak **exportovat soukromý klíč**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-338">Select **Yes**, then **Export the private key**.</span></span>
3. <span data-ttu-id="7ddf1-339">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-339">Click **Next**.</span></span>
4. <span data-ttu-id="7ddf1-340">Vyberte formát požadovaný výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-340">Select the desired output file format.</span></span>
5. <span data-ttu-id="7ddf1-341">Zkontrolujte si požadované možnosti.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-341">Check the desired options.</span></span>
6. <span data-ttu-id="7ddf1-342">Zkontrolujte **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-342">Check **Password**.</span></span>
7. <span data-ttu-id="7ddf1-343">Zadejte silné heslo a potvrďte ho.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-343">Enter a strong password and confirm it.</span></span>
8. <span data-ttu-id="7ddf1-344">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-344">Click **Next**.</span></span>
9. <span data-ttu-id="7ddf1-345">Zadejte nebo vyhledejte název souboru pro uložení certifikátu (použijte. Příponu PFX).</span><span class="sxs-lookup"><span data-stu-id="7ddf1-345">Type or browse a filename where to store the certificate (use a .PFX extension).</span></span>
10. <span data-ttu-id="7ddf1-346">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-346">Click **Next**.</span></span>
11. <span data-ttu-id="7ddf1-347">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-347">Click **Finish**.</span></span>
12. <span data-ttu-id="7ddf1-348">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-348">Click **OK**.</span></span>

## <a name="import-certificate"></a><span data-ttu-id="7ddf1-349">Importovat certifikát</span><span class="sxs-lookup"><span data-stu-id="7ddf1-349">Import certificate</span></span>
<span data-ttu-id="7ddf1-350">V Průvodci importem certifikátu:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-350">In the Certificate Import Wizard:</span></span>

1. <span data-ttu-id="7ddf1-351">Vyberte umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-351">Select the store location.</span></span>
   
   * <span data-ttu-id="7ddf1-352">Vyberte **aktuální uživatel** -li jen procesů spuštěných v rámci aktuální uživatel přistupuje k službě</span><span class="sxs-lookup"><span data-stu-id="7ddf1-352">Select **Current User** if only processes running under current user will access the service</span></span>
   * <span data-ttu-id="7ddf1-353">Vyberte **místního počítače** Pokud jiné procesy v tomto počítači se přístup ke službě</span><span class="sxs-lookup"><span data-stu-id="7ddf1-353">Select **Local Machine** if other processes in this computer will access the service</span></span>
2. <span data-ttu-id="7ddf1-354">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-354">Click **Next**.</span></span>
3. <span data-ttu-id="7ddf1-355">Pokud import ze souboru, zkontrolujte cestu k souboru.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-355">If importing from a file, confirm the file path.</span></span>
4. <span data-ttu-id="7ddf1-356">Pokud import. Soubor PFX:</span><span class="sxs-lookup"><span data-stu-id="7ddf1-356">If importing a .PFX file:</span></span>
   1. <span data-ttu-id="7ddf1-357">Zadejte heslo ochranou privátního klíče</span><span class="sxs-lookup"><span data-stu-id="7ddf1-357">Enter the password protecting the private key</span></span>
   2. <span data-ttu-id="7ddf1-358">Vyberte možnosti importu</span><span class="sxs-lookup"><span data-stu-id="7ddf1-358">Select import options</span></span>
5. <span data-ttu-id="7ddf1-359">Vyberte certifikáty "Místo" v následujícím úložišti</span><span class="sxs-lookup"><span data-stu-id="7ddf1-359">Select "Place" certificates in the following store</span></span>
6. <span data-ttu-id="7ddf1-360">Klikněte na **Browse** (Procházet).</span><span class="sxs-lookup"><span data-stu-id="7ddf1-360">Click **Browse**.</span></span>
7. <span data-ttu-id="7ddf1-361">Vyberte požadované úložiště.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-361">Select the desired store.</span></span>
8. <span data-ttu-id="7ddf1-362">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-362">Click **Finish**.</span></span>
   
   * <span data-ttu-id="7ddf1-363">Pokud jste vybrali úložišti Důvěryhodné kořenové certifikační autority, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-363">If the Trusted Root Certification Authority store was chosen, click **Yes**.</span></span>
9. <span data-ttu-id="7ddf1-364">Klikněte na tlačítko **OK** na všechny systémy windows dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-364">Click **OK** on all dialog windows.</span></span>

## <a name="upload-certificate"></a><span data-ttu-id="7ddf1-365">Nahrání certifikátu</span><span class="sxs-lookup"><span data-stu-id="7ddf1-365">Upload certificate</span></span>
<span data-ttu-id="7ddf1-366">V [portálu Azure](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="7ddf1-366">In the [Azure Portal](https://portal.azure.com/)</span></span>

1. <span data-ttu-id="7ddf1-367">Vyberte **cloudových služeb**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-367">Select **Cloud Services**.</span></span>
2. <span data-ttu-id="7ddf1-368">Vyberte cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-368">Select the cloud service.</span></span>
3. <span data-ttu-id="7ddf1-369">V horní nabídce klikněte na tlačítko **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-369">On the top menu, click **Certificates**.</span></span>
4. <span data-ttu-id="7ddf1-370">Na dolním panelu klikněte na tlačítko **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-370">On the bottom bar, click **Upload**.</span></span>
5. <span data-ttu-id="7ddf1-371">Vyberte soubor certifikátu.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-371">Select the certificate file.</span></span>
6. <span data-ttu-id="7ddf1-372">Pokud je. Soubor PFX souboru, zadejte heslo pro privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-372">If it is a .PFX file, enter the password for the private key.</span></span>
7. <span data-ttu-id="7ddf1-373">Po dokončení, zkopírujte kryptografický otisk certifikátu z nové položky v seznamu.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-373">Once completed, copy the certificate thumbprint from the new entry in the list.</span></span>

## <a name="other-security-considerations"></a><span data-ttu-id="7ddf1-374">Další aspekty zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7ddf1-374">Other security considerations</span></span>
<span data-ttu-id="7ddf1-375">Nastavení protokolu SSL, které jsou popsané v tomto dokumentu šifrování komunikace mezi službou a jeho klienty, pokud koncový bod HTTPS se používá.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-375">The SSL settings described in this document encrypt communication between the service and its clients when the HTTPS endpoint is used.</span></span> <span data-ttu-id="7ddf1-376">To je důležité, protože přihlašovací údaje pro přístup k databázi a další potenciálně citlivé informace, které jsou obsaženy v komunikaci.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-376">This is important since credentials for database access and potentially other sensitive information are contained in the communication.</span></span> <span data-ttu-id="7ddf1-377">Upozorňujeme však, že služba ukládá vnitřní stav, včetně přihlašovacích údajů, v jeho interní tabulky v databázi Microsoft Azure SQL, který jste zadali pro metadata úložiště v rámci vašeho předplatného Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-377">Note, however, that the service persists internal status, including credentials, in its internal tables in the Microsoft Azure SQL database that you have provided for metadata storage in your Microsoft Azure subscription.</span></span> <span data-ttu-id="7ddf1-378">Tuto databázi byl definován jako součást následující nastavení v konfiguračním souboru služby (. Soubor .CSCFG):</span><span class="sxs-lookup"><span data-stu-id="7ddf1-378">That database was defined as part of the following setting in your service configuration file (.CSCFG file):</span></span> 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

<span data-ttu-id="7ddf1-379">Přihlašovací údaje uložené v této databázi se šifrují.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-379">Credentials stored in this database are encrypted.</span></span> <span data-ttu-id="7ddf1-380">Však jako osvědčený postup, zajistěte webové a pracovní role vaše nasazení služby jsou pořád aktuální a co se mají přístup k databázi metadat a certifikát použitý k šifrování a dešifrování uložené přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="7ddf1-380">However, as a best practice, ensure that both web and worker roles of your service deployments are kept up to date and secure as they both have access to the metadata database and the certificate used for encryption and decryption of stored credentials.</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

