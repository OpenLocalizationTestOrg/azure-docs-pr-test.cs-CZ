---
title: "Pomocí nástroje Azure Import/Export - v1 | Microsoft Docs"
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
ms.date: 1/15/2017
ms.author: muralikk
ms.openlocfilehash: 4ce2273cc0dcc456c2edc8c5dd2fc22496f20380
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-the-azure-importexport-tool-classic-deployment-model"></a><span data-ttu-id="7301f-103">Pomocí nástroje Azure Import/Export (modelu nasazení classic)</span><span class="sxs-lookup"><span data-stu-id="7301f-103">Using the Azure Import/Export Tool (classic deployment model)</span></span>

<span data-ttu-id="7301f-104">Nástroj Azure Import/Export (WAImportExport.exe) slouží k vytvoření a Správa úloh pro službu Azure Import/Export umožňuje přenos velkých objemů dat do nebo z Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="7301f-104">The Azure Import/Export Tool (WAImportExport.exe) is used to create and manage jobs for the Azure Import/Export service, enabling you to transfer large amounts of data into or out of Azure Blob Storage.</span></span>

<span data-ttu-id="7301f-105">Tato dokumentace je pro model nasazení classic nástroj Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="7301f-105">This documentation is for the classic deployment model of the Azure Import/Export Tool.</span></span> <span data-ttu-id="7301f-106">Informace o používání nejnovější verzi nástroje najdete v tématu [pomocí nástroje Azure Import/Export](../storage-import-export-tool-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="7301f-106">For information about using the most recent version of the tool, see [Using the Azure Import/Export Tool](../storage-import-export-tool-how-to.md).</span></span>

<span data-ttu-id="7301f-107">V následujících článcích ukazují, jak na:</span><span class="sxs-lookup"><span data-stu-id="7301f-107">The following articles show you how to:</span></span>

- <span data-ttu-id="7301f-108">Instalace a nastavení nástroj importu a exportu.</span><span class="sxs-lookup"><span data-stu-id="7301f-108">Install and set up the Import/Export Tool.</span></span>
- <span data-ttu-id="7301f-109">Připravte pevné disky pro úlohu, kde můžete importovat data z jednotky do úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="7301f-109">Prepare your hard drives for a job where you import data from your drives to Azure Blob Storage.</span></span>
- <span data-ttu-id="7301f-110">Zkontrolujte stav úlohy s kopírovat soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="7301f-110">Review the status of a job with Copy Log Files.</span></span> 
- <span data-ttu-id="7301f-111">Opravte úlohu importu.</span><span class="sxs-lookup"><span data-stu-id="7301f-111">Repair an import job.</span></span> 
- <span data-ttu-id="7301f-112">Opravte úlohu exportu.</span><span class="sxs-lookup"><span data-stu-id="7301f-112">Repair an export job.</span></span> 
- <span data-ttu-id="7301f-113">Řešení potíží s nástroj Azure Import/Export, v případě, že došlo k potížím při během procesu.</span><span class="sxs-lookup"><span data-stu-id="7301f-113">Troubleshoot the Azure Import/Export Tool, in case you had a problem during process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7301f-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7301f-114">Next steps</span></span>

* [<span data-ttu-id="7301f-115">Nastavení nástroje WAImportExport</span><span class="sxs-lookup"><span data-stu-id="7301f-115">Setting up the WAImportExport tool</span></span>](../storage-import-export-tool-how-to.md)