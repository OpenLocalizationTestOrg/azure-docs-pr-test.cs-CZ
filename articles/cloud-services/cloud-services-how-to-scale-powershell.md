---
title: "Škálování cloudové služby Azure v prostředí Windows PowerShell | Microsoft Docs"
description: "(klasické) Zjistěte, jak pomocí prostředí PowerShell škálování webovou roli nebo role pracovního procesu příchozí nebo odchozí v Azure."
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
ms.openlocfilehash: a7ae8ff202d403dff19b8c9a6a09492235db27ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-scale-a-cloud-service-in-powershell"></a><span data-ttu-id="93995-103">Postup škálování cloudové služby v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="93995-103">How to scale a cloud service in PowerShell</span></span>

<span data-ttu-id="93995-104">Prostředí Windows PowerShell můžete škálovat webovou roli nebo role pracovního procesu příchozí nebo odchozí přidáním nebo odebráním instancí.</span><span class="sxs-lookup"><span data-stu-id="93995-104">You can use Windows PowerShell to scale a web role or worker role in or out by adding or removing instances.</span></span>  

## <a name="log-in-to-azure"></a><span data-ttu-id="93995-105">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="93995-105">Log in to Azure</span></span>

<span data-ttu-id="93995-106">Před provedením jakékoli operace na vaše předplatné pomocí prostředí PowerShell, musíte být přihlášení:</span><span class="sxs-lookup"><span data-stu-id="93995-106">Before you can perform any operations on your subscription through PowerShell, you must log in:</span></span>

```powershell
Add-AzureAccount
```

<span data-ttu-id="93995-107">Pokud máte více předplatných, které jsou spojené s vaším účtem, musíte změnit aktuální předplatné v závislosti na tom, kde se nachází cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="93995-107">If you have multiple subscriptions associated with your account, you may need to change the current subscription depending on where your cloud service resides.</span></span> <span data-ttu-id="93995-108">Pokud chcete zkontrolovat aktuální předplatné, spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="93995-108">To check the current subscription, run:</span></span>

```powershell
Get-AzureSubscription -Current
```

<span data-ttu-id="93995-109">Pokud potřebujete změnit aktuální předplatné, spusťte:</span><span class="sxs-lookup"><span data-stu-id="93995-109">If you need to change the current subscription, run:</span></span>

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-the-current-instance-count-for-your-role"></a><span data-ttu-id="93995-110">Zkontrolujte aktuální počet instancí pro vaši roli</span><span class="sxs-lookup"><span data-stu-id="93995-110">Check the current instance count for your role</span></span>

<span data-ttu-id="93995-111">Pokud chcete zkontrolovat aktuální stav role, spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="93995-111">To check the current state of your role, run:</span></span>

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

<span data-ttu-id="93995-112">Informace o roli, včetně jeho aktuální počet verze a instance operačního systému by měla vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="93995-112">You should get back information about the role, including its current OS version and instance count.</span></span> <span data-ttu-id="93995-113">V takovém případě má roli jediné instance.</span><span class="sxs-lookup"><span data-stu-id="93995-113">In this case, the role has a single instance.</span></span>

![Informace o roli](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-the-role-by-adding-more-instances"></a><span data-ttu-id="93995-115">Škálování role přidáním více instancí</span><span class="sxs-lookup"><span data-stu-id="93995-115">Scale out the role by adding more instances</span></span>

<span data-ttu-id="93995-116">Chcete-li škálovat vaše role, předejte požadovaný počet instancí jako **počet** parametru **Set-AzureRole** rutiny:</span><span class="sxs-lookup"><span data-stu-id="93995-116">To scale out your role, pass the desired number of instances as the **Count** parameter to the **Set-AzureRole** cmdlet:</span></span>

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

<span data-ttu-id="93995-117">Rutiny bloky na okamžik při nové instance jsou zřízený a spuštěna.</span><span class="sxs-lookup"><span data-stu-id="93995-117">The cmdlet blocks momentarily while the new instances are provisioned and started.</span></span> <span data-ttu-id="93995-118">Během této doby, otevřete nové okno prostředí PowerShell a volání **Get-AzureRole** jako je uvedené výše, zobrazí se nové cílový počet instancí.</span><span class="sxs-lookup"><span data-stu-id="93995-118">During this time, if you open a new PowerShell window and call **Get-AzureRole** as shown earlier, you will see the new target instance count.</span></span> <span data-ttu-id="93995-119">A je-li si prohlédnout stav rolí na portálu, měli byste vidět novou instanci spuštění:</span><span class="sxs-lookup"><span data-stu-id="93995-119">And if you inspect the role status in the portal, you should see the new instance starting up:</span></span>

![Instance virtuálního počítače od portálu](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

<span data-ttu-id="93995-121">Jakmile nové instance spustili, rutina vrátí úspěšně:</span><span class="sxs-lookup"><span data-stu-id="93995-121">Once the new instances have started, the cmdlet will return successfully:</span></span>

![Úspěch zvýšení instance role](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-the-role-by-removing-instances"></a><span data-ttu-id="93995-123">Škálování v roli odebráním instancí</span><span class="sxs-lookup"><span data-stu-id="93995-123">Scale in the role by removing instances</span></span>

<span data-ttu-id="93995-124">Je možné škálovat v roli odebráním instancí stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="93995-124">You can scale in a role by removing instances in the same way.</span></span> <span data-ttu-id="93995-125">Nastavte **počet** parametr na **Set-AzureRole** na počet instancí, které chcete mít po dokončení měřítka v operaci.</span><span class="sxs-lookup"><span data-stu-id="93995-125">Set the **Count** parameter on **Set-AzureRole** to the number of instances you want to have after the scale in operation is complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93995-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93995-126">Next steps</span></span>

<span data-ttu-id="93995-127">Není možné nakonfigurovat automatické škálování pro cloudové služby z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="93995-127">It is not possible to configure auto-scale for cloud services from PowerShell.</span></span> <span data-ttu-id="93995-128">To lze provést, najdete v části [postup automatické škálování cloudové služby](cloud-services-how-to-scale-portal.md).</span><span class="sxs-lookup"><span data-stu-id="93995-128">To do that, see [How to auto scale a cloud service](cloud-services-how-to-scale-portal.md).</span></span>
