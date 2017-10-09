---
title: "potíže s připojením aaaTroubleshoot Azure point-to-site | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot potíže s připojením point-to-site."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: genli
ms.openlocfilehash: 98d66074be62ad8c7153a903f69cb0d01f988cd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a><span data-ttu-id="4427c-103">Řešení potíží: Problémy Azure připojení point-to-site</span><span class="sxs-lookup"><span data-stu-id="4427c-103">Troubleshooting: Azure point-to-site connection problems</span></span>

<span data-ttu-id="4427c-104">Tento článek obsahuje seznam běžných problémů připojení point-to-site, které můžete zaznamenat.</span><span class="sxs-lookup"><span data-stu-id="4427c-104">This article lists common point-to-site connection problems that you might experience.</span></span> <span data-ttu-id="4427c-105">Popisuje také možné příčiny a řešení těchto problémů.</span><span class="sxs-lookup"><span data-stu-id="4427c-105">It also discusses possible causes and solutions for these problems.</span></span>

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a><span data-ttu-id="4427c-106">Chyba klienta sítě VPN: certifikát nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="4427c-106">VPN client error: A certificate could not be found</span></span>

### <a name="symptom"></a><span data-ttu-id="4427c-107">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4427c-107">Symptom</span></span>

<span data-ttu-id="4427c-108">Pokusíte-li pomocí klienta VPN hello tooconnect tooan virtuální síť Azure, můžete zobrazit hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="4427c-108">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="4427c-109">**Certifikát nebyl nalezen, který může být použit s Tento Extensible Authentication Protocol. (Chyba 798)**</span><span class="sxs-lookup"><span data-stu-id="4427c-109">**A certificate could not be found that can be used with this Extensible Authentication Protocol. (Error 798)**</span></span>

### <a name="cause"></a><span data-ttu-id="4427c-110">Příčina</span><span class="sxs-lookup"><span data-stu-id="4427c-110">Cause</span></span>

<span data-ttu-id="4427c-111">K tomuto problému dochází, pokud chybí certifikát klienta hello **certifikáty – aktuální User\Personal\Certificates**.</span><span class="sxs-lookup"><span data-stu-id="4427c-111">This problem occurs if hello client certificate is missing from **Certificates - Current User\Personal\Certificates**.</span></span>

### <a name="solution"></a><span data-ttu-id="4427c-112">Řešení</span><span class="sxs-lookup"><span data-stu-id="4427c-112">Solution</span></span>

<span data-ttu-id="4427c-113">Ujistěte se, že tento certifikát klienta hello je nainstalován v hello následující umístění úložiště certifikátů hello (Certmgr.msc):</span><span class="sxs-lookup"><span data-stu-id="4427c-113">Make sure that hello client certificate is installed in hello following location of hello Certificates store (Certmgr.msc):</span></span>
 
<span data-ttu-id="4427c-114">**Certifikáty – aktuální User\Personal\Certificates**</span><span class="sxs-lookup"><span data-stu-id="4427c-114">**Certificates - Current User\Personal\Certificates**</span></span>

<span data-ttu-id="4427c-115">Další informace o tom, jak tooinstall hello klientský certifikát, najdete v části [generování a exportu certifikátů pro připojení point-to-site](vpn-gateway-certificates-point-to-site.md).</span><span class="sxs-lookup"><span data-stu-id="4427c-115">For more information about how tooinstall hello client certificate, see [Generate and export certificates for point-to-site connections](vpn-gateway-certificates-point-to-site.md).</span></span>

> [!NOTE]
> <span data-ttu-id="4427c-116">Když importujete hello klientský certifikát, nevybírejte hello **povolit silnou ochranu privátního klíče** možnost.</span><span class="sxs-lookup"><span data-stu-id="4427c-116">When you import hello client certificate, do not select hello **Enable strong private key protection** option.</span></span>

## <a name="vpn-client-error-hello-message-received-was-unexpected-or-badly-formatted"></a><span data-ttu-id="4427c-117">Chyba klienta sítě VPN: byla přijata zpráva hello neočekávaná nebo chybně formátovaná</span><span class="sxs-lookup"><span data-stu-id="4427c-117">VPN client error: hello message received was unexpected or badly formatted</span></span>

### <a name="symptom"></a><span data-ttu-id="4427c-118">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4427c-118">Symptom</span></span>

<span data-ttu-id="4427c-119">Pokusíte-li pomocí klienta VPN hello tooconnect tooan virtuální síť Azure, můžete zobrazit hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="4427c-119">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="4427c-120">**přijatá zpráva Hello se neočekávaná nebo chybně formátovaná. (Chyba 0x80090326)**</span><span class="sxs-lookup"><span data-stu-id="4427c-120">**hello message received was unexpected or badly formatted. (Error 0x80090326)**</span></span>

