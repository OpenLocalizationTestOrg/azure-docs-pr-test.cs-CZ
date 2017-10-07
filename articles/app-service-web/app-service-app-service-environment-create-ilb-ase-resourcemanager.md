---
title: "aaaHow tooCreate ILB App Service Environment pomocí šablon Azure Resource Manageru | Microsoft Docs"
description: "Zjistěte, jak načíst toocreate na interní nástroje pro vyrovnávání App Service Environment pomocí šablony Azure Resource Manager."
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 091decb6-b0de-42a1-9f2f-c18d9b2e67df
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: stefsch
ms.openlocfilehash: 16db20eccc232ccc73107fcc8291de180fb2a323
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-ilb-ase-using-azure-resource-manager-templates"></a>Jak tooCreate ILB App Service Environment pomocí šablon Azure Resource Manageru

> [!NOTE] 
> Tento článek je o hello App Service Environment v1. Není k dispozici novější verze hello služby App Service Environment, je snazší toouse a běží na výkonnější infrastruktury. Další informace o nové verzi hello začínat hello toolearn [toohello Úvod App Service Environment](../app-service/app-service-environment/intro.md).
>

## <a name="overview"></a>Přehled
Služby App Service Environment může být vytvořen pomocí adresu interní virtuální sítě místo veřejných virtuálních IP adres.  Tato interní adresa zajišťuje komponentu Azure, názvem vyrovnávání hello interní zatížení (ILB).  App Service Environment ILB lze vytvořit pomocí hello portálu Azure.  Je také možné vytvářet pomocí automatizace prostřednictvím šablon Azure Resource Manageru.  Tento článek vás provede kroky hello a syntaxe potřeby toocreate ILB App Service Environment pomocí šablon Azure Resource Manager.

Účastnící se automatizace vytváření ILB App Service Environment jsou tři kroky:

1. První základní App Service Environment hello je vytvořená ve virtuální sítě pomocí adresu služby Vyrovnávání zatížení interní místo veřejných virtuálních IP adres.  V rámci tohoto kroku je přiřazen název kořenové domény toohello ILB App Service Environment.
2. Po nahrání hello App Service Environment ILB je vytvořena, certifikát SSL.  
3. Hello odeslaný certifikát SSL není výslovně přiřazena toohello App Service Environment ILB jako certifikát SSL jeho "default".  Tento certifikát SSL se použije pro tooapps provoz protokolu SSL na hello ILB App Service Environment, když jsou aplikace hello řešit pomocí hello běžné kořenové domény přiřazené toohello App Service Environment (např. https://someapp.mycustomrootcomain.com)

## <a name="creating-hello-base-ilb-ase"></a>Vytváření hello základní ILB App Service Environment
Příklad šablony Azure Resource Manageru a jeho přidružené parametry souboru jsou dostupné na Githubu [sem][quickstartilbasecreate].

Většina hello parametrů v hello *azuredeploy.parameters.json* souboru jsou běžné toocreating obou ASEs ILB, stejně jako ASEs vázaný tooa veřejné VIP.  seznam Hello níže volá metodu výstupní parametry speciální Poznámka nebo které jsou jedinečné, při vytváření ILB App Service Environment:

* *interalLoadBalancingMode*: ve většině případů nastavena tato too3, to znamená i přenosy HTTP/HTTPS na porty 80/443, a porty channel data ovládacího prvku nebo hello naslouchali tooby služby hello FTP na hello App Service Environment, budou vázané tooan ILB přidělené virtuální sítě Interní adresa.  Pokud tuto vlastnost je místo toho nastavit too2, souvisí pouze hello služby FTP, že bude vázán porty (řízení a datové kanály) tooan ILB adresu, zatímco zůstane nainstalována na veřejné VIP hello hello přenosy HTTP/HTTPS.
* *dnsSuffix*: Tento parametr definuje hello výchozí kořenové domény, který se přiřadí toohello App Service Environment.  V hello veřejné varianta Azure App Service, hello výchozí kořenové domény pro všechny webové aplikace je *azurewebsites.net*.  Ale vzhledem k tomu, že App Service Environment ILB je interní tooa zákazníka virtuální sítě, doesn't make sense toouse hello veřejné služby výchozí kořenové domény.  Místo toho ILB App Service Environment, musí mít výchozí kořenové domény, která je vhodná pro použití v rámci společnosti interní virtuální sítě.  Společnosti Contoso Corporation může například použít výchozí kořenovou doménou *interní contoso.com* pro aplikace, které jsou určené tooonly přeložit a dostupné v rámci společnosti Contoso virtuální sítě. 
* *ipSslAddressCount*: Tento parametr je automaticky použita jako výchozí hodnota 0 v hello tooa *azuredeploy.json* soubor protože ILB ASEs mít pouze jednu adresu ILB.  Nejsou žádné explicitní IP SSL adresy pro ILB App Service Environment, a proto hello fond adres IP SSL pro ILB App Service Environment musí být nastavena toozero, jinak dojde k chybě při zřizování. 

Jednou hello *azuredeploy.parameters.json* souboru bylo vyplněno pro ILB App Service Environment, hello ILB App Service Environment bude možné vytvořit pomocí hello následující fragment kódu prostředí Powershell.  Změňte hello souboru cesty toomatch, kde jsou umístěny soubory šablony Azure Resource Manager hello na váš počítač.  Nezapomeňte taky, toosupply vlastních hodnot pro název nasazení Azure Resource Manager hello a název skupiny prostředků.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Šablona je odeslána po hello Azure Resource Manager, že bude trvat několik hodin, než hello App Service Environment ILB toobe vytvořit.  Po dokončení vytvoření hello hello ILB App Service Environment se zobrazí na portálu hello UX hello seznamu prostředí App Service pro hello odběr, který aktivoval nasazení hello.

## <a name="uploading-and-configuring-hello-default-ssl-certificate"></a>Nahrání a konfigurace hello "Výchozí" certifikát SSL
Jednou hello je vytvořena ILB App Service Environment, certifikát SSL by měly být přidružené App Service Environment hello jako pomocí certifikátu SSL hello "Výchozí" k vytvoření tooapps připojení SSL.  Pokračováním hello společnosti Contoso Corporation příklad, pokud hello App Service Environment pro výchozí DNS přípona je *interní contoso.com*, pak připojení příliš*https://some-random-app.internal-contoso.com*vyžaduje certifikát SSL, který je platný pro **.internal contoso.com*. 

Existují různé způsoby tooobtain platný certifikát SSL včetně interní certifikační autority, nákupu certifikát z externího vystavitelů a používá certifikát podepsaný svým držitelem.  Bez ohledu na to hello zdroje hello certifikátu protokolu SSL hello následující atributy certifikát potřebovat toobe správně nakonfigurována:

* *Předmět*: Tento atribut musí být nastaven příliš **.your kořenové domény here.com*
* *Alternativní název subjektu*: Tento atribut musí obsahovat i **.your kořenové domény here.com*, a **.scm.your-kořenové-domain-here.com*.  Hello pro druhou položku hello skutečnost, že toohello připojení SSL SCM/Kudu lokality spojené s každou aplikaci bude provedeno pomocí adresy hello formuláře *your-app-name.scm.your-root-domain-here.com*.

S platným certifikátem SSL v dolním je potřeba dva další přípravné kroky.  certifikát SSL Hello musí toobe převést nebo uložený jako soubor .pfx.  Mějte na paměti, že soubor .pfx hello potřebuje tooinclude všechny zprostředkující a kořenové certifikáty a také je nutné toobe zabezpečené heslem.

Soubor .pfx výsledné hello pak musí toobe převést na řetězec ve formátu base64, protože certifikát SSL hello se nahraje pomocí šablony Azure Resource Manager.  Vzhledem k tomu, že šablony Azure Resource Manager jsou textové soubory, musí mít soubor .pfx hello toobe převést na řetězec ve formátu base64, může být zahrnuta jako parametr šablony hello.

Následující fragment kódu prostředí Powershell Hello ukazuje příklad generování certifikát podepsaný svým držitelem, Export certifikátu hello jako soubor .pfx převodu hello soubor .pfx s kódováním base64 řetězec kódovaný, a potom uložení hello base64 kódovaný řetězec tooa samostatný soubor.  Hello kódu prostředí Powershell pro kódování base64 byla přizpůsobena z hello [Blog skripty prostředí Powershell][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")

Jakmile úspěšně vygenerování certifikátu SSL hello a kódováním base64, pomocí tooa převedený řetězec, hello příklad šablony Azure Resource Manageru na Githubu pro [konfigurace certifikátu protokolu SSL výchozí hello] [ configuringDefaultSSLCertificate] lze použít.

Hello parametry v hello *azuredeploy.parameters.json* souboru jsou uvedeny níže:

* *appServiceEnvironmentName*: název hello hello ILB App Service Environment se právě nastavuje.
* *existingAseLocation*: textový řetězec obsahující hello oblast Azure, kde hello App Service Environment ILB byl nasazen.  Například: "Jihu USA".
* *pfxBlobString*: hello based64 kódovaný řetězcovou reprezentaci hello soubor .pfx.  Pomocí fragmentu kódu hello uvedena výše, by hello řetězec obsažené v "exportedcert.pfx.b64" zkopírujte a vložte jej do jako hodnota hello hello *pfxBlobString* atribut.
* *heslo*: soubor .pfx hello heslo použité toosecure hello.
* *certificateThumbprint*: hello kryptografický otisk certifikátu.  Pokud načetlo tuto hodnotu z prostředí Powershell (například *$certificate. Kryptografický otisk* z hello starší fragment kódu), můžete použít hodnotu hello jako-je.  Ale při kopírování hello hodnotu z dialogového okna certifikátu Windows hello, nezapomeňte toostrip out hello nadbytečné mezery.  Hello *certificateThumbprint* by měl vypadat podobně jako: AF3143EB61D43F6727842115BB7F17BBCECAECAE
* *certificateName*: identifikátor popisný řetězec podle vlastní volby použít tooidentity hello certifikátu.  Název Hello se používá jako součást hello jedinečný identifikátor Azure Resource Manageru pro hello *Microsoft.Web/certificates* entity představující hello certifikát SSL.  Název Hello **musí** končit hello následující příponu: \_yourASENameHere_InternalLoadBalancingASE.  Tato přípona se používá portálem hello indikátor, hello certifikát se používá k zabezpečení App Service Environment povolené ILB.

Příklad zkrácené *azuredeploy.parameters.json* jsou uvedeny níže:

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

Jednou hello *azuredeploy.parameters.json* souboru bylo vyplněno, certifikát protokolu SSL výchozí hello můžete konfigurovat pomocí hello následující fragment kódu prostředí Powershell.  Změňte hello souboru cesty toomatch, kde jsou umístěny soubory šablony Azure Resource Manager hello na váš počítač.  Nezapomeňte taky, toosupply vlastních hodnot pro název nasazení Azure Resource Manager hello a název skupiny prostředků.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Šablona je odeslána po hello Azure Resource Manager, že bude trvat přibližně 40 minutách minut App Service Environment front-end tooapply hello změny.  Například s výchozí velikostí App Service Environment pomocí dvou front-end hello šablony bude trvat přibližně jednu hodinu a toocomplete než 20 minut.  Když běží hello šablony hello App Service Environment nebude možné tooscaled.  

Po dokončení hello šablony aplikace na hello App Service Environment ILB je přístupná prostřednictvím protokolu HTTPS a hello připojení nebude zabezpečené pomocí certifikátu protokolu SSL výchozí hello.  certifikát protokolu SSL výchozí Hello se použije, když aplikace na hello ILB App Service Environment jsou řešit pomocí kombinace názvu aplikace hello plus hello výchozí název hostitele.  Například *https://mycustomapp.internal-contoso.com* využije hello výchozí certifikát SSL pro **.internal contoso.com*.

Ale stejně jako aplikace běžící na hello veřejné víceklientské služby, vývojáři můžou také nakonfigurovat vlastní hostitel názvy pro jednotlivé aplikace a pak nakonfigurujte jedinečný vazby certifikátů SNI SSL pro jednotlivé aplikace.  

## <a name="getting-started"></a>Začínáme
tooget začít s prostředí App Service najdete v části [Úvod tooApp Service Environment](app-service-app-service-environment-intro.md)

Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 

