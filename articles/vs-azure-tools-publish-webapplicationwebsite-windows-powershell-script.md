---
title: "Publikování-WebApplicationWebSite (skript prostředí Windows PowerShell) | Microsoft Docs"
description: "Zjistěte, jak publikovat webového projektu do webové stránky Azure. Tento skript vytvoří požadované prostředky ve vašem předplatném Azure, pokud ještě neexistují."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 07d21b7ce6cd8aee1cff704d316e7a2ca8c00437
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a><span data-ttu-id="480d1-104">Publikování-WebApplicationWebSite (skript prostředí Windows PowerShell)</span><span class="sxs-lookup"><span data-stu-id="480d1-104">Publish-WebApplicationWebSite (Windows PowerShell script)</span></span>
## <a name="syntax"></a><span data-ttu-id="480d1-105">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="480d1-105">Syntax</span></span>
<span data-ttu-id="480d1-106">Publikuje webového projektu na web Azure.</span><span class="sxs-lookup"><span data-stu-id="480d1-106">Publishes a web project to an Azure website.</span></span> <span data-ttu-id="480d1-107">Pokud ještě neexistují, vytvoří skript ve vašem předplatném Azure požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="480d1-107">The script creates the required resources in your Azure subscription if they don't exist.</span></span>

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a><span data-ttu-id="480d1-108">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="480d1-108">Configuration</span></span>
<span data-ttu-id="480d1-109">Cesta k souboru konfigurace JSON, který popisuje podrobnosti o nasazení.</span><span class="sxs-lookup"><span data-stu-id="480d1-109">The path to the JSON configuration file that describes the details of the deployment.</span></span>

| <span data-ttu-id="480d1-110">Parametr</span><span class="sxs-lookup"><span data-stu-id="480d1-110">Parameter</span></span> | <span data-ttu-id="480d1-111">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="480d1-111">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="480d1-112">Aliasy</span><span class="sxs-lookup"><span data-stu-id="480d1-112">Aliases</span></span> |<span data-ttu-id="480d1-113">None</span><span class="sxs-lookup"><span data-stu-id="480d1-113">none</span></span> |
| <span data-ttu-id="480d1-114">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="480d1-114">Required?</span></span> |<span data-ttu-id="480d1-115">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="480d1-115">true</span></span> |
| <span data-ttu-id="480d1-116">Pozice</span><span class="sxs-lookup"><span data-stu-id="480d1-116">Position</span></span> |<span data-ttu-id="480d1-117">S názvem</span><span class="sxs-lookup"><span data-stu-id="480d1-117">named</span></span> |
| <span data-ttu-id="480d1-118">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="480d1-118">Default value</span></span> |<span data-ttu-id="480d1-119">None</span><span class="sxs-lookup"><span data-stu-id="480d1-119">none</span></span> |
| <span data-ttu-id="480d1-120">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="480d1-120">Accept pipeline input?</span></span> |<span data-ttu-id="480d1-121">False</span><span class="sxs-lookup"><span data-stu-id="480d1-121">false</span></span> |
| <span data-ttu-id="480d1-122">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="480d1-122">Accept wildcard characters?</span></span> |<span data-ttu-id="480d1-123">False</span><span class="sxs-lookup"><span data-stu-id="480d1-123">false</span></span> |

## <a name="subscriptionname"></a><span data-ttu-id="480d1-124">Název_předplatného</span><span class="sxs-lookup"><span data-stu-id="480d1-124">SubscriptionName</span></span>
<span data-ttu-id="480d1-125">Název předplatného Azure, který chcete vytvořit web v.</span><span class="sxs-lookup"><span data-stu-id="480d1-125">The name of the Azure subscription that you want to create the website in.</span></span>

| <span data-ttu-id="480d1-126">Parametr</span><span class="sxs-lookup"><span data-stu-id="480d1-126">Parameter</span></span> | <span data-ttu-id="480d1-127">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="480d1-127">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="480d1-128">Aliasy</span><span class="sxs-lookup"><span data-stu-id="480d1-128">Aliases</span></span> |<span data-ttu-id="480d1-129">None</span><span class="sxs-lookup"><span data-stu-id="480d1-129">none</span></span> |
| <span data-ttu-id="480d1-130">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="480d1-130">Required?</span></span> |<span data-ttu-id="480d1-131">False</span><span class="sxs-lookup"><span data-stu-id="480d1-131">false</span></span> |
| <span data-ttu-id="480d1-132">Pozice</span><span class="sxs-lookup"><span data-stu-id="480d1-132">Position</span></span> |<span data-ttu-id="480d1-133">S názvem</span><span class="sxs-lookup"><span data-stu-id="480d1-133">named</span></span> |
| <span data-ttu-id="480d1-134">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="480d1-134">Default value</span></span> |<span data-ttu-id="480d1-135">None</span><span class="sxs-lookup"><span data-stu-id="480d1-135">none</span></span> |
| <span data-ttu-id="480d1-136">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="480d1-136">Accept pipeline input?</span></span> |<span data-ttu-id="480d1-137">False</span><span class="sxs-lookup"><span data-stu-id="480d1-137">false</span></span> |
| <span data-ttu-id="480d1-138">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="480d1-138">Accept wildcard characters?</span></span> |<span data-ttu-id="480d1-139">False</span><span class="sxs-lookup"><span data-stu-id="480d1-139">false</span></span> |

