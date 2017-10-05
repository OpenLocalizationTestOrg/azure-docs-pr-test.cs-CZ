---
title: "Vytváření koncových bodů webové služby v Machine Learning | Microsoft Docs"
description: "Vytváření koncových bodů webové služby v Azure Machine Learning"
services: machine-learning
documentationcenter: 
author: hiteshmadan
manager: padou
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.author: himad
ms.openlocfilehash: 9f83ffc9cf7dbe37c1ce9980fd7f5b9133fe78f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="creating-endpoints"></a><span data-ttu-id="96a0e-103">Vytváření koncových bodů</span><span class="sxs-lookup"><span data-stu-id="96a0e-103">Creating Endpoints</span></span>
> [!NOTE]
>  <span data-ttu-id="96a0e-104">Toto téma popisuje techniky pro **Classic** Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="96a0e-104">This topic describes techniques applicable to a **Classic** Machine Learning Web service.</span></span>
> 
> 

<span data-ttu-id="96a0e-105">Při vytváření webové služby, které prodeje dál zákazníkům, je třeba zadat trénované modely u každého zákazníka, které jsou stále spojeny s experiment, ze kterého byl vytvořen webovou službu.</span><span class="sxs-lookup"><span data-stu-id="96a0e-105">When you create Web services that you sell forward to your customers, you need to provide trained models to each customer that are still linked to the experiment from which the Web service was created.</span></span> <span data-ttu-id="96a0e-106">Kromě toho všechny aktualizace experimentu bude použito selektivní pro koncový bod bez přepsal přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="96a0e-106">In addition, any updates to the experiment should be applied selectively to an endpoint without overwriting the customizations.</span></span>

<span data-ttu-id="96a0e-107">K tomu, Azure Machine Learning můžete vytvořit několik koncových bodů pro nasazenou webovou službu.</span><span class="sxs-lookup"><span data-stu-id="96a0e-107">To accomplish this, Azure Machine Learning allows you to create multiple endpoints for a deployed Web service.</span></span> <span data-ttu-id="96a0e-108">Každý koncový bod webové služby je nezávisle řešit, omezení a spravovat.</span><span class="sxs-lookup"><span data-stu-id="96a0e-108">Each endpoint in the Web service is independently addressed, throttled, and managed.</span></span> <span data-ttu-id="96a0e-109">Každý koncový bod je jedinečnou adresu URL a autorizační klíč, které můžete distribuovat zákazníkům.</span><span class="sxs-lookup"><span data-stu-id="96a0e-109">Each endpoint is a unique URL and authorization key that you can distribute to your customers.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a><span data-ttu-id="96a0e-110">Přidání koncových bodů webové služby</span><span class="sxs-lookup"><span data-stu-id="96a0e-110">Adding endpoints to a Web service</span></span>
<span data-ttu-id="96a0e-111">Existují tři způsoby, jak přidat koncový bod webové služby.</span><span class="sxs-lookup"><span data-stu-id="96a0e-111">There are three ways to add an endpoint to a Web service.</span></span>

* <span data-ttu-id="96a0e-112">Prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="96a0e-112">Programmatically</span></span>
* <span data-ttu-id="96a0e-113">Prostřednictvím portálu Azure Machine Learning webové služby</span><span class="sxs-lookup"><span data-stu-id="96a0e-113">Through the Azure Machine Learning Web Services portal</span></span>
* <span data-ttu-id="96a0e-114">Když na portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="96a0e-114">Though the Azure classic portal</span></span>

<span data-ttu-id="96a0e-115">Po vytvoření koncového bodu, můžete využívat prostřednictvím synchronní rozhraní API, batch rozhraní API a listy aplikace excel.</span><span class="sxs-lookup"><span data-stu-id="96a0e-115">Once the endpoint is created, you can consume it through synchronous APIs, batch APIs, and excel worksheets.</span></span> <span data-ttu-id="96a0e-116">Kromě přidání koncových bodů prostřednictvím tohoto uživatelského rozhraní, můžete také použít rozhraní API pro správu koncový bod programově přidat koncové body.</span><span class="sxs-lookup"><span data-stu-id="96a0e-116">In addition to adding endpoints through this UI, you can also use the Endpoint Management APIs to programmatically add endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="96a0e-117">Pokud jste přidali další koncové body k webové službě, nelze odstranit výchozí koncový bod.</span><span class="sxs-lookup"><span data-stu-id="96a0e-117">If you have added additional endpoints to the Web service, you cannot delete the default endpoint.</span></span>
> 
> 