### <a name="cause"></a><span data-ttu-id="4427c-121">Příčina</span><span class="sxs-lookup"><span data-stu-id="4427c-121">Cause</span></span>

<span data-ttu-id="4427c-122">K tomuto problému dochází, pokud není veřejný klíč certifikátu kořenové hello nahraje do hello Azure VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="4427c-122">This problem occurs if hello root certificate public key is not uploaded into hello Azure VPN gateway.</span></span> <span data-ttu-id="4427c-123">Může také dojít, pokud klíč hello je poškozený nebo vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="4427c-123">It can also occur if hello key is corrupted or expired.</span></span>

### <a name="solution"></a><span data-ttu-id="4427c-124">Řešení</span><span class="sxs-lookup"><span data-stu-id="4427c-124">Solution</span></span>

<span data-ttu-id="4427c-125">tooresolve tento problém, zkontrolujte stav hello hello kořenových certifikátů v hello Azure portálu toosee, zda byl odvolán.</span><span class="sxs-lookup"><span data-stu-id="4427c-125">tooresolve this problem, check hello status of hello root certificate in hello Azure portal toosee whether it was revoked.</span></span> <span data-ttu-id="4427c-126">Pokud ho nebude odvolaný, zkuste toodelete hello kořenový certifikát a reupload.</span><span class="sxs-lookup"><span data-stu-id="4427c-126">If it is not revoked, try toodelete hello root certificate and reupload.</span></span> <span data-ttu-id="4427c-127">Další informace najdete v tématu [vytvářet certifikáty](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).</span><span class="sxs-lookup"><span data-stu-id="4427c-127">For more information, see [Create certificates](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).</span></span>

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a><span data-ttu-id="4427c-128">Chyba klienta sítě VPN: řetěz certifikátů zpracuje, ale byla ukončena</span><span class="sxs-lookup"><span data-stu-id="4427c-128">VPN client error: A certificate chain processed but terminated</span></span> 

### <a name="symptom"></a><span data-ttu-id="4427c-129">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4427c-129">Symptom</span></span> 

<span data-ttu-id="4427c-130">Pokusíte-li pomocí klienta VPN hello tooconnect tooan virtuální síť Azure, můžete zobrazit hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="4427c-130">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="4427c-131">**Řetěz certifikátů zpracuje, ale ukončen v kořenovém certifikátu, který není důvěryhodný vztah důvěryhodnosti zprostředkovatele hello.**</span><span class="sxs-lookup"><span data-stu-id="4427c-131">**A certificate chain processed but terminated in a root certificate which is not trusted by hello trust provider.**</span></span>

### <a name="solution"></a><span data-ttu-id="4427c-132">Řešení</span><span class="sxs-lookup"><span data-stu-id="4427c-132">Solution</span></span>

1. <span data-ttu-id="4427c-133">Ujistěte se, že hello následující certifikáty jsou ve správném umístění hello:</span><span class="sxs-lookup"><span data-stu-id="4427c-133">Make sure that hello following certificates are in hello correct location:</span></span>

    | <span data-ttu-id="4427c-134">Certifikát</span><span class="sxs-lookup"><span data-stu-id="4427c-134">Certificate</span></span> | <span data-ttu-id="4427c-135">Umístění</span><span class="sxs-lookup"><span data-stu-id="4427c-135">Location</span></span> |
    | ------------- | ------------- |
    | <span data-ttu-id="4427c-136">AzureClient.pfx</span><span class="sxs-lookup"><span data-stu-id="4427c-136">AzureClient.pfx</span></span>  | <span data-ttu-id="4427c-137">Aktuální User\Personal\Certificates</span><span class="sxs-lookup"><span data-stu-id="4427c-137">Current User\Personal\Certificates</span></span> |
    | <span data-ttu-id="4427c-138">Azuregateway -*GUID*. cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="4427c-138">Azuregateway-*GUID*.cloudapp.net</span></span>  | <span data-ttu-id="4427c-139">Aktuální User\Trusted kořenové certifikační autority</span><span class="sxs-lookup"><span data-stu-id="4427c-139">Current User\Trusted Root Certification Authorities</span></span>|
    | <span data-ttu-id="4427c-140">AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer</span><span class="sxs-lookup"><span data-stu-id="4427c-140">AzureGateway-*GUID*.cloudapp.net, AzureRoot.cer</span></span>    | <span data-ttu-id="4427c-141">Místní počítač\Důvěryhodné kořenové certifikační autority</span><span class="sxs-lookup"><span data-stu-id="4427c-141">Local Computer\Trusted Root Certification Authorities</span></span>|

