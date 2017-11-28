---
title: aaaPublish WebApplicationVM | Microsoft Docs
description: "Zjistěte, jak toodeploy webové aplikace tooa virtuálního počítače. Tento skript vytvoří hello požadované prostředky ve vašem předplatném Azure, pokud ještě neexistují."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: e4b52b620bebf44b87ddfc3b19c155bb65111814
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a><span data-ttu-id="3eb27-104">Publikování-WebApplicationVM (skript prostředí Windows PowerShell)</span><span class="sxs-lookup"><span data-stu-id="3eb27-104">Publish-WebApplicationVM (Windows PowerShell script)</span></span>
<span data-ttu-id="3eb27-105">Nasadí webové aplikace tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3eb27-105">Deploys a web application tooa virtual machine.</span></span> <span data-ttu-id="3eb27-106">skript Hello hello požadované prostředky vytvoří ve vašem předplatném Azure, pokud ještě neexistují.</span><span class="sxs-lookup"><span data-stu-id="3eb27-106">hello script creates hello required resources in your Azure subscription if they don't exist.</span></span>

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a><span data-ttu-id="3eb27-107">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="3eb27-107">Configuration</span></span>
<span data-ttu-id="3eb27-108">Hello cesta toohello JSON konfigurační soubor, který popisuje podrobnosti hello hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="3eb27-108">hello path toohello JSON configuration file that describes hello details of hello deployment.</span></span>

| <span data-ttu-id="3eb27-109">Aliasy</span><span class="sxs-lookup"><span data-stu-id="3eb27-109">Aliases</span></span> | <span data-ttu-id="3eb27-110">None</span><span class="sxs-lookup"><span data-stu-id="3eb27-110">none</span></span> |
| --- | --- |
| <span data-ttu-id="3eb27-111">Povinné?</span><span class="sxs-lookup"><span data-stu-id="3eb27-111">Required?</span></span> |<span data-ttu-id="3eb27-112">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="3eb27-112">true</span></span> |
| <span data-ttu-id="3eb27-113">Pozice</span><span class="sxs-lookup"><span data-stu-id="3eb27-113">Position</span></span> |<span data-ttu-id="3eb27-114">S názvem</span><span class="sxs-lookup"><span data-stu-id="3eb27-114">named</span></span> |
| <span data-ttu-id="3eb27-115">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="3eb27-115">Default value</span></span> |<span data-ttu-id="3eb27-116">None</span><span class="sxs-lookup"><span data-stu-id="3eb27-116">none</span></span> |
| <span data-ttu-id="3eb27-117">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="3eb27-117">Accept pipeline input?</span></span> |<span data-ttu-id="3eb27-118">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-118">false</span></span> |
| <span data-ttu-id="3eb27-119">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="3eb27-119">Accept wildcard characters?</span></span> |<span data-ttu-id="3eb27-120">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-120">false</span></span> |

### <a name="subscriptionname"></a><span data-ttu-id="3eb27-121">Název_předplatného</span><span class="sxs-lookup"><span data-stu-id="3eb27-121">SubscriptionName</span></span>
<span data-ttu-id="3eb27-122">Název Hello hello předplatné Azure, ve kterém chcete toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3eb27-122">hello name of hello Azure subscription in which you want toocreate hello virtual machine.</span></span>

| <span data-ttu-id="3eb27-123">Aliasy</span><span class="sxs-lookup"><span data-stu-id="3eb27-123">Aliases</span></span> | <span data-ttu-id="3eb27-124">None</span><span class="sxs-lookup"><span data-stu-id="3eb27-124">none</span></span> |
| --- | --- |
| <span data-ttu-id="3eb27-125">Povinné?</span><span class="sxs-lookup"><span data-stu-id="3eb27-125">Required?</span></span> |<span data-ttu-id="3eb27-126">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-126">false</span></span> |
| <span data-ttu-id="3eb27-127">Pozice</span><span class="sxs-lookup"><span data-stu-id="3eb27-127">Position</span></span> |<span data-ttu-id="3eb27-128">S názvem</span><span class="sxs-lookup"><span data-stu-id="3eb27-128">named</span></span> |
| <span data-ttu-id="3eb27-129">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="3eb27-129">Default value</span></span> |<span data-ttu-id="3eb27-130">Použije první předplatné hello v soubor odběru hello</span><span class="sxs-lookup"><span data-stu-id="3eb27-130">Uses hello first subscription in hello subscription file</span></span> |
| <span data-ttu-id="3eb27-131">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="3eb27-131">Accept pipeline input?</span></span> |<span data-ttu-id="3eb27-132">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-132">false</span></span> |
| <span data-ttu-id="3eb27-133">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="3eb27-133">Accept wildcard characters?</span></span> |<span data-ttu-id="3eb27-134">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-134">false</span></span> |

