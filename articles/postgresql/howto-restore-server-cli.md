---
title: "Jak tooback a obnovení serveru v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Zjistěte, jak hello tooback zálohu a obnovení serveru v databázi Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 0b9ed25e3e3a88dd5c7ffe2ae7c27f8eef9be710
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooback-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-hello-azure-cli"></a>Jak hello tooback zálohu a obnovení serveru v databázi Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure

Použijte databázi Azure pro PostgreSQL toorestore tooan databáze serveru starší data, která zahrnuje ze 7 dní too35.

## <a name="prerequisites"></a>Požadavky
toocomplete tento tooguide postupy, musíte:
- [Databáze Azure pro PostgreSQL server a databáze](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Je-li nainstalovat a používat rozhraní příkazového řádku Azure hello místně, tento jak tooguide vyžaduje, že používáte Azure CLI verze 2.0 nebo novější. Zadejte verzi hello tooconfirm, příkazového řádku Azure CLI hello `az --version`. tooinstall nebo aktualizace, najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="back-up-happens-automatically"></a>Zálohování se stane automaticky
Použijete-li databáze Azure pro PostgreSQL, služba hello databáze automaticky provede zálohování služby hello každých 5 minut. 

Pro úroveň Basic hello zálohy jsou k dispozici po dobu 7 dnů. Pro úroveň Standard hello zálohy jsou k dispozici pro 35 dní. Další informace najdete v tématu [databáze Azure pro PostgreSQL cenové úrovně](concepts-service-tiers.md).

S touto automatické zálohování funkcí můžete obnovit hello server a její databází tooan starší datum nebo bodu v čase.

## <a name="restore-a-database-tooa-previous-point-in-time-by-using-hello-azure-cli"></a>Obnovit do databáze tooa předchozího bodu v čase pomocí hello rozhraní příkazového řádku Azure
Použijte databázi Azure pro PostgreSQL toorestore hello server tooa předchozí bod v čase. Hello obnovit data jsou zkopírované tooa nový server a existující server hello je ponechán beze. Například pokud tabulku omylem, bude vynechána. v poledne dnes můžete obnovit toohello čas těsně před polednem. Potom můžete načíst hello chybějící tabulky a data z hello obnovit kopii hello serveru. 

server hello toorestore, hello pomocí rozhraní příkazového řádku Azure [obnovení serveru postgres az](/cli/azure/postgres/server#restore) příkaz.

### <a name="run-hello-restore-command"></a>Spusťte příkaz restore hello

server hello toorestore, hello příkazového řádku Azure CLI, zadejte následující příkaz hello:

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

Hello `az postgres server restore` příkaz vyžaduje hello následující parametry:
| Nastavení | Navrhovaná hodnota | Popis  |
| --- | --- | --- |
| skupiny prostředků |  myResourceGroup |  Skupinu prostředků, kde existuje hello zdrojového serveru.  |
| jméno | Obnovit mypgserver | Název Hello hello nový server, který je vytvořen pomocí příkazu restore hello. |
| obnovení bodu v čase | 2017-04-13T13:59:00Z | Vyberte bod v toorestore čas k. Toto datum a čas musí být v rámci hello zdrojového serveru zálohovat dobu uchování. Používejte formát hello ISO8601 data a času. Například můžete použít vlastní místní časové pásmo, jako například `2017-04-13T05:59:00-08:00`. Můžete taky hello UTC zulština formátu, například `2017-04-13T13:59:00Z`. |
| zdrojový server | mypgserver 20170401 | Hello název nebo ID hello zdrojový server toorestore z. |

Při obnovení serveru tooan dřívějšího bodu v čase se vytvoří nový server. původní server Hello a její databáze z hello zadané bodu v čase zkopírovaný toohello nový server.

hodnoty Hello umístění a cenovou úroveň pro zůstanou hello obnovit server hello stejné jako původní server hello. 

Hello `az postgres server restore` příkaz je synchronní. Po obnovení serveru hello, můžete ho znovu toorepeat hello proces pro jiný bod v čase. 

Po hello dokončení procesu obnovení, vyhledejte nový server hello a ověřte, že je hello data obnovit podle očekávání.

## <a name="next-steps"></a>Další kroky
[Knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)
