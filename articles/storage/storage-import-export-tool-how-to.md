---
title: "Pomocí nástroje Azure Import/Export | Microsoft Docs"
description: "Další informace o použití nástroje importu a exportu Příprava pevné disky pro úlohy importu, opravte úlohu importu nebo opravit úlohy exportu."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f77535bb-d577-438a-bdd3-e15a82e0c543
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 86073f5d15253d658fcb371e913dd3a543a2b075
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-azure-importexport-tool"></a><span data-ttu-id="9c53c-103">Pomocí nástroje Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="9c53c-103">Using the Azure Import/Export Tool</span></span> 

<span data-ttu-id="9c53c-104">Nástroj Azure Import/Export (WAImportExport.exe) slouží k vytvoření a Správa úloh pro službu Azure Import/Export umožňuje přenos velkých objemů dat do nebo z Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="9c53c-104">The Azure Import/Export Tool (WAImportExport.exe) is used to create and manage jobs for the Azure Import/Export service, enabling you to transfer large amounts of data into or out of Azure Blob Storage.</span></span>

<span data-ttu-id="9c53c-105">Tato dokumentace je pro nejnovější verzi nástroje Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="9c53c-105">This documentation is for the most recent version of the Azure Import/Export Tool.</span></span> <span data-ttu-id="9c53c-106">Informace o použití nástroje v1, najdete v tématu [pomocí nástroje Azure Import/Export v1](storage-import-export-tool-how-to-v1.md).</span><span class="sxs-lookup"><span data-stu-id="9c53c-106">For information about using the v1 tool, please see [Using the Azure Import/Export Tool v1](storage-import-export-tool-how-to-v1.md).</span></span>

<span data-ttu-id="9c53c-107">V těchto článcích uvidíte, jak používat nástroj provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="9c53c-107">In these articles, you will see how to use the tool to do the following:</span></span>  

- <span data-ttu-id="9c53c-108">Instalace a nastavení nástroj importu a exportu.</span><span class="sxs-lookup"><span data-stu-id="9c53c-108">Install and set up the Import/Export Tool.</span></span>
- <span data-ttu-id="9c53c-109">Připravte pevné disky pro úlohu, kde můžete importovat data z jednotky do úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="9c53c-109">Prepare your hard drives for a job where you import data from your drives to Azure Blob Storage.</span></span>
- <span data-ttu-id="9c53c-110">Zkontrolujte stav úlohy s kopírovat soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="9c53c-110">Review the status of a job with Copy Log Files.</span></span> 
- <span data-ttu-id="9c53c-111">Opravte úlohu importu.</span><span class="sxs-lookup"><span data-stu-id="9c53c-111">Repair an import job.</span></span> 
- <span data-ttu-id="9c53c-112">Opravte úlohu exportu.</span><span class="sxs-lookup"><span data-stu-id="9c53c-112">Repair an export job.</span></span> 
- <span data-ttu-id="9c53c-113">Řešení potíží s nástroj Azure Import/Export, v případě, že došlo k potížím při během procesu.</span><span class="sxs-lookup"><span data-stu-id="9c53c-113">Troubleshoot the Azure Import/Export Tool, in case you had a problem during process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9c53c-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c53c-114">Next steps</span></span>

* [<span data-ttu-id="9c53c-115">Nastavení nástroje WAImportExport</span><span class="sxs-lookup"><span data-stu-id="9c53c-115">Setting up the WAImportExport tool</span></span>](storage-import-export-tool-setup.md)