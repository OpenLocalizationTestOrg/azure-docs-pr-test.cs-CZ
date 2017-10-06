---
title: "aaaCreate a obnovit zálohu ve službě BizTalk Services | Microsoft Docs"
description: "BizTalk Services zahrnuje zálohování a obnovení. Zjistěte, jak toocreate a obnovení zálohy a zjistit, co se zálohuje. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 32356ad870678fa5fd5bbbbf13d9377188f770a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Zálohování a obnovení

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Služba Azure BizTalk Services obsahuje funkce zálohování a obnovení. Toto téma popisuje, jak hello toobackup a obnovení služby BizTalk Services pomocí portálu Azure classic.

Můžete také zálohovat služby BizTalk Services pomocí hello [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584). 

> [!NOTE]
> Hybridní připojení se nebudou zálohovány, bez ohledu na to hello Edition. Hybridní připojení, musíte znovu vytvořit.


## <a name="before-you-begin"></a>Než začnete
* Zálohování a obnovení nemusí být dostupné pro všechny edice. V tématu [služby BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md).
* Pomocí hello portál Azure classic, můžete vytvořit zálohu na vyžádání nebo vytvoření naplánovaného zálohování. 
* Zálohování obsah může být obnovené toohello stejné služby BizTalk nebo tooa novou službu BizTalk. toorestore hello služby BizTalk pomocí hello stejný název, hello stávající službu BizTalk musí být odstraněny a hello název musí být k dispozici. Po odstranění služby BizTalk, může trvat delší dobu, než chtěli pro hello stejný název toobe k dispozici. Pokud nemůžete počkat hello stejný název toobe k dispozici, pak obnovit tooa novou službu BizTalk.
* BizTalk Services může být obnovené toohello stejné edice, nebo vyšší verze. Obnovení služby BizTalk Services tooa nižší verzi, z při hello zálohy, není podporováno.
  
    Například zálohy pomocí hello základní verzi může být obnovena toohello edici Premium. Zálohování pomocí hello edice Premium nemůže být obnovena toohello Standard Edition.
* čísla EDI řízení Hello jsou zálohovány toomaintain kontinuity hello řízení čísel. Pokud po poslední záloze hello zpracování zpráv, obnovení tohoto obsahu zálohování může způsobit duplicitní řízení čísla.
* Pokud dávce má aktivní zprávy, zpracování dávky hello **před** spuštěním zálohování. Při vytváření zálohy (jako potřebné nebo plánované), ukládají nikdy zprávy v dávkách. 
  
    **Pokud nedojde k zálohování s active zprávy v dávce, tyto zprávy nejsou zálohovány a proto budou ztraceny.**
* Volitelné: V hello portál služby BizTalk, zastavte č. všechny operace správy.

## <a name="create-a-backup"></a>Vytvoření zálohy
Zálohu můžete provést kdykoli a zcela řídí můžete. Tato část uvádí hello kroky toocreate zálohování pomocí hello Azure classic portálu, včetně:

