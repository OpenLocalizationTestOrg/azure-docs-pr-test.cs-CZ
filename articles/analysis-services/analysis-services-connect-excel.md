---
title: "Připojení ke službě Azure Analysis Services v aplikaci Excel | Microsoft Docs"
description: "Zjistěte, jak se připojit k serveru Azure Analysis Services pomocí aplikace Excel."
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
ms.openlocfilehash: d51b6980120f1cf9bc8d053d463a73ac600b915f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-with-excel"></a><span data-ttu-id="3da7b-103">Propojit s Excelem</span><span class="sxs-lookup"><span data-stu-id="3da7b-103">Connect with Excel</span></span>

<span data-ttu-id="3da7b-104">Jakmile jste vytvořili serveru v Azure a nasadili do ní tabulkový model, budete připraveni k připojení a začít zkoumat data.</span><span class="sxs-lookup"><span data-stu-id="3da7b-104">Once you've created a server in Azure, and deployed a tabular model to it, you're ready to connect and begin exploring data.</span></span>


## <a name="connect-in-excel"></a><span data-ttu-id="3da7b-105">Připojení v aplikaci Excel</span><span class="sxs-lookup"><span data-stu-id="3da7b-105">Connect in Excel</span></span>

<span data-ttu-id="3da7b-106">Připojení k serveru v aplikaci Excel pomocí příkazu Get Data v aplikaci Excel 2016 podporuje.</span><span class="sxs-lookup"><span data-stu-id="3da7b-106">Connecting to a server in Excel is supported by using Get Data in Excel 2016.</span></span> <span data-ttu-id="3da7b-107">Připojení pomocí Průvodce importem tabulky v doplňku Power Pivot není podporováno.</span><span class="sxs-lookup"><span data-stu-id="3da7b-107">Connecting by using the Import Table Wizard in Power Pivot is not supported.</span></span> 

<span data-ttu-id="3da7b-108">**Pokud chcete v aplikaci Excel 2016 připojit**</span><span class="sxs-lookup"><span data-stu-id="3da7b-108">**To connect in Excel 2016**</span></span>

1. <span data-ttu-id="3da7b-109">V aplikaci Excel 2016, na **Data** pásu karet, klikněte na tlačítko **načíst externí Data** > **z jiných zdrojů** > **ze služby Analysis Services** .</span><span class="sxs-lookup"><span data-stu-id="3da7b-109">In Excel 2016, on the **Data** ribbon, click **Get External Data** > **From Other Sources** > **From Analysis Services**.</span></span>

2. <span data-ttu-id="3da7b-110">V průvodci připojení dat v **název serveru**, zadejte název serveru, včetně protokolu a identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="3da7b-110">In the Data Connection Wizard, in **Server name**, enter the server name including protocol and URI.</span></span> <span data-ttu-id="3da7b-111">Potom v **přihlašovací údaje**, vyberte **použít následující uživatelské jméno a heslo**a pak zadejte název organizační uživatele, například nancy@adventureworks.coma heslo.</span><span class="sxs-lookup"><span data-stu-id="3da7b-111">Then, in **Logon credentials**, select **Use the following User Name and Password**, and then type the organizational user name, for example nancy@adventureworks.com, and password.</span></span>

    ![Připojení z aplikace Excel přihlášení](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. <span data-ttu-id="3da7b-113">V **vybrat databázi a tabulku**, vyberte databázi a modelu nebo perspektivy a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="3da7b-113">In **Select Database and Table**, select the database and model or perspective, and then click **Finish**.</span></span>
   
    ![Připojení z vyberte model aplikace Excel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a><span data-ttu-id="3da7b-115">Viz také</span><span class="sxs-lookup"><span data-stu-id="3da7b-115">See also</span></span>
<span data-ttu-id="3da7b-116">[Knihovny klienta](analysis-services-data-providers.md) </span><span class="sxs-lookup"><span data-stu-id="3da7b-116">[Client libraries](analysis-services-data-providers.md) </span></span>  
[<span data-ttu-id="3da7b-117">Správa serveru</span><span class="sxs-lookup"><span data-stu-id="3da7b-117">Manage your server</span></span>](analysis-services-manage.md)     


