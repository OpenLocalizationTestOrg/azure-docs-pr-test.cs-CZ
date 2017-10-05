---
title: "Nasazení do služby Azure Analysis Services pomocí sady SSDT | Dokumentace Microsoftu"
description: "Zjistěte, jak můžete nasadit tabulkový model na server služby Azure Analysis Services pomocí sady SSDT."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 5f1f0ae7-11de-4923-a3da-888b13a3638c
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2017
ms.author: owend
ms.openlocfilehash: e9a3aedfb6e55696e1525e226fada1062fd5eda8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-model-from-ssdt"></a><span data-ttu-id="9b86b-103">Nasazení modelu ze sady SSDT</span><span class="sxs-lookup"><span data-stu-id="9b86b-103">Deploy a model from SSDT</span></span>
<span data-ttu-id="9b86b-104">Jakmile ve svém předplatném Azure vytvoříte server, můžete na něj nasadit databázi tabulkového modelu.</span><span class="sxs-lookup"><span data-stu-id="9b86b-104">Once you've created a server in your Azure subscription, you're ready to deploy a tabular model database to it.</span></span> <span data-ttu-id="9b86b-105">K vytvoření a nasazení projektu tabulkového modelu, na kterém pracujete, můžete použít sadu SSDT (SQL Server Data Tools).</span><span class="sxs-lookup"><span data-stu-id="9b86b-105">You can use SQL Server Data Tools (SSDT) to build and deploy a tabular model project you're working on.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="9b86b-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9b86b-106">Prerequisites</span></span>
<span data-ttu-id="9b86b-107">Na začátek budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="9b86b-107">To get started, you need:</span></span>

