---
title: "Průběžné nasazování pomocí Azure webové aplikace v systému Linux | Microsoft Docs"
description: "Jak nastavit průběžné nasazování ve webové aplikaci Azure v systému Linux."
keywords: "služby Azure app service, linux, operačních systémů, acr"
services: app-service
documentationcenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: f8f7d51003f8a55b7f51e8cc2cea838e8e5a6196
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="continuous-deployment-with-azure-web-app-on-linux"></a>Průběžné nasazování pomocí webové aplikace Azure v systému Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

V tomto kurzu nakonfigurujete průběžné nasazování pro bitovou kopii vlastní kontejner ze spravované [registru kontejner Azure](https://azure.microsoft.com/en-us/services/container-registry/) úložiště nebo [úložiště Docker Hub](https://hub.docker.com).

## <a name="step-1---sign-in-to-azure"></a>Krok 1 – znak v Azure

Přihlaste se k portálu Azure na http://portal.azure.com

## <a name="step-2---enable-container-continuous-deployment-feature"></a>Krok 2 – povolení kontejneru průběžné nasazování funkce

Můžete povolit pomocí funkce průběžné nasazování [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a spuštěním následujícího příkazu

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

V  **[portál Azure](https://portal.azure.com/)**, klikněte **služby App Service** možnost na levé straně stránky.

Klikněte na název vaší aplikace, který chcete provést konfiguraci úložiště Docker Hub průběžné nasazování pro.

V **nastavení aplikace**, přidat aplikaci názvem `DOCKER_ENABLE_CI` s hodnotou `true`.

![Vložit obrázek nastavení aplikace](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="step-3---prepare-webhook-url"></a>Krok 3 – Příprava adresa URL Webhooku

Můžete získat pomocí adresy URL Webhooku [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a spuštěním následujícího příkazu

```azurecli-interactive
az webapp deployment container -n sname1 -g rgname -e true --show-cd-url
``` 

Pro adresu URL Webhooku, musíte mít následující koncový bod: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Můžete získat vaše `publishingusername` a `publishingpwd` stažením webové aplikace profil publikování se pomocí portálu Azure.

![Vložit obrázek přidání webhooku 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="step-4---add-a-web-hook"></a>Krok 4 – přidání webové háku

### <a name="azure-container-registry"></a>Azure Container Registry

V okně portálu registru, klikněte na tlačítko **Webhooky**, kliknutím na vytvořit nový webhooku **přidat**. V **vytvoření webhooku** okno, zadejte název vaší webhooku. Pro identifikátor URI pro Webhook, budete muset zadat adresu URL získané z **krok 3**

Zajistěte, aby definujte rozsah jako úložiště obsahující bitové kopie kontejneru.

![Vložit obrázek webhooku](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

Při aktualizaci získá bitovou kopii, získat webové aplikace automaticky aktualizován s novou bitovou kopii.

### <a name="docker-hub"></a>Úložiště docker Hub

Na stránce úložiště Docker Hub klikněte na tlačítko **Webhooky**, pak **vytvoření WEBHOOKU A**.

![Vložit obrázek přidání webhooku 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

Pro adresu URL Webhooku, budete muset zadat adresu URL získané z **krok 3**

![Vložit obrázek přidání webhooku 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Při aktualizaci získá bitovou kopii, získat webové aplikace automaticky aktualizován s novou bitovou kopii.

## <a name="next-steps"></a>Další kroky
* [Co je Azure webové aplikace v systému Linux?](./app-service-linux-intro.md)
* [Kontejner Azure registru](https://azure.microsoft.com/en-us/services/container-registry/)
* [Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux](app-service-linux-using-nodejs-pm2.md)
* [Pomocí .NET Core v Azure webové aplikace v systému Linux](app-service-linux-using-dotnetcore.md)
* [Použití Ruby v Azure webové aplikace v systému Linux](app-service-linux-ruby-get-started.md)
* [Jak používat vlastní image Docker pro webové aplikace Azure v systému Linux](./app-service-linux-using-custom-docker-image.md)
* [Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](./app-service-linux-faq.md) 
* [Správa webové aplikace v systému Linux pomocí Azure CLI 2.0](./app-service-linux-cli.md)



