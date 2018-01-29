---
title: "Vytvoření funkce aktivované webhookem GitHubu v Azure | Dokumentace Microsoftu"
description: "Pomocí služby Azure Functions vytvoříte funkci bez serveru, která je vyvolána webhookem GitHubu."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: cdfb5db7b304a18d6945328abc0ca7ebf2f9ec6a
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>Vytvoření funkce aktivované webhookem GitHubu

Naučte se vytvořit funkci, která se aktivuje požadavkem webhooku HTTP s datovou částí specifickou pro GitHub.

![Funkce aktivovaná webhookem GitHubu na webu Azure Portal](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Požadavky

+ Účet GitHubu s alespoň jedním projektem.
+ Předplatné Azure. Pokud ho nemáte, než začnete, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

Dál vytvoříte v nové aplikaci Function App funkci.

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a>Vytvoření funkce aktivované webhookem GitHubu

1. Rozbalte aplikaci Function App a klikněte na tlačítko **+** vedle položky **Funkce**. Pokud jde o první funkci ve vaší aplikaci Function App, vyberte možnost **Vlastní funkce**. Zobrazí se kompletní sada šablon funkcí.

    ![Stručný úvod do služby Functions na webu Azure Portal](./media/functions-create-github-webhook-triggered-function/add-first-function.png)

2. Do vyhledávacího pole zadejte `github` a zvolte jazyk požadovaný pro šablonu funkce aktivované webhookem GitHubu. 

     ![Výběr šablony funkce aktivované webhookem GitHubu](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

2. Zadejte **Název** funkce a pak vyberte **Vytvořit**. 

     ![Konfigurace funkce aktivované webhookem GitHubu na webu Azure Portal](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger-2.png) 

3. V nové funkci klikněte na **</> Získat adresu URL funkce** a potom zkopírujte a uložte příslušné hodnoty. To samé udělejte s možností **</> Získat tajný klíč GitHubu**. Tyto hodnoty použijete ke konfiguraci webhooku na GitHubu.

    ![Kontrola kódu funkce](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

Dále vytvoříte webhook ve vašem úložišti GitHub.

## <a name="configure-the-webhook"></a>Konfigurace webhooku

1. V GitHubu přejděte do úložiště, které vlastníte. Můžete také použít libovolné úložiště, které máte rozvětvené. Pokud potřebujete úložiště rozvětvit, použijte stránku <https://github.com/Azure-Samples/functions-quickstart>.

1. Klikněte na **Settings** (Nastavení), pak na **Webhooks** (Webhooky) a **Add webhook** (Přidat webhook).

    ![Přidání webhooku GitHubu](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. Použijte nastavení uvedené v tabulce a potom klikněte na **Přidat webhook**.

    ![Nastavení adresy URL a tajného klíče webhooku](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| Nastavení | Navrhovaná hodnota | Popis |
|---|---|---|
| **Datová část adresy URL** | Zkopírovaná hodnota | Použijte hodnotu vrácenou příkazem **</> Získat adresu URL funkce**. |
| **Tajný kód**   | Zkopírovaná hodnota | Použijte hodnotu vrácenou příkazem **</> Získat tajný kód GitHubu**. |
| **Typ obsahu** | application/json | Funkce očekává datovou část JSON. |
| Aktivační události | Nechat mě vybrat jednotlivé události | Chceme provést aktivaci jenom při událostech komentářů k problémům.  |
| | Komentář k problému |  |

Teď je webhook nakonfigurován tak, aby aktivoval vaši funkci v případě, že dojde k přidání nového komentáře k problému.

## <a name="test-the-function"></a>Testování funkce

1. Ve vašem úložišti GitHub otevřete kartu **Issues** (Problémy) v novém okně prohlížeče.

1. V novém okně klikněte na **New Issue** (Nový problém), zadejte název a klikněte na **Submit new issue** (Odeslat nový problém).

1. U problému zadejte komentář a klikněte na tlačítko **Comment** (Komentář).

    ![Přidejte komentář k problému Githubu.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. Vraťte se na portál a podívejte se do protokolů. Měli byste vidět položku trasování s textem nového komentáře.

     ![Zobrazte si text komentáře v protokolech.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste funkci, která se spustí při přijetí požadavku z webhooku Githubu.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o aktivačních událostech webhooků najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).
