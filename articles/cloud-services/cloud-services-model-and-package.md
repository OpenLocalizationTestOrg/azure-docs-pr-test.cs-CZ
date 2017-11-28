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
# <a name="what-is-hello-cloud-service-model-and-how-do-i-package-it"></a><span data-ttu-id="d01dd-103">Co je model hello cloudové služby a jak ho balíček?</span><span class="sxs-lookup"><span data-stu-id="d01dd-103">What is hello Cloud Service model and how do I package it?</span></span>
<span data-ttu-id="d01dd-104">Cloudová služba je vytvořená z tří součástí definice služby hello *(.csdef)*, hello konfigurace služby *(.cscfg)*a balíček služby *(.cspkg)*.</span><span class="sxs-lookup"><span data-stu-id="d01dd-104">A cloud service is created from three components, hello service definition *(.csdef)*, hello service config *(.cscfg)*, and a service package *(.cspkg)*.</span></span> <span data-ttu-id="d01dd-105">Obě hello **ServiceDefinition.csdef** a **ServiceConfig.cscfg** soubory formátu XML a popisu hello struktury hello cloudové služby a jak jsou nakonfigurované; se nazývají hello modelu.</span><span class="sxs-lookup"><span data-stu-id="d01dd-105">Both hello **ServiceDefinition.csdef** and **ServiceConfig.cscfg** files are XML-based and describe hello structure of hello cloud service and how it's configured; collectively called hello model.</span></span> <span data-ttu-id="d01dd-106">Hello **ServicePackage.cspkg** je soubor zip, který se generují z hello **ServiceDefinition.csdef** a mimo jiné obsahuje všechny požadované hello na základě binární závislosti.</span><span class="sxs-lookup"><span data-stu-id="d01dd-106">hello **ServicePackage.cspkg** is a zip file that is generated from hello **ServiceDefinition.csdef** and among other things, contains all hello required binary-based dependencies.</span></span> <span data-ttu-id="d01dd-107">Azure vytvoří cloudovou službu z obou hello **ServicePackage.cspkg** a hello **ServiceConfig.cscfg**.</span><span class="sxs-lookup"><span data-stu-id="d01dd-107">Azure creates a cloud service from both hello **ServicePackage.cspkg** and hello **ServiceConfig.cscfg**.</span></span>

<span data-ttu-id="d01dd-108">Jakmile hello Cloudová služba běží v Azure, můžete ji překonfigurovat prostřednictvím hello **ServiceConfig.cscfg** soubor, ale nelze změnit definici hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-108">Once hello cloud service is running in Azure, you can reconfigure it through hello **ServiceConfig.cscfg** file, but you cannot alter hello definition.</span></span>