2. <span data-ttu-id="4427c-142">Pokud hello certifikáty jsou již v umístění hello, zkuste toodelete hello certifikáty a je přeinstalovat.</span><span class="sxs-lookup"><span data-stu-id="4427c-142">If hello certificates are already in hello location, try toodelete hello certificates and reinstall them.</span></span> <span data-ttu-id="4427c-143">Hello  **azuregateway -*GUID*. cloudapp.net** certifikát se nachází ve hello konfiguračního balíčku klienta VPN, který jste si stáhli z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="4427c-143">hello **azuregateway-*GUID*.cloudapp.net** certificate is in hello VPN client configuration package that you downloaded from hello Azure portal.</span></span> <span data-ttu-id="4427c-144">Můžete použít soubor archivers tooextract hello soubory z balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="4427c-144">You can use file archivers tooextract hello files from hello package.</span></span>

## <a name="file-download-error-target-uri-is-not-specified"></a><span data-ttu-id="4427c-145">Chyba při stahování souborů: není zadaný cílový identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="4427c-145">File download error: Target URI is not specified</span></span>

### <a name="symptom"></a><span data-ttu-id="4427c-146">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4427c-146">Symptom</span></span>

<span data-ttu-id="4427c-147">Zobrazí se následující chybová zpráva hello:</span><span class="sxs-lookup"><span data-stu-id="4427c-147">You receive hello following error message:</span></span>

<span data-ttu-id="4427c-148">**Stahování souboru došlo k chybě. Není zadaný cílový identifikátor URI.**</span><span class="sxs-lookup"><span data-stu-id="4427c-148">**File download error. Target URI is not specified.**</span></span>

### <a name="cause"></a><span data-ttu-id="4427c-149">Příčina</span><span class="sxs-lookup"><span data-stu-id="4427c-149">Cause</span></span> 

<span data-ttu-id="4427c-150">K tomuto problému dochází kvůli typ nesprávný brány.</span><span class="sxs-lookup"><span data-stu-id="4427c-150">This problem occurs because of an incorrect gateway type.</span></span> 

### <a name="solution"></a><span data-ttu-id="4427c-151">Řešení</span><span class="sxs-lookup"><span data-stu-id="4427c-151">Solution</span></span>

<span data-ttu-id="4427c-152">musí být Hello typ brány VPN **VPN**, a musí být hello typ sítě VPN **RouteBased**.</span><span class="sxs-lookup"><span data-stu-id="4427c-152">hello VPN gateway type must be **VPN**, and hello VPN type must be **RouteBased**.</span></span>

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a><span data-ttu-id="4427c-153">Chyba klienta sítě VPN: Azure VPN vlastního skriptu se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="4427c-153">VPN client error: Azure VPN custom script failed</span></span> 

### <a name="symptom"></a><span data-ttu-id="4427c-154">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4427c-154">Symptom</span></span>

<span data-ttu-id="4427c-155">Pokusíte-li pomocí klienta VPN hello tooconnect tooan virtuální síť Azure, můžete zobrazit hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="4427c-155">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="4427c-156">**Vlastní skript (tooupdate vaše směrování tabulky) se nezdařilo. (Chyba 8007026f)**</span><span class="sxs-lookup"><span data-stu-id="4427c-156">**Custom script (tooupdate your routing table) failed. (Error 8007026f)**</span></span>

### <a name="cause"></a><span data-ttu-id="4427c-157">Příčina</span><span class="sxs-lookup"><span data-stu-id="4427c-157">Cause</span></span>

<span data-ttu-id="4427c-158">Tento problém se může vyskytnout, pokud se pokoušíte připojení k síti VPN tooopen hello bodu lokality pomocí zástupce.</span><span class="sxs-lookup"><span data-stu-id="4427c-158">This problem might occur if you are trying tooopen hello site-to-point VPN connection by using a shortcut.</span></span>

### <a name="solution"></a><span data-ttu-id="4427c-159">Řešení</span><span class="sxs-lookup"><span data-stu-id="4427c-159">Solution</span></span> 

<span data-ttu-id="4427c-160">Otevřete balíček VPN hello přímo místo otevřením z hello zástupce.</span><span class="sxs-lookup"><span data-stu-id="4427c-160">Open hello VPN package directly instead of opening it from hello shortcut.</span></span>

## <a name="cannot-install-hello-vpn-client"></a><span data-ttu-id="4427c-161">Nelze nainstalovat klienta VPN hello</span><span class="sxs-lookup"><span data-stu-id="4427c-161">Cannot install hello VPN client</span></span>

### <a name="cause"></a><span data-ttu-id="4427c-162">Příčina</span><span class="sxs-lookup"><span data-stu-id="4427c-162">Cause</span></span> 

<span data-ttu-id="4427c-163">Je certifikát další požadované tootrust hello VPN gateway pro vaši virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="4427c-163">An additional certificate is required tootrust hello VPN gateway for your virtual network.</span></span> <span data-ttu-id="4427c-164">certifikát Hello je součástí hello konfiguračního balíčku klienta VPN generovaný z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4427c-164">hello certificate is included in hello VPN client configuration package that is generated from hello Azure portal.</span></span>

