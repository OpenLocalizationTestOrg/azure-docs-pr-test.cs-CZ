---
title: "aaaCloud roli služby konfigurace XPath tahák | Microsoft Docs"
description: "Hello různá nastavení XPath, které můžete použít v tooexpose konfigurační nastavení pro hello cloudové služby role jako proměnné prostředí."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c51e4493-0643-4d05-bc44-06c76bcbf7d1
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 27f98f956a1c790c9bb30f9fefe1ab1736b2b150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a><span data-ttu-id="2dd0a-103">Vystavit nastavení konfigurace role v proměnné prostředí, jejichž výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-103">Expose role configuration settings as an environment variable with XPath</span></span>
<span data-ttu-id="2dd0a-104">V souboru definice služby role webový nebo hello cloudové služby pracovního procesu můžete vystavit hodnoty konfigurace modulu runtime v proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-104">In hello cloud service worker or web role service definition file, you can expose runtime configuration values as environment variables.</span></span> <span data-ttu-id="2dd0a-105">Hello následující XPath hodnoty jsou podporovány (která odpovídají tooAPI hodnoty).</span><span class="sxs-lookup"><span data-stu-id="2dd0a-105">hello following XPath values are supported (which correspond tooAPI values).</span></span>

<span data-ttu-id="2dd0a-106">Tyto hodnoty XPath jsou také k dispozici prostřednictvím hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) knihovny.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-106">These XPath values are also available through hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) library.</span></span> 

## <a name="app-running-in-emulator"></a><span data-ttu-id="2dd0a-107">Aplikaci spuštěnou v emulátoru</span><span class="sxs-lookup"><span data-stu-id="2dd0a-107">App running in emulator</span></span>
<span data-ttu-id="2dd0a-108">Označuje, že tuto aplikaci hello běží v emulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-108">Indicates that hello app is running in hello emulator.</span></span>

| <span data-ttu-id="2dd0a-109">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-109">Type</span></span> | <span data-ttu-id="2dd0a-110">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-110">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-111">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-111">XPath</span></span> |<span data-ttu-id="2dd0a-112">výraz XPath = "/RoleEnvironment/Deployment/@emulated"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-112">xpath="/RoleEnvironment/Deployment/@emulated"</span></span> |
| <span data-ttu-id="2dd0a-113">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-113">Code</span></span> |<span data-ttu-id="2dd0a-114">var x = RoleEnvironment.IsEmulated;</span><span class="sxs-lookup"><span data-stu-id="2dd0a-114">var x = RoleEnvironment.IsEmulated;</span></span> |

## <a name="deployment-id"></a><span data-ttu-id="2dd0a-115">ID nasazení</span><span class="sxs-lookup"><span data-stu-id="2dd0a-115">Deployment ID</span></span>
<span data-ttu-id="2dd0a-116">Načte ID hello nasazení pro instanci hello.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-116">Retrieves hello deployment ID for hello instance.</span></span>

| <span data-ttu-id="2dd0a-117">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-117">Type</span></span> | <span data-ttu-id="2dd0a-118">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-118">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-119">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-119">XPath</span></span> |<span data-ttu-id="2dd0a-120">výraz XPath = "/RoleEnvironment/Deployment/@id"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-120">xpath="/RoleEnvironment/Deployment/@id"</span></span> |
| <span data-ttu-id="2dd0a-121">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-121">Code</span></span> |<span data-ttu-id="2dd0a-122">var deploymentId = RoleEnvironment.DeploymentId;</span><span class="sxs-lookup"><span data-stu-id="2dd0a-122">var deploymentId = RoleEnvironment.DeploymentId;</span></span> |

## <a name="role-id"></a><span data-ttu-id="2dd0a-123">Role ID</span><span class="sxs-lookup"><span data-stu-id="2dd0a-123">Role ID</span></span>
<span data-ttu-id="2dd0a-124">Načte hello aktuální ID role pro instanci hello.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-124">Retrieves hello current role ID for hello instance.</span></span>

| <span data-ttu-id="2dd0a-125">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-125">Type</span></span> | <span data-ttu-id="2dd0a-126">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-126">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-127">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-127">XPath</span></span> |<span data-ttu-id="2dd0a-128">výraz XPath = "/RoleEnvironment/CurrentInstance/@id"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-128">xpath="/RoleEnvironment/CurrentInstance/@id"</span></span> |
| <span data-ttu-id="2dd0a-129">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-129">Code</span></span> |<span data-ttu-id="2dd0a-130">var id = RoleEnvironment.CurrentRoleInstance.Id;</span><span class="sxs-lookup"><span data-stu-id="2dd0a-130">var id = RoleEnvironment.CurrentRoleInstance.Id;</span></span> |

## <a name="update-domain"></a><span data-ttu-id="2dd0a-131">Aktualizace domény</span><span class="sxs-lookup"><span data-stu-id="2dd0a-131">Update domain</span></span>
<span data-ttu-id="2dd0a-132">Načte hello aktualizace domény hello instance.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-132">Retrieves hello update domain of hello instance.</span></span>

