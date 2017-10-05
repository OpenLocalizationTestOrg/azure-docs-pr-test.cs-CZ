---
title: "Vytvořit pracovní prostor Machine Learning | Microsoft Docs"
description: "Postup vytvoření pracovního prostoru pro Azure Machine Learning Studio"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye;bradsev;ahgyger
ms.openlocfilehash: 182a34822e71d63f4d7229548ae3f59d9f195337
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a><span data-ttu-id="327c0-103">Vytvoření a sdílení pracovního prostoru Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="327c0-103">Create and share an Azure Machine Learning workspace</span></span>
<span data-ttu-id="327c0-104">Tato nabídka odkazy na témata, které popisují, jak nastavit různé vědě prostředí data používá Cortana proces pro analýzu (CAP).</span><span class="sxs-lookup"><span data-stu-id="327c0-104">This menu links to topics that describe how to set up the various data science environments used by the Cortana Analytics Process (CAPS).</span></span>

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

<span data-ttu-id="327c0-105">Pokud chcete používat Azure Machine Learning Studio, musíte mít pracovní prostor Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="327c0-105">To use Azure Machine Learning Studio, you need to have a Machine Learning workspace.</span></span> <span data-ttu-id="327c0-106">Tento pracovní prostor obsahuje nástroje, které potřebujete k vytváření, správu a publikování experimenty.</span><span class="sxs-lookup"><span data-stu-id="327c0-106">This workspace contains the tools you need to create, manage, and publish experiments.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="to-create-a-workspace"></a><span data-ttu-id="327c0-107">Vytvoření pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="327c0-107">To create a workspace</span></span>
1. <span data-ttu-id="327c0-108">Přihlaste se k [portálu Azure](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="327c0-108">Sign in to the [Azure portal](https://portal.azure.com/)</span></span>

    > [!NOTE]
    > <span data-ttu-id="327c0-109">Přihlaste se a vytvořit pracovní prostor, musíte být správce předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="327c0-109">To sign in and create a workspace, you need to be an Azure subscription administrator.</span></span> 
    >
    > 

2. <span data-ttu-id="327c0-110">Klikněte na tlačítko **+ nové**</span><span class="sxs-lookup"><span data-stu-id="327c0-110">Click **+New**</span></span>

3. <span data-ttu-id="327c0-111">Vyberte **Intelligence + analýzy**, klikněte na tlačítko **pracovní prostor Machine Learning**, pak klikněte na tlačítko **vytvořit**</span><span class="sxs-lookup"><span data-stu-id="327c0-111">Select **Intelligence + analytics**, click **Machine Learning Workspace**, then click **Create**</span></span>

4. <span data-ttu-id="327c0-112">Zadejte informace o pracovním prostoru</span><span class="sxs-lookup"><span data-stu-id="327c0-112">Enter your workspace information</span></span>

    - <span data-ttu-id="327c0-113">*Název pracovního prostoru* může být až 260 znaků, ne končí v prostoru.</span><span class="sxs-lookup"><span data-stu-id="327c0-113">The *workspace name* may be up to 260 characters, not ending in a space.</span></span> <span data-ttu-id="327c0-114">Název nesmí obsahovat tyto znaky:`< > * % & : \ ? + /`</span><span class="sxs-lookup"><span data-stu-id="327c0-114">The name can't include these characters: `< > * % & : \ ? + /`</span></span>
    - <span data-ttu-id="327c0-115">*Plán webové služby* vyberete (nebo vytvořte), spolu s příslušnými *cenová úroveň* můžete vybrat, se používá při nasazování webových služeb z tohoto pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="327c0-115">The *web service plan* you choose (or create), along with the associated *pricing tier* you select, is used if you deploy web services from this workspace.</span></span>

    ![Vytvořit nový pracovní prostor](media/machine-learning-create-workspace/create-new-workspace.png)

5. <span data-ttu-id="327c0-117">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="327c0-117">Click **Create**</span></span>

<span data-ttu-id="327c0-118">Po nasazení pracovním prostoru můžete ho otevřít v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="327c0-118">Once the workspace is deployed, you can open it in Machine Learning Studio.</span></span>

1. <span data-ttu-id="327c0-119">Procházet a strojového učení Studio na [https://studio.azureml.net/](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="327c0-119">Browse to Machine Learning Studio at [https://studio.azureml.net/](https://studio.azureml.net/).</span></span>

2. <span data-ttu-id="327c0-120">V pravém horním rohu vyberte pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="327c0-120">Select your workspace in the upper-right-hand corner.</span></span>

    ![Vyberte pracovní prostor](media/machine-learning-create-workspace/open-workspace.png)

3. <span data-ttu-id="327c0-122">Klikněte na tlačítko **Moje experimenty**.</span><span class="sxs-lookup"><span data-stu-id="327c0-122">Click **my experiments**.</span></span>

    ![Otevřete experimenty](media/machine-learning-create-workspace/my-experiments.png)

<span data-ttu-id="327c0-124">Informace o správě pracovního prostoru najdete v tématu [spravovat pracovní prostor služby Azure Machine Learning](machine-learning-manage-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="327c0-124">For information about managing your workspace, see [Manage an Azure Machine Learning workspace](machine-learning-manage-workspace.md).</span></span>
<span data-ttu-id="327c0-125">Pokud narazíte na potíže při vytváření pracovního prostoru, přečtěte si téma [Průvodce odstraňováním potíží: Vytvořte a připojte se k pracovní prostor Machine Learning](machine-learning-troubleshooting-creating-ml-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="327c0-125">If you encounter a problem creating your workspace, see [Troubleshooting guide: Create and connect to a Machine Learning workspace](machine-learning-troubleshooting-creating-ml-workspace.md).</span></span>


## <a name="sharing-an-azure-machine-learning-workspace"></a><span data-ttu-id="327c0-126">Sdílení pracovní prostor služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="327c0-126">Sharing an Azure Machine Learning workspace</span></span>
<span data-ttu-id="327c0-127">Jednou Machine Learning při vytváření pracovního prostoru, můžete uživatele pozvat, do pracovního prostoru sdílet přístup do pracovního prostoru a všechny jeho experimenty, datové sady, poznámkové bloky, atd. Můžete přidat uživatele v jednom ze dvou rolí:</span><span class="sxs-lookup"><span data-stu-id="327c0-127">Once a Machine Learning workspace is created, you can invite users to your workspace to share access to your workspace and all its experiments, datasets, notebooks, etc. You can add users in one of two roles:</span></span>

* <span data-ttu-id="327c0-128">**Uživatel** -prostoru uživatel může vytvořit, otevřít, upravit a odstranit experimenty, datové sady atd., v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="327c0-128">**User** - A workspace user can create, open, modify, and delete experiments, datasets, etc. in the workspace.</span></span>
* <span data-ttu-id="327c0-129">**Vlastník** – vlastníka můžete pozvat a odebírání uživatelů v pracovním prostoru, kromě uživatele, můžete provést.</span><span class="sxs-lookup"><span data-stu-id="327c0-129">**Owner** - An owner can invite and remove users in the workspace, in addition to what a user can do.</span></span>

> [!NOTE]
> <span data-ttu-id="327c0-130">Účet správce, který vytvoří pracovním prostoru se automaticky přidá do pracovního prostoru jako pracovní prostor vlastníka.</span><span class="sxs-lookup"><span data-stu-id="327c0-130">The administrator account that creates the workspace is automatically added to the workspace as workspace Owner.</span></span> <span data-ttu-id="327c0-131">Ale jiní správci nebo uživatelé v tomto předplatném nejsou automaticky udělí přístup k pracovním prostoru – je potřeba je explicitně pozvat.</span><span class="sxs-lookup"><span data-stu-id="327c0-131">However, other administrators or users in that subscription are not automatically granted access to the workspace - you need to invite them explicitly.</span></span>
> 
> 

### <a name="to-share-a-workspace"></a><span data-ttu-id="327c0-132">Chcete-li sdílet pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="327c0-132">To share a workspace</span></span>

1. <span data-ttu-id="327c0-133">Přihlaste se k strojového učení Studio na [https://studio.azureml.net/Home](https://studio.azureml.net/Home)</span><span class="sxs-lookup"><span data-stu-id="327c0-133">Sign in to Machine Learning Studio at [https://studio.azureml.net/Home](https://studio.azureml.net/Home)</span></span>

2. <span data-ttu-id="327c0-134">V levém panelu klikněte na tlačítko **nastavení**</span><span class="sxs-lookup"><span data-stu-id="327c0-134">In the left panel, click **SETTINGS**</span></span>

3. <span data-ttu-id="327c0-135">Klikněte **uživatelé** karta</span><span class="sxs-lookup"><span data-stu-id="327c0-135">Click the **USERS** tab</span></span>

4. <span data-ttu-id="327c0-136">Klikněte na tlačítko **POZVAT uživatele více** v dolní části stránky</span><span class="sxs-lookup"><span data-stu-id="327c0-136">Click **INVITE MORE USERS** at the bottom of the page</span></span>

    ![Nastavení Studio](media/machine-learning-create-workspace/settings.png)

5. <span data-ttu-id="327c0-138">Zadejte jeden nebo více e-mailové adresy.</span><span class="sxs-lookup"><span data-stu-id="327c0-138">Enter one or more email addresses.</span></span> <span data-ttu-id="327c0-139">Uživatelé, potřebujete účet Microsoft nebo účet organizace (z Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="327c0-139">The users need a valid Microsoft account or an organizational account (from Azure Active Directory).</span></span>

6. <span data-ttu-id="327c0-140">Vyberte, zda chcete přidat uživatele jako vlastníka nebo uživatele.</span><span class="sxs-lookup"><span data-stu-id="327c0-140">Select whether you want to add the users as Owner or User.</span></span>

7. <span data-ttu-id="327c0-141">Klikněte **OK** značku zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="327c0-141">Click the **OK** checkmark button.</span></span>

<span data-ttu-id="327c0-142">Každý uživatel, které přidáte obdrží e-mail s pokyny, jak se přihlásit do sdíleného pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="327c0-142">Each user you add will receive an email with instructions on how to sign in to the shared workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="327c0-143">Pro uživatele, abyste mohli nasadit nebo spravovat webových služeb v tomto pracovním prostoru musí být Přispěvatel nebo správce v rámci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="327c0-143">For users to be able to deploy or manage web services in this workspace, they must be a contributor or administrator in the Azure subscription.</span></span> 



