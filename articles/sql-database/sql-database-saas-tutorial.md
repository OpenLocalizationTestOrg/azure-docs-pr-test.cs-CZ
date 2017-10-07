---
title: "aaaDeploy a seznamte se s hello víceklientské Wingtip SaaS aplikace, která používá Azure SQL Database | Microsoft Docs"
description: "Nasazení a prozkoumejte víceklientské aplikace Wingtip SaaS hello, která demonstruje SaaS vzory pro používání Azure SQL Database."
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: sstein
ms.openlocfilehash: 7c528ee19472d3b8c7a285b2f86013945e8f4bf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-explore-a-multi-tenant-application-that-uses-azure-sql-database---wingtip-saas"></a>Nasazení a prozkoumejte víceklientské aplikace, která používá Azure SQL Database – Wingtip SaaS

V tomto kurzu nasazení a prozkoumejte aplikace Wingtip SaaS hello. aplikace Hello používá databázi za klienta, vzor aplikací SaaS, tooservice několik klientů. aplikace Hello je funkce navrženou tooshowcase databáze SQL Azure, které zjednodušují povolení SaaS scénáře.

Hello pět minut po kliknutí na *nasazení tooAzure* níže uvedené tlačítko je k dispozici více klientů SaaS aplikace, až použití SQL Database a spouštění v cloudu hello. nasazení aplikace Hello s tři ukázkové klienty, každá má svou vlastní databázi, všechny nasazené do elastický fond SQL. Hello aplikace je nasazená tooyour předplatné Azure, která poskytuje úplný přístup tooexplore a pracovat s hello jednotlivé součásti aplikace. Hello aplikace zdrojový kód a správu skriptů jsou k dispozici v hello úložiště WingtipSaaS GitHub.


V tomto kurzu se dozvíte:

> [!div class="checklist"]

> * Jak toodeploy hello Wingtip SaaS aplikace
> * Kde tooget hello zdrojovému kódu aplikace a skripty pro správu
> * O hello servery, fondy a databází, které tvoří aplikace hello
> * Jak klienti jsou namapované tootheir dat pomocí hello *katalogu*
> * Jak tooprovision nového klienta
> * Jak toomonitor klienta aktivity v aplikaci hello

tooexplore různých SaaS návrhu a správu vzorů, [série kurzů související](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials) je k dispozici, stavět na toto počáteční nasazení. Při procházení hello kurzy, podrobně hello poskytuje skripty a zkontrolujte, jak jsou implementované hello různé vzorce SaaS. Krok prostřednictvím hello skriptů v každé kurz toogain lépe pochopili, jak tooimplement hello mnoho SQL Database funkce, které zjednodušují vývoj aplikací SaaS.

## <a name="prerequisites"></a>Požadavky

toocomplete dokončení tohoto kurzu, ujistěte se, hello následující požadavky:

* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-hello-wingtip-saas-application"></a>Nasazení aplikace Wingtip SaaS hello

Nasazení aplikace Wingtip SaaS hello:

1. Kliknutím na hello **nasazení tooAzure** tlačítko otevře šablonu nasazení Wingtip SaaS Azure portálu toohello hello. Šablona Hello vyžaduje dvě hodnoty parametrů; název pro novou skupinu prostředků a uživatelské jméno, která odlišuje od jiných nasazení aplikace Wingtip SaaS hello toto nasazení. dalším krokem Hello poskytuje podrobnosti pro tyto hodnoty nastavení.

   Zda toonote hodnoty přesný hello, které používáte, proveďte, protože budete potřebovat tooenter je do konfigurační soubor později.

   <a href="http://aka.ms/deploywtpapp" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Zadejte požadovaný parametr hodnoty pro nasazení hello:

    > [!IMPORTANT]
    > Některá ověřování a brány firewall serveru jsou pro demonstrační účely záměrně nezabezpečené. **Vytvořit novou skupinu prostředků**a nepoužívejte existující skupiny prostředků, serverům nebo fondů. Tato aplikace nebo všechny prostředky, který vytváří, nepoužívejte pro produkční prostředí. Odstranit tento prostředek skupiny, když jste hotovi s toostop aplikace hello související fakturace.

    * **Skupina prostředků** – vyberte **vytvořit nový** a poskytují **název** pro skupinu prostředků hello. Vyberte **umístění** z rozevíracího seznamu hello.
    * **Uživatel** -některé prostředky, vyžadují názvy, které jsou globálně jedinečné. jedinečnost tooensure pokaždé, když nasadíte aplikaci hello zadejte hodnotu toodifferentiate prostředky, které vytvoříte, z prostředků, které jsou vytvořené nasazení hello Wingtip aplikace. Toouse se doporučuje malou **uživatele** názvem, jako je například vašimi iniciálami plus číslo (například *bg1*) a použít jej v hello název skupiny prostředků (například *wingtip-bg1*). **Uživatele** parametru může obsahovat pouze písmena, číslice a pomlčky (bez mezer). Hello první a poslední znak musí být písmeno nebo číslo (doporučuje se všechny malá).