* <span data-ttu-id="9b86b-108">**Server služby Analysis Services** v Azure</span><span class="sxs-lookup"><span data-stu-id="9b86b-108">**Analysis Services server** in Azure.</span></span> <span data-ttu-id="9b86b-109">Další informace najdete v tématu [Vytvoření serveru služby Azure Analysis Services](analysis-services-create-server.md).</span><span class="sxs-lookup"><span data-stu-id="9b86b-109">To learn more, see [Create an Azure Analysis Services server](analysis-services-create-server.md).</span></span>
* <span data-ttu-id="9b86b-110">**Projekt tabulkového modelu** v sadě SSDT nebo existující tabulkový model na úrovni kompatibility 1200 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="9b86b-110">**Tabular model project** in SSDT or an existing tabular model at the 1200 or higher compatibility level.</span></span> <span data-ttu-id="9b86b-111">Nikdy jste ho nevytvářeli?</span><span class="sxs-lookup"><span data-stu-id="9b86b-111">Never created one?</span></span> <span data-ttu-id="9b86b-112">Vyzkoušejte [Kurz tabulkového modelování Adventure Works Internet Sales](https://msdn.microsoft.com/library/hh231691.aspx).</span><span class="sxs-lookup"><span data-stu-id="9b86b-112">Try the [Adventure Works Internet sales tabular modeling tutorial](https://msdn.microsoft.com/library/hh231691.aspx).</span></span>
* <span data-ttu-id="9b86b-113">**Místní brána** – Pokud máte jeden nebo více místních zdrojů dat v síti organizace, budete si muset nainstalovat [místní bránu dat](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="9b86b-113">**On-premises gateway** - If one or more data sources are on-premises in your organization's network, you need to install an [On-premises data gateway](analysis-services-gateway.md).</span></span> <span data-ttu-id="9b86b-114">Brána je nezbytná pro server v cloudu, aby se mohl připojit k místním zdrojům dat a mohl tak zpracovat a aktualizovat data v modelu.</span><span class="sxs-lookup"><span data-stu-id="9b86b-114">The gateway is necessary for your server in the cloud connect to your on-premises data sources to process and refresh data in the model.</span></span>

> [!TIP]
> <span data-ttu-id="9b86b-115">Než začnete provádět nasazení, ujistěte se, že zvládnete zpracovat data v tabulkách.</span><span class="sxs-lookup"><span data-stu-id="9b86b-115">Before you deploy, make sure you can process the data in your tables.</span></span> <span data-ttu-id="9b86b-116">V SSDT klikněte na **Model** > **Zpracovat** > **Zpracovat vše**.</span><span class="sxs-lookup"><span data-stu-id="9b86b-116">In SSDT, click **Model** > **Process** > **Process All**.</span></span> <span data-ttu-id="9b86b-117">Pokud se zpracování nepodaří, nebudete moct provést úspěšné nasazení.</span><span class="sxs-lookup"><span data-stu-id="9b86b-117">If processing fails, you cannot successfully deploy.</span></span>
> 
> 

## <a name="to-deploy-a-tabular-model-from-ssdt"></a><span data-ttu-id="9b86b-118">Nasazení tabulkového modelu z SSDT</span><span class="sxs-lookup"><span data-stu-id="9b86b-118">To deploy a tabular model from SSDT</span></span>

1. <span data-ttu-id="9b86b-119">Než budete moct provést nasazení, musíte získat název serveru.</span><span class="sxs-lookup"><span data-stu-id="9b86b-119">Before you deploy, you need to get the server name.</span></span> <span data-ttu-id="9b86b-120">Na portálu **Azure Portal** > Server > **Přehled** > **Název serveru** zkopírujte název serveru.</span><span class="sxs-lookup"><span data-stu-id="9b86b-120">In **Azure portal** > server > **Overview** > **Server name**, copy the server name.</span></span>
   
    ![Získání názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. <span data-ttu-id="9b86b-122">V sadě SSDT > **Průzkumník řešení** klikněte pravým tlačítkem myši na projekt > **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="9b86b-122">In SSDT > **Solution Explorer**, right-click the project > **Properties**.</span></span> <span data-ttu-id="9b86b-123">Potom v **Nasazení** > **Server** vložte název serveru.</span><span class="sxs-lookup"><span data-stu-id="9b86b-123">Then in **Deployment** > **Server** paste the server name.</span></span>   
   
    ![Vložení názvu serveru do vlastnosti nasazení serveru](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. <span data-ttu-id="9b86b-125">V **Průzkumníku řešení** klikněte pravým tlačítkem na **Vlastnosti** a pak klikněte na **Nasadit**.</span><span class="sxs-lookup"><span data-stu-id="9b86b-125">In **Solution Explorer**, right-click **Properties**, then click **Deploy**.</span></span> <span data-ttu-id="9b86b-126">Může se zobrazit výzva k přihlášení do Azure.</span><span class="sxs-lookup"><span data-stu-id="9b86b-126">You may be prompted to sign in to Azure.</span></span>
   
    ![Nasazení na server](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    <span data-ttu-id="9b86b-128">Stav nasazení se zobrazí v okně výstupu a nasazení.</span><span class="sxs-lookup"><span data-stu-id="9b86b-128">Deployment status appears in both the Output window and in Deploy.</span></span>
   
    ![Stav nasazení](./media/analysis-services-deploy/aas-deploy-status.png)

<span data-ttu-id="9b86b-130">A je to!</span><span class="sxs-lookup"><span data-stu-id="9b86b-130">That's all there is to it!</span></span>


## <a name="troubleshooting"></a><span data-ttu-id="9b86b-131">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="9b86b-131">Troubleshooting</span></span>
<span data-ttu-id="9b86b-132">Pokud nasazení selže při nasazování metadat, bude to pravděpodobně tím, že se sada SSDT nemohla připojit k serveru.</span><span class="sxs-lookup"><span data-stu-id="9b86b-132">If deployment fails when deploying metadata, it's likely because SSDT couldn't connect to your server.</span></span> <span data-ttu-id="9b86b-133">Ujistěte se, že se k serveru můžete připojit pomocí aplikace SSMS.</span><span class="sxs-lookup"><span data-stu-id="9b86b-133">Make sure you can connect to your server using SSMS.</span></span> <span data-ttu-id="9b86b-134">Potom zkontrolujte, jestli je vlastnost Server nasazení projektu správná.</span><span class="sxs-lookup"><span data-stu-id="9b86b-134">Then make sure the Deployment Server property for the project is correct.</span></span>

<span data-ttu-id="9b86b-135">Pokud nasazení selže u tabulky, bude to pravděpodobně tím, že se server nemohl připojit ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="9b86b-135">If deployment fails on a table, it's likely because your server couldn't connect to a data source.</span></span> <span data-ttu-id="9b86b-136">Pokud je váš zdroj dat místní v síti organizace, nezapomeňte nainstalovat [místní bránu dat](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="9b86b-136">If your data source is on-premises in your organization's network, be sure to install an [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b86b-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b86b-137">Next steps</span></span>
<span data-ttu-id="9b86b-138">Teď když jste tabulkový model nasadili na váš server, můžete se k němu připojit.</span><span class="sxs-lookup"><span data-stu-id="9b86b-138">Now that you have your tabular model deployed to your server, you're ready to connect to it.</span></span> <span data-ttu-id="9b86b-139">Můžete [se k němu připojit pomocí SSMS](analysis-services-manage.md) a spravovat ho.</span><span class="sxs-lookup"><span data-stu-id="9b86b-139">You can [connect to it with SSMS](analysis-services-manage.md) to manage it.</span></span> <span data-ttu-id="9b86b-140">Můžete [se k němu připojit také pomocí klientského nástroje](analysis-services-connect.md), jako je například Power BI, Power BI Desktop nebo Excel, a začít vytvářet sestavy.</span><span class="sxs-lookup"><span data-stu-id="9b86b-140">And, you can [connect to it using a client tool](analysis-services-connect.md) like Power BI, Power BI Desktop, or Excel, and start creating reports.</span></span>

