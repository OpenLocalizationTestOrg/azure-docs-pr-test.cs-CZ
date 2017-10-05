---
title: "Zdroje dat podporované ve službě Azure Analysis Services | Microsoft Docs"
description: "Popisuje zdroje dat, které jsou podporovány pro modely dat v Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 8bd6c3b5a923ce2f3cd0f60af82e59c5cc27cbb4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="data-sources-supported-in-azure-analysis-services"></a><span data-ttu-id="77ae2-103">Zdroje dat podporované ve službě Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="77ae2-103">Data sources supported in Azure Analysis Services</span></span>
<span data-ttu-id="77ae2-104">Servery Azure Analysis Services podporují připojení ke zdrojům dat v cloudu a místně ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="77ae2-104">Azure Analysis Services servers support connecting to data sources in the cloud and on-premises in your organization.</span></span> <span data-ttu-id="77ae2-105">Další podporované zdroje dat se přidávají vždy.</span><span class="sxs-lookup"><span data-stu-id="77ae2-105">Additional supported data sources are being added all the time.</span></span> <span data-ttu-id="77ae2-106">Vraťte se často.</span><span class="sxs-lookup"><span data-stu-id="77ae2-106">Check back often.</span></span> 

<span data-ttu-id="77ae2-107">Aktuálně jsou podporovány následující zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="77ae2-107">The following data sources are currently supported:</span></span>

| <span data-ttu-id="77ae2-108">Cloud</span><span class="sxs-lookup"><span data-stu-id="77ae2-108">Cloud</span></span>  |
|---|
| <span data-ttu-id="77ae2-109">Úložiště objektů Blob v Azure *</span><span class="sxs-lookup"><span data-stu-id="77ae2-109">Azure Blob Storage*</span></span>  |
| <span data-ttu-id="77ae2-110">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="77ae2-110">Azure SQL Database</span></span>  |
| <span data-ttu-id="77ae2-111">Azure datového skladu</span><span class="sxs-lookup"><span data-stu-id="77ae2-111">Azure Data Warehouse</span></span> |


