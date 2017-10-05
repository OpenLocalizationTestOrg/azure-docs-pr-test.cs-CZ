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
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-windows-vms"></a>Vytváření pracovního nebo školního identity v Azure Active Directory pro použití s virtuálními počítači Windows
Pokud jste vytvořili osobní účet Azure nebo osobní předplatné MSDN, a vytvořit účet Azure využívat výhod kredity MSDN Azure –, které jste použili *účtu Microsoft* identity k jeho vytvoření. Mnoho funkcí Azure – [šablony skupiny prostředků](../../azure-resource-manager/resource-group-overview.md) je jedním z příkladů – pro práci se vyžaduje pracovní nebo školní účet (identita spravované službou Azure Active Directory). Můžete postupovat podle pokynů níže vytvořit nový pracovní nebo školní účet, protože jeden z nejlepší, co o svůj osobní účet Azure je naštěstí se dodává s výchozí doménu služby Azure Active Directory, který můžete použít k vytvoření nové pracovní nebo školní účet t Hat, které můžete použít s Azure funkce, které je vyžadují.

Ale nedávné změny umožňují spravovat předplatné s žádným typem účet Azure pomocí `azure login` interaktivní přihlášení metoda popsaná [zde](../../xplat-cli-connect.md). Buď můžete použít tento mechanismus, nebo můžete postupovat podle pokynů, které následují. Můžete také [vytvoření identity pracovní nebo školní v Azure Active Directory pro použití s virtuální počítače s Linuxem](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

