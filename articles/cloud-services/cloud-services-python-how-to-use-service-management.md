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
# <a name="how-toouse-service-management-from-python"></a>Jak toouse Správa služby z Pythonu
Tento průvodce vám ukáže, jak tooprogrammatically provádět běžné úlohy správy služby z Pythonu. Hello **ServiceManagementService** třídy v hello [Azure SDK pro jazyk Python](https://github.com/Azure/azure-sdk-for-python) podporuje programový přístup toomuch hello služby týkajících se správy funkcí, které jsou k dispozici v hello [Portál azure classic] [ management-portal] (například **vytváření, aktualizaci a odstraňování cloudové služby, nasazení, služby pro správu dat a virtuální počítače**). Tato funkce může být užitečné při vytváření aplikace, které potřebují správu tooservice programový přístup.

## <a name="WhatIs"></a>Co je služba správy
Hello Service Management API poskytuje programový přístup toomuch hello služby správy funkcí dostupných prostřednictvím hello [portál Azure classic][management-portal]. Hello Azure SDK pro jazyk Python vám umožní toomanage cloudové služby a účty úložiště.

toouse hello Service Management API, musíte příliš[vytvoření účtu Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"></a>Koncepty
Hello Azure SDK pro jazyk Python zabalí hello [Azure Service Management API][svc-mgmt-rest-api], což je rozhraní REST API. Všechny operace rozhraní API se provádí přes SSL a vzájemně se ověřují pomocí certifikátů X.509 v3. Služba správy Hello může přístup v rámci služby spuštěné v Azure, nebo přímo přes hello Internet z jakékoli aplikace, která může požadavek HTTPS odesílat a přijímat odpověď protokolu HTTPS.

## <a name="Installation"></a>Instalace
Všechny funkce hello popsané v tomto článku jsou k dispozici v hello `azure-servicemanagement-legacy` balíčku, které můžete nainstalovat pomocí nástroje pip. Další informace o instalaci (například pokud jste nový tooPython), najdete v tomto článku: [instalaci Pythonu a hello Azure SDK](../python-how-to-install.md)

## <a name="Connect"></a>Postupy: připojení tooservice správy
koncový bod Service Management toohello tooconnect, musíte svoje ID předplatného Azure a certifikát pro správu platné. Můžete získat svoje ID předplatného prostřednictvím hello [portál Azure classic][management-portal].

> [!NOTE]
> Nyní je možné toouse certifikáty vytvořené pomocí OpenSSL, spuštěná v systému Windows.  Vyžaduje Python 2.7.4 nebo novější. Doporučujeme, abyste uživatelům toouse OpenSSL místo .pfx, od podporu pro .pfx, certifikáty budou odebrány pravděpodobně v budoucnu hello.
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Certifikáty pro správu v systému Windows nebo Mac/Linux (OpenSSL)
Můžete použít [OpenSSL](http://www.openssl.org/) toocreate svůj certifikát pro správu.  Skutečně potřebujete dva certifikáty toocreate, jeden pro hello server ( `.cer` souboru) a jeden pro klienta hello ( `.pem` souboru). toocreate hello `.pem` soubor, spustit:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

toocreate hello `.cer` certifikátů, spustit:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Další informace o Azure certifikáty najdete v tématu [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md). Úplný popis parametrů OpenSSL, naleznete v dokumentaci k hello v [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Po vytvoření těchto souborů, je nutné tooupload hello `.cer` souboru tooAzure prostřednictvím hello akce "Odeslat" hello "Nastavení" karty hello [portál Azure classic][management-portal], a poznamenejte si toomake potřebujete kam jste uložili hello `.pem` souboru.

Po získání svoje ID předplatného, vytvořen certifikát a odesláno hello `.cer` tooAzure souboru můžete připojit koncový bod Azure správy toohello předáním hello id předplatného a hello cesta toohello `.pem` souboru příliš**ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

V předchozím příkladu hello `sms` je **ServiceManagementService** objektu. Hello **ServiceManagementService** třída je hello primární třída používaná toomanage služby Azure.

### <a name="management-certificates-on-windows-makecert"></a>Certifikáty pro správu v systému Windows (MakeCert)
Můžete vytvořit certifikát podepsaný svým držitelem správy na váš počítač pomocí `makecert.exe`.  Otevřete **příkazového řádku Visual Studia** jako **správce** a použijte následující příkaz, nahraďte hello *AzureCertificate* s názvem certifikátu hello chcete toouse.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

příkaz Hello vytvoří hello `.cer` souboru a nainstaluje v hello **osobní** úložiště certifikátů. Další informace najdete v tématu [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md).

Po vytvoření hello certifikát, je nutné tooupload hello `.cer` souboru tooAzure prostřednictvím hello akce "Odeslat" hello "Nastavení" karty hello [portál Azure classic][management-portal].

Po získání svoje ID předplatného, vytvořen certifikát a odesláno hello `.cer` tooAzure souboru můžete připojit koncový bod Azure správy toohello předáním hello id předplatného a hello umístění certifikátu hello ve vaší **Osobní** úložišti certifikátů příliš**ServiceManagementService** (znovu, nahraďte *AzureCertificate* s názvem hello vašeho certifikátu):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

V předchozím příkladu hello `sms` je **ServiceManagementService** objektu. Hello **ServiceManagementService** třída je hello primární třída používaná toomanage služby Azure.

## <a name="ListAvailableLocations"></a>Postupy: seznam dostupných umístění
toolist hello umístění, které jsou k dispozici pro hostování služeb, použijte hello **seznamu\_umístění** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Při vytváření cloudové služby nebo služba úložiště musíte tooprovide platné umístění. Hello **seznamu\_umístění** metoda vždy vrátí hodnotu aktuální seznam aktuálně dostupných umístění hello. Době psaní tohoto textu hello k dispozici umístění jsou:

* Západní Evropa
* Severní Evropa
* Jihovýchodní Asie
* Východní Asie
* Střed USA
* Střed USA – sever
* Střed USA – jih
* Západní USA
* Východ USA
* Japonsko – východ
* Japonsko – západ
* Brazílie – jih
* Austrálie – východ
* Austrálie – jihovýchod

## <a name="CreateCloudService"></a>Postupy: vytvoření cloudové služby
Když vytvoříte aplikaci a spustíte ho v Azure, hello kódu a konfigurace společně se nazývají Azure [Cloudová služba] [ cloud service] (označované jako *hostovaná služba* v dříve Uvolní Azure). Hello **vytvořit\_hostované\_služby** metoda vám umožní toocreate novou hostovanou službu tím, že název hostované služby (které musí být jedinečné v Azure), poskytuje popisek (automaticky kódovaného toobase64), Popis a umístění.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Můžete vytvořit seznam všech hello hostovaných služeb pro vaše předplatné s hello **seznamu\_hostované\_služby** metoda:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Pokud chcete tooget informace o konkrétní hostovanou službu, můžete tak učinit pomocí předání hello hostované služby název toohello **získat\_hostované\_služby\_vlastnosti** metoda:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Po vytvoření cloudové služby, můžete nasadit služby toohello kód prostřednictvím hello **vytvořit\_nasazení** metoda.

## <a name="DeleteCloudService"></a>Postupy: odstranění cloudové služby
Cloudové služby můžete odstranit předáním hello služby název toohello **odstranit\_hostované\_služby** metoda:

    sms.delete_hosted_service('myhostedservice')

Před odstraněním služby, musíte nejprve odstranit všechna nasazení služby hello. (Viz [postupy: odstranění nasazení](#DeleteDeployment) podrobnosti.)

## <a name="DeleteDeployment"></a>Postupy: odstranění nasazení
toodelete nasazení, použijte hello **odstranit\_nasazení** metoda. Hello následující příklad ukazuje, jak toodelete nasazení s názvem `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"></a>Postupy: vytvoření služby úložiště
A [služba úložiště](../storage/common/storage-create-storage-account.md) dává vám přístup tooAzure [objekty BLOB](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabulky](../cosmos-db/table-storage-how-to-use-python.md), a [fronty](../storage/queues/storage-python-how-to-use-queue-storage.md). toocreate služba úložiště, musíte jako název služby hello (mezi 3 a 24 malých písmen a jedinečný v rámci Azure), popis, popisek (až too100 znaky, automaticky kódovaného toobase64) a umístění. Hello následující příklad ukazuje, jak služba toocreate úložiště tak, že zadáte umístění.

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

Poznámka: v předchozím příkladu hello hello stav hello **vytvořit\_úložiště\_účet** operaci se dá načíst pomocí předání hello výsledek vrácený **vytvořit\_úložiště \_účet** toohello **získat\_operace\_stav** metoda.  

Můžete vytvořit seznam účtů úložiště a jejich vlastnosti s hello **seznamu\_úložiště\_účty** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"></a>Postupy: odstranění služby úložiště
Služba úložiště můžete odstranit předáním hello úložiště služby název toohello **odstranit\_úložiště\_účet** metoda. Odstraněním úložiště služby se odstraní všechna data uložená ve službě hello (objekty BLOB, tabulek a front).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"></a>Postupy: seznam dostupných operačních systémů
toolist hello operační systémy, které jsou k dispozici pro hostování služeb, použijte hello **seznamu\_operační\_systémy** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Alternativně můžete použít hello **seznamu\_operační\_systému\_rodiny** metodu, která skupiny hello operační systémy řady:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"></a>Postupy: vytvoření image operačního systému
tooadd úložiště bitové kopie toohello bitové kopie operačního systému použít hello **přidat\_os\_image** metoda:

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

Image operačního systému hello toolist, které jsou k dispozici, použijte hello **seznamu\_os\_bitové kopie** metoda. Obsahuje všechny Image platformy a uživatele bitové kopie:

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

## <a name="DeleteVMImage"></a>Postupy: odstranění image operačního systému
toodelete uživatelská image použít hello **odstranit\_os\_image** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"></a>Postupy: vytvoření virtuálního počítače
toocreate virtuálního počítače, musíte nejprve toocreate [Cloudová služba](#CreateCloudService).  Pak vytvořte hello nasazení virtuálního počítače pomocí hello **vytvořit\_virtuální\_počítač\_nasazení** metoda:

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

## <a name="DeleteVM"></a>Postupy: odstranění virtuálního počítače
toodelete virtuálního počítače, je nejprve odstranit hello nasazení pomocí hello **odstranit\_nasazení** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Hello cloudovou službu můžete odstranit pak pomocí hello **odstranit\_hostované\_služby** metoda:

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Postupy: Vytvoření virtuálního počítače z Image zaznamenané virtuálního počítače
toocapture image virtuálního počítače, první volání hello **zaznamenat\_virtuálních počítačů\_image** metoda:

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

Dále toomake jistotu, že jste se úspěšně zachytil hello image, použijte hello **seznamu\_virtuálních počítačů\_bitové kopie** rozhraní api a zajistěte, aby bitové kopie se zobrazí ve výsledcích hello:

    images = sms.list_vm_images()

toofinally vytvořit hello virtuální počítač pomocí hello zaznamenané bitové kopie, použijte hello **vytvořit\_virtuální\_počítač\_nasazení** jako předtím, ale tentokrát předat hello vm_image_name místo toho – metoda

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

Další informace o toolearn, jak zjistit, toocapture virtuální počítač s Linuxem [jak tooCapture virtuální počítač s Linuxem.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

toolearn Další informace o tom, jak toocapture virtuálního počítače s Windows, najdete v části [jak tooCapture virtuálního počítače s Windows.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="What's Next"></a>Další kroky
Teď, když jste se naučili základy hello služby správy, dostanete hello [referenční dokumentace rozhraní API dokončení pro hello Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) a provádět komplexní úlohy snadno toomanage aplikace python.

Další informace najdete v tématu hello [středisku pro vývojáře Python](/develop/python/).

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