### <a name="webdeploypackage"></a><span data-ttu-id="3eb27-135">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="3eb27-135">WebDeployPackage</span></span>
<span data-ttu-id="3eb27-136">Hello cesta toohello webové nasazení balíčku toopublish toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3eb27-136">hello path toohello web deployment package toopublish toohello virtual machine.</span></span> <span data-ttu-id="3eb27-137">Tento balíček můžete vytvořit pomocí Průvodce publikování webu hello v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3eb27-137">You can create this package by using hello Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="3eb27-138">V tématu [postupy: vytvoření balíčku pro nasazení webu v sadě Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span><span class="sxs-lookup"><span data-stu-id="3eb27-138">See [How to: Create a Web Deployment Package in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span></span>

| <span data-ttu-id="3eb27-139">Aliasy</span><span class="sxs-lookup"><span data-stu-id="3eb27-139">Aliases</span></span> | <span data-ttu-id="3eb27-140">None</span><span class="sxs-lookup"><span data-stu-id="3eb27-140">none</span></span> |
| --- | --- |
| <span data-ttu-id="3eb27-141">Povinné?</span><span class="sxs-lookup"><span data-stu-id="3eb27-141">Required?</span></span> |<span data-ttu-id="3eb27-142">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-142">false</span></span> |
| <span data-ttu-id="3eb27-143">Pozice</span><span class="sxs-lookup"><span data-stu-id="3eb27-143">Position</span></span> |<span data-ttu-id="3eb27-144">S názvem</span><span class="sxs-lookup"><span data-stu-id="3eb27-144">named</span></span> |
| <span data-ttu-id="3eb27-145">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="3eb27-145">Default value</span></span> |<span data-ttu-id="3eb27-146">None</span><span class="sxs-lookup"><span data-stu-id="3eb27-146">none</span></span> |
| <span data-ttu-id="3eb27-147">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="3eb27-147">Accept pipeline input?</span></span> |<span data-ttu-id="3eb27-148">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-148">false</span></span> |
| <span data-ttu-id="3eb27-149">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="3eb27-149">Accept wildcard characters?</span></span> |<span data-ttu-id="3eb27-150">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-150">false</span></span> |

### <a name="allowuntrusted"></a><span data-ttu-id="3eb27-151">AllowUntrusted</span><span class="sxs-lookup"><span data-stu-id="3eb27-151">AllowUntrusted</span></span>
<span data-ttu-id="3eb27-152">V případě hodnoty true, povolí hello používání certifikátů, které nejsou podepsána důvěryhodnou kořenovou autoritou.</span><span class="sxs-lookup"><span data-stu-id="3eb27-152">If true, allow hello use of certificates that aren't signed by a trusted root authority.</span></span>

| <span data-ttu-id="3eb27-153">Aliasy</span><span class="sxs-lookup"><span data-stu-id="3eb27-153">Aliases</span></span> | <span data-ttu-id="3eb27-154">None</span><span class="sxs-lookup"><span data-stu-id="3eb27-154">none</span></span> |
| --- | --- |
| <span data-ttu-id="3eb27-155">Povinné?</span><span class="sxs-lookup"><span data-stu-id="3eb27-155">Required?</span></span> |<span data-ttu-id="3eb27-156">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-156">false</span></span> |
| <span data-ttu-id="3eb27-157">Pozice</span><span class="sxs-lookup"><span data-stu-id="3eb27-157">Position</span></span> |<span data-ttu-id="3eb27-158">S názvem</span><span class="sxs-lookup"><span data-stu-id="3eb27-158">named</span></span> |
| <span data-ttu-id="3eb27-159">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="3eb27-159">Default value</span></span> |<span data-ttu-id="3eb27-160">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-160">false</span></span> |
| <span data-ttu-id="3eb27-161">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="3eb27-161">Accept pipeline input?</span></span> |<span data-ttu-id="3eb27-162">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-162">false</span></span> |
| <span data-ttu-id="3eb27-163">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="3eb27-163">Accept wildcard characters?</span></span> |<span data-ttu-id="3eb27-164">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-164">false</span></span> |

### <a name="vmpassword"></a><span data-ttu-id="3eb27-165">VMPassword</span><span class="sxs-lookup"><span data-stu-id="3eb27-165">VMPassword</span></span>
<span data-ttu-id="3eb27-166">Hello pověření pro účet hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3eb27-166">hello credentials for hello virtual machine account.</span></span> <span data-ttu-id="3eb27-167">Příklad: - VMPassword @{Name = "admin"; Heslo = "password"}</span><span class="sxs-lookup"><span data-stu-id="3eb27-167">Example: -VMPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="3eb27-168">Aliasy</span><span class="sxs-lookup"><span data-stu-id="3eb27-168">Aliases</span></span> | <span data-ttu-id="3eb27-169">None</span><span class="sxs-lookup"><span data-stu-id="3eb27-169">none</span></span> |
| --- | --- |
| <span data-ttu-id="3eb27-170">Povinné?</span><span class="sxs-lookup"><span data-stu-id="3eb27-170">Required?</span></span> |<span data-ttu-id="3eb27-171">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-171">false</span></span> |
| <span data-ttu-id="3eb27-172">Pozice</span><span class="sxs-lookup"><span data-stu-id="3eb27-172">Position</span></span> |<span data-ttu-id="3eb27-173">S názvem</span><span class="sxs-lookup"><span data-stu-id="3eb27-173">named</span></span> |
| <span data-ttu-id="3eb27-174">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="3eb27-174">Default value</span></span> |<span data-ttu-id="3eb27-175">None</span><span class="sxs-lookup"><span data-stu-id="3eb27-175">none</span></span> |
| <span data-ttu-id="3eb27-176">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="3eb27-176">Accept pipeline input?</span></span> |<span data-ttu-id="3eb27-177">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-177">false</span></span> |
| <span data-ttu-id="3eb27-178">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="3eb27-178">Accept wildcard characters?</span></span> |<span data-ttu-id="3eb27-179">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-179">false</span></span> |

### <a name="databaseserverpassword"></a><span data-ttu-id="3eb27-180">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="3eb27-180">DatabaseServerPassword</span></span>
<span data-ttu-id="3eb27-181">Hello přihlašovací údaje pro databázi SQL hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="3eb27-181">hello credentials for hello SQL database in Azure.</span></span> <span data-ttu-id="3eb27-182">Příklad: - DatabaseServerPassword @{Name = "admin"; Heslo = "password"}</span><span class="sxs-lookup"><span data-stu-id="3eb27-182">Example: -DatabaseServerPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="3eb27-183">Aliasy</span><span class="sxs-lookup"><span data-stu-id="3eb27-183">Aliases</span></span> | <span data-ttu-id="3eb27-184">None</span><span class="sxs-lookup"><span data-stu-id="3eb27-184">none</span></span> |
| --- | --- |
| <span data-ttu-id="3eb27-185">Povinné?</span><span class="sxs-lookup"><span data-stu-id="3eb27-185">Required?</span></span> |<span data-ttu-id="3eb27-186">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-186">false</span></span> |
| <span data-ttu-id="3eb27-187">Pozice</span><span class="sxs-lookup"><span data-stu-id="3eb27-187">Position</span></span> |<span data-ttu-id="3eb27-188">S názvem</span><span class="sxs-lookup"><span data-stu-id="3eb27-188">named</span></span> |
| <span data-ttu-id="3eb27-189">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="3eb27-189">Default value</span></span> |<span data-ttu-id="3eb27-190">None</span><span class="sxs-lookup"><span data-stu-id="3eb27-190">none</span></span> |
| <span data-ttu-id="3eb27-191">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="3eb27-191">Accept pipeline input?</span></span> |<span data-ttu-id="3eb27-192">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-192">false</span></span> |
| <span data-ttu-id="3eb27-193">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="3eb27-193">Accept wildcard characters?</span></span> |<span data-ttu-id="3eb27-194">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-194">false</span></span> |

### <a name="sendhostmessagestooutput"></a><span data-ttu-id="3eb27-195">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="3eb27-195">SendHostMessagesToOutput</span></span>
<span data-ttu-id="3eb27-196">V případě hodnoty true tiskové zprávy z hello skriptu toohello výstupního datového proudu.</span><span class="sxs-lookup"><span data-stu-id="3eb27-196">If true, print messages from hello script toohello output stream.</span></span>

| <span data-ttu-id="3eb27-197">Aliasy</span><span class="sxs-lookup"><span data-stu-id="3eb27-197">Aliases</span></span> | <span data-ttu-id="3eb27-198">None</span><span class="sxs-lookup"><span data-stu-id="3eb27-198">none</span></span> |
| --- | --- |
| <span data-ttu-id="3eb27-199">Povinné?</span><span class="sxs-lookup"><span data-stu-id="3eb27-199">Required?</span></span> |<span data-ttu-id="3eb27-200">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-200">false</span></span> |
| <span data-ttu-id="3eb27-201">Pozice</span><span class="sxs-lookup"><span data-stu-id="3eb27-201">Position</span></span> |<span data-ttu-id="3eb27-202">S názvem</span><span class="sxs-lookup"><span data-stu-id="3eb27-202">named</span></span> |
| <span data-ttu-id="3eb27-203">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="3eb27-203">Default value</span></span> |<span data-ttu-id="3eb27-204">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-204">false</span></span> |
| <span data-ttu-id="3eb27-205">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="3eb27-205">Accept pipeline input?</span></span> |<span data-ttu-id="3eb27-206">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-206">false</span></span> |
| <span data-ttu-id="3eb27-207">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="3eb27-207">Accept wildcard characters?</span></span> |<span data-ttu-id="3eb27-208">False</span><span class="sxs-lookup"><span data-stu-id="3eb27-208">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="3eb27-209">Poznámky</span><span class="sxs-lookup"><span data-stu-id="3eb27-209">Remarks</span></span>
<span data-ttu-id="3eb27-210">Podrobné informace o tom, jak toouse hello skriptu toocreate vývojářů a testovací prostředí, najdete v části [tooDev tooPublish pomocí skriptů Windows PowerShell a testovací prostředí](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="3eb27-210">For a complete explanation of how toouse hello script toocreate Dev and Test environments, see [Using Windows PowerShell Scripts tooPublish tooDev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="3eb27-211">konfigurační soubor JSON Hello Určuje podrobnosti hello co je toobe nasazení.</span><span class="sxs-lookup"><span data-stu-id="3eb27-211">hello JSON configuration file specifies hello details of what is toobe deployed.</span></span> <span data-ttu-id="3eb27-212">Obsahuje hello informace, které jste zadali při vytváření projektu hello, jako je například hello název, skupinu vztahů, image virtuálního pevného disku a velikost hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3eb27-212">It includes hello information that you specified when you created hello project, such as hello name, affinity group, VHD image, and size of hello virtual machine.</span></span> <span data-ttu-id="3eb27-213">Také zahrnuje hello koncových bodů na hello virtuálního počítače, tooprovision databáze hello, pokud existuje a webové parametrů nasazení.</span><span class="sxs-lookup"><span data-stu-id="3eb27-213">It also includes hello endpoints on hello virtual machine, hello databases tooprovision, if any, and web deployment parameters.</span></span> <span data-ttu-id="3eb27-214">Hello následující kód ukazuje konfigurační soubor JSON příklad:</span><span class="sxs-lookup"><span data-stu-id="3eb27-214">hello following code shows an example JSON configuration file:</span></span>

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

<span data-ttu-id="3eb27-215">Můžete upravit hello JSON konfigurační soubor toochange co je zřízený.</span><span class="sxs-lookup"><span data-stu-id="3eb27-215">You can edit hello JSON configuration file toochange what is provisioned.</span></span> <span data-ttu-id="3eb27-216">Virtuálního počítače a cloudové služby jsou povinné, ale část databáze hello je volitelné.</span><span class="sxs-lookup"><span data-stu-id="3eb27-216">A virtual machine and a cloud service are required, but hello database section is optional.</span></span>