| <span data-ttu-id="77ae2-112">Lokálně</span><span class="sxs-lookup"><span data-stu-id="77ae2-112">On-premises</span></span>  |   |   |   |
|---|---|---|---|
| <span data-ttu-id="77ae2-113">Databáze Access</span><span class="sxs-lookup"><span data-stu-id="77ae2-113">Access Database</span></span>  | <span data-ttu-id="77ae2-114">Složky *</span><span class="sxs-lookup"><span data-stu-id="77ae2-114">Folder*</span></span> | <span data-ttu-id="77ae2-115">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="77ae2-115">Oracle Database</span></span>  | <span data-ttu-id="77ae2-116">Databáze Teradata</span><span class="sxs-lookup"><span data-stu-id="77ae2-116">Teradata Database</span></span> |
| <span data-ttu-id="77ae2-117">Active Directory *</span><span class="sxs-lookup"><span data-stu-id="77ae2-117">Active Directory*</span></span>  | <span data-ttu-id="77ae2-118">Dokument JSON *</span><span class="sxs-lookup"><span data-stu-id="77ae2-118">JSON document*</span></span>  | <span data-ttu-id="77ae2-119">Databáze Postgre SQL *</span><span class="sxs-lookup"><span data-stu-id="77ae2-119">Postgre SQL Database*</span></span>  |<span data-ttu-id="77ae2-120">XML tabulky *</span><span class="sxs-lookup"><span data-stu-id="77ae2-120">XML table*</span></span> |
| <span data-ttu-id="77ae2-121">Analysis Services</span><span class="sxs-lookup"><span data-stu-id="77ae2-121">Analysis Services</span></span>  | <span data-ttu-id="77ae2-122">Řádky z binárního souboru *</span><span class="sxs-lookup"><span data-stu-id="77ae2-122">Lines from binary*</span></span>  | <span data-ttu-id="77ae2-123">SAP HANA *</span><span class="sxs-lookup"><span data-stu-id="77ae2-123">SAP HANA*</span></span>  |
| <span data-ttu-id="77ae2-124">Analytics Platform System</span><span class="sxs-lookup"><span data-stu-id="77ae2-124">Analytics Platform System</span></span>  | <span data-ttu-id="77ae2-125">Databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="77ae2-125">MySQL Database</span></span>  | <span data-ttu-id="77ae2-126">SAP Business Warehouse *</span><span class="sxs-lookup"><span data-stu-id="77ae2-126">SAP Business Warehouse*</span></span>  | |
| <span data-ttu-id="77ae2-127">Dynamics CRM *</span><span class="sxs-lookup"><span data-stu-id="77ae2-127">Dynamics CRM*</span></span>  | <span data-ttu-id="77ae2-128">Informační kanál OData *</span><span class="sxs-lookup"><span data-stu-id="77ae2-128">OData Feed*</span></span>  | <span data-ttu-id="77ae2-129">SharePoint *</span><span class="sxs-lookup"><span data-stu-id="77ae2-129">SharePoint*</span></span>  |
| <span data-ttu-id="77ae2-130">Sešit aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="77ae2-130">Excel workbook</span></span>  | <span data-ttu-id="77ae2-131">Dotaz rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="77ae2-131">ODBC query</span></span>  | <span data-ttu-id="77ae2-132">SQL Database</span><span class="sxs-lookup"><span data-stu-id="77ae2-132">SQL Database</span></span>  |
| <span data-ttu-id="77ae2-133">Exchange *</span><span class="sxs-lookup"><span data-stu-id="77ae2-133">Exchange*</span></span>  | <span data-ttu-id="77ae2-134">OLE DB</span><span class="sxs-lookup"><span data-stu-id="77ae2-134">OLE DB</span></span>  | <span data-ttu-id="77ae2-135">Databáze Sybase</span><span class="sxs-lookup"><span data-stu-id="77ae2-135">Sybase Database</span></span>  |

<span data-ttu-id="77ae2-136">\*Tabulkové 1400 modely jenom.</span><span class="sxs-lookup"><span data-stu-id="77ae2-136">\* Tabular 1400 models only.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="77ae2-137">Připojení k místním zdrojům dat vyžadují [místní brána dat](analysis-services-gateway.md) nainstalovat do počítače ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="77ae2-137">Connecting to on-premises data sources require an [On-premises data gateway](analysis-services-gateway.md) installed on a computer in your environment.</span></span>

## <a name="data-providers"></a><span data-ttu-id="77ae2-138">Zprostředkovatelé dat</span><span class="sxs-lookup"><span data-stu-id="77ae2-138">Data providers</span></span>

<span data-ttu-id="77ae2-139">Modely dat v Azure Analysis Services může vyžadovat různé datové zprostředkovatele při připojování k určité zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="77ae2-139">Data models in Azure Analysis Services may require different data providers when connecting to certain data sources.</span></span> <span data-ttu-id="77ae2-140">V některých případech může vrátit tabulkové modely služby připojení ke zdrojům dat pomocí nativního zprostředkovatele, jako je například SQL Server Native Client (SQLNCLI11) k chybě.</span><span class="sxs-lookup"><span data-stu-id="77ae2-140">In some cases, tabular models connecting to data sources using native providers such as SQL Server Native Client (SQLNCLI11) may return an error.</span></span>

