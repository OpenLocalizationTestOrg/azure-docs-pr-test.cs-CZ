---
title: "Co je Cloudová služba modelu a balíček | Microsoft Docs"
description: "Popisuje model cloudové služby (.csdef, .cscfg) a balíčku (.cspkg) v Azure"
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
ms.openlocfilehash: 21fbdbc4c24440c6fbbd7487cfbb2e0a3140aa96
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a><span data-ttu-id="c9ec1-103">Co je Cloudová služba modelu a jak ho balíček?</span><span class="sxs-lookup"><span data-stu-id="c9ec1-103">What is the Cloud Service model and how do I package it?</span></span>
<span data-ttu-id="c9ec1-104">Cloudová služba je vytvořená z tří součástí definice služby *(.csdef)*, konfigurace služby *(.cscfg)*a balíček služby *(.cspkg)*.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-104">A cloud service is created from three components, the service definition *(.csdef)*, the service config *(.cscfg)*, and a service package *(.cspkg)*.</span></span> <span data-ttu-id="c9ec1-105">Obě **ServiceDefinition.csdef** a **ServiceConfig.cscfg** soubory formátu XML a popisují strukturu cloudové služby a jak jsou nakonfigurované; se nazývají modelu.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-105">Both the **ServiceDefinition.csdef** and **ServiceConfig.cscfg** files are XML-based and describe the structure of the cloud service and how it's configured; collectively called the model.</span></span> <span data-ttu-id="c9ec1-106">**ServicePackage.cspkg** je soubor zip, který se generují z **ServiceDefinition.csdef** a mimo jiné obsahuje všechny požadované závislosti na základě binární.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-106">The **ServicePackage.cspkg** is a zip file that is generated from the **ServiceDefinition.csdef** and among other things, contains all the required binary-based dependencies.</span></span> <span data-ttu-id="c9ec1-107">Azure vytvoří cloudové služby i **ServicePackage.cspkg** a **ServiceConfig.cscfg**.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-107">Azure creates a cloud service from both the **ServicePackage.cspkg** and the **ServiceConfig.cscfg**.</span></span>

<span data-ttu-id="c9ec1-108">Jakmile Cloudová služba běží v Azure, můžete ji prostřednictvím překonfigurovat **ServiceConfig.cscfg** soubor, ale nelze změnit definici.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-108">Once the cloud service is running in Azure, you can reconfigure it through the **ServiceConfig.cscfg** file, but you cannot alter the definition.</span></span>

