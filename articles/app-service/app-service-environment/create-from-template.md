---
title: "aaaCreate služby Azure App Service environment pomocí šablony Resource Manageru"
description: "Vysvětluje, jak toocreate prostředí externí nebo ILB Azure App Service pomocí šablony Resource Manageru"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 6eb7d43d-e820-4a47-818c-80ff7d3b6f8e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: c8aeedee675a6e931169b725ee916cc7fa8f762f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-ase-by-using-an-azure-resource-manager-template"></a>Vytvořit App Service Environment pomocí šablony Azure Resource Manager

## <a name="overview"></a>Přehled
Prostředí Azure App Service (ASEs) může být vytvořen pomocí koncový bod přístupné z Internetu nebo koncový bod na interní adresu ve virtuální sítě Azure (VNet). Vytvoření s vnitřní koncový bod tohoto koncového bodu je zadaný ve Azure název součásti (ILB) pro vyrovnávání zatížení interní. Hello App Service Environment na interní IP adresu se nazývá ILB App Service Environment. Hello App Service Environment se veřejný koncový bod se nazývá externí App Service Environment. 

App Service Environment lze vytvořit pomocí portálu Azure hello nebo šablonu Azure Resource Manager. Tento článek vás provede kroky hello a syntaxe, je třeba toocreate na externí App Service Environment nebo ILB App Service Environment pomocí šablony Resource Manageru. jak toocreate App Service Environment v hello portál Azure, najdete v části toolearn [zkontrolujte externí App Service Environment] [ MakeExternalASE] nebo [zkontrolujte App Service Environment ILB][MakeILBASE].

Když vytvoříte v hello portál Azure App Service Environment, můžete vytvořit virtuální síť v hello stejný čas nebo vyberte dříve vytvořený toodeploy virtuální sítě do. Při vytváření App Service Environment ze šablony, je nutné začít s: 

* Virtuální sítě Resource Manageru.
* Podsítě v této virtuální sítě. Doporučujeme, abyste velikost podsítě App Service Environment `/25` s růstem do budoucna tooaccomodate 128 adresy. Po vytvoření hello App Service Environment, nelze změnit velikost hello.
* ID prostředku Hello z vaší virtuální sítě. Tyto informace můžete získat z hello portálu Azure v části vlastnosti vaší virtuální sítě.
* Hello předplatné, které chcete toodeploy do.
* Hello umístění, do kterého chcete toodeploy do.

tooautomate vaše vytvoření App Service Environment:

1. Vytvořte App Service Environment hello ze šablony. Pokud vytvoříte externí App Service Environment, budete hotovi po provedení tohoto kroku. Pokud vytvoříte app Service Environment ILB, existuje několik toodo další věci.

2. Po vytvoření vaší App Service Environment ILB je nahrát certifikát SSL, který odpovídá vaší domény ILB App Service Environment.

