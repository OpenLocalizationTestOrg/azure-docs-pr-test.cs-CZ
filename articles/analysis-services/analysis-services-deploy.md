---
title: "aaaDeploy tooAzure služby Analysis Services pomocí rozšíření SSDT | Microsoft Docs"
description: "Zjistěte, jak toodeploy tooan tabulkový model Azure Analysis Services serveru pomocí rozšíření SSDT."
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
ms.openlocfilehash: e3f3771fe32a37b9e0173c274080c647152edd4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-model-from-ssdt"></a><span data-ttu-id="7125c-103">Nasazení modelu ze sady SSDT</span><span class="sxs-lookup"><span data-stu-id="7125c-103">Deploy a model from SSDT</span></span>
<span data-ttu-id="7125c-104">Po vytvoření serveru ve vašem předplatném Azure, jste připravené toodeploy tooit tabulkový model databáze.</span><span class="sxs-lookup"><span data-stu-id="7125c-104">Once you've created a server in your Azure subscription, you're ready toodeploy a tabular model database tooit.</span></span> <span data-ttu-id="7125c-105">Můžete použít SQL Server Data Tools (SSDT) toobuild a nasazení projektu tabulkový model, kterou právě pracujete.</span><span class="sxs-lookup"><span data-stu-id="7125c-105">You can use SQL Server Data Tools (SSDT) toobuild and deploy a tabular model project you're working on.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7125c-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7125c-106">Prerequisites</span></span>
<span data-ttu-id="7125c-107">tooget spuštění, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="7125c-107">tooget started, you need:</span></span>

