---
title: "aaaCreate a správě Azure databáze MySQL pravidla brány firewall pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate a správě Azure databáze MySQL pravidla brány firewall pomocí příkazového řádku Azure CLI."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: b2f0b4ccf34ce88e3a5e72a64d3f8c78a5bc2a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-azure-cli"></a>Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure
Pravidla brány firewall na úrovni serveru povolte správci toomanage přístup tooan Azure databáze MySQL serveru z konkrétní IP adresu nebo rozsah IP adres. Pomocí vhodného rozhraní příkazového řádku Azure, můžete vytvořit, aktualizovat, odstranit, seznamu a zobrazit toomanage pravidla brány firewall serveru. Přehled informací o Azure databáze MySQL brány firewall, najdete v článku [databáze Azure pro pravidla brány firewall serveru MySQL](./concepts-firewall-rules.md)

## <a name="prerequisites"></a>Požadavky
* [Instalace rozhraní příkazového řádku Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)
* Nainstalovat Azure Python SDK pro PostgreSQL a MySQL služby
* Nainstalovat rozhraní příkazového řádku Azure součást hello služeb PostgreSQL a MySQL
* Vytvoření serveru Azure Database for MySQL

## <a name="firewall-rule-commands"></a>Pravidlo brány firewall příkazy:
Hello **az mysql pravidla brány firewall-** se používá příkaz z příkazového řádku Azure CLI toocreate, odstranit, seznam, zobrazit a aktualizovat pravidla brány firewall.

Příkazy:
- **vytvoření**: vytvoření pravidla brány firewall serveru Azure MySQL.
- **Odstranit**: odstranění pravidla brány firewall serveru Azure MySQL.
- **seznam** : seznam pravidla brány firewall serveru Azure MySQL hello.
- **Zobrazit** : Zobrazit podrobnosti hello serveru Azure MySQL pravidlo brány firewall.
- **Aktualizovat**: aktualizovat pravidlo brány firewall serveru Azure MySQL.

## <a name="login-tooazure-and-list-your-azure-database-for-mysql-servers"></a>Seznam vaší databázi Azure pro servery, MySQL a tooAzure přihlášení
Rozhraní příkazového řádku Azure bezpečně připojte s vaším účtem Azure. Použití hello **az přihlášení** příkaz toodo to.

1. Spusťte následující příkaz z příkazového řádku hello hello.
```azurecli
az login
```
Tento příkaz výstup toouse kódu v dalším kroku hello.

2. Použijte stránku hello webového prohlížeče tooopen [https://aka.ms/devicelogin](https://aka.ms/devicelogin) a zadejte kód hello.

3. Na příkazovém řádku hello Přihlaste se pomocí přihlašovacích údajů Azure.

4. Jakmile vaše přihlášení oprávnění, vytiskne se v konzole hello seznam předplatných. Zkopírujte id hello hello potřeby předplatné tooset hello aktuální předplatné toobe používá postoupíte.
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. Zobrazí seznam hello Azure databáze MySQL serverů pro vaše předplatné nebo skupině prostředků, pokud si nejste jistí názvů hello.

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   Všimněte si hello name – atribut v hello výpis, který bude použité toospecify, které server toowork MySQL na. V případě potřeby potvrďte hello podrobnosti pro tento server toousing hello atribut tooconfirm název není správný:

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Seznam pravidel brány Firewall na Azure databáze MySQL Server pro 
Pomocí hello název serveru a název skupiny prostředků hello, seznamu hello existující pravidla brány firewall serveru na hello server. Všimněte si, že atribut name server hello je uveden v hello **– server** přepínače a není hello **– název** přepínače.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
výstup Hello zobrazí seznam pravidel hello, pokud existuje, ve výchozím nastavení ve formátu JSON formátu. Můžete použít přepínač hello **– výstupní tabulku** pro přehlednějším tvaru tabulky jako výstup hello.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-firewall-rule-on-azure-database-for-mysql-server"></a>Vytvořte pravidlo brány Firewall pro Server databáze MySQL na Azure databáze
Pomocí hello Azure MySQL název serveru a název skupiny prostředků hello, vytvořte nové pravidlo brány firewall na serveru hello. Zadejte název pravidla hello hello počáteční IP adresa a koncová IP adresa pro pravidlo toocover hello tooallow přístup k rozsahu IP adres.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
Pro singulární IP adres toobe povolený přístup, zadejte text hello stejnou adresu, protože hello počáteční IP a koncové IP adresy, jako v následujícím příkladu.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
Po úspěšné zobrazí výstup příkazu hello seznam hello podrobnosti o hello pravidlo brány firewall, které jste vytvořili, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, výstup hello zobrazí text chybové zprávy.

## <a name="update-firewall-rule-on-azure-database-for-mysql-server"></a>Aktualizovat pravidla brány Firewall v databázi Azure pro server databáze MySQL 
Pomocí hello Azure MySQL název serveru a název skupiny prostředků hello, aktualizovat existující pravidlo brány firewall na serveru hello. Zadejte název hello hello existující pravidla brány firewall jako vstup a hello počáteční IP adresy a koncové IP atributy tooupdate.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Po úspěšné zobrazí výstup příkazu hello seznam hello podrobnosti hello pravidla brány firewall, které jste aktualizovali, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, výstup hello zobrazí text chybové zprávy.

> [!NOTE]
> Pokud pravidlo brány firewall hello neexistuje, vytvoří se pomocí příkazu update hello.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Zobrazit podrobnosti pravidla brány Firewall pro Server databáze MySQL na Azure databáze
Pomocí názvu serveru Azure MySQL hello a název skupiny prostředků hello, zobrazit podrobnosti pro pravidlo existující brány firewall ze hello ze serveru hello. Zadejte název existujícího pravidla firewallu hello hello jako vstup.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Po úspěšné zobrazí výstup příkazu hello seznam hello podrobnosti o hello pravidlo brány firewall, která jste zadali, ve výchozím nastavení ve formátu JSON. Pokud dojde k selhání, výstup hello zobrazí text chybové zprávy.

## <a name="delete-firewall-rule-on-azure-database-for-mysql-server"></a>Odstranit pravidlo brány Firewall databáze Azure pro Server databáze MySQL
Pomocí hello Azure MySQL název serveru a název skupiny prostředků hello, odeberte existující pravidlo brány firewall ze serveru hello. Zadejte název hello hello existující pravidla brány firewall.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Po úspěšné neexistuje žádný výstup. Při selhání bude vrácen text hello chybové zprávy.

## <a name="next-steps"></a>Další kroky
- Lépe porozumět o [databáze Azure pro pravidla brány firewall serveru MySQL](./concepts-firewall-rules.md)
- [Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí hello portálu Azure](./howto-manage-firewall-using-portal.md)