### <a name="solution"></a><span data-ttu-id="4427c-165">Řešení</span><span class="sxs-lookup"><span data-stu-id="4427c-165">Solution</span></span>

<span data-ttu-id="4427c-166">Rozbalte balíček konfigurace klienta VPN hello a najít soubor .cer hello.</span><span class="sxs-lookup"><span data-stu-id="4427c-166">Extract hello VPN client configuration package, and find hello .cer file.</span></span> <span data-ttu-id="4427c-167">tooinstall hello certifikátů, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4427c-167">tooinstall hello certificate, follow these steps:</span></span>

1. <span data-ttu-id="4427c-168">Otevřete mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="4427c-168">Open mmc.exe.</span></span>
2. <span data-ttu-id="4427c-169">Přidat hello **certifikáty** modul snap-in.</span><span class="sxs-lookup"><span data-stu-id="4427c-169">Add hello **Certificates** snap-in.</span></span>
3. <span data-ttu-id="4427c-170">Vyberte hello **počítače** účet pro místní počítač hello.</span><span class="sxs-lookup"><span data-stu-id="4427c-170">Select hello **Computer** account for hello local computer.</span></span>
4. <span data-ttu-id="4427c-171">Klikněte pravým tlačítkem na hello **důvěryhodné kořenové certifikační autority** uzlu.</span><span class="sxs-lookup"><span data-stu-id="4427c-171">Right-click hello **Trusted Root Certification Authorities** node.</span></span> <span data-ttu-id="4427c-172">Klikněte na tlačítko **všech úloh** > **Import**a soubor .cer toohello procházet jste rozbalili ze hello balíček konfigurace klienta VPN.</span><span class="sxs-lookup"><span data-stu-id="4427c-172">Click **All-Task** > **Import**, and browse toohello .cer file you extracted from hello VPN client configuration package.</span></span>
5. <span data-ttu-id="4427c-173">Restartujte počítač hello.</span><span class="sxs-lookup"><span data-stu-id="4427c-173">Restart hello computer.</span></span> 
6. <span data-ttu-id="4427c-174">Zkuste klienta VPN tooinstall hello.</span><span class="sxs-lookup"><span data-stu-id="4427c-174">Try tooinstall hello VPN client.</span></span>

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-data-is-invalid"></a><span data-ttu-id="4427c-175">Portál Azure Chyba: selhání toosave hello VPN gateway a hello dat je neplatný</span><span class="sxs-lookup"><span data-stu-id="4427c-175">Azure portal error: Failed toosave hello VPN gateway, and hello data is invalid</span></span>

### <a name="symptom"></a><span data-ttu-id="4427c-176">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4427c-176">Symptom</span></span>

<span data-ttu-id="4427c-177">Když zkusíte toosave hello změny pro bránu VPN hello v hello portálu Azure, zobrazí se hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="4427c-177">When you try toosave hello changes for hello VPN gateway in hello Azure portal, you receive hello following error message:</span></span>

<span data-ttu-id="4427c-178">**Brána virtuální sítě se nezdařilo toosave &lt;* název brány*&gt;.</span><span class="sxs-lookup"><span data-stu-id="4427c-178">**Failed toosave virtual network gateway &lt;*gateway name*&gt;.</span></span> <span data-ttu-id="4427c-179">Data pro certifikát &lt; *certifikátu ID* &gt; je invalid.* *</span><span class="sxs-lookup"><span data-stu-id="4427c-179">Data for certificate &lt;*certificate ID*&gt; is invalid.**</span></span>

### <a name="cause"></a><span data-ttu-id="4427c-180">Příčina</span><span class="sxs-lookup"><span data-stu-id="4427c-180">Cause</span></span> 

<span data-ttu-id="4427c-181">Tento problém může dojít, pokud hello kořenový certifikát veřejný klíč, který jste nahráli obsahuje neplatný znak, jako je například mezerou.</span><span class="sxs-lookup"><span data-stu-id="4427c-181">This problem might occur if hello root certificate public key that you uploaded contains an invalid character, such as a space.</span></span>

### <a name="solution"></a><span data-ttu-id="4427c-182">Řešení</span><span class="sxs-lookup"><span data-stu-id="4427c-182">Solution</span></span>

