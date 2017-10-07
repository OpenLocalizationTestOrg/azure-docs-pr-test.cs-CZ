---
title: "aaaWhat je Cloudová služba modelu a balíček | Microsoft Docs"
description: "Popisuje hello model cloudové služby (.csdef, .cscfg) a balíčku (.cspkg) v Azure"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4ce2feb5-0437-496c-98da-1fb6dcb7f59e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 5280cdca4810859b6afdbbe1359fc2fabe871894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-cloud-service-model-and-how-do-i-package-it"></a>Co je model hello cloudové služby a jak ho balíček?
Cloudová služba je vytvořená z tří součástí definice služby hello *(.csdef)*, hello konfigurace služby *(.cscfg)*a balíček služby *(.cspkg)*. Obě hello **ServiceDefinition.csdef** a **ServiceConfig.cscfg** soubory formátu XML a popisu hello struktury hello cloudové služby a jak jsou nakonfigurované; se nazývají hello modelu. Hello **ServicePackage.cspkg** je soubor zip, který se generují z hello **ServiceDefinition.csdef** a mimo jiné obsahuje všechny požadované hello na základě binární závislosti. Azure vytvoří cloudovou službu z obou hello **ServicePackage.cspkg** a hello **ServiceConfig.cscfg**.

Jakmile hello Cloudová služba běží v Azure, můžete ji překonfigurovat prostřednictvím hello **ServiceConfig.cscfg** soubor, ale nelze změnit definici hello.

