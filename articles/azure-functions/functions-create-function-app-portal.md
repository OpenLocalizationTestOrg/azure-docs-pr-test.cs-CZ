---
title: "aaaCreate funkce aplikace z hello portálu Azure | Microsoft Docs"
description: "Vytvořte novou aplikaci funkce ve službě Azure App Service z portálu hello."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
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
ms.openlocfilehash: c531fc71c798edf22e25a5f4b79c15413809dc86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-from-hello-azure-portal"></a>Vytvoření funkce aplikace hello portálu Azure

Aplikace Azure funkce používá infrastrukturu Azure App Service hello. Toto téma ukazuje, jak toocreate funkce aplikace v hello portálu Azure. Funkce aplikace je hello kontejner, který je hostitelem hello provádění jednotlivých funkcí. Když vytvoříte aplikaci funkce v hello hostování plán služby App Service, funkce aplikace můžete používat všechny funkce hello služby App Service.

## <a name="create-a-function-app"></a>Vytvoření Function App

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Když vytvoříte aplikaci function app, zadejte platný **název aplikace**, který může obsahovat pouze písmena, číslice a pomlčky. Podtržítko (**_**) není povolené znak.

Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena. Váš název účtu úložiště musí být jedinečný v rámci Azure. 

Po vytvoření aplikace hello funkce vytvořením jednotlivých funkcí na jeden nebo více různých jazyků. Vytvoření funkce [pomocí portálu hello](functions-create-first-azure-function.md#create-function), [průběžné nasazování](functions-continuous-deployment.md), nebo pomocí [odesílání spolu s FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Plány služeb

Azure Functions nabízí dva druhy různých služeb: plánování využívání a plán služby App Service. plánu spotřeby Hello automaticky přiděluje výpočetní výkon, pokud kód je spuštěný, měřítka na více systémů jako nezbytné toohandle zatížení a potom měřítka – v kódu není. Hello plán služby App Service poskytuje funkce aplikace přístup tooall hello zařízení služby App Service. Když je vytvořena funkce aplikace a nelze ji změnit, musíte zvolit plán služby. Další informace najdete v tématu [zvolte Azure Functions hostování plán](functions-scale.md).

Pokud plánujete toorun funkce jazyka JavaScript na plán služby App Service, měli byste vybrat plán s menším počtem jader. Další informace najdete v tématu hello [referenční dokumentace technologie JavaScript pro funkce](functions-reference-node.md#choose-single-core-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Požadavky na účet úložiště

Když vytváříte aplikaci funkce ve službě App Service, musíte vytvořit nebo připojit tooa pro obecné účely Azure účet úložiště, který podporuje úložiště Blob, fronty a tabulky. Funkce interně používá úložiště pro operace, jako je například Správa aktivační události a protokolování spuštěních funkce. Některé účty úložiště nepodporují fronty a tabulky, jako je například účty pouze objekt blob úložiště Azure Premium Storage a účty úložiště pro obecné účely replikaci ZRS. Tyto účty jsou filtrovány mimo v okně účtu úložiště hello při vytváření aplikaci funkce.

>[!NOTE]
>Při použití hello spotřeba hostingový plán, funkce kód a vazba konfiguračních souborů jsou uložené v Azure File storage v účtu úložiště hlavní hello. Pokud odstraníte účet úložiště hlavní hello, tento obsah se odstraní a nelze jej obnovit.

toolearn Další informace o typy účtů úložiště, najdete v části [Představení služby úložiště Azure hello](../storage/common/storage-introduction.md#introducing-the-azure-storage-services). 

## <a name="next-steps"></a>Další kroky

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



