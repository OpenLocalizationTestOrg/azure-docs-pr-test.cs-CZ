---
title: "Azure Portal: Vytvoření Azure Database for PostgreSQL | Dokumentace Microsoftu"
description: "Úvodní příručka k vytvoření a správě serveru Azure Database for PostgreSQL pomocí uživatelského rozhraní portálu Azure Portal."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 08/10/2017
ms.openlocfilehash: a4ab9e9e7d331d35547d56406e1da993ade58113
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-database-for-postgresql-in-the-azure-portal"></a>Vytvoření Azure Database for PostgreSQL na portálu Azure Portal

Azure Database for PostgreSQL je spravovaná služba, která umožňuje spouštět, spravovat a škálovat vysoce dostupné databáze PostgreSQL v cloudu. V tomto rychlém startu se dozvíte, jak přibližně během pěti minut vytvořit server Azure Database for PostgreSQL pomocí webu Azure Portal.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal
Otevřete svůj webový prohlížeč a přejděte na [portál Microsoft Azure Portal](https://portal.azure.com/). Zadejte přihlašovací údaje pro přihlášení k portálu. Výchozím zobrazením je váš řídicí panel služby.

## <a name="create-an-azure-database-for-postgresql"></a>Vytvoření Azure Database for PostgreSQL

Server Azure Database for PostgreSQL se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](./concepts-compute-unit-and-storage.md). Server se vytvoří v rámci [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md).

Server Azure Database for PostgreSQL vytvoříte pomocí tohoto postupu:
1.  Klikněte na tlačítko **Nový** (+) v levém horním rohu webu Azure Portal.
2.  Na stránce **Nový** vyberte **Databáze** a na stránce **Databáze** vyberte **Azure Database for PostgreSQL**.
 ![Azure Database for PostgreSQL – vytvoření databáze](./media/quickstart-create-database-portal/1-create-database.png)

