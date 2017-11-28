---
title: aaaHow toomigrate logiku aplikace tooschema verze 2015-08-01-preview | Microsoft Docs
description: "Můžete snadno migrovat vaší toohello nejnovější verzi schématu logic apps. Postupujte podle těchto kroků."
services: logic-apps
documentationcenter: 
author: MSFTMAN
manager: erikre
editor: 
tags: connectors
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: deonhe
ms.openlocfilehash: c7b42aaec547eddd28b0c649a3c0625047f9f805
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-logic-apps-tooschema-version-2015-08-01-preview"></a><span data-ttu-id="8e109-104">Jak toomigrate logiku aplikace tooschema verze 2015-08-01-preview</span><span class="sxs-lookup"><span data-stu-id="8e109-104">How toomigrate logic apps tooschema version 2015-08-01-preview</span></span>
<span data-ttu-id="8e109-105">toomove existující logiku aplikace toohello nové schéma, hello následující:</span><span class="sxs-lookup"><span data-stu-id="8e109-105">toomove your existing logic apps toohello new schema, do hello following:</span></span>  

1. <span data-ttu-id="8e109-106">Otevřete aplikaci logiky v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8e109-106">Open your logic app in hello Azure portal</span></span>  
2. <span data-ttu-id="8e109-107">Klikněte na tlačítko Aktualizovat schéma:</span><span class="sxs-lookup"><span data-stu-id="8e109-107">Click Update Schema:</span></span>
   
   <span data-ttu-id="8e109-108">![Ikona rozhraní API][step1] </span><span class="sxs-lookup"><span data-stu-id="8e109-108">![API Icon][step1] </span></span>  
   <span data-ttu-id="8e109-109">stránce aktualizovat schéma Hello zobrazí a poskytuje odkaz tooa dokument obsahující informace o vylepšení hello v novém schématu hello: ![ikona rozhraní API][step2]</span><span class="sxs-lookup"><span data-stu-id="8e109-109">hello Update Schema page displays and provides a link tooa document that provide details on hello improvements in hello new schema: ![API Icon][step2]</span></span>

> [!NOTE]
> <span data-ttu-id="8e109-110">Když vyberete **aktualizovat schéma**, můžeme automaticky spustit kroky migrace hello a poskytnout vám výstupní kód hello.</span><span class="sxs-lookup"><span data-stu-id="8e109-110">When you select **Update Schema**, we automatically run hello migration steps and provide hello code output for you.</span></span> <span data-ttu-id="8e109-111">Můžete použít tento tooupdate vaší definice, ale, ujistěte se, postupujte drželi osvědčených kódovacích postupů jsou uvedeny níže v hello **osvědčené postupy** části níže.</span><span class="sxs-lookup"><span data-stu-id="8e109-111">You can use this tooupdate your definition, however, ensure you follow good coding practices such as those outlined in hello **Best practices** section below.</span></span>
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-toohello-latest-schema-version"></a><span data-ttu-id="8e109-112">Osvědčené postupy při migraci vaší toohello nejnovější verzi schématu Logic apps:</span><span class="sxs-lookup"><span data-stu-id="8e109-112">Best practices when migrating your Logic apps toohello latest schema version:</span></span>
* <span data-ttu-id="8e109-113">Kopírování hello migrovat skriptu tooa nové logiku aplikace – přepsání hello staré jednu až po dokončení testování a potvrzené hello migrované aplikace funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="8e109-113">Copy hello migrated script tooa new Logic App - don't overwrite hello old one until you've completed your testing and confirmed hello migrated app works as expected.</span></span>
* <span data-ttu-id="8e109-114">Svou aplikaci logiky si otestujte ještě **předtím**, než ji předáte do produkce.</span><span class="sxs-lookup"><span data-stu-id="8e109-114">Test your Logic app **before** putting in production</span></span>
* <span data-ttu-id="8e109-115">Po dokončení migrace spusťte aktualizaci vašeho logiku aplikace toouse hello [spravované rozhraní API](apis-list.md) kde je to možné.</span><span class="sxs-lookup"><span data-stu-id="8e109-115">After migration completes, start updating your Logic apps toouse hello [managed APIs](apis-list.md) where possible.</span></span> <span data-ttu-id="8e109-116">Například můžete začít používat Dropbox v2 všude, kde jste používali DropBox v1.</span><span class="sxs-lookup"><span data-stu-id="8e109-116">For example, you can start using Dropbox v2, whereever you are using DropBox v1.</span></span>

## <a name="whats-next"></a><span data-ttu-id="8e109-117">Kam dál</span><span class="sxs-lookup"><span data-stu-id="8e109-117">What's next</span></span>
* [<span data-ttu-id="8e109-118">Zjistěte, jak migrovat toomanually Logic apps</span><span class="sxs-lookup"><span data-stu-id="8e109-118">Learn how toomanually migrate your Logic apps</span></span>](../logic-apps/logic-apps-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






