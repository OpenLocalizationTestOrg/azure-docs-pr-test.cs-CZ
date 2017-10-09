---
title: "aaaCreate funkce v Azure aktivovány webhook Githubu | Microsoft Docs"
description: "Pomocí Azure Functions toocreate bez serveru funkci, která je vyvolat webhook Githubu."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
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
ms.openlocfilehash: 8ffcde82c9310d749159ed53eab113658e38a030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>Vytvoření funkce aktivované webhookem GitHubu

Zjistěte, jak toocreate funkci, která se aktivuje požadavkem HTTP webhooku s datovou část specifický pro GitHub.

![Webhook Githubu aktivuje funkce v hello portálu Azure](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Požadavky

+ Účet GitHubu s alespoň jedním projektem.
+ Předplatné Azure. Pokud ho nemáte, než začnete, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

Dál vytvořte funkci v nové funkce aplikace hello.

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a>Vytvoření funkce aktivované webhookem GitHubu

1. Rozšířit funkce aplikace a klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**. Pokud je to první funkce hello ve vaší aplikaci funkce, vyberte **vlastní funkce**. Zobrazí se hello kompletní sada šablon funkcí.

    ![Funkce Rychlé spuštění stránku hello portálu Azure](./media/functions-create-github-webhook-triggered-function/add-first-function.png)

2. Vyberte hello **WebHook Githubu** šablonu pro požadovaný jazyk. **Pojmenujte funkci** a pak vyberte **Vytvořit**.

     ![Vytvoření webhooku se aktivuje funkce Githubu v hello portálu Azure](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

3. V nové funkce, klikněte na **URL funkce <> / Get**, zkopírujte a uložte hello hodnoty. Hello samé pro **tajný klíč <> / získat Githubu**. Použijte tyto hodnoty tooconfigure hello webhooku v Githubu.

    ![Zkontrolujte kód funkce hello](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

Dále vytvoříte webhook ve vašem úložišti GitHub.

## <a name="configure-hello-webhook"></a>Konfigurace webhooku hello

1. V Githubu přejděte tooa úložiště, který vlastníte. Můžete také použít libovolné úložiště, které máte rozvětvené. Pokud potřebujete toofork úložiště, použijte <https://github.com/Azure-Samples/functions-quickstart>.

1. Klikněte na **Settings** (Nastavení), pak na **Webhooks** (Webhooky) a **Add webhook** (Přidat webhook).

    ![Přidání webhooku GitHubu](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. Použití nastavení uvedeného v tabulce hello a pak klikněte na **přidat webhooku**.

    ![Adresa URL webhooku hello sady a tajný klíč](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| Nastavení | Navrhovaná hodnota | Popis |
|---|---|---|
| **Datová část adresy URL** | Zkopírovaná hodnota | Použít hello hodnoty vrácené **URL funkce <> / Get**. |
| **Tajný kód**   | Zkopírovaná hodnota | Použít hello hodnoty vrácené **tajný klíč <> / získat Githubu**. |
| **Typ obsahu** | application/json | Funkce Hello očekává datové části JSON. |
| Aktivační události | Nechat mě vybrat jednotlivé události | Chceme jenom tootrigger na události komentář problém.  |
| | Komentář k problému |  |

Nyní, hello webhooku je funkce nakonfigurovaná tootrigger při přidání nové komentář problém.

## <a name="test-hello-function"></a>Testování funkce hello

1. V úložišti GitHub, otevřete hello **problémy** ve nové okno prohlížeče.

1. V novém okně hello, klikněte na tlačítko **nový problém**, zadejte název a potom klikněte na **odeslání nové problém**.

1. V hello problém, zadejte komentář a klikněte na tlačítko **komentář**.

    ![Přidejte komentář k problému Githubu.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. Přejděte zpět toohello portál a zobrazit protokoly hello. Měli byste vidět položku trasování s nový komentář textem hello.

     ![Zobrazit text komentáře hello v protokolech hello.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste funkci, která se spustí při přijetí požadavku z webhooku Githubu.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o aktivačních událostech webhooků najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).
