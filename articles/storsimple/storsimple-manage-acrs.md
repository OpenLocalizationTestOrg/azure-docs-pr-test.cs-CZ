---
title: "Spravovat záznamy řízení přístupu v zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak používat záznamy řízení přístupu (ACRs) k určení, které hostitele může připojit k svazek v zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2f1475d8-36a5-4cc4-84b9-adf8a310b60c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: a87624b5706c1d9b8c2b9926e5580996a89ce984
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a><span data-ttu-id="42878-103">Použít službu StorSimple Manager ke správě záznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="42878-103">Use the StorSimple Manager service to manage access control records</span></span>
## <a name="overview"></a><span data-ttu-id="42878-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="42878-104">Overview</span></span>
<span data-ttu-id="42878-105">Záznamy řízení přístupu (ACRs) umožňují určit, které hostitele může připojit k svazek v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="42878-105">Access control records (ACRs) allow you to specify which hosts can connect to a volume on the StorSimple device.</span></span> <span data-ttu-id="42878-106">ACRs jsou nastaveny na konkrétním svazku a obsahovat kvalifikované názvy iSCSI (IQN) hostitele.</span><span class="sxs-lookup"><span data-stu-id="42878-106">ACRs are set to a specific volume and contain the iSCSI Qualified Names (IQNs) of the hosts.</span></span> <span data-ttu-id="42878-107">Když hostitel pokusí o připojení ke svazku, zařízení se kontroluje ACR přidružený tento svazek pro název IQN a pokud je nalezena shoda, pak připojení.</span><span class="sxs-lookup"><span data-stu-id="42878-107">When a host tries to connect to a volume, the device checks the ACR associated with that volume for the IQN name and if there is a match, then the connection is established.</span></span> <span data-ttu-id="42878-108">Řízení přístupu na zaznamenává části **konfigurace** stránka zobrazuje všechny záznamy řízení přístupu s odpovídající IQN hostitelů.</span><span class="sxs-lookup"><span data-stu-id="42878-108">The access control records section on the **Configure** page displays all the access control records with the corresponding IQNs of the hosts.</span></span>

<span data-ttu-id="42878-109">Tento kurz vysvětluje následující běžné úlohy související s ACR:</span><span class="sxs-lookup"><span data-stu-id="42878-109">This tutorial explains the following common ACR-related tasks:</span></span>

* <span data-ttu-id="42878-110">Přidání záznamu o řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="42878-110">Add an access control record</span></span> 
* <span data-ttu-id="42878-111">Upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="42878-111">Edit an access control record</span></span> 
* <span data-ttu-id="42878-112">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="42878-112">Delete an access control record</span></span> 

> [!IMPORTANT]
> * <span data-ttu-id="42878-113">Při přiřazování ACR svazku, vezměte v potaz, že svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku.</span><span class="sxs-lookup"><span data-stu-id="42878-113">When assigning an ACR to a volume, take care that the volume is not concurrently accessed by more than one non-clustered host because this could corrupt the volume.</span></span> 
> * <span data-ttu-id="42878-114">Při odstraňování ACR ze svazku, ujistěte se, že odpovídající hostitele není přístup k svazku vzhledem k tomu, že odstranění může mít za následek přerušení pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="42878-114">When deleting an ACR from a volume, make sure that the corresponding host is not accessing the volume because the deletion could result in a read-write disruption.</span></span>
> 
> 

## <a name="add-an-access-control-record"></a><span data-ttu-id="42878-115">Přidání záznamu o řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="42878-115">Add an access control record</span></span>
<span data-ttu-id="42878-116">Použít službu StorSimple Manager **konfigurace** stránku přidáte ACRs.</span><span class="sxs-lookup"><span data-stu-id="42878-116">You use the StorSimple Manager service **Configure** page to add ACRs.</span></span> <span data-ttu-id="42878-117">Obvykle přidružíte jednu ACR jeden svazek.</span><span class="sxs-lookup"><span data-stu-id="42878-117">Typically, you will associate one ACR with one volume.</span></span>

