---
title: aaaSecure Azure SQL database | Microsoft Docs
description: "Další informace o toosecure techniky a funkce Azure SQL database."
services: sql-database
documentationcenter: 
author: DRediske
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/28/2017
ms.author: daredis
ms.openlocfilehash: 1450d633d6f65faf1b8a2dc0dc7dfe996fb0719d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-your-azure-sql-database"></a>Zabezpečení databáze Azure SQL

Databáze SQL slouží k zabezpečení dat omezením přístupu k databázi tooyour pomocí pravidel brány firewall, ověřovací mechanismy, které by uživatelé tooprove své identity a autorizace toodata prostřednictvím členství na základě rolí a oprávnění, a také prostřednictvím zabezpečení na úrovni řádků a maskování dynamická data.

Hello ochranu databáze před uživateli se zlými úmysly a neoprávněným přístupem pomocí několika jednoduchých kroků můžete zvýšit. V tomto kurzu jste postup: 

> [!div class="checklist"]
> * Nastavit pravidla brány firewall na úrovni serveru pro váš server v hello portálu Azure
> * Nastavit pravidla brány firewall na úrovni databáze pro vaši databázi pomocí aplikace SSMS
> * Připojit databáze tooyour pomocí zabezpečené připojovací řetězec
> * Spravovat přístup uživatelů
> * Chraňte svá data pomocí šifrování
> * Povolit auditování databáze SQL
> * Povolení detekce hrozeb databáze SQL

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.

## <a name="prerequisites"></a>Požadavky

toocomplete tento kurz, ověřte zda máte hello následující:

- Nejnovější verze nainstalované hello [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). 
- Nainstalovaný Microsoft Excel
- Vytvoření služby Azure SQL server a databáze - najdete v části [vytvoření Azure SQL database v hello portál Azure](sql-database-get-started-portal.md), [vytvářet izolované databáze Azure SQL pomocí rozhraní příkazového řádku Azure hello](sql-database-get-started-cli.md), a [vytvořit jeden SQL Azure databáze pomocí prostředí PowerShell](sql-database-get-started-powershell.md). 

## <a name="log-in-toohello-azure-portal"></a>Přihlaste se toohello portálu Azure

