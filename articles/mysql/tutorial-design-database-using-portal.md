---
title: "aaaDesign vaše první Azure databáze pro databázi MySQL. portálu Azure | Microsoft Docs"
description: "Tento kurz vysvětluje, jak toocreate a správě Azure databáze MySQL serveru a databáze pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/06/2017
ms.custom: mvc
ms.openlocfilehash: 06dd952acc5356b8cccaf36917df1ff8db4f7139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Navrhnout první databáze Azure pro databázi MySQL
Azure databáze pro databázi MySQL je spravovaná služba, která vám umožní toorun, spravovat a škálování vysoce dostupné databáze MySQL v cloudu hello. Pomocí hello portálu Azure, můžete snadno spravovat váš server a návrhu databáze.

V tomto kurzu použijete hello Azure portálu toolearn jak na:

> [!div class="checklist"]
> * Vytvoření Azure databáze pro databázi MySQL
> * Konfigurace brány firewall serveru hello
> * Použijte nástroj příkazového řádku toocreate mysql databáze
> * Načíst ukázková data
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

## <a name="sign-in-toohello-azure-portal"></a>Přihlaste se toohello portálu Azure
Otevřete Oblíbené webový prohlížeč a přejděte hello [portálu Microsoft Azure](https://portal.azure.com/). Zadejte vaše přihlašovací údaje toosign toohello portálu. Výchozí zobrazení Hello je váš řídicí panel služby.

## <a name="create-an-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Server Azure Database for MySQL se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](./concepts-compute-unit-and-storage.md). Hello server je vytvořen v rámci [skupina prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).

1. Přejděte příliš**databáze** > **Azure Database pro databázi MySQL**. Pokud nemůžete najít MySQL serveru v části **databáze** kategorii, klikněte na tlačítko **zobrazit všechny** tooshow všechny dostupné databáze služby. Můžete také zadat **Azure Database pro databázi MySQL** v tooquickly pole hledání hello najít službu hello.
![Navigovat tooMySQL 2-1](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. Klikněte na tlačítko **Azure Database pro databázi MySQL** dlaždici a potom klikněte na **vytvořit**.

V našem příkladu vyplňte hello databáze Azure pro formulář MySQL s hello následující informace:

| **Nastavení** | **Navrhovaná hodnota** | **Popis pole** |
|---|---|---|
| *Název serveru* | myserver4demo  | Název serveru má toobe globálně jedinečný. |
| *Předplatné* | mysubscription | Vyberte své předplatné z rozevíracího seznamu hello. |
| *Skupina prostředků* | myresourcegroup | Vytvořte skupinu prostředků nebo použijte existující. |
| *Přihlašovací jméno správce serveru* | myadmin | Instalační program název účtu správce. |
| *Heslo* |  | Nastavte silné heslo účtu správce. |
| *Potvrdit heslo* |  | Potvrďte heslo pro účet správce hello. |
| *Umístění* |  | Vyberte dostupnou oblast. |
| *Verze* | 5.7 | Vyberte nejnovější verzi hello. |
| *Konfigurovat výkon* | Základní, 50 výpočetní jednotky, 50 GB  | Zvolte **Cenovou úroveň**, **Výpočetní jednotky**, **Úložiště (GB)** a potom klikněte na **OK**. |
| *TooDashboard PIN kód* | Zaškrtnout | Doporučená toocheck toto políčko, a může se stát hello server později snadno na |
Poté klikněte na možnost **Vytvořit**. V minutu nebo dvě nové databáze Azure pro MySQL server běží v cloudu hello. Můžete kliknout na **oznámení** tlačítko v procesu nasazení hello nástrojů toomonitor hello.

## <a name="configure-firewall"></a>Konfigurace brány firewall
Azure databáze pro databázi MySQL jsou chráněné bránou firewall. Ve výchozím nastavení všechna připojení toohello server hello databáze a uvnitř hello server odmítají. Před připojením tooAzure databáze pro databázi MySQL hello poprvé, nakonfigurujte veřejnou síť IP adresu hello brány firewall tooadd hello klientské počítače (nebo rozsah IP adres).

1. Klikněte na nově vytvořený serveru a pak klikněte na tlačítko **zabezpečení připojení**.
   ![Zabezpečení připojení 3-1](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. Můžete **přidat Moje IP**, nebo můžete nakonfigurovat pravidla brány firewall v tomto poli. Mějte na paměti, tooclick **Uložit** po vytvoření pravidla hello.
Teď se můžete připojit server toohello pomocí nástroje příkazového řádku mysql nebo MySQL Workbench grafickým uživatelským rozhraním.

> [!TIP]
> Azure databáze MySQL server pro komunikuje přes port 3306. Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 3306 nemusí mít povolený bránou firewall vaší sítě. Pokud ano, pokud vaše IT oddělení otevře port 3306 nemůžete připojit tooAzure MySQL server.

## <a name="get-connection-information"></a>Získání informací o připojení
Plně kvalifikovaný hello Get **název serveru** a **přihlašovací jméno správce pro Server** pro vaši databázi Azure pro server databáze MySQL z hello portálu Azure. Použijete hello plně kvalifikovaný název tooconnect tooyour serveru pomocí nástroje příkazového řádku mysql. 

1. V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **všechny prostředky** z nabídky na levé straně hello, název typu hello a vyhledávání pro vaši databázi Azure pro server databáze MySQL. Vyberte hello serveru název tooview hello podrobnosti.

2. V části hello nastavení klikněte na **vlastnosti**. Poznamenejte si **název serveru** a **PŘIHLAŠOVACÍ jméno pro SERVER správce**. Kliknutí na tlačítko hello kopírování tlačítko Další tooeach pole toocopy toohello schránky.
   ![Vlastnosti serveru 4-2](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

V tomto příkladu je název serveru hello *myserver4demo.mysql.database.azure.com*, a je přihlašovací jméno správce serveru hello  *myadmin@myserver4demo* .

## <a name="connect-toohello-server-using-mysql"></a>Připojení serveru toohello pomocí mysql
Použití [nástroj příkazového řádku mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) tooestablish tooyour připojení databáze Azure pro server databáze MySQL. Nástroj příkazového řádku mysql hello můžete spustit z hello prostředí cloudu Azure v prohlížeči hello nebo vlastní počítače pomocí nástroje mysql nainstalované místně. toolaunch hello prostředí cloudu Azure, klikněte na tlačítko hello `Try It` tlačítko blok kódu v tomto článku, nebo navštívit hello portál Azure a klikněte na hello `>_` ikonu v horním pravém panelu nástrojů hello. 

Zadejte příkaz tooconnect hello:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a>Vytvoření prázdné databáze
Jakmile jste server připojený toohello, vytvořte prázdnou databázi toowork s.
```sql
CREATE DATABASE mysampledb;
```

V řádku hello spusťte následující příkaz tooswitch připojení toothis nově vytvořený databáze hello:
```sql
USE mysampledb;
```

## <a name="create-tables-in-hello-database"></a>Vytváření tabulek v databázi hello
Teď, když víte, jak tooconnect toohello Azure Database pro databázi MySQL, jsme můžete projít postupy toocomplete některé základní úlohy.

Jsme nejprve vytvořit tabulku a načíst určitými daty. Umožňuje vytvořit tabulku, která ukládá informace o inventáři.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-hello-tables"></a>Načtení dat do tabulky hello
Teď, když máme tabulku, jsme do něj vložte některá data. V okně hello spusťte příkazový řádek spusťte hello následující dotaz tooinsert některé řádky dat.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Nyní máte dva řádky ukázková data do hello tabulky, které jste vytvořili dříve.

## <a name="query-and-update-hello-data-in-hello-tables"></a>Dotazování a aktualizovat hello data v tabulkách hello
Spusťte následující dotaz tooretrieve informace z tabulky databáze hello hello.
```sql
SELECT * FROM inventory;
```

Můžete také aktualizovat hello data v tabulkách hello.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Při načítání dat, získá Hello řádek příslušným způsobem aktualizuje.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a>Obnovit do databáze tooa předchozího bodu v čase
Představte si, že omylem odstraněn k tabulce důležité databáze a nelze snadno obnovit hello data. Databáze pro databázi MySQL Azure vám umožní toorestore hello server tooa bod v čase, vytváření kopie databáze hello do nového serveru. Můžete použít tento nový server toorecover odstraněná data. Následující kroky obnovení hello ukázkový server tooa bod před přidáním tabulky hello Hello.

1. V hello portálu Azure vyhledejte Azure Database pro databázi MySQL. Na hello **přehled** klikněte na tlačítko **obnovení** na panelu nástrojů hello. Otevře se stránka obnovení Hello.

   ![10-1 obnovení databáze](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. Vyplňte hello **obnovení** formulář hello požadované informace.
   
   ![Formulář obnovení 10-2](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Bod obnovení**: Vyberte bodu v čase, které chcete toorestore, v časovém rámci hello uvedené. Ujistěte se, že tooconvert tooUTC vaše místní časové pásmo.
   - **Obnovení serveru toonew**: Zadejte nový název serveru, který chcete toorestore k.
   - **Umístění**: oblast hello je stejný jako zdrojový server hello a nedá se změnit.
   - **Cenová úroveň**: hello cenová úroveň je hello stejný jako hello zdrojového serveru a nelze je změnit.
   
3. Klikněte na tlačítko **OK** toorestore hello serveru příliš[obnovit tooa bodu v čase](./howto-restore-server-portal.md) před hello tabulka byla odstraněna. Obnovení serveru vytvoří novou kopii server hello od hello bodu v čase, který zadáte. 

## <a name="next-steps"></a>Další kroky
V tomto kurzu použijete hello Azure portálu toolearned jak na:

> [!div class="checklist"]
> * Vytvoření Azure databáze pro databázi MySQL
> * Konfigurace brány firewall serveru hello
> * Použijte nástroj příkazového řádku toocreate mysql databáze
> * Načíst ukázková data
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

> [!div class="nextstepaction"]
> [Jak aplikace tooAzure tooconnect databáze pro databázi MySQL](./howto-connection-string.md)
