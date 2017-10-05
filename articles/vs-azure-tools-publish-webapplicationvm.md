---
title: "Publikování WebApplicationVM | Microsoft Docs"
description: "Zjistěte, jak nasadit webovou aplikaci pro virtuální počítač. Tento skript vytvoří požadované prostředky ve vašem předplatném Azure, pokud ještě neexistují."
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
ms.openlocfilehash: 2738fc1dff50a177a227ae2c7719bd9a192d82ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a><span data-ttu-id="79585-104">Publikování-WebApplicationVM (skript prostředí Windows PowerShell)</span><span class="sxs-lookup"><span data-stu-id="79585-104">Publish-WebApplicationVM (Windows PowerShell script)</span></span>
<span data-ttu-id="79585-105">Nasadí webovou aplikaci pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="79585-105">Deploys a web application to a virtual machine.</span></span> <span data-ttu-id="79585-106">Pokud ještě neexistují, vytvoří skript ve vašem předplatném Azure požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="79585-106">The script creates the required resources in your Azure subscription if they don't exist.</span></span>

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

### <a name="configuration"></a><span data-ttu-id="79585-107">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="79585-107">Configuration</span></span>
<span data-ttu-id="79585-108">Cesta k souboru konfigurace JSON, který popisuje podrobnosti o nasazení.</span><span class="sxs-lookup"><span data-stu-id="79585-108">The path to the JSON configuration file that describes the details of the deployment.</span></span>

| <span data-ttu-id="79585-109">Aliasy</span><span class="sxs-lookup"><span data-stu-id="79585-109">Aliases</span></span> | <span data-ttu-id="79585-110">None</span><span class="sxs-lookup"><span data-stu-id="79585-110">none</span></span> |
| --- | --- |
| <span data-ttu-id="79585-111">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="79585-111">Required?</span></span> |<span data-ttu-id="79585-112">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="79585-112">true</span></span> |
| <span data-ttu-id="79585-113">Pozice</span><span class="sxs-lookup"><span data-stu-id="79585-113">Position</span></span> |<span data-ttu-id="79585-114">S názvem</span><span class="sxs-lookup"><span data-stu-id="79585-114">named</span></span> |
| <span data-ttu-id="79585-115">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="79585-115">Default value</span></span> |<span data-ttu-id="79585-116">None</span><span class="sxs-lookup"><span data-stu-id="79585-116">none</span></span> |
| <span data-ttu-id="79585-117">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="79585-117">Accept pipeline input?</span></span> |<span data-ttu-id="79585-118">False</span><span class="sxs-lookup"><span data-stu-id="79585-118">false</span></span> |
| <span data-ttu-id="79585-119">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="79585-119">Accept wildcard characters?</span></span> |<span data-ttu-id="79585-120">False</span><span class="sxs-lookup"><span data-stu-id="79585-120">false</span></span> |

### <a name="subscriptionname"></a><span data-ttu-id="79585-121">Název_předplatného</span><span class="sxs-lookup"><span data-stu-id="79585-121">SubscriptionName</span></span>
<span data-ttu-id="79585-122">Název předplatného Azure, ve kterém chcete vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="79585-122">The name of the Azure subscription in which you want to create the virtual machine.</span></span>

| <span data-ttu-id="79585-123">Aliasy</span><span class="sxs-lookup"><span data-stu-id="79585-123">Aliases</span></span> | <span data-ttu-id="79585-124">None</span><span class="sxs-lookup"><span data-stu-id="79585-124">none</span></span> |
| --- | --- |
| <span data-ttu-id="79585-125">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="79585-125">Required?</span></span> |<span data-ttu-id="79585-126">False</span><span class="sxs-lookup"><span data-stu-id="79585-126">false</span></span> |
| <span data-ttu-id="79585-127">Pozice</span><span class="sxs-lookup"><span data-stu-id="79585-127">Position</span></span> |<span data-ttu-id="79585-128">S názvem</span><span class="sxs-lookup"><span data-stu-id="79585-128">named</span></span> |
| <span data-ttu-id="79585-129">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="79585-129">Default value</span></span> |<span data-ttu-id="79585-130">Použije první předplatné v souboru předplatného</span><span class="sxs-lookup"><span data-stu-id="79585-130">Uses the first subscription in the subscription file</span></span> |
| <span data-ttu-id="79585-131">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="79585-131">Accept pipeline input?</span></span> |<span data-ttu-id="79585-132">False</span><span class="sxs-lookup"><span data-stu-id="79585-132">false</span></span> |
| <span data-ttu-id="79585-133">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="79585-133">Accept wildcard characters?</span></span> |<span data-ttu-id="79585-134">False</span><span class="sxs-lookup"><span data-stu-id="79585-134">false</span></span> |

