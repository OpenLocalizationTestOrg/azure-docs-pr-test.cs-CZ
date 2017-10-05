---
title: "Vytvoření testovacího prostředí ve službě Azure DevTest Labs | Dokumentace Microsoftu"
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
ms.openlocfilehash: 265a968f902f53c7561c8c7e937f8eacfdb37167
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="ba826-103">Vytvoření testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="ba826-103">Create a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="ba826-104">Testovací prostředí ve službě Azure DevTest Labs je infrastruktura, která zahrnuje skupinu prostředků, třeba službu Virtual Machines, která vám umožní lépe spravovat tyto prostředky zadáním omezení a kvót.</span><span class="sxs-lookup"><span data-stu-id="ba826-104">A lab in Azure DevTest Labs is the infrastructure that encompasses a group of resources, such as Virtual Machines (VMs), that lets you better manage those resources by specifying limits and quotas.</span></span> <span data-ttu-id="ba826-105">Tento článek vás provede procesem vytvoření testovacího prostředí pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ba826-105">This article walks you through the process of creating a lab using the Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba826-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ba826-106">Prerequisites</span></span>
<span data-ttu-id="ba826-107">K vytvoření testovacího prostředí potřebujete:</span><span class="sxs-lookup"><span data-stu-id="ba826-107">To create a lab, you need:</span></span>

