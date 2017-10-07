---
title: "aaaManage záznamy řízení přístupu v zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak řízení přístupu toouse záznamy toodetermine (ACRs), které hostitele může připojit tooa svazek v zařízení StorSimple hello."
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
ms.openlocfilehash: a1e718c2679301b34221a233557a1eaae869a94f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a><span data-ttu-id="258dc-103">Použití služby StorSimple Manager hello, toomanage záznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="258dc-103">Use hello StorSimple Manager service toomanage access control records</span></span>
## <a name="overview"></a><span data-ttu-id="258dc-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="258dc-104">Overview</span></span>
<span data-ttu-id="258dc-105">Záznamy řízení přístupu (ACRs) umožňují toospecify, které hostitele může připojit tooa svazek v zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="258dc-105">Access control records (ACRs) allow you toospecify which hosts can connect tooa volume on hello StorSimple device.</span></span> <span data-ttu-id="258dc-106">ACRs jsou nastavené určité svazku tooa a obsahovat hello iSCSI (IQN) kvalifikované názvy hostitelů hello.</span><span class="sxs-lookup"><span data-stu-id="258dc-106">ACRs are set tooa specific volume and contain hello iSCSI Qualified Names (IQNs) of hello hosts.</span></span> <span data-ttu-id="258dc-107">Když se hostitel pokusí tooconnect tooa svazku, zkontroluje zařízení hello hello ACR přidruženého tento svazek pro název IQN hello a pokud je nalezen, pak hello připojení.</span><span class="sxs-lookup"><span data-stu-id="258dc-107">When a host tries tooconnect tooa volume, hello device checks hello ACR associated with that volume for hello IQN name and if there is a match, then hello connection is established.</span></span> <span data-ttu-id="258dc-108">řízení přístupu Hello zaznamenává část hello **konfigurace** stránka zobrazuje všechny záznamy řízení přístupu hello hello odpovídající IQN hello hostitelů.</span><span class="sxs-lookup"><span data-stu-id="258dc-108">hello access control records section on hello **Configure** page displays all hello access control records with hello corresponding IQNs of hello hosts.</span></span>

<span data-ttu-id="258dc-109">Tento kurz vysvětluje hello následující běžné úlohy související s ACR:</span><span class="sxs-lookup"><span data-stu-id="258dc-109">This tutorial explains hello following common ACR-related tasks:</span></span>

* <span data-ttu-id="258dc-110">Přidání záznamu o řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="258dc-110">Add an access control record</span></span> 
* <span data-ttu-id="258dc-111">Upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="258dc-111">Edit an access control record</span></span> 
* <span data-ttu-id="258dc-112">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="258dc-112">Delete an access control record</span></span> 

> [!IMPORTANT]
> * <span data-ttu-id="258dc-113">Při přiřazování svazek tooa ACR, vezměte v potaz, že hello svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku hello.</span><span class="sxs-lookup"><span data-stu-id="258dc-113">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span> 
> * <span data-ttu-id="258dc-114">Při odstraňování ACR ze svazku, ujistěte se, že tohoto hostitele odpovídající hello není přístup k hello svazek, protože odstranění hello může mít za následek přerušení pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="258dc-114">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>
> 
> 

## <a name="add-an-access-control-record"></a><span data-ttu-id="258dc-115">Přidání záznamu o řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="258dc-115">Add an access control record</span></span>
<span data-ttu-id="258dc-116">Použít službu StorSimple Manager hello **konfigurace** stránka tooadd ACRs.</span><span class="sxs-lookup"><span data-stu-id="258dc-116">You use hello StorSimple Manager service **Configure** page tooadd ACRs.</span></span> <span data-ttu-id="258dc-117">Obvykle přidružíte jednu ACR jeden svazek.</span><span class="sxs-lookup"><span data-stu-id="258dc-117">Typically, you will associate one ACR with one volume.</span></span>

<span data-ttu-id="258dc-118">Proveďte následující kroky tooadd ACR hello.</span><span class="sxs-lookup"><span data-stu-id="258dc-118">Perform hello following steps tooadd an ACR.</span></span>