## <a name="webdeploypackage"></a><span data-ttu-id="480d1-140">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="480d1-140">WebDeployPackage</span></span>
<span data-ttu-id="480d1-141">Cesta k balíčku pro nasazení webu k publikování na web.</span><span class="sxs-lookup"><span data-stu-id="480d1-141">The path to the web deployment package to publish to the website.</span></span> <span data-ttu-id="480d1-142">Tento balíček můžete vytvořit pomocí Průvodce Publikovat Web v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="480d1-142">You can create this package by using the Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="480d1-143">Další informace najdete v tématu [Začínáme s Azure Cloud Services a technologií ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span><span class="sxs-lookup"><span data-stu-id="480d1-143">For more information, see [Get started with Azure Cloud Services and ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span></span>

| <span data-ttu-id="480d1-144">Parametr</span><span class="sxs-lookup"><span data-stu-id="480d1-144">Parameter</span></span> | <span data-ttu-id="480d1-145">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="480d1-145">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="480d1-146">Aliasy</span><span class="sxs-lookup"><span data-stu-id="480d1-146">Aliases</span></span> |<span data-ttu-id="480d1-147">None</span><span class="sxs-lookup"><span data-stu-id="480d1-147">none</span></span> |
| <span data-ttu-id="480d1-148">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="480d1-148">Required?</span></span> |<span data-ttu-id="480d1-149">False</span><span class="sxs-lookup"><span data-stu-id="480d1-149">false</span></span> |
| <span data-ttu-id="480d1-150">Pozice</span><span class="sxs-lookup"><span data-stu-id="480d1-150">Position</span></span> |<span data-ttu-id="480d1-151">S názvem</span><span class="sxs-lookup"><span data-stu-id="480d1-151">named</span></span> |
| <span data-ttu-id="480d1-152">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="480d1-152">Default value</span></span> |<span data-ttu-id="480d1-153">None</span><span class="sxs-lookup"><span data-stu-id="480d1-153">none</span></span> |
| <span data-ttu-id="480d1-154">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="480d1-154">Accept pipeline input?</span></span> |<span data-ttu-id="480d1-155">False</span><span class="sxs-lookup"><span data-stu-id="480d1-155">false</span></span> |
| <span data-ttu-id="480d1-156">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="480d1-156">Accept wildcard characters?</span></span> |<span data-ttu-id="480d1-157">False</span><span class="sxs-lookup"><span data-stu-id="480d1-157">false</span></span> |

## <a name="databaseserverpassword"></a><span data-ttu-id="480d1-158">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="480d1-158">DatabaseServerPassword</span></span>
<span data-ttu-id="480d1-159">Uživatelské jméno a heslo pro databázi SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="480d1-159">The username and password for the SQL database in Azure.</span></span>

| <span data-ttu-id="480d1-160">Parametr</span><span class="sxs-lookup"><span data-stu-id="480d1-160">Parameter</span></span> | <span data-ttu-id="480d1-161">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="480d1-161">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="480d1-162">Aliasy</span><span class="sxs-lookup"><span data-stu-id="480d1-162">Aliases</span></span> |<span data-ttu-id="480d1-163">None</span><span class="sxs-lookup"><span data-stu-id="480d1-163">none</span></span> |
| <span data-ttu-id="480d1-164">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="480d1-164">Required?</span></span> |<span data-ttu-id="480d1-165">False</span><span class="sxs-lookup"><span data-stu-id="480d1-165">false</span></span> |
| <span data-ttu-id="480d1-166">Pozice</span><span class="sxs-lookup"><span data-stu-id="480d1-166">Position</span></span> |<span data-ttu-id="480d1-167">S názvem</span><span class="sxs-lookup"><span data-stu-id="480d1-167">named</span></span> |
| <span data-ttu-id="480d1-168">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="480d1-168">Default value</span></span> |<span data-ttu-id="480d1-169">None</span><span class="sxs-lookup"><span data-stu-id="480d1-169">none</span></span> |
| <span data-ttu-id="480d1-170">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="480d1-170">Accept pipeline input?</span></span> |<span data-ttu-id="480d1-171">False</span><span class="sxs-lookup"><span data-stu-id="480d1-171">false</span></span> |
| <span data-ttu-id="480d1-172">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="480d1-172">Accept wildcard characters?</span></span> |<span data-ttu-id="480d1-173">False</span><span class="sxs-lookup"><span data-stu-id="480d1-173">false</span></span> |

## <a name="sendhostmessagestooutput"></a><span data-ttu-id="480d1-174">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="480d1-174">SendHostMessagesToOutput</span></span>
<span data-ttu-id="480d1-175">V případě hodnoty true tiskových zpráv ze skriptu do výstupního datového proudu.</span><span class="sxs-lookup"><span data-stu-id="480d1-175">If true, print messages from the script to the output stream.</span></span>

| <span data-ttu-id="480d1-176">Parametr</span><span class="sxs-lookup"><span data-stu-id="480d1-176">Parameter</span></span> | <span data-ttu-id="480d1-177">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="480d1-177">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="480d1-178">Aliasy</span><span class="sxs-lookup"><span data-stu-id="480d1-178">Aliases</span></span> |<span data-ttu-id="480d1-179">None</span><span class="sxs-lookup"><span data-stu-id="480d1-179">none</span></span> |
| <span data-ttu-id="480d1-180">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="480d1-180">Required?</span></span> |<span data-ttu-id="480d1-181">False</span><span class="sxs-lookup"><span data-stu-id="480d1-181">false</span></span> |
| <span data-ttu-id="480d1-182">Pozice</span><span class="sxs-lookup"><span data-stu-id="480d1-182">Position</span></span> |<span data-ttu-id="480d1-183">S názvem</span><span class="sxs-lookup"><span data-stu-id="480d1-183">named</span></span> |
| <span data-ttu-id="480d1-184">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="480d1-184">Default value</span></span> |<span data-ttu-id="480d1-185">False</span><span class="sxs-lookup"><span data-stu-id="480d1-185">false</span></span> |
| <span data-ttu-id="480d1-186">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="480d1-186">Accept pipeline input?</span></span> |<span data-ttu-id="480d1-187">False</span><span class="sxs-lookup"><span data-stu-id="480d1-187">false</span></span> |
| <span data-ttu-id="480d1-188">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="480d1-188">Accept wildcard characters?</span></span> |<span data-ttu-id="480d1-189">False</span><span class="sxs-lookup"><span data-stu-id="480d1-189">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="480d1-190">Poznámky</span><span class="sxs-lookup"><span data-stu-id="480d1-190">Remarks</span></span>
<span data-ttu-id="480d1-191">Podrobné informace o tom, jak vytvořit pomocí skriptu najdete v prostředí vývoje a testování [pomocí skriptů prostředí PowerShell systému Windows k publikování pro vývoj a testovací prostředí](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="480d1-191">For a complete explanation of how to use the script to create Dev and Test environments, see [Using Windows PowerShell Scripts to Publish to Dev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="480d1-192">Konfigurační soubor JSON Určuje podrobnosti co je k nasazení.</span><span class="sxs-lookup"><span data-stu-id="480d1-192">The JSON configuration file specifies the details of what is to be deployed.</span></span> <span data-ttu-id="480d1-193">Obsahuje informace, kterou jste zadali při vytváření projektu, například název a uživatelské jméno pro web.</span><span class="sxs-lookup"><span data-stu-id="480d1-193">It includes the information that you specified when you created the project, such as the name and username for the website.</span></span> <span data-ttu-id="480d1-194">Zahrnuje také databázi a zřizovat, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="480d1-194">It also includes the database to provision, if any.</span></span> <span data-ttu-id="480d1-195">Následující kód ukazuje konfigurační soubor JSON příklad:</span><span class="sxs-lookup"><span data-stu-id="480d1-195">The following code shows an example JSON configuration file:</span></span>

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

<span data-ttu-id="480d1-196">Můžete upravit konfigurační soubor JSON změnit, co je nasazen.</span><span class="sxs-lookup"><span data-stu-id="480d1-196">You can edit the JSON configuration file to change what is deployed.</span></span> <span data-ttu-id="480d1-197">Části webu není požadována, ale část databáze je volitelný.</span><span class="sxs-lookup"><span data-stu-id="480d1-197">A webSite section is required, but the database section is optional.</span></span>

## <a name="next-steps"></a><span data-ttu-id="480d1-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="480d1-198">Next steps</span></span>
<span data-ttu-id="480d1-199">Další informace najdete v tématu [Publish-WebApplicationVM (skript prostředí Windows PowerShell)](vs-azure-tools-publish-webapplicationvm.md)</span><span class="sxs-lookup"><span data-stu-id="480d1-199">For more information, see [Publish-WebApplicationVM (Windows PowerShell script)](vs-azure-tools-publish-webapplicationvm.md)</span></span>

