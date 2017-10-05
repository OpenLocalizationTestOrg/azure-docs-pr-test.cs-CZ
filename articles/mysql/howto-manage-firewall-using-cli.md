---
title: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje postup vytvoření a správě Azure databáze MySQL pravidla brány firewall pomocí příkazového řádku Azure CLI."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 9a03722e9f71be307bdbf0b846a4cbf7b34cd7ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-azure-cli"></a>Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure
Pravidla brány firewall na úrovni serveru umožňují správcům řídit přístup k databázi Azure pro Server databáze MySQL z konkrétní IP adresu nebo rozsah IP adres. Pomocí vhodného rozhraní příkazového řádku Azure, můžete vytvořit, aktualizovat, odstranit, seznamu a zobrazit pravidla brány firewall ke správě serveru. Přehled informací o Azure databáze MySQL brány firewall, najdete v článku [databáze Azure pro pravidla brány firewall serveru MySQL](./concepts-firewall-rules.md)

## <a name="prerequisites"></a>Požadavky
* [Instalace rozhraní příkazového řádku Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)
* Nainstalovat Azure Python SDK pro PostgreSQL a MySQL služby
* Nainstalovat součást příkazového řádku Azure CLI pro PostgreSQL a databáze MySQL služby
* Vytvoření serveru Azure Database for MySQL

## <a name="firewall-rule-commands"></a>Pravidlo brány firewall příkazy:
**Az mysql pravidla brány firewall-** příkaz z příkazového řádku Azure slouží k vytvoření, odstranění, seznam, zobrazit a aktualizovat pravidla brány firewall.

Příkazy:
- **vytvoření**: vytvoření pravidla brány firewall serveru Azure MySQL.
- **Odstranit**: odstranění pravidla brány firewall serveru Azure MySQL.
- **seznam** : seznam pravidla brány firewall serveru Azure MySQL.
- **Zobrazit** : Zobrazit podrobnosti o serveru databáze MySQL Azure pravidlo brány firewall.
- **Aktualizovat**: aktualizovat pravidlo brány firewall serveru Azure MySQL.

## <a name="login-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Přihlášení k Azure a seznam Azure databáze MySQL servery
Rozhraní příkazového řádku Azure bezpečně připojte s vaším účtem Azure. Použití **az přihlášení** příkaz k tomu.

1. V příkazovém řádku spusťte následující příkaz.
```azurecli
az login
```
Tento příkaz na výstupu zobrazí kód, který použijete v dalším kroku.

2. Ve webovém prohlížeči otevřete stránku [https://aka.ms/devicelogin](https://aka.ms/devicelogin) a zadejte kód.

3. V příkazovém řádku se přihlaste pomocí vašich přihlašovacích údajů Azure.

4. Jakmile vaše přihlášení oprávnění, vytiskne se v konzole seznam předplatných. Zkopírujte id požadované předplatné nastavte aktuální předplatné, které má být použit přesunutí dál.
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. Zobrazí seznam Azure databáze MySQL serverů pro vaše předplatné nebo skupině prostředků, pokud si nejste jistí názvy.

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   Poznámka: v seznamu, který se použije k určení které server MySQL pro práci na atribut názvu. V případě potřeby potvrďte podrobnosti pro tento server k použití a ověřte, zda je název správný atribut názvu:

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Seznam pravidel brány Firewall na Azure databáze MySQL Server pro 
Pomocí názvu serveru a název skupiny prostředků, seznamu existující pravidla brány firewall serveru na serveru. Všimněte si, že je atribut název serveru zadaný v **– server** přepínače a ne **– název** přepínače.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
Výstup se zobrazí seznam pravidel, pokud existuje, ve výchozím nastavení ve formátu JSON. Můžete použít přepínač **– výstupní tabulku** pro přehlednějším tvaru tabulky jako výstup.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-firewall-rule-on-azure-database-for-mysql-server"></a>Vytvořte pravidlo brány Firewall pro Server databáze MySQL na Azure databáze
Pomocí Azure MySQL název serveru a název skupiny prostředků, vytvořte nové pravidlo brány firewall na serveru. Zadejte název pro toto pravidlo, počáteční IP adresa a koncové IP adresu pro pravidlo tak, aby pokrývalo rozsah IP adres pro povolení přístupu.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
Pro singulární IP adresu, bude přístup povolen zadejte stejnou adresu jako počáteční IP a koncové IP adresy, jako v následujícím příkladu.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
Po úspěšné zobrazí seznam výstupu příkazu podrobnosti o pravidlo brány firewall, které jste vytvořili, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, výstup zobrazí text chybové zprávy.

## <a name="update-firewall-rule-on-azure-database-for-mysql-server"></a>Aktualizovat pravidla brány Firewall v databázi Azure pro server databáze MySQL 
Pomocí Azure MySQL název serveru a název skupiny prostředků, aktualizujte existující pravidlo brány firewall na serveru. Zadejte název existující pravidla brány firewall jako vstup a spusťte IP adresy a koncové IP atributů k aktualizaci.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Po úspěšné zobrazí seznam výstupu příkazu podrobnosti o pravidlo brány firewall, které jste aktualizovali, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, výstup zobrazí text chybové zprávy.

> [!NOTE]
> Pokud pravidlo brány firewall neexistuje, vytvoří se pomocí příkazu update.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Zobrazit podrobnosti pravidla brány Firewall pro Server databáze MySQL na Azure databáze
Pomocí Azure MySQL název serveru a název skupiny prostředků, zobrazit existující brány firewall pravidla podrobnosti ze serveru. Zadejte název existující pravidla brány firewall jako vstup.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Po úspěšné zobrazí seznam výstupu příkazu podrobnosti o pravidlo brány firewall, které jste zadali, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, výstup zobrazí text chybové zprávy.

## <a name="delete-firewall-rule-on-azure-database-for-mysql-server"></a>Odstranit pravidlo brány Firewall databáze Azure pro Server databáze MySQL
Pomocí Azure MySQL název serveru a název skupiny prostředků, odeberte existující pravidlo brány firewall ze serveru. Zadejte název existující pravidlo brány firewall.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Po úspěšné neexistuje žádný výstup. Při selhání bude vrácen text chybové zprávy.

## <a name="next-steps"></a>Další kroky
- Lépe porozumět o [databáze Azure pro pravidla brány firewall serveru MySQL](./concepts-firewall-rules.md)
- [Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí portálu Azure](./howto-manage-firewall-using-portal.md)