#### <a name="tooadd-an-access-control-record"></a><span data-ttu-id="258dc-119">tooadd záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="258dc-119">tooadd an access control record</span></span>
1. <span data-ttu-id="258dc-120">Na cílovou stránku hello služby, vyberte svoji službu, dvakrát klikněte na název služby hello a pak klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="258dc-120">On hello service landing page, select your service, double-click hello service name, and then click hello **Configure** tab.</span></span>
2. <span data-ttu-id="258dc-121">V tabulkovém výpis pod hello **záznamy řízení přístupu**, dodávky **název** acr.</span><span class="sxs-lookup"><span data-stu-id="258dc-121">In hello tabular listing under **Access control records**, supply a **Name** for your ACR.</span></span>
3. <span data-ttu-id="258dc-122">Zadejte název IQN hello hostitele s Windows v rámci **název iniciátoru iSCSI**.</span><span class="sxs-lookup"><span data-stu-id="258dc-122">Provide hello IQN name of your Windows host under **iSCSI Initiator Name**.</span></span> <span data-ttu-id="258dc-123">hello tooget názvu IQN hostitele vašeho systému Windows Server hello následující:</span><span class="sxs-lookup"><span data-stu-id="258dc-123">tooget hello IQN of your Windows Server host, do hello following:</span></span>
   
   * <span data-ttu-id="258dc-124">Na hostiteli s Windows spusťte iniciátor iSCSI společnosti Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="258dc-124">Start hello Microsoft iSCSI initiator on your Windows host.</span></span>
   * <span data-ttu-id="258dc-125">V hello **vlastnosti iniciátoru iSCSI** okně na hello **konfigurace** , vyberte a zkopírujte řetězec hello z hello **název iniciátoru** pole.</span><span class="sxs-lookup"><span data-stu-id="258dc-125">In hello **iSCSI Initiator Properties** window, on hello **Configuration** tab, select and copy hello string from hello **Initiator Name** field.</span></span>
   * <span data-ttu-id="258dc-126">Vložte tento řetězec hello **název iniciátoru iSCSI** pole v tabulce ACRs hello v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="258dc-126">Paste this string in hello **iSCSI Initiator Name** field on hello ACRs table in hello Azure classic portal.</span></span>
4. <span data-ttu-id="258dc-127">Klikněte na tlačítko **Uložit** toosave hello nově vytvořený ACR.</span><span class="sxs-lookup"><span data-stu-id="258dc-127">Click **Save** toosave hello newly created ACR.</span></span> <span data-ttu-id="258dc-128">Hello tabulkové výpis bude možné aktualizované tooreflect přidání.</span><span class="sxs-lookup"><span data-stu-id="258dc-128">hello tabular listing will be updated tooreflect this addition.</span></span>

## <a name="edit-an-access-control-record"></a><span data-ttu-id="258dc-129">Upravit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="258dc-129">Edit an access control record</span></span>
<span data-ttu-id="258dc-130">Použít hello **konfigurace** stránku hello ACRs Azure tooedit portálu classic.</span><span class="sxs-lookup"><span data-stu-id="258dc-130">You use hello **Configure** page in hello Azure classic portal tooedit ACRs.</span></span> 

> [!NOTE]
> <span data-ttu-id="258dc-131">Můžete upravit pouze ACRs, které nejsou aktuálně používá.</span><span class="sxs-lookup"><span data-stu-id="258dc-131">You can modify only those ACRs that are currently not in use.</span></span> <span data-ttu-id="258dc-132">tooedit přidruženého ACR svazku, který je aktuálně používán, musíte nejdřív udělat hello svazek offline.</span><span class="sxs-lookup"><span data-stu-id="258dc-132">tooedit an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>
> 
> 

<span data-ttu-id="258dc-133">Proveďte následující kroky tooedit ACR hello.</span><span class="sxs-lookup"><span data-stu-id="258dc-133">Perform hello following steps tooedit an ACR.</span></span>

