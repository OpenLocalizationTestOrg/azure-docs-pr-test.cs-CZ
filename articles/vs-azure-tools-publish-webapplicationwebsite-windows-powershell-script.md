---
title: "aaaPublish-WebApplicationWebSite (skript prostředí Windows PowerShell) | Microsoft Docs"
description: "Zjistěte, jak toopublish webového projektu tooan webu Azure. Tento skript vytvoří hello požadované prostředky ve vašem předplatném Azure, pokud ještě neexistují."
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
ms.openlocfilehash: d46904e30e3c2e040e57888fa31543e8e366527f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a><span data-ttu-id="0408d-104">Publikování-WebApplicationWebSite (skript prostředí Windows PowerShell)</span><span class="sxs-lookup"><span data-stu-id="0408d-104">Publish-WebApplicationWebSite (Windows PowerShell script)</span></span>
## <a name="syntax"></a><span data-ttu-id="0408d-105">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="0408d-105">Syntax</span></span>
<span data-ttu-id="0408d-106">Publikuje webového projektu tooan webu Azure.</span><span class="sxs-lookup"><span data-stu-id="0408d-106">Publishes a web project tooan Azure website.</span></span> <span data-ttu-id="0408d-107">skript Hello hello požadované prostředky vytvoří ve vašem předplatném Azure, pokud ještě neexistují.</span><span class="sxs-lookup"><span data-stu-id="0408d-107">hello script creates hello required resources in your Azure subscription if they don't exist.</span></span>

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a><span data-ttu-id="0408d-108">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="0408d-108">Configuration</span></span>
<span data-ttu-id="0408d-109">Hello cesta toohello JSON konfigurační soubor, který popisuje podrobnosti hello hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="0408d-109">hello path toohello JSON configuration file that describes hello details of hello deployment.</span></span>

| <span data-ttu-id="0408d-110">Parametr</span><span class="sxs-lookup"><span data-stu-id="0408d-110">Parameter</span></span> | <span data-ttu-id="0408d-111">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="0408d-111">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="0408d-112">Aliasy</span><span class="sxs-lookup"><span data-stu-id="0408d-112">Aliases</span></span> |<span data-ttu-id="0408d-113">None</span><span class="sxs-lookup"><span data-stu-id="0408d-113">none</span></span> |
| <span data-ttu-id="0408d-114">Povinné?</span><span class="sxs-lookup"><span data-stu-id="0408d-114">Required?</span></span> |<span data-ttu-id="0408d-115">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="0408d-115">true</span></span> |
| <span data-ttu-id="0408d-116">Pozice</span><span class="sxs-lookup"><span data-stu-id="0408d-116">Position</span></span> |<span data-ttu-id="0408d-117">S názvem</span><span class="sxs-lookup"><span data-stu-id="0408d-117">named</span></span> |
| <span data-ttu-id="0408d-118">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="0408d-118">Default value</span></span> |<span data-ttu-id="0408d-119">None</span><span class="sxs-lookup"><span data-stu-id="0408d-119">none</span></span> |
| <span data-ttu-id="0408d-120">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="0408d-120">Accept pipeline input?</span></span> |<span data-ttu-id="0408d-121">False</span><span class="sxs-lookup"><span data-stu-id="0408d-121">false</span></span> |
| <span data-ttu-id="0408d-122">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="0408d-122">Accept wildcard characters?</span></span> |<span data-ttu-id="0408d-123">False</span><span class="sxs-lookup"><span data-stu-id="0408d-123">false</span></span> |

## <a name="subscriptionname"></a><span data-ttu-id="0408d-124">Název_předplatného</span><span class="sxs-lookup"><span data-stu-id="0408d-124">SubscriptionName</span></span>
<span data-ttu-id="0408d-125">Název Hello hello předplatného Azure, které chcete toocreate hello webu v.</span><span class="sxs-lookup"><span data-stu-id="0408d-125">hello name of hello Azure subscription that you want toocreate hello website in.</span></span>

| <span data-ttu-id="0408d-126">Parametr</span><span class="sxs-lookup"><span data-stu-id="0408d-126">Parameter</span></span> | <span data-ttu-id="0408d-127">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="0408d-127">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="0408d-128">Aliasy</span><span class="sxs-lookup"><span data-stu-id="0408d-128">Aliases</span></span> |<span data-ttu-id="0408d-129">None</span><span class="sxs-lookup"><span data-stu-id="0408d-129">none</span></span> |
| <span data-ttu-id="0408d-130">Povinné?</span><span class="sxs-lookup"><span data-stu-id="0408d-130">Required?</span></span> |<span data-ttu-id="0408d-131">False</span><span class="sxs-lookup"><span data-stu-id="0408d-131">false</span></span> |
| <span data-ttu-id="0408d-132">Pozice</span><span class="sxs-lookup"><span data-stu-id="0408d-132">Position</span></span> |<span data-ttu-id="0408d-133">S názvem</span><span class="sxs-lookup"><span data-stu-id="0408d-133">named</span></span> |
| <span data-ttu-id="0408d-134">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="0408d-134">Default value</span></span> |<span data-ttu-id="0408d-135">None</span><span class="sxs-lookup"><span data-stu-id="0408d-135">none</span></span> |
| <span data-ttu-id="0408d-136">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="0408d-136">Accept pipeline input?</span></span> |<span data-ttu-id="0408d-137">False</span><span class="sxs-lookup"><span data-stu-id="0408d-137">false</span></span> |
| <span data-ttu-id="0408d-138">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="0408d-138">Accept wildcard characters?</span></span> |<span data-ttu-id="0408d-139">False</span><span class="sxs-lookup"><span data-stu-id="0408d-139">false</span></span> |