<span data-ttu-id="4427c-183">Ujistěte se, že hello data v certifikátu hello neobsahuje neplatné znaky, jako je například konce řádků (návrat na začátek).</span><span class="sxs-lookup"><span data-stu-id="4427c-183">Make sure that hello data in hello certificate does not contain invalid characters, such as line breaks (carriage returns).</span></span> <span data-ttu-id="4427c-184">Hello celý hodnota musí být dlouhý jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="4427c-184">hello entire value should be one long line.</span></span> <span data-ttu-id="4427c-185">Následující text Hello je ukázka hello certifikátu:</span><span class="sxs-lookup"><span data-stu-id="4427c-185">hello following text is a sample of hello certificate:</span></span>

    -----BEGIN CERTIFICATE-----
    MIIC5zCCAc+gAwIBAgIQFSwsLuUrCIdHwI3hzJbdBjANBgkqhkiG9w0BAQsFADAW
    MRQwEgYDVQQDDAtQMlNSb290Q2VydDAeFw0xNzA2MTUwMjU4NDZaFw0xODA2MTUw
    MzE4NDZaMBYxFDASBgNVBAMMC1AyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEF
    AAOCAQ8AMIIBCgKCAQEAz8QUCWCxxxTrxF5yc5uUpL/bzwC5zZ804ltB1NpPa/PI
    sa5uwLw/YFb8XG/JCWxUJpUzS/kHUKFluqkY80U+fAmRmTEMq5wcaMhp3wRfeq+1
    G9OPBNTyqpnHe+i54QAnj1DjsHXXNL4AL1N8/TSzYTm7dkiq+EAIyRRMrZlYwije
    407ChxIp0stB84MtMShhyoSm2hgl+3zfwuaGXoJQwWiXh715kMHVTSj9zFechYd7
    5OLltoRRDyyxsf0qweTFKIgFj13Hn/bq/UJG3AcyQNvlCv1HwQnXO+hckVBB29wE
    sF8QSYk2MMGimPDYYt4ZM5tmYLxxxvGmrGhc+HWXzMeQIDAQABozEwLzAOBgNVHQ8B
    Af8EBAMCAgQwHQYDVR0OBBYEFBE9zZWhQftVLBQNATC/LHLvMb0OMA0GCSqGSIb3
    DQEBCwUAA4IBAQB7k0ySFUQu72sfj3BdNxrXSyOT4L2rADLhxxxiK0U6gHUF6eWz
    /0h6y4mNkg3NgLT3j/WclqzHXZruhWAXSF+VbAGkwcKA99xGWOcUJ+vKVYL/kDja
    gaZrxHlhTYVVmwn4F7DWhteFqhzZ89/W9Mv6p180AimF96qDU8Ez8t860HQaFkU6
    2Nw9ZMsGkvLePZZi78yVBDCWMogBMhrRVXG/xQkBajgvL5syLwFBo2kWGdC+wyWY
    U/Z+EK9UuHnn3Hkq/vXEzRVsYuaxchta0X2UNRzRq+o706l+iyLTpe6fnvW6ilOi
    e8Jcej7mzunzyjz4chN0/WVF94MtxbUkLkqP
    -----END CERTIFICATE-----

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-resource-name-is-invalid"></a><span data-ttu-id="4427c-186">Portál Azure Chyba: selhání toosave hello VPN gateway a hello název prostředku je neplatný</span><span class="sxs-lookup"><span data-stu-id="4427c-186">Azure portal error: Failed toosave hello VPN gateway, and hello resource name is invalid</span></span>

### <a name="symptom"></a><span data-ttu-id="4427c-187">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4427c-187">Symptom</span></span>

<span data-ttu-id="4427c-188">Když zkusíte toosave hello změny pro bránu VPN hello v hello portálu Azure, zobrazí se hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="4427c-188">When you try toosave hello changes for hello VPN gateway in hello Azure portal, you receive hello following error message:</span></span> 

<span data-ttu-id="4427c-189">**Brána virtuální sítě se nezdařilo toosave &lt;* název brány*&gt;.</span><span class="sxs-lookup"><span data-stu-id="4427c-189">**Failed toosave virtual network gateway &lt;*gateway name*&gt;.</span></span> <span data-ttu-id="4427c-190">Název prostředku &lt; *název certifikátu zkuste tooupload* &gt; je neplatný **.</span><span class="sxs-lookup"><span data-stu-id="4427c-190">Resource name &lt;*certificate name you try tooupload*&gt; is invalid**.</span></span>

### <a name="cause"></a><span data-ttu-id="4427c-191">Příčina</span><span class="sxs-lookup"><span data-stu-id="4427c-191">Cause</span></span>

<span data-ttu-id="4427c-192">K tomuto problému dochází, protože název hello hello certifikátu obsahuje neplatný znak, jako je například mezerou.</span><span class="sxs-lookup"><span data-stu-id="4427c-192">This problem occurs because hello name of hello certificate contains an invalid character, such as a space.</span></span> 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a><span data-ttu-id="4427c-193">Portál Azure Chyba: Chyba stažení souboru balíčku VPN 503</span><span class="sxs-lookup"><span data-stu-id="4427c-193">Azure portal error: VPN package file download error 503</span></span>

