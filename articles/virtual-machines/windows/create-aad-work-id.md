---
title: "Vytvoření identity pracovní nebo školní v AAD pro Windows | Microsoft Docs"
description: "Naučte se vytvářet pracovní nebo školní identity v Azure Active Directory pro použití s virtuální počítače s Windows."
services: virtual-machines-windows
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d07dca34-618a-48aa-9971-03d9c1210f4a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/23/2016
ms.author: rasquill
ms.openlocfilehash: 7694b959a384aaed213adc31e02debca31b7c131
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-windows-vms"></a><span data-ttu-id="50841-103">Vytváření pracovního nebo školního identity v Azure Active Directory pro použití s virtuálními počítači Windows</span><span class="sxs-lookup"><span data-stu-id="50841-103">Creating a Work or School identity in Azure Active Directory to use with Windows VMs</span></span>
<span data-ttu-id="50841-104">Pokud jste vytvořili osobní účet Azure nebo osobní předplatné MSDN, a vytvořit účet Azure využívat výhod kredity MSDN Azure –, které jste použili *účtu Microsoft* identity k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="50841-104">If you created a personal Azure account or have a personal MSDN subscription and created the Azure account to take advantage of the MSDN Azure credits -- you used a *Microsoft account* identity to create it.</span></span> <span data-ttu-id="50841-105">Mnoho funkcí Azure – [šablony skupiny prostředků](../../azure-resource-manager/resource-group-overview.md) je jedním z příkladů – pro práci se vyžaduje pracovní nebo školní účet (identita spravované službou Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="50841-105">Many great features of Azure -- [resource group templates](../../azure-resource-manager/resource-group-overview.md) is one example -- require a work or school account (an identity managed by Azure Active Directory) to work.</span></span> <span data-ttu-id="50841-106">Můžete postupovat podle pokynů níže vytvořit nový pracovní nebo školní účet, protože jeden z nejlepší, co o svůj osobní účet Azure je naštěstí se dodává s výchozí doménu služby Azure Active Directory, který můžete použít k vytvoření nové pracovní nebo školní účet t Hat, které můžete použít s Azure funkce, které je vyžadují.</span><span class="sxs-lookup"><span data-stu-id="50841-106">You can follow the instructions below to create a new work or school account because fortunately, one of the best things about your personal Azure account is that it comes with a default Azure Active Directory domain that you can use to create a new work or school account that you can use with Azure features that require it.</span></span>

<span data-ttu-id="50841-107">Ale nedávné změny umožňují spravovat předplatné s žádným typem účet Azure pomocí `azure login` interaktivní přihlášení metoda popsaná [zde](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="50841-107">However, recent changes make it possible to manage your subscription with any type of Azure account using the `azure login` interactive login method described [here](../../xplat-cli-connect.md).</span></span> <span data-ttu-id="50841-108">Buď můžete použít tento mechanismus, nebo můžete postupovat podle pokynů, které následují.</span><span class="sxs-lookup"><span data-stu-id="50841-108">You can either use that mechanism, or you can follow the instructions that follow.</span></span> <span data-ttu-id="50841-109">Můžete také [vytvoření identity pracovní nebo školní v Azure Active Directory pro použití s virtuální počítače s Linuxem](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="50841-109">You can also [create a work or school identity in Azure Active Directory to use with Linux VMs](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

