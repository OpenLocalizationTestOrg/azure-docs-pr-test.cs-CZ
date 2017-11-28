---
title: "aaaHow toouse hello služby rozhraní API pro správu (Python) – Průvodce funkcemi"
description: "Zjistěte, jak tooprogrammatically provádět běžné úlohy správy služby z Pythonu."
services: cloud-services
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: 61538ec0-1536-4a7e-ae89-95967fe35d73
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/30/2017
ms.author: lmazuel
ms.openlocfilehash: b59622203470e1586484cec4033515edb39ca4d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-management-from-python"></a><span data-ttu-id="859e0-103">Jak toouse Správa služby z Pythonu</span><span class="sxs-lookup"><span data-stu-id="859e0-103">How toouse Service Management from Python</span></span>
<span data-ttu-id="859e0-104">Tento průvodce vám ukáže, jak tooprogrammatically provádět běžné úlohy správy služby z Pythonu.</span><span class="sxs-lookup"><span data-stu-id="859e0-104">This guide shows you how tooprogrammatically perform common service management tasks from Python.</span></span> <span data-ttu-id="859e0-105">Hello **ServiceManagementService** třídy v hello [Azure SDK pro jazyk Python](https://github.com/Azure/azure-sdk-for-python) podporuje programový přístup toomuch hello služby týkajících se správy funkcí, které jsou k dispozici v hello [Portál azure classic] [ management-portal] (například **vytváření, aktualizaci a odstraňování cloudové služby, nasazení, služby pro správu dat a virtuální počítače**).</span><span class="sxs-lookup"><span data-stu-id="859e0-105">hello **ServiceManagementService** class in hello [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python) supports programmatic access toomuch of hello service management-related functionality that is available in hello [Azure classic portal][management-portal] (such as **creating, updating, and deleting cloud services, deployments, data management services, and virtual machines**).</span></span> <span data-ttu-id="859e0-106">Tato funkce může být užitečné při vytváření aplikace, které potřebují správu tooservice programový přístup.</span><span class="sxs-lookup"><span data-stu-id="859e0-106">This functionality can be useful in building applications that need programmatic access tooservice management.</span></span>

## <span data-ttu-id="859e0-107"><a name="WhatIs"></a>Co je služba správy</span><span class="sxs-lookup"><span data-stu-id="859e0-107"><a name="WhatIs"> </a>What is Service Management</span></span>
<span data-ttu-id="859e0-108">Hello Service Management API poskytuje programový přístup toomuch hello služby správy funkcí dostupných prostřednictvím hello [portál Azure classic][management-portal].</span><span class="sxs-lookup"><span data-stu-id="859e0-108">hello Service Management API provides programmatic access toomuch of hello service management functionality available through hello [Azure classic portal][management-portal].</span></span> <span data-ttu-id="859e0-109">Hello Azure SDK pro jazyk Python vám umožní toomanage cloudové služby a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="859e0-109">hello Azure SDK for Python allows you toomanage your cloud services and storage accounts.</span></span>

<span data-ttu-id="859e0-110">toouse hello Service Management API, musíte příliš[vytvoření účtu Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="859e0-110">toouse hello Service Management API, you need too[create an Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <span data-ttu-id="859e0-111"><a name="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="859e0-111"><a name="Concepts"> </a>Concepts</span></span>
<span data-ttu-id="859e0-112">Hello Azure SDK pro jazyk Python zabalí hello [Azure Service Management API][svc-mgmt-rest-api], což je rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="859e0-112">hello Azure SDK for Python wraps hello [Azure Service Management API][svc-mgmt-rest-api], which is a REST API.</span></span> <span data-ttu-id="859e0-113">Všechny operace rozhraní API se provádí přes SSL a vzájemně se ověřují pomocí certifikátů X.509 v3.</span><span class="sxs-lookup"><span data-stu-id="859e0-113">All API operations are performed over SSL and mutually authenticated using X.509 v3 certificates.</span></span> <span data-ttu-id="859e0-114">Služba správy Hello může přístup v rámci služby spuštěné v Azure, nebo přímo přes hello Internet z jakékoli aplikace, která může požadavek HTTPS odesílat a přijímat odpověď protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="859e0-114">hello management service may be accessed from within a service running in Azure, or directly over hello Internet from any application that can send an HTTPS request and receive an HTTPS response.</span></span>

## <span data-ttu-id="859e0-115"><a name="Installation"></a>Instalace</span><span class="sxs-lookup"><span data-stu-id="859e0-115"><a name="Installation"> </a>Installation</span></span>
<span data-ttu-id="859e0-116">Všechny funkce hello popsané v tomto článku jsou k dispozici v hello `azure-servicemanagement-legacy` balíčku, které můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="859e0-116">All hello features described in this article are available in hello `azure-servicemanagement-legacy` package, which you can install using pip.</span></span> <span data-ttu-id="859e0-117">Další informace o instalaci (například pokud jste nový tooPython), najdete v tomto článku: [instalaci Pythonu a hello Azure SDK](../python-how-to-install.md)</span><span class="sxs-lookup"><span data-stu-id="859e0-117">For more information about installation (for example, if you are new tooPython), see this article: [Installing Python and hello Azure SDK](../python-how-to-install.md)</span></span>

## <span data-ttu-id="859e0-118"><a name="Connect"></a>Postupy: připojení tooservice správy</span><span class="sxs-lookup"><span data-stu-id="859e0-118"><a name="Connect"> </a>How to: Connect tooservice management</span></span>
<span data-ttu-id="859e0-119">koncový bod Service Management toohello tooconnect, musíte svoje ID předplatného Azure a certifikát pro správu platné.</span><span class="sxs-lookup"><span data-stu-id="859e0-119">tooconnect toohello Service Management endpoint, you need your Azure subscription ID and a valid management certificate.</span></span> <span data-ttu-id="859e0-120">Můžete získat svoje ID předplatného prostřednictvím hello [portál Azure classic][management-portal].</span><span class="sxs-lookup"><span data-stu-id="859e0-120">You can obtain your subscription ID through hello [Azure classic portal][management-portal].</span></span>

> [!NOTE]
> <span data-ttu-id="859e0-121">Nyní je možné toouse certifikáty vytvořené pomocí OpenSSL, spuštěná v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="859e0-121">It is now possible toouse certificates created with OpenSSL when running on Windows.</span></span>  <span data-ttu-id="859e0-122">Vyžaduje Python 2.7.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="859e0-122">It requires Python 2.7.4 or later.</span></span> <span data-ttu-id="859e0-123">Doporučujeme, abyste uživatelům toouse OpenSSL místo .pfx, od podporu pro .pfx, certifikáty budou odebrány pravděpodobně v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="859e0-123">We recommend users toouse OpenSSL instead of .pfx, since support for .pfx certificates will likely be removed in hello future.</span></span>
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a><span data-ttu-id="859e0-124">Certifikáty pro správu v systému Windows nebo Mac/Linux (OpenSSL)</span><span class="sxs-lookup"><span data-stu-id="859e0-124">Management certificates on Windows/Mac/Linux (OpenSSL)</span></span>
<span data-ttu-id="859e0-125">Můžete použít [OpenSSL](http://www.openssl.org/) toocreate svůj certifikát pro správu.</span><span class="sxs-lookup"><span data-stu-id="859e0-125">You can use [OpenSSL](http://www.openssl.org/) toocreate your management certificate.</span></span>  <span data-ttu-id="859e0-126">Skutečně potřebujete dva certifikáty toocreate, jeden pro hello server ( `.cer` souboru) a jeden pro klienta hello ( `.pem` souboru).</span><span class="sxs-lookup"><span data-stu-id="859e0-126">You actually need toocreate two certificates, one for hello server (a `.cer` file) and one for hello client (a `.pem` file).</span></span> <span data-ttu-id="859e0-127">toocreate hello `.pem` soubor, spustit:</span><span class="sxs-lookup"><span data-stu-id="859e0-127">toocreate hello `.pem` file, execute:</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="859e0-128">toocreate hello `.cer` certifikátů, spustit:</span><span class="sxs-lookup"><span data-stu-id="859e0-128">toocreate hello `.cer` certificate, execute:</span></span>

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

<span data-ttu-id="859e0-129">Další informace o Azure certifikáty najdete v tématu [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="859e0-129">For more information about Azure certificates, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span> <span data-ttu-id="859e0-130">Úplný popis parametrů OpenSSL, naleznete v dokumentaci k hello v [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span><span class="sxs-lookup"><span data-stu-id="859e0-130">For a complete description of OpenSSL parameters, see hello documentation at [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span></span>

<span data-ttu-id="859e0-131">Po vytvoření těchto souborů, je nutné tooupload hello `.cer` souboru tooAzure prostřednictvím hello akce "Odeslat" hello "Nastavení" karty hello [portál Azure classic][management-portal], a poznamenejte si toomake potřebujete kam jste uložili hello `.pem` souboru.</span><span class="sxs-lookup"><span data-stu-id="859e0-131">After you have created these files, you need tooupload hello `.cer` file tooAzure via hello "Upload" action of hello "Settings" tab of hello [Azure classic portal][management-portal], and you need toomake note of where you saved hello `.pem` file.</span></span>

<span data-ttu-id="859e0-132">Po získání svoje ID předplatného, vytvořen certifikát a odesláno hello `.cer` tooAzure souboru můžete připojit koncový bod Azure správy toohello předáním hello id předplatného a hello cesta toohello `.pem` souboru příliš**ServiceManagementService**:</span><span class="sxs-lookup"><span data-stu-id="859e0-132">After you have obtained your subscription ID, created a certificate, and uploaded hello `.cer` file tooAzure, you can connect toohello Azure management endpoint by passing hello subscription id and hello path toohello `.pem` file too**ServiceManagementService**:</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="859e0-133">V předchozím příkladu hello `sms` je **ServiceManagementService** objektu.</span><span class="sxs-lookup"><span data-stu-id="859e0-133">In hello preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="859e0-134">Hello **ServiceManagementService** třída je hello primární třída používaná toomanage služby Azure.</span><span class="sxs-lookup"><span data-stu-id="859e0-134">hello **ServiceManagementService** class is hello primary class used toomanage Azure services.</span></span>

### <a name="management-certificates-on-windows-makecert"></a><span data-ttu-id="859e0-135">Certifikáty pro správu v systému Windows (MakeCert)</span><span class="sxs-lookup"><span data-stu-id="859e0-135">Management certificates on Windows (MakeCert)</span></span>
<span data-ttu-id="859e0-136">Můžete vytvořit certifikát podepsaný svým držitelem správy na váš počítač pomocí `makecert.exe`.</span><span class="sxs-lookup"><span data-stu-id="859e0-136">You can create a self-signed management certificate on your machine using `makecert.exe`.</span></span>  <span data-ttu-id="859e0-137">Otevřete **příkazového řádku Visual Studia** jako **správce** a použijte následující příkaz, nahraďte hello *AzureCertificate* s názvem certifikátu hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="859e0-137">Open a **Visual Studio command prompt** as an **administrator** and use hello following command, replacing *AzureCertificate* with hello certificate name you would like toouse.</span></span>

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

<span data-ttu-id="859e0-138">příkaz Hello vytvoří hello `.cer` souboru a nainstaluje v hello **osobní** úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="859e0-138">hello command creates hello `.cer` file, and installs it in hello **Personal** certificate store.</span></span> <span data-ttu-id="859e0-139">Další informace najdete v tématu [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="859e0-139">For more information, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span>

<span data-ttu-id="859e0-140">Po vytvoření hello certifikát, je nutné tooupload hello `.cer` souboru tooAzure prostřednictvím hello akce "Odeslat" hello "Nastavení" karty hello [portál Azure classic][management-portal].</span><span class="sxs-lookup"><span data-stu-id="859e0-140">After you have created hello certificate, you need tooupload hello `.cer` file tooAzure via hello "Upload" action of hello "Settings" tab of hello [Azure classic portal][management-portal].</span></span>

<span data-ttu-id="859e0-141">Po získání svoje ID předplatného, vytvořen certifikát a odesláno hello `.cer` tooAzure souboru můžete připojit koncový bod Azure správy toohello předáním hello id předplatného a hello umístění certifikátu hello ve vaší **Osobní** úložišti certifikátů příliš**ServiceManagementService** (znovu, nahraďte *AzureCertificate* s názvem hello vašeho certifikátu):</span><span class="sxs-lookup"><span data-stu-id="859e0-141">After you have obtained your subscription ID, created a certificate, and uploaded hello `.cer` file tooAzure, you can connect toohello Azure management endpoint by passing hello subscription id and hello location of hello certificate in your **Personal** certificate store too**ServiceManagementService** (again, replace *AzureCertificate* with hello name of your certificate):</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="859e0-142">V předchozím příkladu hello `sms` je **ServiceManagementService** objektu.</span><span class="sxs-lookup"><span data-stu-id="859e0-142">In hello preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="859e0-143">Hello **ServiceManagementService** třída je hello primární třída používaná toomanage služby Azure.</span><span class="sxs-lookup"><span data-stu-id="859e0-143">hello **ServiceManagementService** class is hello primary class used toomanage Azure services.</span></span>

## <span data-ttu-id="859e0-144"><a name="ListAvailableLocations"></a>Postupy: seznam dostupných umístění</span><span class="sxs-lookup"><span data-stu-id="859e0-144"><a name="ListAvailableLocations"> </a>How to: List available locations</span></span>
<span data-ttu-id="859e0-145">toolist hello umístění, které jsou k dispozici pro hostování služeb, použijte hello **seznamu\_umístění** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-145">toolist hello locations that are available for hosting services, use hello **list\_locations** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

<span data-ttu-id="859e0-146">Při vytváření cloudové služby nebo služba úložiště musíte tooprovide platné umístění.</span><span class="sxs-lookup"><span data-stu-id="859e0-146">When you create a cloud service or storage service you need tooprovide a valid location.</span></span> <span data-ttu-id="859e0-147">Hello **seznamu\_umístění** metoda vždy vrátí hodnotu aktuální seznam aktuálně dostupných umístění hello.</span><span class="sxs-lookup"><span data-stu-id="859e0-147">hello **list\_locations** method always returns an up-to-date list of hello currently available locations.</span></span> <span data-ttu-id="859e0-148">Době psaní tohoto textu hello k dispozici umístění jsou:</span><span class="sxs-lookup"><span data-stu-id="859e0-148">As of this writing, hello available locations are:</span></span>

* <span data-ttu-id="859e0-149">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="859e0-149">West Europe</span></span>
* <span data-ttu-id="859e0-150">Severní Evropa</span><span class="sxs-lookup"><span data-stu-id="859e0-150">North Europe</span></span>
* <span data-ttu-id="859e0-151">Jihovýchodní Asie</span><span class="sxs-lookup"><span data-stu-id="859e0-151">Southeast Asia</span></span>
* <span data-ttu-id="859e0-152">Východní Asie</span><span class="sxs-lookup"><span data-stu-id="859e0-152">East Asia</span></span>
* <span data-ttu-id="859e0-153">Střed USA</span><span class="sxs-lookup"><span data-stu-id="859e0-153">Central US</span></span>
* <span data-ttu-id="859e0-154">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="859e0-154">North Central US</span></span>
* <span data-ttu-id="859e0-155">Střed USA – jih</span><span class="sxs-lookup"><span data-stu-id="859e0-155">South Central US</span></span>
* <span data-ttu-id="859e0-156">Západní USA</span><span class="sxs-lookup"><span data-stu-id="859e0-156">West US</span></span>
* <span data-ttu-id="859e0-157">Východ USA</span><span class="sxs-lookup"><span data-stu-id="859e0-157">East US</span></span>
* <span data-ttu-id="859e0-158">Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="859e0-158">Japan East</span></span>
* <span data-ttu-id="859e0-159">Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="859e0-159">Japan West</span></span>
* <span data-ttu-id="859e0-160">Brazílie – jih</span><span class="sxs-lookup"><span data-stu-id="859e0-160">Brazil South</span></span>
* <span data-ttu-id="859e0-161">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="859e0-161">Australia East</span></span>
* <span data-ttu-id="859e0-162">Austrálie – jihovýchod</span><span class="sxs-lookup"><span data-stu-id="859e0-162">Australia Southeast</span></span>

## <span data-ttu-id="859e0-163"><a name="CreateCloudService"></a>Postupy: vytvoření cloudové služby</span><span class="sxs-lookup"><span data-stu-id="859e0-163"><a name="CreateCloudService"> </a>How to: Create a cloud service</span></span>
<span data-ttu-id="859e0-164">Když vytvoříte aplikaci a spustíte ho v Azure, hello kódu a konfigurace společně se nazývají Azure [Cloudová služba] [ cloud service] (označované jako *hostovaná služba* v dříve Uvolní Azure).</span><span class="sxs-lookup"><span data-stu-id="859e0-164">When you create an application and run it in Azure, hello code and configuration together are called an Azure [cloud service][cloud service] (known as a *hosted service* in earlier Azure releases).</span></span> <span data-ttu-id="859e0-165">Hello **vytvořit\_hostované\_služby** metoda vám umožní toocreate novou hostovanou službu tím, že název hostované služby (které musí být jedinečné v Azure), poskytuje popisek (automaticky kódovaného toobase64), Popis a umístění.</span><span class="sxs-lookup"><span data-stu-id="859e0-165">hello **create\_hosted\_service** method allows you toocreate a new hosted service by providing a hosted service name (which must be unique in Azure), a label (automatically encoded toobase64), a description, and a location.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

<span data-ttu-id="859e0-166">Můžete vytvořit seznam všech hello hostovaných služeb pro vaše předplatné s hello **seznamu\_hostované\_služby** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-166">You can list all hello hosted services for your subscription with hello **list\_hosted\_services** method:</span></span>

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

<span data-ttu-id="859e0-167">Pokud chcete tooget informace o konkrétní hostovanou službu, můžete tak učinit pomocí předání hello hostované služby název toohello **získat\_hostované\_služby\_vlastnosti** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-167">If you want tooget information about a particular hosted service, you can do so by passing hello hosted service name toohello **get\_hosted\_service\_properties** method:</span></span>

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

<span data-ttu-id="859e0-168">Po vytvoření cloudové služby, můžete nasadit služby toohello kód prostřednictvím hello **vytvořit\_nasazení** metoda.</span><span class="sxs-lookup"><span data-stu-id="859e0-168">After you have created a cloud service, you can deploy your code toohello service with hello **create\_deployment** method.</span></span>

## <span data-ttu-id="859e0-169"><a name="DeleteCloudService"></a>Postupy: odstranění cloudové služby</span><span class="sxs-lookup"><span data-stu-id="859e0-169"><a name="DeleteCloudService"> </a>How to: Delete a cloud service</span></span>
<span data-ttu-id="859e0-170">Cloudové služby můžete odstranit předáním hello služby název toohello **odstranit\_hostované\_služby** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-170">You can delete a cloud service by passing hello service name toohello **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service('myhostedservice')

<span data-ttu-id="859e0-171">Před odstraněním služby, musíte nejprve odstranit všechna nasazení služby hello.</span><span class="sxs-lookup"><span data-stu-id="859e0-171">Before you can delete a service, all deployments for hello service must first be deleted.</span></span> <span data-ttu-id="859e0-172">(Viz [postupy: odstranění nasazení](#DeleteDeployment) podrobnosti.)</span><span class="sxs-lookup"><span data-stu-id="859e0-172">(See [How to: Delete a deployment](#DeleteDeployment) for details.)</span></span>

## <span data-ttu-id="859e0-173"><a name="DeleteDeployment"></a>Postupy: odstranění nasazení</span><span class="sxs-lookup"><span data-stu-id="859e0-173"><a name="DeleteDeployment"> </a>How to: Delete a deployment</span></span>
<span data-ttu-id="859e0-174">toodelete nasazení, použijte hello **odstranit\_nasazení** metoda.</span><span class="sxs-lookup"><span data-stu-id="859e0-174">toodelete a deployment, use hello **delete\_deployment** method.</span></span> <span data-ttu-id="859e0-175">Hello následující příklad ukazuje, jak toodelete nasazení s názvem `v1`.</span><span class="sxs-lookup"><span data-stu-id="859e0-175">hello following example shows how toodelete a deployment named `v1`.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <span data-ttu-id="859e0-176"><a name="CreateStorageService"></a>Postupy: vytvoření služby úložiště</span><span class="sxs-lookup"><span data-stu-id="859e0-176"><a name="CreateStorageService"> </a>How to: Create a storage service</span></span>
<span data-ttu-id="859e0-177">A [služba úložiště](../storage/common/storage-create-storage-account.md) dává vám přístup tooAzure [objekty BLOB](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabulky](../cosmos-db/table-storage-how-to-use-python.md), a [fronty](../storage/queues/storage-python-how-to-use-queue-storage.md).</span><span class="sxs-lookup"><span data-stu-id="859e0-177">A [storage service](../storage/common/storage-create-storage-account.md) gives you access tooAzure [Blobs](../storage/blobs/storage-python-how-to-use-blob-storage.md), [Tables](../cosmos-db/table-storage-how-to-use-python.md), and [Queues](../storage/queues/storage-python-how-to-use-queue-storage.md).</span></span> <span data-ttu-id="859e0-178">toocreate služba úložiště, musíte jako název služby hello (mezi 3 a 24 malých písmen a jedinečný v rámci Azure), popis, popisek (až too100 znaky, automaticky kódovaného toobase64) a umístění.</span><span class="sxs-lookup"><span data-stu-id="859e0-178">toocreate a storage service, you need a name for hello service (between 3 and 24 lowercase characters and unique within Azure), a description, a label (up too100 characters, automatically encoded toobase64), and a location.</span></span> <span data-ttu-id="859e0-179">Hello následující příklad ukazuje, jak služba toocreate úložiště tak, že zadáte umístění.</span><span class="sxs-lookup"><span data-stu-id="859e0-179">hello following example shows how toocreate a storage service by specifying a location.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

<span data-ttu-id="859e0-180">Poznámka: v předchozím příkladu hello hello stav hello **vytvořit\_úložiště\_účet** operaci se dá načíst pomocí předání hello výsledek vrácený **vytvořit\_úložiště \_účet** toohello **získat\_operace\_stav** metoda.</span><span class="sxs-lookup"><span data-stu-id="859e0-180">Note in hello preceding example that hello status of hello **create\_storage\_account** operation can be retrieved by passing hello result returned by **create\_storage\_account** toohello **get\_operation\_status** method.</span></span>  

<span data-ttu-id="859e0-181">Můžete vytvořit seznam účtů úložiště a jejich vlastnosti s hello **seznamu\_úložiště\_účty** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-181">You can list your storage accounts and their properties with hello **list\_storage\_accounts** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <span data-ttu-id="859e0-182"><a name="DeleteStorageService"></a>Postupy: odstranění služby úložiště</span><span class="sxs-lookup"><span data-stu-id="859e0-182"><a name="DeleteStorageService"> </a>How to: Delete a storage service</span></span>
<span data-ttu-id="859e0-183">Služba úložiště můžete odstranit předáním hello úložiště služby název toohello **odstranit\_úložiště\_účet** metoda.</span><span class="sxs-lookup"><span data-stu-id="859e0-183">You can delete a storage service by passing hello storage service name toohello **delete\_storage\_account** method.</span></span> <span data-ttu-id="859e0-184">Odstraněním úložiště služby se odstraní všechna data uložená ve službě hello (objekty BLOB, tabulek a front).</span><span class="sxs-lookup"><span data-stu-id="859e0-184">Deleting a storage service deletes all data stored in hello service (blobs, tables, and queues).</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <span data-ttu-id="859e0-185"><a name="ListOperatingSystems"></a>Postupy: seznam dostupných operačních systémů</span><span class="sxs-lookup"><span data-stu-id="859e0-185"><a name="ListOperatingSystems"> </a>How to: List available operating systems</span></span>
<span data-ttu-id="859e0-186">toolist hello operační systémy, které jsou k dispozici pro hostování služeb, použijte hello **seznamu\_operační\_systémy** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-186">toolist hello operating systems that are available for hosting services, use hello **list\_operating\_systems** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

<span data-ttu-id="859e0-187">Alternativně můžete použít hello **seznamu\_operační\_systému\_rodiny** metodu, která skupiny hello operační systémy řady:</span><span class="sxs-lookup"><span data-stu-id="859e0-187">Alternatively, you can use hello **list\_operating\_system\_families** method, which groups hello operating systems by family:</span></span>

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <span data-ttu-id="859e0-188"><a name="CreateVMImage"></a>Postupy: vytvoření image operačního systému</span><span class="sxs-lookup"><span data-stu-id="859e0-188"><a name="CreateVMImage"> </a>How to: Create an operating system image</span></span>
<span data-ttu-id="859e0-189">tooadd úložiště bitové kopie toohello bitové kopie operačního systému použít hello **přidat\_os\_image** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-189">tooadd an operating system image toohello image repository, use hello **add\_os\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

<span data-ttu-id="859e0-190">Image operačního systému hello toolist, které jsou k dispozici, použijte hello **seznamu\_os\_bitové kopie** metoda.</span><span class="sxs-lookup"><span data-stu-id="859e0-190">toolist hello operating system images that are available, use hello **list\_os\_images** method.</span></span> <span data-ttu-id="859e0-191">Obsahuje všechny Image platformy a uživatele bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="859e0-191">It includes all platform images and user images:</span></span>

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <span data-ttu-id="859e0-192"><a name="DeleteVMImage"></a>Postupy: odstranění image operačního systému</span><span class="sxs-lookup"><span data-stu-id="859e0-192"><a name="DeleteVMImage"> </a>How to: Delete an operating system image</span></span>
<span data-ttu-id="859e0-193">toodelete uživatelská image použít hello **odstranit\_os\_image** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-193">toodelete a user image, use hello **delete\_os\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <span data-ttu-id="859e0-194"><a name="CreateVM"></a>Postupy: vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="859e0-194"><a name="CreateVM"> </a>How to: Create a virtual machine</span></span>
<span data-ttu-id="859e0-195">toocreate virtuálního počítače, musíte nejprve toocreate [Cloudová služba](#CreateCloudService).</span><span class="sxs-lookup"><span data-stu-id="859e0-195">toocreate a virtual machine, you first need toocreate a [cloud service](#CreateCloudService).</span></span>  <span data-ttu-id="859e0-196">Pak vytvořte hello nasazení virtuálního počítače pomocí hello **vytvořit\_virtuální\_počítač\_nasazení** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-196">Then create hello virtual machine deployment using hello **create\_virtual\_machine\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set hello location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where hello VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <span data-ttu-id="859e0-197"><a name="DeleteVM"></a>Postupy: odstranění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="859e0-197"><a name="DeleteVM"> </a>How to: Delete a virtual machine</span></span>
<span data-ttu-id="859e0-198">toodelete virtuálního počítače, je nejprve odstranit hello nasazení pomocí hello **odstranit\_nasazení** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-198">toodelete a virtual machine, you first delete hello deployment using hello **delete\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

<span data-ttu-id="859e0-199">Hello cloudovou službu můžete odstranit pak pomocí hello **odstranit\_hostované\_služby** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-199">hello cloud service can then be deleted using hello **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a><span data-ttu-id="859e0-200">Postupy: Vytvoření virtuálního počítače z Image zaznamenané virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="859e0-200">How To: Create a Virtual Machine from a Captured Virtual Machine Image</span></span>
<span data-ttu-id="859e0-201">toocapture image virtuálního počítače, první volání hello **zaznamenat\_virtuálních počítačů\_image** metoda:</span><span class="sxs-lookup"><span data-stu-id="859e0-201">toocapture a VM image, you first call hello **capture\_vm\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace hello below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

<span data-ttu-id="859e0-202">Dále toomake jistotu, že jste se úspěšně zachytil hello image, použijte hello **seznamu\_virtuálních počítačů\_bitové kopie** rozhraní api a zajistěte, aby bitové kopie se zobrazí ve výsledcích hello:</span><span class="sxs-lookup"><span data-stu-id="859e0-202">Next, toomake sure that you have successfully captured hello image, use hello **list\_vm\_images** api, and make sure your image is displayed in hello results:</span></span>

    images = sms.list_vm_images()

<span data-ttu-id="859e0-203">toofinally vytvořit hello virtuální počítač pomocí hello zaznamenané bitové kopie, použijte hello **vytvořit\_virtuální\_počítač\_nasazení** jako předtím, ale tentokrát předat hello vm_image_name místo toho – metoda</span><span class="sxs-lookup"><span data-stu-id="859e0-203">toofinally create hello virtual machine using hello captured image, use hello **create\_virtual\_machine\_deployment** method as before, but this time pass in hello vm_image_name instead</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set hello location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

<span data-ttu-id="859e0-204">Další informace o toolearn, jak zjistit, toocapture virtuální počítač s Linuxem [jak tooCapture virtuální počítač s Linuxem.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="859e0-204">toolearn more about how toocapture a Linux Virtual Machine, see [How tooCapture a Linux Virtual Machine.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="859e0-205">toolearn Další informace o tom, jak toocapture virtuálního počítače s Windows, najdete v části [jak tooCapture virtuálního počítače s Windows.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="859e0-205">toolearn more about how toocapture a Windows Virtual Machine, see [How tooCapture a Windows Virtual Machine.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

## <span data-ttu-id="859e0-206"><a name="What's Next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="859e0-206"><a name="What's Next"> </a>Next Steps</span></span>
<span data-ttu-id="859e0-207">Teď, když jste se naučili základy hello služby správy, dostanete hello [referenční dokumentace rozhraní API dokončení pro hello Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) a provádět komplexní úlohy snadno toomanage aplikace python.</span><span class="sxs-lookup"><span data-stu-id="859e0-207">Now that you've learned hello basics of service management, you can access hello [Complete API reference documentation for hello Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) and perform complex tasks easily toomanage your python application.</span></span>

<span data-ttu-id="859e0-208">Další informace najdete v tématu hello [středisku pro vývojáře Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="859e0-208">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect tooservice management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/services/cloud-services/
