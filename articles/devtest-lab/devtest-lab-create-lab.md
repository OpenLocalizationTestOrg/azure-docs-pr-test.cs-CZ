---
title: "aaaCreate testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Vytvoření testovacího prostředí ve službě Azure DevTest Labs pro virtuální počítače"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/30/2017
ms.author: tarcher
ms.openlocfilehash: 2ec5498f10e0cc06a196a71e319e340937abb1a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="3c9ff-103">Vytvoření testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="3c9ff-103">Create a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="3c9ff-104">Testovacího prostředí v Azure DevTest Labs je hello infrastrukturu, která zahrnuje skupinu prostředků, třeba virtuální počítače (VM), které vám umožní lépe spravovat tyto prostředky zadáním omezení a kvóty.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-104">A lab in Azure DevTest Labs is hello infrastructure that encompasses a group of resources, such as Virtual Machines (VMs), that lets you better manage those resources by specifying limits and quotas.</span></span> <span data-ttu-id="3c9ff-105">Tento článek vás provede procesem vytvoření testovacího prostředí pomocí portálu Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-105">This article walks you through hello process of creating a lab using hello Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c9ff-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3c9ff-106">Prerequisites</span></span>
<span data-ttu-id="3c9ff-107">toocreate testovacím prostředí, je třeba:</span><span class="sxs-lookup"><span data-stu-id="3c9ff-107">toocreate a lab, you need:</span></span>

