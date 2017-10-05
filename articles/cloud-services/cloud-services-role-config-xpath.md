---
title: "Cloudové služby Role konfigurace XPath tahák | Microsoft Docs"
description: "Různá nastavení XPath můžete v konfiguračním cloudové služby role vystavit nastavení jako proměnné prostředí."
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
ms.openlocfilehash: fd6efac829d3fd9e2840362b8d2ff423add566d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a><span data-ttu-id="bfdf2-103">Vystavit nastavení konfigurace role v proměnné prostředí, jejichž výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-103">Expose role configuration settings as an environment variable with XPath</span></span>
<span data-ttu-id="bfdf2-104">V pracovní cloudové služby nebo v souboru definice služby role Webový můžou zpřístupnit hodnoty konfigurace modulu runtime jako proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-104">In the cloud service worker or web role service definition file, you can expose runtime configuration values as environment variables.</span></span> <span data-ttu-id="bfdf2-105">Následující výraz XPath hodnoty jsou podporovány (které odpovídají hodnotám rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="bfdf2-105">The following XPath values are supported (which correspond to API values).</span></span>

<span data-ttu-id="bfdf2-106">Tyto hodnoty XPath jsou také k dispozici prostřednictvím [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) knihovny.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-106">These XPath values are also available through the [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) library.</span></span> 

## <a name="app-running-in-emulator"></a><span data-ttu-id="bfdf2-107">Aplikaci spuštěnou v emulátoru</span><span class="sxs-lookup"><span data-stu-id="bfdf2-107">App running in emulator</span></span>
<span data-ttu-id="bfdf2-108">Označuje, že aplikace běží v emulátoru.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-108">Indicates that the app is running in the emulator.</span></span>

| <span data-ttu-id="bfdf2-109">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-109">Type</span></span> | <span data-ttu-id="bfdf2-110">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-110">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-111">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-111">XPath</span></span> |<span data-ttu-id="bfdf2-112">výraz XPath = "/RoleEnvironment/Deployment/@emulated"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-112">xpath="/RoleEnvironment/Deployment/@emulated"</span></span> |
| <span data-ttu-id="bfdf2-113">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-113">Code</span></span> |<span data-ttu-id="bfdf2-114">var x = RoleEnvironment.IsEmulated;</span><span class="sxs-lookup"><span data-stu-id="bfdf2-114">var x = RoleEnvironment.IsEmulated;</span></span> |

## <a name="deployment-id"></a><span data-ttu-id="bfdf2-115">ID nasazení</span><span class="sxs-lookup"><span data-stu-id="bfdf2-115">Deployment ID</span></span>
<span data-ttu-id="bfdf2-116">Načte identifikátor ID nasazení pro instanci.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-116">Retrieves the deployment ID for the instance.</span></span>

| <span data-ttu-id="bfdf2-117">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-117">Type</span></span> | <span data-ttu-id="bfdf2-118">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-118">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-119">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-119">XPath</span></span> |<span data-ttu-id="bfdf2-120">výraz XPath = "/RoleEnvironment/Deployment/@id"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-120">xpath="/RoleEnvironment/Deployment/@id"</span></span> |
| <span data-ttu-id="bfdf2-121">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-121">Code</span></span> |<span data-ttu-id="bfdf2-122">var deploymentId = RoleEnvironment.DeploymentId;</span><span class="sxs-lookup"><span data-stu-id="bfdf2-122">var deploymentId = RoleEnvironment.DeploymentId;</span></span> |

## <a name="role-id"></a><span data-ttu-id="bfdf2-123">Role ID</span><span class="sxs-lookup"><span data-stu-id="bfdf2-123">Role ID</span></span>
<span data-ttu-id="bfdf2-124">Načte aktuální ID role pro instanci.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-124">Retrieves the current role ID for the instance.</span></span>

| <span data-ttu-id="bfdf2-125">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-125">Type</span></span> | <span data-ttu-id="bfdf2-126">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-126">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-127">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-127">XPath</span></span> |<span data-ttu-id="bfdf2-128">výraz XPath = "/RoleEnvironment/CurrentInstance/@id"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-128">xpath="/RoleEnvironment/CurrentInstance/@id"</span></span> |
| <span data-ttu-id="bfdf2-129">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-129">Code</span></span> |<span data-ttu-id="bfdf2-130">var id = RoleEnvironment.CurrentRoleInstance.Id;</span><span class="sxs-lookup"><span data-stu-id="bfdf2-130">var id = RoleEnvironment.CurrentRoleInstance.Id;</span></span> |

## <a name="update-domain"></a><span data-ttu-id="bfdf2-131">Aktualizace domény</span><span class="sxs-lookup"><span data-stu-id="bfdf2-131">Update domain</span></span>
<span data-ttu-id="bfdf2-132">Načte aktualizace domény instance.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-132">Retrieves the update domain of the instance.</span></span>

