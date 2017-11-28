---
title: "aaaConnect tooAzure služby Analysis Services v aplikaci Excel | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooan Azure Analysis Services serveru pomocí aplikace Excel."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 175b83e7d936716a626aa4b3bf22b5598bb983d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-excel"></a><span data-ttu-id="e7c4e-103">Propojit s Excelem</span><span class="sxs-lookup"><span data-stu-id="e7c4e-103">Connect with Excel</span></span>

<span data-ttu-id="e7c4e-104">Jakmile jste vytvořili serveru v Azure a nasadí tooit tabulkový model, jste připravené tooconnect a začít zkoumat data.</span><span class="sxs-lookup"><span data-stu-id="e7c4e-104">Once you've created a server in Azure, and deployed a tabular model tooit, you're ready tooconnect and begin exploring data.</span></span>


## <a name="connect-in-excel"></a><span data-ttu-id="e7c4e-105">Připojení v aplikaci Excel</span><span class="sxs-lookup"><span data-stu-id="e7c4e-105">Connect in Excel</span></span>

<span data-ttu-id="e7c4e-106">Připojení serveru tooa v aplikaci Excel pomocí příkazu Get Data v aplikaci Excel 2016 podporuje.</span><span class="sxs-lookup"><span data-stu-id="e7c4e-106">Connecting tooa server in Excel is supported by using Get Data in Excel 2016.</span></span> <span data-ttu-id="e7c4e-107">Připojení pomocí hello Průvodce importem tabulky v doplňku Power Pivot není podporováno.</span><span class="sxs-lookup"><span data-stu-id="e7c4e-107">Connecting by using hello Import Table Wizard in Power Pivot is not supported.</span></span> 

<span data-ttu-id="e7c4e-108">**tooconnect v aplikaci Excel 2016**</span><span class="sxs-lookup"><span data-stu-id="e7c4e-108">**tooconnect in Excel 2016**</span></span>

1. <span data-ttu-id="e7c4e-109">V aplikaci Excel 2016 na hello **Data** pásu karet, klikněte na tlačítko **načíst externí Data** > **z jiných zdrojů** > **ze služby Analysis Services** .</span><span class="sxs-lookup"><span data-stu-id="e7c4e-109">In Excel 2016, on hello **Data** ribbon, click **Get External Data** > **From Other Sources** > **From Analysis Services**.</span></span>

2. <span data-ttu-id="e7c4e-110">V hello Průvodce datovým připojením v **název serveru**, zadejte název serveru hello včetně protokolu a identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="e7c4e-110">In hello Data Connection Wizard, in **Server name**, enter hello server name including protocol and URI.</span></span> <span data-ttu-id="e7c4e-111">Potom v **přihlašovací údaje**, vyberte **hello použijte následující uživatelské jméno a heslo**a pak zadejte hello organizační uživatelské jméno, například nancy@adventureworks.coma heslo.</span><span class="sxs-lookup"><span data-stu-id="e7c4e-111">Then, in **Logon credentials**, select **Use hello following User Name and Password**, and then type hello organizational user name, for example nancy@adventureworks.com, and password.</span></span>

    ![Připojení z aplikace Excel přihlášení](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. <span data-ttu-id="e7c4e-113">V **vybrat databázi a tabulku**, vyberte databázi hello a modelu nebo perspektivy a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e7c4e-113">In **Select Database and Table**, select hello database and model or perspective, and then click **Finish**.</span></span>
   
    ![Připojení z vyberte model aplikace Excel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a><span data-ttu-id="e7c4e-115">Viz také</span><span class="sxs-lookup"><span data-stu-id="e7c4e-115">See also</span></span>
<span data-ttu-id="e7c4e-116">[Knihovny klienta](analysis-services-data-providers.md) </span><span class="sxs-lookup"><span data-stu-id="e7c4e-116">[Client libraries](analysis-services-data-providers.md) </span></span>  
[<span data-ttu-id="e7c4e-117">Správa serveru</span><span class="sxs-lookup"><span data-stu-id="e7c4e-117">Manage your server</span></span>](analysis-services-manage.md)     


