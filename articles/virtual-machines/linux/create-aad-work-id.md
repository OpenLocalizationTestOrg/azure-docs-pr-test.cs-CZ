---
title: "aaaCreate pracovní nebo školní identity v AAD pro Linux | Microsoft Docs"
description: "Zjistěte, jak toocreate pracovní nebo školní identity v Azure Active Directory toouse u virtuálních počítačů Linux."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: b0f86d77-c669-4aa1-a095-c2aa4d9105fe
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/23/2016
ms.author: rasquill
ms.openlocfilehash: 54c3d0b0e89fe1b2d6a9b58a46776fe446ed72b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-toouse-with-linux-vms"></a><span data-ttu-id="f3d66-103">Vytváření pracovního nebo školního identity v Azure Active Directory toouse s virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="f3d66-103">Creating a Work or School identity in Azure Active Directory toouse with Linux VMs</span></span>
<span data-ttu-id="f3d66-104">Pokud jste vytvořili osobní účet Azure nebo osobní předplatné MSDN, a vytvořit hello účet Azure tootake hello kredity MSDN Azure – výhody, které jste použili *účtu Microsoft* identity toocreate ho.</span><span class="sxs-lookup"><span data-stu-id="f3d66-104">If you created a personal Azure account or have a personal MSDN subscription and created hello Azure account tootake advantage of hello MSDN Azure credits -- you used a *Microsoft account* identity toocreate it.</span></span> <span data-ttu-id="f3d66-105">Mnoho funkcí Azure – [šablony skupiny prostředků](../../azure-resource-manager/resource-group-overview.md) je jedním z příkladů – vyžadují pracovní nebo školní účet (identita spravované službou Azure Active Directory) toowork.</span><span class="sxs-lookup"><span data-stu-id="f3d66-105">Many great features of Azure -- [resource group templates](../../azure-resource-manager/resource-group-overview.md) is one example -- require a work or school account (an identity managed by Azure Active Directory) toowork.</span></span> <span data-ttu-id="f3d66-106">Můžete postupovat podle pokynů hello níže toocreate, že nový pracovní nebo školní účet, protože naštěstí jedním z nejlepší, co o svému osobnímu účtu Azure hello je, že se dodává s výchozí doménu služby Azure Active Directory, které můžete použít toocreate nový pracovní nebo školní účet, který můžete použít s Azure funkce, které je vyžadují.</span><span class="sxs-lookup"><span data-stu-id="f3d66-106">You can follow hello instructions below toocreate a new work or school account because fortunately, one of hello best things about your personal Azure account is that it comes with a default Azure Active Directory domain that you can use toocreate a new work or school account that you can use with Azure features that require it.</span></span>

<span data-ttu-id="f3d66-107">Ale nedávné změny Ujistěte se, je možné toomanage vaše předplatné s žádným typem účet Azure pomocí hello `azure login` interaktivní přihlášení metoda popsaná [zde](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="f3d66-107">However, recent changes make it possible toomanage your subscription with any type of Azure account using hello `azure login` interactive login method described [here](../../xplat-cli-connect.md).</span></span> <span data-ttu-id="f3d66-108">Můžete použít tento mechanismus, nebo můžete provést hello pokyny.</span><span class="sxs-lookup"><span data-stu-id="f3d66-108">You can either use that mechanism, or you can follow hello instructions that follow.</span></span> <span data-ttu-id="f3d66-109">Můžete také [vytvoření identity pracovní nebo školní v Azure Active Directory toouse s virtuálními počítači Windows](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f3d66-109">You can also [create a work or school identity in Azure Active Directory toouse with Windows VMs](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