### <a name="webdeploypackage"></a><span data-ttu-id="79585-135">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="79585-135">WebDeployPackage</span></span>
<span data-ttu-id="79585-136">Cesta k balíčku pro nasazení webu k publikování do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="79585-136">The path to the web deployment package to publish to the virtual machine.</span></span> <span data-ttu-id="79585-137">Tento balíček můžete vytvořit pomocí Průvodce Publikovat Web v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="79585-137">You can create this package by using the Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="79585-138">V tématu [postupy: vytvoření balíčku pro nasazení webu v sadě Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span><span class="sxs-lookup"><span data-stu-id="79585-138">See [How to: Create a Web Deployment Package in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span></span>

| <span data-ttu-id="79585-139">Aliasy</span><span class="sxs-lookup"><span data-stu-id="79585-139">Aliases</span></span> | <span data-ttu-id="79585-140">None</span><span class="sxs-lookup"><span data-stu-id="79585-140">none</span></span> |
| --- | --- |
| <span data-ttu-id="79585-141">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="79585-141">Required?</span></span> |<span data-ttu-id="79585-142">False</span><span class="sxs-lookup"><span data-stu-id="79585-142">false</span></span> |
| <span data-ttu-id="79585-143">Pozice</span><span class="sxs-lookup"><span data-stu-id="79585-143">Position</span></span> |<span data-ttu-id="79585-144">S názvem</span><span class="sxs-lookup"><span data-stu-id="79585-144">named</span></span> |
| <span data-ttu-id="79585-145">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="79585-145">Default value</span></span> |<span data-ttu-id="79585-146">None</span><span class="sxs-lookup"><span data-stu-id="79585-146">none</span></span> |
| <span data-ttu-id="79585-147">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="79585-147">Accept pipeline input?</span></span> |<span data-ttu-id="79585-148">False</span><span class="sxs-lookup"><span data-stu-id="79585-148">false</span></span> |
| <span data-ttu-id="79585-149">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="79585-149">Accept wildcard characters?</span></span> |<span data-ttu-id="79585-150">False</span><span class="sxs-lookup"><span data-stu-id="79585-150">false</span></span> |

### <a name="allowuntrusted"></a><span data-ttu-id="79585-151">AllowUntrusted</span><span class="sxs-lookup"><span data-stu-id="79585-151">AllowUntrusted</span></span>
<span data-ttu-id="79585-152">V případě hodnoty true povolte používání certifikátů, které nejsou podepsána důvěryhodnou kořenovou autoritou.</span><span class="sxs-lookup"><span data-stu-id="79585-152">If true, allow the use of certificates that aren't signed by a trusted root authority.</span></span>

| <span data-ttu-id="79585-153">Aliasy</span><span class="sxs-lookup"><span data-stu-id="79585-153">Aliases</span></span> | <span data-ttu-id="79585-154">None</span><span class="sxs-lookup"><span data-stu-id="79585-154">none</span></span> |
| --- | --- |
| <span data-ttu-id="79585-155">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="79585-155">Required?</span></span> |<span data-ttu-id="79585-156">False</span><span class="sxs-lookup"><span data-stu-id="79585-156">false</span></span> |
| <span data-ttu-id="79585-157">Pozice</span><span class="sxs-lookup"><span data-stu-id="79585-157">Position</span></span> |<span data-ttu-id="79585-158">S názvem</span><span class="sxs-lookup"><span data-stu-id="79585-158">named</span></span> |
| <span data-ttu-id="79585-159">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="79585-159">Default value</span></span> |<span data-ttu-id="79585-160">False</span><span class="sxs-lookup"><span data-stu-id="79585-160">false</span></span> |
| <span data-ttu-id="79585-161">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="79585-161">Accept pipeline input?</span></span> |<span data-ttu-id="79585-162">False</span><span class="sxs-lookup"><span data-stu-id="79585-162">false</span></span> |
| <span data-ttu-id="79585-163">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="79585-163">Accept wildcard characters?</span></span> |<span data-ttu-id="79585-164">False</span><span class="sxs-lookup"><span data-stu-id="79585-164">false</span></span> |

### <a name="vmpassword"></a><span data-ttu-id="79585-165">VMPassword</span><span class="sxs-lookup"><span data-stu-id="79585-165">VMPassword</span></span>
<span data-ttu-id="79585-166">Pověření pro účet virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="79585-166">The credentials for the virtual machine account.</span></span> <span data-ttu-id="79585-167">Příklad: - VMPassword @{Name = "admin"; Heslo = "password"}</span><span class="sxs-lookup"><span data-stu-id="79585-167">Example: -VMPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="79585-168">Aliasy</span><span class="sxs-lookup"><span data-stu-id="79585-168">Aliases</span></span> | <span data-ttu-id="79585-169">None</span><span class="sxs-lookup"><span data-stu-id="79585-169">none</span></span> |
| --- | --- |
| <span data-ttu-id="79585-170">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="79585-170">Required?</span></span> |<span data-ttu-id="79585-171">False</span><span class="sxs-lookup"><span data-stu-id="79585-171">false</span></span> |
| <span data-ttu-id="79585-172">Pozice</span><span class="sxs-lookup"><span data-stu-id="79585-172">Position</span></span> |<span data-ttu-id="79585-173">S názvem</span><span class="sxs-lookup"><span data-stu-id="79585-173">named</span></span> |
| <span data-ttu-id="79585-174">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="79585-174">Default value</span></span> |<span data-ttu-id="79585-175">None</span><span class="sxs-lookup"><span data-stu-id="79585-175">none</span></span> |
| <span data-ttu-id="79585-176">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="79585-176">Accept pipeline input?</span></span> |<span data-ttu-id="79585-177">False</span><span class="sxs-lookup"><span data-stu-id="79585-177">false</span></span> |
| <span data-ttu-id="79585-178">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="79585-178">Accept wildcard characters?</span></span> |<span data-ttu-id="79585-179">False</span><span class="sxs-lookup"><span data-stu-id="79585-179">false</span></span> |

### <a name="databaseserverpassword"></a><span data-ttu-id="79585-180">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="79585-180">DatabaseServerPassword</span></span>
<span data-ttu-id="79585-181">Přihlašovací údaje pro databázi SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="79585-181">The credentials for the SQL database in Azure.</span></span> <span data-ttu-id="79585-182">Příklad: - DatabaseServerPassword @{Name = "admin"; Heslo = "password"}</span><span class="sxs-lookup"><span data-stu-id="79585-182">Example: -DatabaseServerPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="79585-183">Aliasy</span><span class="sxs-lookup"><span data-stu-id="79585-183">Aliases</span></span> | <span data-ttu-id="79585-184">None</span><span class="sxs-lookup"><span data-stu-id="79585-184">none</span></span> |
| --- | --- |
| <span data-ttu-id="79585-185">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="79585-185">Required?</span></span> |<span data-ttu-id="79585-186">False</span><span class="sxs-lookup"><span data-stu-id="79585-186">false</span></span> |
| <span data-ttu-id="79585-187">Pozice</span><span class="sxs-lookup"><span data-stu-id="79585-187">Position</span></span> |<span data-ttu-id="79585-188">S názvem</span><span class="sxs-lookup"><span data-stu-id="79585-188">named</span></span> |
| <span data-ttu-id="79585-189">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="79585-189">Default value</span></span> |<span data-ttu-id="79585-190">None</span><span class="sxs-lookup"><span data-stu-id="79585-190">none</span></span> |
| <span data-ttu-id="79585-191">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="79585-191">Accept pipeline input?</span></span> |<span data-ttu-id="79585-192">False</span><span class="sxs-lookup"><span data-stu-id="79585-192">false</span></span> |
| <span data-ttu-id="79585-193">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="79585-193">Accept wildcard characters?</span></span> |<span data-ttu-id="79585-194">False</span><span class="sxs-lookup"><span data-stu-id="79585-194">false</span></span> |

### <a name="sendhostmessagestooutput"></a><span data-ttu-id="79585-195">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="79585-195">SendHostMessagesToOutput</span></span>
<span data-ttu-id="79585-196">V případě hodnoty true tiskových zpráv ze skriptu do výstupního datového proudu.</span><span class="sxs-lookup"><span data-stu-id="79585-196">If true, print messages from the script to the output stream.</span></span>

| <span data-ttu-id="79585-197">Aliasy</span><span class="sxs-lookup"><span data-stu-id="79585-197">Aliases</span></span> | <span data-ttu-id="79585-198">None</span><span class="sxs-lookup"><span data-stu-id="79585-198">none</span></span> |
| --- | --- |
| <span data-ttu-id="79585-199">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="79585-199">Required?</span></span> |<span data-ttu-id="79585-200">False</span><span class="sxs-lookup"><span data-stu-id="79585-200">false</span></span> |
| <span data-ttu-id="79585-201">Pozice</span><span class="sxs-lookup"><span data-stu-id="79585-201">Position</span></span> |<span data-ttu-id="79585-202">S názvem</span><span class="sxs-lookup"><span data-stu-id="79585-202">named</span></span> |
| <span data-ttu-id="79585-203">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="79585-203">Default value</span></span> |<span data-ttu-id="79585-204">False</span><span class="sxs-lookup"><span data-stu-id="79585-204">false</span></span> |
| <span data-ttu-id="79585-205">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="79585-205">Accept pipeline input?</span></span> |<span data-ttu-id="79585-206">False</span><span class="sxs-lookup"><span data-stu-id="79585-206">false</span></span> |
| <span data-ttu-id="79585-207">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="79585-207">Accept wildcard characters?</span></span> |<span data-ttu-id="79585-208">False</span><span class="sxs-lookup"><span data-stu-id="79585-208">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="79585-209">Poznámky</span><span class="sxs-lookup"><span data-stu-id="79585-209">Remarks</span></span>
<span data-ttu-id="79585-210">Podrobné informace o tom, jak vytvořit pomocí skriptu najdete v prostředí vývoje a testování [pomocí skriptů prostředí PowerShell systému Windows k publikování pro vývoj a testovací prostředí](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="79585-210">For a complete explanation of how to use the script to create Dev and Test environments, see [Using Windows PowerShell Scripts to Publish to Dev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="79585-211">Konfigurační soubor JSON Určuje podrobnosti co je k nasazení.</span><span class="sxs-lookup"><span data-stu-id="79585-211">The JSON configuration file specifies the details of what is to be deployed.</span></span> <span data-ttu-id="79585-212">Obsahuje informace, které jste zadali při vytváření projektu, například název, skupinu vztahů, image virtuálního pevného disku a velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="79585-212">It includes the information that you specified when you created the project, such as the name, affinity group, VHD image, and size of the virtual machine.</span></span> <span data-ttu-id="79585-213">Také zahrnuje koncových bodů na virtuálním počítači, databází, které chcete zřídit, pokud existuje a webové parametrů nasazení.</span><span class="sxs-lookup"><span data-stu-id="79585-213">It also includes the endpoints on the virtual machine, the databases to provision, if any, and web deployment parameters.</span></span> <span data-ttu-id="79585-214">Následující kód ukazuje konfigurační soubor JSON příklad:</span><span class="sxs-lookup"><span data-stu-id="79585-214">The following code shows an example JSON configuration file:</span></span>

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

<span data-ttu-id="79585-215">Můžete upravit konfigurační soubor JSON změnit, co je zřízený.</span><span class="sxs-lookup"><span data-stu-id="79585-215">You can edit the JSON configuration file to change what is provisioned.</span></span> <span data-ttu-id="79585-216">Virtuální počítač a cloudové služby jsou povinné, ale části databáze je volitelný.</span><span class="sxs-lookup"><span data-stu-id="79585-216">A virtual machine and a cloud service are required, but the database section is optional.</span></span>