<span data-ttu-id="77ae2-141">Zdroje pro datové modely, které se připojují k data v cloudu, například Azure SQL Database, pokud používáte nativní zprostředkovatelé než SQLOLEDB, mohou se zobrazit chybová zpráva: **"Zprostředkovatele 'SQLNCLI11.1' není zaregistrován."**</span><span class="sxs-lookup"><span data-stu-id="77ae2-141">For data models that connect to a cloud data source such as Azure SQL Database, if you use native providers other than SQLOLEDB, you may see error message: **“The provider 'SQLNCLI11.1' is not registered.”**</span></span> <span data-ttu-id="77ae2-142">Nebo, pokud máte model DirectQuery připojení k místním zdrojům dat, pokud používáte nativní poskytovatelů, mohou se zobrazit chybová zpráva: **"Chyba při vytváření sady řádků OLE DB. Nesprávná syntaxe poblíž textu 'LIMIT' "**.</span><span class="sxs-lookup"><span data-stu-id="77ae2-142">Or, if you have a DirectQuery model connecting to on-premises data sources, if you use native providers you may see error message: **“Error creating OLE DB row set. Incorrect syntax near 'LIMIT'”**.</span></span>

<span data-ttu-id="77ae2-143">Následující zdroje dat zprostředkovatele jsou podporovány v paměti nebo datové modely DirectQuery při připojování ke zdrojům dat v cloudu nebo místně:</span><span class="sxs-lookup"><span data-stu-id="77ae2-143">The following datasource providers are supported for in-memory or DirectQuery data models when connecting to data sources in the cloud or on-premises:</span></span>

### <a name="cloud"></a><span data-ttu-id="77ae2-144">Cloud</span><span class="sxs-lookup"><span data-stu-id="77ae2-144">Cloud</span></span>
| <span data-ttu-id="77ae2-145">**Zdroj dat**</span><span class="sxs-lookup"><span data-stu-id="77ae2-145">**Datasource**</span></span> | <span data-ttu-id="77ae2-146">**V paměti**</span><span class="sxs-lookup"><span data-stu-id="77ae2-146">**In-memory**</span></span> | <span data-ttu-id="77ae2-147">**DirectQuery**</span><span class="sxs-lookup"><span data-stu-id="77ae2-147">**DirectQuery**</span></span> |
|  --- | --- | --- |
| <span data-ttu-id="77ae2-148">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="77ae2-148">Azure SQL Data Warehouse</span></span> |<span data-ttu-id="77ae2-149">Zprostředkovatel dat .NET framework pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-149">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="77ae2-150">Zprostředkovatel dat .NET framework pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-150">.NET Framework Data Provider for SQL Server</span></span> |
| <span data-ttu-id="77ae2-151">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="77ae2-151">Azure SQL Database</span></span> |<span data-ttu-id="77ae2-152">Zprostředkovatel dat .NET framework pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-152">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="77ae2-153">Zprostředkovatel dat .NET framework pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-153">.NET Framework Data Provider for SQL Server</span></span> | |