## <a name="what-would-you-like-tooknow-more-about"></a>Co chcete více informací o tooknow?
* Chci další informace o hello tooknow [ServiceDefinition.csdef](#csdef) a [ServiceConfig.cscfg](#cscfg) soubory.
* Již vědět o, mě [některé příklady](#next-steps) na Co mám nakonfigurovat.
* Chci toocreate hello [ServicePackage.cspkg](#cspkg).
* Používám Visual Studio a chcete...
  * [Vytvoření cloudové služby][vs_create]
  * [Znovu nakonfigurujte stávající cloudovou službu][vs_reconfigure]
  * [Nasazení projektu cloudové služby][vs_deploy]
  * [Vzdálená plocha do instance cloudové služby][remotedesktop]

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
Hello **ServiceDefinition.csdef** souboru určuje hello nastavení, které používají Azure tooconfigure cloudové služby. Hello [Azure schématu definice služby (.csdef souboru)](https://msdn.microsoft.com/library/azure/ee758711.aspx) poskytuje hello povolený formát souboru definice služby. Hello následující příklad ukazuje hello nastavení, které lze definovat pro hello webové a pracovní role:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Můžete se podívat toohello [definice schématu služby](https://msdn.microsoft.com/library/azure/ee758711.aspx) lépe porozumět schématu XML hello použít se zde, ale tady je rychlý vysvětlení některých elementů hello:

**Weby**  
Obsahuje definice hello pro weby nebo webové aplikace, které jsou hostované ve službě IIS7.

**InputEndpoints**  
Obsahuje definice hello pro koncové body, které jsou použity toocontact hello cloudové služby.

**InternalEndpoints**  
Obsahuje definice hello pro koncové body, které jsou používány toocommunicate instancí role mezi sebou.

**ConfigurationSettings**  
Obsahuje definice hello nastavení pro funkce určité role.

**Certifikáty**  
Obsahuje definice hello pro certifikáty, které jsou potřebné pro roli. Hello předchozí příklad kódu ukazuje certifikát, který se používá pro konfiguraci hello Azure připojit.

**LocalResources**  
Obsahuje definice hello pro místní prostředky pro úložiště. Prostředek Místní úložiště je vyhrazené adresáře v systému souborů hello hello virtuálního počítače, ve kterém je spuštěna instance role.

**Importy**  
Obsahuje definice hello importovaných modulů. Hello předchozí příklad kódu ukazuje hello moduly pro připojení ke vzdálené ploše a Azure připojit.

**Spuštění**  
Obsahuje úlohy, které se spustí při spuštění hello role. Hello úlohy jsou definovány v .cmd nebo spustitelný soubor.

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a>Souboru ServiceConfiguration.cscfg
Konfigurace Hello hello nastavení pro cloudové služby je určen podle hodnoty hello v hello **souboru ServiceConfiguration.cscfg** souboru. Zadáte hello počet instancí, který má toodeploy pro každou roli v tomto souboru. konfigurační soubor služby toohello přidání hodnot Hello hello nastavení konfigurace, které jste zadali v souboru definice služby hello. Hello kryptografické otisky pro všechny certifikáty pro správu, které jsou přidruženy hello cloudové služby jsou rovněž přidány toohello souboru. Hello [schéma konfigurace služby Azure (.cscfg souboru)](https://msdn.microsoft.com/library/azure/ee758710.aspx) poskytuje hello povolený formát pro konfigurační soubor služby.

Hello konfigurační soubor služby není spojených s aplikací hello, ale je nahrané tooAzure jako samostatný soubor a použít tooconfigure hello Cloudová služba. Můžete nahrát nový soubor konfigurace služby bez opětovného nasazení cloudové služby. Hello konfigurační hodnoty pro cloudové služby hello lze změnit během spuštěné hello cloudové služby. Hello následující příklad ukazuje hello nastavení konfigurace, které lze definovat pro hello webové a pracovní role:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Můžete se podívat toohello [schéma konfigurace služby](https://msdn.microsoft.com/library/azure/ee758710.aspx) pro lepší pochopení hello schématu XML použít se zde, ale tady je rychlý vysvětlení hello elementů:

**Instance**  
Nakonfiguruje hello počet spuštěných instancí hello role. tooprevent vaše cloudové služby potenciálně stal během upgradu není k dispozici, doporučujeme nasadit více než jednu instanci směřujících webové role. Nasazením více než jednu instanci, jsou dodržujte pokyny toohello v hello [Azure výpočetní služby smlouvou o úrovni (SLA)](http://azure.microsoft.com/support/legal/sla/), což zaručuje 99,95 % externí připojení internetových rolí, pokud dvě nebo více rolí instance jsou nasazeny pro službu.

**ConfigurationSettings**  
Nakonfiguruje nastavení hello hello spuštěné instance role. Název Hello hello `<Setting>` elementy se musí shodovat hello Definice nastavení v souboru definice služby hello.

**Certifikáty**  
Nakonfiguruje hello certifikáty, které používá služba hello. Hello předchozí příklad kódu ukazuje, jak toodefine hello certifikát pro modul RemoteAccess hello. Hello hodnotu hello *kryptografický otisk* musí být nastaven toohello kryptografický otisk certifikátu toouse hello.

<p/>

> [!NOTE]
> Hello kryptografický otisk certifikátu hello lze přidat pomocí textového editoru toohello konfigurační soubor. Nebo hodnota hello lze přidat na hello **certifikáty** kartě hello **vlastnosti** stránku hello role v sadě Visual Studio.
> 
> 

## <a name="defining-ports-for-role-instances"></a>Definování porty pro instance rolí
Azure umožňuje pouze jedna položka tooa bodu webové role. Znamená, že dojde k všechny přenosy přes jednu IP adresu. Vaše weby tooshare port můžete nakonfigurovat tak, že nakonfigurujete hello hostitele záhlaví toodirect hello požadavek toohello správné umístění. Můžete také nakonfigurovat porty toowell známé toolisten vaší aplikace hello IP adresy.

Hello následující příklad ukazuje hello konfigurace pro webové role s webových stránek a webových aplikací. Hello web je nakonfigurován jako hello výchozí položku umístění na portu 80 a jsou nakonfigurované tooreceive požadavky od hlavičku alternativním hostiteli, který se nazývá "mail.mysite.cloudapp.net" hello webové aplikace.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" port="80" />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" hostheader="mail.mysite.cloudapp.net" />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-hello-configuration-of-a-role"></a>Změna konfigurace hello role
Konfigurace hello cloudové služby můžete aktualizovat, když je spuštěná v Azure, bez nutnosti převádět služby hello offline. informace o konfiguraci toochange, můžete buď nahrát nový soubor konfigurace, nebo upravit hello konfigurační soubor v umístění a použijte ho tooyour službou. Hello následující můžete provedeny změny konfigurace toohello služby:

* **Změna hodnot hello nastavení konfigurace**  
  Při konfiguraci nastavení se změní na instanci role můžete zvolte tooapply hello změn při hello instance není online, nebo toorecycle hello instance řádně a použít hello změn při hello instance je offline.
* **Změna topologie služby hello instancí role**  
  Topologie změny neovlivňují spuštěné instance, s výjimkou případů, kdy je odebírána instance. Všechny zbývající instance obecně nemusí toobe recyklované; však můžete toorecycle instance rolí v odpovědi tooa topologie změn.
* **Změna hello kryptografický otisk certifikátu**  
  Certifikát lze aktualizovat pouze pokud je role instance offline. Pokud certifikát přidat, odstranit nebo změnit, pokud je role instance online, Azure řádně trvá hello instance offline tooupdate hello certifikátu a vrátí ji zpět online po dokončení změn hello.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Zpracování změny konfigurace s událostmi služby modulu Runtime
Hello [knihovna Runtime Azure](https://msdn.microsoft.com/library/azure/mt419365.aspx) zahrnuje hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) názvů, který poskytuje třídy pro interakci s hello prostředí Azure z role. Hello [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) třída definuje hello následující události, které jsou vyvolány před a po změně konfigurace:

* **[Změna](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) událostí**  
  K tomu dojde, předtím, než se změna konfigurace hello je použité tooa Zadaná instance role budete prvního tootake dolů instance rolí hello v případě potřeby.
* **[Změnit](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) událostí**  
  Vyskytne se po změně konfigurace hello je použité tooa Zadaná instance role.

> [!NOTE]
> Protože změny certifikátu vždy provést hello instancí role do offline režimu, že není vyvolávání událostí RoleEnvironment.Changing nebo RoleEnvironment.Changed hello.
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
toodeploy aplikace jako cloudové služby v Azure, musíte první aplikace hello balíčku v příslušném formátu hello. Můžete použít hello **CSPack** nástroj příkazového řádku (nainstalované s hello [Azure SDK](https://azure.microsoft.com/downloads/)) soubor balíčku hello toocreate jako alternativní tooVisual Studio.

**CSPack** používá hello obsah hello služby definice Souborová služba a služba konfigurace toodefine hello obsah souboru balíčku hello. **CSPack** vygeneruje soubor balíčku aplikace (.cspkg), můžete nahrát tooAzure pomocí hello [portál Azure](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Ve výchozím nastavení, je hello balíček s názvem `[ServiceDefinitionFileName].cspkg`, ale můžete zadat jiný název pomocí hello `/out` možnost **CSPack**.

**CSPack** se nachází v  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> CSPack.exe (v systému windows) je k dispozici spuštěním hello **Microsoft Azure příkazového řádku** zástupce, který je nainstalován pomocí hello SDK.  
> 
> Spusťte hello CSPack.exe program samostatně toosee dokumentaci o všech možných přepínače hello a příkazy.
> 
> 

<p />

> [!TIP]
> Spustit cloudové služby místně v hello **Microsoft Azure výpočetní emulátor**, použijte hello **/copyonly** možnost. Tato možnost zkopíruje hello binární soubory pro hello aplikace tooa directory rozložení, ze kterého se můžete spustit v emulátoru služby výpočty hello.
> 
> 

### <a name="example-command-toopackage-a-cloud-service"></a>Příklad příkaz toopackage cloudové služby
Hello následující příklad vytvoří balíček aplikace obsahující hello informace pro webové role. příkaz Hello určuje toouse souboru definice služby hello, hello adresáře, kde může být binární soubory najít a názvem souboru balíčku hello hello.

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

Pokud aplikace hello obsahuje webovou roli a roli pracovního procesu, hello následující příkaz se používá:

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

Kde hello proměnné jsou definovány takto:

| Proměnná | Hodnota |
| --- | --- |
| \[DirectoryName\] |Hello podadresáře v adresáři projektu kořenové hello, která obsahuje soubor .csdef hello hello projektu Azure. |
| \[ServiceDefinition\] |Název souboru definice služby hello Hello. Ve výchozím nastavení je tento soubor s názvem ServiceDefinition.csdef. |
| \[Název_výstupního_souboru\] |Název Hello hello vygeneruje soubor balíčku. Obvykle je nastavena toohello název aplikace hello. Pokud není zadán žádný název souboru, balíček aplikace hello je vytvořen jako \[ApplicationName\].cspkg. |
| \[RoleName\] |Název Hello hello role, jak jsou definovány v souboru definice služby hello. |
| \[RoleBinariesDirectory] |umístění Hello hello binární soubory pro roli hello. |
| \[VirtualPath\] |Hello fyzické adresáře pro každý virtuální cestu definovaný v oddílu lokality hello definice služby hello. |
| \[PhysicalPath\] |Hello fyzické adresáře hello obsah pro každý virtuální cestu definovanou v uzlu lokality hello definice služby hello. |
| \[RoleAssemblyName\] |Název Hello hello binárního souboru pro roli hello. |

## <a name="next-steps"></a>Další kroky
Vytváření balíčku cloudové služby a chcete...

* [Instalační program vzdálené plochy pro instanci cloudové služby][remotedesktop]
* [Nasazení projektu cloudové služby][deploy]

Používám Visual Studio a chcete...

* [Vytvořte novou cloudovou službu][vs_create]
* [Znovu nakonfigurujte stávající cloudovou službu][vs_reconfigure]
* [Nasazení projektu cloudové služby][vs_deploy]
* [Instalační program vzdálené plochy pro instanci cloudové služby][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