## <a name="webdeploypackage"></a><span data-ttu-id="0408d-140">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="0408d-140">WebDeployPackage</span></span>
<span data-ttu-id="0408d-141">Hello cesta toohello webové nasazení balíčku toopublish toohello webu.</span><span class="sxs-lookup"><span data-stu-id="0408d-141">hello path toohello web deployment package toopublish toohello website.</span></span> <span data-ttu-id="0408d-142">Tento balíček můžete vytvořit pomocí Průvodce publikování webu hello v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0408d-142">You can create this package by using hello Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="0408d-143">Další informace najdete v tématu [Začínáme s Azure Cloud Services a technologií ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span><span class="sxs-lookup"><span data-stu-id="0408d-143">For more information, see [Get started with Azure Cloud Services and ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span></span>

| <span data-ttu-id="0408d-144">Parametr</span><span class="sxs-lookup"><span data-stu-id="0408d-144">Parameter</span></span> | <span data-ttu-id="0408d-145">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="0408d-145">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="0408d-146">Aliasy</span><span class="sxs-lookup"><span data-stu-id="0408d-146">Aliases</span></span> |<span data-ttu-id="0408d-147">None</span><span class="sxs-lookup"><span data-stu-id="0408d-147">none</span></span> |
| <span data-ttu-id="0408d-148">Povinné?</span><span class="sxs-lookup"><span data-stu-id="0408d-148">Required?</span></span> |<span data-ttu-id="0408d-149">False</span><span class="sxs-lookup"><span data-stu-id="0408d-149">false</span></span> |
| <span data-ttu-id="0408d-150">Pozice</span><span class="sxs-lookup"><span data-stu-id="0408d-150">Position</span></span> |<span data-ttu-id="0408d-151">S názvem</span><span class="sxs-lookup"><span data-stu-id="0408d-151">named</span></span> |
| <span data-ttu-id="0408d-152">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="0408d-152">Default value</span></span> |<span data-ttu-id="0408d-153">None</span><span class="sxs-lookup"><span data-stu-id="0408d-153">none</span></span> |
| <span data-ttu-id="0408d-154">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="0408d-154">Accept pipeline input?</span></span> |<span data-ttu-id="0408d-155">False</span><span class="sxs-lookup"><span data-stu-id="0408d-155">false</span></span> |
| <span data-ttu-id="0408d-156">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="0408d-156">Accept wildcard characters?</span></span> |<span data-ttu-id="0408d-157">False</span><span class="sxs-lookup"><span data-stu-id="0408d-157">false</span></span> |

## <a name="databaseserverpassword"></a><span data-ttu-id="0408d-158">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="0408d-158">DatabaseServerPassword</span></span>
<span data-ttu-id="0408d-159">Hello uživatelské jméno a heslo pro hello databázi SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="0408d-159">hello username and password for hello SQL database in Azure.</span></span>

| <span data-ttu-id="0408d-160">Parametr</span><span class="sxs-lookup"><span data-stu-id="0408d-160">Parameter</span></span> | <span data-ttu-id="0408d-161">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="0408d-161">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="0408d-162">Aliasy</span><span class="sxs-lookup"><span data-stu-id="0408d-162">Aliases</span></span> |<span data-ttu-id="0408d-163">None</span><span class="sxs-lookup"><span data-stu-id="0408d-163">none</span></span> |
| <span data-ttu-id="0408d-164">Povinné?</span><span class="sxs-lookup"><span data-stu-id="0408d-164">Required?</span></span> |<span data-ttu-id="0408d-165">False</span><span class="sxs-lookup"><span data-stu-id="0408d-165">false</span></span> |
| <span data-ttu-id="0408d-166">Pozice</span><span class="sxs-lookup"><span data-stu-id="0408d-166">Position</span></span> |<span data-ttu-id="0408d-167">S názvem</span><span class="sxs-lookup"><span data-stu-id="0408d-167">named</span></span> |
| <span data-ttu-id="0408d-168">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="0408d-168">Default value</span></span> |<span data-ttu-id="0408d-169">None</span><span class="sxs-lookup"><span data-stu-id="0408d-169">none</span></span> |
| <span data-ttu-id="0408d-170">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="0408d-170">Accept pipeline input?</span></span> |<span data-ttu-id="0408d-171">False</span><span class="sxs-lookup"><span data-stu-id="0408d-171">false</span></span> |
| <span data-ttu-id="0408d-172">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="0408d-172">Accept wildcard characters?</span></span> |<span data-ttu-id="0408d-173">False</span><span class="sxs-lookup"><span data-stu-id="0408d-173">false</span></span> |

