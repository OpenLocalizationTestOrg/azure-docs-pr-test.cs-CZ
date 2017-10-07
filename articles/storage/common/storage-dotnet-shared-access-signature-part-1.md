---
title: "aaaUsing sdílené přístupové podpisy (SAS) ve službě Azure Storage | Microsoft Docs"
description: "Přečtěte si toouse sdíleného přístupu podpisy (SAS) toodelegate přístup tooAzure prostředků úložiště, včetně objektů BLOB, fronty, tabulky a soubory."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 46fd99d7-36b3-4283-81e3-f214b29f1152
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2017
ms.author: marsma
ms.openlocfilehash: 5b75a3c25bcfb9f1ceb81f31dc2d42376bd105cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-shared-access-signatures-sas"></a>Použití sdílených přístupových podpisů (SAS)

Sdílený přístupový podpis (SAS) vám poskytne způsob toogrant omezený přístup tooobjects ve vašem účtu úložiště tooother klientů bez vystavení klíč účtu. V tomto článku jsme poskytovat přehled o modelu hello SAS, SAS osvědčené postupy a podívejte se na některé příklady.

Další příklady kódu pomocí SAS nad rámec těch, které jsou tu popsané, najdete v části [Začínáme s Azure Blob Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) a ostatních vzorků, které jsou k dispozici v hello [ukázky kódu Azure](https://azure.microsoft.com/documentation/samples/?service=storage) knihovny. Můžete stáhnout hello ukázkové aplikace a jejich spuštění nebo procházet kód hello na Githubu.

## <a name="what-is-a-shared-access-signature"></a>Co je sdílený přístupový podpis?
Sdílený přístupový podpis poskytuje Delegovaný přístup tooresources ve vašem účtu úložiště. S SAS můžete udělit, že klienti přístup k tooresources ve vašem účtu úložiště bez sdílení klíče účtu. Toto je použití sdílených přístupových podpisů ve svých aplikacích – SAS hello klíče bod je bezpečný tooshare svým prostředkům úložiště bez kompromisů klíče účtu.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

SAS vám poskytuje podrobnou kontrolu nad hello typ přístupu, kterou byste udělit tooclients, kteří mají hello SAS, včetně:

* interval Hello, přes které hello SAS platný, včetně hello počáteční čas a čas vypršení platnosti hello.
* Hello oprávnění udělují hello SAS. Například SAS pro objekt blob může udělit pro čtení a zápisu objektů blob toothat oprávnění, ale oprávnění k odstranění.
* Volitelné IP adresu nebo rozsah IP adres, od kterých bude přijímat Azure Storage hello SAS. Například můžete určit rozsah IP adres, které patří tooyour organizace.
* Hello protokol, přes který bude přijímat Azure Storage hello SAS. Můžete použít tento volitelný parametr tooclients přístup toorestrict pomocí protokolu HTTPS.

## <a name="when-should-you-use-a-shared-access-signature"></a>Když budete používat sdílený přístupový podpis?
SAS můžete použít, pokud chcete přístup tooresources tooprovide v klientské tooany účtu úložiště není každý má váš účet úložiště přístupové klíče. Váš účet úložiště zahrnuje jak primární a sekundární přístupový klíč, které obě udělte účtu přístup pro správu tooyour a všechny prostředky v něm. Vystavení některý z nich otevře toohello možnost škodlivý nebo nedbalosti použití vašeho účtu. Sdílené přístupové podpisy zadejte bezpečné alternativu, která umožňuje klientům tooread, zápisu a odstranění dat ve vašem účtu úložiště podle toohello oprávnění, které jste explicitně udělí oprávnění a bez nutnosti klíč účtu.

Běžný scénář, kde je užitečné SAS je služba, kde uživatelé pro čtení a zápis účet úložiště tooyour svá vlastní data. Ve scénáři, kde účet úložiště ukládá data o uživatele existují dva vzory typické návrhu:

1. Klienti nahrávání a stahování dat přes službu proxy serveru front-end, který provádí ověřování. Tuto službu front-endu proxy musí výhod hello povolení ověření obchodní pravidla, ale pro velké objemy dat nebo vysoký počet transakcí, vytváření služby, který dokáže vyhovět toomatch vyžádání může být nákladné nebo obtížná.

  ![Diagram scénáře: Front-end proxy služby](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-fe-proxy-service.png)   

1. Jednoduché služby ověří hello klienta podle potřeby a pak vygeneruje SAS. Po hello klient obdrží hello SAS, můžete přímo s definována hello SAS a pro interval hello povolenou hello SAS hello oprávnění přístupu k prostředkům účet úložiště. Hello SAS snižuje potřebu hello směrování všechna data prostřednictvím hello proxy front-endové služby.

  ![Diagram scénáře: SAS zprostředkovatel služby](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-provider-service.png)   

Mnoho reálných služby mohou používat hybridní tyto dva přístupy. Například některá data mohou zpracovat a ověřit prostřednictvím hello front-end proxy serveru, zatímco jiné dat je uložit nebo číst přímo pomocí SAS.

Kromě toho budete potřebovat toouse objekt SAS tooauthenticate hello zdroje v operaci kopírování v některých scénářích:

* Pokud kopírujete objekt blob tooanother objektů blob, který se nachází v jiný účet úložiště, musíte použít SAS tooauthenticate hello zdrojový objekt blob. Volitelně můžete SAS tooauthenticate hello cílového objektu blob i.
* Při kopírování souboru tooanother soubor, který se nachází v jiný účet úložiště, musíte použít SAS tooauthenticate hello zdrojového souboru. Volitelně můžete SAS tooauthenticate hello cílového souboru i.
* Při kopírování souboru tooa blob nebo soubor tooa objekt blob, musíte použít objekt SAS tooauthenticate hello zdroje i v případě, že hello zdrojové a cílové objekty jsou umístěny v rámci hello stejný účet úložiště.

## <a name="types-of-shared-access-signatures"></a>Druhy sdílených přístupových podpisů
Můžete vytvořit dva druhy sdílených přístupových podpisů:

* **SAS služby.** Delegáti SAS služby Hello přístup k prostředku tooa jen v jedné služby úložiště hello: hello služby objektů Blob, fronty, tabulka nebo souboru. V tématu [vytváření SAS služby](https://msdn.microsoft.com/library/dn140255.aspx) a [příklady SAS služby](https://msdn.microsoft.com/library/dn140256.aspx) podrobné informace o vytváření hello token SAS služby.
* **SAS účtu.** Delegáti SAS účtu Hello přístup tooresources v jedné nebo více služeb úložiště hello. Všechny operace hello dostupný přes SAS služby jsou dostupné prostřednictvím SAS účtu. Kromě toho s účtem hello SAS, můžete delegovat přístup toooperations, které se vztahují tooa danou službu, jako například **vlastnosti služby Get/Set** a **získat statistiky služby**. Můžete taky delegovat přístup tooread, zápisu a operace odstranění pro kontejnery objektů blob, tabulky, fronty a sdílených složek, které se nedá vymezit přes SAS služby. V tématu [vytváření SAS účtu](https://msdn.microsoft.com/library/mt584140.aspx) podrobné informace o vytváření tokenu SAS účtu hello.

## <a name="how-a-shared-access-signature-works"></a>Jak funguje sdílený přístupový podpis
Sdílený přístupový podpis je podepsaný identifikátor URI, který ukazuje tooone nebo další prostředky úložiště a obsahuje token, který obsahuje speciální sadu parametry dotazu. Hello token Určuje, jak hello může přistupovat k prostředkům klientem hello. Jeden z parametrů dotazu hello, hello podpis, se vytvářejí na základě parametrů hello SAS a podepsaný pomocí hello klíč účtu. Tento podpis používá Azure Storage tooauthenticate hello SAS.

Tady je příklad identifikátor URI SAS, identifikátor URI prostředku hello zobrazující a tokenu SAS hello:

![Komponenty identifikátoru URI SAS](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-uri.png)   

Hello SAS token je řetězec vygenerovat v hello *klienta* straně (viz hello [příklady SAS](#sas-examples) části Příklady kódu). Token SAS, který generovat s hello Klientská knihovna pro úložiště, například není sledována úložiště Azure žádným způsobem. Můžete vytvořit neomezený počet tokeny SAS na straně klienta hello.

Pokud klient zajišťuje tooAzure identifikátor URI SAS úložiště jako součást požadavku, služba hello zkontroluje hello SAS parametry a podpis tooverify, zda je platný pro ověření žádosti hello. Pokud služba hello ověřuje, že je tento podpis hello platný, je žádost hello ověřit. Jinak je odmítnuta žádost hello s kódem chyby 403 (zakázáno).

## <a name="shared-access-signature-parameters"></a>Sdílený přístupový podpis parametry
SAS účtu Hello tokeny SAS služby zahrnují některé společné parametry a také provést několik parametrů, aby se liší.

### <a name="parameters-common-tooaccount-sas-and-service-sas-tokens"></a>Společné parametry tooaccount SAS a tokeny SAS služby
* **Verze rozhraní API** volitelný parametr, který určuje hello úložiště verze toouse tooexecute hello žádost o služby.
* **Verze služby** povinný parametr, který určuje hello úložiště verze toouse tooauthenticate hello žádost o služby.
* **Počáteční čas.** Toto je hello čas, na které hello SAS vstupuje v platnost. čas zahájení Hello pro sdílený přístupový podpis je volitelný. Pokud čas spuštění je vynechán, hello SAS je hned platná. Hello počáteční čas musí být vyjádřena v UTC (Coordinated Universal Time), s speciální označení UTC ("Z"), například `1994-11-05T13:15:30Z`.
* **Čas vypršení platnosti.** Toto je hello čas, po jejímž uplynutí hello SAS již není platný. Osvědčené postupy doporučujeme, abyste zadejte čas vypršení platnosti pro SAS, nebo přidružit k zásadám uložené přístup. Hello čas vypršení platnosti musí být vyjádřena v UTC (Coordinated Universal Time), s speciální označení UTC ("Z"), například `1994-11-05T13:15:30Z` (Další informace níže).
* **Oprávnění.** Hello oprávnění určená v hello SAS znamenat jakým operacím hello klienta můžete provést u prostředků úložiště hello pomocí hello SAS. K dispozici oprávnění se liší pro SAS účtu a SAS služby.
* **IP.** Volitelný parametr, který určuje IP adresu nebo rozsah IP adres mimo Azure (části hello [stav konfigurace relace směrování](../../expressroute/expressroute-workflows.md#routing-session-configuration-state) pro Express Route) z které tooaccept požadavky.
* **Protokol.** Volitelný parametr, který určuje protokol, hello povolené pro žádost. Možné hodnoty jsou protokolů HTTPS a HTTP (`https,http`), který je pouze hello výchozí hodnotu, nebo HTTPS (`https`). Všimněte si, že HTTP jenom není povolenou hodnotu.
* **Podpis.** podpis Hello se vytvářejí na základě hello další parametry, zadaný jako součást tokenu a pak se zašifrují. Použije se tooauthenticate hello SAS.

### <a name="parameters-for-a-service-sas-token"></a>Parametry pro token SAS služby
* **Úložiště prostředků.** Úložiště prostředků, pro které můžete delegovat přístup se službou SAS patří:
  * Kontejnery a objekty BLOB
  * Sdílených složek a souborů
  * Fronty
  * Tabulky a rozsahy adres entity tabulky.

### <a name="parameters-for-an-account-sas-token"></a>Parametry pro token SAS účtu
* **Služba nebo služby.** SAS účtu můžete delegovat přístup tooone nebo více služeb úložiště hello. Například můžete vytvořit účet SAS, delegáti přístup toohello služby objektů Blob a souboru. Nebo můžete vytvořit SAS, delegáti přístup tooall čtyři služby (objekt Blob, fronty, tabulky a soubor).
* **Typy prostředků úložiště.** SAS účtu vztahuje tooone nebo další třídy prostředků úložiště, nikoli konkrétní prostředek. Můžete vytvořit účtu SAS toodelegate přístup do:
  * API úrovně služeb, které se nazývají proti prostředků účtu úložiště hello. Mezi příklady patří **vlastnosti služby Get/Set**, **získat statistiky služby**, a **seznamu kontejnery nebo fronty nebo tabulky nebo sdílené složky**.
  * Kontejner úrovni rozhraní API, které se nazývají u hello kontejneru objektů pro každou službu: blob kontejnery, front, tabulky a sdílené složky. Mezi příklady patří **vytvoření nebo odstranění kontejneru**, **vytvoření nebo odstranění fronty**, **vytvoření nebo odstranění tabulky**, **vytvoření nebo odstranění sdílené složky**a  **Seznam objektů BLOB nebo souborů a adresářů**.
  * API úrovni objektů, které se nazývají proti objekty BLOB, fronty zpráv, entity tabulky a soubory. Například **Put Blob**, **dotazu Entity**, **získat zprávy**, a **vytvořit soubor**.

## <a name="examples-of-sas-uris"></a>Příklady identifikátorů URI SAS

### <a name="service-sas-uri-example"></a>Příklad identifikátor URI SAS služby

Tady je příklad služby identifikátor URI SAS, která poskytuje čtení a zápisu objektů blob tooa oprávnění. Tabulka Hello rozpis jednotlivých součástí hello URI toounderstand jak přispívá toohello SAS:

```
https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2015-04-05&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D
```

| Name (Název) | Část SAS | Popis |
| --- | --- | --- |
| Identifikátor URI objektu BLOB |`https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt` |Adresa Hello hello objektů blob. Všimněte si, že pomocí protokolu HTTPS se důrazně doporučuje. |
| Verze služby úložiště |`sv=2015-04-05` |Pro úložiště služby verze 2012-02-12 a novější, tento parametr určuje toouse verze hello. |
| Čas spuštění |`st=2015-04-29T22%3A18%3A26Z` |Zadat ve formátu času UTC. Pokud chcete hello SAS toobe platný okamžitě, vynechejte hello počáteční čas. |
| Čas vypršení platnosti |`se=2015-04-30T02%3A23%3A26Z` |Zadat ve formátu času UTC. |
| Prostředek |`sr=b` |prostředek Hello je objekt blob. |
| Oprávnění |`sp=rw` |Hello oprávnění udělují hello SAS zahrnují Read (r) a zápis (w). |
| Rozsah IP adres |`sip=168.1.5.60-168.1.5.70` |Hello rozsah IP adres, ze kterých budou přijímány žádost. |
| Protocol (Protokol) |`spr=https` |Jsou povoleny pouze požadavky pomocí protokolu HTTPS. |
| Podpis |`sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D` |Použít tooauthenticate přístup toohello objektů blob. podpis Hello je klíčem HMAC, vypočítán na řetězec k podepsání a klíč pomocí algoritmus SHA256 hello a pak zakódován pomocí kódování Base64. |

### <a name="account-sas-uri-example"></a>Příklad identifikátor URI pro SAS účtu

Tady je příklad účtu SAS, hello používá stejné společné parametry na hello tokenu. Vzhledem k tomu, že tyto parametry jsou popsané výše, nejsou popsané v tomto poli. Pouze hello parametry, které jsou specifické tooaccount SAS jsou popsány v tabulce hello.

```
https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&srt=s&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B
```

| Name (Název) | Část SAS | Popis |
| --- | --- | --- |
| Identifikátor URI |`https://myaccount.blob.core.windows.net/?restype=service&comp=properties` |Hello koncový bod služby objektů Blob, s parametry pro získávání vlastnosti služby (Pokud je volána s GET) nebo nastavení vlastností služby (při volání sadou). |
| Služby |`ss=bf` |Hello SAS platí toohello objektů Blob a souborové služby |
| Typy prostředků |`srt=s` |operace na úrovni tooservice se vztahuje Hello SAS. |
| Oprávnění |`sp=rw` |Hello oprávnění udělují přístup tooread a operací zápisu. |

Vzhledem k tomu, že oprávnění jsou omezeny toohello úrovně služby, přístupný operací s Tento SAS jsou **získat vlastnosti objektu Blob služby** (čtení) a **nastavit vlastnosti služby objektů Blob** (zápisu). Ale a jiný zdroj v URI hello stejný token SAS mohou být také příliš použít přístup toodelegate**získat statistiky služby objektů Blob** (načíst).

## <a name="controlling-a-sas-with-a-stored-access-policy"></a>Řízení SAS se zásadami uložené přístupu
Sdílený přístupový podpis může trvat dvě formy:

* **Ad hoc SAS:** při vytváření ad hoc SAS, čas spuštění hello, čas vypršení platnosti a oprávnění pro hello SAS jsou určené v hello identifikátor URI pro SAS (nebo implicitní v případě hello, kde je vynechán čas spuštění). Tento typ SAS se dá vytvořit jako SAS účtu nebo SAS služby.
* **SAS se zásadami přístupu uložené:** zásadu uložené přístupu je definovaný na kontejner prostředku--kontejner objektů blob, tabulky, fronty, nebo sdílení – souborů a může být omezení použité toomanage pro jeden nebo více sdílených přístupových podpisů. Pokud přidružíte SAS se zásadami přístupu uložené, zdědí hello SAS hello omezení – čas spuštění hello, čas vypršení platnosti a--definována pro zásady přístupu hello uložené oprávnění.

> [!NOTE]
> SAS účtu musí být v současné době ad hoc SAS. Uložených přístupu zásady se ještě nepodporují pro SAS účtu.

Hello rozdíl mezi formuláři hello dva je důležité pro jeden klíč scénář: odvolaných certifikátů. Protože identifikátor URI SAS je adresa URL, každý uživatel, který získá hello SAS můžete použít, bez ohledu na to, který byl původně vytvořen. Pokud veřejně publikována SAS, můžete použít každý, vítáme. SAS uděluje přístup tooresources tooanyone každý má dokud jednu ze čtyř akcí se stane:

1. čas vypršení platnosti Hello zadaný na hello dosaženo SAS.
2. čas vypršení platnosti Hello zadaný v zásadách přístupu hello uložené odkazuje hello je dosaženo SAS, (Pokud je odkazována zásadu uložené přístupu a určuje, že čas vypršení platnosti). Tato situace může nastat, protože hello uplyne, nebo protože jste změnili hello uložené zásady přístupu, doba vypršení platnosti v minulosti, hello, což je jedním ze způsobů toorevoke hello SAS.
3. Hello uložené zásady přístupu odkazuje hello odstranění SAS, který je jiný způsob toorevoke hello SAS. Všimněte si, že pokud je znovu vytvořit zásady přístupu hello uložené s přesně hello stejný název, všechny existující SAS tokeny budou znovu platné podle oprávnění toohello přidružen (za předpokladu, že čas vypršení platnosti hello na hello SAS neuplynul) uložené přístup zásad. Pokud jsou záměrné toorevoke hello SAS, mít jistotu toouse jiný název znovu hello zásady přístupu, doba vypršení platnosti v budoucnu hello.
4. Hello klíč účtu, který byl použité toocreate hello SAS vygenerován znovu. Znovu vygenerovat klíč účtu způsobí, že všechny součásti aplikace pomocí tohoto klíče toofail tooauthenticate dokud jste aktualizovány toouse buď hello jiných platný účet nebo hello účet nově obnoveného klíče.

> [!IMPORTANT]
> Sdílený přístupový podpis identifikátor URI je přidružen hello účet klíče používané toocreate hello podpisu a hello související zásady uložené přístupu (pokud existuje). Pokud je zadána žádná zásada uložené přístup, hello pouze způsob toorevoke sdílený přístupový podpis je klíč účtu toochange hello.

## <a name="authenticating-from-a-client-application-with-a-sas"></a>Ověřování z klientské aplikace s SAS
Klienta, která je vlastníkem SAS můžete použít hello SAS tooauthenticate požadavek účet úložiště, pro kterou, které nemají klíče účtu hello. SAS můžete zahrnout do připojovací řetězec nebo použití přímo z odpovídající konstruktor hello nebo metoda.

### <a name="using-a-sas-in-a-connection-string"></a>Pomocí SAS v připojovacím řetězci
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

### <a name="using-a-sas-in-a-constructor-or-method"></a>Pomocí SAS v konstruktoru nebo – metoda
Několik konstruktorů knihovny klienta Azure Storage a přetížení metody nabízejí parametr SAS, aby se můžete ověřit žádost toohello služby pomocí SAS.

Zde identifikátor URI SAS je třeba použít toocreate tooa referenční – objekt blob bloku. Hello SAS poskytuje hello pouze přihlašovacích údajů nezbytných pro požadavek hello. odkaz na objekt blob bloku Hello se pak použije pro operaci zápisu:

```csharp
string sasUri = "https://storagesample.blob.core.windows.net/sample-container/" +
    "sampleBlob.txt?sv=2015-07-08&sr=b&sig=39Up9JzHkxhUIhFEjEH9594DJxe7w6cIRCg0V6lCGSo%3D" +
    "&se=2016-10-18T21%3A51%3A37Z&sp=rcw";

CloudBlockBlob blob = new CloudBlockBlob(new Uri(sasUri));

// Create operation: Upload a blob with hello specified name toohello container.
// If hello blob does not exist, it will be created. If it does exist, it will be overwritten.
try
{
    MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    msWrite.Position = 0;
    using (msWrite)
    {
        await blob.UploadFromStreamAsync(msWrite);
    }

    Console.WriteLine("Create operation succeeded for SAS {0}", sasUri);
    Console.WriteLine();
}
catch (StorageException e)
{
    if (e.RequestInformation.HttpStatusCode == 403)
    {
        Console.WriteLine("Create operation failed for SAS {0}", sasUri);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    else
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}

```

## <a name="best-practices-when-using-sas"></a>Osvědčené postupy při použití SAS
Při použití sdílených přístupových podpisů v aplikacích, je třeba toobe vědět dva potenciální rizika:

* Pokud SAS došlo k úniku, můžete použít libovolný uživatel, který získává, což může potenciálně ohrozit váš účet úložiště.
* Pokud vyprší platnost tooa klientská aplikace a aplikace hello je nelze tooretrieve nové SAS ze služby k dispozici SAS, může být narušen funkčnost hello aplikace.

Hello následující doporučení pro použití sdílených přístupových podpisů můžete zmírnit tato rizika:

1. **Vždycky používají protokol HTTPS** toocreate nebo distribuovat SAS. Pokud SAS je předán přes protokol HTTP a zachycení, útočník provádění útok man-in-the-middle je možné tooread hello SAS a použít jej jako hello zamýšlená uživatel může mít potenciálně nebezpečné citlivá data nebo aby vám umožnil poškození dat podle hello Uživatel se zlými úmysly.
2. **Odkaz na uložený zásady přístupu, kde je to možné.** Udělení přístupu zásady hello možnost toorevoke oprávnění bez nutnosti klíče účtu úložiště hello tooregenerate uložené. Nastavení vypršení platnosti hello na velmi daleko v hello budoucí (nebo nekonečné) a ujistěte se, zda se pravidelně aktualizují toomove dále do budoucích hello.
3. **Použijte ucelené časy vypršení platnosti na ad hoc SAS.** Tímto způsobem i když je ohrožení SAS, je platná pouze pro po krátkou dobu. Tento postup je zvlášť důležité, pokud nemůže odkazovat zásadu uložené přístupu. Ucelené časy vypršení platnosti také omezit hello množství dat, s možností zápisu objektů blob tooa omezením hello čas k dispozici tooupload tooit.
4. **Mají klienti automaticky obnovte hello SAS v případě potřeby.** Klienti musí obnovit hello SAS dobře před vypršením platnosti hello v pořadí tooallow dobu opakovaných pokusů, pokud není k dispozici hello služba poskytující hello SAS. Pokud vaše SAS slouží toobe používá pro malý počet okamžitou, krátkodobou operace, které jsou očekává toobe dokončeny v rámci hello doba vypršení platnosti a potom to může být zbytečné hello SAS není správně toobe obnovit. Ale pokud máte klienta, který je pravidelně zajistit požadavky přes SAS, pak hello možnost vypršení platnosti dodává do play. Hello klíče pozornost je třeba hello toobalance pro hello SAS toobe krátkodobou (jako výše uvedená) s tooensure nutné hello, který hello klient požaduje obnovení již v rané fázi dostatek (tooavoid přerušení kvůli toohello SAS kterým vyprší platnost předchozí toosuccessful obnovení).
5. **Pečlivě se časem spuštění SAS.** Pokud nastavíte čas zahájení hello pro SAS příliš**nyní**, pak z důvodu tooclock zkosení (rozdíly v aktuální čas podle toodifferent počítače), selhání může být dodržen občas pro hello první několik minut. Obecně platí nastavte hello počáteční čas toobe alespoň 15 minut v posledních hello. Nebo si ho nastavit vůbec, což znamená, že platný okamžitě ve všech případech. Hello obecně totéž platí i – čas tooexpiry mějte na paměti, můžete pozorovat až too15 minut od času zkosení v obou směrech na žádnou žádostí. Pro klienty pomocí REST verze předchozí too2012-02-12 hello maximální doba trvání SAS, která neodkazuje na zásadu uložené přístupu je 1 hodinu a všechny zásady zadání dlouhodobější než, se nezdaří.
6. **Buďte konkrétní s toobe hello prostředku získat přístup.** Nejlepším způsobem zabezpečení je tooprovide uživatele s hello minimální požadovaná oprávnění. Když uživatel potřebuje pouze jednu entitu tooa přístup pro čtení, pak jim přidělte jednu entitu toothat přístup pro čtení a přístup není pro čtení, zápisu a odstraňování tooall entity. Navíc pomáhají zmenšit prostor hello škody, pokud SAS dojde k ohrožení bezpečnosti vzhledem k tomu hello SAS má méně hello rukou útočníka.
7. **Pochopte, že váš účet bude účtován na jakékoli využití, včetně udělat pomocí SAS.** Pokud zadáte tooa oprávnění k zápisu objektů blob, můžete zvolit tooupload objekt blob 200GB. Pokud jste dali je také přístup pro čtení, vybírá toodownload ho 10krát, přijímají 2 TB odchozí stojí za vás. Znovu zadejte omezenými oprávněními toohelp zmírnit potenciální akce hello uživatelé se zlými úmysly. Použít krátkodobou SAS tooreduce této hrozby (ale být s vědomím zkosení podél hello koncový čas hodiny).
8. **Ověření dat zapsaných pomocí SAS.** Když klientské aplikace zapíše data účtu úložiště tooyour, mějte na paměti, že mohou být problémy s daty. Pokud vaše aplikace vyžaduje, aby tento data být ověřen nebo oprávnění, než bude připravena toouse, byste měli provést toto ověření po zapsání hello dat, a než bude použit v aplikaci. Tento postup také chrání před poškozený nebo škodlivý data zapisovaný tooyour účtu, uživatel, který správně získali hello SAS nebo uživatelem zneužitím uniklé SAS.
9. **Nepoužívejte vždy SAS.** Někdy hello rizika spojená s konkrétní operaci u vašeho účtu úložiště převáží nad hello výhod SAS. Pro tyto operace vytvoření služby střední vrstvy, který zapíše účet úložiště tooyour po provedení obchodní pravidla ověřování, ověřování a auditování. Někdy je také jednodušší přístup toomanage jinými způsoby. Například pokud chcete toomake všech objektů BLOB v kontejneru veřejně čitelné, můžete použít hello kontejneru Public, místo poskytnutí klienta tooevery SAS pro přístup.
10. **Toomonitor Storage Analytics použijte vaši aplikaci.** Můžete použít protokolování a metriky tooobserve žádné špiček selhání ověřování kvůli výpadku tooan ve vaší SAS poskytovatele služby nebo toohello nechtěnému odebrání zásad přístupu uložené. V tématu hello [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) Další informace.

## <a name="sas-examples"></a>Příklady SAS
Zde jsou některé příklady oba typy sdílené přístupové podpisy SAS účtu a SAS služby.

toorun tyto příklady jazyka C#, je nutné tooreference hello následující balíčky NuGet ve vašem projektu:

* [Azure Storage Client Library for .NET](http://www.nuget.org/packages/WindowsAzure.Storage), verze 6.x nebo novější (toouse SAS účtu).
* [Azure Configuration Manager](http://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager)

Další příklady, které ukazují, jak toocreate a testovací SAS, najdete v části [ukázky kódu Azure pro úložiště](https://azure.microsoft.com/documentation/samples/?service=storage).

### <a name="example-create-and-use-an-account-sas"></a>Příklad: Vytvoření a použití SAS účtu
Hello následující příklad kódu vytvoří účet SAS, který je platný pro hello Blob souborové služby a služby, a poskytuje hello klienta oprávnění pro čtení, zápisu a seznam oprávnění tooaccess úrovni služby rozhraní API. SAS účtu Hello omezuje tooHTTPS protokol hello, takže hello požadavek musí být provedeny s protokolem HTTPS.

```csharp
static string GetAccountSASToken()
{
    // toocreate hello account SAS, you need toouse your shared key credentials. Modify for your account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create a new access policy for hello account.
    SharedAccessAccountPolicy policy = new SharedAccessAccountPolicy()
        {
            Permissions = SharedAccessAccountPermissions.Read | SharedAccessAccountPermissions.Write | SharedAccessAccountPermissions.List,
            Services = SharedAccessAccountServices.Blob | SharedAccessAccountServices.File,
            ResourceTypes = SharedAccessAccountResourceTypes.Service,
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Protocols = SharedAccessProtocol.HttpsOnly
        };

    // Return hello SAS token.
    return storageAccount.GetSharedAccessSignature(policy);
}
```

toouse hello účet SAS tooaccess úrovně služeb rozhraní API pro hello služby objektů Blob, vytvořit objekt Blob klienta pomocí hello SAS a hello koncový bod úložiště objektů Blob pro váš účet úložiště.

```csharp
static void UseAccountSAS(string sasToken)
{
    // Create new storage credentials using hello SAS token.
    StorageCredentials accountSAS = new StorageCredentials(sasToken);
    // Use these credentials and hello account name toocreate a Blob service client.
    CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "account-name", endpointSuffix: null, useHttps: true);
    CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();

    // Now set hello service properties for hello Blob client created with hello SAS.
    blobClientWithSAS.SetServiceProperties(new ServiceProperties()
    {
        HourMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        MinuteMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        Logging = new LoggingProperties()
        {
            LoggingOperations = LoggingOperations.All,
            RetentionDays = 14,
            Version = "1.0"
        }
    });

    // hello permissions granted by hello account SAS also permit you tooretrieve service properties.
    ServiceProperties serviceProperties = blobClientWithSAS.GetServiceProperties();
    Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
    Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
    Console.WriteLine(serviceProperties.HourMetrics.Version);
}
```

### <a name="example-create-a-stored-access-policy"></a>Příklad: Vytvoření zásady uložené přístupu
Hello následující kód vytvoří zásadu uložené přístup do kontejneru. Můžete vytvořit hello přístup zásad toospecify omezení pro SAS na hello kontejneru služby nebo jeho objekty BLOB.

```csharp
private static async Task CreateSharedAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new shared access policy and define its constraints.
    // hello access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When hello start time for hello SAS is omitted, hello start time is assumed toobe hello time when hello storage service receives hello request.
        // Omitting hello start time for a SAS that is effective immediately helps tooavoid clock skew.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
    };

    // Get hello container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add hello new policy toohello container's permissions, and set hello container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

### <a name="example-create-a-service-sas-on-a-container"></a>Příklad: Vytvoření SAS služby na kontejneru
Hello následující kód vytvoří SAS do kontejneru. Pokud je zadaný název hello existující zásady uložené přístup, je přidružen hello SAS tuto zásadu. Pokud je k dispozici žádné uložené přístup zásady, hello kód vytvoří SAS ad-hoc hello kontejneru.

```csharp
private static string GetContainerSasUri(CloudBlobContainer container, string storedPolicyName = null)
{
    string sasContainerToken;

    // If no stored policy is specified, create a new access policy and define its constraints.
    if (storedPolicyName == null)
    {
        // Note that hello SharedAccessBlobPolicy class is used both toodefine hello parameters of an ad-hoc SAS, and
        // tooconstruct a shared access policy that is saved toohello container's shared access policies.
        SharedAccessBlobPolicy adHocPolicy = new SharedAccessBlobPolicy()
        {
            // When hello start time for hello SAS is omitted, hello start time is assumed toobe hello time when hello storage service receives hello request.
            // Omitting hello start time for a SAS that is effective immediately helps tooavoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List
        };

        // Generate hello shared access signature on hello container, setting hello constraints directly on hello signature.
        sasContainerToken = container.GetSharedAccessSignature(adHocPolicy, null);

        Console.WriteLine("SAS for blob container (ad hoc): {0}", sasContainerToken);
        Console.WriteLine();
    }
    else
    {
        // Generate hello shared access signature on hello container. In this case, all of hello constraints for the
        // shared access signature are specified on hello stored access policy, which is provided by name.
        // It is also possible toospecify some constraints on an ad-hoc SAS and others on hello stored access policy.
        sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

        Console.WriteLine("SAS for blob container (stored access policy): {0}", sasContainerToken);
        Console.WriteLine();
    }

    // Return hello URI string for hello container, including hello SAS token.
    return container.Uri + sasContainerToken;
}
```

### <a name="example-create-a-service-sas-on-a-blob"></a>Příklad: Vytvoření SAS služby na objekt blob
Hello následující kód vytvoří SAS na objekt blob. Pokud je zadaný název hello existující zásady uložené přístup, je přidružen hello SAS tuto zásadu. Pokud je k dispozici žádné uložené přístup zásady, hello kód vytvoří SAS ad-hoc u objektu blob hello.

```csharp
private static string GetBlobSasUri(CloudBlobContainer container, string blobName, string policyName = null)
{
    string sasBlobToken;

    // Get a reference tooa blob within hello container.
    // Note that hello blob may not exist yet, but a SAS can still be created for it.
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    if (policyName == null)
    {
        // Create a new access policy and define its constraints.
        // Note that hello SharedAccessBlobPolicy class is used both toodefine hello parameters of an ad-hoc SAS, and
        // tooconstruct a shared access policy that is saved toohello container's shared access policies.
        SharedAccessBlobPolicy adHocSAS = new SharedAccessBlobPolicy()
        {
            // When hello start time for hello SAS is omitted, hello start time is assumed toobe hello time when hello storage service receives hello request.
            // Omitting hello start time for a SAS that is effective immediately helps tooavoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create
        };

        // Generate hello shared access signature on hello blob, setting hello constraints directly on hello signature.
        sasBlobToken = blob.GetSharedAccessSignature(adHocSAS);

        Console.WriteLine("SAS for blob (ad hoc): {0}", sasBlobToken);
        Console.WriteLine();
    }
    else
    {
        // Generate hello shared access signature on hello blob. In this case, all of hello constraints for the
        // shared access signature are specified on hello container's stored access policy.
        sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        Console.WriteLine("SAS for blob (stored access policy): {0}", sasBlobToken);
        Console.WriteLine();
    }

    // Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```

## <a name="conclusion"></a>Závěr
Sdílené přístupové podpisy jsou užitečné pro zajištění tooyour tooclients účet úložiště, který by neměl mít klíč účtu hello omezenými oprávněními. Jako takový jsou sice podstatná součást hello model zabezpečení pro všechny aplikace pomocí Azure Storage. Pokud budete postupovat podle osvědčených postupů hello zde uvedeny, můžete ve vašem účtu úložiště SAS tooprovide větší flexibilitu tooresources přístup bez ohrožení zabezpečení hello vaší aplikace.

## <a name="next-steps"></a>Další kroky
* [Správa toocontainers anonymní přístup pro čtení a objekty BLOB](../blobs/storage-manage-access-to-resources.md)
* [Delegování přístupu se sdíleným přístupovým podpisem](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Představení tabulky a fronty SAS](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
