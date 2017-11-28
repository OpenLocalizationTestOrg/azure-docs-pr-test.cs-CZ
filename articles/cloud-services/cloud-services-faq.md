---
title: "cloudové služby role aaaAzure – nejčastější dotazy | Microsoft Docs"
description: "Časté otázky k Azure Cloud Services. Odpovídá na některé běžné dotazy týkající se certifikátů, webových rolí a rolí pracovního procesu."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: b07a1990e031e60ae919a5f7c636945b89c7d3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-services-faq"></a><span data-ttu-id="c7c0a-104">Cloud Services – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="c7c0a-104">Cloud Services FAQ</span></span>
<span data-ttu-id="c7c0a-105">Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se služby Microsoft Azure Cloud.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-105">This article answers some frequently asked questions about Microsoft Azure Cloud Services.</span></span> <span data-ttu-id="c7c0a-106">Můžete také navštívit hello [Azure podporují – nejčastější dotazy](http://go.microsoft.com/fwlink/?LinkID=185083) obecné Azure – ceny a podporu informace.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-106">You can also visit hello [Azure Support FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) for general Azure pricing and support information.</span></span> <span data-ttu-id="c7c0a-107">Můžete také obrátit hello [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-107">You can also consult hello [Cloud Services VM Size page](cloud-services-sizes-specs.md) for size information.</span></span>

## <a name="certificates"></a><span data-ttu-id="c7c0a-108">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="c7c0a-108">Certificates</span></span>
### <a name="where-should-i-install-my-certificate"></a><span data-ttu-id="c7c0a-109">Kde nainstalujte tento certifikát?</span><span class="sxs-lookup"><span data-stu-id="c7c0a-109">Where should I install my certificate?</span></span>
* <span data-ttu-id="c7c0a-110">**Moje**</span><span class="sxs-lookup"><span data-stu-id="c7c0a-110">**My**</span></span>  
  <span data-ttu-id="c7c0a-111">Aplikace certifikát s privátním klíčem (\*.pfx, \*.p12).</span><span class="sxs-lookup"><span data-stu-id="c7c0a-111">Application Certificate with private key (\*.pfx, \*.p12).</span></span>
* <span data-ttu-id="c7c0a-112">**CERTIFIKAČNÍ AUTORITY**</span><span class="sxs-lookup"><span data-stu-id="c7c0a-112">**CA**</span></span>  
  <span data-ttu-id="c7c0a-113">Všechny zprostředkující certifikáty přejděte v tomto úložišti (zásady a Sub CAs).</span><span class="sxs-lookup"><span data-stu-id="c7c0a-113">All your intermediate certificates go in this store (Policy and Sub CAs).</span></span>
* <span data-ttu-id="c7c0a-114">**KOŘENOVÉ**</span><span class="sxs-lookup"><span data-stu-id="c7c0a-114">**ROOT**</span></span>  
  <span data-ttu-id="c7c0a-115">Hello kořenové certifikační Autority uložte, aby vaše hlavní kořenový certifikát certifikační Autority by měl přejděte sem.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-115">hello root CA store, so your main root CA cert should go here.</span></span>

### <a name="i-cant-remove-expired-certificate"></a><span data-ttu-id="c7c0a-116">Vypršela platnost certifikát nelze odebrat</span><span class="sxs-lookup"><span data-stu-id="c7c0a-116">I can't remove expired certificate</span></span>
<span data-ttu-id="c7c0a-117">Azure zabrání odebírání certifikátu, pokud se používá.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-117">Azure prevents you from removing a certificate while it is in use.</span></span> <span data-ttu-id="c7c0a-118">Potřebujete tooeither odstranění hello nasazení, které používá certifikát hello nebo nasazení hello aktualizace s jinou nebo obnovený certifikát.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-118">You need tooeither delete hello deployment that uses hello certificate, or update hello deployment with a different or renewed certificate.</span></span>

### <a name="delete-an-expired-certificate"></a><span data-ttu-id="c7c0a-119">Odstranit certifikát s vypršenou platností</span><span class="sxs-lookup"><span data-stu-id="c7c0a-119">Delete an expired certificate</span></span>
<span data-ttu-id="c7c0a-120">Dokud není certifikát hello používá, můžete použít hello [odebrat AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) tooremove rutiny prostředí PowerShell certifikát.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-120">As long as hello certificate is not in use, you can use hello [Remove-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell cmdlet tooremove a certificate.</span></span>

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a><span data-ttu-id="c7c0a-121">I vypršela platnost certifikátů s názvem Windows Azure Service Management pro rozšíření</span><span class="sxs-lookup"><span data-stu-id="c7c0a-121">I have expired certificates named Windows Azure Service Management for Extensions</span></span>
<span data-ttu-id="c7c0a-122">Tyto certifikáty se vytvoří vždy, když rozšíření je přidány toohello cloudové službě, například hello rozšíření vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-122">These certificates are created whenever an extension is added toohello cloud service such as hello Remote Desktop extension.</span></span> <span data-ttu-id="c7c0a-123">Tyto certifikáty se používají jenom pro šifrování a dešifrování hello privátní konfigurace rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-123">These certificates are only used for encrypting and decrypting hello private configuration of hello extension.</span></span> <span data-ttu-id="c7c0a-124">Pokud tyto certifikáty vyprší nezáleží.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-124">It does not matter if these certificates expire.</span></span> <span data-ttu-id="c7c0a-125">Datum vypršení platnosti Hello není zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-125">hello expiration date is not checked.</span></span>

### <a name="certificates-i-have-deleted-keep-reappearing"></a><span data-ttu-id="c7c0a-126">Zobrazují se certifikátů, které mají vymazání</span><span class="sxs-lookup"><span data-stu-id="c7c0a-126">Certificates I have deleted keep reappearing</span></span>
<span data-ttu-id="c7c0a-127">Tyto zobrazují se pravděpodobně z důvodu nástroj, který používáte, jako je například Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-127">These keep reappearing most likely because of a tool you're using, such as Visual Studio.</span></span> <span data-ttu-id="c7c0a-128">Vždy, když se znovu připojíte s nástroj, který používá certifikát, bude znovu nahrané tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-128">Whenever you reconnect with a tool that is using a certificate, it will again be uploaded tooAzure.</span></span>

### <a name="my-certificates-keep-disappearing"></a><span data-ttu-id="c7c0a-129">Zachovat zmizení certifikáty</span><span class="sxs-lookup"><span data-stu-id="c7c0a-129">My certificates keep disappearing</span></span>
<span data-ttu-id="c7c0a-130">Dojde k recyklování hello instanci virtuálního počítače, všechny místní změny budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-130">When hello virtual machine instance recycles, all local changes are lost.</span></span> <span data-ttu-id="c7c0a-131">Použití [úloha spuštění](cloud-services-startup-tasks.md) tooinstall certifikáty toohello virtuální počítač spustí každou roli hello čas.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-131">Use a [startup task](cloud-services-startup-tasks.md) tooinstall certificates toohello virtual machine each time hello role starts.</span></span>

### <a name="i-cannot-find-my-management-certificates-in-hello-portal"></a><span data-ttu-id="c7c0a-132">Nelze najít certifikáty pro správu portálu hello</span><span class="sxs-lookup"><span data-stu-id="c7c0a-132">I cannot find my management certificates in hello portal</span></span>
<span data-ttu-id="c7c0a-133">[Certifikáty pro správu](../azure-api-management-certs.md) jsou dostupné jenom v hello portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-133">[Management certificates](../azure-api-management-certs.md) are only available in hello Azure Classic Portal.</span></span> <span data-ttu-id="c7c0a-134">aktuální portál Azure Hello nepoužívá certifikáty pro správu.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-134">hello current Azure portal does not use management certificates.</span></span> 

### <a name="how-can-i-disable-a-management-certificate"></a><span data-ttu-id="c7c0a-135">Jak můžete zakázat certifikát pro správu?</span><span class="sxs-lookup"><span data-stu-id="c7c0a-135">How can I disable a management certificate?</span></span>
<span data-ttu-id="c7c0a-136">[Certifikáty pro správu](../azure-api-management-certs.md) nelze zakázat.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-136">[Management certificates](../azure-api-management-certs.md) cannot be disabled.</span></span> <span data-ttu-id="c7c0a-137">Můžete je odstranit prostřednictvím portálu Azure Classic hello když nechcete, aby je toobe už používá.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-137">You delete them through hello Azure Classic Portal when you do not want them toobe used anymore.</span></span>

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a><span data-ttu-id="c7c0a-138">Jak vytvořit certifikát protokolu SSL pro konkrétní IP adresu?</span><span class="sxs-lookup"><span data-stu-id="c7c0a-138">How do I create an SSL certificate for a specific IP address?</span></span>
<span data-ttu-id="c7c0a-139">Postupujte podle pokynů hello v hello [vytvořit certifikát kurzu](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="c7c0a-139">Follow hello directions in hello [create a certificate tutorial](cloud-services-certs-create.md).</span></span> <span data-ttu-id="c7c0a-140">Použijte hello IP adresu jako hello název DNS.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-140">Use hello IP address as hello DNS Name.</span></span>

## <a name="security"></a><span data-ttu-id="c7c0a-141">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="c7c0a-141">Security</span></span>
### <a name="disable-ssl-30"></a><span data-ttu-id="c7c0a-142">Protokol SSL 3.0 zakázat</span><span class="sxs-lookup"><span data-stu-id="c7c0a-142">Disable SSL 3.0</span></span>
<span data-ttu-id="c7c0a-143">toodisable SSL 3.0 a TLS zabezpečení pomocí vytvoření spuštění úloh, která je popsána v tomto příspěvku na blogu: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/</span><span class="sxs-lookup"><span data-stu-id="c7c0a-143">toodisable SSL 3.0 and use TLS security, create a startup task which is documented on this blog post: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/</span></span>

### <a name="add-nosniff-tooyour-website"></a><span data-ttu-id="c7c0a-144">Přidat **nosniff** tooyour webu</span><span class="sxs-lookup"><span data-stu-id="c7c0a-144">Add **nosniff** tooyour website</span></span>
<span data-ttu-id="c7c0a-145">Klienti tooprevent z sledování toku dat hello typy MIME, přidejte nastavení vaší *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-145">tooprevent clients from sniffing hello MIME types, add a setting in your *web.config* file.</span></span>

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

<span data-ttu-id="c7c0a-146">To můžete také přidat jako nastavení ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-146">You can also add this as a setting in IIS.</span></span> <span data-ttu-id="c7c0a-147">Použití hello následující příkaz s hello [běžné úlohy spuštění](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) článku.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-147">Use hello following command with hello [common startup tasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) article.</span></span>

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="customize-iis-for-a-web-role"></a><span data-ttu-id="c7c0a-148">Přizpůsobení služby IIS pro webovou roli</span><span class="sxs-lookup"><span data-stu-id="c7c0a-148">Customize IIS for a web role</span></span>
<span data-ttu-id="c7c0a-149">Použít skript spuštění služby IIS hello z hello [běžné úlohy spuštění](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) článku.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-149">Use hello IIS startup script from hello [common startup tasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) article.</span></span>

## <a name="scaling"></a><span data-ttu-id="c7c0a-150">Škálování</span><span class="sxs-lookup"><span data-stu-id="c7c0a-150">Scaling</span></span>
### <a name="i-cannot-scale-beyond-x-instances"></a><span data-ttu-id="c7c0a-151">Nelze nastavit mimo X instancí</span><span class="sxs-lookup"><span data-stu-id="c7c0a-151">I cannot scale beyond X instances</span></span>
<span data-ttu-id="c7c0a-152">Vaše předplatné Azure může mít na hello počet jader, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-152">Your Azure Subscription has a limit on hello number of cores you can use.</span></span> <span data-ttu-id="c7c0a-153">Škálování nebude fungovat, pokud jste použili všechny jader hello k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-153">Scaling will not work if you have used all hello cores available.</span></span> <span data-ttu-id="c7c0a-154">Například pokud máte limit 100 jader, znamená to může mít 100 instancí virtuální počítač A1 s nastavenou velikostí pro cloudové služby, nebo velikosti 50 A2 instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-154">For example, if you have a limit of 100 cores, this means you could have 100 A1 sized virtual machine instances for your cloud service, or 50 A2 sized virtual machine instances.</span></span>

## <a name="networking"></a><span data-ttu-id="c7c0a-155">Sítě</span><span class="sxs-lookup"><span data-stu-id="c7c0a-155">Networking</span></span>
### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a><span data-ttu-id="c7c0a-156">I nelze rezervovat IP adresy v cloudové službě více virtuálních IP adres</span><span class="sxs-lookup"><span data-stu-id="c7c0a-156">I can't reserve an IP in a multi-VIP cloud service</span></span>
<span data-ttu-id="c7c0a-157">Nejdřív zkontrolujte, zda je zapnutý tuto instanci hello virtuální počítač, který se pokoušíte tooreserve hello IP pro.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-157">First, make sure that hello virtual machine instance that you're trying tooreserve hello IP for is turned on.</span></span> <span data-ttu-id="c7c0a-158">Poté se ujistěte, že používáte vyhrazené IP adresy pro nepokoušejte hello pracovní a provozní nasazení.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-158">Second, make sure that you're using Reserved IPs for bother hello staging and production deployments.</span></span> <span data-ttu-id="c7c0a-159">**Nechcete** změnit nastavení hello při nasazení hello je upgradu.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-159">**Do not** change hello settings while hello deployment is upgrading.</span></span>

## <a name="remote-desktop"></a><span data-ttu-id="c7c0a-160">Vzdálená plocha</span><span class="sxs-lookup"><span data-stu-id="c7c0a-160">Remote desktop</span></span>
### <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a><span data-ttu-id="c7c0a-161">Jak mohu vzdálené plochy Pokud skupinu NSG?</span><span class="sxs-lookup"><span data-stu-id="c7c0a-161">How do I remote desktop when I have an NSG?</span></span>
<span data-ttu-id="c7c0a-162">Přidat toohello pravidla NSG, který povolí komunikaci na portech **3389** a **20000**.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-162">Add rules toohello NSG that allow traffic on ports **3389** and **20000**.</span></span>  <span data-ttu-id="c7c0a-163">Vzdálená plocha používá port **3389**.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-163">Remote Desktop uses port **3389**.</span></span>  <span data-ttu-id="c7c0a-164">Cloudové služby instance jsou vyrovnáváním, zatížení, takže nemůže přímo řídit které tooconnect instance k.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-164">Cloud Service instances are load balanced, so you can't directly control which instance tooconnect to.</span></span>  <span data-ttu-id="c7c0a-165">Hello *RemoteForwarder* a *RemoteAccess* agenty spravovat provoz protokolu RDP a povolit hello klienta toosend soubor cookie s RDP a zadejte jednotlivé instance tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-165">hello *RemoteForwarder* and *RemoteAccess* agents manage RDP traffic and allow hello client toosend an RDP cookie and specify an individual instance tooconnect to.</span></span>  <span data-ttu-id="c7c0a-166">Hello *RemoteForwarder* a *RemoteAccess* agentů vyžadují tento port **20000*** otevřít, což může být zablokován, pokud máte skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="c7c0a-166">hello *RemoteForwarder* and *RemoteAccess* agents require that port **20000*** be opened, which may be blocked if you have an NSG.</span></span>
