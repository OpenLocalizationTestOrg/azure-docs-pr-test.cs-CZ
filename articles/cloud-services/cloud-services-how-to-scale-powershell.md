---
title: "aaaScale Azure cloud service v prostředí Windows PowerShell | Microsoft Docs"
description: "(klasické) Zjistěte, jak toouse prostředí PowerShell tooscale webovou roli nebo role pracovního procesu nebo zmenšit v Azure."
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: ee37dd8c-6714-4c61-adb8-03d6bbf76c9a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: mmccrory
ms.openlocfilehash: cfac6660e84f8ae24e4e9bdd5bf2016fb9cd7045
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-a-cloud-service-in-powershell"></a><span data-ttu-id="54c80-103">Jak služba tooscale cloudu v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="54c80-103">How tooscale a cloud service in PowerShell</span></span>

<span data-ttu-id="54c80-104">Tooscale prostředí Windows PowerShell můžete použít webovou roli nebo role pracovního procesu příchozí nebo odchozí přidáním nebo odebráním instancí.</span><span class="sxs-lookup"><span data-stu-id="54c80-104">You can use Windows PowerShell tooscale a web role or worker role in or out by adding or removing instances.</span></span>  

## <a name="log-in-tooazure"></a><span data-ttu-id="54c80-105">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="54c80-105">Log in tooAzure</span></span>

<span data-ttu-id="54c80-106">Před provedením jakékoli operace na vaše předplatné pomocí prostředí PowerShell, musíte být přihlášení:</span><span class="sxs-lookup"><span data-stu-id="54c80-106">Before you can perform any operations on your subscription through PowerShell, you must log in:</span></span>

```powershell
Add-AzureAccount
```

<span data-ttu-id="54c80-107">Pokud máte více předplatných, které jsou spojené s vaším účtem, musíte toochange hello aktuálního předplatného v závislosti na tom, kde se nachází cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="54c80-107">If you have multiple subscriptions associated with your account, you may need toochange hello current subscription depending on where your cloud service resides.</span></span> <span data-ttu-id="54c80-108">toocheck hello aktuální předplatné, spusťte:</span><span class="sxs-lookup"><span data-stu-id="54c80-108">toocheck hello current subscription, run:</span></span>

```powershell
Get-AzureSubscription -Current
```

<span data-ttu-id="54c80-109">Pokud potřebujete toochange hello aktuální předplatné, spusťte:</span><span class="sxs-lookup"><span data-stu-id="54c80-109">If you need toochange hello current subscription, run:</span></span>

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-hello-current-instance-count-for-your-role"></a><span data-ttu-id="54c80-110">Zkontrolujte hello aktuální počet instancí pro vaši roli</span><span class="sxs-lookup"><span data-stu-id="54c80-110">Check hello current instance count for your role</span></span>

<span data-ttu-id="54c80-111">toocheck hello aktuální stav role, spusťte:</span><span class="sxs-lookup"><span data-stu-id="54c80-111">toocheck hello current state of your role, run:</span></span>

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

<span data-ttu-id="54c80-112">Informace o hello role, včetně jeho aktuální počet verze a instance operačního systému by měla vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="54c80-112">You should get back information about hello role, including its current OS version and instance count.</span></span> <span data-ttu-id="54c80-113">V takovém případě má hello role jediné instance.</span><span class="sxs-lookup"><span data-stu-id="54c80-113">In this case, hello role has a single instance.</span></span>

![Informace o roli hello](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-hello-role-by-adding-more-instances"></a><span data-ttu-id="54c80-115">Škálování hello role přidáním více instancí</span><span class="sxs-lookup"><span data-stu-id="54c80-115">Scale out hello role by adding more instances</span></span>

<span data-ttu-id="54c80-116">tooscale se vaše role průchodu hello požadovaného počtu instancí jako hello **počet** parametr toohello **Set-AzureRole** rutiny:</span><span class="sxs-lookup"><span data-stu-id="54c80-116">tooscale out your role, pass hello desired number of instances as hello **Count** parameter toohello **Set-AzureRole** cmdlet:</span></span>

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

<span data-ttu-id="54c80-117">bloky rutiny Hello na okamžik při hello nové instance jsou zřízený a spuštěna.</span><span class="sxs-lookup"><span data-stu-id="54c80-117">hello cmdlet blocks momentarily while hello new instances are provisioned and started.</span></span> <span data-ttu-id="54c80-118">Během této doby, otevřete nové okno prostředí PowerShell a volání **Get-AzureRole** jako je uvedené výše, zobrazí se počet instancí cílové nové hello.</span><span class="sxs-lookup"><span data-stu-id="54c80-118">During this time, if you open a new PowerShell window and call **Get-AzureRole** as shown earlier, you will see hello new target instance count.</span></span> <span data-ttu-id="54c80-119">A je-li si prohlédnout stav role hello hello portálu, měli byste vidět hello novou instanci spuštění:</span><span class="sxs-lookup"><span data-stu-id="54c80-119">And if you inspect hello role status in hello portal, you should see hello new instance starting up:</span></span>

![Instance virtuálního počítače od portálu](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

<span data-ttu-id="54c80-121">Jakmile nové instance hello spustili, vrátí rutina hello úspěšně:</span><span class="sxs-lookup"><span data-stu-id="54c80-121">Once hello new instances have started, hello cmdlet will return successfully:</span></span>

![Úspěch zvýšení instance role](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-hello-role-by-removing-instances"></a><span data-ttu-id="54c80-123">Škálování role hello odebráním instancí</span><span class="sxs-lookup"><span data-stu-id="54c80-123">Scale in hello role by removing instances</span></span>

<span data-ttu-id="54c80-124">Je možné škálovat v roli odebráním instancí v hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="54c80-124">You can scale in a role by removing instances in hello same way.</span></span> <span data-ttu-id="54c80-125">Sada hello **počet** parametr na **Set-AzureRole** toohello počet instancí chcete toohave po dokončení škálování hello v operaci.</span><span class="sxs-lookup"><span data-stu-id="54c80-125">Set hello **Count** parameter on **Set-AzureRole** toohello number of instances you want toohave after hello scale in operation is complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54c80-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54c80-126">Next steps</span></span>

<span data-ttu-id="54c80-127">Není možné tooconfigure automatickému škálování pro cloudové služby z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="54c80-127">It is not possible tooconfigure auto-scale for cloud services from PowerShell.</span></span> <span data-ttu-id="54c80-128">toodo, který najdete v části [jak tooauto škálování cloudové služby](cloud-services-how-to-scale-portal.md).</span><span class="sxs-lookup"><span data-stu-id="54c80-128">toodo that, see [How tooauto scale a cloud service](cloud-services-how-to-scale-portal.md).</span></span>
