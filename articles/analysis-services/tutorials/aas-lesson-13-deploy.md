---
<span data-ttu-id="3818f-101">Title: aaa "Azure Analysis Services kurz lekce 13: nasazení | Microsoft Docs"Popis: Popisuje, jak toodeploy hello kurzu projektu tooAzure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="3818f-101">title: aaa"Azure Analysis Services tutorial lesson 13: Deploy | Microsoft Docs" description:  Describes how toodeploy hello tutorial project tooAzure Analysis Services.</span></span>
<span data-ttu-id="3818f-102">služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "</span><span class="sxs-lookup"><span data-stu-id="3818f-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="3818f-103">MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 07/17/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="3818f-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 07/17/2017 ms.author: owend</span></span>
---
# <a name="lesson-13-deploy"></a><span data-ttu-id="3818f-104">Lekce 13: Nasazení</span><span class="sxs-lookup"><span data-stu-id="3818f-104">Lesson 13: Deploy</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="3818f-105">V této lekci nakonfigurujete vlastnosti nasazení; zadání toodeploy tooand serveru Azure Analysis Services název modelu hello.</span><span class="sxs-lookup"><span data-stu-id="3818f-105">In this lesson, you configure deployment properties; specifying an Azure Analysis Services server toodeploy tooand a name for hello model.</span></span> <span data-ttu-id="3818f-106">Potom nasadíte instanci toothat hello modelu.</span><span class="sxs-lookup"><span data-stu-id="3818f-106">You then deploy hello model toothat instance.</span></span> <span data-ttu-id="3818f-107">Po nasazení modelu uživatelé můžou připojovat tooit pomocí sestav klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="3818f-107">After your model is deployed, users can connect tooit by using a reporting client application.</span></span> <span data-ttu-id="3818f-108">Další, najdete v části toolearn [nasazení služby Analysis Services tooAzure](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).</span><span class="sxs-lookup"><span data-stu-id="3818f-108">toolearn more, see [Deploy tooAzure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).</span></span>  
  
<span data-ttu-id="3818f-109">Odhadovaný čas toocomplete této lekci: **5 minut**</span><span class="sxs-lookup"><span data-stu-id="3818f-109">Estimated time toocomplete this lesson: **5 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="3818f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3818f-110">Prerequisites</span></span>  
<span data-ttu-id="3818f-111">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="3818f-111">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="3818f-112">Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekce 12: analyzovat v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span><span class="sxs-lookup"><span data-stu-id="3818f-112">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 12: Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="3818f-113">Musíte mít [oprávnění správce](../analysis-services-server-admins.md) na hello vzdálené služby Analysis Services serveru v daném pořadí toodeploy tooit.</span><span class="sxs-lookup"><span data-stu-id="3818f-113">You must have [Administrator permissions](../analysis-services-server-admins.md) on hello remote Analysis Services server in-order toodeploy tooit.</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="3818f-114">Pokud jste nainstalovali hello AdventureWorksDW2014 ukázkové databáze na serveru SQL na místě a nasazujete serveru Azure Analysis Services tooan modelu, [místní brána dat](../analysis-services-gateway.md) je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="3818f-114">If you installed hello AdventureWorksDW2014 sample database on an on-premises SQL Server, and you're deploying your model tooan Azure Analysis Services server, an [On-premises data gateway](../analysis-services-gateway.md) is required.</span></span>
  
## <a name="deploy-hello-model"></a><span data-ttu-id="3818f-115">Nasazení modelu hello</span><span class="sxs-lookup"><span data-stu-id="3818f-115">Deploy hello model</span></span>  
  
#### <a name="tooconfigure-deployment-properties"></a><span data-ttu-id="3818f-116">Vlastnosti nasazení tooconfigure</span><span class="sxs-lookup"><span data-stu-id="3818f-116">tooconfigure deployment properties</span></span>  

  
1.  <span data-ttu-id="3818f-117">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **AW Internet prodej** projektu a pak klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="3818f-117">In **Solution Explorer**, right-click hello **AW Internet Sales** project, and then click **Properties**.</span></span>  
  
2.  <span data-ttu-id="3818f-118">V hello **stránky vlastností prodej Internet AW** dialogovém **Server nasazení**, v hello **Server** vlastnost, zadejte celý server hello.</span><span class="sxs-lookup"><span data-stu-id="3818f-118">In hello **AW Internet Sales Property Pages** dialog box, under **Deployment Server**, in hello **Server** property, enter hello full server.</span></span>  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  <span data-ttu-id="3818f-120">V hello **databáze** vlastnosti, typ **společnosti Adventure Works Internet prodej**.</span><span class="sxs-lookup"><span data-stu-id="3818f-120">In hello **Database** property, type **Adventure Works Internet Sales**.</span></span>  
  
4.  <span data-ttu-id="3818f-121">V hello **název modelu** vlastnosti, typ **společnosti Adventure Works Internet prodej modelu**.</span><span class="sxs-lookup"><span data-stu-id="3818f-121">In hello **Model Name** property, type **Adventure Works Internet Sales Model**.</span></span>  
  