1. **Nasazení aplikace hello**.

    * Klikněte na tlačítko tooagree toohello podmínky a ujednání.
    * Klikněte na **Koupit**.

1. Monitorování stavu nasazení kliknutím **oznámení** (hello ikonu zvonku napravo od pole hledání hello). Nasazení aplikace Wingtip SaaS hello trvá přibližně pět minut.

   ![nasazení bylo úspěšné](media/sql-database-saas-tutorial/succeeded.png)

## <a name="download-and-unblock-hello-wingtip-saas-scripts"></a>Stáhněte si a odblokování hello Wingtip SaaS skriptů

Když je nasazení aplikace hello, stáhněte si hello zdrojový kód a správu skripty.

> [!IMPORTANT]
> Spustitelný soubor obsah (skripty, knihovny DLL) mohou být blokovány Windows, když jsou soubory zip stažené z externího zdroje a rozbalené. Při extrahování hello skripty ze souboru zip, postupujte podle kroků hello níže soubor .zip hello toounblock před extrahování. Tím se zajistí, že hello skriptů jsou povolené toorun.

1. Procházet příliš[úložiště github Wingtip SaaS hello](https://github.com/Microsoft/WingtipSaaS).
1. Klikněte na tlačítko **klonovat nebo stáhnout**.
1. Klikněte na tlačítko **stáhnout ZIP** a uložte soubor hello.
1. Klikněte pravým tlačítkem na hello **WingtipSaaS-master.zip** soubor a vyberte **vlastnosti**.
1. Na hello **Obecné** vyberte **Odblokovat**a klikněte na tlačítko **použít**.
1. Klikněte na **OK**.
1. Extrahujte soubory hello.

Skripty, které jsou umístěné v hello *... \\WingtipSaaS hlavní\\Learning moduly* složky.

## <a name="update-hello-configuration-file-for-this-deployment"></a>Aktualizovat hello konfigurační soubor pro toto nasazení

Před spuštěním všech skriptů, nastavte hello *skupiny prostředků* a *uživatele* hodnoty v **UserConfig.psm1**. Nastavení těchto proměnných toohello hodnoty, které můžete zadat během nasazování.

1. Otevřete... \\Learning moduly\\*UserConfig.psm1* v hello *prostředí PowerShell ISE*
1. Aktualizace *ResourceGroupName* a *název* s konkrétními hodnotami hello pro vaše nasazení (na řádky 10 a 11 pouze).
1. Uložte změny hello!

Toto nastavení tady jednoduše nebude nutné tooupdate tyto hodnoty specifické pro nasazení v každé skriptu.

## <a name="run-hello-application"></a>Spuštění aplikace hello

aplikace Hello umožňující prezentovat místa, například vzájemné součinnosti místnosti, jazz kluby sportovní kluby, který hostitelem události. Místa, zaregistrujte se jako zákazníky (nebo klienty) hello Wingtip platformu, pro události toolist snadný způsob a prodej lístků. Každý místo získá toomanage přizpůsobené webové aplikace a seznamu svoje události a prodej lístků, nezávislé a izolované od ostatních klientů. V části zahrnuje hello každý klient získá databázi SQL, který je nasazený do elastický fond SQL.

Centrálního **události rozbočovače** poskytuje seznam klientů nasazení konkrétní tooyour adresy URL.

1. Otevřete hello _události rozbočovače_ ve webovém prohlížeči: http://events.wtp.&lt; Uživatel&gt;. trafficmanager.net (nahraďte názvem vašeho nasazení uživatelské jméno):

    ![centrum akcí](media/sql-database-saas-tutorial/events-hub.png)

1. V *Centru akcí* klikněte na **Fabrikam Jazz Club**.

   ![Události](./media/sql-database-saas-tutorial/fabrikam.png)


distribuce hello toocontrol příchozí požadavky a používá aplikace hello [ *Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview). stránky Hello události, které jsou specifické pro klienta, vyžadují názvy klienta jsou součástí adresy URL hello. Všechny hello klienta adresy URL zahrnují konkrétní *uživatele* hodnotu a použijte tento formát: http://events.wtp.&lt; Uživatel&gt;.trafficmanager.net/*fabrikamjazzclub*. události aplikace Hello analyzuje hello název klienta z adresy URL hello a používá je toocreate klíče tooaccess katalogu pomocí [ *horizontálního oddílu mapy správu*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-scale-shard-map-management). mapy katalogu Hello hello umístění databáze toohello klíče klienta. Hello **události rozbočovače** používá rozšířené metadata v názvu hello katalogu tooretrieve hello klienta, které jsou spojené s každou databázi tooprovide hello seznam adres URL.

V produkčním prostředí, obvykle vytvořili byste záznam CNAME DNS do [ *nasměrování internetové domény společnosti* ](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) pro profil správce provozu hello.

## <a name="start-generating-load-on-hello-tenant-databases"></a>Spuštění generování zatížení databáze klienta hello

Teď, když hello je aplikace nasazená, přidejme ho toowork! Hello *ukázku LoadGenerator* zatížení spuštěným pro všechny databáze klienta spustí skript prostředí PowerShell. Hello reálného zatížení na velký počet aplikací SaaS je obvykle ojediněle a nepředvídatelným. toosimulate tento typ zatížení, generátor hello vytvoří zatížení distribuovaná do všech klientů s náhodnou shluky na každého klienta, ke kterým dochází v náhodnou intervalech. Z tohoto důvodu trvá několik minut, než tooemerge vzor zatížení hello proto je nejlepší generátor hello toolet spustit pro alespoň tři nebo čtyři minut před monitorování hello zatížení.

1. V hello *prostředí PowerShell ISE*, otevřete hello... \\Learning moduly\\nástroje\\*ukázku LoadGenerator.ps1* skriptu.
1. Stiskněte klávesu **F5** a spusťte skript hello spustit hello zatížení generátor (ponechejte hello výchozí hodnoty parametrů pro nyní).

> [!IMPORTANT]
> toorun jiné skripty, otevřete nové okno prostředí PowerShell ISE. Generátor zatížení Hello běží jako řadu úloh v místní relace prostředí PowerShell. Hello *ukázku LoadGenerator.ps1* skript se spustí hello vlastní operaci načtení generátor skript, který se spouští jako úlohu popředí plus řadu úloh na pozadí zatížení generace. Pro všechny databáze zaregistrované v katalogu hello je volána úlohu generátor zatížení. Hello úlohy běží v místní relace prostředí PowerShell, takže ukončení relace prostředí PowerShell hello zastaví všechny úlohy. Pokud přepnete na váš počítač, generování zatížení je pozastavený a bude pokračovat, až budete probouzet svůj počítač.

Jakmile hello zatížení generátor vyvolá zatížení generování úlohy pro každého klienta, hello popředí úkol zůstane v vyvolání úlohy stavu, kdy začne další úlohy pro jakékoli nové klienty, které jsou následně zřízené. Můžete použít *Ctrl-C* nebo stiskněte klávesu hello *Zastavit* tlačítko toostop hello popředí úloh, ale stávající pozadí úloh bude pokračovat generování zatížení na každou databázi. Pokud potřebujete toomonitor a řídit hello úlohy na pozadí, použijte *Get-Job*, *Receive-Job* a *úlohu zastavení*. Když hello popředí úloha běží nelze použít hello stejné tooexecute relace prostředí PowerShell jiné skripty. toorun jiné skripty, otevřete nové okno prostředí PowerShell ISE.

Pokud chcete toorestart hello zatížení generátor relace, například s odlišnými parametry, můžete zastavit hello popředí úloh a poté znovu spusťte hello *ukázku LoadGenerator.ps1* skriptu. Opětovné spuštění *ukázku LoadGenerator.ps1* nejprve zastaví všechny aktuálně spuštěné úlohy a potom restartuje hello generátor, který se spustí novou sadu úloh pomocí hello aktuální parametry.

Nyní ponechte generátor zatížení hello spuštěna ve stavu úlohy vyvolání hello.


## <a name="provision-a-new-tenant"></a>Zřízení nového tenanta

počáteční nasazení Hello vytvoří tři ukázkové klienty, ale vytvoříme jiného klienta toosee jak to ovlivní hello nasazené aplikace. pracovní postup Hello Wingtip SaaS zřizování klientů je podrobně popsaná v hello [kurzu zřizování a katalog](sql-database-saas-tutorial-provision-and-catalog.md). V tomto kroku rychle vytvořit nového klienta.

1. Otevřete... \\Modules\Provision učení a katalog\\*ukázku ProvisionAndCatalog.ps1* v hello *prostředí PowerShell ISE*.
1. Stiskněte klávesu **F5** pro spuštění skriptu hello (ponechejte hello výchozí hodnoty pro nyní).

   > [!NOTE]
   > Použít mnoho skriptů Wingtip SaaS *$PSScriptRoot* tooallow navigace funkce toocall složky v jiné skripty. Tato proměnná je Vyhodnocená jenom když hello skript se spustí, stisknutím klávesy **F5**.  Zvýraznění a spuštění výběr (**F8**) může vést k chybám, takže stiskněte **F5** při spouštění skriptů.

Nová databáze klienta Hello je vytvořená v elastický fond SQL, inicializovat a zaregistrovaná v katalogu hello. Po úspěšném zřízení nového klienta hello je prodávané ticket *události* server se zobrazí v prohlížeči:

![Nový tenant](./media/sql-database-saas-tutorial/red-maple-racing.png)

Aktualizujte hello *události rozbočovače* a hello nového klienta se teď zobrazí v seznamu hello.


## <a name="explore-hello-servers-pools-and-tenant-databases"></a>Prozkoumejte hello servery, fondy a klienta databází

Teď, když jste spuštění, spuštění zátěžového proti hello kolekci klientů, podíváme se na některé hello prostředky, které byly nasazené:

1. V [portál Azure](http://portal.azure.com), procházet seznam tooyour systému SQL Server a otevřete **katalogu -&lt;uživatele&gt;**  serveru. server Hello katalogu obsahuje dvě databáze. Hello **tenantcatalog**a hello **basetenantdb** (prázdnou *zlaté* nebo šablonu databáze, která je zkopírovaných toocreate nové klienty).

   ![databáze](./media/sql-database-saas-tutorial/databases.png)

1. Přejděte zpět tooyour seznam serverů SQL a otevřete hello **tenants1 -&lt;uživatele&gt;**  serveru, který obsahuje hello klienta databází. Každá databáze klienta je _elastické standardní_ databáze ve fondu standardní 50 eDTU. Také Všimněte si, že je _Red javor Racing_ databáze, databáze klienta hello dříve jste zřídili.

   ![server](./media/sql-database-saas-tutorial/server.png)

## <a name="monitor-hello-pool"></a>Monitorování hello fondu

Pokud generátor zatížení hello běží několik minut, dostatek data by měla být k dispozici toostart prohlížení některé možnosti, které jsou integrovány do fondů a databází monitorování hello.

1. Procházet toohello server **tenants1 -&lt;uživatele&gt;**a klikněte na tlačítko **Pool1** zobrazení využití prostředků pro fond hello (generátor zatížení hello spustili jednu hodinu v hello následující grafy) :

   ![monitorování fondu](./media/sql-database-saas-tutorial/monitor-pool.png)

Na těchto dvou grafech je krásně vidět, jak dobře se elastické fondy a databáze SQL hodí pro úlohy SaaS aplikací. Čtyř databází, které jsou všechny bursting tooas tolik, jako jsou snadno není podporována 40 Edtu 50 eDTU fondu. Pokud byly zřízení jako samostatné databáze jejich by každý toobe nutné S2 (50 DTU) toosupport hello shluky. Hello náklady 4 samostatné S2 databází by měly být téměř 3krát hello cena hello fondu a hello fondu má stále dostatek prostoru pro mnoho více databází. V situacích, reálného databáze SQL zákazníků aktuálně používá až too500 databází ve fondech 200 eDTU. Další informace najdete v tématu hello [kurzu pro monitorování výkonu](sql-database-saas-tutorial-performance-monitoring.md).


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli:

> [!div class="checklist"]

> * Jak toodeploy hello Wingtip SaaS aplikace
> * O hello servery, fondy a databází, které tvoří aplikace hello
> * Klienti jsou namapované tootheir data s hello *katalogu*
> * Jak tooprovision nové klienty
> * Jak toomonitor využití fondu tooview klienta aktivity
> * Jak toodelete ukázkové prostředky toostop související fakturace

Nyní zkuste hello [kurzu zřizování a katalog](sql-database-saas-tutorial-provision-and-catalog.md)



## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které vychází z hello Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* toolearn o elastické fondy, najdete v části [ *co je fondu elastické databáze Azure SQL*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* toolearn o elastické úlohy, najdete v části [ *správu cloudu upraveným databáze*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
* toolearn o víceklientské aplikace SaaS, najdete v části [ *vzory pro víceklientské aplikace SaaS návrhu*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)