* <span data-ttu-id="3c9ff-108">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-108">An Azure subscription.</span></span> <span data-ttu-id="3c9ff-109">toolearn o možnostech nákupu Azure najdete v části [jak toobuy Azure](https://azure.microsoft.com/pricing/purchase-options/) nebo [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3c9ff-109">toolearn about Azure purchase options, see [How toobuy Azure](https://azure.microsoft.com/pricing/purchase-options/) or [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="3c9ff-110">Musíte být vlastníkem hello hello předplatné toocreate hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-110">You must be hello owner of hello subscription toocreate hello lab.</span></span>

## <a name="steps-toocreate-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="3c9ff-111">Kroky toocreate testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="3c9ff-111">Steps toocreate a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="3c9ff-112">Hello následující kroky ukazují, jak toouse hello Azure portálu toocreate testovacího prostředí v Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-112">hello following steps illustrate how toouse hello Azure portal toocreate a lab in Azure DevTest Labs.</span></span> 

1. <span data-ttu-id="3c9ff-113">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="3c9ff-113">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="3c9ff-114">Hello hlavní nabídky na levé straně hello, vyberte **více služeb** (v hello dolní části seznamu hello).</span><span class="sxs-lookup"><span data-stu-id="3c9ff-114">From hello main menu on hello left side, select **More Services** (at hello bottom of hello list).</span></span>

    ![Možnost nabídky Další služby](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. <span data-ttu-id="3c9ff-116">Ze seznamu dostupných služeb, hello **DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-116">From hello list of available services, **DevTest Labs**.</span></span>
1. <span data-ttu-id="3c9ff-117">Na hello **DevTest Labs** vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-117">On hello **DevTest Labs** blade, select **Add**.</span></span>
   
    ![Přidání testovacího prostředí](./media/devtest-lab-create-lab/add-lab-button.png)

1. <span data-ttu-id="3c9ff-119">Na hello **vytvoření testovacího prostředí DevTest** okno:</span><span class="sxs-lookup"><span data-stu-id="3c9ff-119">On hello **Create a DevTest Lab** blade:</span></span>
   
    1. <span data-ttu-id="3c9ff-120">Zadejte **název testovacího prostředí** pro hello nového testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-120">Enter a **Lab Name** for hello new lab.</span></span>
    2. <span data-ttu-id="3c9ff-121">Vyberte hello **předplatné** tooassociate s testovacím hello.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-121">Select hello **Subscription** tooassociate with hello lab.</span></span>
    3. <span data-ttu-id="3c9ff-122">Vyberte **umístění** v testovacím prostředí které toostore hello.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-122">Select a **Location** in which toostore hello lab.</span></span>
    4. <span data-ttu-id="3c9ff-123">Vyberte **Auto-shutdown** toospecify Pokud chcete tooenable - a definovat parametry hello - hello automatické vypínání virtuálních počítačů. všechny hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-123">Select **Auto-shutdown** toospecify if you want tooenable - and define hello parameters for - hello automatic shutting down of all hello lab's VMs.</span></span> <span data-ttu-id="3c9ff-124">Hello automaticky vypnutí funkce je především náklady na ukládání funkce, které můžete zadat, když chcete hello virtuální počítač vypnout tooautomatically.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-124">hello auto-shutdown feature is mainly a cost-saving feature whereby you can specify when you want hello VM tooautomatically be shut down.</span></span> <span data-ttu-id="3c9ff-125">Vypnutí automatického nastavení můžete změnit po vytvoření testovacího prostředí hello podle následujících kroků hello uvedených v článku hello [Správa všech zásad pro testovací prostředí v Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).</span><span class="sxs-lookup"><span data-stu-id="3c9ff-125">You can change auto-shutdown settings after creating hello lab by following hello steps outlined in hello article, [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).</span></span>
    5. <span data-ttu-id="3c9ff-126">Vyberte **Pin tooDashboard** Pokud chcete zástupce hello tooappear testovacího prostředí na řídicí panel portálu hello.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-126">Select **Pin tooDashboard** if you want a shortcut of hello lab tooappear on hello portal dashboard.</span></span>
    6. <span data-ttu-id="3c9ff-127">Vyberte **Možnosti automatizace** tooget šablon Azure Resource Manageru pro automatizaci konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-127">Select **Automation options** tooget Azure Resource Manager templates for configuration automation.</span></span> 
    7. <span data-ttu-id="3c9ff-128">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-128">Select **Create**.</span></span> <span data-ttu-id="3c9ff-129">Po výběru **vytvořit**, hello **DevTest Labs** zobrazí okno.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-129">After selecting **Create**, hello **DevTest Labs** blade displays.</span></span> <span data-ttu-id="3c9ff-130">Stav hello hello proces vytvoření testovacího prostředí můžete sledovat pomocí sledování hello **oznámení** oblasti.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-130">You can monitor hello status of hello lab creation process by watching hello **Notifications** area.</span></span> <span data-ttu-id="3c9ff-131">Po dokončení aktualizace hello stránky toosee hello nově vytvořený testovacího prostředí v seznamu hello laboratoře.</span><span class="sxs-lookup"><span data-stu-id="3c9ff-131">Once completed, refresh hello page toosee hello newly created lab in hello list of labs.</span></span>  
    
    ![Okno pro vytvoření testovacího prostředí](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="3c9ff-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c9ff-133">Next steps</span></span>
<span data-ttu-id="3c9ff-134">Po vytvoření testovacího prostředí, zde jsou některé další kroky tooconsider:</span><span class="sxs-lookup"><span data-stu-id="3c9ff-134">Once you've created your lab, here are some next steps tooconsider:</span></span>

* <span data-ttu-id="3c9ff-135">[Laboratoř tooa zabezpečeného přístupu](devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="3c9ff-135">[Secure access tooa lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="3c9ff-136">[Nastavení zásad testovacího prostředí](devtest-lab-set-lab-policy.md)</span><span class="sxs-lookup"><span data-stu-id="3c9ff-136">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="3c9ff-137">[Vytvoření šablony testovacího prostředí](devtest-lab-create-template.md)</span><span class="sxs-lookup"><span data-stu-id="3c9ff-137">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="3c9ff-138">[Vytvoření vlastních artefaktů pro virtuální počítače](devtest-lab-artifact-author.md)</span><span class="sxs-lookup"><span data-stu-id="3c9ff-138">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="3c9ff-139">[Přidání virtuálního počítače s artefakty tooa testovacím](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).</span><span class="sxs-lookup"><span data-stu-id="3c9ff-139">[Add a VM with artifacts tooa lab](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).</span></span>