### <a name="symptom"></a><span data-ttu-id="4427c-194">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4427c-194">Symptom</span></span>

<span data-ttu-id="4427c-195">Když zkusíte konfiguračního balíčku klienta VPN toodownload hello, zobrazí se hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="4427c-195">When you try toodownload hello VPN client configuration package, you receive hello following error message:</span></span>

<span data-ttu-id="4427c-196">**Nepodařilo toodownload hello souboru. Podrobnosti o chybě: chyba 503. Hello server je zaneprázdněn.**</span><span class="sxs-lookup"><span data-stu-id="4427c-196">**Failed toodownload hello file. Error details: error 503. hello server is busy.**</span></span>
 
### <a name="solution"></a><span data-ttu-id="4427c-197">Řešení</span><span class="sxs-lookup"><span data-stu-id="4427c-197">Solution</span></span>

<span data-ttu-id="4427c-198">Tato chyba může být způsobeno k dočasným potížím sítě.</span><span class="sxs-lookup"><span data-stu-id="4427c-198">This error can be caused by a temporary network problem.</span></span> <span data-ttu-id="4427c-199">Zkuste balíček VPN hello toodownload znovu za několik minut.</span><span class="sxs-lookup"><span data-stu-id="4427c-199">Try toodownload hello VPN package again after a few minutes.</span></span>

## <a name="azure-vpn-gateway-upgrade-all-p2s-clients-are-unable-tooconnect"></a><span data-ttu-id="4427c-200">Upgrade služby Azure VPN Gateway: nelze tooconnect jsou klienti všechny P2S</span><span class="sxs-lookup"><span data-stu-id="4427c-200">Azure VPN Gateway upgrade: All P2S clients are unable tooconnect</span></span>

### <a name="cause"></a><span data-ttu-id="4427c-201">Příčina</span><span class="sxs-lookup"><span data-stu-id="4427c-201">Cause</span></span>

<span data-ttu-id="4427c-202">Pokud je certifikát hello více než 50 procent prostřednictvím celé jeho životnosti certifikátu hello převracet.</span><span class="sxs-lookup"><span data-stu-id="4427c-202">If hello certificate is more than 50 percent through its lifetime, hello certificate is rolled over.</span></span>

### <a name="solution"></a><span data-ttu-id="4427c-203">Řešení</span><span class="sxs-lookup"><span data-stu-id="4427c-203">Solution</span></span>

<span data-ttu-id="4427c-204">tooresolve tento problém, vytvořte a znovu distribuovat noví klienti VPN toohello certifikáty.</span><span class="sxs-lookup"><span data-stu-id="4427c-204">tooresolve this problem, create and redistribute new certificates toohello VPN clients.</span></span> 

## <a name="too-many-vpn-clients-connected-at-once"></a><span data-ttu-id="4427c-205">Příliš mnoho klientů VPN připojení najednou</span><span class="sxs-lookup"><span data-stu-id="4427c-205">Too many VPN clients connected at once</span></span>

<span data-ttu-id="4427c-206">Pro každou bránu VPN hello maximální povolený počet připojení je 128.</span><span class="sxs-lookup"><span data-stu-id="4427c-206">For each VPN gateway, hello maximum number of allowable connections is 128.</span></span> <span data-ttu-id="4427c-207">Uvidíte hello celkový počet připojených klientů v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4427c-207">You can see hello total number of connected clients in hello Azure portal.</span></span>

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-toohello-route-table"></a><span data-ttu-id="4427c-208">Point-to-site VPN nesprávně přidá trasu pro 10.0.0.0/8 toohello směrovací tabulku</span><span class="sxs-lookup"><span data-stu-id="4427c-208">Point-to-site VPN incorrectly adds a route for 10.0.0.0/8 toohello route table</span></span>

### <a name="symptom"></a><span data-ttu-id="4427c-209">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4427c-209">Symptom</span></span>

<span data-ttu-id="4427c-210">Při přidávání hello připojení VPN na klienta point-to-site hello klienta VPN hello nutné přidat směrování směrem k hello virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="4427c-210">When you dial hello VPN connection on hello point-to-site client, hello VPN client should add a route toward hello Azure virtual network.</span></span> <span data-ttu-id="4427c-211">Pomocná služba Hello IP nutné přidat směrování pro podsíť hello klientů VPN hello.</span><span class="sxs-lookup"><span data-stu-id="4427c-211">hello IP helper service should add a route for hello subnet of hello VPN clients.</span></span> 

