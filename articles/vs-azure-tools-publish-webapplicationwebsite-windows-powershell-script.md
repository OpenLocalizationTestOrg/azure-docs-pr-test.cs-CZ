---
title: "aaaPublish-WebApplicationWebSite (skript prostředí Windows PowerShell) | Microsoft Docs"
description: "Zjistěte, jak toopublish webového projektu tooan webu Azure. Tento skript vytvoří hello požadované prostředky ve vašem předplatném Azure, pokud ještě neexistují."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d46904e30e3c2e040e57888fa31543e8e366527f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publikování-WebApplicationWebSite (skript prostředí Windows PowerShell)
## <a name="syntax"></a>Syntaxe
Publikuje webového projektu tooan webu Azure. skript Hello hello požadované prostředky vytvoří ve vašem předplatném Azure, pokud ještě neexistují.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfigurace
Hello cesta toohello JSON konfigurační soubor, který popisuje podrobnosti hello hello nasazení.

| Parametr | Výchozí hodnota |
| --- | --- |
| Aliasy |None |
| Povinné? |Hodnota TRUE |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="subscriptionname"></a>Název_předplatného
Název Hello hello předplatného Azure, které chcete toocreate hello webu v.

| Parametr | Výchozí hodnota |
| --- | --- |
| Aliasy |None |
| Povinné? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="webdeploypackage"></a>WebDeployPackage
Hello cesta toohello webové nasazení balíčku toopublish toohello webu. Tento balíček můžete vytvořit pomocí Průvodce publikování webu hello v sadě Visual Studio. Další informace najdete v tématu [Začínáme s Azure Cloud Services a technologií ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parametr | Výchozí hodnota |
| --- | --- |
| Aliasy |None |
| Povinné? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
Hello uživatelské jméno a heslo pro hello databázi SQL v Azure.

| Parametr | Výchozí hodnota |
| --- | --- |
| Aliasy |None |
| Povinné? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
V případě hodnoty true tiskové zprávy z hello skriptu toohello výstupního datového proudu.

| Parametr | Výchozí hodnota |
| --- | --- |
| Aliasy |None |
| Povinné? |False |
| Pozice |S názvem |
| Výchozí hodnota |False |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="remarks"></a>Poznámky
Podrobné informace o tom, jak toouse hello skriptu toocreate vývojářů a testovací prostředí, najdete v části [tooDev tooPublish pomocí skriptů Windows PowerShell a testovací prostředí](vs-azure-tools-publishing-using-powershell-scripts.md).

konfigurační soubor JSON Hello Určuje podrobnosti hello co je toobe nasazení. Obsahuje hello informace, které jste zadali při vytváření projektu hello, jako je například hello název a uživatelské jméno pro web hello. Zahrnuje také hello tooprovision databáze, pokud existuje. Hello následující kód ukazuje konfigurační soubor JSON příklad:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Můžete upravit hello JSON konfigurační soubor toochange co je nasazen. Části webu není požadována, ale část databáze hello je volitelný.

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [Publish-WebApplicationVM (skript prostředí Windows PowerShell)](vs-azure-tools-publish-webapplicationvm.md)

