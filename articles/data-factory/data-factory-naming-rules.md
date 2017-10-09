---
title: "aaaRules u názvů běžných entit služby Azure Data Factory | Microsoft Docs"
description: "Popisuje pravidla pojmenování entit služby Data Factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 98c5fc5fc932b72b65894afad438b4dc321c8aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---naming-rules"></a><span data-ttu-id="d1ce3-103">Azure Data Factory - pravidla po pojmenování</span><span class="sxs-lookup"><span data-stu-id="d1ce3-103">Azure Data Factory - naming rules</span></span>
<span data-ttu-id="d1ce3-104">Hello následující tabulka obsahuje pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-104">hello following table provides naming rules for Data Factory artifacts.</span></span>

| <span data-ttu-id="d1ce3-105">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="d1ce3-105">Name</span></span> | <span data-ttu-id="d1ce3-106">Jedinečnost názvu</span><span class="sxs-lookup"><span data-stu-id="d1ce3-106">Name Uniqueness</span></span> | <span data-ttu-id="d1ce3-107">Ověřovací kontroly</span><span class="sxs-lookup"><span data-stu-id="d1ce3-107">Validation Checks</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="d1ce3-108">Data Factory</span><span class="sxs-lookup"><span data-stu-id="d1ce3-108">Data Factory</span></span> |<span data-ttu-id="d1ce3-109">Jedinečná napříč Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-109">Unique across Microsoft Azure.</span></span> <span data-ttu-id="d1ce3-110">Názvy jsou velká a malá písmena, který je `MyDF` a `mydf` odkazovat toohello stejné služby data factory.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-110">Names are case-insensitive, that is, `MyDF` and `mydf` refer toohello same data factory.</span></span> |<ul><li><span data-ttu-id="d1ce3-111">Každý objekt pro vytváření dat je vázané tooexactly jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-111">Each data factory is tied tooexactly one Azure subscription.</span></span></li><li><span data-ttu-id="d1ce3-112">Názvy objektů musí začínat písmenem nebo číslicí a může obsahovat pouze písmena, číslice a pomlčky (-) znak hello.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-112">Object names must start with a letter or a number, and can contain only letters, numbers, and hello dash (-) character.</span></span></li><li><span data-ttu-id="d1ce3-113">Každý znak pomlčka (-) musí být okamžitě a následnou písmenem nebo číslem.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-113">Every dash (-) character must be immediately preceded and followed by a letter or a number.</span></span> <span data-ttu-id="d1ce3-114">Po sobě jdoucí pomlčky nejsou povolené v názvech kontejneru.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-114">Consecutive dashes are not permitted in container names.</span></span></li><li><span data-ttu-id="d1ce3-115">Název může být 3 až 63 znaků dlouhý.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-115">Name can be 3-63 characters long.</span></span></li></ul> |
| <span data-ttu-id="d1ce3-116">Propojených služeb/tabulek/kanálů</span><span class="sxs-lookup"><span data-stu-id="d1ce3-116">Linked Services/Tables/Pipelines</span></span> |<span data-ttu-id="d1ce3-117">Jedinečný s ve službě data factory.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-117">Unique with in a data factory.</span></span> <span data-ttu-id="d1ce3-118">Názvy jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-118">Names are case-insensitive.</span></span> |<ul><li><span data-ttu-id="d1ce3-119">Maximální počet znaků v názvu tabulky: 260.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-119">Maximum number of characters in a table name: 260.</span></span></li><li><span data-ttu-id="d1ce3-120">Názvy objektů musí začínat písmenem, číslo nebo podtržítko (_).</span><span class="sxs-lookup"><span data-stu-id="d1ce3-120">Object names must start with a letter, number, or an underscore (_).</span></span></li><li><span data-ttu-id="d1ce3-121">Nejsou povolené tyto znaky: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</span><span class="sxs-lookup"><span data-stu-id="d1ce3-121">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”, ”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |
| <span data-ttu-id="d1ce3-122">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="d1ce3-122">Resource Group</span></span> |<span data-ttu-id="d1ce3-123">Jedinečná napříč Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-123">Unique across Microsoft Azure.</span></span> <span data-ttu-id="d1ce3-124">Názvy jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-124">Names are case-insensitive.</span></span> |<ul><li><span data-ttu-id="d1ce3-125">Maximální počet znaků: 1 000.</span><span class="sxs-lookup"><span data-stu-id="d1ce3-125">Maximum number of characters: 1000.</span></span></li><li><span data-ttu-id="d1ce3-126">Název může obsahovat písmena, číslice a hello následující znaky: "-", "_",","a"."</span><span class="sxs-lookup"><span data-stu-id="d1ce3-126">Name can contain letters, digits, and hello following characters: “-”, “_”, “,” and “.”</span></span></li></ul> |