3. Hello nahraný certifikát SSL přiřazený toohello App Service Environment ILB jako certifikát SSL jeho "default".  Tento certifikát se používá pro tooapps provoz protokolu SSL na hello ILB App Service Environment, když používají hello běžné kořenové domény, která je přiřazená toohello App Service Environment (například https://someapp.mycustomrootcomain.com).


## <a name="create-hello-ase"></a>Vytvořit App Service Environment hello
Šablony Resource Manageru, která vytvoří App Service Environment a jeho přidružené parametry souboru je k dispozici [v příklad] [ quickstartasev2create] na Githubu.

Pokud chcete toomake App Service Environment ILB, použijte tyto šablony Resource Manageru [příklady][quickstartilbasecreate]. Budou nahrazovat toothat případ použití. Většina hello parametrů v hello *azuredeploy.parameters.json* souboru jsou běžné toohello vytvoření ILB ASEs a externí ASEs. Hello následující seznam volá výstupní parametry speciální Poznámka, nebo které jsou jedinečné, při vytváření App Service Environment ILB:

* *interalLoadBalancingMode*: ve většině případů nastavena tato too3, to znamená i přenosy HTTP/HTTPS na porty 80/443, a porty channel data ovládacího prvku nebo hello naslouchali tooby služby hello FTP na hello App Service Environment, bude vázané tooan ILB přidělené virtuální sítě Interní adresa. Pokud je tato vlastnost nastavená too2, pouze porty hello FTP související se službou (řízení a datové kanály) jsou vázané tooan ILB adresu. Hello přenosy HTTP/HTTPS zůstává na veřejné VIP hello.
* *dnsSuffix*: Tento parametr definuje hello výchozí kořenové domény, který je přiřazený toohello App Service Environment. V hello veřejné varianta Azure App Service, hello výchozí kořenové domény pro všechny webové aplikace je *azurewebsites.net*. App Service Environment ILB je interní tooa zákazníka virtuální sítě, a proto doesn't make sense toouse hello veřejné služby výchozí kořenové domény. Místo toho ILB App Service Environment, musí mít výchozí kořenové domény, která je vhodná pro použití v rámci společnosti interní virtuální sítě. Společnost Contoso může například použít výchozí kořenovou doménu z *interní contoso.com* pro aplikace, které jsou určené toobe přeložit a přístupné pouze v rámci virtuální síť společnosti Contoso. 
* *ipSslAddressCount*: Tento parametr automaticky výchozí hodnota 0 v hello tooa *azuredeploy.json* soubor protože ILB ASEs mít pouze jednu adresu ILB. Nejsou žádné explicitní IP SSL adresy pro ILB App Service Environment. Proto hello fond adres IP SSL pro ILB App Service Environment musí být nastavena toozero. Jinak dojde k chybě při zřizování. 

Po hello *azuredeploy.parameters.json* souboru je vyplněno, vytvořte hello App Service Environment pomocí fragmentu kódu Powershellu hello. Změna hello cesty toomatch hello Resource Manager soubor šablony umístění souborů na váš počítač. Pamatovat si toosupply vlastních hodnot pro název nasazení Resource Manager hello a název skupiny prostředků hello:

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Trvá hodinu toobe App Service Environment hello vytvořili. Potom hello App Service Environment se zobrazí v portálu hello hello seznamu ASEs hello odběru, který aktivuje hello nasazení.

## <a name="upload-and-configure-hello-default-ssl-certificate"></a>Nahrání a konfigurace certifikátu protokolu SSL "Výchozí" hello
Certifikát SSL musí být přidružen App Service Environment hello jako certifikát SSL "Výchozí" hello, který je tooapps připojení SSL používané tooestablish. Pokud přípona DNS hello App Service Environment na výchozím nastavení je *interní contoso.com*, toohttps://some-random-app.internal-contoso.com připojení vyžaduje certifikát SSL, který je platný pro **.internal contoso.com* . 

Získejte platný certifikát SSL pomocí interní certifikačních autorit, nákupu certifikát z externího vystavitele, nebo certifikát podepsaný svým držitelem. Bez ohledu na to hello zdroje hello certifikátu protokolu SSL musí být správně nakonfigurované hello následující atributy certifikátu:

* **Předmět**: Tento atribut musí být nastaven příliš **.your kořenové domény here.com*.
* **Alternativní název subjektu**: Tento atribut musí obsahovat i **.your kořenové domény here.com* a **.scm.your-kořenové-domain-here.com*. Toohello připojení SSL SCM/Kudu lokality spojené s každou aplikaci použít adresu ve tvaru hello *your-app-name.scm.your-root-domain-here.com*.

S platným certifikátem SSL v dolním je potřeba dva další přípravné kroky. Převést nebo uložení certifikátu SSL hello jako soubor .pfx. Mějte na paměti, že soubor .pfx hello musí zahrnovat všechny zprostředkující a kořenové certifikáty. Zabezpečte ji pomocí hesla.

soubor .pfx Hello musí toobe převést na řetězec ve formátu base64, protože certifikát SSL hello je odeslán pomocí šablony Resource Manageru. Protože šablony Resource Manageru jsou textové soubory, vyhledejte soubor .pfx hello je převést na řetězec ve formátu base64. Tímto způsobem může být zahrnuta jako parametr šablony hello.

Následující fragment kódu prostředí PowerShell k použití hello:

* Vygenerujte certifikát podepsaný svým držitelem.
* Hello certifikát exportujte jako soubor .pfx.
* Soubor .pfx hello převeďte řetězec s kódováním base64.
* Hello řetězec s kódováním base64 tooa samostatný soubor uložte. 

Tento kód prostředí PowerShell pro kódování base64 byla přizpůsobena z hello [skriptů prostředí PowerShell blog][examplebase64encoding]:

        $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

        $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
        $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

        $fileName = "exportedcert.pfx"
        Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

        $fileContentBytes = get-content -encoding byte $fileName
        $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
        $fileContentEncoded | set-content ($fileName + ".b64")

Po hello certifikátu SSL se úspěšně vygeneroval a převést řetězec tooa kódováním base64, pomocí šablony Resource Manageru příklad hello [certifikát protokolu SSL výchozí konfigurace hello] [ quickstartconfiguressl] na Githubu. 

Hello parametry v hello *azuredeploy.parameters.json* souboru jsou tady:

* *appServiceEnvironmentName*: název hello hello ILB App Service Environment se právě nastavuje.
* *existingAseLocation*: textový řetězec obsahující hello oblast Azure, kde hello App Service Environment ILB byl nasazen.  Například: "Jihu USA".
* *pfxBlobString*: hello řetězec s kódováním based64 reprezentace hello souboru .pfx. Pomocí fragmentu kódu hello uvedena výše a zkopírujte řetězec hello obsažené v "exportedcert.pfx.b64". Vložte jej jako hodnota hello hello *pfxBlobString* atribut.
* *heslo*: soubor .pfx hello heslo použité toosecure hello.
* *certificateThumbprint*: hello kryptografický otisk certifikátu. Pokud načetlo tuto hodnotu z prostředí PowerShell (například *$certificate. Kryptografický otisk* z hello starší fragment kódu), můžete použít hello hodnotu, jako je. Pokud zkopírujete hello hodnotu z dialogového okna certifikátu Windows hello, mějte na paměti toostrip out hello nadbytečné mezery. Hello *certificateThumbprint* by měl vypadat podobně jako AF3143EB61D43F6727842115BB7F17BBCECAECAE.
* *certificateName*: identifikátor popisný řetězec podle vlastní volby použít tooidentity hello certifikátu. Název Hello se používá jako součást hello jedinečný identifikátor správce prostředků pro hello *Microsoft.Web/certificates* entita, která představuje hello certifikát SSL. Název Hello *musí* končit hello následující příponu: \_yourASENameHere_InternalLoadBalancingASE. Hello portál Azure používá tato přípona jako indikátor, hello certifikátu je použité toosecure App Service Environment povolené ILB.

Příklad zkrácené *azuredeploy.parameters.json* zobrazí se zde:

    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Po hello *azuredeploy.parameters.json* souboru je vyplněno, nakonfigurujte certifikát protokolu SSL výchozí hello pomocí fragmentu kódu Powershellu hello. Změnit hello souboru cesty toomatch, kde jsou umístěny soubory šablony Resource Manageru hello na váš počítač. Pamatovat si toosupply vlastních hodnot pro název nasazení Resource Manager hello a název skupiny prostředků hello:

     $templatePath="PATH\azuredeploy.json"
     $parameterPath="PATH\azuredeploy.parameters.json"

     New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Jak dlouho trvá přibližně 40 minutách App Service Environment front-endu tooapply hello změny. Například výchozí velikosti App Service Environment používající dva elementy end front, hello šablony trvá přibližně jednu hodinu a toocomplete 20 minut. Je spuštěn hello šablony, nelze škálovat hello App Service Environment.  

Po dokončení hello šablony aplikace na hello ILB App Service Environment jsou přístupné přes protokol HTTPS. Hello připojení jsou zabezpečené pomocí certifikátu protokolu SSL výchozí hello. certifikát protokolu SSL výchozí Hello se používá při aplikace na hello ILB App Service Environment jsou řešit pomocí kombinace názvu aplikace hello plus hello výchozí název hostitele. Například https://mycustomapp.internal-contoso.com používá hello výchozí certifikát SSL pro **.internal contoso.com*.

Stejně jako aplikace, které běží na hello veřejné víceklientské služby, ale vývojáři můžete nakonfigurovat vlastní hostitel názvy pro jednotlivé aplikace. Také mohou nakonfigurovat jedinečnou vazby certifikátů SNI SSL pro jednotlivé aplikace.

## <a name="app-service-environment-v1"></a>App Service Environment v1 ##
Služba App Service Environment má dvě verze: ASEv1 a ASEv2. Hello předcházející informace bylo založeno na ASEv2. Tato část obsahuje hello rozdíly mezi ASEv1 a ASEv2.

V ASEv1 můžete spravovat všechny prostředky hello ručně. Která obsahuje hello front-end, pracovní procesy a adres IP použitých pro založená na protokolu IP. Předtím, než můžete škálovat plán služby App Service, musí vertikální navýšení kapacity fondu hello pracovních procesů, které chcete toohost ho.

ASEv1 používá jiný model tvorby cen z ASEv2. V ASEv1 platí pro každý jádra přidělené. Počet jader, které jsou používány pro front-end nebo pracovních procesů, které nejsou hostování jakékoli úlohy, který zahrnuje. V ASEv1 hello výchozí škálování maximální velikost App Service Environment je 55 celkový počet hostitelů. Který zahrnuje pracovníků a front-end. Jeden tooASEv1 výhod je, aby se dala nasadit v klasické virtuální sítě a virtuální sítě Resource Manager. toolearn Další informace o ASEv1, najdete v části [App Service Environment v1 ÚVOD][ASEv1Intro].

toocreate ASEv1 pomocí šablony Resource Manageru, najdete v části [vytvoření App Service Environment ILB v1 pomocí šablony Resource Manageru][ILBASEv1Template].


<!--Links-->
[quickstartilbasecreate]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-ilb-create
[quickstartasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-create
[quickstartconfiguressl]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl
[quickstartwebapponasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asp-app-on-asev2-create
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ILBASEv1Template]: ../../app-service-web/app-service-app-service-environment-create-ilb-ase-resourcemanager.md
