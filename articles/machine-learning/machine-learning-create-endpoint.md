---
title: "koncových bodů webové služby aaaCreating v Machine Learning | Microsoft Docs"
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
ms.openlocfilehash: 10a2bc586c6fe35e28d8bf0293854c578827c453
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-endpoints"></a><span data-ttu-id="52ceb-103">Vytváření koncových bodů</span><span class="sxs-lookup"><span data-stu-id="52ceb-103">Creating Endpoints</span></span>
> [!NOTE]
>  <span data-ttu-id="52ceb-104">Toto téma popisuje techniky použít tooa **Classic** Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="52ceb-104">This topic describes techniques applicable tooa **Classic** Machine Learning Web service.</span></span>
> 
> 

<span data-ttu-id="52ceb-105">Při vytváření webové služby, které prodeje dopředného tooyour zákazníků, je nutné tooprovide trénované modely tooeach zákazníka, které jsou stále propojené toohello experiment, ze které hello webové služby byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="52ceb-105">When you create Web services that you sell forward tooyour customers, you need tooprovide trained models tooeach customer that are still linked toohello experiment from which hello Web service was created.</span></span> <span data-ttu-id="52ceb-106">Kromě toho aktualizace toohello experiment by měla být použita selektivně tooan koncový bod bez přepsal hello přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="52ceb-106">In addition, any updates toohello experiment should be applied selectively tooan endpoint without overwriting hello customizations.</span></span>

<span data-ttu-id="52ceb-107">tooaccomplish, Azure Machine Learning vám umožní toocreate několik koncových bodů pro nasazenou webovou službu.</span><span class="sxs-lookup"><span data-stu-id="52ceb-107">tooaccomplish this, Azure Machine Learning allows you toocreate multiple endpoints for a deployed Web service.</span></span> <span data-ttu-id="52ceb-108">Každý koncový bod v hello webové služby je nezávisle řešit, omezení a spravovat.</span><span class="sxs-lookup"><span data-stu-id="52ceb-108">Each endpoint in hello Web service is independently addressed, throttled, and managed.</span></span> <span data-ttu-id="52ceb-109">Každý koncový bod je jedinečný klíč adresy URL a autorizace, které můžete distribuovat tooyour zákazníků.</span><span class="sxs-lookup"><span data-stu-id="52ceb-109">Each endpoint is a unique URL and authorization key that you can distribute tooyour customers.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-tooa-web-service"></a><span data-ttu-id="52ceb-110">Přidání koncové body tooa webové služby</span><span class="sxs-lookup"><span data-stu-id="52ceb-110">Adding endpoints tooa Web service</span></span>
<span data-ttu-id="52ceb-111">Existují tři způsoby tooadd tooa koncový bod webové služby.</span><span class="sxs-lookup"><span data-stu-id="52ceb-111">There are three ways tooadd an endpoint tooa Web service.</span></span>

* <span data-ttu-id="52ceb-112">Prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="52ceb-112">Programmatically</span></span>
* <span data-ttu-id="52ceb-113">Prostřednictvím portálu webové služby Azure Machine Learning hello</span><span class="sxs-lookup"><span data-stu-id="52ceb-113">Through hello Azure Machine Learning Web Services portal</span></span>
* <span data-ttu-id="52ceb-114">Přestože hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="52ceb-114">Though hello Azure classic portal</span></span>

<span data-ttu-id="52ceb-115">Jakmile je vytvořen koncový bod hello, můžete využívat prostřednictvím synchronní rozhraní API, batch rozhraní API a listy aplikace excel.</span><span class="sxs-lookup"><span data-stu-id="52ceb-115">Once hello endpoint is created, you can consume it through synchronous APIs, batch APIs, and excel worksheets.</span></span> <span data-ttu-id="52ceb-116">Kromě toho tooadding koncových bodů prostřednictvím tohoto uživatelského rozhraní, můžete použít také hello koncový bod rozhraní API pro správu tooprogrammatically přidat koncové body.</span><span class="sxs-lookup"><span data-stu-id="52ceb-116">In addition tooadding endpoints through this UI, you can also use hello Endpoint Management APIs tooprogrammatically add endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="52ceb-117">Pokud jste přidali další koncové body toohello webové služby, nelze odstranit hello výchozí koncový bod.</span><span class="sxs-lookup"><span data-stu-id="52ceb-117">If you have added additional endpoints toohello Web service, you cannot delete hello default endpoint.</span></span>
> 
> 

## <a name="adding-an-endpoint-programmatically"></a><span data-ttu-id="52ceb-118">Přidání koncového bodu prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="52ceb-118">Adding an endpoint programmatically</span></span>
<span data-ttu-id="52ceb-119">Můžete přidat koncový bod tooyour webové služby programově pomocí hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="52ceb-119">You can add an endpoint tooyour Web service programmatically using hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code.</span></span>

