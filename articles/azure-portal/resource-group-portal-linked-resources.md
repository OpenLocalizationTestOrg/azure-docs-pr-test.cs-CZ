---
title: "Související a propojené prostředky v galerii dlaždice"
description: "Další informace o souvisejících a propojené prostředky, které jsou zobrazeny v galerii dlaždice portál Azure preview."
services: azure-portal
documentationcenter: 
author: adamabdelhamed
manager: wpickett
editor: 
ms.assetid: dba96d29-f518-49c8-bfd2-f14cecb44cbf
ms.service: azure-portal
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2015
ms.author: adamab
ms.openlocfilehash: efa7bce26c2c4c153b083e0e34d689a11d27dd16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="related-and-linked-resources-in-the-tile-gallery"></a>Související a propojené prostředky v galerii dlaždice
Galerie dlaždice umožňuje najít dlaždice pro určitý prostředek a přetáhněte je na vaše aktuální okno. Pomocí dlaždice galerie, můžete vytvořit zobrazení správy, které jsou rozmístěny prostředky. Zadaný prostředek související prostředky zahrnují všechny prostředky v jeho skupin prostředků a všechny prostředky, které odkazují na nebo z prostředku.

## <a name="linked-resources-in-resource-manager"></a>Propojené prostředky ve službě Správce prostředků
Propojení je funkce služby Správce prostředků.  Umožňuje deklarovat vztahy mezi prostředky i v případě, že nejsou umístěny ve stejné skupině prostředků. Propojení nemá žádný vliv na modul runtime vašich prostředků, bez dopadu na fakturaci a mít žádný vliv na přístup na základě rolí.  Je jednoduše mechanismus, který můžete použít k reprezentaci relace tak, aby prostředí nástroje jako dlaždice galerie může poskytnout bohaté správy.  Vaše nástroje můžete zkontrolovat odkazy, pomocí rozhraní API odkazů a poskytují i prostředí pro správu vlastní relaci. 

## <a name="how-do-i-link-my-resources"></a>Jak lze propojit Moje prostředky?
Když vytvoříte prostředky prostřednictvím portálu nebo pomocí nasazení šablonu prostřednictvím Azure PowerShell nebo rozhraní příkazového řádku Azure, propojení se vytvářejí automaticky pro některé závislé prostředky. Můžete také programově propojení prostředky pomocí [propojené prostředky REST API](/rest/api/resources/resourcelinks).

## <a name="next-steps"></a>Další kroky
* Pokud potřebujete úvodní informace o zápis šablony Resource Manageru, najdete v části [vytváření šablon](../azure-resource-manager/resource-group-authoring-templates.md).
* Bližší informace o práci s skupiny prostředků prostřednictvím portálu naleznete v tématu [použití portálu Azure ke správě prostředků Azure](../azure-resource-manager/resource-group-portal.md).

