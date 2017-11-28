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
# <a name="related-and-linked-resources-in-the-tile-gallery"></a><span data-ttu-id="59ebf-103">Související a propojené prostředky v galerii dlaždice</span><span class="sxs-lookup"><span data-stu-id="59ebf-103">Related and linked resources in the tile gallery</span></span>
<span data-ttu-id="59ebf-104">Galerie dlaždice umožňuje najít dlaždice pro určitý prostředek a přetáhněte je na vaše aktuální okno.</span><span class="sxs-lookup"><span data-stu-id="59ebf-104">The tile gallery enables you to find tiles for a particular resource and drag them onto your current blade.</span></span> <span data-ttu-id="59ebf-105">Pomocí dlaždice galerie, můžete vytvořit zobrazení správy, které jsou rozmístěny prostředky.</span><span class="sxs-lookup"><span data-stu-id="59ebf-105">Using the tile gallery, you can create management views that span resources.</span></span> <span data-ttu-id="59ebf-106">Zadaný prostředek související prostředky zahrnují všechny prostředky v jeho skupin prostředků a všechny prostředky, které odkazují na nebo z prostředku.</span><span class="sxs-lookup"><span data-stu-id="59ebf-106">For any specified resource, the related resources include all the resources in its resource group, and any resources that link to or from the resource.</span></span>

## <a name="linked-resources-in-resource-manager"></a><span data-ttu-id="59ebf-107">Propojené prostředky ve službě Správce prostředků</span><span class="sxs-lookup"><span data-stu-id="59ebf-107">Linked resources in Resource Manager</span></span>
<span data-ttu-id="59ebf-108">Propojení je funkce služby Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="59ebf-108">Linking is a feature of the Resource Manager.</span></span>  <span data-ttu-id="59ebf-109">Umožňuje deklarovat vztahy mezi prostředky i v případě, že nejsou umístěny ve stejné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="59ebf-109">It enables you to declare relationships between resources even if they do not reside in the same resource group.</span></span> <span data-ttu-id="59ebf-110">Propojení nemá žádný vliv na modul runtime vašich prostředků, bez dopadu na fakturaci a mít žádný vliv na přístup na základě rolí.</span><span class="sxs-lookup"><span data-stu-id="59ebf-110">Linking has no impact on the runtime of your resources, no impact on billing, and no impact on role-based access.</span></span>  <span data-ttu-id="59ebf-111">Je jednoduše mechanismus, který můžete použít k reprezentaci relace tak, aby prostředí nástroje jako dlaždice galerie může poskytnout bohaté správy.</span><span class="sxs-lookup"><span data-stu-id="59ebf-111">It's simply a mechanism you can use to represent relationships so that tools like the tile gallery can provide a rich management experience.</span></span>  <span data-ttu-id="59ebf-112">Vaše nástroje můžete zkontrolovat odkazy, pomocí rozhraní API odkazů a poskytují i prostředí pro správu vlastní relaci.</span><span class="sxs-lookup"><span data-stu-id="59ebf-112">Your tools can inspect the links using the links API and provide custom relationship management experiences as well.</span></span> 

## <a name="how-do-i-link-my-resources"></a><span data-ttu-id="59ebf-113">Jak lze propojit Moje prostředky?</span><span class="sxs-lookup"><span data-stu-id="59ebf-113">How do I link my resources?</span></span>
<span data-ttu-id="59ebf-114">Když vytvoříte prostředky prostřednictvím portálu nebo pomocí nasazení šablonu prostřednictvím Azure PowerShell nebo rozhraní příkazového řádku Azure, propojení se vytvářejí automaticky pro některé závislé prostředky.</span><span class="sxs-lookup"><span data-stu-id="59ebf-114">When you create resources through the portal or by deploying a template through Azure PowerShell or Azure CLI, links are automatically created for some dependent resources.</span></span> <span data-ttu-id="59ebf-115">Můžete také programově propojení prostředky pomocí [propojené prostředky REST API](/rest/api/resources/resourcelinks).</span><span class="sxs-lookup"><span data-stu-id="59ebf-115">You can also programmatically link resources by using the [Linked Resources REST API](/rest/api/resources/resourcelinks).</span></span>

## <a name="next-steps"></a><span data-ttu-id="59ebf-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59ebf-116">Next steps</span></span>
* <span data-ttu-id="59ebf-117">Pokud potřebujete úvodní informace o zápis šablony Resource Manageru, najdete v části [vytváření šablon](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="59ebf-117">If you need an introduction to writing Resource Manager templates, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="59ebf-118">Bližší informace o práci s skupiny prostředků prostřednictvím portálu naleznete v tématu [použití portálu Azure ke správě prostředků Azure](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="59ebf-118">To understand more about working with resource groups through the portal, see [Using the Azure portal to manage your Azure resources](../azure-resource-manager/resource-group-portal.md).</span></span>