## <a name="sendhostmessagestooutput"></a><span data-ttu-id="0408d-174">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="0408d-174">SendHostMessagesToOutput</span></span>
<span data-ttu-id="0408d-175">V případě hodnoty true tiskové zprávy z hello skriptu toohello výstupního datového proudu.</span><span class="sxs-lookup"><span data-stu-id="0408d-175">If true, print messages from hello script toohello output stream.</span></span>

| <span data-ttu-id="0408d-176">Parametr</span><span class="sxs-lookup"><span data-stu-id="0408d-176">Parameter</span></span> | <span data-ttu-id="0408d-177">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="0408d-177">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="0408d-178">Aliasy</span><span class="sxs-lookup"><span data-stu-id="0408d-178">Aliases</span></span> |<span data-ttu-id="0408d-179">None</span><span class="sxs-lookup"><span data-stu-id="0408d-179">none</span></span> |
| <span data-ttu-id="0408d-180">Povinné?</span><span class="sxs-lookup"><span data-stu-id="0408d-180">Required?</span></span> |<span data-ttu-id="0408d-181">False</span><span class="sxs-lookup"><span data-stu-id="0408d-181">false</span></span> |
| <span data-ttu-id="0408d-182">Pozice</span><span class="sxs-lookup"><span data-stu-id="0408d-182">Position</span></span> |<span data-ttu-id="0408d-183">S názvem</span><span class="sxs-lookup"><span data-stu-id="0408d-183">named</span></span> |
| <span data-ttu-id="0408d-184">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="0408d-184">Default value</span></span> |<span data-ttu-id="0408d-185">False</span><span class="sxs-lookup"><span data-stu-id="0408d-185">false</span></span> |
| <span data-ttu-id="0408d-186">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="0408d-186">Accept pipeline input?</span></span> |<span data-ttu-id="0408d-187">False</span><span class="sxs-lookup"><span data-stu-id="0408d-187">false</span></span> |
| <span data-ttu-id="0408d-188">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="0408d-188">Accept wildcard characters?</span></span> |<span data-ttu-id="0408d-189">False</span><span class="sxs-lookup"><span data-stu-id="0408d-189">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="0408d-190">Poznámky</span><span class="sxs-lookup"><span data-stu-id="0408d-190">Remarks</span></span>
<span data-ttu-id="0408d-191">Podrobné informace o tom, jak toouse hello skriptu toocreate vývojářů a testovací prostředí, najdete v části [tooDev tooPublish pomocí skriptů Windows PowerShell a testovací prostředí](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="0408d-191">For a complete explanation of how toouse hello script toocreate Dev and Test environments, see [Using Windows PowerShell Scripts tooPublish tooDev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="0408d-192">konfigurační soubor JSON Hello Určuje podrobnosti hello co je toobe nasazení.</span><span class="sxs-lookup"><span data-stu-id="0408d-192">hello JSON configuration file specifies hello details of what is toobe deployed.</span></span> <span data-ttu-id="0408d-193">Obsahuje hello informace, které jste zadali při vytváření projektu hello, jako je například hello název a uživatelské jméno pro web hello.</span><span class="sxs-lookup"><span data-stu-id="0408d-193">It includes hello information that you specified when you created hello project, such as hello name and username for hello website.</span></span> <span data-ttu-id="0408d-194">Zahrnuje také hello tooprovision databáze, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="0408d-194">It also includes hello database tooprovision, if any.</span></span> <span data-ttu-id="0408d-195">Hello následující kód ukazuje konfigurační soubor JSON příklad:</span><span class="sxs-lookup"><span data-stu-id="0408d-195">hello following code shows an example JSON configuration file:</span></span>

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

<span data-ttu-id="0408d-196">Můžete upravit hello JSON konfigurační soubor toochange co je nasazen.</span><span class="sxs-lookup"><span data-stu-id="0408d-196">You can edit hello JSON configuration file toochange what is deployed.</span></span> <span data-ttu-id="0408d-197">Části webu není požadována, ale část databáze hello je volitelný.</span><span class="sxs-lookup"><span data-stu-id="0408d-197">A webSite section is required, but hello database section is optional.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0408d-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0408d-198">Next steps</span></span>
<span data-ttu-id="0408d-199">Další informace najdete v tématu [Publish-WebApplicationVM (skript prostředí Windows PowerShell)](vs-azure-tools-publish-webapplicationvm.md)</span><span class="sxs-lookup"><span data-stu-id="0408d-199">For more information, see [Publish-WebApplicationVM (Windows PowerShell script)](vs-azure-tools-publish-webapplicationvm.md)</span></span>

