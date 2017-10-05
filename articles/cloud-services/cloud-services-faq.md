---
title: "Cloudové služby role Azure – nejčastější dotazy | Microsoft Docs"
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
ms.openlocfilehash: d887f3b31693c414254dc01dac4dbdd6d9224b6d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="cloud-services-faq"></a><span data-ttu-id="d033b-104">Cloud Services – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="d033b-104">Cloud Services FAQ</span></span>
<span data-ttu-id="d033b-105">Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se služby Microsoft Azure Cloud.</span><span class="sxs-lookup"><span data-stu-id="d033b-105">This article answers some frequently asked questions about Microsoft Azure Cloud Services.</span></span> <span data-ttu-id="d033b-106">Další informace získáte [Azure podporují – nejčastější dotazy](http://go.microsoft.com/fwlink/?LinkID=185083) obecné Azure – ceny a podporu informace.</span><span class="sxs-lookup"><span data-stu-id="d033b-106">You can also visit the [Azure Support FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) for general Azure pricing and support information.</span></span> <span data-ttu-id="d033b-107">Můžete také obrátit [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.</span><span class="sxs-lookup"><span data-stu-id="d033b-107">You can also consult the [Cloud Services VM Size page](cloud-services-sizes-specs.md) for size information.</span></span>

## <a name="certificates"></a><span data-ttu-id="d033b-108">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="d033b-108">Certificates</span></span>
### <a name="where-should-i-install-my-certificate"></a><span data-ttu-id="d033b-109">Kde nainstalujte tento certifikát?</span><span class="sxs-lookup"><span data-stu-id="d033b-109">Where should I install my certificate?</span></span>
* <span data-ttu-id="d033b-110">**Moje**</span><span class="sxs-lookup"><span data-stu-id="d033b-110">**My**</span></span>  
  <span data-ttu-id="d033b-111">Aplikace certifikát s privátním klíčem (\*.pfx, \*.p12).</span><span class="sxs-lookup"><span data-stu-id="d033b-111">Application Certificate with private key (\*.pfx, \*.p12).</span></span>
* <span data-ttu-id="d033b-112">**CERTIFIKAČNÍ AUTORITY**</span><span class="sxs-lookup"><span data-stu-id="d033b-112">**CA**</span></span>  
  <span data-ttu-id="d033b-113">Všechny zprostředkující certifikáty přejděte v tomto úložišti (zásady a Sub CAs).</span><span class="sxs-lookup"><span data-stu-id="d033b-113">All your intermediate certificates go in this store (Policy and Sub CAs).</span></span>
* <span data-ttu-id="d033b-114">**KOŘENOVÉ**</span><span class="sxs-lookup"><span data-stu-id="d033b-114">**ROOT**</span></span>  
  <span data-ttu-id="d033b-115">Kořenové certifikační Autority uložte, aby vaše hlavní kořenový certifikát certifikační Autority by měl přejděte sem.</span><span class="sxs-lookup"><span data-stu-id="d033b-115">The root CA store, so your main root CA cert should go here.</span></span>

### <a name="i-cant-remove-expired-certificate"></a><span data-ttu-id="d033b-116">Vypršela platnost certifikát nelze odebrat</span><span class="sxs-lookup"><span data-stu-id="d033b-116">I can't remove expired certificate</span></span>
<span data-ttu-id="d033b-117">Azure zabrání odebírání certifikátu, pokud se používá.</span><span class="sxs-lookup"><span data-stu-id="d033b-117">Azure prevents you from removing a certificate while it is in use.</span></span> <span data-ttu-id="d033b-118">Musíte buď odstraňte nasazení, které používá certifikát, nebo aktualizaci nasazení s jinou nebo obnovený certifikát.</span><span class="sxs-lookup"><span data-stu-id="d033b-118">You need to either delete the deployment that uses the certificate, or update the deployment with a different or renewed certificate.</span></span>

### <a name="delete-an-expired-certificate"></a><span data-ttu-id="d033b-119">Odstranit certifikát s vypršenou platností</span><span class="sxs-lookup"><span data-stu-id="d033b-119">Delete an expired certificate</span></span>
<span data-ttu-id="d033b-120">Také certifikát není používán, můžete použít [odebrat AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) rutiny prostředí PowerShell odebrat certifikát.</span><span class="sxs-lookup"><span data-stu-id="d033b-120">As long as the certificate is not in use, you can use the [Remove-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell cmdlet to remove a certificate.</span></span>

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a><span data-ttu-id="d033b-121">I vypršela platnost certifikátů s názvem Windows Azure Service Management pro rozšíření</span><span class="sxs-lookup"><span data-stu-id="d033b-121">I have expired certificates named Windows Azure Service Management for Extensions</span></span>
<span data-ttu-id="d033b-122">Tyto certifikáty se vytvoří při každém přidání rozšíření do cloudové služby, jako je například rozšíření vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="d033b-122">These certificates are created whenever an extension is added to the cloud service such as the Remote Desktop extension.</span></span> <span data-ttu-id="d033b-123">Tyto certifikáty se používají jenom pro šifrování a dešifrování privátní konfigurace rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d033b-123">These certificates are only used for encrypting and decrypting the private configuration of the extension.</span></span> <span data-ttu-id="d033b-124">Pokud tyto certifikáty vyprší nezáleží.</span><span class="sxs-lookup"><span data-stu-id="d033b-124">It does not matter if these certificates expire.</span></span> <span data-ttu-id="d033b-125">Datum vypršení platnosti není zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="d033b-125">The expiration date is not checked.</span></span>

### <a name="certificates-i-have-deleted-keep-reappearing"></a><span data-ttu-id="d033b-126">Zobrazují se certifikátů, které mají vymazání</span><span class="sxs-lookup"><span data-stu-id="d033b-126">Certificates I have deleted keep reappearing</span></span>
<span data-ttu-id="d033b-127">Tyto zobrazují se pravděpodobně z důvodu nástroj, který používáte, jako je například Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d033b-127">These keep reappearing most likely because of a tool you're using, such as Visual Studio.</span></span> <span data-ttu-id="d033b-128">Vždy, když se znovu připojíte s nástroj, který používá certifikát, bude znovu nahrán do Azure.</span><span class="sxs-lookup"><span data-stu-id="d033b-128">Whenever you reconnect with a tool that is using a certificate, it will again be uploaded to Azure.</span></span>

### <a name="my-certificates-keep-disappearing"></a><span data-ttu-id="d033b-129">Zachovat zmizení certifikáty</span><span class="sxs-lookup"><span data-stu-id="d033b-129">My certificates keep disappearing</span></span>
<span data-ttu-id="d033b-130">Dojde k recyklování instanci virtuálního počítače, všechny místní změny budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="d033b-130">When the virtual machine instance recycles, all local changes are lost.</span></span> <span data-ttu-id="d033b-131">Použití [úloha spuštění](cloud-services-startup-tasks.md) instalace certifikátů k virtuálnímu počítači při každém spuštění role.</span><span class="sxs-lookup"><span data-stu-id="d033b-131">Use a [startup task](cloud-services-startup-tasks.md) to install certificates to the virtual machine each time the role starts.</span></span>

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a><span data-ttu-id="d033b-132">Certifikáty pro správu nelze najít na portálu</span><span class="sxs-lookup"><span data-stu-id="d033b-132">I cannot find my management certificates in the portal</span></span>
<span data-ttu-id="d033b-133">[Certifikáty pro správu](../azure-api-management-certs.md) jsou dostupné jenom v portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="d033b-133">[Management certificates](../azure-api-management-certs.md) are only available in the Azure Classic Portal.</span></span> <span data-ttu-id="d033b-134">Na aktuálním portálu Azure nepoužívá certifikáty pro správu.</span><span class="sxs-lookup"><span data-stu-id="d033b-134">The current Azure portal does not use management certificates.</span></span> 

### <a name="how-can-i-disable-a-management-certificate"></a><span data-ttu-id="d033b-135">Jak můžete zakázat certifikát pro správu?</span><span class="sxs-lookup"><span data-stu-id="d033b-135">How can I disable a management certificate?</span></span>
<span data-ttu-id="d033b-136">[Certifikáty pro správu](../azure-api-management-certs.md) nelze zakázat.</span><span class="sxs-lookup"><span data-stu-id="d033b-136">[Management certificates](../azure-api-management-certs.md) cannot be disabled.</span></span> <span data-ttu-id="d033b-137">Můžete je odstranit prostřednictvím portálu Azure Classic když nechcete, aby se už používá.</span><span class="sxs-lookup"><span data-stu-id="d033b-137">You delete them through the Azure Classic Portal when you do not want them to be used anymore.</span></span>

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a><span data-ttu-id="d033b-138">Jak vytvořit certifikát protokolu SSL pro konkrétní IP adresu?</span><span class="sxs-lookup"><span data-stu-id="d033b-138">How do I create an SSL certificate for a specific IP address?</span></span>
<span data-ttu-id="d033b-139">Postupujte podle pokynů [vytvořit certifikát kurzu](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="d033b-139">Follow the directions in the [create a certificate tutorial](cloud-services-certs-create.md).</span></span> <span data-ttu-id="d033b-140">Použijte IP adresu jako název DNS.</span><span class="sxs-lookup"><span data-stu-id="d033b-140">Use the IP address as the DNS Name.</span></span>

## <a name="security"></a><span data-ttu-id="d033b-141">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="d033b-141">Security</span></span>
### <a name="disable-ssl-30"></a><span data-ttu-id="d033b-142">Protokol SSL 3.0 zakázat</span><span class="sxs-lookup"><span data-stu-id="d033b-142">Disable SSL 3.0</span></span>
<span data-ttu-id="d033b-143">Zákaz protokolu SSL 3.0 a TLS zabezpečení použít, vytvořte spuštění úloh, která je popsaná v tomto příspěvku na blogu: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/</span><span class="sxs-lookup"><span data-stu-id="d033b-143">To disable SSL 3.0 and use TLS security, create a startup task which is documented on this blog post: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/</span></span>

### <a name="add-nosniff-to-your-website"></a><span data-ttu-id="d033b-144">Přidat **nosniff** na váš web</span><span class="sxs-lookup"><span data-stu-id="d033b-144">Add **nosniff** to your website</span></span>
<span data-ttu-id="d033b-145">Pokud chcete klientům zabránit sledování toku dat typy MIME, přidejte nastavení ve vaší *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="d033b-145">To prevent clients from sniffing the MIME types, add a setting in your *web.config* file.</span></span>

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

<span data-ttu-id="d033b-146">To můžete také přidat jako nastavení ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="d033b-146">You can also add this as a setting in IIS.</span></span> <span data-ttu-id="d033b-147">Použijte následující příkaz se [běžné úlohy spuštění](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) článku.</span><span class="sxs-lookup"><span data-stu-id="d033b-147">Use the following command with the [common startup tasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) article.</span></span>

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="customize-iis-for-a-web-role"></a><span data-ttu-id="d033b-148">Přizpůsobení služby IIS pro webovou roli</span><span class="sxs-lookup"><span data-stu-id="d033b-148">Customize IIS for a web role</span></span>
<span data-ttu-id="d033b-149">Použijte spouštěcí skript služby IIS z [běžné úlohy spuštění](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) článku.</span><span class="sxs-lookup"><span data-stu-id="d033b-149">Use the IIS startup script from the [common startup tasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) article.</span></span>

## <a name="scaling"></a><span data-ttu-id="d033b-150">Škálování</span><span class="sxs-lookup"><span data-stu-id="d033b-150">Scaling</span></span>
### <a name="i-cannot-scale-beyond-x-instances"></a><span data-ttu-id="d033b-151">Nelze nastavit mimo X instancí</span><span class="sxs-lookup"><span data-stu-id="d033b-151">I cannot scale beyond X instances</span></span>
<span data-ttu-id="d033b-152">Vaše předplatné Azure limit je počet jader, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="d033b-152">Your Azure Subscription has a limit on the number of cores you can use.</span></span> <span data-ttu-id="d033b-153">Škálování nebude fungovat, pokud jste použili všechna jádra, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d033b-153">Scaling will not work if you have used all the cores available.</span></span> <span data-ttu-id="d033b-154">Například pokud máte limit 100 jader, znamená to může mít 100 instancí virtuální počítač A1 s nastavenou velikostí pro cloudové služby, nebo velikosti 50 A2 instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d033b-154">For example, if you have a limit of 100 cores, this means you could have 100 A1 sized virtual machine instances for your cloud service, or 50 A2 sized virtual machine instances.</span></span>

## <a name="networking"></a><span data-ttu-id="d033b-155">Sítě</span><span class="sxs-lookup"><span data-stu-id="d033b-155">Networking</span></span>
### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a><span data-ttu-id="d033b-156">I nelze rezervovat IP adresy v cloudové službě více virtuálních IP adres</span><span class="sxs-lookup"><span data-stu-id="d033b-156">I can't reserve an IP in a multi-VIP cloud service</span></span>
<span data-ttu-id="d033b-157">Zkontrolujte, jestli je zapnutá instanci virtuálního počítače, který se pokoušíte rezervovat IP adresu pro.</span><span class="sxs-lookup"><span data-stu-id="d033b-157">First, make sure that the virtual machine instance that you're trying to reserve the IP for is turned on.</span></span> <span data-ttu-id="d033b-158">Poté se ujistěte, že používáte vyhrazené IP adresy pro nepokoušejte pracovní a provozní nasazení.</span><span class="sxs-lookup"><span data-stu-id="d033b-158">Second, make sure that you're using Reserved IPs for bother the staging and production deployments.</span></span> <span data-ttu-id="d033b-159">**Nechcete** změnit nastavení při nasazení je upgradu.</span><span class="sxs-lookup"><span data-stu-id="d033b-159">**Do not** change the settings while the deployment is upgrading.</span></span>

## <a name="remote-desktop"></a><span data-ttu-id="d033b-160">Vzdálená plocha</span><span class="sxs-lookup"><span data-stu-id="d033b-160">Remote desktop</span></span>
### <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a><span data-ttu-id="d033b-161">Jak mohu vzdálené plochy Pokud skupinu NSG?</span><span class="sxs-lookup"><span data-stu-id="d033b-161">How do I remote desktop when I have an NSG?</span></span>
<span data-ttu-id="d033b-162">Přidat pravidla k této skupině, která povolí komunikaci na portech **3389** a **20000**.</span><span class="sxs-lookup"><span data-stu-id="d033b-162">Add rules to the NSG that allow traffic on ports **3389** and **20000**.</span></span>  <span data-ttu-id="d033b-163">Vzdálená plocha používá port **3389**.</span><span class="sxs-lookup"><span data-stu-id="d033b-163">Remote Desktop uses port **3389**.</span></span>  <span data-ttu-id="d033b-164">Instance cloudové služby jsou Vyrovnávané, takže nemůže přímo řídit kterou instanci pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="d033b-164">Cloud Service instances are load balanced, so you can't directly control which instance to connect to.</span></span>  <span data-ttu-id="d033b-165">*RemoteForwarder* a *RemoteAccess* agenty spravovat provoz protokolu RDP a umožňují klientu odesílat soubor cookie s RDP a zadejte jednotlivé instance pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="d033b-165">The *RemoteForwarder* and *RemoteAccess* agents manage RDP traffic and allow the client to send an RDP cookie and specify an individual instance to connect to.</span></span>  <span data-ttu-id="d033b-166">*RemoteForwarder* a *RemoteAccess* agentů vyžadují tento port **20000*** otevřít, což může být zablokován, pokud máte skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="d033b-166">The *RemoteForwarder* and *RemoteAccess* agents require that port **20000*** be opened, which may be blocked if you have an NSG.</span></span>
