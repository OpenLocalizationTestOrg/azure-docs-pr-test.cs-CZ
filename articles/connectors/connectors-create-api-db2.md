---
title: "konektor hello DB2 aaaAdd ve vašich Logic Apps | Microsoft Docs"
description: "Přehled konektoru DB2 s parametry rozhraní REST API"
services: 
documentationcenter: 
author: gplarsen
manager: erikre
editor: 
tags: connectors
ms.assetid: 1c6b010c-beee-496d-943a-a99e168c99aa
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/26/2016
ms.author: plarsen; ladocs
ms.openlocfilehash: d836c61231d3c9cfdb30ff9c40a48b1718f3ffb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-db2-connector"></a>Začínáme s konektorem DB2 hello
Konektor Microsoft pro DB2 připojí tooresources Logic Apps, které jsou uloženy v databázi IBM DB2. Tento konektor zahrnuje toocommunicate klienta Microsoft se vzdálenými počítači server DB2 přes síť TCP/IP. To zahrnuje cloudu databáze, například IBM Bluemix dashDB nebo IBM DB2 pro systém Windows spuštěn v Azure virtualizace a místní databáze pomocí hello místní data gateway. V tématu hello [podporované seznamu](connectors-create-api-db2.md#supported-db2-platforms-and-versions) IBM DB2 platforem a verzí (v tomto tématu).

Hello DB2 konektor podporuje hello následujících databázové operace:

* Seznam tabulek databáze
* Pomocí vyberte jeden řádek pro čtení
* Číst všechny řádky pomocí vyberte
* Přidejte jeden řádek pomocí vložení
* Příkaz ALTER jeden řádek použití aktualizace
* Odeberte jeden řádek používání příkazu DELETE

Toto téma ukazuje, jak toouse hello konektoru logiku aplikace tooprocess databáze operace.

toolearn Další informace o Logic Apps, najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="available-actions"></a>Dostupné akce
Hello DB2 konektor podporuje následující akce aplikace logiky hello:

* GetTables
* GetRow
* Proces GetRows
* Insertrow –
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Seznam tabulek
Vytvoření aplikace logiky pro všechny operace se skládá z mnoho kroků, které se provádí prostřednictvím portálu Microsoft Azure hello.

V rámci aplikace logiky hello můžete přidat akci toolist tabulky v databázi DB2. Hello akce dá pokyn příkaz hello konektor tooprocess DB2 schématu, jako například `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název**, jako například `Db2getTables`, **předplatné**, **skupiny prostředků**, **umístění**, a **aplikace Služba plán**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu.
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a poté nastavte hello **Interval** tootype **7**.  
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** zadejte `db2` v hello **vyhledejte další akce** textového pole a pak vyberte **DB2 - Get tabulky (Preview)**.
   
   ![](./media/connectors-create-api-db2/Db2connectorActions.png)  
6. V hello **DB2 - Get tabulky** konfigurace podokně, vyberte **políčko** tooenable **připojit prostřednictvím místní brána dat**. Všimněte si, že nastavení hello změní z tooon místní cloudu.
   
   * Zadejte hodnotu pro **Server**, hello tvar adresu nebo alias číslo portu dvojtečkou. Můžete například zadat `ibmserver01:50000`.
   * Zadejte hodnotu pro **databáze**. Můžete například zadat `nwind`.
   * Vyberte hodnotu pro **ověřování**. Vyberte například **základní**.
   * Zadejte hodnotu pro **uživatelské jméno**. Můžete například zadat `db2admin`.
   * Zadejte hodnotu pro **heslo**. Můžete například zadat `Password1`.
   * Vyberte hodnotu pro **brány**. Vyberte například **datagateway01**.
7. Vyberte **vytvořit**a potom vyberte **Uložit**. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)
8. V hello **Db2getTables** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
9. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_tables**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat seznam tabulek.
   
   ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-hello-connections"></a>Vytvoření připojení hello
Tento konektor podporuje připojení toodatabases hostovaná místně a v cloudu hello pomocí hello následující vlastnosti připojení. 

| Vlastnost | Popis |
| --- | --- |
| server |Povinná hodnota. Přijme řetězcovou hodnotu, kterou představuje adresu TCP/IP nebo alias, ve formátu protokolu IPv4 nebo IPv6, a potom (oddělený středníkem) podle čísla portu TCP/IP. |
| Databáze |Povinná hodnota. Přijme hodnotu řetězce, který představuje DRDA název pro relační databáze (RDBNAM). DB2 pro z/OS přijímá řetězec 16 bajtů (databáze se označuje jako IBM DB2 pro umístění z/OS). DB2 pro i5/OS přijímá řetězec 18 bajtů (databáze se označuje jako IBM DB2 pro i relační databáze). DB2 pro LUW přijme řetězec 8 bajtů. |
| Ověřování |Volitelné. Přijme hodnotu položky seznamu, Basic nebo Windows (kerberos). |
| uživatelské jméno |Povinná hodnota. Přijme hodnotu řetězce. DB2 pro z/OS přijme řetězec 8 bajtů. DB2 pro i přijímá řetězec 10 bajtů. DB2 pro Linux nebo UNIX přijme řetězec 8 bajtů. DB2 pro systém Windows přijme řetězec 30 bajtů. |
| heslo |Povinná hodnota. Přijme hodnotu řetězce. |
| Brány |Povinná hodnota. Přijme hodnotu položky seznamu, představující hello místní data gateway definované tooLogic aplikací v rámci skupiny úložišť hello. |

## <a name="create-hello-on-premises-gateway-connection"></a>Vytvoření připojení k bráně hello na místě
Tento konektor můžete přístup k místní databázi DB2 pomocí hello místní brány. Další informace naleznete v tématech brány. 

1. V hello **brány** konfigurace podokně, vyberte **políčko** tooenable **připojit prostřednictvím brány**. Všimněte si, že nastavení hello změní z tooon místní cloudu.
2. Zadejte hodnotu pro **Server**, hello tvar adresu nebo alias číslo portu dvojtečkou. Můžete například zadat `ibmserver01:50000`.
3. Zadejte hodnotu pro **databáze**. Můžete například zadat `nwind`.
4. Vyberte hodnotu pro **ověřování**. Vyberte například **základní**.
5. Zadejte hodnotu pro **uživatelské jméno**. Můžete například zadat `db2admin`.
6. Zadejte hodnotu pro **heslo**. Můžete například zadat `Password1`.
7. Vyberte hodnotu pro **brány**. Vyberte například **datagateway01**.
8. Vyberte **vytvořit** toocontinue. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-hello-cloud-connection"></a>Vytvoření připojení cloudu hello
Tento konektor můžete přístup k databázi DB2 cloudu. 

1. V hello **brány** konfigurace podokně, ponechejte hello **políčko** zakázáno (nepoužité) **připojit prostřednictvím brány**. 
2. Zadejte hodnotu pro **název připojení**. Můžete například zadat `hisdemo2`.
3. Zadejte hodnotu pro **název serveru DB2**, hello tvar adresu nebo alias číslo portu dvojtečkou. Můžete například zadat `hisdemo2.cloudapp.net:50000`.
4. Zadejte hodnotu pro **název databáze DB2**. Můžete například zadat `nwind`.
5. Zadejte hodnotu pro **uživatelské jméno**. Můžete například zadat `db2admin`.
6. Zadejte hodnotu pro **heslo**. Můžete například zadat `Password1`.
7. Vyberte **vytvořit** toocontinue. 
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Načíst pomocí vyberte všechny řádky
Můžete definovat logiku aplikace akce toofetch všechny řádky v tabulce DB2. To dává pokyn hello konektor tooprocess příkaz DB2 SELECT, například `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název**, jako například `Db2getRows`, **předplatné**, **skupiny prostředků**, **umístění**, a **aplikace Služba plán**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu.
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a potom vyberte  **Interval** tootype **7**. 
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** zadejte `db2` v hello **vyhledejte další akce** textového pole a pak vyberte **DB2 - Get řádků (Preview)**.
6. V hello **získat řádky (Preview)** akce, vyberte **změnit připojení**.
7. V hello **připojení** konfigurace podokně, vyberte **vytvořit nový**. 
   
    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
8. V hello **brány** konfigurace podokně, ponechejte hello **políčko** zakázáno (nepoužité) **připojit prostřednictvím brány**.
   
   * Zadejte hodnotu pro **název připojení**. Můžete například zadat `HISDEMO2`.
   * Zadejte hodnotu pro **název serveru DB2**, hello tvar adresu nebo alias číslo portu dvojtečkou. Můžete například zadat `HISDEMO2.cloudapp.net:50000`.
   * Zadejte hodnotu pro **název databáze DB2**. Můžete například zadat `NWIND`.
   * Zadejte hodnotu pro **uživatelské jméno**. Můžete například zadat `db2admin`.
   * Zadejte hodnotu pro **heslo**. Můžete například zadat `Password1`.
9. Vyberte **vytvořit** toocontinue.
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)
10. V hello **název tabulky** seznamu, vyberte hello **šipka dolů**a potom vyberte **oblasti**.
11. Volitelně vyberte **zobrazit rozšířené možnosti** toospecify možnosti dotazu.
12. Vyberte **Uložit**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)
13. V hello **Db2getRows** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
14. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_rows**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat seznam řádků.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Přidejte jeden řádek pomocí vložení
Můžete definovat logiku aplikace akce tooadd jeden řádek v tabulce DB2. Tato akce dá pokyn hello konektor tooprocess příkazu DB2 INSERT, jako například `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název**, jako například `Db2insertRow`, **předplatné**, **skupiny prostředků**, **umístění**, a **aplikace Služba plán**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu.
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a potom vyberte  **Interval** tootype **7**. 
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** zadejte **db2** v hello **vyhledejte další akce** textového pole a pak vyberte **DB2 - vložit řádek (Preview)**.
6. V hello **DB2 - vložit řádek (Preview)** akce, vyberte **změnit připojení**. 
7. V hello **připojení** konfigurace podokně, vyberte připojení. Vyberte například **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. V hello **název tabulky** seznamu, vyberte hello **šipka dolů**a potom vyberte **oblasti**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červenou hvězdičkou). Můžete například zadat `99999` pro **AREAID**, typ `Area 99999`a typ `102` pro **REGIONID**. 
10. Vyberte **Uložit**.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
11. V hello **Db2insertRow** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
12. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_rows**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat hello nový řádek.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Načíst pomocí vyberte jeden řádek
Můžete definovat logiku aplikace akce toofetch jeden řádek v tabulce DB2. Tato akce dá pokyn hello konektor tooprocess výpis DB2 vyberte kde, jako například `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název** (např.) "**Db2getRow**"), **předplatné**, **skupiny prostředků**, **umístění**, a **plán služby App Service**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu. 
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a potom vyberte  **Interval** tootype **7**. 
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** zadejte **db2** v hello **vyhledejte další akce** textového pole a pak vyberte **DB2 - Get řádků (Preview)**.
6. V hello **získat řádky (Preview)** akce, vyberte **změnit připojení**. 
7. V hello **připojení** konfigurace podokně, vyberte existující připojení. Vyberte například **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. V hello **název tabulky** seznamu, vyberte hello **šipka dolů**a potom vyberte **oblasti**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červenou hvězdičkou). Můžete například zadat `99999` pro **AREAID**. 
10. Volitelně vyberte **zobrazit rozšířené možnosti** toospecify možnosti dotazu.
11. Vyberte **Uložit**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)
12. V hello **Db2getRow** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
13. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_rows**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat řádek.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Změnit jeden řádek použití aktualizace
Můžete definovat logiku aplikace akce toochange jeden řádek v tabulce DB2. Tato akce dá pokyn hello konektor tooprocess výpis DB2 aktualizace, jako například `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název**, jako například `Db2updateRow`, **předplatné**, **skupiny prostředků**, **umístění**, a **aplikace Služba plán**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu.
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a potom vyberte  **Interval** tootype **7**. 
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** zadejte **db2** v hello **vyhledejte další akce** textového pole a pak vyberte **DB2 - aktualizace řádek (Preview)**.
6. V hello **DB2 - aktualizace řádek (Preview)** akce, vyberte **změnit připojení**. 
7. V hello **připojení** konfigurace podokně, vyberte tooselect stávající připojení. Vyberte například **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. V hello **název tabulky** seznamu, vyberte hello **šipka dolů**a potom vyberte **oblasti**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červenou hvězdičkou). Můžete například zadat `99999` pro **AREAID**, typ `Updated 99999`a typ `102` pro **REGIONID**. 
10. Vyberte **Uložit**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)
11. V hello **Db2updateRow** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
12. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_rows**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat hello nový řádek.
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Odeberte jeden řádek používání příkazu DELETE
Můžete definovat logiku aplikace akce tooremove jeden řádek v tabulce DB2. Tato akce dá pokyn hello konektor tooprocess výpis DB2 odstranit, jako například `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název**, jako například `Db2deleteRow`, **předplatné**, **skupiny prostředků**, **umístění**, a **aplikace Služba plán**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu. 
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a potom vyberte  **Interval** tootype **7**. 
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** seznamu, vyberte **db2** v hello **vyhledejte další akce** textového pole a pak vyberte **DB2 - odstranit řádek (Preview)**.
6. V hello **DB2 - odstranit řádek (Preview)** akce, vyberte **změnit připojení**. 
7. V hello **připojení** konfigurace podokně, vyberte existující připojení. Vyberte například **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. V hello **název tabulky** seznamu, vyberte hello **šipka dolů**a potom vyberte **oblasti**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červenou hvězdičkou). Můžete například zadat `99999` pro **AREAID**. 
10. Vyberte **Uložit**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)
11. V hello **Db2deleteRow** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
12. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_rows**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat hello odstranit řádek.
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="supported-db2-platforms-and-versions"></a>Podporované platformy DB2 a verze
Tento konektor podporuje následující IBM DB2 platformy a verze, jakož i IBM DB2 kompatibilní produkty (např. IBM Bluemix dashDB) podporující distribuované relační databáze architektura (DRDA) SQL přístup správce (SQLAM) verze 10 a 11 hello:

* IBM DB2 pro z 11.1 operačního systému
* IBM DB2 pro z 10.1 operačního systému
* IBM DB2 pro i 7.3
* IBM DB2 pro i 7.2
* IBM DB2 pro i 7.1
* IBM DB2 pro LUW 11
* IBM DB2 pro LUW 10.5

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/db2/). 

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).