[Na vyžádání zálohování](#backupnow)

[Plán zálohování](#backupschedule)

#### <a name="backupnow"></a>Na vyžádání zálohování
1. V hello portál Azure classic, vyberte **BizTalk Services**, a pak vyberte hello chcete toobackup služby BizTalk.
2. V hello **řídicí panel** vyberte **zálohování** na hello dolní části stránky hello.
3. Zadejte název zálohy. Zadejte například *myBizTalkService*BU*datum*.
4. Zvolte účet úložiště blob a vyberte hello zaškrtnutí toostart hello zálohování.

Po dokončení zálohování hello, vytvoří se v účtu úložiště hello kontejner s názvem Zálohování hello, které zadáte. Tento kontejner obsahuje konfiguraci zálohování služby BizTalk.

#### <a name="backupschedule"></a>Plán zálohování
1. V hello portál Azure classic, vyberte **BizTalk Services**, vyberte hello název služby BizTalk mají tooschedule hello zálohování a potom vyberte hello **konfigurace** kartě.
2. Sada hello **zálohování stav** příliš**automatické**. 
3. Vyberte hello **účet úložiště** toostore hello zálohování, zadejte hello **frekvence** toocreate hello a jak dlouho tookeep hello zálohování (**dní uchovávání**):
   
    ![][AutomaticBU]
   
    **Poznámky k**     
   
   * V **dní uchovávání**, doba uchování hello musí být větší než četnost záloh hello.
   * Vyberte **vždy vždycky mějte aspoň jednu zálohu**i v případě, že je po dobu uchování hello.
4. Vyberte **Uložit**.

Když spustí naplánované úlohy zálohování, vytvoří kontejner (toostore zálohovaná data) v hello účet úložiště, které jste zadali. Hello název kontejneru hello jmenuje *služby BizTalk název data a času*. 

Pokud se bude zobrazovat hello řídicího panelu služby BizTalk **se nezdařilo** stavu:

![Stav poslední naplánované zálohy][BackupStatus] 

Hello odkaz otevře hello toohelp řešení potíží s protokolů pro operace správy služeb. V tématu [BizTalk Services: řešení problémů pomocí protokolů operací](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Obnovení
Můžete obnovit zálohy z hello portál Azure classic nebo z hello [obnovit REST API služby BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325582). Tato část uvádí toorestore kroky hello pomocí portálu classic hello.

#### <a name="before-restoring-a-backup"></a>Před obnovením zálohy
* Nové sledování, archivace a monitorování úložiště lze zadat během obnovování služby BizTalk.
* Dobrý den, je stejné EDI Runtime data obnovit. zálohování EDI Runtime Hello ukládá hello řízení čísla. čísla řízení Hello obnovení jsou v pořadí od času hello hello zálohy. Pokud po poslední záloze hello zpracování zpráv, obnovení tohoto obsahu zálohování může způsobit duplicitní řízení čísla.

#### <a name="restore-a-backup"></a>Obnovení zálohy
1. V hello portál Azure classic, vyberte **nový** > **App Services** > **služby BizTalk** > **obnovení** :
   
    ![Obnovení zálohy][Restore]
2. V **zálohování URL**vyberte ikonu hello složky a rozbalte hello účtu úložiště Azure, ukládá hello zálohu konfigurace služby BizTalk. Rozbalte kontejner hello a v pravém podokně hello, vyberte odpovídající zálohování souboru .txt hello. 
   <br/><br/>
   Vyberte **otevřete**.
3. Na hello **obnovení služby BizTalk** stránky, zadejte **název služby BizTalk** a ověřte hello **adresa URL domény**, **edice**a **Oblast** pro hello obnovit služby BizTalk. **Vytvoření nové instance databáze SQL** pro hello sledování databáze:
   
    ![][RestoreBizTalkService]
   
    Vyberte šipku další hello.
4. Ověřte název hello hello SQL databáze, zadejte hello fyzický server, kde bude vytvořen hello SQL database a uživatelské jméno a heslo pro tento server.

    Pokud chcete tooconfigure hello SQL databáze edice, velikosti a další vlastnosti, vyberte **nakonfigurovat rozšířené nastavení databáze**. 

    Vyberte šipku další hello.

1. Vytvořit nový účet úložiště nebo zadejte existující účet úložiště pro hello služby BizTalk.
2. Vyberte hello zaškrtnutí toostart hello obnovení.

Po úspěšném dokončení obnovení hello novou službu BizTalk je uvedena v pozastaveném stavu na stránce služby BizTalk Services hello hello portál Azure classic.

### <a name="postrestore"></a>Po obnovení ze zálohy
Hello služba BizTalk je vždy obnovit **pozastaveno** stavu. V tomto stavu můžete provést změny konfigurace před hello nové prostředí je funkční, včetně:

* Pokud jste vytvořili pomocí sady SDK Azure BizTalk Services hello aplikace služby BizTalk, musíte tootooupdate hello řízení přístupu (ACS) přihlašovací údaje v těchto aplikací toowork s prostředím hello obnovit.
* Obnovení služby BizTalk tooreplicate stávajícího prostředí služby BizTalk. V této situaci pokud jsou nakonfigurované služby BizTalk Services portálu původní hello smlouvy, které použijte složku zdroje FTP, může být nutné tooupdate hello smluv v prostředí toouse hello nově obnovit jiné zdrojové složky FTP. Jinak, mohou existovat dvě různé smlouvy při toopull hello stejnou zprávu.
* Pokud jste obnovili toohave prostředí s více služby BizTalk, zkontrolujte, zda že cílíte hello správné prostředí v sadě Visual Studio aplikace hello, rutiny prostředí PowerShell, rozhraní REST API nebo Trading Partner OM rozhraní API pro správu.
* Je dobrým zvykem tooconfigure automatizované v prostředí služby BizTalk hello nově obnovit zálohování.

toostart hello služby BizTalk v hello portál Azure classic, vyberte hello obnovit služby BizTalk a vyberte **obnovit** hello hlavním panelu. 

## <a name="what-gets-backed-up"></a>Co se zálohuje
Při vytváření zálohy hello následující položky jsou zálohovány:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Zálohovaných položek</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Portál Azure BizTalk Services</strong></td>
</tr> 
<tr>
<td>Konfigurace a prostředí Runtime</td> 
<td>
<ul>
<li>Podrobnosti o partnera a profilu</li>
<li>Partner smlouvy</li>
<li>Vlastní sestavení, nasazení</li>
<li>Mosty nasazení</li>
<li>Certifikáty</li>
<li>Transformace nasazení</li>
<li>Kanály</li>
<li>Šablony vytvořili a uložili v hello portál služby BizTalk</li>
<li>X12 mapování ST01 a GS01</li>
<li>Ovládací prvek čísla (EDI)</li>
<li>Hodnoty AS2 zprávy povinná kontrola úrovně Důvěryhodnosti</li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2">
 <strong>Služba Azure BizTalk</strong></td>
</tr> 
<tr>
<td>Certifikát SSL</td> 
<td>
<ul>
<li>Data certifikátu SSL</li>
<li>Heslo certifikátu SSL</li>
</ul>
</td>
</tr> 
<tr>
<td>Nastavení služby BizTalk</td> 
<td>
<ul>
<li>Počet jednotek škálování</li>
<li>Edice</li>
<li>Verze produktu</li>
<li>Oblast nebo Datacenter</li>
<li>Ovládací prvek Service (ACS) přístup k oboru názvů a klíče</li>
<li>Sledování připojovací řetězec databáze</li>
<li>Archivace připojovací řetězce k účtu úložiště</li>
<li>Monitorování připojovací řetězce k účtu úložiště</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Další položky</strong></td>
</tr> 
<tr>
<td>Sledování databáze</td> 
<td>Při vytváření služby BizTalk hello jsou zadány hello sledování databáze podrobnosti, včetně hello Server databází SQL Azure a název databáze sledování hello. Hello sledování databáze není automaticky zálohovat.
<br/><br/>
<strong>Důležité upozornění</strong><br/>
Pokud hello sledování databáze bude odstraněna a hello potřeby databáze obnovena, musí existovat předchozí zálohy. Pokud zálohu neexistuje, hello sledování databáze a jeho data nejsou použitelná pro obnovení. V této situaci, vytvořte novou databázi pro sledování s hello název stejné databáze. Doporučuje se geografická replikace.</td>
</tr> 
</table>

## <a name="next"></a>Další
toocreate Azure BizTalk Services v hello Azure classic přejděte příliš[BizTalk Services: zřízení Azure pomocí portálu classic](http://go.microsoft.com/fwlink/p/?LinkID=302280). vytváření aplikací, přejděte příliš toostart[služby Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Viz také
* [Zálohování služby BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Služba BizTalk obnovit ze zálohy](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Developer, Basic, Standard a Premium tabulka edic](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: Zřízení pomocí Azure portál classic](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Tabulka stavů zřízení](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Karty Řídicí panel, Sledování a Škálování](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Omezování](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Název a klíč vystavitele](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Jak začít používat hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