<span data-ttu-id="42878-118">Proveďte následující postup pro přidání ACR.</span><span class="sxs-lookup"><span data-stu-id="42878-118">Perform the following steps to add an ACR.</span></span>

#### <a name="to-add-an-access-control-record"></a><span data-ttu-id="42878-119">Chcete-li přidat záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="42878-119">To add an access control record</span></span>
1. <span data-ttu-id="42878-120">Na stránce cílové služby, vyberte svoji službu, dvakrát klikněte na název služby a klikněte **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="42878-120">On the service landing page, select your service, double-click the service name, and then click the **Configure** tab.</span></span>
2. <span data-ttu-id="42878-121">V tabulkovém seznamu pod **záznamy řízení přístupu**, dodávky **název** acr.</span><span class="sxs-lookup"><span data-stu-id="42878-121">In the tabular listing under **Access control records**, supply a **Name** for your ACR.</span></span>
3. <span data-ttu-id="42878-122">Zadejte název IQN hostitele s Windows v rámci **název iniciátoru iSCSI**.</span><span class="sxs-lookup"><span data-stu-id="42878-122">Provide the IQN name of your Windows host under **iSCSI Initiator Name**.</span></span> <span data-ttu-id="42878-123">Získání názvu IQN hostitele vašeho systému Windows Server, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="42878-123">To get the IQN of your Windows Server host, do the following:</span></span>
   
   * <span data-ttu-id="42878-124">Na hostiteli s Windows spusťte iniciátor iSCSI od Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="42878-124">Start the Microsoft iSCSI initiator on your Windows host.</span></span>
   * <span data-ttu-id="42878-125">V okně **iSCSI Initiator Properties** (Vlastnosti iniciátoru iSCSI) na kartě **Konfigurace** vyberte a zkopírujte řetězec z pole **Název iniciátoru**.</span><span class="sxs-lookup"><span data-stu-id="42878-125">In the **iSCSI Initiator Properties** window, on the **Configuration** tab, select and copy the string from the **Initiator Name** field.</span></span>
   * <span data-ttu-id="42878-126">Vložte tento řetězec v **název iniciátoru iSCSI** pole v tabulce ACRs na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="42878-126">Paste this string in the **iSCSI Initiator Name** field on the ACRs table in the Azure classic portal.</span></span>
4. <span data-ttu-id="42878-127">Klikněte na tlačítko **Uložit** uložte nově vytvořený ACR.</span><span class="sxs-lookup"><span data-stu-id="42878-127">Click **Save** to save the newly created ACR.</span></span> <span data-ttu-id="42878-128">Tabulkové výpis budou aktualizovány tak, aby odrážela přidání.</span><span class="sxs-lookup"><span data-stu-id="42878-128">The tabular listing will be updated to reflect this addition.</span></span>

## <a name="edit-an-access-control-record"></a><span data-ttu-id="42878-129">Upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="42878-129">Edit an access control record</span></span>
<span data-ttu-id="42878-130">Můžete použít **konfigurace** na portálu Azure classic upravit ACRs.</span><span class="sxs-lookup"><span data-stu-id="42878-130">You use the **Configure** page in the Azure classic portal to edit ACRs.</span></span> 

> [!NOTE]
> <span data-ttu-id="42878-131">Můžete upravit pouze ACRs, které nejsou aktuálně používá.</span><span class="sxs-lookup"><span data-stu-id="42878-131">You can modify only those ACRs that are currently not in use.</span></span> <span data-ttu-id="42878-132">Chcete-li upravit ACR přidružené k svazku, který je aktuálně používán, musíte nejdřív udělat svazek offline.</span><span class="sxs-lookup"><span data-stu-id="42878-132">To edit an ACR associated with a volume that is currently in use, you must first take the volume offline.</span></span>
> 
> 

<span data-ttu-id="42878-133">Proveďte následující kroky, chcete-li upravit ACR.</span><span class="sxs-lookup"><span data-stu-id="42878-133">Perform the following steps to edit an ACR.</span></span>