## <a name="what-would-you-like-tooknow-more-about"></a><span data-ttu-id="d01dd-109">Co chcete více informací o tooknow?</span><span class="sxs-lookup"><span data-stu-id="d01dd-109">What would you like tooknow more about?</span></span>
* <span data-ttu-id="d01dd-110">Chci další informace o hello tooknow [ServiceDefinition.csdef](#csdef) a [ServiceConfig.cscfg](#cscfg) soubory.</span><span class="sxs-lookup"><span data-stu-id="d01dd-110">I want tooknow more about hello [ServiceDefinition.csdef](#csdef) and [ServiceConfig.cscfg](#cscfg) files.</span></span>
* <span data-ttu-id="d01dd-111">Již vědět o, mě [některé příklady](#next-steps) na Co mám nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="d01dd-111">I already know about that, give me [some examples](#next-steps) on what I can configure.</span></span>
* <span data-ttu-id="d01dd-112">Chci toocreate hello [ServicePackage.cspkg](#cspkg).</span><span class="sxs-lookup"><span data-stu-id="d01dd-112">I want toocreate hello [ServicePackage.cspkg](#cspkg).</span></span>
* <span data-ttu-id="d01dd-113">Používám Visual Studio a chcete...</span><span class="sxs-lookup"><span data-stu-id="d01dd-113">I am using Visual Studio and I want to...</span></span>
  * <span data-ttu-id="d01dd-114">[Vytvoření cloudové služby][vs_create]</span><span class="sxs-lookup"><span data-stu-id="d01dd-114">[Create a cloud service][vs_create]</span></span>
  * <span data-ttu-id="d01dd-115">[Znovu nakonfigurujte stávající cloudovou službu][vs_reconfigure]</span><span class="sxs-lookup"><span data-stu-id="d01dd-115">[Reconfigure an existing cloud service][vs_reconfigure]</span></span>
  * <span data-ttu-id="d01dd-116">[Nasazení projektu cloudové služby][vs_deploy]</span><span class="sxs-lookup"><span data-stu-id="d01dd-116">[Deploy a Cloud Service project][vs_deploy]</span></span>
  * <span data-ttu-id="d01dd-117">[Vzdálená plocha do instance cloudové služby][remotedesktop]</span><span class="sxs-lookup"><span data-stu-id="d01dd-117">[Remote desktop into a cloud service instance][remotedesktop]</span></span>

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a><span data-ttu-id="d01dd-118">ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="d01dd-118">ServiceDefinition.csdef</span></span>
<span data-ttu-id="d01dd-119">Hello **ServiceDefinition.csdef** souboru určuje hello nastavení, které používají Azure tooconfigure cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="d01dd-119">hello **ServiceDefinition.csdef** file specifies hello settings that are used by Azure tooconfigure a cloud service.</span></span> <span data-ttu-id="d01dd-120">Hello [Azure schématu definice služby (.csdef souboru)](https://msdn.microsoft.com/library/azure/ee758711.aspx) poskytuje hello povolený formát souboru definice služby.</span><span class="sxs-lookup"><span data-stu-id="d01dd-120">hello [Azure Service Definition Schema (.csdef File)](https://msdn.microsoft.com/library/azure/ee758711.aspx) provides hello allowable format for a service definition file.</span></span> <span data-ttu-id="d01dd-121">Hello následující příklad ukazuje hello nastavení, které lze definovat pro hello webové a pracovní role:</span><span class="sxs-lookup"><span data-stu-id="d01dd-121">hello following example shows hello settings that can be defined for hello Web and Worker roles:</span></span>

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

<span data-ttu-id="d01dd-122">Můžete se podívat toohello [definice schématu služby](https://msdn.microsoft.com/library/azure/ee758711.aspx) lépe porozumět schématu XML hello použít se zde, ale tady je rychlý vysvětlení některých elementů hello:</span><span class="sxs-lookup"><span data-stu-id="d01dd-122">You can refer toohello [Service Definition Schema](https://msdn.microsoft.com/library/azure/ee758711.aspx) for a better understanding of hello XML schema used here, however, here is a quick explanation of some of hello elements:</span></span>

<span data-ttu-id="d01dd-123">**Weby**</span><span class="sxs-lookup"><span data-stu-id="d01dd-123">**Sites**</span></span>  
<span data-ttu-id="d01dd-124">Obsahuje definice hello pro weby nebo webové aplikace, které jsou hostované ve službě IIS7.</span><span class="sxs-lookup"><span data-stu-id="d01dd-124">Contains hello definitions for websites or web applications that are hosted in IIS7.</span></span>

<span data-ttu-id="d01dd-125">**InputEndpoints**</span><span class="sxs-lookup"><span data-stu-id="d01dd-125">**InputEndpoints**</span></span>  
<span data-ttu-id="d01dd-126">Obsahuje definice hello pro koncové body, které jsou použity toocontact hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="d01dd-126">Contains hello definitions for endpoints that are used toocontact hello cloud service.</span></span>

<span data-ttu-id="d01dd-127">**InternalEndpoints**</span><span class="sxs-lookup"><span data-stu-id="d01dd-127">**InternalEndpoints**</span></span>  
<span data-ttu-id="d01dd-128">Obsahuje definice hello pro koncové body, které jsou používány toocommunicate instancí role mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="d01dd-128">Contains hello definitions for endpoints that are used by role instances toocommunicate with each other.</span></span>

<span data-ttu-id="d01dd-129">**ConfigurationSettings**</span><span class="sxs-lookup"><span data-stu-id="d01dd-129">**ConfigurationSettings**</span></span>  
<span data-ttu-id="d01dd-130">Obsahuje definice hello nastavení pro funkce určité role.</span><span class="sxs-lookup"><span data-stu-id="d01dd-130">Contains hello setting definitions for features of a specific role.</span></span>

<span data-ttu-id="d01dd-131">**Certifikáty**</span><span class="sxs-lookup"><span data-stu-id="d01dd-131">**Certificates**</span></span>  
<span data-ttu-id="d01dd-132">Obsahuje definice hello pro certifikáty, které jsou potřebné pro roli.</span><span class="sxs-lookup"><span data-stu-id="d01dd-132">Contains hello definitions for certificates that are needed for a role.</span></span> <span data-ttu-id="d01dd-133">Hello předchozí příklad kódu ukazuje certifikát, který se používá pro konfiguraci hello Azure připojit.</span><span class="sxs-lookup"><span data-stu-id="d01dd-133">hello previous code example shows a certificate that is used for hello configuration of Azure Connect.</span></span>

<span data-ttu-id="d01dd-134">**LocalResources**</span><span class="sxs-lookup"><span data-stu-id="d01dd-134">**LocalResources**</span></span>  
<span data-ttu-id="d01dd-135">Obsahuje definice hello pro místní prostředky pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="d01dd-135">Contains hello definitions for local storage resources.</span></span> <span data-ttu-id="d01dd-136">Prostředek Místní úložiště je vyhrazené adresáře v systému souborů hello hello virtuálního počítače, ve kterém je spuštěna instance role.</span><span class="sxs-lookup"><span data-stu-id="d01dd-136">A local storage resource is a reserved directory on hello file system of hello virtual machine in which an instance of a role is running.</span></span>

<span data-ttu-id="d01dd-137">**Importy**</span><span class="sxs-lookup"><span data-stu-id="d01dd-137">**Imports**</span></span>  
<span data-ttu-id="d01dd-138">Obsahuje definice hello importovaných modulů.</span><span class="sxs-lookup"><span data-stu-id="d01dd-138">Contains hello definitions for imported modules.</span></span> <span data-ttu-id="d01dd-139">Hello předchozí příklad kódu ukazuje hello moduly pro připojení ke vzdálené ploše a Azure připojit.</span><span class="sxs-lookup"><span data-stu-id="d01dd-139">hello previous code example shows hello modules for Remote Desktop Connection and Azure Connect.</span></span>

<span data-ttu-id="d01dd-140">**Spuštění**</span><span class="sxs-lookup"><span data-stu-id="d01dd-140">**Startup**</span></span>  
<span data-ttu-id="d01dd-141">Obsahuje úlohy, které se spustí při spuštění hello role.</span><span class="sxs-lookup"><span data-stu-id="d01dd-141">Contains tasks that are run when hello role starts.</span></span> <span data-ttu-id="d01dd-142">Hello úlohy jsou definovány v .cmd nebo spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="d01dd-142">hello tasks are defined in a .cmd or executable file.</span></span>

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a><span data-ttu-id="d01dd-143">Souboru ServiceConfiguration.cscfg</span><span class="sxs-lookup"><span data-stu-id="d01dd-143">ServiceConfiguration.cscfg</span></span>
<span data-ttu-id="d01dd-144">Konfigurace Hello hello nastavení pro cloudové služby je určen podle hodnoty hello v hello **souboru ServiceConfiguration.cscfg** souboru.</span><span class="sxs-lookup"><span data-stu-id="d01dd-144">hello configuration of hello settings for your cloud service is determined by hello values in hello **ServiceConfiguration.cscfg** file.</span></span> <span data-ttu-id="d01dd-145">Zadáte hello počet instancí, který má toodeploy pro každou roli v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="d01dd-145">You specify hello number of instances that you want toodeploy for each role in this file.</span></span> <span data-ttu-id="d01dd-146">konfigurační soubor služby toohello přidání hodnot Hello hello nastavení konfigurace, které jste zadali v souboru definice služby hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-146">hello values for hello configuration settings that you defined in hello service definition file are added toohello service configuration file.</span></span> <span data-ttu-id="d01dd-147">Hello kryptografické otisky pro všechny certifikáty pro správu, které jsou přidruženy hello cloudové služby jsou rovněž přidány toohello souboru.</span><span class="sxs-lookup"><span data-stu-id="d01dd-147">hello thumbprints for any management certificates that are associated with hello cloud service are also added toohello file.</span></span> <span data-ttu-id="d01dd-148">Hello [schéma konfigurace služby Azure (.cscfg souboru)](https://msdn.microsoft.com/library/azure/ee758710.aspx) poskytuje hello povolený formát pro konfigurační soubor služby.</span><span class="sxs-lookup"><span data-stu-id="d01dd-148">hello [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx) provides hello allowable format for a service configuration file.</span></span>

<span data-ttu-id="d01dd-149">Hello konfigurační soubor služby není spojených s aplikací hello, ale je nahrané tooAzure jako samostatný soubor a použít tooconfigure hello Cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="d01dd-149">hello service configuration file is not packaged with hello application, but is uploaded tooAzure as a separate file and is used tooconfigure hello cloud service.</span></span> <span data-ttu-id="d01dd-150">Můžete nahrát nový soubor konfigurace služby bez opětovného nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="d01dd-150">You can upload a new service configuration file without redeploying your cloud service.</span></span> <span data-ttu-id="d01dd-151">Hello konfigurační hodnoty pro cloudové služby hello lze změnit během spuštěné hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="d01dd-151">hello configuration values for hello cloud service can be changed while hello cloud service is running.</span></span> <span data-ttu-id="d01dd-152">Hello následující příklad ukazuje hello nastavení konfigurace, které lze definovat pro hello webové a pracovní role:</span><span class="sxs-lookup"><span data-stu-id="d01dd-152">hello following example shows hello configuration settings that can be defined for hello Web and Worker roles:</span></span>

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

<span data-ttu-id="d01dd-153">Můžete se podívat toohello [schéma konfigurace služby](https://msdn.microsoft.com/library/azure/ee758710.aspx) pro lepší pochopení hello schématu XML použít se zde, ale tady je rychlý vysvětlení hello elementů:</span><span class="sxs-lookup"><span data-stu-id="d01dd-153">You can refer toohello [Service Configuration Schema](https://msdn.microsoft.com/library/azure/ee758710.aspx) for better understanding hello XML schema used here, however, here is a quick explanation of hello elements:</span></span>

<span data-ttu-id="d01dd-154">**Instance**</span><span class="sxs-lookup"><span data-stu-id="d01dd-154">**Instances**</span></span>  
<span data-ttu-id="d01dd-155">Nakonfiguruje hello počet spuštěných instancí hello role.</span><span class="sxs-lookup"><span data-stu-id="d01dd-155">Configures hello number of running instances for hello role.</span></span> <span data-ttu-id="d01dd-156">tooprevent vaše cloudové služby potenciálně stal během upgradu není k dispozici, doporučujeme nasadit více než jednu instanci směřujících webové role.</span><span class="sxs-lookup"><span data-stu-id="d01dd-156">tooprevent your cloud service from potentially becoming unavailable during upgrades, it is recommended that you deploy more than one instance of your web-facing roles.</span></span> <span data-ttu-id="d01dd-157">Nasazením více než jednu instanci, jsou dodržujte pokyny toohello v hello [Azure výpočetní služby smlouvou o úrovni (SLA)](http://azure.microsoft.com/support/legal/sla/), což zaručuje 99,95 % externí připojení internetových rolí, pokud dvě nebo více rolí instance jsou nasazeny pro službu.</span><span class="sxs-lookup"><span data-stu-id="d01dd-157">By deploying more than one instance, you are adhering toohello guidelines in hello [Azure Compute Service Level Agreement (SLA)](http://azure.microsoft.com/support/legal/sla/), which guarantees 99.95% external connectivity for Internet-facing roles when two or more role instances are deployed for a service.</span></span>

<span data-ttu-id="d01dd-158">**ConfigurationSettings**</span><span class="sxs-lookup"><span data-stu-id="d01dd-158">**ConfigurationSettings**</span></span>  
<span data-ttu-id="d01dd-159">Nakonfiguruje nastavení hello hello spuštěné instance role.</span><span class="sxs-lookup"><span data-stu-id="d01dd-159">Configures hello settings for hello running instances for a role.</span></span> <span data-ttu-id="d01dd-160">Název Hello hello `<Setting>` elementy se musí shodovat hello Definice nastavení v souboru definice služby hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-160">hello name of hello `<Setting>` elements must match hello setting definitions in hello service definition file.</span></span>

<span data-ttu-id="d01dd-161">**Certifikáty**</span><span class="sxs-lookup"><span data-stu-id="d01dd-161">**Certificates**</span></span>  
<span data-ttu-id="d01dd-162">Nakonfiguruje hello certifikáty, které používá služba hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-162">Configures hello certificates that are used by hello service.</span></span> <span data-ttu-id="d01dd-163">Hello předchozí příklad kódu ukazuje, jak toodefine hello certifikát pro modul RemoteAccess hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-163">hello previous code example shows how toodefine hello certificate for hello RemoteAccess module.</span></span> <span data-ttu-id="d01dd-164">Hello hodnotu hello *kryptografický otisk* musí být nastaven toohello kryptografický otisk certifikátu toouse hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-164">hello value of hello *thumbprint* attribute must be set toohello thumbprint of hello certificate toouse.</span></span>

<p/>

> [!NOTE]
> <span data-ttu-id="d01dd-165">Hello kryptografický otisk certifikátu hello lze přidat pomocí textového editoru toohello konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="d01dd-165">hello thumbprint for hello certificate can be added toohello configuration file by using a text editor.</span></span> <span data-ttu-id="d01dd-166">Nebo hodnota hello lze přidat na hello **certifikáty** kartě hello **vlastnosti** stránku hello role v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d01dd-166">Or, hello value can be added on hello **Certificates** tab of hello **Properties** page of hello role in Visual Studio.</span></span>
> 
> 

## <a name="defining-ports-for-role-instances"></a><span data-ttu-id="d01dd-167">Definování porty pro instance rolí</span><span class="sxs-lookup"><span data-stu-id="d01dd-167">Defining ports for role instances</span></span>
<span data-ttu-id="d01dd-168">Azure umožňuje pouze jedna položka tooa bodu webové role.</span><span class="sxs-lookup"><span data-stu-id="d01dd-168">Azure allows only one entry point tooa web role.</span></span> <span data-ttu-id="d01dd-169">Znamená, že dojde k všechny přenosy přes jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d01dd-169">Meaning that all traffic occurs through one IP address.</span></span> <span data-ttu-id="d01dd-170">Vaše weby tooshare port můžete nakonfigurovat tak, že nakonfigurujete hello hostitele záhlaví toodirect hello požadavek toohello správné umístění.</span><span class="sxs-lookup"><span data-stu-id="d01dd-170">You can configure your websites tooshare a port by configuring hello host header toodirect hello request toohello correct location.</span></span> <span data-ttu-id="d01dd-171">Můžete také nakonfigurovat porty toowell známé toolisten vaší aplikace hello IP adresy.</span><span class="sxs-lookup"><span data-stu-id="d01dd-171">You can also configure your applications toolisten toowell-known ports on hello IP address.</span></span>

<span data-ttu-id="d01dd-172">Hello následující příklad ukazuje hello konfigurace pro webové role s webových stránek a webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="d01dd-172">hello following sample shows hello configuration for a web role with a website and web application.</span></span> <span data-ttu-id="d01dd-173">Hello web je nakonfigurován jako hello výchozí položku umístění na portu 80 a jsou nakonfigurované tooreceive požadavky od hlavičku alternativním hostiteli, který se nazývá "mail.mysite.cloudapp.net" hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d01dd-173">hello website is configured as hello default entry location on port 80, and hello web applications are configured tooreceive requests from an alternate host header that is called “mail.mysite.cloudapp.net”.</span></span>

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


## <a name="changing-hello-configuration-of-a-role"></a><span data-ttu-id="d01dd-174">Změna konfigurace hello role</span><span class="sxs-lookup"><span data-stu-id="d01dd-174">Changing hello configuration of a role</span></span>
<span data-ttu-id="d01dd-175">Konfigurace hello cloudové služby můžete aktualizovat, když je spuštěná v Azure, bez nutnosti převádět služby hello offline.</span><span class="sxs-lookup"><span data-stu-id="d01dd-175">You can update hello configuration of your cloud service while it is running in Azure, without taking hello service offline.</span></span> <span data-ttu-id="d01dd-176">informace o konfiguraci toochange, můžete buď nahrát nový soubor konfigurace, nebo upravit hello konfigurační soubor v umístění a použijte ho tooyour službou.</span><span class="sxs-lookup"><span data-stu-id="d01dd-176">toochange configuration information, you can either upload a new configuration file, or edit hello configuration file in place and apply it tooyour running service.</span></span> <span data-ttu-id="d01dd-177">Hello následující můžete provedeny změny konfigurace toohello služby:</span><span class="sxs-lookup"><span data-stu-id="d01dd-177">hello following changes can be made toohello configuration of a service:</span></span>

* <span data-ttu-id="d01dd-178">**Změna hodnot hello nastavení konfigurace**</span><span class="sxs-lookup"><span data-stu-id="d01dd-178">**Changing hello values of configuration settings**</span></span>  
  <span data-ttu-id="d01dd-179">Při konfiguraci nastavení se změní na instanci role můžete zvolte tooapply hello změn při hello instance není online, nebo toorecycle hello instance řádně a použít hello změn při hello instance je offline.</span><span class="sxs-lookup"><span data-stu-id="d01dd-179">When a configuration setting changes, a role instance can choose tooapply hello change while hello instance is online, or toorecycle hello instance gracefully and apply hello change while hello instance is offline.</span></span>
* <span data-ttu-id="d01dd-180">**Změna topologie služby hello instancí role**</span><span class="sxs-lookup"><span data-stu-id="d01dd-180">**Changing hello service topology of role instances**</span></span>  
  <span data-ttu-id="d01dd-181">Topologie změny neovlivňují spuštěné instance, s výjimkou případů, kdy je odebírána instance.</span><span class="sxs-lookup"><span data-stu-id="d01dd-181">Topology changes do not affect running instances, except where an instance is being removed.</span></span> <span data-ttu-id="d01dd-182">Všechny zbývající instance obecně nemusí toobe recyklované; však můžete toorecycle instance rolí v odpovědi tooa topologie změn.</span><span class="sxs-lookup"><span data-stu-id="d01dd-182">All remaining instances generally do not need toobe recycled; however, you can choose toorecycle role instances in response tooa topology change.</span></span>
* <span data-ttu-id="d01dd-183">**Změna hello kryptografický otisk certifikátu**</span><span class="sxs-lookup"><span data-stu-id="d01dd-183">**Changing hello certificate thumbprint**</span></span>  
  <span data-ttu-id="d01dd-184">Certifikát lze aktualizovat pouze pokud je role instance offline.</span><span class="sxs-lookup"><span data-stu-id="d01dd-184">You can only update a certificate when a role instance is offline.</span></span> <span data-ttu-id="d01dd-185">Pokud certifikát přidat, odstranit nebo změnit, pokud je role instance online, Azure řádně trvá hello instance offline tooupdate hello certifikátu a vrátí ji zpět online po dokončení změn hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-185">If a certificate is added, deleted, or changed while a role instance is online, Azure gracefully takes hello instance offline tooupdate hello certificate and bring it back online after hello change is complete.</span></span>

### <a name="handling-configuration-changes-with-service-runtime-events"></a><span data-ttu-id="d01dd-186">Zpracování změny konfigurace s událostmi služby modulu Runtime</span><span class="sxs-lookup"><span data-stu-id="d01dd-186">Handling configuration changes with Service Runtime Events</span></span>
<span data-ttu-id="d01dd-187">Hello [knihovna Runtime Azure](https://msdn.microsoft.com/library/azure/mt419365.aspx) zahrnuje hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) názvů, který poskytuje třídy pro interakci s hello prostředí Azure z role.</span><span class="sxs-lookup"><span data-stu-id="d01dd-187">hello [Azure Runtime Library](https://msdn.microsoft.com/library/azure/mt419365.aspx) includes hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) namespace, which provides classes for interacting with hello Azure environment from a role.</span></span> <span data-ttu-id="d01dd-188">Hello [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) třída definuje hello následující události, které jsou vyvolány před a po změně konfigurace:</span><span class="sxs-lookup"><span data-stu-id="d01dd-188">hello [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) class defines hello following events that are raised before and after a configuration change:</span></span>

* <span data-ttu-id="d01dd-189">**[Změna](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) událostí**</span><span class="sxs-lookup"><span data-stu-id="d01dd-189">**[Changing](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) event**</span></span>  
  <span data-ttu-id="d01dd-190">K tomu dojde, předtím, než se změna konfigurace hello je použité tooa Zadaná instance role budete prvního tootake dolů instance rolí hello v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="d01dd-190">This occurs before hello configuration change is applied tooa specified instance of a role giving you a chance tootake down hello role instances if necessary.</span></span>
* <span data-ttu-id="d01dd-191">**[Změnit](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) událostí**</span><span class="sxs-lookup"><span data-stu-id="d01dd-191">**[Changed](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) event**</span></span>  
  <span data-ttu-id="d01dd-192">Vyskytne se po změně konfigurace hello je použité tooa Zadaná instance role.</span><span class="sxs-lookup"><span data-stu-id="d01dd-192">Occurs after hello configuration change is applied tooa specified instance of a role.</span></span>

> [!NOTE]
> <span data-ttu-id="d01dd-193">Protože změny certifikátu vždy provést hello instancí role do offline režimu, že není vyvolávání událostí RoleEnvironment.Changing nebo RoleEnvironment.Changed hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-193">Because certificate changes always take hello instances of a role offline, they do not raise hello RoleEnvironment.Changing or RoleEnvironment.Changed events.</span></span>
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a><span data-ttu-id="d01dd-194">ServicePackage.cspkg</span><span class="sxs-lookup"><span data-stu-id="d01dd-194">ServicePackage.cspkg</span></span>
<span data-ttu-id="d01dd-195">toodeploy aplikace jako cloudové služby v Azure, musíte první aplikace hello balíčku v příslušném formátu hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-195">toodeploy an application as a cloud service in Azure, you must first package hello application in hello appropriate format.</span></span> <span data-ttu-id="d01dd-196">Můžete použít hello **CSPack** nástroj příkazového řádku (nainstalované s hello [Azure SDK](https://azure.microsoft.com/downloads/)) soubor balíčku hello toocreate jako alternativní tooVisual Studio.</span><span class="sxs-lookup"><span data-stu-id="d01dd-196">You can use hello **CSPack** command-line tool (installed with hello [Azure SDK](https://azure.microsoft.com/downloads/)) toocreate hello package file as an alternative tooVisual Studio.</span></span>

<span data-ttu-id="d01dd-197">**CSPack** používá hello obsah hello služby definice Souborová služba a služba konfigurace toodefine hello obsah souboru balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-197">**CSPack** uses hello contents of hello service definition file and service configuration file toodefine hello contents of hello package.</span></span> <span data-ttu-id="d01dd-198">**CSPack** vygeneruje soubor balíčku aplikace (.cspkg), můžete nahrát tooAzure pomocí hello [portál Azure](cloud-services-how-to-create-deploy-portal.md#create-and-deploy).</span><span class="sxs-lookup"><span data-stu-id="d01dd-198">**CSPack** generates an application package file (.cspkg) that you can upload tooAzure by using hello [Azure portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy).</span></span> <span data-ttu-id="d01dd-199">Ve výchozím nastavení, je hello balíček s názvem `[ServiceDefinitionFileName].cspkg`, ale můžete zadat jiný název pomocí hello `/out` možnost **CSPack**.</span><span class="sxs-lookup"><span data-stu-id="d01dd-199">By default, hello package is named `[ServiceDefinitionFileName].cspkg`, but you can specify a different name by using hello `/out` option of **CSPack**.</span></span>

<span data-ttu-id="d01dd-200">**CSPack** se nachází v</span><span class="sxs-lookup"><span data-stu-id="d01dd-200">**CSPack** is located at</span></span>  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> <span data-ttu-id="d01dd-201">CSPack.exe (v systému windows) je k dispozici spuštěním hello **Microsoft Azure příkazového řádku** zástupce, který je nainstalován pomocí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="d01dd-201">CSPack.exe (on windows) is available by running hello **Microsoft Azure Command Prompt** shortcut that is installed with hello SDK.</span></span>  
> 
> <span data-ttu-id="d01dd-202">Spusťte hello CSPack.exe program samostatně toosee dokumentaci o všech možných přepínače hello a příkazy.</span><span class="sxs-lookup"><span data-stu-id="d01dd-202">Run hello CSPack.exe program by itself toosee documentation about all hello possible switches and commands.</span></span>
> 
> 

<p />

> [!TIP]
> <span data-ttu-id="d01dd-203">Spustit cloudové služby místně v hello **Microsoft Azure výpočetní emulátor**, použijte hello **/copyonly** možnost.</span><span class="sxs-lookup"><span data-stu-id="d01dd-203">Run your cloud service locally in hello **Microsoft Azure Compute Emulator**, use hello **/copyonly** option.</span></span> <span data-ttu-id="d01dd-204">Tato možnost zkopíruje hello binární soubory pro hello aplikace tooa directory rozložení, ze kterého se můžete spustit v emulátoru služby výpočty hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-204">This option copies hello binary files for hello application tooa directory layout from which they can be run in hello compute emulator.</span></span>
> 
> 

### <a name="example-command-toopackage-a-cloud-service"></a><span data-ttu-id="d01dd-205">Příklad příkaz toopackage cloudové služby</span><span class="sxs-lookup"><span data-stu-id="d01dd-205">Example command toopackage a cloud service</span></span>
<span data-ttu-id="d01dd-206">Hello následující příklad vytvoří balíček aplikace obsahující hello informace pro webové role.</span><span class="sxs-lookup"><span data-stu-id="d01dd-206">hello following example creates an application package that contains hello information for a web role.</span></span> <span data-ttu-id="d01dd-207">příkaz Hello určuje toouse souboru definice služby hello, hello adresáře, kde může být binární soubory najít a názvem souboru balíčku hello hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-207">hello command specifies hello service definition file toouse, hello directory where binary files can be found, and hello name of hello package file.</span></span>

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

<span data-ttu-id="d01dd-208">Pokud aplikace hello obsahuje webovou roli a roli pracovního procesu, hello následující příkaz se používá:</span><span class="sxs-lookup"><span data-stu-id="d01dd-208">If hello application contains both a web role and a worker role, hello following command is used:</span></span>

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

<span data-ttu-id="d01dd-209">Kde hello proměnné jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="d01dd-209">Where hello variables are defined as follows:</span></span>

| <span data-ttu-id="d01dd-210">Proměnná</span><span class="sxs-lookup"><span data-stu-id="d01dd-210">Variable</span></span> | <span data-ttu-id="d01dd-211">Hodnota</span><span class="sxs-lookup"><span data-stu-id="d01dd-211">Value</span></span> |
| --- | --- |
| <span data-ttu-id="d01dd-212">\[DirectoryName\]</span><span class="sxs-lookup"><span data-stu-id="d01dd-212">\[DirectoryName\]</span></span> |<span data-ttu-id="d01dd-213">Hello podadresáře v adresáři projektu kořenové hello, která obsahuje soubor .csdef hello hello projektu Azure.</span><span class="sxs-lookup"><span data-stu-id="d01dd-213">hello subdirectory under hello root project directory that contains hello .csdef file of hello Azure project.</span></span> |
| <span data-ttu-id="d01dd-214">\[ServiceDefinition\]</span><span class="sxs-lookup"><span data-stu-id="d01dd-214">\[ServiceDefinition\]</span></span> |<span data-ttu-id="d01dd-215">Název souboru definice služby hello Hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-215">hello name of hello service definition file.</span></span> <span data-ttu-id="d01dd-216">Ve výchozím nastavení je tento soubor s názvem ServiceDefinition.csdef.</span><span class="sxs-lookup"><span data-stu-id="d01dd-216">By default, this file is named ServiceDefinition.csdef.</span></span> |
| <span data-ttu-id="d01dd-217">\[Název_výstupního_souboru\]</span><span class="sxs-lookup"><span data-stu-id="d01dd-217">\[OutputFileName\]</span></span> |<span data-ttu-id="d01dd-218">Název Hello hello vygeneruje soubor balíčku.</span><span class="sxs-lookup"><span data-stu-id="d01dd-218">hello name for hello generated package file.</span></span> <span data-ttu-id="d01dd-219">Obvykle je nastavena toohello název aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-219">Typically, this is set toohello name of hello application.</span></span> <span data-ttu-id="d01dd-220">Pokud není zadán žádný název souboru, balíček aplikace hello je vytvořen jako \[ApplicationName\].cspkg.</span><span class="sxs-lookup"><span data-stu-id="d01dd-220">If no file name is specified, hello application package is created as \[ApplicationName\].cspkg.</span></span> |
| <span data-ttu-id="d01dd-221">\[RoleName\]</span><span class="sxs-lookup"><span data-stu-id="d01dd-221">\[RoleName\]</span></span> |<span data-ttu-id="d01dd-222">Název Hello hello role, jak jsou definovány v souboru definice služby hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-222">hello name of hello role as defined in hello service definition file.</span></span> |
| <span data-ttu-id="d01dd-223">\[RoleBinariesDirectory]</span><span class="sxs-lookup"><span data-stu-id="d01dd-223">\[RoleBinariesDirectory]</span></span> |<span data-ttu-id="d01dd-224">umístění Hello hello binární soubory pro roli hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-224">hello location of hello binary files for hello role.</span></span> |
| <span data-ttu-id="d01dd-225">\[VirtualPath\]</span><span class="sxs-lookup"><span data-stu-id="d01dd-225">\[VirtualPath\]</span></span> |<span data-ttu-id="d01dd-226">Hello fyzické adresáře pro každý virtuální cestu definovaný v oddílu lokality hello definice služby hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-226">hello physical directories for each virtual path defined in hello Sites section of hello service definition.</span></span> |
| <span data-ttu-id="d01dd-227">\[PhysicalPath\]</span><span class="sxs-lookup"><span data-stu-id="d01dd-227">\[PhysicalPath\]</span></span> |<span data-ttu-id="d01dd-228">Hello fyzické adresáře hello obsah pro každý virtuální cestu definovanou v uzlu lokality hello definice služby hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-228">hello physical directories of hello contents for each virtual path defined in hello site node of hello service definition.</span></span> |
| <span data-ttu-id="d01dd-229">\[RoleAssemblyName\]</span><span class="sxs-lookup"><span data-stu-id="d01dd-229">\[RoleAssemblyName\]</span></span> |<span data-ttu-id="d01dd-230">Název Hello hello binárního souboru pro roli hello.</span><span class="sxs-lookup"><span data-stu-id="d01dd-230">hello name of hello binary file for hello role.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d01dd-231">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d01dd-231">Next steps</span></span>
<span data-ttu-id="d01dd-232">Vytváření balíčku cloudové služby a chcete...</span><span class="sxs-lookup"><span data-stu-id="d01dd-232">I'm creating a cloud service package and I want to...</span></span>

* <span data-ttu-id="d01dd-233">[Instalační program vzdálené plochy pro instanci cloudové služby][remotedesktop]</span><span class="sxs-lookup"><span data-stu-id="d01dd-233">[Setup remote desktop for a cloud service instance][remotedesktop]</span></span>
* <span data-ttu-id="d01dd-234">[Nasazení projektu cloudové služby][deploy]</span><span class="sxs-lookup"><span data-stu-id="d01dd-234">[Deploy a Cloud Service project][deploy]</span></span>

<span data-ttu-id="d01dd-235">Používám Visual Studio a chcete...</span><span class="sxs-lookup"><span data-stu-id="d01dd-235">I am using Visual Studio and I want to...</span></span>

* <span data-ttu-id="d01dd-236">[Vytvořte novou cloudovou službu][vs_create]</span><span class="sxs-lookup"><span data-stu-id="d01dd-236">[Create a new cloud service][vs_create]</span></span>
* <span data-ttu-id="d01dd-237">[Znovu nakonfigurujte stávající cloudovou službu][vs_reconfigure]</span><span class="sxs-lookup"><span data-stu-id="d01dd-237">[Reconfigure an existing cloud service][vs_reconfigure]</span></span>
* <span data-ttu-id="d01dd-238">[Nasazení projektu cloudové služby][vs_deploy]</span><span class="sxs-lookup"><span data-stu-id="d01dd-238">[Deploy a Cloud Service project][vs_deploy]</span></span>
* <span data-ttu-id="d01dd-239">[Instalační program vzdálené plochy pro instanci cloudové služby][vs_remote]</span><span class="sxs-lookup"><span data-stu-id="d01dd-239">[Setup remote desktop for a cloud service instance][vs_remote]</span></span>

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
