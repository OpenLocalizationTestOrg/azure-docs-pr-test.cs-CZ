---
title: "aaaCreate pracovní nebo školní identity v AAD pro Windows | Microsoft Docs"
description: "Zjistěte, jak toocreate pracovní nebo školní identity v Azure Active Directory toouse u virtuálních počítačů Windows."
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
ms.openlocfilehash: dd6e2381fd0aa503483aa786b36232e557729c4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-toouse-with-windows-vms"></a><span data-ttu-id="37a8d-103">Vytváření pracovního nebo školního identity v Azure Active Directory toouse s virtuálními počítači Windows</span><span class="sxs-lookup"><span data-stu-id="37a8d-103">Creating a Work or School identity in Azure Active Directory toouse with Windows VMs</span></span>
<span data-ttu-id="37a8d-104">Pokud jste vytvořili osobní účet Azure nebo osobní předplatné MSDN, a vytvořit hello účet Azure tootake hello kredity MSDN Azure – výhody, které jste použili *účtu Microsoft* identity toocreate ho.</span><span class="sxs-lookup"><span data-stu-id="37a8d-104">If you created a personal Azure account or have a personal MSDN subscription and created hello Azure account tootake advantage of hello MSDN Azure credits -- you used a *Microsoft account* identity toocreate it.</span></span> <span data-ttu-id="37a8d-105">Mnoho funkcí Azure – [šablony skupiny prostředků](../../azure-resource-manager/resource-group-overview.md) je jedním z příkladů – vyžadují pracovní nebo školní účet (identita spravované službou Azure Active Directory) toowork.</span><span class="sxs-lookup"><span data-stu-id="37a8d-105">Many great features of Azure -- [resource group templates](../../azure-resource-manager/resource-group-overview.md) is one example -- require a work or school account (an identity managed by Azure Active Directory) toowork.</span></span> <span data-ttu-id="37a8d-106">Můžete postupovat podle pokynů hello níže toocreate, že nový pracovní nebo školní účet, protože naštěstí jedním z nejlepší, co o svému osobnímu účtu Azure hello je, že se dodává s výchozí doménu služby Azure Active Directory, které můžete použít toocreate nový pracovní nebo školní účet, který můžete použít s Azure funkce, které je vyžadují.</span><span class="sxs-lookup"><span data-stu-id="37a8d-106">You can follow hello instructions below toocreate a new work or school account because fortunately, one of hello best things about your personal Azure account is that it comes with a default Azure Active Directory domain that you can use toocreate a new work or school account that you can use with Azure features that require it.</span></span>

<span data-ttu-id="37a8d-107">Ale nedávné změny Ujistěte se, je možné toomanage vaše předplatné s žádným typem účet Azure pomocí hello `azure login` interaktivní přihlášení metoda popsaná [zde](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="37a8d-107">However, recent changes make it possible toomanage your subscription with any type of Azure account using hello `azure login` interactive login method described [here](../../xplat-cli-connect.md).</span></span> <span data-ttu-id="37a8d-108">Můžete použít tento mechanismus, nebo můžete provést hello pokyny.</span><span class="sxs-lookup"><span data-stu-id="37a8d-108">You can either use that mechanism, or you can follow hello instructions that follow.</span></span> <span data-ttu-id="37a8d-109">Můžete také [vytvoření identity pracovní nebo školní v Azure Active Directory toouse s virtuální počítače s Linuxem](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="37a8d-109">You can also [create a work or school identity in Azure Active Directory toouse with Linux VMs](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