## <a name="what-would-you-like-to-know-more-about"></a><span data-ttu-id="c9ec1-109">Co chcete vědět více o?</span><span class="sxs-lookup"><span data-stu-id="c9ec1-109">What would you like to know more about?</span></span>
* <span data-ttu-id="c9ec1-110">Chci vědět více o [ServiceDefinition.csdef](#csdef) a [ServiceConfig.cscfg](#cscfg) soubory.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-110">I want to know more about the [ServiceDefinition.csdef](#csdef) and [ServiceConfig.cscfg](#cscfg) files.</span></span>
* <span data-ttu-id="c9ec1-111">Již vědět o, mě [některé příklady](#next-steps) na Co mám nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-111">I already know about that, give me [some examples](#next-steps) on what I can configure.</span></span>
* <span data-ttu-id="c9ec1-112">Vytvořit [ServicePackage.cspkg](#cspkg).</span><span class="sxs-lookup"><span data-stu-id="c9ec1-112">I want to create the [ServicePackage.cspkg](#cspkg).</span></span>
* <span data-ttu-id="c9ec1-113">Používám Visual Studio a chcete...</span><span class="sxs-lookup"><span data-stu-id="c9ec1-113">I am using Visual Studio and I want to...</span></span>
  * <span data-ttu-id="c9ec1-114">[Vytvoření cloudové služby][vs_create]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-114">[Create a cloud service][vs_create]</span></span>
  * <span data-ttu-id="c9ec1-115">[Znovu nakonfigurujte stávající cloudovou službu][vs_reconfigure]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-115">[Reconfigure an existing cloud service][vs_reconfigure]</span></span>
  * <span data-ttu-id="c9ec1-116">[Nasazení projektu cloudové služby][vs_deploy]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-116">[Deploy a Cloud Service project][vs_deploy]</span></span>
  * <span data-ttu-id="c9ec1-117">[Vzdálená plocha do instance cloudové služby][remotedesktop]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-117">[Remote desktop into a cloud service instance][remotedesktop]</span></span>

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a><span data-ttu-id="c9ec1-118">ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="c9ec1-118">ServiceDefinition.csdef</span></span>
<span data-ttu-id="c9ec1-119">**ServiceDefinition.csdef** souboru Určuje nastavení, které používají Azure ke konfigurování cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-119">The **ServiceDefinition.csdef** file specifies the settings that are used by Azure to configure a cloud service.</span></span> <span data-ttu-id="c9ec1-120">[Azure schématu definice služby (.csdef souboru)](https://msdn.microsoft.com/library/azure/ee758711.aspx) poskytuje povolený formát souboru definice služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-120">The [Azure Service Definition Schema (.csdef File)](https://msdn.microsoft.com/library/azure/ee758711.aspx) provides the allowable format for a service definition file.</span></span> <span data-ttu-id="c9ec1-121">Následující příklad ukazuje nastavení, které lze definovat pro webové a pracovní role:</span><span class="sxs-lookup"><span data-stu-id="c9ec1-121">The following example shows the settings that can be defined for the Web and Worker roles:</span></span>

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

<span data-ttu-id="c9ec1-122">Můžete se podívat do [služby definice schématu](https://msdn.microsoft.com/library/azure/ee758711.aspx) lépe porozumět schématu XML použít se zde, ale tady je rychlý vysvětlení některých prvků:</span><span class="sxs-lookup"><span data-stu-id="c9ec1-122">You can refer to the [Service Definition Schema](https://msdn.microsoft.com/library/azure/ee758711.aspx) for a better understanding of the XML schema used here, however, here is a quick explanation of some of the elements:</span></span>

<span data-ttu-id="c9ec1-123">**Weby**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-123">**Sites**</span></span>  
<span data-ttu-id="c9ec1-124">Obsahuje definice pro weby nebo webové aplikace, které jsou hostované ve službě IIS7.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-124">Contains the definitions for websites or web applications that are hosted in IIS7.</span></span>

<span data-ttu-id="c9ec1-125">**InputEndpoints**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-125">**InputEndpoints**</span></span>  
<span data-ttu-id="c9ec1-126">Obsahuje definice pro koncové body, které se používají ke kontaktování cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-126">Contains the definitions for endpoints that are used to contact the cloud service.</span></span>

<span data-ttu-id="c9ec1-127">**InternalEndpoints**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-127">**InternalEndpoints**</span></span>  
<span data-ttu-id="c9ec1-128">Obsahuje definice pro koncové body, které jsou používány instance rolí pro komunikaci mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-128">Contains the definitions for endpoints that are used by role instances to communicate with each other.</span></span>

<span data-ttu-id="c9ec1-129">**ConfigurationSettings**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-129">**ConfigurationSettings**</span></span>  
<span data-ttu-id="c9ec1-130">Obsahuje definice nastavení pro funkce určité role.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-130">Contains the setting definitions for features of a specific role.</span></span>

<span data-ttu-id="c9ec1-131">**Certifikáty**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-131">**Certificates**</span></span>  
<span data-ttu-id="c9ec1-132">Obsahuje definice pro certifikáty, které jsou potřebné pro roli.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-132">Contains the definitions for certificates that are needed for a role.</span></span> <span data-ttu-id="c9ec1-133">Předchozí příklad kódu ukazuje certifikát, který se používá pro konfiguraci Azure připojit.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-133">The previous code example shows a certificate that is used for the configuration of Azure Connect.</span></span>

<span data-ttu-id="c9ec1-134">**LocalResources**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-134">**LocalResources**</span></span>  
<span data-ttu-id="c9ec1-135">Obsahuje definice pro místní prostředky pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-135">Contains the definitions for local storage resources.</span></span> <span data-ttu-id="c9ec1-136">Prostředek Místní úložiště je vyhrazené adresáře v systému souborů virtuálního počítače, ve kterém je spuštěna instance role.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-136">A local storage resource is a reserved directory on the file system of the virtual machine in which an instance of a role is running.</span></span>

<span data-ttu-id="c9ec1-137">**Importy**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-137">**Imports**</span></span>  
<span data-ttu-id="c9ec1-138">Obsahuje definice pro importovaných modulů.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-138">Contains the definitions for imported modules.</span></span> <span data-ttu-id="c9ec1-139">Předchozí příklad kódu ukazuje moduly pro připojení ke vzdálené ploše a Azure připojit.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-139">The previous code example shows the modules for Remote Desktop Connection and Azure Connect.</span></span>

<span data-ttu-id="c9ec1-140">**Spuštění**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-140">**Startup**</span></span>  
<span data-ttu-id="c9ec1-141">Obsahuje úlohy, které se spustí při spuštění role.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-141">Contains tasks that are run when the role starts.</span></span> <span data-ttu-id="c9ec1-142">Úkoly jsou definovány v .cmd nebo spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-142">The tasks are defined in a .cmd or executable file.</span></span>

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a><span data-ttu-id="c9ec1-143">Souboru ServiceConfiguration.cscfg</span><span class="sxs-lookup"><span data-stu-id="c9ec1-143">ServiceConfiguration.cscfg</span></span>
<span data-ttu-id="c9ec1-144">Konfigurace nastavení pro cloudové služby je určen podle hodnot v **souboru ServiceConfiguration.cscfg** souboru.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-144">The configuration of the settings for your cloud service is determined by the values in the **ServiceConfiguration.cscfg** file.</span></span> <span data-ttu-id="c9ec1-145">Můžete zadat počet instancí, které chcete nasadit pro každou roli v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-145">You specify the number of instances that you want to deploy for each role in this file.</span></span> <span data-ttu-id="c9ec1-146">Hodnoty pro nastavení konfigurace, která jste zadali v souboru definice služby se přidají do konfiguračního souboru služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-146">The values for the configuration settings that you defined in the service definition file are added to the service configuration file.</span></span> <span data-ttu-id="c9ec1-147">Kryptografické otisky pro všechny certifikáty pro správu, které jsou spojeny s cloudovou službou jsou rovněž přidány do souboru.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-147">The thumbprints for any management certificates that are associated with the cloud service are also added to the file.</span></span> <span data-ttu-id="c9ec1-148">[Schéma konfigurace služby Azure (.cscfg souboru)](https://msdn.microsoft.com/library/azure/ee758710.aspx) poskytuje povolený formát pro konfigurační soubor služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-148">The [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx) provides the allowable format for a service configuration file.</span></span>

<span data-ttu-id="c9ec1-149">Konfigurační soubor služby není spojených s aplikací, ale nahraje do Azure jako samostatný soubor a slouží ke konfiguraci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-149">The service configuration file is not packaged with the application, but is uploaded to Azure as a separate file and is used to configure the cloud service.</span></span> <span data-ttu-id="c9ec1-150">Můžete nahrát nový soubor konfigurace služby bez opětovného nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-150">You can upload a new service configuration file without redeploying your cloud service.</span></span> <span data-ttu-id="c9ec1-151">Hodnoty konfigurace pro cloudové služby lze změnit, když běží v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-151">The configuration values for the cloud service can be changed while the cloud service is running.</span></span> <span data-ttu-id="c9ec1-152">Následující příklad ukazuje nastavení konfigurace, které lze definovat pro webové a pracovní role:</span><span class="sxs-lookup"><span data-stu-id="c9ec1-152">The following example shows the configuration settings that can be defined for the Web and Worker roles:</span></span>

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

<span data-ttu-id="c9ec1-153">Můžete se podívat do [schéma konfigurace služby](https://msdn.microsoft.com/library/azure/ee758710.aspx) pro lepší pochopení schématu XML použít se zde, ale tady je rychlý vysvětlení elementů:</span><span class="sxs-lookup"><span data-stu-id="c9ec1-153">You can refer to the [Service Configuration Schema](https://msdn.microsoft.com/library/azure/ee758710.aspx) for better understanding the XML schema used here, however, here is a quick explanation of the elements:</span></span>

<span data-ttu-id="c9ec1-154">**Instance**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-154">**Instances**</span></span>  
<span data-ttu-id="c9ec1-155">Konfiguruje počet spuštěných instancí role.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-155">Configures the number of running instances for the role.</span></span> <span data-ttu-id="c9ec1-156">Abyste zabránili potenciálně stává stále k dispozici při upgradech cloudové služby, doporučujeme nasadit více než jednu instanci směřujících webové role.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-156">To prevent your cloud service from potentially becoming unavailable during upgrades, it is recommended that you deploy more than one instance of your web-facing roles.</span></span> <span data-ttu-id="c9ec1-157">Nasazením více než jednu instanci, jsou splněny podle pokynů v [Azure výpočetní služby smlouvou o úrovni (SLA)](http://azure.microsoft.com/support/legal/sla/), což zaručuje 99,95 % externí připojení internetových rolí, pokud dvě nebo více instancí role jsou nasazeny pro službu.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-157">By deploying more than one instance, you are adhering to the guidelines in the [Azure Compute Service Level Agreement (SLA)](http://azure.microsoft.com/support/legal/sla/), which guarantees 99.95% external connectivity for Internet-facing roles when two or more role instances are deployed for a service.</span></span>

<span data-ttu-id="c9ec1-158">**ConfigurationSettings**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-158">**ConfigurationSettings**</span></span>  
<span data-ttu-id="c9ec1-159">Konfiguruje nastavení pro spuštěné instance role.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-159">Configures the settings for the running instances for a role.</span></span> <span data-ttu-id="c9ec1-160">Název `<Setting>` elementy se musí shodovat definice nastavení v definičním souboru služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-160">The name of the `<Setting>` elements must match the setting definitions in the service definition file.</span></span>

<span data-ttu-id="c9ec1-161">**Certifikáty**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-161">**Certificates**</span></span>  
<span data-ttu-id="c9ec1-162">Nakonfiguruje certifikáty, které používají službu.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-162">Configures the certificates that are used by the service.</span></span> <span data-ttu-id="c9ec1-163">Předchozí příklad kódu ukazuje, jak definovat certifikát pro modul vzdáleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-163">The previous code example shows how to define the certificate for the RemoteAccess module.</span></span> <span data-ttu-id="c9ec1-164">Hodnota *kryptografický otisk* musí být nastaven na kryptografický otisk certifikátu používat.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-164">The value of the *thumbprint* attribute must be set to the thumbprint of the certificate to use.</span></span>

<p/>

> [!NOTE]
> <span data-ttu-id="c9ec1-165">Kryptografický otisk certifikátu lze přidat do konfiguračního souboru pomocí textového editoru.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-165">The thumbprint for the certificate can be added to the configuration file by using a text editor.</span></span> <span data-ttu-id="c9ec1-166">Nebo hodnota lze přidat na **certifikáty** kartě **vlastnosti** stránky role v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-166">Or, the value can be added on the **Certificates** tab of the **Properties** page of the role in Visual Studio.</span></span>
> 
> 

## <a name="defining-ports-for-role-instances"></a><span data-ttu-id="c9ec1-167">Definování porty pro instance rolí</span><span class="sxs-lookup"><span data-stu-id="c9ec1-167">Defining ports for role instances</span></span>
<span data-ttu-id="c9ec1-168">Azure umožňuje pouze jeden vstupní bod do webové role.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-168">Azure allows only one entry point to a web role.</span></span> <span data-ttu-id="c9ec1-169">Znamená, že dojde k všechny přenosy přes jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-169">Meaning that all traffic occurs through one IP address.</span></span> <span data-ttu-id="c9ec1-170">Můžete nakonfigurovat své weby pro sdílení port nakonfigurováním hlavičku hostitele pro směrování požadavku na správné místo.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-170">You can configure your websites to share a port by configuring the host header to direct the request to the correct location.</span></span> <span data-ttu-id="c9ec1-171">Můžete také nakonfigurovat aplikace tak, aby naslouchala na dobře známé porty IP adresu.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-171">You can also configure your applications to listen to well-known ports on the IP address.</span></span>

<span data-ttu-id="c9ec1-172">Následující příklad ukazuje konfiguraci pro webové role s webových stránek a webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-172">The following sample shows the configuration for a web role with a website and web application.</span></span> <span data-ttu-id="c9ec1-173">Web je nakonfigurován jako výchozí umístění položky na portu 80 a webových aplikací konfigurovány tak, aby přijímal požadavky z hlavičku alternativním hostiteli, který se nazývá "mail.mysite.cloudapp.net".</span><span class="sxs-lookup"><span data-stu-id="c9ec1-173">The website is configured as the default entry location on port 80, and the web applications are configured to receive requests from an alternate host header that is called “mail.mysite.cloudapp.net”.</span></span>

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


## <a name="changing-the-configuration-of-a-role"></a><span data-ttu-id="c9ec1-174">Změna konfigurace role</span><span class="sxs-lookup"><span data-stu-id="c9ec1-174">Changing the configuration of a role</span></span>
<span data-ttu-id="c9ec1-175">Konfigurace cloudové služby můžete aktualizovat, když je spuštěná v Azure, bez nutnosti převádět služby do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-175">You can update the configuration of your cloud service while it is running in Azure, without taking the service offline.</span></span> <span data-ttu-id="c9ec1-176">Chcete-li změnit informace o konfiguraci, můžete nahrát nový soubor konfigurace, nebo upravit konfigurační soubor na místě a použijte ho pro spuštěnou službu.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-176">To change configuration information, you can either upload a new configuration file, or edit the configuration file in place and apply it to your running service.</span></span> <span data-ttu-id="c9ec1-177">Ke konfiguraci služby můžete provedeny následující změny:</span><span class="sxs-lookup"><span data-stu-id="c9ec1-177">The following changes can be made to the configuration of a service:</span></span>

* <span data-ttu-id="c9ec1-178">**Změna hodnoty nastavení konfigurace**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-178">**Changing the values of configuration settings**</span></span>  
  <span data-ttu-id="c9ec1-179">Při konfiguraci nastavení změny role instance můžete zvolit použití změny, zatímco instance online nebo recyklovat instance řádně a použití změn při instance je offline.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-179">When a configuration setting changes, a role instance can choose to apply the change while the instance is online, or to recycle the instance gracefully and apply the change while the instance is offline.</span></span>
* <span data-ttu-id="c9ec1-180">**Změna topologie služby instancí role**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-180">**Changing the service topology of role instances**</span></span>  
  <span data-ttu-id="c9ec1-181">Topologie změny neovlivňují spuštěné instance, s výjimkou případů, kdy je odebírána instance.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-181">Topology changes do not affect running instances, except where an instance is being removed.</span></span> <span data-ttu-id="c9ec1-182">Všechny zbývající instance obecně nemusí být recyklována; Můžete však recyklovat instance rolí v reakci na změnu topologie.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-182">All remaining instances generally do not need to be recycled; however, you can choose to recycle role instances in response to a topology change.</span></span>
* <span data-ttu-id="c9ec1-183">**Změna kryptografický otisk certifikátu**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-183">**Changing the certificate thumbprint**</span></span>  
  <span data-ttu-id="c9ec1-184">Certifikát lze aktualizovat pouze pokud je role instance offline.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-184">You can only update a certificate when a role instance is offline.</span></span> <span data-ttu-id="c9ec1-185">Pokud certifikát přidat, odstranit nebo změnit, pokud je role instance online, Azure řádně trvá instance offline aktualizovat certifikát a převeďte ho zpátky do online režimu, po dokončení změn.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-185">If a certificate is added, deleted, or changed while a role instance is online, Azure gracefully takes the instance offline to update the certificate and bring it back online after the change is complete.</span></span>

### <a name="handling-configuration-changes-with-service-runtime-events"></a><span data-ttu-id="c9ec1-186">Zpracování změny konfigurace s událostmi služby modulu Runtime</span><span class="sxs-lookup"><span data-stu-id="c9ec1-186">Handling configuration changes with Service Runtime Events</span></span>
<span data-ttu-id="c9ec1-187">[Knihovna Runtime Azure](https://msdn.microsoft.com/library/azure/mt419365.aspx) zahrnuje [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) názvů, který poskytuje třídy pro interakci s prostředím Azure z role.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-187">The [Azure Runtime Library](https://msdn.microsoft.com/library/azure/mt419365.aspx) includes the [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) namespace, which provides classes for interacting with the Azure environment from a role.</span></span> <span data-ttu-id="c9ec1-188">[RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) třída definuje následující události, které jsou vyvolány před a po změně konfigurace:</span><span class="sxs-lookup"><span data-stu-id="c9ec1-188">The [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) class defines the following events that are raised before and after a configuration change:</span></span>

* <span data-ttu-id="c9ec1-189">**[Změna](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) událostí**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-189">**[Changing](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) event**</span></span>  
  <span data-ttu-id="c9ec1-190">K tomu dojde, před použitím změn konfigurace do zadané instance role s možností umožňuje vypnout instance rolí, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-190">This occurs before the configuration change is applied to a specified instance of a role giving you a chance to take down the role instances if necessary.</span></span>
* <span data-ttu-id="c9ec1-191">**[Změnit](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) událostí**</span><span class="sxs-lookup"><span data-stu-id="c9ec1-191">**[Changed](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) event**</span></span>  
  <span data-ttu-id="c9ec1-192">Vyskytne se po změně konfigurace se použije k zadané instanci role.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-192">Occurs after the configuration change is applied to a specified instance of a role.</span></span>

> [!NOTE]
> <span data-ttu-id="c9ec1-193">Protože změny certifikátu vždy provést instancí role do offline režimu, vyvolají není RoleEnvironment.Changing nebo RoleEnvironment.Changed událostí.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-193">Because certificate changes always take the instances of a role offline, they do not raise the RoleEnvironment.Changing or RoleEnvironment.Changed events.</span></span>
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a><span data-ttu-id="c9ec1-194">ServicePackage.cspkg</span><span class="sxs-lookup"><span data-stu-id="c9ec1-194">ServicePackage.cspkg</span></span>
<span data-ttu-id="c9ec1-195">Pokud chcete nasadit aplikaci jako cloudové služby v Azure, musí nejprve balíčku aplikace v příslušném formátu.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-195">To deploy an application as a cloud service in Azure, you must first package the application in the appropriate format.</span></span> <span data-ttu-id="c9ec1-196">Můžete použít **CSPack** nástroj příkazového řádku (nainstalované s [Azure SDK](https://azure.microsoft.com/downloads/)) k vytvoření souboru balíčku jako alternativu k sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-196">You can use the **CSPack** command-line tool (installed with the [Azure SDK](https://azure.microsoft.com/downloads/)) to create the package file as an alternative to Visual Studio.</span></span>

<span data-ttu-id="c9ec1-197">**CSPack** používá obsah souboru definice služby a konfigurační soubor služby zadat obsah balíčku.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-197">**CSPack** uses the contents of the service definition file and service configuration file to define the contents of the package.</span></span> <span data-ttu-id="c9ec1-198">**CSPack** generuje balíčku souboru aplikace (.cspkg), který můžete nahrát do Azure pomocí [portál Azure](cloud-services-how-to-create-deploy-portal.md#create-and-deploy).</span><span class="sxs-lookup"><span data-stu-id="c9ec1-198">**CSPack** generates an application package file (.cspkg) that you can upload to Azure by using the [Azure portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy).</span></span> <span data-ttu-id="c9ec1-199">Ve výchozím nastavení, balíček s názvem `[ServiceDefinitionFileName].cspkg`, ale můžete zadat jiný název pomocí `/out` možnost **CSPack**.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-199">By default, the package is named `[ServiceDefinitionFileName].cspkg`, but you can specify a different name by using the `/out` option of **CSPack**.</span></span>

<span data-ttu-id="c9ec1-200">**CSPack** se nachází v</span><span class="sxs-lookup"><span data-stu-id="c9ec1-200">**CSPack** is located at</span></span>  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> <span data-ttu-id="c9ec1-201">CSPack.exe (v systému windows) je k dispozici spuštěním **Microsoft Azure příkazového řádku** zástupce, který je nainstalován pomocí sady SDK.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-201">CSPack.exe (on windows) is available by running the **Microsoft Azure Command Prompt** shortcut that is installed with the SDK.</span></span>  
> 
> <span data-ttu-id="c9ec1-202">Spusťte CSPack.exe program samostatně najdete v dokumentaci o všech možných přepínačích a příkazy.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-202">Run the CSPack.exe program by itself to see documentation about all the possible switches and commands.</span></span>
> 
> 

<p />

> [!TIP]
> <span data-ttu-id="c9ec1-203">Spustit místně v cloudové služby **Microsoft Azure výpočetní emulátor**, použijte **/copyonly** možnost.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-203">Run your cloud service locally in the **Microsoft Azure Compute Emulator**, use the **/copyonly** option.</span></span> <span data-ttu-id="c9ec1-204">Tato možnost zkopíruje binární soubory pro aplikaci do adresáře rozložení, ze kterého se můžete spustit v emulátoru služby výpočty v.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-204">This option copies the binary files for the application to a directory layout from which they can be run in the compute emulator.</span></span>
> 
> 

### <a name="example-command-to-package-a-cloud-service"></a><span data-ttu-id="c9ec1-205">Příklad do balíčku cloudové služby</span><span class="sxs-lookup"><span data-stu-id="c9ec1-205">Example command to package a cloud service</span></span>
<span data-ttu-id="c9ec1-206">Následující příklad vytvoří balíček aplikace, který obsahuje informace pro webové role.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-206">The following example creates an application package that contains the information for a web role.</span></span> <span data-ttu-id="c9ec1-207">Příkaz určuje soubor definice služby, kterou chcete použít, adresáři, kde můžete najít binární soubory, a název souboru balíčku.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-207">The command specifies the service definition file to use, the directory where binary files can be found, and the name of the package file.</span></span>

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

<span data-ttu-id="c9ec1-208">Pokud aplikace obsahuje webovou roli a roli pracovního procesu, je použít následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c9ec1-208">If the application contains both a web role and a worker role, the following command is used:</span></span>

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

<span data-ttu-id="c9ec1-209">Kde proměnné jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="c9ec1-209">Where the variables are defined as follows:</span></span>

| <span data-ttu-id="c9ec1-210">Proměnná</span><span class="sxs-lookup"><span data-stu-id="c9ec1-210">Variable</span></span> | <span data-ttu-id="c9ec1-211">Hodnota</span><span class="sxs-lookup"><span data-stu-id="c9ec1-211">Value</span></span> |
| --- | --- |
| <span data-ttu-id="c9ec1-212">\[DirectoryName\]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-212">\[DirectoryName\]</span></span> |<span data-ttu-id="c9ec1-213">Podadresáři v kořenovém adresáři projektu, který obsahuje soubor .csdef Azure projektu.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-213">The subdirectory under the root project directory that contains the .csdef file of the Azure project.</span></span> |
| <span data-ttu-id="c9ec1-214">\[ServiceDefinition\]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-214">\[ServiceDefinition\]</span></span> |<span data-ttu-id="c9ec1-215">Název souboru definice služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-215">The name of the service definition file.</span></span> <span data-ttu-id="c9ec1-216">Ve výchozím nastavení je tento soubor s názvem ServiceDefinition.csdef.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-216">By default, this file is named ServiceDefinition.csdef.</span></span> |
| <span data-ttu-id="c9ec1-217">\[Název_výstupního_souboru\]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-217">\[OutputFileName\]</span></span> |<span data-ttu-id="c9ec1-218">Název souboru vygenerovaný balíček.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-218">The name for the generated package file.</span></span> <span data-ttu-id="c9ec1-219">Obvykle je nastavena na název aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-219">Typically, this is set to the name of the application.</span></span> <span data-ttu-id="c9ec1-220">Pokud není zadán žádný název souboru, balíček aplikace je vytvořen jako \[ApplicationName\].cspkg.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-220">If no file name is specified, the application package is created as \[ApplicationName\].cspkg.</span></span> |
| <span data-ttu-id="c9ec1-221">\[RoleName\]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-221">\[RoleName\]</span></span> |<span data-ttu-id="c9ec1-222">Název role, jak jsou definovány v souboru definice služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-222">The name of the role as defined in the service definition file.</span></span> |
| <span data-ttu-id="c9ec1-223">\[RoleBinariesDirectory]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-223">\[RoleBinariesDirectory]</span></span> |<span data-ttu-id="c9ec1-224">Umístění binární soubory pro roli.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-224">The location of the binary files for the role.</span></span> |
| <span data-ttu-id="c9ec1-225">\[VirtualPath\]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-225">\[VirtualPath\]</span></span> |<span data-ttu-id="c9ec1-226">Fyzické adresáře pro každý virtuální cestu definovaný v oddílu lokality definice služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-226">The physical directories for each virtual path defined in the Sites section of the service definition.</span></span> |
| <span data-ttu-id="c9ec1-227">\[PhysicalPath\]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-227">\[PhysicalPath\]</span></span> |<span data-ttu-id="c9ec1-228">Fyzické adresáře obsah pro každý virtuální cestu definovanou v uzlu lokality definice služby.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-228">The physical directories of the contents for each virtual path defined in the site node of the service definition.</span></span> |
| <span data-ttu-id="c9ec1-229">\[RoleAssemblyName\]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-229">\[RoleAssemblyName\]</span></span> |<span data-ttu-id="c9ec1-230">Název binární soubor pro roli.</span><span class="sxs-lookup"><span data-stu-id="c9ec1-230">The name of the binary file for the role.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c9ec1-231">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9ec1-231">Next steps</span></span>
<span data-ttu-id="c9ec1-232">Vytváření balíčku cloudové služby a chcete...</span><span class="sxs-lookup"><span data-stu-id="c9ec1-232">I'm creating a cloud service package and I want to...</span></span>

* <span data-ttu-id="c9ec1-233">[Instalační program vzdálené plochy pro instanci cloudové služby][remotedesktop]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-233">[Setup remote desktop for a cloud service instance][remotedesktop]</span></span>
* <span data-ttu-id="c9ec1-234">[Nasazení projektu cloudové služby][deploy]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-234">[Deploy a Cloud Service project][deploy]</span></span>

<span data-ttu-id="c9ec1-235">Používám Visual Studio a chcete...</span><span class="sxs-lookup"><span data-stu-id="c9ec1-235">I am using Visual Studio and I want to...</span></span>

* <span data-ttu-id="c9ec1-236">[Vytvořte novou cloudovou službu][vs_create]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-236">[Create a new cloud service][vs_create]</span></span>
* <span data-ttu-id="c9ec1-237">[Znovu nakonfigurujte stávající cloudovou službu][vs_reconfigure]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-237">[Reconfigure an existing cloud service][vs_reconfigure]</span></span>
* <span data-ttu-id="c9ec1-238">[Nasazení projektu cloudové služby][vs_deploy]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-238">[Deploy a Cloud Service project][vs_deploy]</span></span>
* <span data-ttu-id="c9ec1-239">[Instalační program vzdálené plochy pro instanci cloudové služby][vs_remote]</span><span class="sxs-lookup"><span data-stu-id="c9ec1-239">[Setup remote desktop for a cloud service instance][vs_remote]</span></span>

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