| <span data-ttu-id="bfdf2-133">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-133">Type</span></span> | <span data-ttu-id="bfdf2-134">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-134">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-135">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-135">XPath</span></span> |<span data-ttu-id="bfdf2-136">výraz XPath = "/RoleEnvironment/CurrentInstance/@updateDomain"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-136">xpath="/RoleEnvironment/CurrentInstance/@updateDomain"</span></span> |
| <span data-ttu-id="bfdf2-137">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-137">Code</span></span> |<span data-ttu-id="bfdf2-138">var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain;</span><span class="sxs-lookup"><span data-stu-id="bfdf2-138">var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain;</span></span> |

## <a name="fault-domain"></a><span data-ttu-id="bfdf2-139">Doména selhání</span><span class="sxs-lookup"><span data-stu-id="bfdf2-139">Fault domain</span></span>
<span data-ttu-id="bfdf2-140">Načte domény selhání instance.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-140">Retrieves the fault domain of the instance.</span></span>

| <span data-ttu-id="bfdf2-141">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-141">Type</span></span> | <span data-ttu-id="bfdf2-142">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-142">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-143">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-143">XPath</span></span> |<span data-ttu-id="bfdf2-144">výraz XPath = "/RoleEnvironment/CurrentInstance/@faultDomain"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-144">xpath="/RoleEnvironment/CurrentInstance/@faultDomain"</span></span> |
| <span data-ttu-id="bfdf2-145">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-145">Code</span></span> |<span data-ttu-id="bfdf2-146">var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain;</span><span class="sxs-lookup"><span data-stu-id="bfdf2-146">var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain;</span></span> |

## <a name="role-name"></a><span data-ttu-id="bfdf2-147">Název role</span><span class="sxs-lookup"><span data-stu-id="bfdf2-147">Role name</span></span>
<span data-ttu-id="bfdf2-148">Načte název role instance.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-148">Retrieves the role name of the instances.</span></span>

| <span data-ttu-id="bfdf2-149">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-149">Type</span></span> | <span data-ttu-id="bfdf2-150">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-150">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-151">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-151">XPath</span></span> |<span data-ttu-id="bfdf2-152">výraz XPath = "/RoleEnvironment/CurrentInstance/@roleName"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-152">xpath="/RoleEnvironment/CurrentInstance/@roleName"</span></span> |
| <span data-ttu-id="bfdf2-153">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-153">Code</span></span> |<span data-ttu-id="bfdf2-154">var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;</span><span class="sxs-lookup"><span data-stu-id="bfdf2-154">var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;</span></span> |

## <a name="config-setting"></a><span data-ttu-id="bfdf2-155">Nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="bfdf2-155">Config setting</span></span>
<span data-ttu-id="bfdf2-156">Načte hodnotu zadaného konfiguračního nastavení.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-156">Retrieves the value of the specified configuration setting.</span></span>

| <span data-ttu-id="bfdf2-157">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-157">Type</span></span> | <span data-ttu-id="bfdf2-158">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-158">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-159">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-159">XPath</span></span> |<span data-ttu-id="bfdf2-160">výraz XPath = "/ RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting [@name= 'Setting1']/@value"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-160">xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value"</span></span> |
| <span data-ttu-id="bfdf2-161">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-161">Code</span></span> |<span data-ttu-id="bfdf2-162">var nastavení = RoleEnvironment.GetConfigurationSettingValue("Setting1");</span><span class="sxs-lookup"><span data-stu-id="bfdf2-162">var setting = RoleEnvironment.GetConfigurationSettingValue("Setting1");</span></span> |

## <a name="local-storage-path"></a><span data-ttu-id="bfdf2-163">Cesta k místnímu úložišti</span><span class="sxs-lookup"><span data-stu-id="bfdf2-163">Local storage path</span></span>
<span data-ttu-id="bfdf2-164">Načte cestu místní úložiště pro instanci.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-164">Retrieves the local storage path for the instance.</span></span>

| <span data-ttu-id="bfdf2-165">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-165">Type</span></span> | <span data-ttu-id="bfdf2-166">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-166">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-167">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-167">XPath</span></span> |<span data-ttu-id="bfdf2-168">výraz XPath = "/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@path"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-168">xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path"</span></span> |
| <span data-ttu-id="bfdf2-169">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-169">Code</span></span> |<span data-ttu-id="bfdf2-170">var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath;</span><span class="sxs-lookup"><span data-stu-id="bfdf2-170">var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1").RootPath;</span></span> |

## <a name="local-storage-size"></a><span data-ttu-id="bfdf2-171">Velikost místní úložiště</span><span class="sxs-lookup"><span data-stu-id="bfdf2-171">Local storage size</span></span>
<span data-ttu-id="bfdf2-172">Získá velikost místního úložiště pro instanci.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-172">Retrieves the size of the local storage for the instance.</span></span>

| <span data-ttu-id="bfdf2-173">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-173">Type</span></span> | <span data-ttu-id="bfdf2-174">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-174">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-175">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-175">XPath</span></span> |<span data-ttu-id="bfdf2-176">výraz XPath = "/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@sizeInMB"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-176">xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB"</span></span> |
| <span data-ttu-id="bfdf2-177">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-177">Code</span></span> |<span data-ttu-id="bfdf2-178">var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes;</span><span class="sxs-lookup"><span data-stu-id="bfdf2-178">var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1").MaximumSizeInMegabytes;</span></span> |

