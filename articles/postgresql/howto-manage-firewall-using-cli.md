---
title: "aaaCreate a správě Azure databáze pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate a správě Azure databáze pro pravidla brány firewall PostgreSQL pomocí příkazového řádku Azure CLI."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 06e34c9e3996c2ec3df63191d794bc34d0ca0cf2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure
Pravidla brány firewall na úrovni serveru povolte správci toomanage přístup tooan Azure databáze PostgreSQL serveru z konkrétní IP adresu nebo rozsah IP adres. Pomocí vhodného rozhraní příkazového řádku Azure, můžete vytvořit, aktualizovat, odstranit, seznamu a zobrazit toomanage pravidla brány firewall serveru. Přehled Azure databáze pro PostgreSQL brány firewall, najdete v tématu [databáze Azure pro pravidla brány firewall serveru PostgreSQL](concepts-firewall-rules.md)

## <a name="prerequisites"></a>Požadavky
toostep prostřednictvím této jak tooguide, budete potřebovat:
- [Databáze Azure pro PostgreSQL server a databáze](quickstart-create-server-database-azure-cli.md)
- Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) příkazového řádku nástroje nebo použití hello prostředí cloudu Azure v prohlížeči hello.

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Konfigurace pravidel brány firewall pro databázi Azure pro PostgreSQL
Hello [az postgres pravidla brány firewall-](/cli/azure/postgres/server/firewall-rule) příkazy jsou použité tooconfigure pravidla brány firewall.

## <a name="list-firewall-rules"></a>Seznam pravidel brány firewall 
existující hello toolist pravidel brány firewall serveru na hello server spustit hello [seznamu pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#list) příkaz.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401
```
výstup Hello obsahuje seznam pravidel hello, pokud existuje, ve výchozím nastavení ve formátu JSON formátu. Můžete použít přepínač hello `--output table` pro přehlednějším tvaru tabulky jako výstup hello.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a>Vytvořte pravidlo brány firewall
toocreate nové pravidlo brány firewall na serveru hello, spuštěním hello [az postgres pravidla brány firewall-vytvořit](/cli/azure/postgres/server/firewall-rule#create) příkaz. 

Tento příklad umožňuje řadu všechny IP adresy tooaccess hello server **mypgserver 20170401.postgres.database.azure.com**
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
tooallow singulární tooaccess IP adresu, zadejte text hello stejnou adresu, protože hello počáteční IP a koncové IP adresy, jako v následujícím příkladu.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  
--server mypgserver-20170401 --name "AllowSingleIpAddress" --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Po úspěšné výstupu příkazu hello jsou uvedeny podrobnosti hello hello pravidla brány firewall, které jste vytvořili, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, výstup hello showserror text zprávy.

## <a name="update-firewall-rule"></a>Aktualizovat pravidla brány firewall 
Aktualizovat existující pravidlo brány firewall na serveru pomocí hello [aktualizace pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#update) příkaz. Zadejte název hello hello existující pravidla brány firewall jako vstup a hello počáteční IP adresy a koncové IP atributy tooupdate.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
Po úspěšné výstupu příkazu hello jsou uvedeny podrobnosti hello hello pravidla brány firewall, které jste aktualizovali, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, výstup hello showserror text zprávy.
> [!NOTE]
> Pokud pravidlo brány firewall hello neexistuje, získá vytvořit pomocí příkazu update hello.

## <a name="show-firewall-rule-details"></a>Zobrazit podrobnosti o pravidlech brány firewall
Můžete také zobrazit existující brány firewall hello pravidlo podrobnosti serveru spuštěním [zobrazit pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#show) příkaz.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
Po úspěšné výstupu příkazu hello jsou uvedeny podrobnosti hello hello pravidla brány firewall, která jste zadali, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, výstup hello showserror text zprávy.

## <a name="delete-firewall-rule"></a>Odstranit pravidlo brány firewall
toorevoke přístup pro rozsah IP ze serveru hello, odstraňte existující pravidlo brány firewall tak, že spustíte hello [odstranit az postgres pravidla brány firewall-](/cli/azure/postgres/server/firewall-rule#delete) příkaz. Zadejte název hello hello existující pravidla brány firewall.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
Po úspěšné neexistuje žádný výstup. Při selhání je vrácen text hello chybové zprávy.

## <a name="next-steps"></a>Další kroky
- Podobně můžete použít webový prohlížeč příliš[vytvořit a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí hello portálu Azure](howto-manage-firewall-using-portal.md)
- Lépe porozumět o [databáze Azure pro pravidla brány firewall serveru PostgreSQL](concepts-firewall-rules.md)
- Pomoc při připojování tooan Azure databáze PostgreSQL serveru najdete v tématu [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)
