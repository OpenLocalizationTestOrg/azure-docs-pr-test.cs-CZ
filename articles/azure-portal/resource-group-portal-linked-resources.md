---
title: "aaaRelated a propojené prostředky v hello dlaždici Galerie"
description: "Další informace o souvisejících a propojené prostředky, které jsou zobrazeny v galerii hello dlaždice portál Azure preview hello."
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
ms.openlocfilehash: c8f99be8e23dc9641ec3cd3169604b33a4b049f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="related-and-linked-resources-in-hello-tile-gallery"></a><span data-ttu-id="4bd86-103">Související a propojené prostředky v galerii dlaždice hello</span><span class="sxs-lookup"><span data-stu-id="4bd86-103">Related and linked resources in hello tile gallery</span></span>
<span data-ttu-id="4bd86-104">Galerie dlaždice Hello vám umožní toofind dlaždice pro určitý prostředek a přetáhněte je na vaše aktuální okno.</span><span class="sxs-lookup"><span data-stu-id="4bd86-104">hello tile gallery enables you toofind tiles for a particular resource and drag them onto your current blade.</span></span> <span data-ttu-id="4bd86-105">Pomocí dlaždice Galerie hello, můžete vytvořit zobrazení správy, které jsou rozmístěny prostředky.</span><span class="sxs-lookup"><span data-stu-id="4bd86-105">Using hello tile gallery, you can create management views that span resources.</span></span> <span data-ttu-id="4bd86-106">Pro zadaný prostředek hello související s prostředky zahrnují všechny prostředky hello v jeho skupin prostředků a všechny prostředky, které odkazují tooor z prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="4bd86-106">For any specified resource, hello related resources include all hello resources in its resource group, and any resources that link tooor from hello resource.</span></span>

## <a name="linked-resources-in-resource-manager"></a><span data-ttu-id="4bd86-107">Propojené prostředky ve službě Správce prostředků</span><span class="sxs-lookup"><span data-stu-id="4bd86-107">Linked resources in Resource Manager</span></span>
<span data-ttu-id="4bd86-108">Propojení je funkce hello Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4bd86-108">Linking is a feature of hello Resource Manager.</span></span>  <span data-ttu-id="4bd86-109">Umožní vám toodeclare vztahy mezi prostředky i v případě, že nejsou umístěny v hello stejné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="4bd86-109">It enables you toodeclare relationships between resources even if they do not reside in hello same resource group.</span></span> <span data-ttu-id="4bd86-110">Propojení nemá žádný vliv na hello runtime vaše prostředky, bez dopadu na fakturaci a mít žádný vliv na přístup na základě rolí.</span><span class="sxs-lookup"><span data-stu-id="4bd86-110">Linking has no impact on hello runtime of your resources, no impact on billing, and no impact on role-based access.</span></span>  <span data-ttu-id="4bd86-111">Je jednoduše mechanismus toorepresent vztahů můžete použít tak, aby prostředí nástroje jako hello dlaždice galerie může poskytnout bohaté správy.</span><span class="sxs-lookup"><span data-stu-id="4bd86-111">It's simply a mechanism you can use toorepresent relationships so that tools like hello tile gallery can provide a rich management experience.</span></span>  <span data-ttu-id="4bd86-112">Vaše nástroje můžete zkontrolovat odkazy hello pomocí odkazů hello rozhraní API a poskytují i prostředí pro správu vlastní relaci.</span><span class="sxs-lookup"><span data-stu-id="4bd86-112">Your tools can inspect hello links using hello links API and provide custom relationship management experiences as well.</span></span> 

## <a name="how-do-i-link-my-resources"></a><span data-ttu-id="4bd86-113">Jak lze propojit Moje prostředky?</span><span class="sxs-lookup"><span data-stu-id="4bd86-113">How do I link my resources?</span></span>
<span data-ttu-id="4bd86-114">Když vytvoříte prostředky prostřednictvím portálu hello nebo nasazením šablonu prostřednictvím Azure PowerShell nebo rozhraní příkazového řádku Azure, propojení se vytvářejí automaticky pro některé závislé prostředky.</span><span class="sxs-lookup"><span data-stu-id="4bd86-114">When you create resources through hello portal or by deploying a template through Azure PowerShell or Azure CLI, links are automatically created for some dependent resources.</span></span> <span data-ttu-id="4bd86-115">Můžete také programově propojit prostředky pomocí hello [propojené prostředky REST API](/rest/api/resources/resourcelinks).</span><span class="sxs-lookup"><span data-stu-id="4bd86-115">You can also programmatically link resources by using hello [Linked Resources REST API](/rest/api/resources/resourcelinks).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4bd86-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4bd86-116">Next steps</span></span>
* <span data-ttu-id="4bd86-117">Pokud budete potřebovat šablon Úvod toowriting Resource Manageru, najdete v části [vytváření šablon](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="4bd86-117">If you need an introduction toowriting Resource Manager templates, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="4bd86-118">najdete v části Další informace o práce se skupinami prostředků prostřednictvím portálu hello toounderstand [pomocí hello Azure portálu toomanage vašich prostředků Azure](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4bd86-118">toounderstand more about working with resource groups through hello portal, see [Using hello Azure portal toomanage your Azure resources](../azure-resource-manager/resource-group-portal.md).</span></span>