* <span data-ttu-id="7125c-108">**Server služby Analysis Services** v Azure</span><span class="sxs-lookup"><span data-stu-id="7125c-108">**Analysis Services server** in Azure.</span></span> <span data-ttu-id="7125c-109">Další, najdete v části toolearn [vytvoření serveru Azure Analysis Services](analysis-services-create-server.md).</span><span class="sxs-lookup"><span data-stu-id="7125c-109">toolearn more, see [Create an Azure Analysis Services server](analysis-services-create-server.md).</span></span>
* <span data-ttu-id="7125c-110">**Tabulkový model projektu** v rozšíření SSDT nebo existující tabulkový model na hello 1200 nebo vyšší úroveň kompatibility.</span><span class="sxs-lookup"><span data-stu-id="7125c-110">**Tabular model project** in SSDT or an existing tabular model at hello 1200 or higher compatibility level.</span></span> <span data-ttu-id="7125c-111">Nikdy jste ho nevytvářeli?</span><span class="sxs-lookup"><span data-stu-id="7125c-111">Never created one?</span></span> <span data-ttu-id="7125c-112">Zkuste hello [společnosti Adventure Works Internet prodeje tabulkové modelování kurzu](https://msdn.microsoft.com/library/hh231691.aspx).</span><span class="sxs-lookup"><span data-stu-id="7125c-112">Try hello [Adventure Works Internet sales tabular modeling tutorial](https://msdn.microsoft.com/library/hh231691.aspx).</span></span>
* <span data-ttu-id="7125c-113">**Místní brána** – Pokud jeden nebo více zdrojů dat jsou místně v síti vaší organizace, měli byste tooinstall [místní brána dat](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="7125c-113">**On-premises gateway** - If one or more data sources are on-premises in your organization's network, you need tooinstall an [On-premises data gateway](analysis-services-gateway.md).</span></span> <span data-ttu-id="7125c-114">Hello brány je nutné pro váš server v cloudu hello připojení místní tooyour datového zdroje tooprocess a aktualizace dat v modelu hello.</span><span class="sxs-lookup"><span data-stu-id="7125c-114">hello gateway is necessary for your server in hello cloud connect tooyour on-premises data sources tooprocess and refresh data in hello model.</span></span>

> [!TIP]
> <span data-ttu-id="7125c-115">Před nasazením, ujistěte se, že může zpracovat hello data v tabulkách.</span><span class="sxs-lookup"><span data-stu-id="7125c-115">Before you deploy, make sure you can process hello data in your tables.</span></span> <span data-ttu-id="7125c-116">V SSDT klikněte na **Model** > **Zpracovat** > **Zpracovat vše**.</span><span class="sxs-lookup"><span data-stu-id="7125c-116">In SSDT, click **Model** > **Process** > **Process All**.</span></span> <span data-ttu-id="7125c-117">Pokud se zpracování nepodaří, nebudete moct provést úspěšné nasazení.</span><span class="sxs-lookup"><span data-stu-id="7125c-117">If processing fails, you cannot successfully deploy.</span></span>
> 
> 

## <a name="toodeploy-a-tabular-model-from-ssdt"></a><span data-ttu-id="7125c-118">toodeploy tabulkový model ze rozšíření SSDT</span><span class="sxs-lookup"><span data-stu-id="7125c-118">toodeploy a tabular model from SSDT</span></span>

1. <span data-ttu-id="7125c-119">Než nasadíte, je třeba název serveru tooget hello.</span><span class="sxs-lookup"><span data-stu-id="7125c-119">Before you deploy, you need tooget hello server name.</span></span> <span data-ttu-id="7125c-120">V **portál Azure** > serveru > **přehled** > **název serveru**, název serveru hello kopie.</span><span class="sxs-lookup"><span data-stu-id="7125c-120">In **Azure portal** > server > **Overview** > **Server name**, copy hello server name.</span></span>
   
    ![Získání názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. <span data-ttu-id="7125c-122">V sadě SSDT > **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello > **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="7125c-122">In SSDT > **Solution Explorer**, right-click hello project > **Properties**.</span></span> <span data-ttu-id="7125c-123">Potom v **nasazení** > **Server** vložit název serveru hello.</span><span class="sxs-lookup"><span data-stu-id="7125c-123">Then in **Deployment** > **Server** paste hello server name.</span></span>   
   
    ![Vložení názvu serveru do vlastnosti nasazení serveru](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. <span data-ttu-id="7125c-125">V **Průzkumníku řešení** klikněte pravým tlačítkem na **Vlastnosti** a pak klikněte na **Nasadit**.</span><span class="sxs-lookup"><span data-stu-id="7125c-125">In **Solution Explorer**, right-click **Properties**, then click **Deploy**.</span></span> <span data-ttu-id="7125c-126">Může být výzvami toosign v tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7125c-126">You may be prompted toosign in tooAzure.</span></span>
   
    ![Nasazení tooserver](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    <span data-ttu-id="7125c-128">Stav nasazení se zobrazí v okně Výstup hello i v nasazení.</span><span class="sxs-lookup"><span data-stu-id="7125c-128">Deployment status appears in both hello Output window and in Deploy.</span></span>
   
    ![Stav nasazení](./media/analysis-services-deploy/aas-deploy-status.png)

<span data-ttu-id="7125c-130">To je všechno je tooit!</span><span class="sxs-lookup"><span data-stu-id="7125c-130">That's all there is tooit!</span></span>


## <a name="troubleshooting"></a><span data-ttu-id="7125c-131">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="7125c-131">Troubleshooting</span></span>
<span data-ttu-id="7125c-132">Pokud se nasazení nezdaří, při nasazování metadata, je pravděpodobné, protože rozšíření SSDT se nemohli připojit tooyour serveru.</span><span class="sxs-lookup"><span data-stu-id="7125c-132">If deployment fails when deploying metadata, it's likely because SSDT couldn't connect tooyour server.</span></span> <span data-ttu-id="7125c-133">Ujistěte se, že se můžete připojit server tooyour pomocí aplikace SSMS.</span><span class="sxs-lookup"><span data-stu-id="7125c-133">Make sure you can connect tooyour server using SSMS.</span></span> <span data-ttu-id="7125c-134">Potom zkontrolujte, zda je správný hello vlastnost nasazení serveru pro projekt hello.</span><span class="sxs-lookup"><span data-stu-id="7125c-134">Then make sure hello Deployment Server property for hello project is correct.</span></span>

<span data-ttu-id="7125c-135">Pokud se nasazení nezdaří v tabulce, je pravděpodobné, protože váš server se nemohl připojit zdroj dat tooa.</span><span class="sxs-lookup"><span data-stu-id="7125c-135">If deployment fails on a table, it's likely because your server couldn't connect tooa data source.</span></span> <span data-ttu-id="7125c-136">Pokud zdroj dat je místně v síti vaší organizace, být jisti tooinstall [místní brána dat](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="7125c-136">If your data source is on-premises in your organization's network, be sure tooinstall an [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7125c-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7125c-137">Next steps</span></span>
<span data-ttu-id="7125c-138">Nyní když máte nasazené tooyour serveru tabulkový model, jste připravené tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="7125c-138">Now that you have your tabular model deployed tooyour server, you're ready tooconnect tooit.</span></span> <span data-ttu-id="7125c-139">Můžete [připojit tooit pomocí SSMS](analysis-services-manage.md) toomanage ho.</span><span class="sxs-lookup"><span data-stu-id="7125c-139">You can [connect tooit with SSMS](analysis-services-manage.md) toomanage it.</span></span> <span data-ttu-id="7125c-140">A můžete [připojit pomocí klienta nástroje tooit](analysis-services-connect.md) jako Power BI, Power BI Desktop, nebo aplikaci Excel a zahájení vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="7125c-140">And, you can [connect tooit using a client tool](analysis-services-connect.md) like Power BI, Power BI Desktop, or Excel, and start creating reports.</span></span>