## <a name="adding-an-endpoint-programmatically"></a><span data-ttu-id="96a0e-118">Přidání koncového bodu prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="96a0e-118">Adding an endpoint programmatically</span></span>
<span data-ttu-id="96a0e-119">Koncový bod můžete přidat k webové službě programově pomocí [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="96a0e-119">You can add an endpoint to your Web service programmatically using the [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code.</span></span>

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a><span data-ttu-id="96a0e-120">Přidání koncového bodu pomocí portálu Azure Machine Learning webové služby</span><span class="sxs-lookup"><span data-stu-id="96a0e-120">Adding an endpoint using the Azure Machine Learning Web Services portal</span></span>
1. <span data-ttu-id="96a0e-121">Machine Learning Studio v levém navigačním sloupec, klikněte na možnost webové služby.</span><span class="sxs-lookup"><span data-stu-id="96a0e-121">In Machine Learning Studio, on the left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="96a0e-122">V dolní části řídicího panelu webové služby, klikněte na tlačítko **umožňuje spravovat koncové body**.</span><span class="sxs-lookup"><span data-stu-id="96a0e-122">At the bottom of the Web service dashboard, click **Manage endpoints**.</span></span> <span data-ttu-id="96a0e-123">Webové služby Azure Machine Learning portál otevře stránku koncové body pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="96a0e-123">The Azure Machine Learning Web Services portal opens to the endpoints page for the Web service.</span></span>
3. <span data-ttu-id="96a0e-124">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="96a0e-124">Click **New**.</span></span>
4. <span data-ttu-id="96a0e-125">Zadejte název a popis pro nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="96a0e-125">Type a name and description for the new endpoint.</span></span> <span data-ttu-id="96a0e-126">Názvy koncových bodů musí být 24 znaky nebo méně délku a musí být tvořen malá písmena nebo číslice.</span><span class="sxs-lookup"><span data-stu-id="96a0e-126">Endpoint names must be 24 character or less in length, and must be made up of lower-case alphabets or numbers.</span></span> <span data-ttu-id="96a0e-127">Vyberte úroveň protokolování a určuje, jestli je povolená ukázková data.</span><span class="sxs-lookup"><span data-stu-id="96a0e-127">Select the logging level and whether sample data is enabled.</span></span> <span data-ttu-id="96a0e-128">Další informace o protokolování naleznete v tématu [povolení protokolování pro Machine Learning webové služby](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="96a0e-128">For more information on logging, see [Enable logging for Machine Learning Web services](machine-learning-web-services-logging.md).</span></span>

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a><span data-ttu-id="96a0e-129">Přidání koncového bodu pomocí portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="96a0e-129">Adding an endpoint using the Azure classic portal</span></span>
1. <span data-ttu-id="96a0e-130">Přihlaste se k [portál Azure classic](http://manage.windowsazure.com), klikněte na tlačítko **Machine Learning** v levém sloupci.</span><span class="sxs-lookup"><span data-stu-id="96a0e-130">Sign in to the [Azure classic portal](http://manage.windowsazure.com), click **Machine Learning** in the left column.</span></span> <span data-ttu-id="96a0e-131">Klikněte na pracovní prostor, který obsahuje webovou službu, která vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="96a0e-131">Click the workspace which contains the Web service in which you are interested.</span></span>
   
    ![Přejděte do pracovního prostoru](./media/machine-learning-create-endpoint/figure-1.png)
2. <span data-ttu-id="96a0e-133">Klikněte na tlačítko **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="96a0e-133">Click **Web Services**.</span></span>
   
    ![Přejděte na webové služby](./media/machine-learning-create-endpoint/figure-2.png)
3. <span data-ttu-id="96a0e-135">Klikněte na webovou službu, které vás zajímají zobrazíte seznam dostupných koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="96a0e-135">Click the Web service you're interested in to see the list of available endpoints.</span></span>
   
    ![Přejděte ke koncovému bodu](./media/machine-learning-create-endpoint/figure-3.png)
4. <span data-ttu-id="96a0e-137">V dolní části stránky klikněte na tlačítko **přidat koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="96a0e-137">At the bottom of the page, click **Add Endpoint**.</span></span> <span data-ttu-id="96a0e-138">Zadejte název a popis, ujistěte se, že neexistují žádné koncové body se stejným názvem v této webové služby.</span><span class="sxs-lookup"><span data-stu-id="96a0e-138">Type a name and description, ensure there are no other endpoints with the same name in this Web service.</span></span> <span data-ttu-id="96a0e-139">Pokud máte speciální požadavky, ponechte úroveň omezení s jeho výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="96a0e-139">Leave the throttle level with its default value unless you have special requirements.</span></span> <span data-ttu-id="96a0e-140">Další informace o omezování najdete v tématu [škálování koncových bodů API](machine-learning-scaling-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="96a0e-140">To learn more about throttling, see [Scaling API Endpoints](machine-learning-scaling-webservice.md).</span></span>
   
    ![Vytvoření koncového bodu](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a><span data-ttu-id="96a0e-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96a0e-142">Next Steps</span></span>
<span data-ttu-id="96a0e-143">[Jak používat Azure Machine Learning webové služby](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="96a0e-143">[How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

