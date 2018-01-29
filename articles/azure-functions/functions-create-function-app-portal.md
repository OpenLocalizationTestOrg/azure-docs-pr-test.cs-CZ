---
title: "Vytvoření aplikace funkcí na webu Azure Portal | Dokumentace Microsoftu"
description: "Vytvořte novou aplikaci funkcí ve službě Azure App Service na portálu."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 4f9882ebc86b8119e6e683c7442ee3448a8d111b
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Vytvoření aplikace funkcí na webu Azure Portal

Azure Function Apps využívá infrastrukturu služby Azure App Service. Toto téma ukazuje, jak vytvořit aplikaci funkcí na webu Azure Portal. Aplikace funkcí je kontejner, který hostuje provádění jednotlivých funkcí. Když vytvoříte aplikaci funkcí v plánu hostování služby App Service, vaše aplikace funkcí může využívat všechny funkce služby App Service.

## <a name="create-a-function-app"></a>Vytvoření Function App

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Při vytváření aplikace funkcí zadejte platný **Název aplikace**, který může obsahovat pouze písmena, číslice a pomlčky. Podtržítko (**_**) není povolené znak.

Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena. Váš název účtu úložiště musí být jedinečný v rámci Azure. 

Po vytvoření aplikace funkcí můžete jednotlivé funkce vytvářet v jednom nebo několika různých jazycích. Funkce můžete vytvářet [pomocí portálu](functions-create-first-azure-function.md#create-function), [průběžného nasazování](functions-continuous-deployment.md) nebo prostřednictvím [nahrávání přes protokol FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Plány služby

Azure Functions má dva různé plány služby: plán Consumption a plán služby App Service. Plán Consumption automaticky přiděluje výpočetní výkon za běhu kódu, podle potřeby se škáluje na více instancí s ohledem na zatížení a pak se škáluje na méně instancí, když kód není spuštěný. Plán služby App Service poskytuje aplikaci funkcí přístup ke všem zařízením služby App Service. Plán služby musíte zvolit při vytváření aplikace funkcí a v současné době nejde změnit. Další informace najdete v tématu [Výběr plánu hostování služby Azure Functions](functions-scale.md).

Pokud v plánu služby App Service plánujete spouštět funkce JavaScriptu, měli byste zvolit plán s méně jádry. Další informace najdete v tématu [Referenční informace k JavaScriptu pro službu Functions](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Požadavky na účet úložiště

Při vytváření aplikace funkcí ve službě App Service musíte vytvořit nebo propojit účet služby Azure Storage pro obecné účely, který podporuje Blob, Queue a Table Storage. Služba Functions interně používá službu Storage pro operace, jako jsou správa triggerů a protokolování provádění funkcí. Některé účty úložiště nepodporují fronty a tabulky, například účty úložiště pouze objektů blob, Azure Storage úrovně Premium a účty úložiště pro obecné účely s replikací zónově redundantního úložiště. Tyto účty jsou při vytváření aplikace funkcí vyfiltrované z okna Účet úložiště.

>[!NOTE]
>Pokud použijete plán hostování Consumption, kód vaší funkce a vázané konfigurační soubory se ukládají ve službě Azure File Storage v hlavním účtu úložiště. Když odstraníte hlavní účet úložiště, tento obsah se odstraní a není možné ho obnovit.

Další informace o typech účtů úložiště najdete v tématu [Seznámení se službami Azure Storage](../storage/common/storage-introduction.md#introducing-the-azure-storage-services). 

## <a name="next-steps"></a>Další kroky

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