## <a name="endpoint-protocol"></a><span data-ttu-id="bfdf2-179">Koncový bod protokolu</span><span class="sxs-lookup"><span data-stu-id="bfdf2-179">Endpoint protocol</span></span>
<span data-ttu-id="bfdf2-180">Načte protokol koncový bod pro instanci.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-180">Retrieves the endpoint protocol for the instance.</span></span>

| <span data-ttu-id="bfdf2-181">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-181">Type</span></span> | <span data-ttu-id="bfdf2-182">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-182">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-183">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-183">XPath</span></span> |<span data-ttu-id="bfdf2-184">výraz XPath = "/ RoleEnvironment nebo CurrentInstance nebo koncových bodů nebo koncového bodu [@name= 'koncovém bodě 1']/@protocol"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-184">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol"</span></span> |
| <span data-ttu-id="bfdf2-185">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-185">Code</span></span> |<span data-ttu-id="bfdf2-186">var ochranu = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protokol;</span><span class="sxs-lookup"><span data-stu-id="bfdf2-186">var prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].Protocol;</span></span> |

## <a name="endpoint-ip"></a><span data-ttu-id="bfdf2-187">Koncový bod IP</span><span class="sxs-lookup"><span data-stu-id="bfdf2-187">Endpoint IP</span></span>
<span data-ttu-id="bfdf2-188">Získá zadaný koncový bod IP adresu.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-188">Gets the specified endpoint's IP address.</span></span>

| <span data-ttu-id="bfdf2-189">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-189">Type</span></span> | <span data-ttu-id="bfdf2-190">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-190">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-191">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-191">XPath</span></span> |<span data-ttu-id="bfdf2-192">výraz XPath = "/ RoleEnvironment nebo CurrentInstance nebo koncových bodů nebo koncového bodu [@name= 'koncovém bodě 1']/@address"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-192">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address"</span></span> |
| <span data-ttu-id="bfdf2-193">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-193">Code</span></span> |<span data-ttu-id="bfdf2-194">var adresu = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address</span><span class="sxs-lookup"><span data-stu-id="bfdf2-194">var address = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].IPEndpoint.Address</span></span> |

## <a name="endpoint-port"></a><span data-ttu-id="bfdf2-195">koncový port</span><span class="sxs-lookup"><span data-stu-id="bfdf2-195">Endpoint port</span></span>
<span data-ttu-id="bfdf2-196">Načte koncový port pro instanci.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-196">Retrieves the endpoint port for the instance.</span></span>

| <span data-ttu-id="bfdf2-197">Typ</span><span class="sxs-lookup"><span data-stu-id="bfdf2-197">Type</span></span> | <span data-ttu-id="bfdf2-198">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-198">Example</span></span> |
| --- | --- |
| <span data-ttu-id="bfdf2-199">Výraz XPath</span><span class="sxs-lookup"><span data-stu-id="bfdf2-199">XPath</span></span> |<span data-ttu-id="bfdf2-200">výraz XPath = "/ RoleEnvironment nebo CurrentInstance nebo koncových bodů nebo koncového bodu [@name= 'koncovém bodě 1']/@port"</span><span class="sxs-lookup"><span data-stu-id="bfdf2-200">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port"</span></span> |
| <span data-ttu-id="bfdf2-201">Kód</span><span class="sxs-lookup"><span data-stu-id="bfdf2-201">Code</span></span> |<span data-ttu-id="bfdf2-202">var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port;</span><span class="sxs-lookup"><span data-stu-id="bfdf2-202">var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].IPEndpoint.Port;</span></span> |

## <a name="example"></a><span data-ttu-id="bfdf2-203">Příklad</span><span class="sxs-lookup"><span data-stu-id="bfdf2-203">Example</span></span>
<span data-ttu-id="bfdf2-204">Tady je příklad role pracovního procesu, který vytváří úloha spuštění s proměnnou prostředí s názvem `TestIsEmulated` nastavte na [ @emulated hodnotu xpath](#app-running-in-emulator).</span><span class="sxs-lookup"><span data-stu-id="bfdf2-204">Here is an example of a worker role that creates a startup task with an environment variable named `TestIsEmulated` set to the [@emulated xpath value](#app-running-in-emulator).</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="bfdf2-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bfdf2-205">Next steps</span></span>
<span data-ttu-id="bfdf2-206">Další informace o [souboru ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) souboru.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-206">Learn more about the [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) file.</span></span>

<span data-ttu-id="bfdf2-207">Vytvoření [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) balíčku.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-207">Create a [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) package.</span></span>

<span data-ttu-id="bfdf2-208">Povolit [vzdálené plochy](cloud-services-role-enable-remote-desktop.md) pro roli.</span><span class="sxs-lookup"><span data-stu-id="bfdf2-208">Enable [remote desktop](cloud-services-role-enable-remote-desktop.md) for a role.</span></span>