| <span data-ttu-id="2dd0a-133">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-133">Type</span></span> | <span data-ttu-id="2dd0a-134">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-134">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-135">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-135">XPath</span></span> |<span data-ttu-id="2dd0a-136">výraz XPath = "/RoleEnvironment/CurrentInstance/@updateDomain"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-136">xpath="/RoleEnvironment/CurrentInstance/@updateDomain"</span></span> |
| <span data-ttu-id="2dd0a-137">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-137">Code</span></span> |<span data-ttu-id="2dd0a-138">var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain;</span><span class="sxs-lookup"><span data-stu-id="2dd0a-138">var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain;</span></span> |

## <a name="fault-domain"></a><span data-ttu-id="2dd0a-139">Doména selhání</span><span class="sxs-lookup"><span data-stu-id="2dd0a-139">Fault domain</span></span>
<span data-ttu-id="2dd0a-140">Načte doména selhání hello hello instance.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-140">Retrieves hello fault domain of hello instance.</span></span>

| <span data-ttu-id="2dd0a-141">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-141">Type</span></span> | <span data-ttu-id="2dd0a-142">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-142">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-143">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-143">XPath</span></span> |<span data-ttu-id="2dd0a-144">výraz XPath = "/RoleEnvironment/CurrentInstance/@faultDomain"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-144">xpath="/RoleEnvironment/CurrentInstance/@faultDomain"</span></span> |
| <span data-ttu-id="2dd0a-145">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-145">Code</span></span> |<span data-ttu-id="2dd0a-146">var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain;</span><span class="sxs-lookup"><span data-stu-id="2dd0a-146">var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain;</span></span> |

## <a name="role-name"></a><span data-ttu-id="2dd0a-147">Název role</span><span class="sxs-lookup"><span data-stu-id="2dd0a-147">Role name</span></span>
<span data-ttu-id="2dd0a-148">Načte název role hello hello instancí.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-148">Retrieves hello role name of hello instances.</span></span>

| <span data-ttu-id="2dd0a-149">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-149">Type</span></span> | <span data-ttu-id="2dd0a-150">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-150">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-151">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-151">XPath</span></span> |<span data-ttu-id="2dd0a-152">výraz XPath = "/RoleEnvironment/CurrentInstance/@roleName"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-152">xpath="/RoleEnvironment/CurrentInstance/@roleName"</span></span> |
| <span data-ttu-id="2dd0a-153">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-153">Code</span></span> |<span data-ttu-id="2dd0a-154">var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;</span><span class="sxs-lookup"><span data-stu-id="2dd0a-154">var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;</span></span> |

## <a name="config-setting"></a><span data-ttu-id="2dd0a-155">Nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="2dd0a-155">Config setting</span></span>
<span data-ttu-id="2dd0a-156">Načte hodnotu hello hello zadat nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-156">Retrieves hello value of hello specified configuration setting.</span></span>

| <span data-ttu-id="2dd0a-157">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-157">Type</span></span> | <span data-ttu-id="2dd0a-158">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-158">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-159">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-159">XPath</span></span> |<span data-ttu-id="2dd0a-160">výraz XPath = "/ RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting [@name= 'Setting1']/@value"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-160">xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value"</span></span> |
| <span data-ttu-id="2dd0a-161">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-161">Code</span></span> |<span data-ttu-id="2dd0a-162">var nastavení = RoleEnvironment.GetConfigurationSettingValue("Setting1");</span><span class="sxs-lookup"><span data-stu-id="2dd0a-162">var setting = RoleEnvironment.GetConfigurationSettingValue("Setting1");</span></span> |

## <a name="local-storage-path"></a><span data-ttu-id="2dd0a-163">Cesta k místnímu úložišti</span><span class="sxs-lookup"><span data-stu-id="2dd0a-163">Local storage path</span></span>
<span data-ttu-id="2dd0a-164">Načte cestu hello místní úložiště pro instanci hello.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-164">Retrieves hello local storage path for hello instance.</span></span>

| <span data-ttu-id="2dd0a-165">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-165">Type</span></span> | <span data-ttu-id="2dd0a-166">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-166">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-167">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-167">XPath</span></span> |<span data-ttu-id="2dd0a-168">výraz XPath = "/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@path"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-168">xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path"</span></span> |
| <span data-ttu-id="2dd0a-169">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-169">Code</span></span> |<span data-ttu-id="2dd0a-170">var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath;</span><span class="sxs-lookup"><span data-stu-id="2dd0a-170">var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1").RootPath;</span></span> |

## <a name="local-storage-size"></a><span data-ttu-id="2dd0a-171">Velikost místní úložiště</span><span class="sxs-lookup"><span data-stu-id="2dd0a-171">Local storage size</span></span>
<span data-ttu-id="2dd0a-172">Načte hello velikost hello místní úložiště pro instanci hello.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-172">Retrieves hello size of hello local storage for hello instance.</span></span>

| <span data-ttu-id="2dd0a-173">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-173">Type</span></span> | <span data-ttu-id="2dd0a-174">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-174">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-175">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-175">XPath</span></span> |<span data-ttu-id="2dd0a-176">výraz XPath = "/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@sizeInMB"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-176">xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB"</span></span> |
| <span data-ttu-id="2dd0a-177">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-177">Code</span></span> |<span data-ttu-id="2dd0a-178">var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes;</span><span class="sxs-lookup"><span data-stu-id="2dd0a-178">var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1").MaximumSizeInMegabytes;</span></span> |

