---
title: "Kurz služby Azure Analysis Services – Lekce 13: Nasazení | Dokumentace Microsoftu"
description: "Popisuje, jak nasadit projekt Kurz služby Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/17/2017
ms.author: owend
ms.openlocfilehash: 70dbf5786262f75199270aa8009e03b9b48b8559
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="lesson-13-deploy"></a><span data-ttu-id="3e5e8-103">Lekce 13: Nasazení</span><span class="sxs-lookup"><span data-stu-id="3e5e8-103">Lesson 13: Deploy</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="3e5e8-104">V této lekci nakonfigurujete vlastnosti nasazení – zadáte server služby Azure Analysis Services, na který se má nasazení provést, a název modelu.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-104">In this lesson, you configure deployment properties; specifying an Azure Analysis Services server to deploy to and a name for the model.</span></span> <span data-ttu-id="3e5e8-105">Pak model nasadíte do příslušné instance.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-105">You then deploy the model to that instance.</span></span> <span data-ttu-id="3e5e8-106">Jakmile bude váš model nasazen, uživatelé se k němu budou moci připojit pomocí klientské aplikace pro vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-106">After your model is deployed, users can connect to it by using a reporting client application.</span></span> <span data-ttu-id="3e5e8-107">Další informace najdete v tématu [Nasazení do služby Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).</span><span class="sxs-lookup"><span data-stu-id="3e5e8-107">To learn more, see [Deploy to Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).</span></span>  
  
<span data-ttu-id="3e5e8-108">Odhadovaný čas dokončení této lekce: **5 minut**</span><span class="sxs-lookup"><span data-stu-id="3e5e8-108">Estimated time to complete this lesson: **5 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="3e5e8-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3e5e8-109">Prerequisites</span></span>  
<span data-ttu-id="3e5e8-110">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-110">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="3e5e8-111">Před provedením úkolů v této lekci byste měli mít dokončenou předchozí lekci: [Lekce 12: Analýza v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span><span class="sxs-lookup"><span data-stu-id="3e5e8-111">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 12: Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="3e5e8-112">Abyste mohli nasadit vzdálený server služby Analysis Services, musíte k němu mít [oprávnění správce](../analysis-services-server-admins.md).</span><span class="sxs-lookup"><span data-stu-id="3e5e8-112">You must have [Administrator permissions](../analysis-services-server-admins.md) on the remote Analysis Services server in-order to deploy to it.</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="3e5e8-113">Pokud jste nainstalovali ukázkovou databázi AdventureWorksDW2014 na místní SQL Server a model nasazujete na server služby Azure Analysis Services, vyžaduje se [Místní brána dat](../analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="3e5e8-113">If you installed the AdventureWorksDW2014 sample database on an on-premises SQL Server, and you're deploying your model to an Azure Analysis Services server, an [On-premises data gateway](../analysis-services-gateway.md) is required.</span></span>
  
## <a name="deploy-the-model"></a><span data-ttu-id="3e5e8-114">Nasazení modelu</span><span class="sxs-lookup"><span data-stu-id="3e5e8-114">Deploy the model</span></span>  
  
#### <a name="to-configure-deployment-properties"></a><span data-ttu-id="3e5e8-115">Konfigurace vlastností nasazení</span><span class="sxs-lookup"><span data-stu-id="3e5e8-115">To configure deployment properties</span></span>  

  
1.  <span data-ttu-id="3e5e8-116">V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt **AW Internet Sales** a pak klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-116">In **Solution Explorer**, right-click the **AW Internet Sales** project, and then click **Properties**.</span></span>  
  
2.  <span data-ttu-id="3e5e8-117">V dialogovém okně **AW Internet Sales – Stránky vlastností** v části **Server nasazení** zadejte do vlastnosti **Server** úplný název serveru.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-117">In the **AW Internet Sales Property Pages** dialog box, under **Deployment Server**, in the **Server** property, enter the full server.</span></span>  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  <span data-ttu-id="3e5e8-119">Do vlastnosti **Databáze** zadejte **Adventure Works Internet Sales**.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-119">In the **Database** property, type **Adventure Works Internet Sales**.</span></span>  
  
4.  <span data-ttu-id="3e5e8-120">Do vlastnosti **Název modelu** zadejte **Adventure Works Internet Sales Model**.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-120">In the **Model Name** property, type **Adventure Works Internet Sales Model**.</span></span>  
  
5.  <span data-ttu-id="3e5e8-121">Zkontrolujte výběr a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-121">Verify your selections and then click **OK**.</span></span>  
  
