---
title: "aaaCreate pracovní prostor Machine Learning | Microsoft Docs"
description: "Jak toocreate pracovního prostoru pro Azure Machine Learning Studio"
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
ms.openlocfilehash: 178293af222365993fade666124f34269d892325
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a><span data-ttu-id="f0bd0-103">Vytvoření a sdílení pracovního prostoru Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="f0bd0-103">Create and share an Azure Machine Learning workspace</span></span>
<span data-ttu-id="f0bd0-104">Tato nabídka propojí tootopics, které popisují, jak tooset až hello různé vědě prostředí datových používají v hello Cortana Analytics procesu (CAP).</span><span class="sxs-lookup"><span data-stu-id="f0bd0-104">This menu links tootopics that describe how tooset up hello various data science environments used by hello Cortana Analytics Process (CAPS).</span></span>

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

<span data-ttu-id="f0bd0-105">toouse Azure Machine Learning Studio, musíte toohave pracovní prostor Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-105">toouse Azure Machine Learning Studio, you need toohave a Machine Learning workspace.</span></span> <span data-ttu-id="f0bd0-106">Tento pracovní prostor obsahuje hello nástroje, které potřebujete toocreate, spravovat a publikovat experimenty.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-106">This workspace contains hello tools you need toocreate, manage, and publish experiments.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="toocreate-a-workspace"></a><span data-ttu-id="f0bd0-107">toocreate pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="f0bd0-107">toocreate a workspace</span></span>
1. <span data-ttu-id="f0bd0-108">Přihlaste se toohello [portálu Azure](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="f0bd0-108">Sign in toohello [Azure portal](https://portal.azure.com/)</span></span>

    > [!NOTE]
    > <span data-ttu-id="f0bd0-109">toosign v a vytvořit pracovní prostor, musíte toobe správce předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-109">toosign in and create a workspace, you need toobe an Azure subscription administrator.</span></span> 
    >
    > 

2. <span data-ttu-id="f0bd0-110">Klikněte na tlačítko **+ nové**</span><span class="sxs-lookup"><span data-stu-id="f0bd0-110">Click **+New**</span></span>

3. <span data-ttu-id="f0bd0-111">Vyberte **Intelligence + analýzy**, klikněte na tlačítko **pracovní prostor Machine Learning**, pak klikněte na tlačítko **vytvořit**</span><span class="sxs-lookup"><span data-stu-id="f0bd0-111">Select **Intelligence + analytics**, click **Machine Learning Workspace**, then click **Create**</span></span>

4. <span data-ttu-id="f0bd0-112">Zadejte informace o pracovním prostoru</span><span class="sxs-lookup"><span data-stu-id="f0bd0-112">Enter your workspace information</span></span>

    - <span data-ttu-id="f0bd0-113">Hello *název pracovního prostoru* může být až too260 znaky, není končí v prostoru.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-113">hello *workspace name* may be up too260 characters, not ending in a space.</span></span> <span data-ttu-id="f0bd0-114">Hello název nesmí obsahovat tyto znaky:`< > * % & : \ ? + /`</span><span class="sxs-lookup"><span data-stu-id="f0bd0-114">hello name can't include these characters: `< > * % & : \ ? + /`</span></span>
    - <span data-ttu-id="f0bd0-115">Hello *plán webové služby* vyberete (nebo vytvořte), spolu s hello přidružené *cenová úroveň* můžete vybrat, se používá při nasazování webových služeb z tohoto pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-115">hello *web service plan* you choose (or create), along with hello associated *pricing tier* you select, is used if you deploy web services from this workspace.</span></span>

    ![Vytvořit nový pracovní prostor](media/machine-learning-create-workspace/create-new-workspace.png)

5. <span data-ttu-id="f0bd0-117">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="f0bd0-117">Click **Create**</span></span>

<span data-ttu-id="f0bd0-118">Po nasazení hello prostoru můžete ho otevřít v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-118">Once hello workspace is deployed, you can open it in Machine Learning Studio.</span></span>

1. <span data-ttu-id="f0bd0-119">Procházet tooMachine Learning Studio na [https://studio.azureml.net/](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="f0bd0-119">Browse tooMachine Learning Studio at [https://studio.azureml.net/](https://studio.azureml.net/).</span></span>

2. <span data-ttu-id="f0bd0-120">Vyberte pracovní prostor v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-120">Select your workspace in hello upper-right-hand corner.</span></span>

    ![Vyberte pracovní prostor](media/machine-learning-create-workspace/open-workspace.png)

3. <span data-ttu-id="f0bd0-122">Klikněte na tlačítko **Moje experimenty**.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-122">Click **my experiments**.</span></span>

    ![Otevřete experimenty](media/machine-learning-create-workspace/my-experiments.png)

<span data-ttu-id="f0bd0-124">Informace o správě pracovního prostoru najdete v tématu [spravovat pracovní prostor služby Azure Machine Learning](machine-learning-manage-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="f0bd0-124">For information about managing your workspace, see [Manage an Azure Machine Learning workspace](machine-learning-manage-workspace.md).</span></span>
<span data-ttu-id="f0bd0-125">Pokud narazíte na potíže při vytváření pracovního prostoru, přečtěte si téma [Průvodce odstraňováním potíží: Vytvořte a připojte pracovní prostor Machine Learning tooa](machine-learning-troubleshooting-creating-ml-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="f0bd0-125">If you encounter a problem creating your workspace, see [Troubleshooting guide: Create and connect tooa Machine Learning workspace](machine-learning-troubleshooting-creating-ml-workspace.md).</span></span>


## <a name="sharing-an-azure-machine-learning-workspace"></a><span data-ttu-id="f0bd0-126">Sdílení pracovní prostor služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="f0bd0-126">Sharing an Azure Machine Learning workspace</span></span>
<span data-ttu-id="f0bd0-127">Po vytvoření pracovního prostoru Machine Learning můžete pozvat uživatele tooyour prostoru tooshare přístup tooyour prostoru a všechny jeho experimenty, datové sady, poznámkových bloků, atd. Můžete přidat uživatele v jednom ze dvou rolí:</span><span class="sxs-lookup"><span data-stu-id="f0bd0-127">Once a Machine Learning workspace is created, you can invite users tooyour workspace tooshare access tooyour workspace and all its experiments, datasets, notebooks, etc. You can add users in one of two roles:</span></span>

* <span data-ttu-id="f0bd0-128">**Uživatel** -prostoru uživatel může vytvořit, otevřít, upravit a odstranit experimenty, datové sady atd., v pracovním prostoru hello.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-128">**User** - A workspace user can create, open, modify, and delete experiments, datasets, etc. in hello workspace.</span></span>
* <span data-ttu-id="f0bd0-129">**Vlastník** – vlastníka můžete pozvat a odebírání uživatelů v prostoru hello v toowhat přidání uživatele můžete provést.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-129">**Owner** - An owner can invite and remove users in hello workspace, in addition toowhat a user can do.</span></span>

> [!NOTE]
> <span data-ttu-id="f0bd0-130">Hello účet správce, který vytvoří hello prostoru se automaticky přidá toohello prostoru jako pracovní prostor vlastníka.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-130">hello administrator account that creates hello workspace is automatically added toohello workspace as workspace Owner.</span></span> <span data-ttu-id="f0bd0-131">Ale jiní správci nebo uživatelé v tomto předplatném nejsou automaticky udělen přístup toohello prostoru – potřebovat tooinvite je explicitně.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-131">However, other administrators or users in that subscription are not automatically granted access toohello workspace - you need tooinvite them explicitly.</span></span>
> 
> 

### <a name="tooshare-a-workspace"></a><span data-ttu-id="f0bd0-132">tooshare pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="f0bd0-132">tooshare a workspace</span></span>

1. <span data-ttu-id="f0bd0-133">Přihlaste se tooMachine Learning Studio zadává [https://studio.azureml.net/Home](https://studio.azureml.net/Home)</span><span class="sxs-lookup"><span data-stu-id="f0bd0-133">Sign in tooMachine Learning Studio at [https://studio.azureml.net/Home](https://studio.azureml.net/Home)</span></span>

2. <span data-ttu-id="f0bd0-134">V levém panelu hello, klikněte na tlačítko **nastavení**</span><span class="sxs-lookup"><span data-stu-id="f0bd0-134">In hello left panel, click **SETTINGS**</span></span>

3. <span data-ttu-id="f0bd0-135">Klikněte na tlačítko hello **uživatelé** karta</span><span class="sxs-lookup"><span data-stu-id="f0bd0-135">Click hello **USERS** tab</span></span>

4. <span data-ttu-id="f0bd0-136">Klikněte na tlačítko **POZVAT uživatele více** na hello dolní části stránky hello</span><span class="sxs-lookup"><span data-stu-id="f0bd0-136">Click **INVITE MORE USERS** at hello bottom of hello page</span></span>

    ![Nastavení Studio](media/machine-learning-create-workspace/settings.png)

5. <span data-ttu-id="f0bd0-138">Zadejte jeden nebo více e-mailové adresy.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-138">Enter one or more email addresses.</span></span> <span data-ttu-id="f0bd0-139">Uživatelé Hello, potřebujete účet Microsoft nebo účet organizace (z Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="f0bd0-139">hello users need a valid Microsoft account or an organizational account (from Azure Active Directory).</span></span>

6. <span data-ttu-id="f0bd0-140">Vyberte, zda chcete, aby uživatelé hello tooadd jako vlastníka nebo uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-140">Select whether you want tooadd hello users as Owner or User.</span></span>

7. <span data-ttu-id="f0bd0-141">Klikněte na tlačítko hello **OK** značku zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-141">Click hello **OK** checkmark button.</span></span>

<span data-ttu-id="f0bd0-142">Každý uživatel, které přidáte obdrží e-mail s pokyny, jak sdílet toosign v toohello prostoru.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-142">Each user you add will receive an email with instructions on how toosign in toohello shared workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="f0bd0-143">Pro možnost toodeploy toobe uživatelé nebo spravovat webových služeb v tomto pracovním prostoru, musí to být Přispěvatel nebo správce v hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="f0bd0-143">For users toobe able toodeploy or manage web services in this workspace, they must be a contributor or administrator in hello Azure subscription.</span></span> 



