---
title: "aaaCreate funkci, která spouští podle plánu v Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate funkce v Azure, který běží podle plánu, který definujete."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 793b06a65a154466dfd4c121bcc88082227cd597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Vytvoření funkce v Azure aktivované časovačem

Zjistěte, jak Azure Functions toocreate toouse funkci, která běží na základě plánu, který definujete.

![Vytvoření aplikace pro funkce v hello portálu Azure](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Požadavky

toocomplete v tomto kurzu:

+ Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

Dál vytvořte funkci v nové funkce aplikace hello.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Vytvoření funkce aktivované časovačem

1. Rozšířit funkce aplikace a klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**. Pokud je to první funkce hello ve vaší aplikaci funkce, vyberte **vlastní funkce**. Zobrazí se hello kompletní sada šablon funkcí.

    ![Funkce Rychlé spuštění stránku hello portálu Azure](./media/functions-create-scheduled-function/add-first-function.png)

2. Vyberte hello **TimerTrigger** šablonu pro požadovaný jazyk. Pak použijte hello nastavení uvedeného v tabulce hello:

    ![Vytvoření funkce časovače aktivuje v hello portálu Azure.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

    | Nastavení | Navrhovaná hodnota | Popis |
    |---|---|---|
    | **Pojmenujte svoji funkci** | TimerTriggerCSharp1 | Definuje název hello funkce spustí časovač. |
    | **[Plán](http://en.wikipedia.org/wiki/Cron#CRON_expression)** | 0 \*/1 \* \* \* \* | Šest pole [výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) která plánuje vaší funkce toorun každou minutu. |

2. Klikněte na možnost **Vytvořit**. Ve zvoleném jazyce se vytvoří funkce, která se bude spouštět každou minutu.

3. Zobrazení informací trasování zapsat toohello protokoly a zkontrolujte provádění.

    ![Funkce přihlášení prohlížeč hello portálu Azure.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Nyní můžete změnit plán hello funkce tak, aby běžel méně často, například jednou za hodinu. 

## <a name="update-hello-timer-schedule"></a>Plán aktualizace hello časovače

1. Rozbalte funkci a klikněte na **Integrace**. Toto je, kde můžete definovat vstup a výstup vazby pro funkce a také nastavit plán hello. 

2. V poli **Plán** zadejte novou hodnotu `0 0 */1 * * *` a potom klikněte na **Uložit**.  

![Funkce Aktualizovat plán časovače v hello portálu Azure.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Teď máte funkci, která se spouští jednou za hodinu. 

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste funkci, která se spouští na základě plánu.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o aktivačních časovačích najdete v tématu [Plánování spouštění kódu v Azure Functions](functions-bindings-timer.md).