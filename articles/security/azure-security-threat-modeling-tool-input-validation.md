---
title: "aaaInput ověření - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "způsoby zmírnění hrozeb v hello nástroj modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 823503881f4bae292ef021834d5e64acf2a0f54a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-input-validation--mitigations"></a>Rámce zabezpečení: Ověřování vstupu | Způsoby zmírnění rizik 
| Produktům a službám | Článek |
| --------------- | ------- |
| **Webové aplikace** | <ul><li>[Zakázat XSLT skriptování všechny transformací pomocí nedůvěryhodné šablony stylů](#disable-xslt)</li><li>[Ujistěte se, že každé stránce, která může obsahovat uživatele ovladatelné obsah výslovný nesouhlas automatické sledování toku dat MIME](#out-sniffing)</li><li>[Posílení zabezpečení nebo zakázat řešení Entity XML](#xml-resolution)</li><li>[Aplikace využívá ovladač http.sys provést ověření kanonizace adresy URL](#app-verification)</li><li>[Zajistěte, aby byl příslušný ovládací prvky jsou zavedené při přijetí soubory od uživatelů](#controls-users)</li><li>[Ujistěte se, jestli je pro přístup k datům ve webové aplikaci používají bezpečnost typů parametrů](#typesafe)</li><li>[Používat samostatný model vazby třídy nebo filtr vazeb vypíše tooprevent MVC velkokapacitního přiřazení ohrožení zabezpečení](#binding-mvc)</li><li>[Kódování předchozí toorendering nedůvěryhodné webové výstup](#rendering)</li><li>[Provedení ověření vstupu a filtrování u všech řetězec typu vlastnosti modelu](#typemodel)</li><li>[Čištění bude použito na pole formuláře, které přijímají všechny znaky, např, bohaté textového editoru](#richtext)</li><li>[Nepřiřazujte toosinks DOM elementy, které nemají integrované kódování](#inbuilt-encode)</li><li>[Ověřit, zda všechny jsou uzavřeny nebo bezpečně provést přesměrování v rámci aplikace hello](#redirect-safe)</li><li>[Implementace ověření vstupu na všechny parametry typu řetězec akceptovat metody Kontroleru](#string-method)</li><li>[Nastavit časový limit horní limit pro regulární výraz zpracování tooprevent DoS z důvodu toobad regulární výrazy](#dos-expression)</li><li>[Nepoužívejte Html.Raw v zobrazení syntaxe Razor](#html-razor)</li></ul> | 
| **Database** | <ul><li>[Nepoužívejte dynamické dotazy v uložené procedury](#stored-proc)</li></ul> |
| **Webové rozhraní API** | <ul><li>[Zajistit, aby ověření modelu pro metody webového rozhraní API](#validation-api)</li><li>[Implementace ověření vstupu na všechny parametry typu řetězec přijata metodami webového rozhraní API](#string-api)</li><li>[Ujistěte se, že bezpečnost typů parametrů se používají v webového rozhraní API pro přístup k datům](#typesafe-api)</li></ul> | 
| **Azure Documentdb** | <ul><li>[Použít umožňující dotazy SQL pro DocumentDB](#sql-docdb)</li></ul> | 
| **WCF** | <ul><li>[Ověření vstupu WCF prostřednictvím vazbou schématu](#schema-binding)</li><li>[Ověření vstupu WCF prostřednictvím parametru kontroly](#parameters)</li></ul> |

## <a id="disable-xslt"></a>Zakázat XSLT skriptování všechny transformací pomocí nedůvěryhodné šablony stylů

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Zabezpečení XSLT](https://msdn.microsoft.com/library/ms763800(v=vs.85).aspx), [XsltSettings.EnableScript vlastnost](http://msdn.microsoft.com/library/system.xml.xsl.xsltsettings.enablescript.aspx) |
| **Kroky** | XSLT podporuje skriptování uvnitř stylů pomocí hello `<msxml:script>` elementu. To umožňuje používat v transformaci XSLT toobe vlastní funkce. Hello skript se spustí v kontextu hello procesu hello provádění hello transformace. XSLT skriptu musí být zakázáno v nedůvěryhodných prostředí spuštění tooprevent nedůvěryhodné kódu. *Pokud pomocí rozhraní .NET:* skriptování XSLT je ve výchozím nastavení zakázané; ale ujistěte se, že ho nebylo povoleno explicitně prostřednictvím hello `XsltSettings.EnableScript` vlastnost.|

### <a name="example"></a>Příklad 

```C#
XsltSettings settings = new XsltSettings();
settings.EnableScript = true; // WRONG: THIS SHOULD BE SET toofalse
```

### <a name="example"></a>Příklad
Pokud používáte pomocí MSXML 6.0, skriptování XSLT je zakázané ve výchozím nastavení; Nicméně je nutné zajistit, aby ho nebylo povoleno explicitně prostřednictvím vlastnosti objektu XML DOM hello AllowXsltScript. 

```C#
doc.setProperty("AllowXsltScript", true); // WRONG: THIS SHOULD BE SET toofalse
```

### <a name="example"></a>Příklad
Pokud používáte MSXML 5 nebo níže, XSLT je povoleno skriptování ve výchozím nastavení je potřeba explicitně zakážete. Nastavte hello XML DOM objektu vlastnost AllowXsltScript toofalse. 

```C#
doc.setProperty("AllowXsltScript", false); // CORRECT. Setting toofalse disables XSLT scripting.
```

## <a id="out-sniffing"></a>Ujistěte se, že každé stránce, která může obsahovat uživatele ovladatelné obsah výslovný nesouhlas automatické sledování toku dat MIME

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [IE8 V části zabezpečení - komplexní ochranu](http://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)  |
| **Kroky** | <p>Pro jednotlivé stránky, která by mohla obsahovat ovladatelné obsah uživatele, je nutné použít hello hlavičky protokolu HTTP `X-Content-Type-Options:nosniff`. toocomply s tímto požadavkem, můžete buď sadu hello požadovaná hlavička pro pouze stránky, které může obsahovat uživatele ovladatelné obsahu stránce stránky, nebo můžete ho nastavit globálně pro všechny stránky v aplikaci hello.</p><p>Každý typ souboru doručit z webového serveru má přidruženou [typ MIME](http://en.wikipedia.org/wiki/Mime_type) (označované taky jako *typu obsahu*), který popisuje hello povaha hello obsah (to znamená, obrázek, text, aplikace, atd.)</p><p>záhlaví Hello X obsah typu možnosti je hlavičky protokolu HTTP, která vývojářům umožní toospecify, že jejich obsah by neměl být MIME zachycení. Tuto hlavičku je navrženou toomitigate sledování toku dat MIME útoky. Přidala se podpora pro tuto hlavičku v aplikaci Internet Explorer 8 (IE8)</p><p>Jenom uživatelé aplikace Internet Explorer 8 (IE8) budou využívat X obsah typu možnosti. Předchozí verze aplikace Internet Explorer nerespektují aktuálně Hlavička X-obsah-typ-Options hello</p><p>Internet Explorer 8 (nebo novější) jsou hello pouze hlavní prohlížeče MIME-sledování toku dat tooimplement výslovný nesouhlas s funkcí. Pokud další hlavní prohlížeče (Firefox, Safari, Chrome) implementovat podobné funkce, toto doporučení budou aktualizované tooinclude syntaxe také tyto prohlížeče</p>|

### <a name="example"></a>Příklad
hello požadovaná hlavička tooenable globálně pro všechny stránky v aplikaci hello, můžete provést jednu z následujících akcí hello: 

* Přidat hlavičku hello v souboru web.config hello, pokud je aplikace hello hostovaná pomocí Internetové informační služby (IIS) 7 

```
<system.webServer> 
  <httpProtocol> 
    <customHeaders> 
      <add name=""X-Content-Type-Options"" value=""nosniff""/>
    </customHeaders>
  </httpProtocol>
</system.webServer> 
```

* Přidat hlavičku hello prostřednictvím hello globální aplikace\_BeginRequest 

``` 
void Application_BeginRequest(object sender, EventArgs e)
{
  this.Response.Headers[""X-Content-Type-Options""] = ""nosniff"";
} 
```

* Implementace vlastního modulu HTTP 

``` 
public class XContentTypeOptionsModule : IHttpModule 
  {
    #region IHttpModule Members 
    public void Dispose() 
    { 

    } 
    public void Init(HttpApplication context)
    { 
      context.PreSendRequestHeaders += newEventHandler(context_PreSendRequestHeaders); 
    } 
    #endregion 
    void context_PreSendRequestHeaders(object sender, EventArgs e) 
      { 
        HttpApplication application = sender as HttpApplication; 
        if (application == null) 
          return; 
        if (application.Response.Headers[""X-Content-Type-Options ""] != null) 
          return; 
        application.Response.Headers.Add(""X-Content-Type-Options "", ""nosniff""); 
      } 
  } 

``` 

* Požadovaná hlavička hello pouze pro konkrétní stránky můžete povolit přidáním tooindividual odpovědí: 

```
this.Response.Headers[""X-Content-Type-Options""] = ""nosniff""; 
``` 

## <a id="xml-resolution"></a>Posílení zabezpečení nebo zakázat řešení Entity XML

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Rozšíření Entity XML](http://capec.mitre.org/data/definitions/197.html), [útoků a obrany XML útok DoS](http://msdn.microsoft.com/magazine/ee335713.aspx), [Přehled zabezpečení MSXML](http://msdn.microsoft.com/library/ms754611(v=VS.85).aspx), [osvědčené postupy pro zabezpečení MSXML kódu](http://msdn.microsoft.com/library/ms759188(VS.85).aspx), [referenci na protokol NSXMLParserDelegate](http://developer.apple.com/library/ios/#documentation/cocoa/reference/NSXMLParserDelegate_Protocol/Reference/Reference.html), [řešení externí odkazy](https://msdn.microsoft.com/library/5fcwybb2.aspx) |
| **Kroky**| <p>I když není využívány často, je funkce XML, který umožňuje tooexpand analyzátor XML hello makro entity s hodnotami, které jsou definované v rámci samotného hello dokumentu nebo z externích zdrojů. Například hello dokument může definovat entity "NázevSpolečnosti" s hodnotou hello "Microsoft", aby pokaždé, když hello text "&companyname;" se zobrazí v dokumentu hello se automaticky nahradí textem hello. Microsoft. Nebo hello dokument může definovat entity "MSFTStock", který odkazuje externí webovou službu toofetch hello aktuální hodnota Microsoft stock.</p><p>Pak kdykoli "&MSFTStock;" se zobrazí v dokumentu hello se automaticky nahradí aktuální uložených cena hello. Tato funkce se však může být překročen toocreate odepření služby (DoS) podmínek. Útočník může vnořit více entit toocreate bomb XML exponenciální rozšíření, které zabírá všechny dostupné paměti v systému hello. </p><p>Alternativně může udělat externího odkazu, který datové proudy zpět nekonečné množství dat nebo která jednoduše přestane reagovat hello přístup z více vláken. Všechny týmy v důsledku toho musíte zakázat interní nebo externí řešení entity XML, zcela, pokud jejich aplikace nemá ho použít, nebo ručně omezit hello množství paměti a dobu, po kterou hello aplikace můžou využívat pro překlad entity, pokud je tato funkce nezbytně nutné. Pokud řešení entity není vyžadován na základě vaší aplikace, potom jej vypněte. </p>|

### <a name="example"></a>Příklad
Pro kód .NET Framework můžete použít hello následujících postupů:

```C#
XmlTextReader reader = new XmlTextReader(stream);
reader.ProhibitDtd = true;

XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);

// for .NET 4
XmlReaderSettings settings = new XmlReaderSettings();
settings.DtdProcessing = DtdProcessing.Prohibit;
XmlReader reader = XmlReader.Create(stream, settings);
```
Všimněte si, že hello výchozí hodnotu `ProhibitDtd` v `XmlReaderSettings` má hodnotu true, ale v `XmlTextReader` je false. Pokud používáte XmlReaderSettings, tooset ProhibitDtd tootrue není nutné explicitně, ale se doporučuje pro zabezpečení saké, abyste provedli. Všimněte si také, že třídy XmlDocument hello umožňuje entity řešení ve výchozím nastavení. 

### <a name="example"></a>Příklad
toodisable entity řešení pro XmlDocuments, použijte hello `XmlDocument.Load(XmlReader)` přetížení hello Load – metoda a nastavte hello příslušné vlastnosti hello XmlReader argument toodisable rozlišení, jak je znázorněno v následujícím kódu hello: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);
XmlDocument doc = new XmlDocument();
doc.Load(reader);
```

### <a name="example"></a>Příklad
Pokud zakázání řešení entity není možné pro vaši aplikaci, nastavte hello XmlReaderSettings.MaxCharactersFromEntities tooa přiměřenou hodnotu vlastnosti podle potřeb tooyour aplikace. To bude omezovat hello dopad útoků DoS potenciální exponenciální rozšíření. Hello následující kód představuje příklad tohoto přístupu: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
XmlReader reader = XmlReader.Create(stream, settings);
```

### <a name="example"></a>Příklad
Pokud třeba tooresolve vložené entity, ale není nutné tooresolve externí entity, nastavte toonull vlastnost XmlReaderSettings.XmlResolver hello. Například: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
settings.XmlResolver = null;
XmlReader reader = XmlReader.Create(stream, settings);
```
Všimněte si, že v MSXML6, ProhibitDTD tootrue (zakázání zpracování souboru DTD protokolu) ve výchozím nastavení. Pro kód Apple OSX/iOS jsou dvě analyzátory jazyka XML, můžete použít: NSXMLParser a libXML2. 

## <a id="app-verification"></a>Aplikace využívá ovladač http.sys provést ověření kanonizace adresy URL

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Všechny aplikace, která používá soubor http.sys postupujte podle těchto pokynů:</p><ul><li>Omezte toono délka adresy URL hello více než 16 384 znaky (ASCII nebo Unicode). Toto je hello absolutní maximální délka adresy URL na základě nastavení Internetové informační služby (IIS) 6 výchozí hello. Weby musí zajistit dobu kratší než to, pokud je to možné</li><li>Použijte hello standardní rozhraní .NET Framework vstupně-výstupní třídy (například FileStream), jak tyto budou využívat výhod hello kanonizace pravidla v rozhraní .NET FX hello</li><li>Explicitně sestavit seznam povolených známé názvů souborů</li><li>Explicitně odmítnout známý typ souborů, neprovede UrlScan odmítne: exe bat, cmd, com, htw, ida, idq, htr, idc, shtm [l], stm, tiskárny, ini, pol, soubory dat</li><li>Catch hello následující výjimky:<ul><li>System.ArgumentException (pro názvy zařízení)</li><li>System.NotSupportedException (pro datové proudy)</li><li>System.IO.FileNotFoundException (pro neplatný uvozený názvy souborů)</li><li>System.IO.DirectoryNotFoundException (pro neplatný uvozený adresáře)</li></ul></li><li>*Nechcete* vyvolávající tooWin32 souborů API vstupně-výstupní operace. Při neplatná adresa URL elegantně vrátí uživatele toohello Chyba 400 a protokolu hello skutečné chyby.</li></ul>|

## <a id="controls-users"></a>Zajistěte, aby byl příslušný ovládací prvky jsou zavedené při přijetí soubory od uživatelů

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Neomezený nahrávání souborů](https://www.owasp.org/index.php/Unrestricted_File_Upload), [soubor podpisu tabulky](http://www.garykessler.net/library/file_sigs.html) |
| **Kroky** | <p>Odeslané soubory představují tooapplications významné riziko.</p><p>prvním krokem Hello v řada útoků je tooget některé kód toohello systému toobe napadení. Potom hello útoku stačí toofind kód hello tooget způsob, jak provést. Použití nahrávání souborů pomáhá hello útočník provést první krok hello. Hello důsledcích při nahrávání souborů neomezený se může lišit, včetně převzetí celý systém, databáze, předávání útoky tooback-end systémy a jednoduchý poškození vzhledu aplikace systému přetížené souborů nebo.</p><p>To závisí na jaké aplikace hello nepodporuje souborem hello nahrán a hlavně kde je uložen. Ověřování na straně serveru z nahrávání souborů nebyla nalezena. Následující kontrolní mechanismy zabezpečení, by měla být implementována pro nahrání souboru funkce:</p><ul><li>Soubor rozšíření kontrolu (jenom platnou sadu povolený typ měli accepted)</li><li>Maximální limit velikosti souboru</li><li>Soubor by neměl být nahrané toowebroot; Hello umístění musí být adresáře na nesystémové jednotky</li><li>Zásady vytváření názvů musí být sledována, tak, aby hello nahrávaný soubor název měly některé náhodnost tak jako tooprevent přepíše soubor</li><li>Soubory by měl být kontrolována antivirový před zápisem toohello disku</li><li>Aby se zajistilo hello název souboru a všechny další metadata (například cesta k souboru) ověření škodlivý znaků</li><li>Zkontrolovat podpis formát souboru, tooprevent uživatele z nahrávání masqueraded souboru (například odeslat soubor exe změnou tootxt rozšíření)</li></ul>| 

### <a name="example"></a>Příklad
Hello posledního bodu týkající se ověřování podpisu formát souboru najdete v části toohello třída následující podrobnosti: 

```C#
        private static Dictionary<string, List<byte[]>> fileSignature = new Dictionary<string, List<byte[]>>
                    {
                    { ".DOC", new List<byte[]> { new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 } } },
                    { ".DOCX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".PDF", new List<byte[]> { new byte[] { 0x25, 0x50, 0x44, 0x46 } } },
                    { ".ZIP", new List<byte[]> 
                                            {
                                              new byte[] { 0x50, 0x4B, 0x03, 0x04 },
                                              new byte[] { 0x50, 0x4B, 0x4C, 0x49, 0x54, 0x55 },
                                              new byte[] { 0x50, 0x4B, 0x53, 0x70, 0x58 },
                                              new byte[] { 0x50, 0x4B, 0x05, 0x06 },
                                              new byte[] { 0x50, 0x4B, 0x07, 0x08 },
                                              new byte[] { 0x57, 0x69, 0x6E, 0x5A, 0x69, 0x70 }
                                                }
                                            },
                    { ".PNG", new List<byte[]> { new byte[] { 0x89, 0x50, 0x4E, 0x47, 0x0D, 0x0A, 0x1A, 0x0A } } },
                    { ".JPG", new List<byte[]>
                                    {
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE1 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE8 }
                                    }
                                    },
                    { ".JPEG", new List<byte[]>
                                        { 
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 }
                                        }
                                        },
                    { ".XLS", new List<byte[]>
                                            {
                                              new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 },
                                              new byte[] { 0x09, 0x08, 0x10, 0x00, 0x00, 0x06, 0x05, 0x00 },
                                              new byte[] { 0xFD, 0xFF, 0xFF, 0xFF }
                                            }
                                            },
                    { ".XLSX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".GIF", new List<byte[]> { new byte[] { 0x47, 0x49, 0x46, 0x38 } } }
                };

        public static bool IsValidFileExtension(string fileName, byte[] fileData, byte[] allowedChars)
        {
            if (string.IsNullOrEmpty(fileName) || fileData == null || fileData.Length == 0)
            {
                return false;
            }

            bool flag = false;
            string ext = Path.GetExtension(fileName);
            if (string.IsNullOrEmpty(ext))
            {
                return false;
            }

            ext = ext.ToUpperInvariant();

            if (ext.Equals(".TXT") || ext.Equals(".CSV") || ext.Equals(".PRN"))
            {
                foreach (byte b in fileData)
                {
                    if (b > 0x7F)
                    {
                        if (allowedChars != null)
                        {
                            if (!allowedChars.Contains(b))
                            {
                                return false;
                            }
                        }
                        else
                        {
                            return false;
                        }
                    }
                }

                return true;
            }

            if (!fileSignature.ContainsKey(ext))
            {
                return true;
            }

            List<byte[]> sig = fileSignature[ext];
            foreach (byte[] b in sig)
            {
                var curFileSig = new byte[b.Length];
                Array.Copy(fileData, curFileSig, b.Length);
                if (curFileSig.SequenceEqual(b))
                {
                    flag = true;
                    break;
                }
            }

            return flag;
        }
```

## <a id="typesafe"></a>Ujistěte se, jestli je pro přístup k datům ve webové aplikaci používají bezpečnost typů parametrů

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Pokud používáte hello parametry kolekce, vstup hello vyhodnotí SQL je jako hodnotu literálu místo pak jako spustitelný kód. Hello kolekce parametrů lze použít tooenforce typ a délku omezení na vstupní data. Hodnoty mimo rozsah hello aktivovat výjimku. Pokud se nepoužívají bezpečnost typů parametrů SQL, může být útočníci možné tooexecute vkládání útoků, které jsou součástí hello nefiltrovaná vstup.</p><p>Při vytváření SQL dotazuje tooavoid možné SQL vkládání útoků, které mohou nastat u nefiltrované vstup, použijte bezpečné parametry typu. Parametry typu bezpečné můžete použít s uložené procedury a dynamických příkazů SQL. Parametry jsou zpracovány jako literálových hodnot hello databáze a ne jako spustitelný kód. Parametry jsou zaškrtnutá políčka, typ a délku.</p>|

### <a name="example"></a>Příklad 
Hello následující kód ukazuje, jak toouse bezpečné parametry typu s hello SqlParameterCollection při volání uložené procedury. 

```C#
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter(LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
V předchozím příkladu kódu hello hello vstupní hodnota nemůže být delší než 11 znaků. Pokud hello data neodpovídá typu toohello nebo délka definované parametrem hello, hello SqlParameter třída vyvolá výjimku. 

## <a id="binding-mvc"></a>Používat samostatný model vazby třídy nebo filtr vazeb vypíše tooprevent MVC velkokapacitního přiřazení ohrožení zabezpečení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC5 MVC6 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Atributy metadat](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.metadatatypeattribute), [veřejný klíč ohrožení zabezpečení a snížení rizika zabezpečení](https://github.com/blog/1068-public-key-security-vulnerability-and-mitigation), [tooMass kompletní Příručka pro přiřazení v aplikaci ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2012/03/11/complete-guide-to-mass-assignment-in-asp-net-mvc.aspx), [Začínáme s EF pomocí MVC](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost) |
| **Kroky** | <ul><li>**Pokud by měl vypadat pro typu overpost ohrožení zabezpečení? -** Typu overpost ohrožení zabezpečení může dojít, všechny místní vázat třídy modelu ze vstupu uživatele. Architektury, jako je MVC může představovat uživatelská data ve vlastní třídy rozhraní .NET, včetně prostý staré objekty CLR (POCOs). MVC automaticky naplní tyto třídy modelu s daty z požadavku hello poskytuje pohodlné reprezentaci pro práci s vstup uživatele. Pokud tyto třídy obsahovat vlastnosti, které by neměl být nastavený uživatelem hello, hello aplikace může být snadno napadnutelný tooover zveřejňování útoků, které povolí uživatelský ovládací prvek dat, která aplikace hello nikdy určena. Jako vazby modelu MVC, například objekt nebo relační mappers jako Entity Framework technologie pro přístup k databázi často také podporují, pomocí dat objektů POCO objekty toorepresent databáze. Tyto třídy modelu dat poskytují hello stejné pohodlí při plánování práce s data databáze stejně MVC v práci s vstup uživatele. Vzhledem k tomu MVC a hello databáze podporují podobné modely jako objektů POCO, se zdá být snadno tooreuse hello stejné třídy pro oba účely. Tento postup selže, toopreserve oddělení otázky ale je jeden běžné oblasti, kde nezamýšleným vlastnosti zveřejněné toomodel vazby, povolení přečerpání příspěvků útoky.</li><li>**Proč by neměl používat Můj třídy modelu nefiltrované databáze jako parametry toomy MVC akce? -** Vazby modelu MVC protože nic vazby v dané třídě. I když hello data nezobrazí v zobrazení, že uživatel se zlými úmysly může odeslat požadavek HTTP s těmito daty zahrnuté a MVC bude Ochotně navázat jej protože akci říká, že třída databáze je tvar hello dat musí přijmout na vstup uživatele.</li><li>**Proč by měla vědět o hello tvaru používanou pro vazbu modelu? -** Vazby modelu pomocí ASP.NET MVC s příliš široká modely zpřístupní útoky tooover příspěvků aplikaci. Přečerpání příspěvků by mohl útočník toochange aplikace dat přesahující jaké hello vývojáře určený, jako je například přepsání hello ceny pro položku nebo hello zabezpečení oprávnění pro účet. Aplikace by měly používat konkrétní akce vazby modelů (nebo seznamy filtrů určitou vlastnost povolené) tooprovide explicitní kontrakt, pro jaké nedůvěryhodné vstupní tooallow prostřednictvím vazby modelu.</li><li>**Má samostatnou vazbu modely právě duplikování kód? -** Ne, je otázka jsou oddělené oblasti zájmu. Použijete-li opakovaně modely databáze v metody akce, můžete si všechny vlastnosti (nebo dílčí vlastnosti) třída může být nastavena uživatelem hello v požadavku HTTP. Pokud nechcete MVC toodo, potřebujete seznam filtrů nebo samostatné třídy tvar tooshow MVC, jaká data mohou pocházet z místo vstupu uživatele.</li><li>**Pokud je nutné modely samostatnou vazbu na vstup uživatele, je nutné provést tooduplicate všechny moje atributy poznámky dat? -** Nemusí. Můžete vytvořit MetadataTypeAttribute hello databáze třída toolink toohello metadat modelu na třídu vazby modelu. Jenom Všimněte si, že hello typ odkazuje hello MetadataTypeAttribute musí být podmnožinou hello odkazující na typ (může mít méně vlastností, ale ne více).</li><li>**Přesunutí dat a zpět mezi modely vstupu uživatele a modely databáze je zdlouhavé. Je možné pouze kopírovat přes všechny vlastnosti pomocí reflexe? -** Ano. Hello pouze vlastnosti, které se zobrazují v hello vazby modely jsou hello ty, které jsou zjistíte toobe bezpečné pro vstup uživatele. Neexistuje žádný důvod zabezpečení, která zabraňuje pomocí reflexe toocopy přes všechny vlastnosti, které existují společné mezi tyto dva modely.</li><li>**Co se chystáte [Bind (vyloučit = "WinMgmt€ ¦")]. Můžete použít, místo nutnosti modely samostatnou vazbu? -** Tento přístup se nedoporučuje. Použití [Bind (vyloučit = "WinMgmt€ ¦")] znamená, že se všechny nové vlastnosti vazbu ve výchozím nastavení. Při přidání nové vlastnosti je bezpečné další krok tooremember tookeep věcí, nikoli s hello návrhu se ve výchozím nastavení zabezpečení. V závislosti na vývojáře hello pokaždé, když je vlastnost přidána kontrola tento seznam je rizikové.</li><li>**Je [Bind (zahrnout = "WinMgmt€ ¦")] užitečné pro operace upravit? -** Ne. [Bind (zahrnout = "WinMgmt€ ¦")] je vhodný pro operace INSERT stylu (přidání nových dat). Operace aktualizace stylu (Úprava existující data) použijte jiný přístup, jako má samostatnou vazbu modely nebo předání explicitní seznam povolených vlastnosti tooUpdateModel nebo TryUpdateModel. Přidání [Bind (zahrnout = "¦ €")] atribut u operace úpravy znamená, že MVC se vytvořit instanci objektu a nastavit jen hello uvedené vlastnosti, a všechny ostatní na jejich výchozí hodnoty. Při hello dat, nahradí zcela stávající entity hello resetování hello hodnoty pro všechny výchozí hodnoty tootheir vynechání vlastnosti. Například, pokud byl IsAdmin vynechaný [Bind (zahrnout = "¦ €")] atribut u operace úpravy, každý uživatel, jehož název byl upraven přes tato akce by tooIsAdmin resetování = false (všechny upravená uživatel ztratí přihlášeni jako správce). Pokud chcete tooprevent aktualizací toocertain vlastnosti, použijte jednu z hello jiné postupy výše. Všimněte si, že některé verze nástrojů pro MVC generovat řadiče tříd pomocí [Bind (zahrnout = "¦ €")] upravit akce a neznamená, že odebrání vlastnosti z tohoto seznamu, bude zabránit útokům přečerpání účtování. Ale jak je popsáno výše, nefunguje tak, jak má tento přístup a místo toho resetuje všechna data v hello vynechání vlastnosti tootheir výchozí hodnoty.</li><li>**Pro operace vytvoření, existují jakékoli upozornění pomocí [Bind (zahrnout = "WinMgmt€ ¦")] místo modely samostatnou vazbu? -** Ano. Nejprve tento přístup nefunguje pro scénářům, úpravy, které vyžadují zachování dva samostatné přístupy pro minimalizaci všechna ohrožení zabezpečení přečerpání příspěvků. Druhou, samostatnou vazbu modely vynutit oddělené oblasti zájmu mezi hello tvaru používanou pro vstup a hello tvar uživatele používané pro trvalost, něco [Bind (zahrnout = "WinMgmt€ ¦")] neprovádí. Třetí, Všimněte si, že [Bind (zahrnout = "WinMgmt€ ¦")] lze zpracovat pouze nejvyšší úrovně vlastnosti; v atributu hello nemůže povolit jenom části dílčí vlastnosti (například "Details.Name"). Nakonec a případně co je nejdůležitější – pomocí [Bind (zahrnout = "WinMgmt€ ¦")] Přidá další krok, který musí mít na paměti, všechny time hello třída se používá pro vazbu modelu. Pokud nové metody akce váže třída dat toohello přímo a zapomene tooinclude [vazby (zahrnout = "¦ €")] atribut, mohou být zranitelné v důsledku tooover zveřejňování útoky, takže hello [Bind (zahrnout = "¦ €")] přístup je méně zabezpečené ve výchozím nastavení. Pokud používáte [Bind (zahrnout = "WinMgmt€ ¦")], tooremember toospecify vždy postará se pokaždé, když vaše data třídy se zobrazí jako parametry metody akce.</li><li>**Pro operace vytvoření, co o spojování hello [Bind (zahrnout = "WinMgmt€ ¦")] atributu na vlastní třídy modelu hello? Není tento přístup vyhnout hello nutné tooremember uváděním hello atribut na každou metodu akce? -** Tento postup funguje v některých případech. Pomocí [Bind (zahrnout = "WinMgmt€ ¦")] na vlastní typ modelu hello (ne na parametry akci pomocí této třídy), vyhněte se hello nutné tooremember tooinclude hello [Bind (zahrnout = "WinMgmt€ ¦")] atribut na každou metodu akce. Pomocí atributu hello přímo na třídě hello efektivně vytvoří samostatné oblasti plochy této třídy pro účely vazby modelu. Však tento přístup umožňuje pouze pro jeden tvar vazby modelu za třídu modelu. Pokud musí jedna metoda akce vazby modelu tooallow pole (například pouze správce akci, která aktualizuje rolí uživatele) a další akce musí vazby modelu tooprevent tohoto pole, tento postup nebude fungovat. Každá třída může mít pouze jeden tvar vazby modelu; Pokud různé akce potřebovat tvarů vazbu jiného modelu, které potřebují toorepresent tyto samostatné obrazců pomocí třídy vazby buď samostatné modelu nebo oddělení [Bind (zahrnout = "WinMgmt€ ¦")] atributů na hello metody akce.</li><li>**Co jsou vazby modelů? Jsou že jejich hello samé jako Zobrazit modely? -** Toto jsou dva související koncepty. termín Hello odkazuje vazba modelu tooa modelu třída používaná v akce je seznam parametrů (hello tvaru předat z metody akce toohello vazby modelu MVC). model zobrazení termín Hello odkazuje třídu modelu tooa předán ze zobrazení tooa metoda akce. Použití modelu specifické pro zobrazení je běžný postup pro předávání dat ze zobrazení tooa metoda akce. Často tento tvar je také vhodná pro vazby modelu a modelu zobrazení hello podmínek může být použité toorefer hello používá stejný model na obou místech. toobe přesné, tento postup bude zmíněn konkrétně vazby modely, které jsou zaměřené na tvar hello předán toohello akce, která je to důležité pro účely velkokapacitního přiřazení.</li></ul>| 

## <a id="rendering"></a>Kódování předchozí toorendering nedůvěryhodné webové výstup

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, webových formulářů, MVC5, MVC6 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Jak tooprevent webů skriptování v technologii ASP.NET](http://msdn.microsoft.com/library/ms998274.aspx), [skriptování mezi](http://cwe.mitre.org/data/definitions/79.html), [list cheaty prevence XSS (skriptování mezi weby)](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet) |
| **Kroky** | Skriptování mezi (často se používá zkratka jako XSS) je způsob útoku pro online služby nebo jakékoli aplikace nebo součásti, která využívá vstup z webové hello. Skriptování XSS může útočníkovi tooexecute skriptu v počítači jiného uživatele prostřednictvím ohrožené webové aplikace. Škodlivých skriptů může být toosteal použité soubory cookie a jinak manipulovat s počítači napadeného počítače prostřednictvím jazyka JavaScript. XSS brání ověřování uživatelského vstupu, zajistíte, že je správně vytvořen a kódování před vykreslením na webové stránce. Pomocí webové ochrany knihovny lze provést ověření vstupu a výstupu kódování. Pro spravovaný kód (C\#, VB.net, atd.), použít jeden nebo více vhodné kódování metody z hello knihovny webové ochrany (Anti-XSS), v závislosti na kontextu hello kde získá označované hello uživatelský vstup:| 

### <a name="example"></a>Příklad

```C#
* Encoder.HtmlEncode 
* Encoder.HtmlAttributeEncode 
* Encoder.JavaScriptEncode 
* Encoder.UrlEncode
* Encoder.VisualBasicScriptEncode 
* Encoder.XmlEncode 
* Encoder.XmlAttributeEncode 
* Encoder.CssEncode 
* Encoder.LdapEncode 
```

## <a id="typemodel"></a>Provedení ověření vstupu a filtrování u všech řetězec typu vlastnosti modelu

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, MVC5, MVC6 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Přidání ověřování](http://www.asp.net/mvc/overview/getting-started/introduction/adding-validation), [ověřování modelu dat v aplikaci MVC](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [hlavní zásady pro vaše aplikace ASP.NET MVC](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Kroky** | <p>Všechny hello vstupní parametry musí být ověřený, než jsou použity v tooensure aplikace hello, aplikace hello zabezpečení proti vstupy uživatel se zlými úmysly. Ověření hello vstupní hodnoty pomocí regulárního výrazu ověření na straně serveru s strategie ověření seznamu povolených IP adres. Unsanitized uživatelské vstupy / parametry předané metody toohello, může způsobit kódu chyby vkládání.</p><p>Pro webové aplikace vstupní body zahrnují také polí formuláře, QueryStrings, soubory cookie, hlaviček protokolu HTTP a parametry webové služby.</p><p>Následující kontroly ověření vstupu Hello musí provést při vazby modelu:</p><ul><li>Vlastnosti modelu Hello by měl být označený poznámkou s poznámkou regulární výraz pro přijetí povolený počet znaků a maximální povolenou délku</li><li>metody kontroleru Hello proveďte ModelState platnosti</li></ul>|

## <a id="richtext"></a>Čištění bude použito na pole formuláře, které přijímají všechny znaky, např, bohaté textového editoru

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Kódování Unsafe vstup](https://msdn.microsoft.com/library/ff647397.aspx#paght000003_step3), [HTML Sanitizér](https://github.com/mganss/HtmlSanitizer) |
| **Kroky** | <p>Identifikujte všechny značky statické značek, které chcete toouse. Běžnou praxí je toorestrict formátování toosafe HTML prvky, jako například `<b>` (tučné) a `<i>` (kurzíva).</p><p>Před zápisem hello data, kódování HTML ho. Tato díky všech škodlivý skriptů bezpečné tím, že ho na toobe spravovány jako text, ne jako spustitelný kód.</p><ol><li>Zakázat žádost ASP.NET ověření přidáním hello hello ValidateRequest = "false" atribut toohello @ Page – direktiva</li><li>Kódování hello vstupní řetězec pomocí metody HtmlEncode hello</li><li>Použití StringBuilder a jeho tooselectively metoda nahradit odebrat hello kódování u elementů hello HTML, které chcete toopermit volání</li></ol><p>Hello odkazů na stránky v hello zakáže ověření požadavku ASP.NET nastavením `ValidateRequest="false"`. Ji umístí kódování HTML hello vstup a umožňuje selektivně hello `<b>` a `<i>` Alternativně knihovny .NET pro čištění HTML mohou být využity také.</p><p>HtmlSanitizer je knihovna pro .NET pro čištění fragmentů kódu HTML a dokumenty z konstrukce, které můžou vést útoky tooXSS. Používá AngleSharp tooparse, manipulaci a vykreslení HTML a CSS. HtmlSanitizer je možné nainstalovat jako balíčku NuGet a hello uživatelský vstup může být předána relevantní HTML nebo šablon stylů CSS čištění metody, jako příslušné na straně serveru hello. Upozorňujeme, který čištění jako ovládací prvek zabezpečení by měl být považován za pouze jako poslední možnost.</p><p>Ověření vstupu a výstupu kódování jsou považovány za lepší kontrolních mechanismů pro zabezpečení.</p> |

## <a id="inbuilt-encode"></a>Nepřiřazujte toosinks DOM elementy, které nemají integrované kódování

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Mnoho funkcí javascript nedělají nic kódování ve výchozím nastavení. Při přiřazování nedůvěryhodné vstupní tooDOM elementy prostřednictvím takové funkce, může vést k křížové spouštění skriptů (XSS) lokality.| 

### <a name="example"></a>Příklad
Následuje několik příkladů, nezabezpečené: 

```
document.getElementByID("div1").innerHtml = value;
$("#userName").html(res.Name);
return $('<div/>').html(value)
$('body').append(resHTML);   
```
Nepoužívejte `innerHtml`; místo toho použijte `innerText`. Podobně, místo provedení `$("#elm").html()`, použijte`$("#elm").text()` 

## <a id="redirect-safe"></a>Ověřit, zda všechny jsou uzavřeny nebo bezpečně provést přesměrování v rámci aplikace hello

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Hello Framework autorizace OAuth 2.0 - otevřete přesměrovačů](http://tools.ietf.org/html/rfc6749#section-10.15) |
| **Kroky** | <p>Návrh aplikace, které vyžadují přesměrování tooa uživatelem zadané umístění musí omezit hello možné přesměrování cíle tooa předdefinovaného "bezpečnou" seznamu sítě nebo domény. Všechny přesměrování aplikace hello musí být uzavřen nebo bezpečné.</p><p>toodo toto:</p><ul><li>Identifikujte všechny přesměrování</li><li>Implementujte příslušné zmírnění dopadů pro každou funkci přesměrování. Odpovídající jejich zmírnění zahrnují přesměrování potvrzení seznamu povolených IP adres nebo uživatele. Pokud webové stránky nebo služby s chybou otevřete přesměrování umožňující používá zprostředkovatelů identity Facebook nebo OAuth/OpenID, může útočník ukrást token přihlášení uživatele a zosobnit uživatele. To je nese riziko při použití OAuth, které jsou uvedené v RFC 6749 "hello Framework autorizace OAuth 2.0", podobně jako část 10.15 "otevřete přesměruje", přihlašovací údaje uživatelů můžete ohroženy útoky spear phishing pomocí otevřete přesměrování</li></ul>|

## <a id="string-method"></a>Implementace ověření vstupu na všechny parametry typu řetězec akceptovat metody Kontroleru

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, MVC5, MVC6 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Ověřování dat modelu v aplikaci MVC](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [hlavní zásady pro vaše aplikace ASP.NET MVC](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Kroky** | Pro metody, které právě přijímají primitivní datový typ a ne modely jako argument by mělo být provedeno ověření vstupu pomocí regulárního výrazu. Regex.IsMatch – v tomto poli musí být použit s vzor platný regulární výraz. Pokud vstup hello neodpovídá hello určený regulární výraz, řízení neměli pokračovat a má být zobrazena odpovídajícího upozornění týkající se chyby ověření.| 

## <a id="dos-expression"></a>Nastavit časový limit horní limit pro regulární výraz zpracování tooprevent DoS z důvodu toobad regulární výrazy

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, webových formulářů, MVC5, MVC6  |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Vlastnost DefaultRegexMatchTimeout](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.defaultregexmatchtimeout.aspx) |
| **Kroky** | útoků tooensure DOS proti chybně vytvořen regulární výrazy, které způsobí mnoho zpětné navracení, nastavte hello globální výchozí časový limit. Pokud doba zpracování hello trvá déle, než hello definované horní limit, by vyvolat výjimku časového limitu. Pokud je nakonfigurovaná nic, by nekonečné hello vypršení časového limitu.| 

### <a name="example"></a>Příklad
Například hello následující konfiguraci vyvolá RegexMatchTimeoutException, pokud se zpracování hello trvá déle než 5 sekund: 

```C#
<httpRuntime targetFramework="4.5" defaultRegexMatchTimeout="00:00:05" />
```

## <a id="html-razor"></a>Nepoužívejte Html.Raw v zobrazení syntaxe Razor

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC5 MVC6 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| Krok | Webových stránek ASP.Net (Razor) provést automatické kódování HTML. Všechny řetězce tištěné útržky vloženého kódu (@ bloky) jsou automaticky kódovaný jazykem HTML. Ale když `HtmlHelper.Raw` metoda je volána, vrátí kód, který není kódován jazykem HTML. Pokud `Html.Raw()` Pomocná metoda se používá, obchází hello automatické kódování ochrany, která poskytuje Razor.|

### <a name="example"></a>Příklad
Tady je příklad nezabezpečené: 

```C#
<div class="form-group">
            @Html.Raw(Model.AccountConfirmText)
        </div>
        <div class="form-group">
            @Html.Raw(Model.PaymentConfirmText)
        </div>
</div>
```
Nepoužívejte `Html.Raw()` Pokud potřebujete toodisplay značek. Tato metoda neprovádí kódování implicitně výstup. Například použijte další Pomocníci ASP.NET`@Html.DisplayFor()` 

## <a id="stored-proc"></a>Nepoužívejte dynamické dotazy v uložené procedury

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Útok prostřednictvím injektáže SQL zneužije chyby zabezpečení v libovolné příkazy toorun ověření vstupu v databázi hello. Ho může dojít, když vaše aplikace používá vstupní tooconstruct dynamické, že tooaccess příkazy SQL hello databáze. Může také dojít, pokud kód používá uložené procedury, které se předávají řetězce, které obsahují nezpracovaná uživatelský vstup. Pomocí hello útok prostřednictvím injektáže SQL, hello útočník může spustit libovolný příkazy v databázi hello. Všechny příkazy SQL (včetně příkazů SQL hello v uložené procedury) musí být parametry. Parametrizované příkazy SQL, bude přijímat znaků, které mají zvláštní význam tooSQL (např. jednoduchých uvozovkách) bez problémů, protože jsou silného typu. |

### <a name="example"></a>Příklad
Následuje příklad nezabezpečené dynamické uloženou proceduru: 

```C#
CREATE PROCEDURE [dbo].[uspGetProductsByCriteria]
(
  @productName nvarchar(200) = NULL,
  @startPrice float = NULL,
  @endPrice float = NULL
)
AS
 BEGIN
  DECLARE @sql nvarchar(max)
  SELECT @sql = ' SELECT ProductID, ProductName, Description, UnitPrice, ImagePath' +
       ' FROM dbo.Products WHERE 1 = 1 '
       PRINT @sql
  IF @productName IS NOT NULL
     SELECT @sql = @sql + ' AND ProductName LIKE ''%' + @productName + '%'''
  IF @startPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice > ''' + CONVERT(VARCHAR(10),@startPrice) + ''''
  IF @endPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice < ''' + CONVERT(VARCHAR(10),@endPrice) + ''''

  PRINT @sql
  EXEC(@sql)
 END
```

### <a name="example"></a>Příklad
Toto je stejný uložené procedury implementována bezpečně hello: 
```C#
CREATE PROCEDURE [dbo].[uspGetProductsByCriteriaSecure]
(
             @productName nvarchar(200) = NULL,
             @startPrice float = NULL,
             @endPrice float = NULL
)
AS
       BEGIN
             SELECT ProductID, ProductName, Description, UnitPrice, ImagePath
             FROM dbo.Products where
             (@productName IS NULL or ProductName like '%'+ @productName +'%')
             AND
             (@startPrice IS NULL or UnitPrice > @startPrice)
             AND
             (@endPrice IS NULL or UnitPrice < @endPrice)         
       END
```

## <a id="validation-api"></a>Zajistit, aby ověření modelu pro metody webového rozhraní API

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC5 MVC6 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Ověření modelu v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Kroky** | Když klient odešle data tooa webové rozhraní API, je povinné toovalidate hello data před provedením jakékoli zpracovávání. Pro rozhraní ASP.NET Web API, kterou přijmout modely jako vstup, pomocí datových poznámek na modely tooset ověřovací pravidla ve vlastnostech hello hello modelu.|

### <a name="example"></a>Příklad
Hello následující kód ukazuje hello stejné: 

```C#
using System.ComponentModel.DataAnnotations;

namespace MyApi.Models
{
    public class Product
    {
        public int Id { get; set; }
        [Required]
        [RegularExpression(@"^[a-zA-Z0-9]*$", ErrorMessage="Only alphanumeric characters are allowed.")]
        public string Name { get; set; }
        public decimal Price { get; set; }
        [Range(0, 999)]
        public double Weight { get; set; }
    }
}
```

### <a name="example"></a>Příklad
V metodě akce hello řadičů hello rozhraní API má platnosti hello modelu toobe explicitně zaškrtnuto, jak je uvedeno níže: 

```C#
namespace MyApi.Controllers
{
    public class ProductsController : ApiController
    {
        public HttpResponseMessage Post(Product product)
        {
            if (ModelState.IsValid)
            {
                // Do something with hello product (not shown).

                return new HttpResponseMessage(HttpStatusCode.OK);
            }
            else
            {
                return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
            }
        }
    }
}
```

## <a id="string-api"></a>Implementace ověření vstupu na všechny parametry typu řetězec přijata metodami webového rozhraní API

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, MVC 5, 6 MVC |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Ověřování dat modelu v aplikaci MVC](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [hlavní zásady pro vaše aplikace ASP.NET MVC](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Kroky** | Pro metody, které právě přijímají primitivní datový typ a ne modely jako argument by mělo být provedeno ověření vstupu pomocí regulárního výrazu. Regex.IsMatch – v tomto poli musí být použit s vzor platný regulární výraz. Pokud vstup hello neodpovídá hello určený regulární výraz, řízení neměli pokračovat a má být zobrazena odpovídajícího upozornění týkající se chyby ověření.|

## <a id="typesafe-api"></a>Ujistěte se, že bezpečnost typů parametrů se používají v webového rozhraní API pro přístup k datům

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Pokud používáte hello parametry kolekce, vstup hello vyhodnotí SQL je jako hodnotu literálu místo pak jako spustitelný kód. Hello kolekce parametrů lze použít tooenforce typ a délku omezení na vstupní data. Hodnoty mimo rozsah hello aktivovat výjimku. Pokud se nepoužívají bezpečnost typů parametrů SQL, může být útočníci možné tooexecute vkládání útoků, které jsou součástí hello nefiltrovaná vstup.</p><p>Při vytváření SQL dotazuje tooavoid možné SQL vkládání útoků, které mohou nastat u nefiltrované vstup, použijte bezpečné parametry typu. Parametry typu bezpečné můžete použít s uložené procedury a dynamických příkazů SQL. Parametry jsou zpracovány jako literálových hodnot hello databáze a ne jako spustitelný kód. Parametry jsou zaškrtnutá políčka, typ a délku.</p>|

### <a name="example"></a>Příklad
Hello následující kód ukazuje, jak toouse bezpečné parametry typu s hello SqlParameterCollection při volání uložené procedury. 

```C#
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter(LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
V předchozím příkladu kódu hello hello vstupní hodnota nemůže být delší než 11 znaků. Pokud hello data neodpovídá typu toohello nebo délka definované parametrem hello, hello SqlParameter třída vyvolá výjimku. 

## <a id="sql-docdb"></a>Použít umožňující dotazy SQL pro Cosmos DB

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Documentdb | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Uvedení Parametrizace SQL v DocumentDB](https://azure.microsoft.com/blog/announcing-sql-parameterization-in-documentdb/) |
| **Kroky** | I když DocumentDB podporuje pouze dotazy jen pro čtení, je stále možné v případě, že dotazy jsou vytvořený zřetězením s uživatelský vstup Injektáž SQL. Je možné pro toodata toogain přístupu uživatele, by neměl být přístup v rámci hello stejné kolekci tím, že vytvoří škodlivý dotazy SQL. Parametrizované dotazy SQL pomocí Pokud dotazy se vytvářejí na základě na vstup uživatele. |

## <a id="schema-binding"></a>Ověření vstupu WCF prostřednictvím vazbou schématu

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, NET Framework 3 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff647820.aspx) |
| **Kroky** | <p>Nedostatečná ověření vede toodifferent typů vkládání útoků.</p><p>Zpráva ověření představuje jeden řádek obrany v hello ochrany aplikace WCF. S tímto přístupem ověřit zpráv pomocí operací služby WCF tooprotect schémata před útokem škodlivý klientem. Ověřte všechny zprávy přijaté službou hello klient tooprotect hello klient před útokem škodlivý službou. Zpráva ověření je možné toovalidate zprávy při operations využívat kontrakty zpráv nebo datové kontrakty, které není možné pomocí ověření parametru. Zpráva ověření umožňuje logiku ověření toocreate uvnitř schémat, a tím poskytuje větší flexibilitu a zkrácení doby vývoj. Schémata můžete opětovně použít napříč různými aplikacemi uvnitř organizace hello, vytváření standardy pro znázornění dat. Kromě toho zpráva ověření můžete tooprotect operace při spotřebování kontrakty představující obchodní logiku zahrnující komplexnější datové typy.</p><p>tooperform zpráva ověření, nejprve vytvořit schéma, které představuje hello operations vaší služby a hello datových typů, které spotřebovávají tyto operace. Pak vytvoříte třídu rozhraní .NET, která implementuje inspector zpráva vlastního klienta a vlastní dispečera zpráv inspector toovalidate hello zprávy ze služby hello odeslat/přijmout. V dalším kroku implementovat vlastní koncový bod chování tooenable zpráva ověřování klienta hello i hello služby. Nakonec implementovat vlastní konfigurace element na hello třídu, která umožňuje tooexpose hello rozšířené chování vlastní koncového bodu v konfiguračním souboru hello hello služby nebo hello klienta"</p>|

## <a id="parameters"></a>Ověření vstupu WCF prostřednictvím parametru kontroly

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, NET Framework 3 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff647875.aspx) |
| **Kroky** | <p>Vstup a ověření dat představuje jeden důležité linii obrany v hello ochrany aplikace WCF. Měli byste ověřit, všechny parametry, které jsou zveřejněné v WCF operations tooprotect hello služby před útokem škodlivého klienta. Naopak by mělo také ověřit všechny návratové hodnoty přijatých hello klient tooprotect hello klient před útokem škodlivý službou</p><p>WCF poskytuje body rozšiřitelnosti jiný, které vám umožňují toocustomize hello WCF modul runtime chování tak, že vytvoříte vlastní rozšíření. Zpráva, že kontrol a parametr inspektoři dvěma způsoby rozšiřitelnost použít toogain větší kontrolu nad daty hello předávání mezi klientem a služby. Můžete by měl použít parametr inspektoři pro ověření vstupu a používat inspektoři zpráv pouze v případě potřeby tooinspect hello celý zpráv předávaných do/z služby.</p><p>ověřování vstupu tooperform, bude sestavovat třídu rozhraní .NET a implementovat vlastní parametr inspector v pořadí parametrů toovalidate na operace ve službě. Pak budete implementovat vlastní koncový bod ověřování tooenable chování hello klienta i hello služby. Nakonec budete implementovat vlastní konfigurace element na hello třídu, která umožňuje tooexpose hello rozšířené chování vlastní koncového bodu v konfiguračním souboru hello hello služby nebo hello klienta</p>|
