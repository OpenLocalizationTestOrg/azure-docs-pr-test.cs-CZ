---
title: "aaaSet nastavení replikace pro Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak toodeploy Site Recovery tooorchestrate replikace, převzetí služeb při selhání a obnovení virtuálních počítačů technologie Hyper-V v nástroji VMM cloudů tooAzure."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: 618e92e42411732a2a1bb75c5e5ea8a433cd7d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-replication-policy-for-vmware-tooazure"></a><span data-ttu-id="93fef-103">Správa zásad replikace pro VMware tooAzure</span><span class="sxs-lookup"><span data-stu-id="93fef-103">Manage replication policy for VMware tooAzure</span></span>


## <a name="create-a-replication-policy"></a><span data-ttu-id="93fef-104">Vytvoření zásady replikace</span><span class="sxs-lookup"><span data-stu-id="93fef-104">Create a replication policy</span></span>

1. <span data-ttu-id="93fef-105">Vyberte **Spravovat** > **Infrastruktura Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="93fef-105">Select **Manage** > **Site Recovery Infrastructure**.</span></span>
2. <span data-ttu-id="93fef-106">V části **Pro VMware a fyzické počítače** vyberte **Zásady replikace**.</span><span class="sxs-lookup"><span data-stu-id="93fef-106">Select **Replication policies** under **For VMware and Physical machines**.</span></span>
3. <span data-ttu-id="93fef-107">Vyberte **+Zásada replikace**.</span><span class="sxs-lookup"><span data-stu-id="93fef-107">Select **+Replication policy**.</span></span>

    ![Vytvoření zásady replikace](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. <span data-ttu-id="93fef-109">Zadejte název zásady hello.</span><span class="sxs-lookup"><span data-stu-id="93fef-109">Enter hello policy name.</span></span>

5. <span data-ttu-id="93fef-110">V **prahovou hodnotu RPO**, zadejte limit hello plánovaný bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="93fef-110">In **RPO threshold**, specify hello RPO limit.</span></span> <span data-ttu-id="93fef-111">Když toto omezení průběžná replikace překročí, vygenerují se upozornění.</span><span class="sxs-lookup"><span data-stu-id="93fef-111">Alerts will be generated when continuous replication exceeds this limit.</span></span>
6. <span data-ttu-id="93fef-112">V **uchování bodu obnovení**, zadejte (v hodinách) hello trvání intervalem pro uchovávání dat hello pro každý bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="93fef-112">In **Recovery point retention**, specify (in hours) hello duration of hello retention window for each recovery point.</span></span> <span data-ttu-id="93fef-113">Chráněné počítače může být obnovena tooany bodu v rámci časového období uchování.</span><span class="sxs-lookup"><span data-stu-id="93fef-113">Protected machines can be recovered tooany point within a retention window.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93fef-114">Pro úložiště replikované toopremium počítače je podporováno až too24 hodin uchování.</span><span class="sxs-lookup"><span data-stu-id="93fef-114">Up too24 hours of retention is supported for machines replicated toopremium storage.</span></span> <span data-ttu-id="93fef-115">Pro úložiště replikované toostandard počítače je podporováno až too72 hodin uchování.</span><span class="sxs-lookup"><span data-stu-id="93fef-115">Up too72 hours of retention is supported for machines replicated toostandard storage.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93fef-116">Zásada replikace pro navrácení služeb po obnovení se vytvoří automaticky.</span><span class="sxs-lookup"><span data-stu-id="93fef-116">A replication policy for failback is automatically created.</span></span>

7. <span data-ttu-id="93fef-117">V nastavení **Frekvence snímků konzistentní vzhledem k aplikacím** určete, jak často (v minutách) se mají vytvářet body obnovení obsahující snímky konzistentní vzhledem k aplikacím.</span><span class="sxs-lookup"><span data-stu-id="93fef-117">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points that contain application-consistent snapshots will be created.</span></span>

8. <span data-ttu-id="93fef-118">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="93fef-118">Click **OK**.</span></span> <span data-ttu-id="93fef-119">Hello zásad by měl být vytvořen v 30 sekund too60.</span><span class="sxs-lookup"><span data-stu-id="93fef-119">hello policy should be created in 30 too60 seconds.</span></span>

![Generování zásad replikace](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-a-configuration-server-with-a-replication-policy"></a><span data-ttu-id="93fef-121">Přidružení konfiguračního serveru k zásadě replikace</span><span class="sxs-lookup"><span data-stu-id="93fef-121">Associate a configuration server with a replication policy</span></span>
1. <span data-ttu-id="93fef-122">Zvolte toowhich zásady replikace hello chcete tooassociate hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="93fef-122">Choose hello replication policy toowhich you want tooassociate hello configuration server.</span></span>
2. <span data-ttu-id="93fef-123">Klikněte na **Přidružit**.</span><span class="sxs-lookup"><span data-stu-id="93fef-123">Click **Associate**.</span></span>
<span data-ttu-id="93fef-124">![Přidružení konfiguračního serveru](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)</span><span class="sxs-lookup"><span data-stu-id="93fef-124">![Associate configuration server](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)</span></span>

3. <span data-ttu-id="93fef-125">Vyberte konfigurační server hello hello seznamu serverů.</span><span class="sxs-lookup"><span data-stu-id="93fef-125">Select hello configuration server from hello list of servers.</span></span>
4. <span data-ttu-id="93fef-126">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="93fef-126">Click **OK**.</span></span> <span data-ttu-id="93fef-127">Hello konfigurační server by měl spojené v jedné tootwo minut.</span><span class="sxs-lookup"><span data-stu-id="93fef-127">hello configuration server should be associated in one tootwo minutes.</span></span>

![Přidružení konfiguračního serveru](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-a-replication-policy"></a><span data-ttu-id="93fef-129">Úprava zásady replikace</span><span class="sxs-lookup"><span data-stu-id="93fef-129">Edit a replication policy</span></span>
1. <span data-ttu-id="93fef-130">Vyberte zásadu replikace hello, pro které chcete tooedit nastavení replikace.</span><span class="sxs-lookup"><span data-stu-id="93fef-130">Choose hello replication policy for which you want tooedit replication settings.</span></span>
<span data-ttu-id="93fef-131">![Úprava zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)</span><span class="sxs-lookup"><span data-stu-id="93fef-131">![Edit replication policy](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)</span></span>

2. <span data-ttu-id="93fef-132">Klikněte na **Upravit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="93fef-132">Click **Edit Settings**.</span></span>
<span data-ttu-id="93fef-133">![Úprava nastavení zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)</span><span class="sxs-lookup"><span data-stu-id="93fef-133">![Edit replication policy settings](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)</span></span>

3. <span data-ttu-id="93fef-134">Změňte nastavení hello podle vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="93fef-134">Change hello settings based on your need.</span></span>
4. <span data-ttu-id="93fef-135">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="93fef-135">Click **Save**.</span></span> <span data-ttu-id="93fef-136">Hello zásad by měla být uložena ve dvou minut toofive, v závislosti na tom, kolik virtuálních počítačů používají tuto zásadu replikace.</span><span class="sxs-lookup"><span data-stu-id="93fef-136">hello policy should be saved in two toofive minutes, depending on how many VMs are using that replication policy.</span></span>

![Uložení zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-a-configuration-server-from-a-replication-policy"></a><span data-ttu-id="93fef-138">Zrušení přidružení konfiguračního serveru k zásadě replikace</span><span class="sxs-lookup"><span data-stu-id="93fef-138">Dissociate a configuration server from a replication policy</span></span>
1. <span data-ttu-id="93fef-139">Zvolte toowhich zásady replikace hello chcete tooassociate hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="93fef-139">Choose hello replication policy toowhich you want tooassociate hello configuration server.</span></span>
2. <span data-ttu-id="93fef-140">Klikněte na **Zrušit přidružení**.</span><span class="sxs-lookup"><span data-stu-id="93fef-140">Click **Dissociate**.</span></span>
3. <span data-ttu-id="93fef-141">Vyberte konfigurační server hello hello seznamu serverů.</span><span class="sxs-lookup"><span data-stu-id="93fef-141">Select hello configuration server from hello list of servers.</span></span>
4. <span data-ttu-id="93fef-142">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="93fef-142">Click **OK**.</span></span> <span data-ttu-id="93fef-143">v jedné tootwo minut by měl zrušit její přidružení Hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="93fef-143">hello configuration server should be dissociated in one tootwo minutes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93fef-144">Konfigurační server nemůže zrušit přidružení, pokud je alespoň jednu položku replikovaná pomocí zásad hello.</span><span class="sxs-lookup"><span data-stu-id="93fef-144">You cannot dissociate a configuration server if there is at least one replicated item using hello policy.</span></span> <span data-ttu-id="93fef-145">Zajistěte, aby nebyly nalezeny žádné replikované položky pomocí zásad hello před zrušíte přidružení hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="93fef-145">Make sure there are no replicated items using hello policy before you dissociate hello configuration server.</span></span>

## <a name="delete-a-replication-policy"></a><span data-ttu-id="93fef-146">Odstranění zásady replikace</span><span class="sxs-lookup"><span data-stu-id="93fef-146">Delete a replication policy</span></span>

1. <span data-ttu-id="93fef-147">Vyberte zásadu replikace hello, které chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="93fef-147">Choose hello replication policy that you want toodelete.</span></span>
2. <span data-ttu-id="93fef-148">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="93fef-148">Click **Delete**.</span></span> <span data-ttu-id="93fef-149">30 sekund too60 je potřeba odstranit zásady Hello.</span><span class="sxs-lookup"><span data-stu-id="93fef-149">hello policy should be deleted in 30 too60 seconds.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93fef-150">Zásady replikace nelze odstranit, pokud má alespoň jeden server přidružené tooit konfigurace.</span><span class="sxs-lookup"><span data-stu-id="93fef-150">You cannot delete a replication policy if it has at least one configuration server associated tooit.</span></span> <span data-ttu-id="93fef-151">Ujistěte se, že neexistují žádné replikované položky pomocí zásad hello a odstranění všech hello související konfigurační servery před odstraněním hello zásad.</span><span class="sxs-lookup"><span data-stu-id="93fef-151">Make sure there are no replicated items using hello policy and delete all hello associated configuration servers before you delete hello policy.</span></span>
