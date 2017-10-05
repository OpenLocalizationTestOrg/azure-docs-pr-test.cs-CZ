---
title: "Jak používat rozhraní API (Python) – Průvodce funkcemi pro správu služby"
description: "Zjistěte, jak programově provádět běžné úlohy správy služby z Pythonu."
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
ms.openlocfilehash: 13249ba9a4b317a3154776b411ce0bb1f316b3bb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-service-management-from-python"></a><span data-ttu-id="fbd18-103">Jak používat správu služby z Pythonu</span><span class="sxs-lookup"><span data-stu-id="fbd18-103">How to use Service Management from Python</span></span>
<span data-ttu-id="fbd18-104">Tento průvodce vám ukáže, jak programově provádět běžné úlohy správy služby z Pythonu.</span><span class="sxs-lookup"><span data-stu-id="fbd18-104">This guide shows you how to programmatically perform common service management tasks from Python.</span></span> <span data-ttu-id="fbd18-105">**ServiceManagementService** třídy v [Azure SDK pro jazyk Python](https://github.com/Azure/azure-sdk-for-python) podporuje programový přístup k mnohem týkajících se správy funkcí služby, který je k dispozici v [Azure portál Classic] [ management-portal] (například **vytváření, aktualizaci a odstraňování cloudové služby, nasazení, služby pro správu dat a virtuální počítače**).</span><span class="sxs-lookup"><span data-stu-id="fbd18-105">The **ServiceManagementService** class in the [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python) supports programmatic access to much of the service management-related functionality that is available in the [Azure classic portal][management-portal] (such as **creating, updating, and deleting cloud services, deployments, data management services, and virtual machines**).</span></span> <span data-ttu-id="fbd18-106">Tato funkce může být užitečné při vytváření aplikace, které potřebují programový přístup ke správě služby.</span><span class="sxs-lookup"><span data-stu-id="fbd18-106">This functionality can be useful in building applications that need programmatic access to service management.</span></span>

## <span data-ttu-id="fbd18-107"><a name="WhatIs"></a>Co je služba správy</span><span class="sxs-lookup"><span data-stu-id="fbd18-107"><a name="WhatIs"> </a>What is Service Management</span></span>
<span data-ttu-id="fbd18-108">Service Management API zajišťují programový přístup ke většinu funkcí správy služby k dispozici prostřednictvím [portál Azure classic][management-portal].</span><span class="sxs-lookup"><span data-stu-id="fbd18-108">The Service Management API provides programmatic access to much of the service management functionality available through the [Azure classic portal][management-portal].</span></span> <span data-ttu-id="fbd18-109">Sada Azure SDK pro Python umožňuje spravovat cloudové služby a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="fbd18-109">The Azure SDK for Python allows you to manage your cloud services and storage accounts.</span></span>

<span data-ttu-id="fbd18-110">Chcete-li použít rozhraní API pro správu služby, je potřeba [vytvoření účtu Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fbd18-110">To use the Service Management API, you need to [create an Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <span data-ttu-id="fbd18-111"><a name="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="fbd18-111"><a name="Concepts"> </a>Concepts</span></span>
<span data-ttu-id="fbd18-112">Sada Azure SDK pro Python zabalí [Azure Service Management API][svc-mgmt-rest-api], což je rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="fbd18-112">The Azure SDK for Python wraps the [Azure Service Management API][svc-mgmt-rest-api], which is a REST API.</span></span> <span data-ttu-id="fbd18-113">Všechny operace rozhraní API se provádí přes SSL a vzájemně se ověřují pomocí certifikátů X.509 v3.</span><span class="sxs-lookup"><span data-stu-id="fbd18-113">All API operations are performed over SSL and mutually authenticated using X.509 v3 certificates.</span></span> <span data-ttu-id="fbd18-114">Ke službě pro správu se dá přistupovat ze služby spuštěné v Azure nebo přímo přes internet z jakékoliv aplikace, která umí poslat požadavek HTTPS a přijmout odpověď HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fbd18-114">The management service may be accessed from within a service running in Azure, or directly over the Internet from any application that can send an HTTPS request and receive an HTTPS response.</span></span>

## <span data-ttu-id="fbd18-115"><a name="Installation"></a>Instalace</span><span class="sxs-lookup"><span data-stu-id="fbd18-115"><a name="Installation"> </a>Installation</span></span>
<span data-ttu-id="fbd18-116">Všechny funkce, které jsou popsané v tomto článku jsou k dispozici v `azure-servicemanagement-legacy` balíčku, které můžete nainstalovat pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="fbd18-116">All the features described in this article are available in the `azure-servicemanagement-legacy` package, which you can install using pip.</span></span> <span data-ttu-id="fbd18-117">Další informace o instalaci (například pokud jste novým uživatelem Python), najdete v tomto článku: [instalaci Python a sady Azure SDK](../python-how-to-install.md)</span><span class="sxs-lookup"><span data-stu-id="fbd18-117">For more information about installation (for example, if you are new to Python), see this article: [Installing Python and the Azure SDK](../python-how-to-install.md)</span></span>

## <span data-ttu-id="fbd18-118"><a name="Connect"></a>Postupy: připojení služby správy</span><span class="sxs-lookup"><span data-stu-id="fbd18-118"><a name="Connect"> </a>How to: Connect to service management</span></span>
<span data-ttu-id="fbd18-119">Pro připojení ke koncovému bodu služby správy, musíte svoje ID předplatného Azure a certifikát pro správu platné.</span><span class="sxs-lookup"><span data-stu-id="fbd18-119">To connect to the Service Management endpoint, you need your Azure subscription ID and a valid management certificate.</span></span> <span data-ttu-id="fbd18-120">Můžete získat svoje ID předplatného prostřednictvím [portál Azure classic][management-portal].</span><span class="sxs-lookup"><span data-stu-id="fbd18-120">You can obtain your subscription ID through the [Azure classic portal][management-portal].</span></span>

> [!NOTE]
> <span data-ttu-id="fbd18-121">Nyní je možné použít certifikáty vytvořené pomocí OpenSSL spuštěná v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="fbd18-121">It is now possible to use certificates created with OpenSSL when running on Windows.</span></span>  <span data-ttu-id="fbd18-122">Vyžaduje Python 2.7.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fbd18-122">It requires Python 2.7.4 or later.</span></span> <span data-ttu-id="fbd18-123">Doporučujeme, abyste uživatelům používat OpenSSL místo .pfx, protože podpora pro .pfx, certifikáty budou odebrány pravděpodobně v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="fbd18-123">We recommend users to use OpenSSL instead of .pfx, since support for .pfx certificates will likely be removed in the future.</span></span>
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a><span data-ttu-id="fbd18-124">Certifikáty pro správu v systému Windows nebo Mac/Linux (OpenSSL)</span><span class="sxs-lookup"><span data-stu-id="fbd18-124">Management certificates on Windows/Mac/Linux (OpenSSL)</span></span>
<span data-ttu-id="fbd18-125">Můžete použít [OpenSSL](http://www.openssl.org/) vytvořit svůj certifikát pro správu.</span><span class="sxs-lookup"><span data-stu-id="fbd18-125">You can use [OpenSSL](http://www.openssl.org/) to create your management certificate.</span></span>  <span data-ttu-id="fbd18-126">Ve skutečnosti je nutné vytvořit dva certifikáty, jeden pro server ( `.cer` souboru) a jeden pro klienta ( `.pem` souboru).</span><span class="sxs-lookup"><span data-stu-id="fbd18-126">You actually need to create two certificates, one for the server (a `.cer` file) and one for the client (a `.pem` file).</span></span> <span data-ttu-id="fbd18-127">Chcete-li vytvořit `.pem` soubor, spustit:</span><span class="sxs-lookup"><span data-stu-id="fbd18-127">To create the `.pem` file, execute:</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="fbd18-128">Chcete-li vytvořit `.cer` certifikátů, spustit:</span><span class="sxs-lookup"><span data-stu-id="fbd18-128">To create the `.cer` certificate, execute:</span></span>

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

<span data-ttu-id="fbd18-129">Další informace o Azure certifikáty najdete v tématu [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="fbd18-129">For more information about Azure certificates, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span> <span data-ttu-id="fbd18-130">Úplný popis parametrů OpenSSL, naleznete v dokumentaci na [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span><span class="sxs-lookup"><span data-stu-id="fbd18-130">For a complete description of OpenSSL parameters, see the documentation at [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span></span>

<span data-ttu-id="fbd18-131">Po vytvoření těchto souborů, budete muset nahrát `.cer` souboru k Azure přes "Nahrávání" akce "Nastavení" karty [portál Azure classic][management-portal], a budete muset poznamenejte kde jste Uložit `.pem` souboru.</span><span class="sxs-lookup"><span data-stu-id="fbd18-131">After you have created these files, you need to upload the `.cer` file to Azure via the "Upload" action of the "Settings" tab of the [Azure classic portal][management-portal], and you need to make note of where you saved the `.pem` file.</span></span>

<span data-ttu-id="fbd18-132">Po získání svoje ID předplatného, vytvořit certifikát a nahrán `.cer` souboru do Azure, se můžete připojit ke koncovému bodu správy Azure pomocí id předplatného a cestu k předání `.pem` do souboru  **ServiceManagementService**:</span><span class="sxs-lookup"><span data-stu-id="fbd18-132">After you have obtained your subscription ID, created a certificate, and uploaded the `.cer` file to Azure, you can connect to the Azure management endpoint by passing the subscription id and the path to the `.pem` file to **ServiceManagementService**:</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="fbd18-133">V předchozím příkladu `sms` je **ServiceManagementService** objektu.</span><span class="sxs-lookup"><span data-stu-id="fbd18-133">In the preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="fbd18-134">**ServiceManagementService** třída je primární třída, která slouží ke správě služby Azure.</span><span class="sxs-lookup"><span data-stu-id="fbd18-134">The **ServiceManagementService** class is the primary class used to manage Azure services.</span></span>

### <a name="management-certificates-on-windows-makecert"></a><span data-ttu-id="fbd18-135">Certifikáty pro správu v systému Windows (MakeCert)</span><span class="sxs-lookup"><span data-stu-id="fbd18-135">Management certificates on Windows (MakeCert)</span></span>
<span data-ttu-id="fbd18-136">Můžete vytvořit certifikát podepsaný svým držitelem správy na váš počítač pomocí `makecert.exe`.</span><span class="sxs-lookup"><span data-stu-id="fbd18-136">You can create a self-signed management certificate on your machine using `makecert.exe`.</span></span>  <span data-ttu-id="fbd18-137">Otevřete **příkazového řádku Visual Studia** jako **správce** a použijte následující příkaz, nahraďte *AzureCertificate* se název certifikátu, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="fbd18-137">Open a **Visual Studio command prompt** as an **administrator** and use the following command, replacing *AzureCertificate* with the certificate name you would like to use.</span></span>

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

<span data-ttu-id="fbd18-138">Příkaz vytvoří `.cer` souboru a nainstaluje do **osobní** úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="fbd18-138">The command creates the `.cer` file, and installs it in the **Personal** certificate store.</span></span> <span data-ttu-id="fbd18-139">Další informace najdete v tématu [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="fbd18-139">For more information, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span>

<span data-ttu-id="fbd18-140">Po vytvoření certifikátu, budete muset nahrát `.cer` souboru k Azure přes "Nahrávání" akce "Nastavení" karty [portál Azure classic][management-portal].</span><span class="sxs-lookup"><span data-stu-id="fbd18-140">After you have created the certificate, you need to upload the `.cer` file to Azure via the "Upload" action of the "Settings" tab of the [Azure classic portal][management-portal].</span></span>

<span data-ttu-id="fbd18-141">Po získání svoje ID předplatného, vytvořit certifikát a nahrán `.cer` souboru do Azure, se můžete připojit ke koncovému bodu správy Azure pomocí předáním id předplatného a umístění certifikátu v vaší **osobní**  úložišti certifikátů **ServiceManagementService** (znovu, nahraďte *AzureCertificate* s názvem vašeho certifikátu):</span><span class="sxs-lookup"><span data-stu-id="fbd18-141">After you have obtained your subscription ID, created a certificate, and uploaded the `.cer` file to Azure, you can connect to the Azure management endpoint by passing the subscription id and the location of the certificate in your **Personal** certificate store to **ServiceManagementService** (again, replace *AzureCertificate* with the name of your certificate):</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="fbd18-142">V předchozím příkladu `sms` je **ServiceManagementService** objektu.</span><span class="sxs-lookup"><span data-stu-id="fbd18-142">In the preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="fbd18-143">**ServiceManagementService** třída je primární třída, která slouží ke správě služby Azure.</span><span class="sxs-lookup"><span data-stu-id="fbd18-143">The **ServiceManagementService** class is the primary class used to manage Azure services.</span></span>

## <span data-ttu-id="fbd18-144"><a name="ListAvailableLocations"></a>Postupy: seznam dostupných umístění</span><span class="sxs-lookup"><span data-stu-id="fbd18-144"><a name="ListAvailableLocations"> </a>How to: List available locations</span></span>
<span data-ttu-id="fbd18-145">K zobrazení seznamu umístění, které jsou k dispozici pro hostování služeb, použijte **seznamu\_umístění** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-145">To list the locations that are available for hosting services, use the **list\_locations** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

<span data-ttu-id="fbd18-146">Při vytváření cloudové služby nebo služba úložiště, musíte zadat platné umístění.</span><span class="sxs-lookup"><span data-stu-id="fbd18-146">When you create a cloud service or storage service you need to provide a valid location.</span></span> <span data-ttu-id="fbd18-147">**Seznamu\_umístění** metoda vždy vrátí hodnotu aktuální seznam aktuálně dostupných umístění.</span><span class="sxs-lookup"><span data-stu-id="fbd18-147">The **list\_locations** method always returns an up-to-date list of the currently available locations.</span></span> <span data-ttu-id="fbd18-148">Době psaní tohoto textu, jsou k dispozici umístění:</span><span class="sxs-lookup"><span data-stu-id="fbd18-148">As of this writing, the available locations are:</span></span>

* <span data-ttu-id="fbd18-149">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="fbd18-149">West Europe</span></span>
* <span data-ttu-id="fbd18-150">Severní Evropa</span><span class="sxs-lookup"><span data-stu-id="fbd18-150">North Europe</span></span>
* <span data-ttu-id="fbd18-151">Jihovýchodní Asie</span><span class="sxs-lookup"><span data-stu-id="fbd18-151">Southeast Asia</span></span>
* <span data-ttu-id="fbd18-152">Východní Asie</span><span class="sxs-lookup"><span data-stu-id="fbd18-152">East Asia</span></span>
* <span data-ttu-id="fbd18-153">Střed USA</span><span class="sxs-lookup"><span data-stu-id="fbd18-153">Central US</span></span>
* <span data-ttu-id="fbd18-154">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="fbd18-154">North Central US</span></span>
* <span data-ttu-id="fbd18-155">Střed USA – jih</span><span class="sxs-lookup"><span data-stu-id="fbd18-155">South Central US</span></span>
* <span data-ttu-id="fbd18-156">Západní USA</span><span class="sxs-lookup"><span data-stu-id="fbd18-156">West US</span></span>
* <span data-ttu-id="fbd18-157">Východ USA</span><span class="sxs-lookup"><span data-stu-id="fbd18-157">East US</span></span>
* <span data-ttu-id="fbd18-158">Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="fbd18-158">Japan East</span></span>
* <span data-ttu-id="fbd18-159">Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="fbd18-159">Japan West</span></span>
* <span data-ttu-id="fbd18-160">Brazílie – jih</span><span class="sxs-lookup"><span data-stu-id="fbd18-160">Brazil South</span></span>
* <span data-ttu-id="fbd18-161">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="fbd18-161">Australia East</span></span>
* <span data-ttu-id="fbd18-162">Austrálie – jihovýchod</span><span class="sxs-lookup"><span data-stu-id="fbd18-162">Australia Southeast</span></span>

## <span data-ttu-id="fbd18-163"><a name="CreateCloudService"></a>Postupy: vytvoření cloudové služby</span><span class="sxs-lookup"><span data-stu-id="fbd18-163"><a name="CreateCloudService"> </a>How to: Create a cloud service</span></span>
<span data-ttu-id="fbd18-164">Když vytvoříte aplikaci a spustíte ho v Azure, kód a konfigurace společně se nazývají Azure [Cloudová služba] [ cloud service] (označované jako *hostovaná služba* v dřívější Azure uvolní).</span><span class="sxs-lookup"><span data-stu-id="fbd18-164">When you create an application and run it in Azure, the code and configuration together are called an Azure [cloud service][cloud service] (known as a *hosted service* in earlier Azure releases).</span></span> <span data-ttu-id="fbd18-165">**Vytvořit\_hostované\_služby** metoda vám umožní vytvořit novou hostovanou službu tím, že poskytuje název hostované služby (které musí být jedinečné v Azure), popisek (automaticky kódovaný formátu Base64), popis, a umístění.</span><span class="sxs-lookup"><span data-stu-id="fbd18-165">The **create\_hosted\_service** method allows you to create a new hosted service by providing a hosted service name (which must be unique in Azure), a label (automatically encoded to base64), a description, and a location.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

<span data-ttu-id="fbd18-166">Můžete vytvořit seznam všechny hostované služby pro vaše předplatné s **seznamu\_hostované\_služby** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-166">You can list all the hosted services for your subscription with the **list\_hosted\_services** method:</span></span>

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

<span data-ttu-id="fbd18-167">Pokud chcete získat informace o konkrétní hostovanou službu, můžete tak učinit pomocí název hostované služby k předání **získat\_hostované\_služby\_vlastnosti** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-167">If you want to get information about a particular hosted service, you can do so by passing the hosted service name to the **get\_hosted\_service\_properties** method:</span></span>

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

<span data-ttu-id="fbd18-168">Po vytvoření cloudové služby, můžete nasadit kódu na služby s **vytvořit\_nasazení** metoda.</span><span class="sxs-lookup"><span data-stu-id="fbd18-168">After you have created a cloud service, you can deploy your code to the service with the **create\_deployment** method.</span></span>

## <span data-ttu-id="fbd18-169"><a name="DeleteCloudService"></a>Postupy: odstranění cloudové služby</span><span class="sxs-lookup"><span data-stu-id="fbd18-169"><a name="DeleteCloudService"> </a>How to: Delete a cloud service</span></span>
<span data-ttu-id="fbd18-170">Cloudové služby můžete odstranit pomocí názvu služby k předání **odstranit\_hostované\_služby** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-170">You can delete a cloud service by passing the service name to the **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service('myhostedservice')

<span data-ttu-id="fbd18-171">Před odstraněním služby, musíte nejprve odstranit všechna nasazení pro službu.</span><span class="sxs-lookup"><span data-stu-id="fbd18-171">Before you can delete a service, all deployments for the service must first be deleted.</span></span> <span data-ttu-id="fbd18-172">(Viz [postupy: odstranění nasazení](#DeleteDeployment) podrobnosti.)</span><span class="sxs-lookup"><span data-stu-id="fbd18-172">(See [How to: Delete a deployment](#DeleteDeployment) for details.)</span></span>

## <span data-ttu-id="fbd18-173"><a name="DeleteDeployment"></a>Postupy: odstranění nasazení</span><span class="sxs-lookup"><span data-stu-id="fbd18-173"><a name="DeleteDeployment"> </a>How to: Delete a deployment</span></span>
<span data-ttu-id="fbd18-174">Pokud chcete odstranit nasazení, použijte **odstranit\_nasazení** metoda.</span><span class="sxs-lookup"><span data-stu-id="fbd18-174">To delete a deployment, use the **delete\_deployment** method.</span></span> <span data-ttu-id="fbd18-175">Následující příklad ukazuje, jak odstranit nasazení s názvem `v1`.</span><span class="sxs-lookup"><span data-stu-id="fbd18-175">The following example shows how to delete a deployment named `v1`.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <span data-ttu-id="fbd18-176"><a name="CreateStorageService"></a>Postupy: vytvoření služby úložiště</span><span class="sxs-lookup"><span data-stu-id="fbd18-176"><a name="CreateStorageService"> </a>How to: Create a storage service</span></span>
<span data-ttu-id="fbd18-177">A [služba úložiště](../storage/common/storage-create-storage-account.md) dává vám přístup k Azure [objekty BLOB](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabulky](../cosmos-db/table-storage-how-to-use-python.md), a [fronty](../storage/queues/storage-python-how-to-use-queue-storage.md).</span><span class="sxs-lookup"><span data-stu-id="fbd18-177">A [storage service](../storage/common/storage-create-storage-account.md) gives you access to Azure [Blobs](../storage/blobs/storage-python-how-to-use-blob-storage.md), [Tables](../cosmos-db/table-storage-how-to-use-python.md), and [Queues](../storage/queues/storage-python-how-to-use-queue-storage.md).</span></span> <span data-ttu-id="fbd18-178">Vytvoření služby, úložiště, je třeba název služby (mezi 3 a 24 malých písmen a jedinečný v rámci Azure), popis, popisek (až 100 znaků, automaticky kódovaný formátu Base64) a umístění.</span><span class="sxs-lookup"><span data-stu-id="fbd18-178">To create a storage service, you need a name for the service (between 3 and 24 lowercase characters and unique within Azure), a description, a label (up to 100 characters, automatically encoded to base64), and a location.</span></span> <span data-ttu-id="fbd18-179">Následující příklad ukazuje, jak vytvořit službu úložiště tak, že zadáte umístění.</span><span class="sxs-lookup"><span data-stu-id="fbd18-179">The following example shows how to create a storage service by specifying a location.</span></span>

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

<span data-ttu-id="fbd18-180">Poznámka: v předchozím příkladu, stav **vytvořit\_úložiště\_účet** operaci se dá načíst pomocí předání výsledek vrácený **vytvořit\_úložiště\_účet** k **získat\_operace\_stav** metoda.</span><span class="sxs-lookup"><span data-stu-id="fbd18-180">Note in the preceding example that the status of the **create\_storage\_account** operation can be retrieved by passing the result returned by **create\_storage\_account** to the **get\_operation\_status** method.</span></span>  

<span data-ttu-id="fbd18-181">Můžete vytvořit seznam účtů úložiště a jejich vlastnosti s **seznamu\_úložiště\_účty** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-181">You can list your storage accounts and their properties with the **list\_storage\_accounts** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <span data-ttu-id="fbd18-182"><a name="DeleteStorageService"></a>Postupy: odstranění služby úložiště</span><span class="sxs-lookup"><span data-stu-id="fbd18-182"><a name="DeleteStorageService"> </a>How to: Delete a storage service</span></span>
<span data-ttu-id="fbd18-183">Služba úložiště můžete odstranit pomocí předání názvu služby úložiště na **odstranit\_úložiště\_účet** metoda.</span><span class="sxs-lookup"><span data-stu-id="fbd18-183">You can delete a storage service by passing the storage service name to the **delete\_storage\_account** method.</span></span> <span data-ttu-id="fbd18-184">Odstraněním úložiště služby se odstraní všechna data uložená ve službě (objekty BLOB, tabulek a front).</span><span class="sxs-lookup"><span data-stu-id="fbd18-184">Deleting a storage service deletes all data stored in the service (blobs, tables, and queues).</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <span data-ttu-id="fbd18-185"><a name="ListOperatingSystems"></a>Postupy: seznam dostupných operačních systémů</span><span class="sxs-lookup"><span data-stu-id="fbd18-185"><a name="ListOperatingSystems"> </a>How to: List available operating systems</span></span>
<span data-ttu-id="fbd18-186">K zobrazení seznamu operačních systémů, které jsou k dispozici pro hostování služeb, použijte **seznamu\_operační\_systémy** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-186">To list the operating systems that are available for hosting services, use the **list\_operating\_systems** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

<span data-ttu-id="fbd18-187">Alternativně můžete použít **seznamu\_operační\_systému\_rodiny** metodu, která skupiny operační systémy řady:</span><span class="sxs-lookup"><span data-stu-id="fbd18-187">Alternatively, you can use the **list\_operating\_system\_families** method, which groups the operating systems by family:</span></span>

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <span data-ttu-id="fbd18-188"><a name="CreateVMImage"></a>Postupy: vytvoření image operačního systému</span><span class="sxs-lookup"><span data-stu-id="fbd18-188"><a name="CreateVMImage"> </a>How to: Create an operating system image</span></span>
<span data-ttu-id="fbd18-189">Chcete-li přidat bitovou kopii operačního systému do úložiště bitové kopie, použijte **přidat\_operačního systému\_bitové kopie** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-189">To add an operating system image to the image repository, use the **add\_os\_image** method:</span></span>

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

<span data-ttu-id="fbd18-190">K zobrazení seznamu bitové kopie operačního systému, které jsou k dispozici, použijte **seznamu\_os\_bitové kopie** metoda.</span><span class="sxs-lookup"><span data-stu-id="fbd18-190">To list the operating system images that are available, use the **list\_os\_images** method.</span></span> <span data-ttu-id="fbd18-191">Obsahuje všechny Image platformy a uživatele bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="fbd18-191">It includes all platform images and user images:</span></span>

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

## <span data-ttu-id="fbd18-192"><a name="DeleteVMImage"></a>Postupy: odstranění image operačního systému</span><span class="sxs-lookup"><span data-stu-id="fbd18-192"><a name="DeleteVMImage"> </a>How to: Delete an operating system image</span></span>
<span data-ttu-id="fbd18-193">Chcete-li odstranit uživatelskou image, použijte **odstranit\_operačního systému\_bitové kopie** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-193">To delete a user image, use the **delete\_os\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <span data-ttu-id="fbd18-194"><a name="CreateVM"></a>Postupy: vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fbd18-194"><a name="CreateVM"> </a>How to: Create a virtual machine</span></span>
<span data-ttu-id="fbd18-195">Pokud chcete vytvořit virtuální počítač, musíte nejprve vytvořit [Cloudová služba](#CreateCloudService).</span><span class="sxs-lookup"><span data-stu-id="fbd18-195">To create a virtual machine, you first need to create a [cloud service](#CreateCloudService).</span></span>  <span data-ttu-id="fbd18-196">Pak vytvořte pomocí nasazení virtuálního počítače **vytvořit\_virtuální\_počítač\_nasazení** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-196">Then create the virtual machine deployment using the **create\_virtual\_machine\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
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

## <span data-ttu-id="fbd18-197"><a name="DeleteVM"></a>Postupy: odstranění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fbd18-197"><a name="DeleteVM"> </a>How to: Delete a virtual machine</span></span>
<span data-ttu-id="fbd18-198">Pokud chcete odstranit virtuální počítač, je nejprve odstranit nasazení pomocí **odstranit\_nasazení** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-198">To delete a virtual machine, you first delete the deployment using the **delete\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

<span data-ttu-id="fbd18-199">Cloudové služby můžete odstranit pak pomocí **odstranit\_hostované\_služby** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-199">The cloud service can then be deleted using the **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a><span data-ttu-id="fbd18-200">Postupy: Vytvoření virtuálního počítače z Image zaznamenané virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fbd18-200">How To: Create a Virtual Machine from a Captured Virtual Machine Image</span></span>
<span data-ttu-id="fbd18-201">K zachycení image virtuálního počítače, první volání **zaznamenat\_virtuálních počítačů\_image** metoda:</span><span class="sxs-lookup"><span data-stu-id="fbd18-201">To capture a VM image, you first call the **capture\_vm\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
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

<span data-ttu-id="fbd18-202">Další, abyste měli jistotu, že úspěšně zaznamenáte bitovou kopii, použijte **seznamu\_virtuálních počítačů\_bitové kopie** rozhraní api a zajistěte, aby bitové kopie se zobrazí ve výsledcích:</span><span class="sxs-lookup"><span data-stu-id="fbd18-202">Next, to make sure that you have successfully captured the image, use the **list\_vm\_images** api, and make sure your image is displayed in the results:</span></span>

    images = sms.list_vm_images()

<span data-ttu-id="fbd18-203">Nakonec vytvoření virtuálního počítače pomocí zaznamenané bitové kopie, použijte **vytvořit\_virtuální\_počítač\_nasazení** jako předtím, ale tentokrát předávat vm_image_name místo toho – metoda</span><span class="sxs-lookup"><span data-stu-id="fbd18-203">To finally create the virtual machine using the captured image, use the **create\_virtual\_machine\_deployment** method as before, but this time pass in the vm_image_name instead</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
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

<span data-ttu-id="fbd18-204">Další informace o tom, jak zachytit virtuální počítač s Linuxem najdete v tématu [jak zachytit virtuální počítač s Linuxem.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="fbd18-204">To learn more about how to capture a Linux Virtual Machine, see [How to Capture a Linux Virtual Machine.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="fbd18-205">Další informace o tom, jak zachytit virtuální počítač Windows najdete v tématu [jak zachytit virtuální počítač Windows.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="fbd18-205">To learn more about how to capture a Windows Virtual Machine, see [How to Capture a Windows Virtual Machine.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

## <span data-ttu-id="fbd18-206"><a name="What's Next"> </a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="fbd18-206"><a name="What's Next"> </a>Next Steps</span></span>
<span data-ttu-id="fbd18-207">Teď, když jste se naučili základy používání služby Správa služby, dostanete [referenční dokumentace dokončení rozhraní API pro Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) a provádět komplexní úlohy snadno ke správě aplikace python.</span><span class="sxs-lookup"><span data-stu-id="fbd18-207">Now that you've learned the basics of service management, you can access the [Complete API reference documentation for the Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) and perform complex tasks easily to manage your python application.</span></span>

<span data-ttu-id="fbd18-208">Další informace naleznete ve [Středisku pro vývojáře Python](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="fbd18-208">For more information, see the [Python Developer Center](/develop/python/).</span></span>

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
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
