---
title: "Služby a správu certifikátů aaaCloud | Microsoft Docs"
description: "Zjistěte, jak toocreate a používání certifikátů v Microsoft Azure"
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
ms.openlocfilehash: 69cb5467ece58a91dae06b4120954aeb2826bde1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="certificates-overview-for-azure-cloud-services"></a><span data-ttu-id="6032a-103">Přehled certifikáty pro cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="6032a-103">Certificates overview for Azure Cloud Services</span></span>
<span data-ttu-id="6032a-104">Certifikáty se používají v Azure pro cloudové služby ([služby certifikáty](#what-are-service-certificates)) a pro ověřování pomocí rozhraní API pro správu hello ([certifikáty pro správu](#what-are-management-certificates) při použití hello portál Azure classic a ne Hello ne klasický portál Azure).</span><span class="sxs-lookup"><span data-stu-id="6032a-104">Certificates are used in Azure for cloud services ([service certificates](#what-are-service-certificates)) and for authenticating with hello management API ([management certificates](#what-are-management-certificates) when using hello Azure classic portal and not hello non-classic Azure portal).</span></span> <span data-ttu-id="6032a-105">Toto téma obsahuje obecný přehled oba typy certifikátů, jak příliš[vytvořit](#create) a [nasazení](#deploy) je tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6032a-105">This topic gives a general overview of both certificate types, how too[create](#create) and [deploy](#deploy) them tooAzure.</span></span>

<span data-ttu-id="6032a-106">Certifikáty používané v Azure jsou x.509 v3 certifikáty a může být podepsány jiný certifikát důvěryhodné nebo mohou být podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="6032a-106">Certificates used in Azure are x.509 v3 certificates and can be signed by another trusted certificate or they can be self-signed.</span></span> <span data-ttu-id="6032a-107">Certifikát podepsaný svým držitelem je podepsána vlastní creator, proto není důvěryhodný ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6032a-107">A self-signed certificate is signed by its own creator, therefore it is not trusted by default.</span></span> <span data-ttu-id="6032a-108">Tento problém můžete ignorovat většina prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="6032a-108">Most browsers can ignore this problem.</span></span> <span data-ttu-id="6032a-109">Byste měli používat jenom certifikáty podepsané svým držitelem při vývoji a testování vaší cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6032a-109">You should only use self-signed certificates when developing and testing your cloud services.</span></span> 

<span data-ttu-id="6032a-110">Certifikáty používané Azure může obsahovat privátního nebo veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="6032a-110">Certificates used by Azure can contain a private or a public key.</span></span> <span data-ttu-id="6032a-111">Kryptografický otisk, která poskytuje tooidentify znamená mít certifikáty je jednoznačným způsobem.</span><span class="sxs-lookup"><span data-stu-id="6032a-111">Certificates have a thumbprint that provides a means tooidentify them in an unambiguous way.</span></span> <span data-ttu-id="6032a-112">Tímto kryptografickým otiskem se používá v hello Azure [konfigurační soubor](cloud-services-configure-ssl-certificate.md) tooidentify, který certifikát Cloudová služba by měl používat.</span><span class="sxs-lookup"><span data-stu-id="6032a-112">This thumbprint is used in hello Azure [configuration file](cloud-services-configure-ssl-certificate.md) tooidentify which certificate a cloud service should use.</span></span> 

## <a name="what-are-service-certificates"></a><span data-ttu-id="6032a-113">Jaké jsou certifikáty služby?</span><span class="sxs-lookup"><span data-stu-id="6032a-113">What are service certificates?</span></span>
<span data-ttu-id="6032a-114">Certifikáty služby jsou připojené toocloud služby a povolte tooand zabezpečenou komunikaci ze služby hello.</span><span class="sxs-lookup"><span data-stu-id="6032a-114">Service certificates are attached toocloud services and enable secure communication tooand from hello service.</span></span> <span data-ttu-id="6032a-115">Například pokud jste nasadili webovou roli, je vhodné toosupply certifikát, který může ověřit zveřejněné koncový bod HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6032a-115">For example, if you deployed a web role, you would want toosupply a certificate that can authenticate an exposed HTTPS endpoint.</span></span> <span data-ttu-id="6032a-116">Certifikáty služby definované v definice služby, jsou automaticky nasazené toohello virtuální počítač, který je spuštěna instance role.</span><span class="sxs-lookup"><span data-stu-id="6032a-116">Service certificates, defined in your service definition, are automatically deployed toohello virtual machine that is running an instance of your role.</span></span> 

<span data-ttu-id="6032a-117">Můžete nahrát služby certifikáty tooAzure klasickém portálu buď pomocí hello portál Azure classic portálu nebo pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="6032a-117">You can upload service certificates tooAzure classic portal either using hello Azure classic portal or by using hello classic deployment model.</span></span> <span data-ttu-id="6032a-118">Certifikáty služby jsou přidruženy ke konkrétní cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="6032a-118">Service certificates are associated with a specific cloud service.</span></span> <span data-ttu-id="6032a-119">Jsou přiřazeny tooa nasazení v souboru definice služby hello.</span><span class="sxs-lookup"><span data-stu-id="6032a-119">They are assigned tooa deployment in hello service definition file.</span></span>

<span data-ttu-id="6032a-120">Certifikáty služby může být spravován od něj odděleně vašim službám a mohou být spravovány nástrojem různí jednotlivci.</span><span class="sxs-lookup"><span data-stu-id="6032a-120">Service certificates can be managed separately from your services, and may be managed by different individuals.</span></span> <span data-ttu-id="6032a-121">Vývojář může například odeslat balíček služby, která odkazuje správce IT se předtím nahrála tooAzure certifikát tooa.</span><span class="sxs-lookup"><span data-stu-id="6032a-121">For example, a developer may upload a service package that refers tooa certificate that an IT manager has previously uploaded tooAzure.</span></span> <span data-ttu-id="6032a-122">Správce IT můžete spravovat a obnovit tento certifikát (Změna konfigurace hello služby hello) bez nutnosti tooupload nový balíček pro službu.</span><span class="sxs-lookup"><span data-stu-id="6032a-122">An IT manager can manage and renew that certificate (changing hello configuration of hello service) without needing tooupload a new service package.</span></span> <span data-ttu-id="6032a-123">Aktualizace bez nový balíček služby je možné, protože logický název hello, název úložiště a umístění certifikátu hello je v souboru definice služby hello a když hello kryptografický otisk certifikátu je uveden v konfiguračním souboru služby hello.</span><span class="sxs-lookup"><span data-stu-id="6032a-123">Updating without a new service package is possible because hello logical name, store name, and location of hello certificate is in hello service definition file and while hello certificate thumbprint is specified in hello service configuration file.</span></span> <span data-ttu-id="6032a-124">tooupdate hello certifikát, je pouze nezbytné tooupload nový certifikát a kryptografický otisk hello změnit hodnotu v konfiguračním souboru služby hello.</span><span class="sxs-lookup"><span data-stu-id="6032a-124">tooupdate hello certificate, it's only necessary tooupload a new certificate and change hello thumbprint value in hello service configuration file.</span></span>

>[!Note]
><span data-ttu-id="6032a-125">Hello [nejčastější dotazy týkající se služby Cloud](cloud-services-faq.md) článek obsahuje některé užitečné informace o certifikátech.</span><span class="sxs-lookup"><span data-stu-id="6032a-125">hello [Cloud Services FAQ](cloud-services-faq.md) article has some helpful information about certificates.</span></span>

## <a name="what-are-management-certificates"></a><span data-ttu-id="6032a-126">Jaké jsou certifikáty pro správu?</span><span class="sxs-lookup"><span data-stu-id="6032a-126">What are management certificates?</span></span>
<span data-ttu-id="6032a-127">Certifikáty pro správu povolit tooauthenticate s modelem nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="6032a-127">Management certificates allow you tooauthenticate with hello classic deployment model.</span></span> <span data-ttu-id="6032a-128">Mnoho programy a nástroje (například Visual Studio nebo hello Azure SDK) používat tyto certifikáty tooautomate konfigurace a nasazení různých služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="6032a-128">Many programs and tools (such as Visual Studio or hello Azure SDK) use these certificates tooautomate configuration and deployment of various Azure services.</span></span> <span data-ttu-id="6032a-129">Tyto nejsou skutečně související toocloud služby.</span><span class="sxs-lookup"><span data-stu-id="6032a-129">These are not really related toocloud services.</span></span> 

> [!WARNING]
> <span data-ttu-id="6032a-130">Dej si pozor!</span><span class="sxs-lookup"><span data-stu-id="6032a-130">Be careful!</span></span> <span data-ttu-id="6032a-131">Tyto typy certifikátů povolit všem uživatelům, kteří s nimi ověřuje toomanage hello předplatného jsou přidruženy.</span><span class="sxs-lookup"><span data-stu-id="6032a-131">These types of certificates allow anyone who authenticates with them toomanage hello subscription they are associated with.</span></span> 
> 
> 

### <a name="limitations"></a><span data-ttu-id="6032a-132">Omezení</span><span class="sxs-lookup"><span data-stu-id="6032a-132">Limitations</span></span>
<span data-ttu-id="6032a-133">Existuje limit 100 certifikáty pro správu podle předplatného.</span><span class="sxs-lookup"><span data-stu-id="6032a-133">There is a limit of 100 management certificates per subscription.</span></span> <span data-ttu-id="6032a-134">Je také limit 100 certifikáty pro správu pro všechna předplatná pod ID specifické služby správce uživatele.</span><span class="sxs-lookup"><span data-stu-id="6032a-134">There is also a limit of 100 management certificates for all subscriptions under a specific service administrator’s user ID.</span></span> <span data-ttu-id="6032a-135">Pokud hello ID uživatele pro správce účtu hello již bylo použité tooadd 100 certifikáty pro správu a je nezbytné pro další certifikáty, můžete přidat další certifikáty spolusprávcem tooadd hello.</span><span class="sxs-lookup"><span data-stu-id="6032a-135">If hello user ID for hello account administrator has already been used tooadd 100 management certificates and there is a need for more certificates, you can add a co-administrator tooadd hello additional certificates.</span></span> 

<span data-ttu-id="6032a-136">Před přidáním více než 100 certifikáty, najdete v části Pokud můžete opakovaně použít stávající certifikát.</span><span class="sxs-lookup"><span data-stu-id="6032a-136">Before adding more than 100 certificates, see if you can reuse an existing certificate.</span></span> <span data-ttu-id="6032a-137">Pomocí spolusprávci přidá potenciálně nepotřebné složitost tooyour certifikátu správy.</span><span class="sxs-lookup"><span data-stu-id="6032a-137">Using co-administrators adds potentially unneeded complexity tooyour certificate management process.</span></span>

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a><span data-ttu-id="6032a-138">Vytvořit nový certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="6032a-138">Create a new self-signed certificate</span></span>
<span data-ttu-id="6032a-139">K dispozici toocreate žádné nástroj certifikát podepsaný svým držitelem můžete použít, dokud budou vyhovovat toothese nastavení:</span><span class="sxs-lookup"><span data-stu-id="6032a-139">You can use any tool available toocreate a self-signed certificate as long as they adhere toothese settings:</span></span>

* <span data-ttu-id="6032a-140">Certifikát X.509.</span><span class="sxs-lookup"><span data-stu-id="6032a-140">An X.509 certificate.</span></span>
* <span data-ttu-id="6032a-141">Obsahuje privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="6032a-141">Contains a private key.</span></span>
* <span data-ttu-id="6032a-142">Vytvoří pro výměnu klíčů (soubor .pfx).</span><span class="sxs-lookup"><span data-stu-id="6032a-142">Created for key exchange (.pfx file).</span></span>
* <span data-ttu-id="6032a-143">Název subjektu musí odpovídat hello domény použít tooaccess hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6032a-143">Subject name must match hello domain used tooaccess hello cloud service.</span></span>

    > <span data-ttu-id="6032a-144">Nelze získat certifikát protokolu SSL pro hello cloudapp.net (nebo pro jakékoli souvisejících s Azure) domény; Název subjektu certifikátu Hello musí odpovídat tooaccess název používaný hello vlastní domény aplikace.</span><span class="sxs-lookup"><span data-stu-id="6032a-144">You cannot acquire an SSL certificate for hello cloudapp.net (or for any Azure-related) domain; hello certificate's subject name must match hello custom domain name used tooaccess your application.</span></span> <span data-ttu-id="6032a-145">Například **contoso.net**, nikoli **contoso.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="6032a-145">For example, **contoso.net**, not **contoso.cloudapp.net**.</span></span>

* <span data-ttu-id="6032a-146">Minimálně 2048bitové šifrování.</span><span class="sxs-lookup"><span data-stu-id="6032a-146">Minimum of 2048-bit encryption.</span></span>
* <span data-ttu-id="6032a-147">**Služba certifikátu pouze**: klientský certifikát se musí nacházet v hello *osobní* úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="6032a-147">**Service Certificate Only**: Client-side certificate must reside in hello *Personal* certificate store.</span></span>

<span data-ttu-id="6032a-148">Existují dva způsoby, jak snadno toocreate certifikát v systému Windows, s hello `makecert.exe` nástroje nebo služby IIS.</span><span class="sxs-lookup"><span data-stu-id="6032a-148">There are two easy ways toocreate a certificate on Windows, with hello `makecert.exe` utility, or IIS.</span></span>

### <a name="makecertexe"></a><span data-ttu-id="6032a-149">MakeCert.exe</span><span class="sxs-lookup"><span data-stu-id="6032a-149">Makecert.exe</span></span>
<span data-ttu-id="6032a-150">Tento nástroj je zastaralá a již jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="6032a-150">This utility has been deprecated and is no longer documented here.</span></span> <span data-ttu-id="6032a-151">Další informace najdete v tématu [tohoto článku na webu MSDN](https://msdn.microsoft.com/library/windows/desktop/aa386968).</span><span class="sxs-lookup"><span data-stu-id="6032a-151">For more information, see [this MSDN article](https://msdn.microsoft.com/library/windows/desktop/aa386968).</span></span>

### <a name="powershell"></a><span data-ttu-id="6032a-152">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6032a-152">PowerShell</span></span>
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> <span data-ttu-id="6032a-153">Pokud chcete certifikát hello toouse s IP adresou místo k doméně, použijte parametr - DnsName hello hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6032a-153">If you want toouse hello certificate with an IP address instead of a domain, use hello IP address in hello -DnsName parameter.</span></span>


<span data-ttu-id="6032a-154">Pokud chcete, aby toouse to [certifikátu pomocí portálu správy hello](../azure-api-management-certs.md), ho exportovat tooa **.cer** souboru:</span><span class="sxs-lookup"><span data-stu-id="6032a-154">If you want toouse this [certificate with hello management portal](../azure-api-management-certs.md), export it tooa **.cer** file:</span></span>

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a><span data-ttu-id="6032a-155">Internetová informační služba (IIS)</span><span class="sxs-lookup"><span data-stu-id="6032a-155">Internet Information Services (IIS)</span></span>
<span data-ttu-id="6032a-156">Existuje mnoho stránek na hello internet, které zahrnují jak toodo to pomocí služby IIS.</span><span class="sxs-lookup"><span data-stu-id="6032a-156">There are many pages on hello internet that cover how toodo this with IIS.</span></span> <span data-ttu-id="6032a-157">[Zde](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) je skvělým našel jsem myslím ji a vysvětluje.</span><span class="sxs-lookup"><span data-stu-id="6032a-157">[Here](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) is a great one I found that I think explains it well.</span></span> 

### <a name="java"></a><span data-ttu-id="6032a-158">Java</span><span class="sxs-lookup"><span data-stu-id="6032a-158">Java</span></span>
<span data-ttu-id="6032a-159">Můžete použít Java příliš[vytvořit certifikát](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).</span><span class="sxs-lookup"><span data-stu-id="6032a-159">You can use Java too[create a certificate](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).</span></span>

### <a name="linux"></a><span data-ttu-id="6032a-160">Linux</span><span class="sxs-lookup"><span data-stu-id="6032a-160">Linux</span></span>
<span data-ttu-id="6032a-161">[To](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) článek popisuje, jak toocreate certifikáty pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="6032a-161">[This](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) article describes how toocreate certificates with SSH.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6032a-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6032a-162">Next steps</span></span>
<span data-ttu-id="6032a-163">[Nahrajte certifikát toohello vaší služby portálu Azure classic](cloud-services-configure-ssl-certificate.md) (nebo hello [portál Azure](cloud-services-configure-ssl-certificate-portal.md)).</span><span class="sxs-lookup"><span data-stu-id="6032a-163">[Upload your service certificate toohello Azure classic portal](cloud-services-configure-ssl-certificate.md) (or hello [Azure portal](cloud-services-configure-ssl-certificate-portal.md)).</span></span>

<span data-ttu-id="6032a-164">Nahrát [certifikát správy rozhraní API](../azure-api-management-certs.md) toohello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="6032a-164">Upload a [management API certificate](../azure-api-management-certs.md) toohello Azure classic portal.</span></span> <span data-ttu-id="6032a-165">Hello portál Azure nepoužívá certifikáty pro správu pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="6032a-165">hello Azure portal does not use management certificates for authentication.</span></span>

