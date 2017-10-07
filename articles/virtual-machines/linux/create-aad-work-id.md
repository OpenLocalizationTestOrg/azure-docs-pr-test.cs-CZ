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
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-toouse-with-linux-vms"></a>Vytváření pracovního nebo školního identity v Azure Active Directory toouse s virtuální počítače s Linuxem
Pokud jste vytvořili osobní účet Azure nebo osobní předplatné MSDN, a vytvořit hello účet Azure tootake hello kredity MSDN Azure – výhody, které jste použili *účtu Microsoft* identity toocreate ho. Mnoho funkcí Azure – [šablony skupiny prostředků](../../azure-resource-manager/resource-group-overview.md) je jedním z příkladů – vyžadují pracovní nebo školní účet (identita spravované službou Azure Active Directory) toowork. Můžete postupovat podle pokynů hello níže toocreate, že nový pracovní nebo školní účet, protože naštěstí jedním z nejlepší, co o svému osobnímu účtu Azure hello je, že se dodává s výchozí doménu služby Azure Active Directory, které můžete použít toocreate nový pracovní nebo školní účet, který můžete použít s Azure funkce, které je vyžadují.

Ale nedávné změny Ujistěte se, je možné toomanage vaše předplatné s žádným typem účet Azure pomocí hello `azure login` interaktivní přihlášení metoda popsaná [zde](../../xplat-cli-connect.md). Můžete použít tento mechanismus, nebo můžete provést hello pokyny. Můžete také [vytvoření identity pracovní nebo školní v Azure Active Directory toouse s virtuálními počítači Windows](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