<span data-ttu-id="4427c-212">Hello rozsah klienta VPN patří tooa menší podsíti 10.0.0.0/8, jako je například 10.0.12.0/24.</span><span class="sxs-lookup"><span data-stu-id="4427c-212">hello VPN client range belongs tooa smaller subnet of 10.0.0.0/8, such as 10.0.12.0/24.</span></span> <span data-ttu-id="4427c-213">Místo trasu pro 10.0.12.0/24 se přidá trasu pro 10.0.0.0/8, který má vyšší prioritu.</span><span class="sxs-lookup"><span data-stu-id="4427c-213">Instead of a route for 10.0.12.0/24, a route for 10.0.0.0/8 is added that has higher priority.</span></span> 

<span data-ttu-id="4427c-214">Tato nesprávná trasa dělí připojení s další místní sítě, které můžou patřit tooanother podsítí v rámci rozsahu hello 10.0.0.0/8, například 10.50.0.0/24, které nemají konkrétní trasy definované.</span><span class="sxs-lookup"><span data-stu-id="4427c-214">This incorrect route breaks connectivity with other on-premises networks that might belong tooanother subnet within hello 10.0.0.0/8 range, such as 10.50.0.0/24, that don't have a specific route defined.</span></span> 

### <a name="cause"></a><span data-ttu-id="4427c-215">Příčina</span><span class="sxs-lookup"><span data-stu-id="4427c-215">Cause</span></span>

<span data-ttu-id="4427c-216">Toto chování je záměrné pro klienty se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="4427c-216">This behavior is by design for Windows clients.</span></span> <span data-ttu-id="4427c-217">Když klient hello používá protokol PPP IPCP hello, získá hello IP adresu pro rozhraní tunelu hello ze serveru hello (hello brána sítě VPN v tomto případě).</span><span class="sxs-lookup"><span data-stu-id="4427c-217">When hello client uses hello PPP IPCP protocol, it obtains hello IP address for hello tunnel interface from hello server (hello VPN gateway in this case).</span></span> <span data-ttu-id="4427c-218">Ale kvůli omezení v protokolu hello hello klient nemá hello masku podsítě.</span><span class="sxs-lookup"><span data-stu-id="4427c-218">However, because of a limitation in hello protocol, hello client does not have hello subnet mask.</span></span> <span data-ttu-id="4427c-219">Protože není k dispozici žádné další způsob tooget, hello klienta pokusí maska podsítě hello tooguess je založeno na třídě hello hello tunelového propojení rozhraní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4427c-219">Because there is no other way tooget it, hello client tries tooguess hello subnet mask based on hello class of hello tunnel interface IP address.</span></span> 

<span data-ttu-id="4427c-220">Proto je přidána trasu podle následující statických mapování hello:</span><span class="sxs-lookup"><span data-stu-id="4427c-220">Therefore, a route is added based on hello following static mapping:</span></span> 

<span data-ttu-id="4427c-221">Pokud adresa patří tooclass A použít aplikaci--> podsíť/8</span><span class="sxs-lookup"><span data-stu-id="4427c-221">If address belongs tooclass A --> apply /8</span></span>

<span data-ttu-id="4427c-222">Pokud adresa patří tooclass--> B použít /16</span><span class="sxs-lookup"><span data-stu-id="4427c-222">If address belongs tooclass B --> apply /16</span></span>

<span data-ttu-id="4427c-223">Pokud adresa patří tooclass--> C použít /24</span><span class="sxs-lookup"><span data-stu-id="4427c-223">If address belongs tooclass C --> apply /24</span></span>

## <a name="vpn-client-cannot-access-network-file-shares"></a><span data-ttu-id="4427c-224">Klient VPN nelze získat přístup k síťové sdílené složky</span><span class="sxs-lookup"><span data-stu-id="4427c-224">VPN client cannot access network file shares</span></span>

### <a name="symptom"></a><span data-ttu-id="4427c-225">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4427c-225">Symptom</span></span>

<span data-ttu-id="4427c-226">Klient VPN Hello připojil toohello virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="4427c-226">hello VPN client has connected toohello Azure virtual network.</span></span> <span data-ttu-id="4427c-227">Hello klienta nemá přístup k síťové sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="4427c-227">However, hello client cannot access network shares.</span></span>

### <a name="cause"></a><span data-ttu-id="4427c-228">Příčina</span><span class="sxs-lookup"><span data-stu-id="4427c-228">Cause</span></span>