## <a name="endpoint-protocol"></a><span data-ttu-id="2dd0a-179">Koncový bod protokolu</span><span class="sxs-lookup"><span data-stu-id="2dd0a-179">Endpoint protocol</span></span>
<span data-ttu-id="2dd0a-180">Načte hello koncový bod protokolu pro instanci hello.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-180">Retrieves hello endpoint protocol for hello instance.</span></span>

| <span data-ttu-id="2dd0a-181">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-181">Type</span></span> | <span data-ttu-id="2dd0a-182">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-182">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-183">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-183">XPath</span></span> |<span data-ttu-id="2dd0a-184">výraz XPath = "/ RoleEnvironment nebo CurrentInstance nebo koncových bodů nebo koncového bodu [@name= 'koncovém bodě 1']/@protocol"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-184">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol"</span></span> |
| <span data-ttu-id="2dd0a-185">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-185">Code</span></span> |<span data-ttu-id="2dd0a-186">var ochranu = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protokol;</span><span class="sxs-lookup"><span data-stu-id="2dd0a-186">var prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].Protocol;</span></span> |

## <a name="endpoint-ip"></a><span data-ttu-id="2dd0a-187">Koncový bod IP</span><span class="sxs-lookup"><span data-stu-id="2dd0a-187">Endpoint IP</span></span>
<span data-ttu-id="2dd0a-188">Získá hello zadaný pro koncový bod IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-188">Gets hello specified endpoint's IP address.</span></span>

| <span data-ttu-id="2dd0a-189">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-189">Type</span></span> | <span data-ttu-id="2dd0a-190">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-190">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-191">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-191">XPath</span></span> |<span data-ttu-id="2dd0a-192">výraz XPath = "/ RoleEnvironment nebo CurrentInstance nebo koncových bodů nebo koncového bodu [@name= 'koncovém bodě 1']/@address"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-192">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address"</span></span> |
| <span data-ttu-id="2dd0a-193">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-193">Code</span></span> |<span data-ttu-id="2dd0a-194">var adresu = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address</span><span class="sxs-lookup"><span data-stu-id="2dd0a-194">var address = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].IPEndpoint.Address</span></span> |

## <a name="endpoint-port"></a><span data-ttu-id="2dd0a-195">koncový port</span><span class="sxs-lookup"><span data-stu-id="2dd0a-195">Endpoint port</span></span>
<span data-ttu-id="2dd0a-196">Načte hello koncový port pro instanci hello.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-196">Retrieves hello endpoint port for hello instance.</span></span>

| <span data-ttu-id="2dd0a-197">Typ</span><span class="sxs-lookup"><span data-stu-id="2dd0a-197">Type</span></span> | <span data-ttu-id="2dd0a-198">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-198">Example</span></span> |
| --- | --- |
| <span data-ttu-id="2dd0a-199">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="2dd0a-199">XPath</span></span> |<span data-ttu-id="2dd0a-200">výraz XPath = "/ RoleEnvironment nebo CurrentInstance nebo koncových bodů nebo koncového bodu [@name= 'koncovém bodě 1']/@port"</span><span class="sxs-lookup"><span data-stu-id="2dd0a-200">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port"</span></span> |
| <span data-ttu-id="2dd0a-201">Kód</span><span class="sxs-lookup"><span data-stu-id="2dd0a-201">Code</span></span> |<span data-ttu-id="2dd0a-202">var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port;</span><span class="sxs-lookup"><span data-stu-id="2dd0a-202">var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].IPEndpoint.Port;</span></span> |

## <a name="example"></a><span data-ttu-id="2dd0a-203">Příklad</span><span class="sxs-lookup"><span data-stu-id="2dd0a-203">Example</span></span>
<span data-ttu-id="2dd0a-204">Tady je příklad role pracovního procesu, který vytváří úloha spuštění s proměnnou prostředí s názvem `TestIsEmulated` nastavit toohello [ @emulated hodnotu xpath](#app-running-in-emulator).</span><span class="sxs-lookup"><span data-stu-id="2dd0a-204">Here is an example of a worker role that creates a startup task with an environment variable named `TestIsEmulated` set toohello [@emulated xpath value](#app-running-in-emulator).</span></span> 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a><span data-ttu-id="2dd0a-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2dd0a-205">Next steps</span></span>
<span data-ttu-id="2dd0a-206">Další informace o hello [souboru ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) souboru.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-206">Learn more about hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) file.</span></span>

<span data-ttu-id="2dd0a-207">Vytvoření [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) balíčku.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-207">Create a [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) package.</span></span>

<span data-ttu-id="2dd0a-208">Povolit [vzdálené plochy](cloud-services-role-enable-remote-desktop.md) pro roli.</span><span class="sxs-lookup"><span data-stu-id="2dd0a-208">Enable [remote desktop](cloud-services-role-enable-remote-desktop.md) for a role.</span></span>