#### <a name="tooedit-an-access-control-record"></a><span data-ttu-id="258dc-134">tooedit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="258dc-134">tooedit an access control record</span></span>
1. <span data-ttu-id="258dc-135">Na cílovou stránku hello služby, vyberte svoji službu, dvakrát klikněte na název služby hello a pak klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="258dc-135">On hello service landing page, select your service, double-click hello service name, and then click hello **Configure** tab.</span></span>
2. <span data-ttu-id="258dc-136">V tabulkovém seznamu hello záznamů hello řízení přístupu, pozastavte ukazatel myši nad hello ACR chcete toomodify.</span><span class="sxs-lookup"><span data-stu-id="258dc-136">In hello tabular listing of hello access control records, hover over hello ACR that you wish toomodify.</span></span>
3. <span data-ttu-id="258dc-137">Zadejte nový název nebo název IQN hello ACR.</span><span class="sxs-lookup"><span data-stu-id="258dc-137">Supply a new name and/or IQN for hello ACR.</span></span>
4. <span data-ttu-id="258dc-138">Klikněte na tlačítko **Uložit** toosave hello upravit ACR.</span><span class="sxs-lookup"><span data-stu-id="258dc-138">Click **Save** toosave hello modified ACR.</span></span> <span data-ttu-id="258dc-139">Hello tabulkové výpis bude možné aktualizované tooreflect tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="258dc-139">hello tabular listing will be updated tooreflect this change.</span></span>

## <a name="delete-an-access-control-record"></a><span data-ttu-id="258dc-140">Odstranit záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="258dc-140">Delete an access control record</span></span>
<span data-ttu-id="258dc-141">Použít hello **konfigurace** stránku hello ACRs Azure toodelete portálu classic.</span><span class="sxs-lookup"><span data-stu-id="258dc-141">You use hello **Configure** page in hello Azure classic portal toodelete ACRs.</span></span> 

> [!NOTE]
> <span data-ttu-id="258dc-142">Lze odstranit pouze ACRs, které nejsou aktuálně používá.</span><span class="sxs-lookup"><span data-stu-id="258dc-142">You can delete only those ACRs that are currently not in use.</span></span> <span data-ttu-id="258dc-143">toodelete přidruženého ACR svazku, který je aktuálně používán, musíte nejdřív udělat hello svazek offline.</span><span class="sxs-lookup"><span data-stu-id="258dc-143">toodelete an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>
> 
> 

<span data-ttu-id="258dc-144">Proveďte následující kroky toodelete záznam řízení přístupu hello.</span><span class="sxs-lookup"><span data-stu-id="258dc-144">Perform hello following steps toodelete an access control record.</span></span>

#### <a name="toodelete-an-access-control-record"></a><span data-ttu-id="258dc-145">toodelete záznam řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="258dc-145">toodelete an access control record</span></span>
1. <span data-ttu-id="258dc-146">Na cílovou stránku hello služby, vyberte svoji službu, dvakrát klikněte na název služby hello a pak klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="258dc-146">On hello service landing page, select your service, double-click hello service name, and then click hello **Configure** tab.</span></span>
2. <span data-ttu-id="258dc-147">V hello tabulkové výpis hello záznamy řízení přístupu (ACRs), přejděte myší hello ACR chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="258dc-147">In hello tabular listing of hello access control records (ACRs), hover over hello ACR that you wish toodelete.</span></span>
3. <span data-ttu-id="258dc-148">Ikony odstranění (**x**) se zobrazí v hello extrémně pravém sloupci pro hello ACR, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="258dc-148">A delete icon (**x**) will appear in hello extreme right column for hello ACR that you select.</span></span> <span data-ttu-id="258dc-149">Klikněte na tlačítko hello **x** ikonu toodelete hello ACR.</span><span class="sxs-lookup"><span data-stu-id="258dc-149">Click hello **x** icon toodelete hello ACR.</span></span>
4. <span data-ttu-id="258dc-150">Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** toocontinue s hello odstranění.</span><span class="sxs-lookup"><span data-stu-id="258dc-150">When prompted for confirmation, click **YES** toocontinue with hello deletion.</span></span> <span data-ttu-id="258dc-151">tabulkový výčet Hello bude aktualizované tooreflect hello odstranění.</span><span class="sxs-lookup"><span data-stu-id="258dc-151">hello tabular listing will be updated tooreflect hello deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="258dc-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="258dc-152">Next steps</span></span>
* <span data-ttu-id="258dc-153">Další informace o [správu svazků zařízení StorSimple](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="258dc-153">Learn more about [managing StorSimple volumes](storsimple-manage-volumes.md).</span></span>
* <span data-ttu-id="258dc-154">Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="258dc-154">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

