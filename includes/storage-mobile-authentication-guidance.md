## <a name="configure-your-application-tooaccess-azure-storage"></a>Konfigurace tooaccess vaše aplikace Azure Storage
Existují dva způsoby tooauthenticate tooaccess vaše aplikace služby úložiště:

* Sdílený klíč: Použít sdílený klíč jenom pro účely testování
* Sdílený přístupový podpis (SAS): Použití SAS pro výrobní aplikace

### <a name="shared-key"></a>Sdílený klíč
Ověření pomocí sdíleného klíče znamená, že aplikace bude používat název účtu a účet klíče tooaccess služby úložiště. Pro účely hello rychle zobrazující jak toouse této knihovny, použijeme ověření sdíleným klíčem v této příručce Začínáme.

> [!WARNING] 
> **Používejte ověření sdíleným klíčem pouze pro účely testování!** Název účtu a klíč účtu, což dává přístup toohello čtení/zápisu přidruženého účtu úložiště, bude distribuované tooevery osoba, který stahuje vaší aplikace. Toto je **není** dobrou praxi, protože riskujete, že má váš klíč nekompromitovali nedůvěryhodní klienti.
> 
> 

Pokud používáte ověření sdíleným klíčem, vytvoříte [připojovací řetězec](../articles/storage/common/storage-configure-connection-string.md). připojovací řetězec Hello se skládá z:  

* Hello **DefaultEndpointsProtocol** – můžete protokolu HTTP nebo HTTPS. Však pomocí protokolu HTTPS důrazně doporučujeme.
* Hello **název účtu** hello – název účtu úložiště
* Hello **klíč účtu** – na hello [portálu Azure](https://portal.azure.com), přejděte tooyour účet úložiště a klikněte na tlačítko hello **klíče** ikonu toofind tyto informace.
* (Volitelné) **EndpointSuffix** – používá se pro služby úložiště v oblasti s přípon jinému koncovému bodu, například Azure China nebo Azure zásad správného řízení.

Tady je příklad připojovacího řetězce, pomocí ověření sdíleným klíčem:

`"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"`

### <a name="shared-access-signatures-sas"></a>Sdílené přístupové podpisy (SAS)
Pro mobilní aplikace hello doporučení metodou ověření požadavku klienta vůči hello služby Azure Storage je pomocí sdíleného přístupového podpisu (SAS). SAS umožňuje toogrant prostředek tooa přístupu klienta zadaném časovém období, se zadanou sadou oprávnění.
Jako vlastník účtu úložiště hello budete potřebovat toogenerate SAS pro vaše tooconsume mobilních klientů. toogenerate hello SAS, budete muset pravděpodobně toowrite samostatnou službu, která generuje hello SAS toobe distribuované tooyour klientů. Pro účely testování můžete hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) nebo hello [portálu Azure](https://portal.azure.com) toogenerate SAS. Když vytvoříte hello SAS, můžete zadat hello časový interval, přes které hello je platný SAS a hello oprávnění, která hello SAS uděluje toohello klienta.

Hello následující příklad ukazuje, jak toouse hello Microsoft Azure Storage Explorer toogenerate SAS.

1. Pokud jste to ještě neudělali, [hello instalace Microsoft Azure Storage Explorer](http://storageexplorer.com)
2. Připojení tooyour odběru.
3. Klikněte na vašem účtu úložiště a klikněte na kartu hello "akce" v levé dolní hello. Klikněte na tlačítko "Získání sdíleného přístupového podpisu" toogenerate "připojovací řetězec" pro vaše SAS.
4. Tady je příklad připojovacího řetězce SAS, že uděluje oprávnění čtení a zápis na hello služby, kontejneru a na úrovni objektu blob služby hello hello účtu úložiště.
   
   `"SharedAccessSignature=sv=2015-04-05&ss=b&srt=sco&sp=rw&se=2016-07-21T18%3A00%3A00Z&sig=3ABdLOJZosCp0o491T%2BqZGKIhafF1nlM3MzESDDD3Gg%3D;BlobEndpoint=https://youraccount.blob.core.windows.net"`

Jak můžete vidět, když pomocí SAS, nejsou vystavení klíč účtu v aplikaci. Další informace o přidružení zabezpečení a doporučené postupy pro používání SAS vyzkoušejte [sdílené přístupové podpisy: vysvětlení modelu SAS hello](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md).