* <span data-ttu-id="ba826-108">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ba826-108">An Azure subscription.</span></span> <span data-ttu-id="ba826-109">Informace o možnostech nákupu Azure najdete v tématech [Jak koupit Azure](https://azure.microsoft.com/pricing/purchase-options/) nebo [Bezplatná zkušební verze na jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ba826-109">To learn about Azure purchase options, see [How to buy Azure](https://azure.microsoft.com/pricing/purchase-options/) or [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="ba826-110">Abyste mohli vytvořit testovací prostředí, musíte být vlastníky předplatného.</span><span class="sxs-lookup"><span data-stu-id="ba826-110">You must be the owner of the subscription to create the lab.</span></span>

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="ba826-111">Postup vytvoření testovacího prostředí ve službě Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="ba826-111">Steps to create a lab in Azure DevTest Labs</span></span>
<span data-ttu-id="ba826-112">Následující kroky ukazují postup vytvoření testovacího prostředí ve službě Azure DevTest Labs pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ba826-112">The following steps illustrate how to use the Azure portal to create a lab in Azure DevTest Labs.</span></span> 

1. <span data-ttu-id="ba826-113">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="ba826-113">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="ba826-114">V hlavní nabídce na levé straně vyberte **Další služby** (v dolní části seznamu).</span><span class="sxs-lookup"><span data-stu-id="ba826-114">From the main menu on the left side, select **More Services** (at the bottom of the list).</span></span>

    ![Možnost nabídky Další služby](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. <span data-ttu-id="ba826-116">V seznamu dostupných služeb vyberte **DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="ba826-116">From the list of available services, **DevTest Labs**.</span></span>
1. <span data-ttu-id="ba826-117">V okně **DevTest Labs** vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ba826-117">On the **DevTest Labs** blade, select **Add**.</span></span>
   
    ![Přidání testovacího prostředí](./media/devtest-lab-create-lab/add-lab-button.png)

1. <span data-ttu-id="ba826-119">V okně pro **vytvoření testovacího prostředí DevTest**:</span><span class="sxs-lookup"><span data-stu-id="ba826-119">On the **Create a DevTest Lab** blade:</span></span>
   
    1. <span data-ttu-id="ba826-120">Zadejte **Název testovacího prostředí** pro nové prostředí.</span><span class="sxs-lookup"><span data-stu-id="ba826-120">Enter a **Lab Name** for the new lab.</span></span>
    2. <span data-ttu-id="ba826-121">Vyberte **Předplatné**, které bude přidruženo k testovacímu prostředí.</span><span class="sxs-lookup"><span data-stu-id="ba826-121">Select the **Subscription** to associate with the lab.</span></span>
    3. <span data-ttu-id="ba826-122">Vyberte **Umístění**, do kterého se testovací prostředí uloží.</span><span class="sxs-lookup"><span data-stu-id="ba826-122">Select a **Location** in which to store the lab.</span></span>
    4. <span data-ttu-id="ba826-123">Chcete-li povolit a nastavit parametry pro automatické vypínání virtuálních počítačů v testovacím prostředí, vyberte **Automatické vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="ba826-123">Select **Auto-shutdown** to specify if you want to enable - and define the parameters for - the automatic shutting down of all the lab's VMs.</span></span> <span data-ttu-id="ba826-124">Funkce automatického vypnutí je především funkce na úsporu nákladů, pomocí které můžete určit, kdy se má virtuální počítač automaticky vypnout.</span><span class="sxs-lookup"><span data-stu-id="ba826-124">The auto-shutdown feature is mainly a cost-saving feature whereby you can specify when you want the VM to automatically be shut down.</span></span> <span data-ttu-id="ba826-125">Po vytvoření testovacího prostředí můžete nastavení automatického vypnutí změnit podle kroků uvedených v článku [Správa všech zásad pro testovací prostředí v Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).</span><span class="sxs-lookup"><span data-stu-id="ba826-125">You can change auto-shutdown settings after creating the lab by following the steps outlined in the article, [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).</span></span>
    5. <span data-ttu-id="ba826-126">Pokud chcete na řídicím panelu portálu vytvořit zástupce testovacího prostředí, vyberte možnost **Připnout na řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="ba826-126">Select **Pin to Dashboard** if you want a shortcut of the lab to appear on the portal dashboard.</span></span>
    6. <span data-ttu-id="ba826-127">Pokud chcete získat šablony Azure Resource Manageru pro automatizaci konfigurace, vyberte **Možnosti automatizace**.</span><span class="sxs-lookup"><span data-stu-id="ba826-127">Select **Automation options** to get Azure Resource Manager templates for configuration automation.</span></span> 
    7. <span data-ttu-id="ba826-128">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ba826-128">Select **Create**.</span></span> <span data-ttu-id="ba826-129">Po výběru možnosti **Vytvořit** se zobrazí okno **DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="ba826-129">After selecting **Create**, the **DevTest Labs** blade displays.</span></span> <span data-ttu-id="ba826-130">Stav procesu vytvoření testovacího prostředí můžete monitorovat v oblasti **Oznámení**.</span><span class="sxs-lookup"><span data-stu-id="ba826-130">You can monitor the status of the lab creation process by watching the **Notifications** area.</span></span> <span data-ttu-id="ba826-131">Po dokončení aktualizujte stránku, abyste zobrazili nově vytvořené testovací prostředí v seznamu testovacích prostředí.</span><span class="sxs-lookup"><span data-stu-id="ba826-131">Once completed, refresh the page to see the newly created lab in the list of labs.</span></span>  
    
    ![Okno pro vytvoření testovacího prostředí](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="ba826-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba826-133">Next steps</span></span>
<span data-ttu-id="ba826-134">Po vytvoření testovacího prostředí je zde několik kroků, které je vhodné zvážit:</span><span class="sxs-lookup"><span data-stu-id="ba826-134">Once you've created your lab, here are some next steps to consider:</span></span>

* <span data-ttu-id="ba826-135">[Zabezpečení přístupu k testovacímu prostředí](devtest-lab-add-devtest-user.md)</span><span class="sxs-lookup"><span data-stu-id="ba826-135">[Secure access to a lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="ba826-136">[Nastavení zásad testovacího prostředí](devtest-lab-set-lab-policy.md)</span><span class="sxs-lookup"><span data-stu-id="ba826-136">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="ba826-137">[Vytvoření šablony testovacího prostředí](devtest-lab-create-template.md)</span><span class="sxs-lookup"><span data-stu-id="ba826-137">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="ba826-138">[Vytvoření vlastních artefaktů pro virtuální počítače](devtest-lab-artifact-author.md)</span><span class="sxs-lookup"><span data-stu-id="ba826-138">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="ba826-139">[Přidání virtuálního počítače s artefakty do testovacího prostředí](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/)</span><span class="sxs-lookup"><span data-stu-id="ba826-139">[Add a VM with artifacts to a lab](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).</span></span>