#### <a name="to-edit-an-access-control-record"></a><span data-ttu-id="42878-134">Chcete-li upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="42878-134">To edit an access control record</span></span>
1. <span data-ttu-id="42878-135">Na stránce cílové služby, vyberte svoji službu, dvakrát klikněte na název služby a klikněte **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="42878-135">On the service landing page, select your service, double-click the service name, and then click the **Configure** tab.</span></span>
2. <span data-ttu-id="42878-136">V tabulkovém seznam záznamy řízení přístupu, pozastavte ukazatel myši nad ACR, který chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="42878-136">In the tabular listing of the access control records, hover over the ACR that you wish to modify.</span></span>
3. <span data-ttu-id="42878-137">Zadejte nový název nebo název IQN ACR.</span><span class="sxs-lookup"><span data-stu-id="42878-137">Supply a new name and/or IQN for the ACR.</span></span>
4. <span data-ttu-id="42878-138">Klikněte na tlačítko **Uložit** uložit upravené ACR.</span><span class="sxs-lookup"><span data-stu-id="42878-138">Click **Save** to save the modified ACR.</span></span> <span data-ttu-id="42878-139">Tabulkové výpis budou aktualizovány tak, aby odrážela tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="42878-139">The tabular listing will be updated to reflect this change.</span></span>

## <a name="delete-an-access-control-record"></a><span data-ttu-id="42878-140">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="42878-140">Delete an access control record</span></span>
<span data-ttu-id="42878-141">Můžete použít **konfigurace** na portálu Azure classic odstranit ACRs.</span><span class="sxs-lookup"><span data-stu-id="42878-141">You use the **Configure** page in the Azure classic portal to delete ACRs.</span></span> 

> [!NOTE]
> <span data-ttu-id="42878-142">Lze odstranit pouze ACRs, které nejsou aktuálně používá.</span><span class="sxs-lookup"><span data-stu-id="42878-142">You can delete only those ACRs that are currently not in use.</span></span> <span data-ttu-id="42878-143">Pokud chcete odstranit ACR přidružené k svazku, který je aktuálně používán, musíte nejdřív udělat svazek offline.</span><span class="sxs-lookup"><span data-stu-id="42878-143">To delete an ACR associated with a volume that is currently in use, you must first take the volume offline.</span></span>
> 
> 

<span data-ttu-id="42878-144">Proveďte následující kroky a odstraňte záznam řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="42878-144">Perform the following steps to delete an access control record.</span></span>

#### <a name="to-delete-an-access-control-record"></a><span data-ttu-id="42878-145">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="42878-145">To delete an access control record</span></span>
1. <span data-ttu-id="42878-146">Na stránce cílové služby, vyberte svoji službu, dvakrát klikněte na název služby a klikněte **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="42878-146">On the service landing page, select your service, double-click the service name, and then click the **Configure** tab.</span></span>
2. <span data-ttu-id="42878-147">V tabulkovém seznam záznamy řízení přístupu (ACRs), přejděte myší ACR, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="42878-147">In the tabular listing of the access control records (ACRs), hover over the ACR that you wish to delete.</span></span>
3. <span data-ttu-id="42878-148">Ikony odstranění (**x**) se zobrazí v pravém sloupci extrémně pro ACR, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="42878-148">A delete icon (**x**) will appear in the extreme right column for the ACR that you select.</span></span> <span data-ttu-id="42878-149">Klikněte **x** ikonu Odstranit ACR.</span><span class="sxs-lookup"><span data-stu-id="42878-149">Click the **x** icon to delete the ACR.</span></span>
4. <span data-ttu-id="42878-150">Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** Chcete-li pokračovat v odstraňování.</span><span class="sxs-lookup"><span data-stu-id="42878-150">When prompted for confirmation, click **YES** to continue with the deletion.</span></span> <span data-ttu-id="42878-151">Tabulkové výpis budou aktualizovány tak, aby odrážela odstranění.</span><span class="sxs-lookup"><span data-stu-id="42878-151">The tabular listing will be updated to reflect the deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42878-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42878-152">Next steps</span></span>
* <span data-ttu-id="42878-153">Další informace o [správu svazků zařízení StorSimple](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="42878-153">Learn more about [managing StorSimple volumes](storsimple-manage-volumes.md).</span></span>
* <span data-ttu-id="42878-154">Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="42878-154">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