### <a name="on-premises-via-gateway"></a><span data-ttu-id="77ae2-154">Místní (prostřednictvím brány)</span><span class="sxs-lookup"><span data-stu-id="77ae2-154">On-premises (via Gateway)</span></span>
|<span data-ttu-id="77ae2-155">**Zdroj dat**</span><span class="sxs-lookup"><span data-stu-id="77ae2-155">**Datasource**</span></span> | <span data-ttu-id="77ae2-156">**V paměti**</span><span class="sxs-lookup"><span data-stu-id="77ae2-156">**In-memory**</span></span> | <span data-ttu-id="77ae2-157">**DirectQuery**</span><span class="sxs-lookup"><span data-stu-id="77ae2-157">**DirectQuery**</span></span> |
|  --- | --- | --- |
| <span data-ttu-id="77ae2-158">SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-158">SQL Server</span></span> |<span data-ttu-id="77ae2-159">SQL Server Native Client 11.0</span><span class="sxs-lookup"><span data-stu-id="77ae2-159">SQL Server Native Client 11.0</span></span> |<span data-ttu-id="77ae2-160">Zprostředkovatel dat .NET framework pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-160">.NET Framework Data Provider for SQL Server</span></span> |
| <span data-ttu-id="77ae2-161">SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-161">SQL Server</span></span> |<span data-ttu-id="77ae2-162">Zprostředkovatel Microsoft OLE DB pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-162">Microsoft OLE DB Provider for SQL Server</span></span> |<span data-ttu-id="77ae2-163">Zprostředkovatel dat .NET framework pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-163">.NET Framework Data Provider for SQL Server</span></span> | |
| <span data-ttu-id="77ae2-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-164">SQL Server</span></span> |<span data-ttu-id="77ae2-165">Zprostředkovatel dat .NET framework pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-165">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="77ae2-166">Zprostředkovatel dat .NET framework pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-166">.NET Framework Data Provider for SQL Server</span></span> | |
| <span data-ttu-id="77ae2-167">Oracle</span><span class="sxs-lookup"><span data-stu-id="77ae2-167">Oracle</span></span> |<span data-ttu-id="77ae2-168">Zprostředkovatel Microsoft OLE DB pro Oracle</span><span class="sxs-lookup"><span data-stu-id="77ae2-168">Microsoft OLE DB Provider for Oracle</span></span> |<span data-ttu-id="77ae2-169">Zprostředkovatel dat Oracle pro .NET</span><span class="sxs-lookup"><span data-stu-id="77ae2-169">Oracle Data Provider for .NET</span></span> | |
| <span data-ttu-id="77ae2-170">Oracle</span><span class="sxs-lookup"><span data-stu-id="77ae2-170">Oracle</span></span> |<span data-ttu-id="77ae2-171">Zprostředkovatel dat Oracle pro .NET</span><span class="sxs-lookup"><span data-stu-id="77ae2-171">Oracle Data Provider for .NET</span></span> |<span data-ttu-id="77ae2-172">Zprostředkovatel dat Oracle pro .NET</span><span class="sxs-lookup"><span data-stu-id="77ae2-172">Oracle Data Provider for .NET</span></span> | |
| <span data-ttu-id="77ae2-173">Teradata</span><span class="sxs-lookup"><span data-stu-id="77ae2-173">Teradata</span></span> |<span data-ttu-id="77ae2-174">Zprostředkovatel OLE DB pro Teradata</span><span class="sxs-lookup"><span data-stu-id="77ae2-174">OLE DB Provider for Teradata</span></span> |<span data-ttu-id="77ae2-175">Zprostředkovatel dat Teradata pro rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="77ae2-175">Teradata Data Provider for .NET</span></span> | |
| <span data-ttu-id="77ae2-176">Teradata</span><span class="sxs-lookup"><span data-stu-id="77ae2-176">Teradata</span></span> |<span data-ttu-id="77ae2-177">Zprostředkovatel dat Teradata pro rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="77ae2-177">Teradata Data Provider for .NET</span></span> |<span data-ttu-id="77ae2-178">Zprostředkovatel dat Teradata pro rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="77ae2-178">Teradata Data Provider for .NET</span></span> | |
| <span data-ttu-id="77ae2-179">Analytics Platform System</span><span class="sxs-lookup"><span data-stu-id="77ae2-179">Analytics Platform System</span></span> |<span data-ttu-id="77ae2-180">Zprostředkovatel dat .NET framework pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-180">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="77ae2-181">Zprostředkovatel dat .NET framework pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="77ae2-181">.NET Framework Data Provider for SQL Server</span></span> | |

> [!NOTE]
> <span data-ttu-id="77ae2-182">Zkontrolujte, zda 64-bit poskytovatelé jsou nainstalovaní při použití místní brána.</span><span class="sxs-lookup"><span data-stu-id="77ae2-182">Ensure 64-bit providers are installed when using On-premises gateway.</span></span>
> 
> 

<span data-ttu-id="77ae2-183">Při migraci místní SQL Server Analysis Services tabulkový model Azure Analysis Services, může být nutné změnit zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="77ae2-183">When migrating an on-premises SQL Server Analysis Services tabular model to Azure Analysis Services, it may be necessary to change the provider.</span></span>

