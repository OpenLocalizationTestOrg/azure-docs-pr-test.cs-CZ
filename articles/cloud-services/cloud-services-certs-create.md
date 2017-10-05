---
title: "Cloudové služby a certifikáty pro správu | Microsoft Docs"
description: "Zjistěte, jak vytvořit a použít certifikáty s Microsoft Azure"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: fc70d00d-899b-4771-855f-44574dc4bfc6
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: f760bfd93b19c43d12889b5dd38015c5eba0a8ac
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="certificates-overview-for-azure-cloud-services"></a><span data-ttu-id="cfdb6-103">Přehled certifikáty pro cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="cfdb6-103">Certificates overview for Azure Cloud Services</span></span>
<span data-ttu-id="cfdb6-104">Certifikáty se používají v Azure pro cloudové služby ([služby certifikáty](#what-are-service-certificates)) a pro ověřování pomocí rozhraní API pro správu ([certifikáty pro správu](#what-are-management-certificates) při použití portálu Azure classic a ne portál ne klasický Azure).</span><span class="sxs-lookup"><span data-stu-id="cfdb6-104">Certificates are used in Azure for cloud services ([service certificates](#what-are-service-certificates)) and for authenticating with the management API ([management certificates](#what-are-management-certificates) when using the Azure classic portal and not the non-classic Azure portal).</span></span> <span data-ttu-id="cfdb6-105">Toto téma obsahuje obecný přehled oba typy certifikátů, jak k [vytvořit](#create) a [nasazení](#deploy) je do Azure.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-105">This topic gives a general overview of both certificate types, how to [create](#create) and [deploy](#deploy) them to Azure.</span></span>

<span data-ttu-id="cfdb6-106">Certifikáty používané v Azure jsou x.509 v3 certifikáty a může být podepsány jiný certifikát důvěryhodné nebo mohou být podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-106">Certificates used in Azure are x.509 v3 certificates and can be signed by another trusted certificate or they can be self-signed.</span></span> <span data-ttu-id="cfdb6-107">Certifikát podepsaný svým držitelem je podepsána vlastní creator, proto není důvěryhodný ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-107">A self-signed certificate is signed by its own creator, therefore it is not trusted by default.</span></span> <span data-ttu-id="cfdb6-108">Tento problém můžete ignorovat většina prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-108">Most browsers can ignore this problem.</span></span> <span data-ttu-id="cfdb6-109">Byste měli používat jenom certifikáty podepsané svým držitelem při vývoji a testování vaší cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-109">You should only use self-signed certificates when developing and testing your cloud services.</span></span> 

<span data-ttu-id="cfdb6-110">Certifikáty používané Azure může obsahovat privátního nebo veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-110">Certificates used by Azure can contain a private or a public key.</span></span> <span data-ttu-id="cfdb6-111">Certifikáty mít kryptografický otisk, která poskytuje prostředky k identifikaci je jednoznačným způsobem.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-111">Certificates have a thumbprint that provides a means to identify them in an unambiguous way.</span></span> <span data-ttu-id="cfdb6-112">Tímto kryptografickým otiskem slouží ve službě Azure [konfigurační soubor](cloud-services-configure-ssl-certificate.md) k identifikaci měli použít který certifikátu cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-112">This thumbprint is used in the Azure [configuration file](cloud-services-configure-ssl-certificate.md) to identify which certificate a cloud service should use.</span></span> 

## <a name="what-are-service-certificates"></a><span data-ttu-id="cfdb6-113">Jaké jsou certifikáty služby?</span><span class="sxs-lookup"><span data-stu-id="cfdb6-113">What are service certificates?</span></span>
<span data-ttu-id="cfdb6-114">Certifikáty služby jsou připojené ke cloudové služby a zajištění zabezpečené komunikace do a ze služby.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-114">Service certificates are attached to cloud services and enable secure communication to and from the service.</span></span> <span data-ttu-id="cfdb6-115">Pokud jste nasadili webovou roli, by například chcete zadat certifikát, který může ověřit zveřejněné koncový bod HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-115">For example, if you deployed a web role, you would want to supply a certificate that can authenticate an exposed HTTPS endpoint.</span></span> <span data-ttu-id="cfdb6-116">Certifikáty služby definované v definice služby, se automaticky nasadí do virtuálního počítače, který je spuštěna instance role.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-116">Service certificates, defined in your service definition, are automatically deployed to the virtual machine that is running an instance of your role.</span></span> 

<span data-ttu-id="cfdb6-117">Můžete nahrát certifikáty služby na portálu Azure classic, buď pomocí portálu Azure classic nebo pomocí modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-117">You can upload service certificates to Azure classic portal either using the Azure classic portal or by using the classic deployment model.</span></span> <span data-ttu-id="cfdb6-118">Certifikáty služby jsou přidruženy ke konkrétní cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-118">Service certificates are associated with a specific cloud service.</span></span> <span data-ttu-id="cfdb6-119">Jsou přiřazeny k nasazení v definičním souboru služby.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-119">They are assigned to a deployment in the service definition file.</span></span>

<span data-ttu-id="cfdb6-120">Certifikáty služby může být spravován od něj odděleně vašim službám a mohou být spravovány nástrojem různí jednotlivci.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-120">Service certificates can be managed separately from your services, and may be managed by different individuals.</span></span> <span data-ttu-id="cfdb6-121">Vývojář může například odeslat balíček služby, který odkazuje na certifikát, který se správce IT se předtím nahrála do Azure.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-121">For example, a developer may upload a service package that refers to a certificate that an IT manager has previously uploaded to Azure.</span></span> <span data-ttu-id="cfdb6-122">Správce IT můžete spravovat a obnovit tento certifikát (Změna konfigurace služby) bez nutnosti nahrát nový balíček pro službu.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-122">An IT manager can manage and renew that certificate (changing the configuration of the service) without needing to upload a new service package.</span></span> <span data-ttu-id="cfdb6-123">Aktualizace bez nový balíček služby je možné, protože logický název, název úložiště a umístění certifikátu naleznete v souboru definice služby, a když kryptografický otisk certifikátu je uveden v konfiguračním souboru služby.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-123">Updating without a new service package is possible because the logical name, store name, and location of the certificate is in the service definition file and while the certificate thumbprint is specified in the service configuration file.</span></span> <span data-ttu-id="cfdb6-124">Pokud chcete aktualizovat certifikát, je pouze potřeba nahrát nový certifikát a změňte hodnotu kryptografický otisk v konfiguračním souboru služby.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-124">To update the certificate, it's only necessary to upload a new certificate and change the thumbprint value in the service configuration file.</span></span>

>[!Note]
><span data-ttu-id="cfdb6-125">[Nejčastější dotazy týkající se služby Cloud](cloud-services-faq.md) článek obsahuje některé užitečné informace o certifikátech.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-125">The [Cloud Services FAQ](cloud-services-faq.md) article has some helpful information about certificates.</span></span>

## <a name="what-are-management-certificates"></a><span data-ttu-id="cfdb6-126">Jaké jsou certifikáty pro správu?</span><span class="sxs-lookup"><span data-stu-id="cfdb6-126">What are management certificates?</span></span>
<span data-ttu-id="cfdb6-127">Certifikáty pro správu umožňují ověření pomocí modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-127">Management certificates allow you to authenticate with the classic deployment model.</span></span> <span data-ttu-id="cfdb6-128">Mnoho programy a nástroje (například Visual Studio nebo sadu Azure SDK) použít tyto certifikáty k automatizaci konfigurace a nasazení různých služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-128">Many programs and tools (such as Visual Studio or the Azure SDK) use these certificates to automate configuration and deployment of various Azure services.</span></span> <span data-ttu-id="cfdb6-129">Tyto spolu nesouvisejí skutečně cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-129">These are not really related to cloud services.</span></span> 

> [!WARNING]
> <span data-ttu-id="cfdb6-130">Dej si pozor!</span><span class="sxs-lookup"><span data-stu-id="cfdb6-130">Be careful!</span></span> <span data-ttu-id="cfdb6-131">Tyto typy certifikátů, povolí všem uživatelům, kteří se mají spravovat předplatné, které jsou přidruženy ověřuje.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-131">These types of certificates allow anyone who authenticates with them to manage the subscription they are associated with.</span></span> 
> 
> 

### <a name="limitations"></a><span data-ttu-id="cfdb6-132">Omezení</span><span class="sxs-lookup"><span data-stu-id="cfdb6-132">Limitations</span></span>
<span data-ttu-id="cfdb6-133">Existuje limit 100 certifikáty pro správu podle předplatného.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-133">There is a limit of 100 management certificates per subscription.</span></span> <span data-ttu-id="cfdb6-134">Je také limit 100 certifikáty pro správu pro všechna předplatná pod ID specifické služby správce uživatele.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-134">There is also a limit of 100 management certificates for all subscriptions under a specific service administrator’s user ID.</span></span> <span data-ttu-id="cfdb6-135">Pokud ID uživatele pro účet správce již byl použit pro přidání 100 certifikáty pro správu a je nezbytné pro další certifikáty, můžete přidat společné správce o přidání dalších certifikátů.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-135">If the user ID for the account administrator has already been used to add 100 management certificates and there is a need for more certificates, you can add a co-administrator to add the additional certificates.</span></span> 

<span data-ttu-id="cfdb6-136">Před přidáním více než 100 certifikáty, najdete v části Pokud můžete opakovaně použít stávající certifikát.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-136">Before adding more than 100 certificates, see if you can reuse an existing certificate.</span></span> <span data-ttu-id="cfdb6-137">Potenciálně nepotřebné složitost pomocí spolusprávci přidá do vašeho certifikátu správy.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-137">Using co-administrators adds potentially unneeded complexity to your certificate management process.</span></span>

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a><span data-ttu-id="cfdb6-138">Vytvořit nový certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="cfdb6-138">Create a new self-signed certificate</span></span>
<span data-ttu-id="cfdb6-139">Můžete použít jakýkoli nástroj, který je možné vytvořit certifikát podepsaný svým držitelem tak dlouho, dokud budou splňovat tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="cfdb6-139">You can use any tool available to create a self-signed certificate as long as they adhere to these settings:</span></span>

* <span data-ttu-id="cfdb6-140">Certifikát X.509.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-140">An X.509 certificate.</span></span>
* <span data-ttu-id="cfdb6-141">Obsahuje privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-141">Contains a private key.</span></span>
* <span data-ttu-id="cfdb6-142">Vytvoří pro výměnu klíčů (soubor .pfx).</span><span class="sxs-lookup"><span data-stu-id="cfdb6-142">Created for key exchange (.pfx file).</span></span>
* <span data-ttu-id="cfdb6-143">Název subjektu musí odpovídat domény používá pro přístup ke cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-143">Subject name must match the domain used to access the cloud service.</span></span>

    > <span data-ttu-id="cfdb6-144">Nelze získat certifikát protokolu SSL pro cloudapp.net (nebo pro jakékoli souvisejících s Azure) domény; Název subjektu certifikátu musí odpovídat názvu vlastní domény, používá pro přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-144">You cannot acquire an SSL certificate for the cloudapp.net (or for any Azure-related) domain; the certificate's subject name must match the custom domain name used to access your application.</span></span> <span data-ttu-id="cfdb6-145">Například **contoso.net**, nikoli **contoso.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-145">For example, **contoso.net**, not **contoso.cloudapp.net**.</span></span>

* <span data-ttu-id="cfdb6-146">Minimálně 2048bitové šifrování.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-146">Minimum of 2048-bit encryption.</span></span>
* <span data-ttu-id="cfdb6-147">**Služba certifikátu pouze**: klientský certifikát se musí nacházet v *osobní* úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-147">**Service Certificate Only**: Client-side certificate must reside in the *Personal* certificate store.</span></span>

<span data-ttu-id="cfdb6-148">Existují dva způsoby snadné vytvoření certifikátu v systému Windows, pomocí `makecert.exe` nástroje nebo služby IIS.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-148">There are two easy ways to create a certificate on Windows, with the `makecert.exe` utility, or IIS.</span></span>

### <a name="makecertexe"></a><span data-ttu-id="cfdb6-149">MakeCert.exe</span><span class="sxs-lookup"><span data-stu-id="cfdb6-149">Makecert.exe</span></span>
<span data-ttu-id="cfdb6-150">Tento nástroj je zastaralá a již jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-150">This utility has been deprecated and is no longer documented here.</span></span> <span data-ttu-id="cfdb6-151">Další informace najdete v tématu [tohoto článku na webu MSDN](https://msdn.microsoft.com/library/windows/desktop/aa386968).</span><span class="sxs-lookup"><span data-stu-id="cfdb6-151">For more information, see [this MSDN article](https://msdn.microsoft.com/library/windows/desktop/aa386968).</span></span>

### <a name="powershell"></a><span data-ttu-id="cfdb6-152">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cfdb6-152">PowerShell</span></span>
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> <span data-ttu-id="cfdb6-153">Pokud chcete použít certifikát s IP adresou místo k doméně, použijte parametr - DnsName IP adresu.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-153">If you want to use the certificate with an IP address instead of a domain, use the IP address in the -DnsName parameter.</span></span>


<span data-ttu-id="cfdb6-154">Pokud chcete použít [certifikátu pomocí portálu pro správu](../azure-api-management-certs.md), exportovat je do **.cer** souboru:</span><span class="sxs-lookup"><span data-stu-id="cfdb6-154">If you want to use this [certificate with the management portal](../azure-api-management-certs.md), export it to a **.cer** file:</span></span>

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a><span data-ttu-id="cfdb6-155">Internetová informační služba (IIS)</span><span class="sxs-lookup"><span data-stu-id="cfdb6-155">Internet Information Services (IIS)</span></span>
<span data-ttu-id="cfdb6-156">Existuje mnoho stránek na Internetu, které se týkají jak to provést pomocí služby IIS.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-156">There are many pages on the internet that cover how to do this with IIS.</span></span> <span data-ttu-id="cfdb6-157">[Zde](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) je skvělým našel jsem myslím ji a vysvětluje.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-157">[Here](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) is a great one I found that I think explains it well.</span></span> 

### <a name="java"></a><span data-ttu-id="cfdb6-158">Java</span><span class="sxs-lookup"><span data-stu-id="cfdb6-158">Java</span></span>
<span data-ttu-id="cfdb6-159">Můžete použít Java k [vytvořit certifikát](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).</span><span class="sxs-lookup"><span data-stu-id="cfdb6-159">You can use Java to [create a certificate](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).</span></span>

### <a name="linux"></a><span data-ttu-id="cfdb6-160">Linux</span><span class="sxs-lookup"><span data-stu-id="cfdb6-160">Linux</span></span>
<span data-ttu-id="cfdb6-161">[To](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) článek popisuje, jak umožnit vytváření certifikátů pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-161">[This](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) article describes how to create certificates with SSH.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cfdb6-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cfdb6-162">Next steps</span></span>
<span data-ttu-id="cfdb6-163">[Nahrajte certifikát služby do portálu Azure classic](cloud-services-configure-ssl-certificate.md) (nebo [portál Azure](cloud-services-configure-ssl-certificate-portal.md)).</span><span class="sxs-lookup"><span data-stu-id="cfdb6-163">[Upload your service certificate to the Azure classic portal](cloud-services-configure-ssl-certificate.md) (or the [Azure portal](cloud-services-configure-ssl-certificate-portal.md)).</span></span>

<span data-ttu-id="cfdb6-164">Nahrát [certifikát správy rozhraní API](../azure-api-management-certs.md) na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-164">Upload a [management API certificate](../azure-api-management-certs.md) to the Azure classic portal.</span></span> <span data-ttu-id="cfdb6-165">Portál Azure nepoužívá certifikáty pro správu pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="cfdb6-165">The Azure portal does not use management certificates for authentication.</span></span>