3.  Vyplňte formulář podrobností nového serveru pomocí následujících informací, jak je vidět na předchozím obrázku:

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    Název serveru |*mypgserver-20170401*|Zvolte jedinečný název serveru, který identifikuje váš server Azure Database for PostgreSQL. K názvu serveru, který zadáte pro připojování aplikací, se připojí název domény *postgres.database.azure.com*. Název serveru může obsahovat pouze malá písmena, číslice a znak spojovníku (-) a musí se skládat ze 3 až 63 znaků.
    Předplatné|*Vaše předplatné*|Předplatné Azure, které chcete použít pro váš server. Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém se prostředek účtuje.
    Skupina prostředků|*myresourcegroup*| Můžete vytvořit nový název skupiny prostředků nebo použít některý ze stávajících ve vašem předplatném.
    Přihlašovací jméno správce serveru |*mylogin*| Vytvořte si vlastní přihlašovací účet, který budete používat při připojování k serveru. Přihlašovací jméno správce nemůže být azure_superuser, azure_pg_admin, admin, administrator, root, guest ani public a nesmí začínat na pg_.
    Heslo |*Nějaké si zvolte* | Vytvořte nové heslo pro účet správce serveru. Musí se skládat z 8 až 128 znaků. Heslo musí obsahovat znaky ze tří z těchto kategorií – velká písmena anglické abecedy, malá písmena anglické abecedy, číslice (0–9) a jiné než alfanumerických znaky (!, $, #, % apod.).
    Umístění|*Oblast nejbližší vašim uživatelům*| Zvolte umístění co nejblíže vašim uživatelům.
    Verze PostgreSQL|*Zvolte nejnovější verzi*| Zvolte nejnovější verzi, pokud nemáte specifické požadavky.
    Cenová úroveň | **Basic**, **50 výpočetních jednotek**, **50 GB** | Klikněte na **Cenová úroveň** a určete úroveň služby a úroveň výkonu pro novou databázi. Na kartě v horní části zvolte úroveň Basic. Klikněte na levý konec posuvníku Výpočetní jednotky a pro účely tohoto rychlého startu upravte hodnotu na nejnižší dostupné množství. Kliknutím na **OK** uložte výběr cenové úrovně. Viz následující snímek obrazovky.
    | Připnutí na řídicí panel | Zaškrtnout | Zaškrtněte možnost **Připnout na řídicí panel**, abyste povolili snadné sledování vašeho serveru na přední stránce řídicího panelu na webu Azure Portal.

  > [!IMPORTANT]
  > Zde zadané jméno správce serveru a heslo se vyžadují k přihlášení na server a jeho databáze dále v tomto rychlém startu. Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.

    ![Azure Database for PostgreSQL – výběr cenové úrovně](./media/quickstart-create-database-portal/2-service-tier.png)

4.  Klikněte na **Vytvořit**, aby se server zřídil. Zřizování trvá několik minut, maximálně však 20 minut.

5.  Na panelu nástrojů klikněte na **Oznámení** a sledujte proces nasazení.
 ![Azure Database for PostgreSQL – zobrazení oznámení](./media/quickstart-create-database-portal/3-notifications.png)
   
  Ve výchozím nastavení se databáze **postgres** vytvoří v rámci vašeho serveru. Databáze [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) je výchozí databáze určená pro uživatele, nástroje a aplikace třetích stran. 

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurace pravidla brány firewall na úrovni serveru

Služba Azure Database for PostgreSQL vytváří bránu firewall na úrovni serveru. Tato brána firewall brání externím aplikacím a nástrojům v připojení k serveru a kterékoli databázi na serveru, pokud není vytvořené pravidlo brány firewall k otevření brány firewall pro konkrétní IP adresy. 

1.  Po dokončení nasazení najděte váš server. V případě potřeby ho můžete vyhledat. Klikněte například na **Všechny prostředky** v levé nabídce a zadejte název serveru (například ukázkový *mypgserver-20170401*). Vyhledáte tak nově vytvořený server. Klikněte na název vašeho serveru uvedený ve výsledcích hledání. Otevře se stránka **Přehled** vašeho serveru a poskytne vám možnosti další konfigurace.
 
    ![Azure Database for PostgreSQL – hledání názvu serveru](./media/quickstart-create-database-portal/4-locate.png)

2.  Na stránce serveru vyberte **Zabezpečení připojení**. 
    ![Azure Database for PostgreSQL – vytvoření pravidla brány firewall](./media/quickstart-create-database-portal/5-firewall-2.png)

3.  Pod nadpisem **Pravidla brány firewall** klikněte do prázdného textového pole ve sloupci **Název pravidla** a začněte vytvářet pravidlo brány firewall. 

    Pro účely tohoto rychlého startu povolíme provoz do serveru ze všech IP adres tak, že do textového pole v každém sloupci vyplníme následující hodnoty:

    Název pravidla | Počáteční IP adresa | Koncová IP adresa 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Na horním panelu nástrojů na stránce Zabezpečení připojení klikněte na **Uložit**. Chvíli počkejte, než se zobrazí oznámení o úspěšném dokončení aktualizace zabezpečení připojení, a pak pokračujte.

    > [!NOTE]
    > Připojení k serveru Azure Database for PostgreSQL komunikují přes port 5432. Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 5432 bránou firewall vaší sítě povolený. Pokud je to tak, k serveru se nebudete moct připojit, dokud vaše IT oddělení neotevře port 5432.
    >

## <a name="get-the-connection-information"></a>Získání informací o připojení

Při vytvoření našeho serveru Azure Database for PostgreSQL se vytvoří i výchozí databáze **postgres**. Pokud se chcete připojit ke svému databázovému serveru, je potřeba si vzpomenout na úplný název serveru a přihlašovací údaje správce. Pravděpodobně jste si tyto hodnoty poznamenali v dřívější části tohoto článku Rychlý start. Pokud ne, název serveru a přihlašovací informace můžete snadno vyhledat na stránce Přehled serveru na webu Azure Portal.

1. Otevřete stránku **Přehled** vašeho serveru. Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.
    Přejeďte kurzorem přes jednotlivá pole a vpravo od textu se zobrazí ikona kopírování. Podle potřeby hodnoty zkopírujte kliknutím na ikonu kopírování.

 ![Azure Database for PostgreSQL – přihlašovací jméno správce serveru](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a>Připojení k databázi PostgreSQL pomocí psql ve službě Cloud Shell

Existuje řada aplikací, které můžete použít pro připojení k vašemu serveru Azure Database for PostgreSQL. Nejprve si ukážeme, jak se k serveru připojit pomocí nástroje příkazového řádku psql.  Můžete použít webový prohlížeč a Azure Cloud Shell, jak je zde popsáno, aniž by bylo nutné instalovat další software. Pokud máte nástroj psql nainstalovaný místně na svém počítači, můžete se připojit i z něj.

1. Pomocí ikony terminálu v horním navigačním podokně spusťte službu Azure Cloud Shell.

   ![Azure Database for PostgreSQL – ikona terminálu Azure Cloud Shell](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Azure Cloud Shell se otevře v prohlížeči a umožní vám zadávat příkazy prostředí Bash.

   ![Azure Database for PostgreSQL – příkazový řádek Bash služby Azure Shell](./media/quickstart-create-database-portal/8-bash.png)

3. V příkazovém řádku služby Cloud Shell se zadáním příkazového řádku psql na zeleném řádku připojte k databázi na vašem serveru Azure Database for PostgreSQL.

    Následující formát se používá pro připojení k serveru Azure Database for PostgreSQL s nástrojem [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html):
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    Například následující příkaz se připojí k ukázkovému serveru:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    Parametr psql |Navrhovaná hodnota|Popis
    ---|---|---
    --host | *název serveru* | Zadejte hodnotu názvu serveru, kterou jste použili dříve při vytváření služby Azure Database for PostgreSQL. Náš ukázkový server v příkladu je mypgserver-20170401.postgres.database.azure.com. Použijte plně kvalifikovaný název domény (\*.postgres.database.azure.com), jak je znázorněno v příkladu. Pokud si název vašeho serveru nepamatujete, získejte informace o připojení pomocí postupu v předchozí části. 
    --port | **5432** | Při připojování ke službě Azure Database for PostgreSQL vždy používejte port 5432. 
    --username | *přihlašovací jméno správce serveru* |Zadejte přihlašovací uživatelské jméno správce serveru, které jste zadali dříve při vytváření služby Azure Database for PostgreSQL. Pokud si uživatelské jméno nepamatujete, získejte informace o připojení pomocí postupu v předchozí části.  Formát je *username@servername*.
    --dbname | **postgres** | Pro první připojení použijte výchozí, systémem vygenerovaný, název databáze *postgres*. Později vytvoříte vlastní databázi.

    Po spuštění příkazu psql s vlastními hodnotami parametrů se zobrazí výzva k zadání hesla správce serveru. Jedná se o stejné heslo, které jste zadali při vytváření serveru. 

    Parametr psql |Navrhovaná hodnota|Popis
    ---|---|---
    heslo | *vaše heslo správce* | Poznámka: na příkazovém řádku Bash se zadávané znaky hesla nezobrazí. Po zadání všech znaků stiskněte enter, abyste se ověřili a připojili.

    Jakmile budete připojeni, nástroj psql zobrazí příkazový řádek postgres, kde můžete zadávat příkazy jazyka SQL. Ve výstupu počátečního připojení se může zobrazit varování, protože nástroj psql ve službě Azure Cloud Shell může mít jinou verzi než na serveru Azure Database for PostgreSQL. 
    
    Příklad výstupu nástroje psql:
    ```bash
    psql (9.5.7, server 9.6.2)
    WARNING: psql major version 9.5, server major version 9.6.
        Some psql features might not work.
    SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
    Type "help" for help.
   
    postgres=> 
    ```

    > [!TIP]
    > Pokud brána firewall není nakonfigurovaná k povolení IP adres služby Azure Cloud Shell, dojde k následující chybě:
    > 
    > "psql: FATAL: no pg_hba.conf entry for host "138.91.195.82", user "mylogin", database "postgres", SSL on FATAL:  SSL connection is required. (psql: ZÁVAŽNÁ CHYBA: nenalezen žádný záznam pg_hba.conf pro hostitele 138.91.195.82, uživatele mylogin, databázi postgres, ZÁVAŽNÁ CHYBA SSL: vyžaduje se připojení SSL.) Zadejte možnosti SSL a zkuste to znovu.
    > 
    > Pokud chcete chybu vyřešit, ujistěte se, že konfigurace serveru odpovídá postupu v části *Konfigurace pravidla brány firewall na úrovni serveru* tohoto článku.

4.  Vytvořte prázdnou databázi zadáním následujícího příkazu na příkazovém řádku:
    ```bash
    CREATE DATABASE mypgsqldb;
    ```
    Dokončení příkazu může chvíli trvat. 

5.  V příkazovém řádku proveďte následující příkaz pro přepnutí připojení na nově vytvořenou databázi **mypgsqldb**.
    ```bash
    \c mypgsqldb
    ```

6.  Zadejte \q a stisknutím klávesy ENTER ukončete psql. Jakmile budete hotovi, můžete zavřít Azure Cloud Shell.

Právě jste se připojili ke službě Azure Database for PostgreSQL a vytvořili prázdnou uživatelskou databázi. Pokračujte k další části a připojte se pomocí dalšího běžného nástroje pgAdmin.

## <a name="connect-to-postgresql-database-using-pgadmin"></a>Připojení k databázi PostgreSQL pomocí aplikace pgAdmin

Připojení k serveru Azure PostgreSQL pomocí grafického uživatelského rozhraní aplikace _pgAdmin_
1.  Na klientském počítači spusťte aplikaci _pgAdmin_. _pgAdmin_ můžete nainstalovat ze stránky http://www.pgadmin.org/.
2.  Klikněte na ikonu **Přidat nový server** v části **Rychlé odkazy** uprostřed stránky Řídicí panel.
3.  V dialogovém okně **Vytvořit – server** na kartě **Obecné** zadejte jedinečný popisný název serveru, jako například **Azure PostgreSQL Server**.
![Nástroj pgAdmin – Vytvořit – server](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)
4.  V dialogovém okně **Vytvořit – server** na kartě **Připojení** použijte uvedená nastavení a klikněte na **Uložit**.
   ![Nástroj pgAdmin – Vytvořit – server](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)

    Parametr pgAdmin |Navrhovaná hodnota|Popis
    ---|---|---
    Název nebo adresa hostitele | *název serveru* | Zadejte hodnotu názvu serveru, kterou jste použili dříve při vytváření služby Azure Database for PostgreSQL. Náš ukázkový server v příkladu je mypgserver-20170401.postgres.database.azure.com. Použijte plně kvalifikovaný název domény (\*.postgres.database.azure.com), jak je znázorněno v příkladu. Pokud si název vašeho serveru nepamatujete, získejte informace o připojení pomocí postupu v předchozí části. 
    Port | **5432** | Při připojování ke službě Azure Database for PostgreSQL vždy používejte port 5432.  
    Databáze údržby | **postgres** | Požijte výchozí systémem generovaný název databáze *postgres*.
    Uživatelské jméno | *přihlašovací jméno správce serveru* | Zadejte přihlašovací uživatelské jméno správce serveru, které jste zadali dříve při vytváření služby Azure Database for PostgreSQL. Pokud si uživatelské jméno nepamatujete, získejte informace o připojení pomocí postupu v předchozí části. Formát je *username@servername*.
    Heslo | *vaše heslo správce* |  Heslo, které jste si zvolili při vytváření serveru dříve v tomto rychlém startu.
    Role | *ponechte prázdné* | V tuto chvíli není potřeba zadávat název role. Ponechte toto pole prázdné.
    Režim SSL | Vyžadovat | Ve výchozím nastavení jsou všechny servery Azure PostgreSQL vytvořeny se zapnutým vynucováním SSL. Pokud chcete vynucování SSL vypnout, přečtěte si podrobnosti v tématu [Vynucování SSL](./concepts-ssl-connection-security.md).
    
5.  Klikněte na **Uložit**.
6.  V levém podokně Prohlížeč rozbalte uzel **Servery**. Vyberte váš server, například **Azure PostgreSQL Server**, a kliknutím se k němu připojte.
7. Rozbalte uzel serveru a pod ním pak rozbalte **Databáze**. Seznam by měl obsahovat vaši existující databázi *postgres* a všechny nově vytvořené uživatelské databáze, jako je *mypgsqldb*, kterou jsme vytvořili v předchozí části. Nezapomeňte, že na jeden server se službou Azure Database for PostgreSQL můžete vytvořit více databází.
8. Klikněte pravým tlačítkem na **Databáze**, zvolte nabídku **Vytvořit** a klikněte na **Databáze**.
9.  Do pole **Databáze** zadejte libovolný název databáze, například *mypgsqldb*, jak je znázorněno v příkladu. 
10. Z rozevírací nabídky vyberte **Vlastníka** databáze. Zvolte přihlašovací jméno správce serveru, jako je *mylogin* v našem příkladu.
10. Kliknutím na **Uložit** vytvořte novou prázdnou databázi.
11. V podokně **Prohlížeč** se vytvořená databáze zobrazí v seznamu databází pod názvem vašeho serveru.
 ![Nástroj pgAdmin – Vytvořit – databáze](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Vyčištění prostředků
Vyčistěte prostředky, které jste vytvořili v rámci tohoto rychlého startu, buď odstraněním [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md), což zahrnuje odstranění všech prostředků ve skupině prostředků, nebo odstraněním příslušného prostředku serveru, pokud chcete ostatní prostředky ponechat beze změn.

> [!TIP]
> Další rychlé starty v této kolekci vycházejí z tohoto rychlého startu. Pokud chcete pokračovat v práci s dalšími rychlými starty, neprovádějte čištění prostředků vytvořených v rámci tohoto rychlého startu. Pokud pokračovat nechcete, pomocí následujících kroků odstraňte prostředky vytvořené tímto rychlým startem na webu Azure Portal.

Pokud chcete odstranit celou skupinu prostředků, včetně nově vytvořeného serveru:
1.  Vyhledejte skupinu prostředků na webu Azure Portal. V nabídce vlevo na webu Azure Portal klikněte na **Skupiny prostředků** a potom klikněte na název vaší skupiny prostředků, jako je **myresourcegroup** v našem příkladu.
2.  Na stránce vaší skupiny prostředků klikněte na **Odstranit**. Pak pro potvrzení odstranění zadejte do textového pole název vaší skupiny prostředků, jako je **myresourcegroup** v našem příkladu, a klikněte na **Odstranit**.

Pokud místo toho chcete odstranit nově vytvořený server:
1.  Vyhledejte server na webu Azure Portal, pokud ho ještě nemáte otevřený. V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte server, který jste vytvořili.
2.  Na stránce **Přehled** klikněte na tlačítko **Odstranit** v horním podokně.
![Azure Database for PostgreSQL – odstranění serveru](./media/quickstart-create-database-portal/12-delete.png)
3.  Potvrďte název serveru, který chcete odstranit, a zobrazte jeho databáze, které tím ovlivníte. Do textového pole zadejte název vašeho serveru, jako je **mypgserver-20170401** v našem příkladu, a potom klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)
