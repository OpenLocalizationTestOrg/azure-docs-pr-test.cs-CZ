---
title: "aaaNode.js Příručka Začínáme | Microsoft Docs"
description: "Zjistěte, jak toocreate jednoduché Node.js webové aplikace a nasadit tooan cloudové služby Azure."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 50951a87-fed4-48e0-bcfa-453b9e50452e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 22945bfcc1b0e5da2a2d37dc5cc86be013cc0b5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-a-nodejs-application-tooan-azure-cloud-service"></a>Sestavení a nasazení tooan aplikace Node.js Cloudová služba Azure

Tento kurz ukazuje, jak toocreate jednoduché Node.js aplikace běžící v cloudové službě Azure. Cloudové služby jsou hello stavebními bloky škálovatelných cloudových aplikací v Azure. Umožňují hello oddělení, nezávislou správu a škálování součástí front-end a back-end vaší aplikace.  Cloud Services poskytují výkonný vyhrazený virtuální počítač, který spolehlivě hostuje jednotlivé role.

Další informace o cloudové služby a jejich porovnání tooAzure weby a virtuálních počítačů najdete v tématu [porovnání webů Azure, cloudové služby a virtuální počítače].

> [!TIP]
> Hledáte toobuild jednoduchý Web? Pokud váš scénář zahrnuje jen jednoduchý front-endový web, zvažte [Použití jednoduché webové aplikace]. Můžete snadno upgradovat tooa cloudové služby jako vaše webová aplikace naroste a vaše požadavky se změní.

V následujícím kurzu si vytvoříte jednoduchou webovou aplikaci, hostovanou v rámci webové role. Budou používat hello výpočetní emulátor tootest vaši aplikaci místně a potom ho pomocí nástroje příkazového řádku Powershellu nasadit.

aplikace Hello je jednoduchou aplikaci "hello world":

![Webový prohlížeč zobrazující webovou stránku Hello World hello][A web browser displaying hello Hello World web page]

## <a name="prerequisites"></a>Požadavky
> [!NOTE]
> Tento kurz používá prostředí Azure PowerShell, které vyžaduje systém Windows.

* Nainstalujte a nakonfigurujte [Azure Powershell].
* Stáhněte a nainstalujte hello [Azure SDK pro .NET 2.7]. Hello nainstalovat Instalační program, vyberte:
  * MicrosoftAzureAuthoringTools
  * MicrosoftAzureComputeEmulator

## <a name="create-an-azure-cloud-service-project"></a>Vytvoření projektu Azure Cloud Service
Proveďte následující úlohy toocreate nový projekt Azure Cloud Service, společně s základní uživatelské rozhraní Node.js hello:

1. Spustit **prostředí Windows PowerShell** jako správce; z hello **nabídce Start** nebo **obrazovce Start**, vyhledejte **prostředí Windows PowerShell**.
2. [Připojení Powershellu] tooyour předplatné.
3. Zadejte hello následující prostředí PowerShell rutinu toocreate toocreate hello projektu:

        New-AzureServiceProject helloworld

    ![výsledek Hello hello New-AzureService helloworld příkazu][hello result of hello New-AzureService helloworld command]

    Hello **New-AzureServiceProject** rutina generuje základní strukturu pro publikování tooa aplikace Node.js cloudové služby. Obsahuje konfigurační soubory, které jsou nezbytné pro publikování tooAzure. Hello rutina také změní pracovní adresář toohello adresář pro službu hello.

    Hello rutina vytvoří hello následující soubory:

   * **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** a **ServiceDefinition.csdef**: soubory specifické pro Azure, které jsou zapotřebí pro publikování aplikace. Další informace najdete v tématu [Přehled vytváření hostované služby pro Azure].
   * **deploymentSettings.json**: uloží místní nastavení, které jsou používány hello rutin nasazení prostředí Azure PowerShell.
4. Zadejte následující příkaz tooadd nové webové role hello:

       Add-AzureNodeWebRole

   ![výstup Hello hello příkazu Add-AzureNodeWebRole][hello output of hello Add-AzureNodeWebRole command]

   Hello **Add-AzureNodeWebRole** rutina vytvoří základní aplikaci Node.js. Také upraví hello **.csfg** a **.csdef** soubory tooadd položky konfigurace pro novou roli hello.

   > [!NOTE]
   > Pokud nezadáte název role, použije se výchozí název. Název můžete zadat jako první parametr rutiny hello:`Add-AzureNodeWebRole MyRole`

aplikace Node.js Hello je definována v souboru hello **server.js**, který je umístěn v adresáři hello hello webovou roli (**WebRole1** ve výchozím nastavení). Tady je kód hello:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Tento kód je v podstatě hello stejná hodnota jako text "Hello, World" hello ukázku hello [nodejs.org] web, s výjimkou používá hello číslo portu přiřazené hello cloudového prostředí.

## <a name="deploy-hello-application-tooazure"></a>Nasazení aplikace tooAzure hello

> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet Azure. Můžete si [aktivovat výhody předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) nebo si [zaregistrovat bezplatný účet](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="download-hello-azure-publishing-settings"></a>Hello Azure stáhnout nastavení publikování
toodeploy tooAzure vaší aplikace, musíte nejprve stáhnout hello publikování nastavení pro vaše předplatné Azure.

1. Spusťte následující rutiny Azure Powershellu hello:

       Get-AzurePublishSettingsFile

   Tímto dojde k použití prohlížeče toonavigate toohello stránky stažení nastavení publikování. Může být výzvami toolog pomocí Account Microsoft. Pokud ano, použijte účet hello spojený s předplatným Azure.

   Uložte hello stáhnout umístění souboru tooa profil, který lze snadno přistupovat.
2. Spusťte následující rutinu tooimport hello stažený profil publikování:

       Import-AzurePublishSettingsFile [path toofile]

    > [!NOTE]
    > Po importu hello nastavení publikování, zvažte odstranění hello stáhli soubor .publishSettings, protože obsahuje informace, které by někomu mohly umožnit tooaccess váš účet.

### <a name="publish-hello-application"></a>Publikování aplikace hello
toopublish spustit hello následující příkazy:

      $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

* **-ServiceName** určuje hello název pro nasazení hello. Toto musí být jedinečný název, jinak hello publikovat proces se nezdaří. Hello **Get-Date** přiřadí k řetězci datum/čas, aby hello název jedinečný.
* **-Umístění** určuje hello datacenter, jejímž hostitelem aplikace hello. seznam dostupných datových center, použijte hello toosee **Get-AzureLocation** rutiny.
* **-Launch** otevře okno prohlížeče a po dokončení nasazení přejde toohello hostované služby.

Po publikování bude úspěšné, zobrazí se odpověď podobnou toohello následující:

![výstup Hello hello příkazu Publish-AzureService][hello output of hello Publish-AzureService command]

> [!NOTE]
> Může trvat několik minut, než toodeploy aplikace hello a k dispozici při prvním publikování.

Po dokončení nasazení hello bude okno prohlížeče otevřete a přejděte toohello cloudové služby.

![Okno prohlížeče zobrazující hello hello world stránky; Hello URL značí, že hello stránka hostována v Azure.][A browser window displaying hello hello world page; hello URL indicates hello page is hosted on Azure.]

Aplikace je nyní spuštěna v systému Azure.

Hello **Publish-AzureServiceProject** rutina provede hello následující kroky:

1. Vytvoří toodeploy balíčku. Hello balíček obsahuje všechny hello soubory ve složce aplikace.
2. Vytvoří nový **účet úložiště**, pokud žádný neexistuje. Hello účtu úložiště Azure je balíček aplikace hello toostore používané během nasazení. Po dokončení nasazení můžete bezpečně odstranit účet úložiště hello.
3. Vytvoří novou **cloudovou službu**, pokud žádná neexistuje. A **Cloudová služba** je hello kontejner, ve kterém je vaše aplikace hostována, když je nasazené tooAzure. Další informace najdete v tématu [Přehled vytváření hostované služby pro Azure].
4. Publikuje tooAzure balíčku nasazení hello.

## <a name="stopping-and-deleting-your-application"></a>Zastavení a odstranění vaší aplikace
Po nasazení aplikace, může být vhodné toodisable ji tak, že se můžete vyhnout poplatkům. Azure účtuje poplatky za instance webových rolí podle času (v hodinách), který na serveru spotřebují. Čas serveru se spotřebovává, jakmile vaše aplikace je nasazená, i když hello instance nejsou spuštěné a jsou ve stavu hello byla zastavena.

1. V okně prostředí Windows PowerShell hello zastavte hello nasazení služby vytvořené v předchozí části hello s hello následující rutiny:

       Stop-AzureService

   Zastavování služby hello může trvat několik minut. Pokud je hello služba zastavená, obdržíte zprávu s upozorněním, že se zastavil.

   ![Hello stav příkazu hello Stop-AzureService][hello status of hello Stop-AzureService command]
2. toodelete hello služba, volání hello následující rutiny:

       Remove-AzureService

   Po zobrazení výzvy zadejte **Y** toodelete hello služby.

   Odstraněním hello služby může trvat několik minut. Po odstranění hello služby obdržíte zprávu s upozorněním, že se odstranila služba hello.

   ![Hello stav příkazu hello Remove-AzureService][hello status of hello Remove-AzureService command]

   > [!NOTE]
   > Odstraněním služby hello nedojde k odstranění účtu úložiště hello, který byl vytvořen při prvním publikování služby hello a bude pokračovat toobe účtovány poplatky za úložiště použít. Pokud se nic jiného používá hello úložiště, může být vhodné toodelete ho.

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello [středisku pro vývojáře Node.js].

<!-- URL List -->

[porovnání webů Azure, cloudové služby a virtuální počítače]: ../app-service-web/choose-web-site-cloud-service-vm.md
[Použití jednoduché webové aplikace]: ../app-service-web/app-service-web-get-started-nodejs.md
[Azure Powershell]: /powershell/azureps-cmdlets-docs
[Azure SDK pro .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Připojení Powershellu]: /powershell/azureps-cmdlets-docs#step-3-connect
[nodejs.org]: http://nodejs.org/
[Přehled vytváření hostované služby pro Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[středisku pro vývojáře Node.js]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[hello result of hello New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[hello output of hello Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying hello Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[hello output of hello Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying hello hello world page; hello URL indicates hello page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[hello status of hello Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[hello status of hello Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