<span data-ttu-id="77ae2-184">**Pokud chcete zadat zprostředkovatele zdroje dat**</span><span class="sxs-lookup"><span data-stu-id="77ae2-184">**To specify a datasource provider**</span></span>

1. <span data-ttu-id="77ae2-185">V sadě SSDT > **tabulkový Model Explorer** > **zdroje dat**, klikněte pravým tlačítkem na připojení ke zdroji dat a pak klikněte na tlačítko **upravit zdroj dat**.</span><span class="sxs-lookup"><span data-stu-id="77ae2-185">In SSDT > **Tabular Model Explorer** > **Data Sources**, right-click a data source connection, and then click **Edit Data Source**.</span></span>
2. <span data-ttu-id="77ae2-186">V **upravit připojení**, klikněte na tlačítko **Upřesnit** a otevřete okno Vlastnosti zálohy.</span><span class="sxs-lookup"><span data-stu-id="77ae2-186">In **Edit Connection**, click **Advanced** to open the Advance properties window.</span></span>
3. <span data-ttu-id="77ae2-187">V **nastavit rozšířené vlastnosti** > **zprostředkovatelé**, pak vyberte příslušného poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="77ae2-187">In **Set Advanced Properties** > **Providers**, then select the appropriate provider.</span></span>

## <a name="impersonation"></a><span data-ttu-id="77ae2-188">Zosobnění</span><span class="sxs-lookup"><span data-stu-id="77ae2-188">Impersonation</span></span>
<span data-ttu-id="77ae2-189">V některých případech může být nutné zadat účet jiné zosobnění.</span><span class="sxs-lookup"><span data-stu-id="77ae2-189">In some cases, it may be necessary to specify a different impersonation account.</span></span> <span data-ttu-id="77ae2-190">Účet zosobnění lze zadat v rozšíření SSDT nebo SSMS.</span><span class="sxs-lookup"><span data-stu-id="77ae2-190">Impersonation account can be specified in SSDT or SSMS.</span></span>

<span data-ttu-id="77ae2-191">Pro místní zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="77ae2-191">For on-premises data sources:</span></span>

* <span data-ttu-id="77ae2-192">Pokud používáte ověřování SQL, musí být zosobnění účtu služby.</span><span class="sxs-lookup"><span data-stu-id="77ae2-192">If using SQL authentication, impersonation should be Service Account.</span></span>
* <span data-ttu-id="77ae2-193">Pokud používáte ověřování systému Windows, nastavte uživatele nebo heslo systému Windows.</span><span class="sxs-lookup"><span data-stu-id="77ae2-193">If using Windows authentication, set Windows user/password.</span></span> <span data-ttu-id="77ae2-194">Ověřování systému Windows s konkrétní zosobnění účtu systému SQL Server, je podporováno pouze pro modely dat v paměti.</span><span class="sxs-lookup"><span data-stu-id="77ae2-194">For SQL Server, Windows authentication with a specific impersonation account is supported only for in-memory data models.</span></span>

<span data-ttu-id="77ae2-195">Pro cloudové zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="77ae2-195">For cloud data sources:</span></span>

* <span data-ttu-id="77ae2-196">Pokud používáte ověřování SQL, musí být zosobnění účtu služby.</span><span class="sxs-lookup"><span data-stu-id="77ae2-196">If using SQL authentication, impersonation should be Service Account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="77ae2-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77ae2-197">Next steps</span></span>
<span data-ttu-id="77ae2-198">Pokud máte místní zdroje dat, je nutné nainstalovat [místní brána](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="77ae2-198">If you have on-premises data sources, be sure to install the [On-premises gateway](analysis-services-gateway.md).</span></span>   
<span data-ttu-id="77ae2-199">Další informace o správě váš server v rozšíření SSDT nebo SSMS najdete v tématu [správě serveru](analysis-services-manage.md).</span><span class="sxs-lookup"><span data-stu-id="77ae2-199">To learn more about managing your server in SSDT or SSMS, see [Manage your server](analysis-services-manage.md).</span></span>