5.  <span data-ttu-id="3818f-122">Zkontrolujte výběr a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3818f-122">Verify your selections and then click **OK**.</span></span>  
  
#### <a name="toodeploy-hello-adventure-works-internet-sales"></a><span data-ttu-id="3818f-123">toodeploy hello společnosti Adventure Works Internet prodeje</span><span class="sxs-lookup"><span data-stu-id="3818f-123">toodeploy hello Adventure Works Internet Sales</span></span>
  
1.  <span data-ttu-id="3818f-124">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **AW Internet prodej** Projekt > **sestavení**.</span><span class="sxs-lookup"><span data-stu-id="3818f-124">In **Solution Explorer**, right-click hello **AW Internet Sales** project > **Build**.</span></span>  

2.  <span data-ttu-id="3818f-125">Klikněte pravým tlačítkem na hello **AW Internet prodej** Projekt > **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="3818f-125">Right-click hello **AW Internet Sales** project > **Deploy**.</span></span>

    <span data-ttu-id="3818f-126">Pokud nasazujete tooAzure Analysis Services, může být výzvami tooenter váš účet.</span><span class="sxs-lookup"><span data-stu-id="3818f-126">When deploying tooAzure Analysis Services, you may be prompted tooenter your account.</span></span> <span data-ttu-id="3818f-127">Zadejte účet organizace a heslo, například nancy@adventureworks.com. Tento účet musí být ve Správci na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="3818f-127">Enter your organizational account and password, for example nancy@adventureworks.com. This account must be in Admins on hello server.</span></span>
  
    <span data-ttu-id="3818f-128">Zobrazí se dialogové okno nasadit Hello a zobrazí stav nasazení hello hello metadat a každá tabulka součástí modelu hello.</span><span class="sxs-lookup"><span data-stu-id="3818f-128">hello Deploy dialog box appears and displays hello deployment status of hello metadata and each table included in hello model.</span></span>  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. <span data-ttu-id="3818f-130">Po úspěšném dokončení nasazení pokračujte kliknutím na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="3818f-130">When deployment successfully completes, go ahead and click **Close**.</span></span>  
  
## <a name="conclusion"></a><span data-ttu-id="3818f-131">Závěr</span><span class="sxs-lookup"><span data-stu-id="3818f-131">Conclusion</span></span>  
<span data-ttu-id="3818f-132">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3818f-132">Congratulations!</span></span> <span data-ttu-id="3818f-133">Vytvořili a nasadili jste první tabelární model služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="3818f-133">You're finished authoring and deploying your first Analysis Services Tabular model.</span></span> <span data-ttu-id="3818f-134">Článek pomůže tento kurz vás provede dokončení hello nejčastějších úloh, při vytváření tabulkový model.</span><span class="sxs-lookup"><span data-stu-id="3818f-134">This tutorial has helped guide you through completing hello most common tasks in creating a tabular model.</span></span> <span data-ttu-id="3818f-135">Teď, když je váš model společnosti Adventure Works Internet prodej nasazenou, můžete použít SQL Server Management Studio toomanage hello modelu; Vytvořte proces skripty a plán zálohování.</span><span class="sxs-lookup"><span data-stu-id="3818f-135">Now that your Adventure Works Internet Sales model is deployed, you can use SQL Server Management Studio toomanage hello model; create process scripts and a backup plan.</span></span> <span data-ttu-id="3818f-136">Uživatelé mohou nyní také připojit toohello model pomocí sestav klientská aplikace, jako je například aplikace Microsoft Excel nebo Power BI.</span><span class="sxs-lookup"><span data-stu-id="3818f-136">Users can also now connect toohello model using a reporting client application such as Microsoft Excel or Power BI.</span></span>  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a><span data-ttu-id="3818f-138">Co dále?</span><span class="sxs-lookup"><span data-stu-id="3818f-138">What's next?</span></span>
<span data-ttu-id="3818f-139">[Propojení s Power BI Desktop](../analysis-services-connect-pbi.md) </span><span class="sxs-lookup"><span data-stu-id="3818f-139">[Connect with Power BI Desktop](../analysis-services-connect-pbi.md) </span></span>  
<span data-ttu-id="3818f-140">[Doplňková lekce – Dynamické zabezpečení](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span><span class="sxs-lookup"><span data-stu-id="3818f-140">[Supplemental Lesson - Dynamic security](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span></span>  
<span data-ttu-id="3818f-141">[Doplňková lekce – Řádky podrobností](../tutorials/aas-supplemental-lesson-detail-rows.md) </span><span class="sxs-lookup"><span data-stu-id="3818f-141">[Supplemental Lesson - Detail rows](../tutorials/aas-supplemental-lesson-detail-rows.md) </span></span>  
[<span data-ttu-id="3818f-142">Doplňková lekce – Nepravidelné hierarchie</span><span class="sxs-lookup"><span data-stu-id="3818f-142">Supplemental Lesson - Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