#### <a name="to-deploy-the-adventure-works-internet-sales"></a><span data-ttu-id="3e5e8-122">Nasazení modelu Adventure Works Internet Sales</span><span class="sxs-lookup"><span data-stu-id="3e5e8-122">To deploy the Adventure Works Internet Sales</span></span>
  
1.  <span data-ttu-id="3e5e8-123">V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt **AW Internet Sales** > **Sestavit**.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-123">In **Solution Explorer**, right-click the **AW Internet Sales** project > **Build**.</span></span>  

2.  <span data-ttu-id="3e5e8-124">Klikněte pravým tlačítkem na projekt **AW Internet Sales** > **Nasadit**.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-124">Right-click the **AW Internet Sales** project > **Deploy**.</span></span>

    <span data-ttu-id="3e5e8-125">Při nasazování do služby Azure Analysis Services můžete být vyzváni k zadání účtu.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-125">When deploying to Azure Analysis Services, you may be prompted to enter your account.</span></span> <span data-ttu-id="3e5e8-126">Zadejte účet organizace a heslo, například nancy@adventureworks.com. Tento účet musí být mezi správci serveru.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-126">Enter your organizational account and password, for example nancy@adventureworks.com. This account must be in Admins on the server.</span></span>
  
    <span data-ttu-id="3e5e8-127">Zobrazí se dialogové okno Nasadit ukazující stav nasazení metadat a každé tabulky zahrnuté v modelu.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-127">The Deploy dialog box appears and displays the deployment status of the metadata and each table included in the model.</span></span>  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. <span data-ttu-id="3e5e8-129">Po úspěšném dokončení nasazení pokračujte kliknutím na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-129">When deployment successfully completes, go ahead and click **Close**.</span></span>  
  
## <a name="conclusion"></a><span data-ttu-id="3e5e8-130">Závěr</span><span class="sxs-lookup"><span data-stu-id="3e5e8-130">Conclusion</span></span>  
<span data-ttu-id="3e5e8-131">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3e5e8-131">Congratulations!</span></span> <span data-ttu-id="3e5e8-132">Vytvořili a nasadili jste první tabelární model služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-132">You're finished authoring and deploying your first Analysis Services Tabular model.</span></span> <span data-ttu-id="3e5e8-133">Tento kurz vás provedl dokončením nejběžnějších úkolů v rámci vytváření tabelárního modelu.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-133">This tutorial has helped guide you through completing the most common tasks in creating a tabular model.</span></span> <span data-ttu-id="3e5e8-134">Teď, když je váš model Adventure Works Internet Sales nasazený, můžete ho spravovat pomocí aplikace SQL Server Management Studio, vytvořit skripty pro zpracování a plán zálohování.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-134">Now that your Adventure Works Internet Sales model is deployed, you can use SQL Server Management Studio to manage the model; create process scripts and a backup plan.</span></span> <span data-ttu-id="3e5e8-135">Uživatelé se nyní také můžou k modelu připojit pomocí klientské aplikace pro vytváření sestav, jako je Microsoft Excel nebo Power BI.</span><span class="sxs-lookup"><span data-stu-id="3e5e8-135">Users can also now connect to the model using a reporting client application such as Microsoft Excel or Power BI.</span></span>  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a><span data-ttu-id="3e5e8-137">Co dále?</span><span class="sxs-lookup"><span data-stu-id="3e5e8-137">What's next?</span></span>
<span data-ttu-id="3e5e8-138">[Propojení s Power BI Desktop](../analysis-services-connect-pbi.md) </span><span class="sxs-lookup"><span data-stu-id="3e5e8-138">[Connect with Power BI Desktop](../analysis-services-connect-pbi.md) </span></span>  
<span data-ttu-id="3e5e8-139">[Doplňková lekce – Dynamické zabezpečení](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span><span class="sxs-lookup"><span data-stu-id="3e5e8-139">[Supplemental Lesson - Dynamic security](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span></span>  
<span data-ttu-id="3e5e8-140">[Doplňková lekce – Řádky podrobností](../tutorials/aas-supplemental-lesson-detail-rows.md) </span><span class="sxs-lookup"><span data-stu-id="3e5e8-140">[Supplemental Lesson - Detail rows](../tutorials/aas-supplemental-lesson-detail-rows.md) </span></span>  
[<span data-ttu-id="3e5e8-141">Doplňková lekce – Nepravidelné hierarchie</span><span class="sxs-lookup"><span data-stu-id="3e5e8-141">Supplemental Lesson - Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