<span data-ttu-id="4427c-229">Hello protokolu SMB se používá pro přístup ke sdílené složce souborů.</span><span class="sxs-lookup"><span data-stu-id="4427c-229">hello SMB protocol is used for file share access.</span></span> <span data-ttu-id="4427c-230">Když je zahájeno připojení hello, klient VPN hello přidá hello relace pověření a dojde k selhání hello.</span><span class="sxs-lookup"><span data-stu-id="4427c-230">When hello connection is initiated, hello VPN client adds hello session credentials and hello failure occurs.</span></span> <span data-ttu-id="4427c-231">Po navázání připojení hello hello klient je donucen toouse hello mezipaměti pověření pro ověřování pomocí protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="4427c-231">After hello connection is established, hello client is forced toouse hello cache credentials for Kerberos authentication.</span></span> <span data-ttu-id="4427c-232">Tento proces se spustí dotazy toohello Key Distribution Center (řadič domény) tooget token.</span><span class="sxs-lookup"><span data-stu-id="4427c-232">This process initiates queries toohello Key Distribution Center (a domain controller) tooget a token.</span></span> <span data-ttu-id="4427c-233">Protože hello klient připojí z hello Internetu, nemusí být řadič domény nemůže tooreach hello.</span><span class="sxs-lookup"><span data-stu-id="4427c-233">Because hello client connects from hello Internet, it might not be able tooreach hello domain controller.</span></span> <span data-ttu-id="4427c-234">Proto hello klienta nelze převzetí služeb při selhání z tooNTLM protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="4427c-234">Therefore, hello client cannot fail over from Kerberos tooNTLM.</span></span> 

<span data-ttu-id="4427c-235">Hello jenom čas, že klient hello se zobrazí výzva pro pověření je, když má platný certifikát (sítě SAN = UPN) vydaný toowhich domény hello je připojen.</span><span class="sxs-lookup"><span data-stu-id="4427c-235">hello only time that hello client is prompted for a credential is when it has a valid certificate (with SAN=UPN) issued by hello domain toowhich it is joined.</span></span> <span data-ttu-id="4427c-236">Hello klienta musí být také fyzicky připojené toohello doménové síti.</span><span class="sxs-lookup"><span data-stu-id="4427c-236">hello client also must be physically connected toohello domain network.</span></span> <span data-ttu-id="4427c-237">V tomto případě klient hello pokusí toouse hello certifikátu a spojí toohello řadič domény.</span><span class="sxs-lookup"><span data-stu-id="4427c-237">In this case, hello client tries toouse hello certificate and reaches out toohello domain controller.</span></span> <span data-ttu-id="4427c-238">Potom hello Key Distribution Center vrátí chybu "KDC_ERR_C_PRINCIPAL_UNKNOWN".</span><span class="sxs-lookup"><span data-stu-id="4427c-238">Then hello Key Distribution Center returns a "KDC_ERR_C_PRINCIPAL_UNKNOWN" error.</span></span> <span data-ttu-id="4427c-239">Klient Hello je vynucené toofail přes tooNTLM.</span><span class="sxs-lookup"><span data-stu-id="4427c-239">hello client is forced toofail over tooNTLM.</span></span> 

### <a name="solution"></a><span data-ttu-id="4427c-240">Řešení</span><span class="sxs-lookup"><span data-stu-id="4427c-240">Solution</span></span>

<span data-ttu-id="4427c-241">toowork vyřešit hello problém, zakažte hello ukládání přihlašovacích údajů do domény z hello následující podklíč registru:</span><span class="sxs-lookup"><span data-stu-id="4427c-241">toowork around hello problem, disable hello caching of domain credentials from hello following registry subkey:</span></span> 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set hello value too1 


## <a name="cannot-find-hello-point-to-site-vpn-connection-in-windows-after-reinstalling-hello-vpn-client"></a><span data-ttu-id="4427c-242">Nelze najít připojení point-to-site VPN hello v systému Windows po opětovné instalaci klienta VPN hello</span><span class="sxs-lookup"><span data-stu-id="4427c-242">Cannot find hello point-to-site VPN connection in Windows after reinstalling hello VPN client</span></span>

### <a name="symptom"></a><span data-ttu-id="4427c-243">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4427c-243">Symptom</span></span>

<span data-ttu-id="4427c-244">Odebrat připojení VPN point-to-site hello a znovu nainstalujete klienta VPN hello.</span><span class="sxs-lookup"><span data-stu-id="4427c-244">You remove hello point-to-site VPN connection and then reinstall hello VPN client.</span></span> <span data-ttu-id="4427c-245">V takovém případě není hello připojení k síti VPN byl úspěšně nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="4427c-245">In this situation, hello VPN connection is not configured successfully.</span></span> <span data-ttu-id="4427c-246">Nevidíte hello připojení VPN v hello **síťová připojení** nastavení v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="4427c-246">You do not see hello VPN connection in hello **Network connections** settings in Windows.</span></span>

### <a name="solution"></a><span data-ttu-id="4427c-247">Řešení</span><span class="sxs-lookup"><span data-stu-id="4427c-247">Solution</span></span>

<span data-ttu-id="4427c-248">tooresolve hello problém odstranit hello staré VPN konfigurační soubory klienta z **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, a poté znovu spusťte instalační program klienta VPN hello.</span><span class="sxs-lookup"><span data-stu-id="4427c-248">tooresolve hello problem, delete hello old VPN client configuration files from **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, and then run hello VPN client installer again.</span></span>