## <a name="adding-an-endpoint-using-hello-azure-machine-learning-web-services-portal"></a><span data-ttu-id="52ceb-120">Přidání koncový bod webové služby Azure Machine Learning portálu hello</span><span class="sxs-lookup"><span data-stu-id="52ceb-120">Adding an endpoint using hello Azure Machine Learning Web Services portal</span></span>
1. <span data-ttu-id="52ceb-121">Machine Learning Studio v levém navigačním sloupec hello, klikněte na možnost webové služby.</span><span class="sxs-lookup"><span data-stu-id="52ceb-121">In Machine Learning Studio, on hello left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="52ceb-122">V dolní části hello řídicího panelu, hello webové služby, klikněte na tlačítko **umožňuje spravovat koncové body**.</span><span class="sxs-lookup"><span data-stu-id="52ceb-122">At hello bottom of hello Web service dashboard, click **Manage endpoints**.</span></span> <span data-ttu-id="52ceb-123">Hello webové služby Azure Machine Learning portál otevře toohello koncové body stránku hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="52ceb-123">hello Azure Machine Learning Web Services portal opens toohello endpoints page for hello Web service.</span></span>
3. <span data-ttu-id="52ceb-124">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="52ceb-124">Click **New**.</span></span>
4. <span data-ttu-id="52ceb-125">Zadejte název a popis pro nový koncový bod hello.</span><span class="sxs-lookup"><span data-stu-id="52ceb-125">Type a name and description for hello new endpoint.</span></span> <span data-ttu-id="52ceb-126">Názvy koncových bodů musí být 24 znaky nebo méně délku a musí být tvořen malá písmena nebo číslice.</span><span class="sxs-lookup"><span data-stu-id="52ceb-126">Endpoint names must be 24 character or less in length, and must be made up of lower-case alphabets or numbers.</span></span> <span data-ttu-id="52ceb-127">Vyberte úroveň protokolování hello a určuje, jestli je povolená ukázková data.</span><span class="sxs-lookup"><span data-stu-id="52ceb-127">Select hello logging level and whether sample data is enabled.</span></span> <span data-ttu-id="52ceb-128">Další informace o protokolování naleznete v tématu [povolení protokolování pro Machine Learning webové služby](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="52ceb-128">For more information on logging, see [Enable logging for Machine Learning Web services](machine-learning-web-services-logging.md).</span></span>

## <a name="adding-an-endpoint-using-hello-azure-classic-portal"></a><span data-ttu-id="52ceb-129">Přidání koncového bodu pomocí hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="52ceb-129">Adding an endpoint using hello Azure classic portal</span></span>
1. <span data-ttu-id="52ceb-130">Přihlaste se toohello [portál Azure classic](http://manage.windowsazure.com), klikněte na tlačítko **Machine Learning** v levém sloupci hello.</span><span class="sxs-lookup"><span data-stu-id="52ceb-130">Sign in toohello [Azure classic portal](http://manage.windowsazure.com), click **Machine Learning** in hello left column.</span></span> <span data-ttu-id="52ceb-131">Klikněte na tlačítko hello pracovní prostor, který obsahuje hello webová služba, která vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="52ceb-131">Click hello workspace which contains hello Web service in which you are interested.</span></span>
   
    ![Přejděte tooworkspace](./media/machine-learning-create-endpoint/figure-1.png)
2. <span data-ttu-id="52ceb-133">Klikněte na tlačítko **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="52ceb-133">Click **Web Services**.</span></span>
   
    ![Přejděte tooWeb služby](./media/machine-learning-create-endpoint/figure-2.png)
3. <span data-ttu-id="52ceb-135">Klikněte na tlačítko hello webové služby, co vás zajímá toosee hello seznam dostupných koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="52ceb-135">Click hello Web service you're interested in toosee hello list of available endpoints.</span></span>
   
    ![Přejděte tooendpoint](./media/machine-learning-create-endpoint/figure-3.png)
4. <span data-ttu-id="52ceb-137">V dolní části hello hello stránky, klikněte na tlačítko **přidat koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="52ceb-137">At hello bottom of hello page, click **Add Endpoint**.</span></span> <span data-ttu-id="52ceb-138">Zadejte název a popis, ujistěte se, že neexistují žádné koncové body s hello stejný název v této webové služby.</span><span class="sxs-lookup"><span data-stu-id="52ceb-138">Type a name and description, ensure there are no other endpoints with hello same name in this Web service.</span></span> <span data-ttu-id="52ceb-139">Pokud máte speciální požadavky, ponechte hello omezení úroveň s jeho výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="52ceb-139">Leave hello throttle level with its default value unless you have special requirements.</span></span> <span data-ttu-id="52ceb-140">toolearn Další informace o omezování, najdete v části [škálování koncových bodů API](machine-learning-scaling-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="52ceb-140">toolearn more about throttling, see [Scaling API Endpoints](machine-learning-scaling-webservice.md).</span></span>
   
    ![Vytvoření koncového bodu](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a><span data-ttu-id="52ceb-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52ceb-142">Next Steps</span></span>
<span data-ttu-id="52ceb-143">[Jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="52ceb-143">[How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

