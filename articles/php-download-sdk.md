---
title: aaaDownload hello Azure SDK pro jazyk PHP
description: "Zjistěte, jak toodownload a nainstalujte hello Azure SDK pro jazyk PHP."
documentationcenter: php
services: app-service\web
author: allclark
manager: douge
editor: 
ms.assetid: bac355ac-4c25-42f4-8273-c5112eafa8d4
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 06/01/2016
ms.author: allclark;yaqiyang
ms.openlocfilehash: 94f56fc4f91bb175c08b9f7a43cb221c827694a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="download-hello-azure-sdk-for-php"></a>Stáhnout hello Azure SDK pro jazyk PHP
## <a name="overview"></a>Přehled
Hello Azure SDK pro jazyk PHP obsahuje součásti, které vám umožňují toodevelop, nasazení a Správa aplikací PHP pro Azure. Konkrétně hello Azure SDK pro jazyk PHP obsahuje hello následující:

* **Hello PHP klientské knihovny pro Azure**. Tyto knihovny tříd poskytují rozhraní pro přístup k Azure funkcí, jako jsou služby pro správu dat a cloudových služeb.  
* **Hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)**. Toto je sadu příkazů pro nasazení a správu služby Azure, jako jsou weby Azure a virtuálních počítačích Azure. rozhraní příkazového řádku Azure pracovní na jakékoli platformě, včetně Mac, Linux a Windows Hello.
* **Prostředí Azure PowerShell (jenom Windows)**. To je sada rutin prostředí PowerShell pro nasazení a Správa služby Azure, jako je cloudových služeb a virtuálních počítačů.
* **Hello (pouze Windows) Azure emulátorů**. Hello emulátorů výpočetního prostředí a úložiště jsou místní emulátorů cloudové služby a služby pro data, které vám umožňují tootest aplikace místně. Hello Azure emulátorů lze spustit pouze v systému Windows.

Hello části níže popisují, jak toodownload a nainstalujte hello komponent popsaných výše.

Hello pokyny v tomto tématu se předpokládá, že máte [PHP] [ install-php] nainstalována.

> [!NOTE]
> PHP 5.5 nebo vyšší toouse hello PHP klientské knihovny musí mít pro Azure.
> 
> 

## <a name="php-client-libraries-for-azure"></a>Klientské knihovny PHP pro Azure
Hello PHP klientské knihovny pro Azure poskytují rozhraní pro přístup k Azure funkcí, jako jsou služby pro správu dat a cloudové služby ve všech operačních systémech. Tyto knihovny se může nainstalovat prostřednictvím hello autora.

Informace o tom, jak toouse hello PHP klientské knihovny pro Azure najdete v tématu [jak tooUse hello služby objektů Blob][blob-service], [jak tooUse hello služby Table] [ table-service] a [jak tooUse hello služby front][queue-service].

### <a name="install-via-composer"></a>Nainstalovat prostřednictvím autora
1. [Nainstalovat Git][install-git].

    > [AZURE.NOTE] V systému Windows budete také potřebovat proměnné prostředí PATH spustitelné tooyour Git tooadd hello.

1. Vytvořte soubor s názvem **composer.json** v kořenu projektu hello a přidejte následující kód tooit hello:
   
        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }
2. Stáhněte si  **[composer.phar] [ composer-phar]**  v kořenového adresáře projektu.
3. Otevřete příkazový řádek a spusťte tento v kořenového adresáře projektu
   
        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a>Prostředí Azure PowerShell a Azure emulátorů
Prostředí Azure PowerShell je sada rutin prostředí PowerShell pro nasazení a Správa služby Azure (například cloudové služby a virtuální počítače). Hello Azure emulátorů jsou emulátorů cloudové služby a služby pro data, které vám umožňují tootest aplikace místně. Tyto součásti jsou podporovány pouze v systému Windows.

Hello doporučeným způsobem tooinstall prostředí Azure PowerShell a hello emulátorů Azure je toouse hello [instalačního programu webové platformy Microsoft][download-wpi]. Všimněte si, že je také možné tooinstall jiné komponenty, vývoj, například PHP, SQL Server, hello Drivers společnosti Microsoft pro systém SQL Server pro PHP a službě WebMatrix.

Informace o tom najdete v části toouse prostředí Azure PowerShell [jak tooUse prostředí Azure PowerShell][powershell-tools].

## <a name="azure-cli"></a>Azure CLI
Hello rozhraní příkazového řádku Azure je sadu příkazů pro nasazení a správu služby Azure, jako jsou weby Azure a virtuálních počítačích Azure. Informace o instalaci rozhraní příkazového řádku Azure najdete v tématu [hello instalace rozhraní příkazového řádku Azure](cli-install-nodejs.md).

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