Přihlaste se toohello [portál Azure](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Vytvoření pravidla brány firewall na úrovni serveru v hello portálu Azure

Databáze SQL jsou chráněné bránou firewall v Azure. Ve výchozím nastavení všechna připojení toohello server hello databáze a uvnitř hello server odmítají s výjimkou připojení z jiných služeb systému Azure. Další informace najdete v tématu [pravidla brány firewall úrovni serveru a na úrovni databáze Azure SQL Database](sql-database-firewall-configure.md).

nejbezpečnější konfigurace Hello je tooOFF tooset 'Povolit přístup k službám tooAzure'. Pokud potřebujete tooconnect toohello databáze ze služby virtuální počítač Azure nebo cloudu, měli byste vytvořit [vyhrazenou IP adresu](../virtual-network/virtual-networks-reserved-public-ip.md) a povolit pouze hello vyhrazené IP adresy přístup přes bránu firewall hello. 

Postupujte podle těchto kroků toocreate [pravidlo brány firewall na úrovni serveru SQL Database](sql-database-firewall-configure.md) pro váš server tooallow připojení z konkrétní IP adresu. 

> [!NOTE]
> Pokud jste vytvořili ukázkové databáze v Azure pomocí jedné z předchozích kurzy hello nebo – elementy QuickStart a jsou pustíte do tohoto kurzu na počítači s hello stejnou IP adresu to, že když si projít tyto kurzy, můžete tento krok přeskočíte, bude mít již vytvořil pravidlo brány firewall na úrovni serveru.
>

1. Klikněte na tlačítko **databází SQL** z hello levé nabídce databáze hello byste chtěli tooconfigure hello pravidlo brány firewall pro hello **databází SQL** stránky. Hello přehledová stránka otevře vaší databáze, zobrazující text hello plně kvalifikovaný název serveru (například **mynewserver 20170313.database.windows.net**) a poskytuje možnosti pro další konfiguraci.

      ![pravidlo brány firewall serveru](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. Klikněte na tlačítko **nastavení brány firewall serveru** na panelu nástrojů hello viz předchozí obrázek hello. Hello **nastavení brány Firewall** otevře se stránka pro hello databáze SQL server. 

3. Klikněte na tlačítko **přidat IP adresu klienta** na hello tooadd nástrojů hello veřejnou IP adresu hello počítače připojené toohello portál nebo ručně zadejte hello pravidlo brány firewall a poté klikněte **Uložit**.

      ![nastavení pravidla brány firewall serveru](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. Klikněte na tlačítko **OK** a pak klikněte na tlačítko hello **X** tooclose hello **nastavení brány Firewall** stránky.

Nyní můžete přihlásit tooany databázi serveru hello s hello zadaná IP adresa nebo rozsah IP adres.

> [!NOTE]
> SQL Database komunikuje přes port 1433. Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 1433 nemusí mít povolený bránou firewall vaší sítě. Pokud ano, nebudou se moct tooconnect serveru Azure SQL Database tooyour, dokud vaše IT oddělení otevře port 1433.
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a>Vytvoření pravidla brány firewall na úrovni databáze pomocí aplikace SSMS

Pravidla brány firewall na úrovni databáze umožňují toocreate jinou bránu firewall nastavení pro jiné databáze v rámci stejné logické serveru a toocreate pravidel brány firewall, které jsou přenosné - znamená, že následují hello databáze během hello [převzetí služeb při selhání ](sql-database-geo-replication-overview.md) místo uložené na serveru SQL server hello. Pravidla brány firewall na úrovni databáze může být pouze nakonfigurovaná pomocí příkazů Transact-SQL a pouze po nakonfigurování hello první pravidlo brány firewall na úrovni serveru. Další informace najdete v tématu [pravidla brány firewall úrovni serveru a na úrovni databáze Azure SQL Database](sql-database-firewall-configure.md).

Následuje tyto kroky toocreate pravidlo brány firewall pro specifické pro databázi.

1. Připojit databáze tooyour, například pomocí [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

2. V Průzkumníku objektů, klikněte pravým tlačítkem na databázi hello chcete tooadd brány firewall pro pravidlo a klikněte na tlačítko **nový dotaz**. Otevře okno prázdné dotazu který je připojený tooyour databáze.

3. V okně dotazu hello upravit hello IP adresu tooyour veřejnou IP adresu a potom spusťte následující dotaz hello:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. Na panelu nástrojů hello, klikněte na tlačítko **Execute** pravidlo brány firewall toocreate hello.

## <a name="view-how-tooconnect-an-application-tooyour-database-using-a-secure-connection-string"></a>Zobrazit, jak tooconnect aplikaci tooyour databáze pomocí zabezpečené připojovací řetězec

tooensure zabezpečené, šifrované připojení mezi klientská aplikace a SQL Database, hello připojovací řetězec má toobe nakonfigurovaný tak, aby:

- Žádosti o šifrované připojení, a
- certifikát serveru hello toonot vztah důvěryhodnosti. 

Tím se naváže připojení pomocí zabezpečení TLS (Transport Layer) a snižuje riziko hello útokům man-in-the-middle. Můžete získat správně nakonfigurované připojovací řetězce pro vaši databázi SQL pro podporované klientské ovladače z portálu Azure hello jak je vidět na tomto snímku obrazovky pro technologii ADO.net.

1. Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky.

2. Na hello **přehled** stránky pro vaši databázi, klikněte na tlačítko **zobrazit databázové připojovací řetězce**.

3. Zkontrolujte hello dokončení **ADO.NET** připojovací řetězec.

    ![Připojovací řetězec pro ADO.NET](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a>Vytváření uživatelů databáze

Před vytvořením všechny uživatele, musíte nejprve zvolit jednu z Azure SQL Database podporuje dva typy ověřování: 

**Ověřování SQL**, který používá uživatelské jméno a heslo pro přihlášení a uživatele, kteří jsou platné pouze v hello kontextu konkrétní databáze v rámci logického serveru. 

**Ověřování služby Azure Active Directory**, identity, které jsou spravované službou Azure Active Directory, který používá. 

Pokud chcete, aby toouse [Azure Active Directory](./sql-database-aad-authentication.md) tooauthenticate proti databázi SQL, vyplněná Azure Active Directory musí existovat, abyste mohli pokračovat.

Postupujte podle těchto kroků toocreate uživatele pomocí ověřování SQL:

1. Připojit databáze tooyour, například pomocí [SQL Server Management Studio](./sql-database-connect-query-ssms.md) pomocí svých přihlašovacích údajů správce serveru.

2. V Průzkumníku objektů, klikněte pravým tlačítkem na databázi hello tooadd nového uživatele na a klikněte na tlačítko **nový dotaz**. Otevře okno prázdné dotazu který je připojený toohello vybrané databáze.

3. V okně dotazu hello zadejte hello následující dotaz:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. Na panelu nástrojů hello, klikněte na tlačítko **Execute** toocreate hello uživatele.

5. Ve výchozím nastavení hello uživatel může připojit toohello databáze, ale neobsahuje žádná oprávnění tooread nebo zapisovat data. toogrant těchto oprávnění toohello nově vytvořený uživatel, spustit hello následující dva příkazy v novém okně dotazu

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

Je nejlepší postup toocreate těchto účtů bez oprávnění správce v hello databáze úrovně tooconnect tooyour databáze, pokud potřebujete tooexecute správce úkoly, jako je vytváření nových uživatelů. Zkontrolujte hello [kurz služby Azure Active Directory](./sql-database-aad-authentication-configure.md) o tooauthenticate pomocí služby Azure Active Directory.


## <a name="protect-your-data-with-encryption"></a>Chraňte svá data pomocí šifrování

Azure SQL Database transparentní šifrování dat (šifrování TDE) automaticky šifruje vaše data v klidu, bez nutnosti jakékoli změny toohello aplikace přístup k hello šifrované databázi. Pro nově vytvořené databáze je ve výchozím nastavení šifrování TDE. tooenable TDE pro databáze nebo tooverify, kterých je na serveru, postupujte takto:

1. Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky. 

2. Klikněte na **transparentní šifrování dat** tooopen hello konfigurační stránku pro šifrování TDE.

    ![Transparentní šifrování dat](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. V případě potřeby nastavte **šifrování dat** tooON a klikněte na tlačítko **Uložit**.

Spustí se proces šifrování Hello hello pozadí. Můžete sledovat průběh hello pomocí připojování tooSQL databáze [SQL Server Management Studio](./sql-database-connect-query-ssms.md) dotazováním hello encryption_state sloupec hello `sys.dm_database_encryption_keys` zobrazení.

## <a name="enable-sql-database-auditing-if-necessary"></a>Povolit auditování databáze SQL, v případě potřeby

Auditování databáze SQL Azure sleduje události databáze a zapisuje je protokol auditování tooan ve vašem účtu úložiště Azure. Auditování vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou případné porušení zabezpečení. Postupujte podle těchto kroků toocreate výchozí zásady auditování pro databázi SQL:

1. Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky. 

2. V okně Nastavení hello, vyberte **auditování a detekce hrozeb**. Všimněte si, že auditování na úrovni serveru je diabled a zda je **zobrazit nastavení serveru** odkaz, který vám umožní tooview nebo změna nastavení auditování serveru hello z tohoto kontextu.

    ![Auditování okno](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Pokud dáváte přednost tooenable auditu typu (nebo umístění?) liší od hello uvedené na úrovni serveru hello, zapněte **ON** auditování a zvolte hello **Blob** typu auditování. Pokud je auditování objektů Blob serveru je povoleno, budou existovat audit databáze nakonfigurované hello node souběžně s auditování objektů Blob server hello.

    ![Zapnout auditování](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. Vyberte **podrobnosti úložiště** tooopen hello okno úložiště protokoly auditu. Účet úložiště Azure vyberte hello uložení protokoly a dobu uchování hello, po které hello se odstraní staré protokoly, pak klikněte na **OK** dolnímu hello. 

   > [!TIP]
   > Použití hello stejný účet úložiště pro všechny tooget hello auditování databází naplno hello auditování sestavy šablony.
   > 

5. Klikněte na **Uložit**.

> [!IMPORTANT]
> Pokud chcete toocustomize hello auditovat události, můžete k tomu pomocí prostředí PowerShell nebo rozhraní REST API - najdete v části hello [automatizace (PowerShell / REST API)](sql-database-auditing.md#subheading-7) další podrobnosti.
>

## <a name="enable-sql-database-threat-detection"></a>Povolení detekce hrozeb databáze SQL

Detekce hrozeb poskytuje novou vrstvu zabezpečení, což umožňuje zákazníkům toodetect a toopotential hrozeb reagovat, když k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity. Uživatele můžete prozkoumat hello podezřelé události pomocí toodetermine auditování databáze SQL, pokud budou výsledkem pokus o tooaccess, porušení nebo využívat data v databázi hello. Detekce hrozeb je jednoduchý tooaddress potenciální hrozby toohello databáze bez hello nutné toobe expert zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.
Například detekce hrozeb zjistila určité nezvyklé databázové aktivity, které indikují potenciální pokusu o Injektáž SQL. Injektáž SQL je jedním z hello běžné webové aplikace problémy se zabezpečením na Internetu, použít tooattack datové aplikace hello. Útočníci využít výhod aplikace ohrožení zabezpečení tooinject škodlivý příkazů SQL do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi hello.

1. Přejděte okno Konfigurace toohello hello chcete toomonitor databáze SQL. V okně Nastavení hello, vyberte **auditování a detekce hrozeb**.

    ![Navigační podokno](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. V hello **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby hello.

3. Zapnout **ON** detekce hrozby.

4. Konfigurace seznamu hello e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklé databázové aktivity.

5. Klikněte na tlačítko **Uložit** v hello **auditování a detekce hrozeb** okno toosave hello nové nebo aktualizované auditování a hrozeb zásady detekce.

    ![Navigační podokno](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    Pokud se zjistila nezvyklé databázové aktivity, obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity. e-mailu Hello se poskytují informace o událostí hello podezřelé zabezpečení včetně hello povaha hello neobvyklé aktivity, název databáze, čas serveru název a hello události. Kromě toho se budou poskytují informace o možných příčin a doporučená akce tooinvestigate a zmírnit hello potenciální hrozby toohello databáze. Hello další kroky procházení měli byste prostřednictvím co toodo budete dostávat e-mailu:

    ![E-mailu detekce hrozeb](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. V e-mailu hello, klikněte na hello **protokolu auditování databáze SQL Azure** odkaz, který bude spusťte hello portál Azure a zobrazit relevantní auditování záznamy hello době hello hello podezřelé události.

    ![Záznamy auditu](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Klikněte na tooview záznamy auditu hello další podrobnosti o hello databáze podezřelé aktivity, například příkazy SQL, selhání důvod a klient IP.

    ![Podrobnosti záznamu](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. V okně hello auditování záznamy, klikněte na tlačítko **otevřít v aplikaci Excel** tooimport šablony a spuštění hlubší analýzu protokolů auditu hello době hello hello podezřelé události v aplikaci excel tooopen předem nakonfigurovaná.

    > [!NOTE]
    > V aplikaci Excel 2010 nebo novější, Power Query a hello **rychle zkombinovat** nastavení je povinné.

    ![Otevřené záznamy v aplikaci Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. tooconfigure hello **rychle zkombinovat** nastavení - hello **POWER QUERY** pásu karet vyberte **možnosti** dialogovém okně Možnosti toodisplay hello. Vyberte část hello ochrany osobních údajů a zvolte hello druhá možnost - 'Ignorovat hello úrovně ochrany osobních údajů a potenciálně tak vylepšit výkon':

    ![Kombinování rychlé aplikace Excel](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. protokoly auditu tooload SQL, zajistěte, aby hello parametry v kartě nastavení hello jsou správně nastavena a poté vyberte pásu karet "Data" hello a klikněte na tlačítko "Aktualizovat všechny" hello.

    ![Parametry aplikace Excel](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. Hello výsledky se zobrazí v hello **protokoly auditu SQL** listu, která vám umožní toorun hlubší analýzu hello neobvyklé aktivity, které byly zjištěny a zmírnit dopad hello hello událostí zabezpečení ve vaší aplikaci.


## <a name="next-steps"></a>Další kroky
Hello ochranu databáze před uživateli se zlými úmysly a neoprávněným přístupem pomocí několika jednoduchých kroků můžete zvýšit. V tomto kurzu jste postup: 

> [!div class="checklist"]
> * Nastavte pravidla firewallu pro databázi serveru a nebo
> * Připojit databáze tooyour pomocí zabezpečené připojovací řetězec
> * Spravovat přístup uživatelů
> * Chraňte svá data pomocí šifrování
> * Povolit auditování databáze SQL
> * Povolení detekce hrozeb databáze SQL

> [!div class="nextstepaction"]
>[Vylepšení výkonu SQL Database](sql-database-performance-tutorial.md)

