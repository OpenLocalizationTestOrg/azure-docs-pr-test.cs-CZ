Hello systému DNS (Domain Name) je použité toolocate prostředky na hello Internetu. Například pokud zadejte webovou adresu aplikace v prohlížeči nebo kliknutím na odkaz na webové stránce, používá DNS tootranslate hello domény na IP adresu. Hello IP adresa je použít jako adresu, ale není velmi lidského popisný. Například je mnohem snazší tooremember název DNS jako **contoso.com** než je tooremember adresu IP, jako je například 192.168.1.88 nebo 2001:0:4137:1f67:24a2:3888:9cce:fea3.

Hello systému DNS je založena na *záznamy*. Zaznamenává přidružit konkrétní *název*, jako například **contoso.com**, IP adresu nebo jiný název DNS. Při aplikaci, například webový prohlížeč, vyhledává název ve službě DNS, najde záznam hello a používá ho body tooas hello adresu. Pokud hello je hodnota toois body IP adresu, použije hello prohlížeče tuto hodnotu. Pokud ukazuje název DNS tooanother, v hello aplikace má toodo řešení znovu. Nakonec překlad všechny vyprší za IP adresu.

Při vytváření webové aplikace ve službě App Service, název DNS se automaticky přiřadí toohello webové aplikace. Tento název má podobu hello  **&lt;yourwebappname&gt;. azurewebsites.net**. Existuje také virtuální IP adresy k dispozici pro použití při vytváření DNS záznamů, můžete buď vytvořit záznamy tento bod toohello **. azurewebsites.net**, nebo můžete přejít toohello IP adresu.

> [!NOTE]
> Hello IP adresa vaší webové aplikace se změní, pokud odstraníte a znovu vytvořte webovou aplikaci nebo změnit hello režimu plán služby App Service příliš**volné** po byla nastavena příliš**základní**, **sdílené**, nebo **standardní**.
> 
> 

Existují také více typů záznamů, každá má své vlastní funkce a omezení, ale pro webové aplikace pouze uživatelské o dva, *A* a *CNAME* záznamy.

### <a name="address-record-a-record"></a>Záznam adresy (záznam)
Záznam A mapuje domény, jako například **contoso.com** nebo **www.contoso.com**, *nebo zástupné domény* například  **\*. contoso.com**, tooan IP adresu. V případě hello webové aplikace ve službě App Service buď hello virtuální IP adresy služby hello nebo konkrétní IP adresu, které jste zakoupili pro vaši webovou aplikaci.

hlavní výhody Hello záznam přes záznam CNAME jsou:

* Můžete namapovat kořenové domény **contoso.com** tooan IP adresu; mnoho registrátorů povolit pouze pomocí záznamů a.
* Může mít jednu položku, která používá zástupný znak, jako například  **\*. contoso.com**, který by například zpracovávat požadavky pro více subdomén **mail.contoso.com**,  **blogs.contoso.com**, nebo **www.contso.com**.

> [!NOTE]
> Vzhledem k tomu, že je namapovaný záznam tooa statickou IP adresu nelze vyřešit automaticky změny toohello IP adresa vaší webové aplikace. Je zadána adresa IP pro použití s záznamy A při konfiguraci nastavení názvu vlastní domény pro vaši webovou aplikaci; však tato hodnota může změnit, pokud odstraníte a znovu vytvořte webovou aplikaci nebo změnit hello tooback režimu plán služby App Service příliš**volné**.
> 
> 

### <a name="alias-record-cname-record"></a>Záznam aliasu (záznam CNAME)
Záznam CNAME se mapuje *konkrétní* název DNS, jako například **mail.contoso.com** nebo **www.contoso.com**, název domény (kanonický) tooanother. V případě hello App Service Web Apps, je název kanonický domény hello hello  **&lt;yourwebappname >. azurewebsites.net** název domény vaší webové aplikace. Po vytvoření hello CNAME tento alias hello  **&lt;yourwebappname >. azurewebsites.net** název domény. Hello záznam CNAME se přeložit IP adresu toohello vaše  **&lt;yourwebappname >. azurewebsites.net** název domény automaticky, takže pokud se změní IP adresu hello hello webové aplikace, není nutné tootake žádnou akci.

> [!NOTE]
> Některé domény registrátorů umožňují pouze toomap subdomény při použití záznam CNAME, jako **www.contoso.com**a nejsou hlavní názvy, například **contoso.com**. Další informace o záznamy CNAME, naleznete v dokumentaci k hello poskytované vašeho registrátora <a href="http://en.wikipedia.org/wiki/CNAME_record">hello Wikipedia položku na záznam CNAME</a>, nebo hello <a href="http://tools.ietf.org/html/rfc1035">IETF názvy domén – implementace a specifikace</a> dokument.
> 
> 

### <a name="web-app-dns-specifics"></a>Specifika DNS webové aplikace
Pomocí záznamu A webové aplikace vyžaduje, abyste toofirst vytvořit jednu z následujících záznamů TXT hello:

* **Pro kořenovou doménu hello** – záznam A DNS TXT  **@**  příliš  **&lt;yourwebappname&gt;. azurewebsites.net**.
* **Pro konkrétní subdomény** – název A DNS  **&lt;subdomény >** příliš**&lt;yourwebappname&gt;. azurewebsites.net**. Například **blogy** Pokud hello záznam pro **blogs.contoso.com**.
* **Pro zástupný text hello sub-dodmains** – záznam A DNS TXT *** příliš  **&lt;yourwebappname&gt;. azurewebsites.net**.

Tento záznam TXT je použité tooverify, že jste vlastníkem domény hello se pokoušíte toouse. Toto je navíc toocreating A záznam odkazující toohello virtuální IP adresy vaší webové aplikace.

Můžete najít hello IP adresu a **. azurewebsites.net** hello názvy pro vaši webovou aplikaci tak, že provedete následující kroky:

1. V prohlížeči otevřete hello [portálu Azure](https://portal.azure.com).
2. V hello **webové aplikace** okně klikněte na název hello vaší webové aplikace a pak vyberte **vlastní domény** hello dolní části stránky hello.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. V hello **vlastní domény** okně uvidíte hello virtuální IP adresy. Uložit tyto informace, jako se dají použít při vytváření záznamů DNS
   
    ![](./media/custom-dns-web-site/virtual-ip-address.png)
   
   > [!NOTE]
   > Nemůžete použít názvy vlastních domén s **volné** webová aplikace a musí upgradovat plán služby App Service hello příliš**sdílené**, **základní**, **standardní**, nebo **Premium** vrstvy. Další informace o hello služby App Service je plán cenové úrovně, včetně jak toochange hello cenová úroveň vaší webové aplikace najdete v tématu [jak tooscale webové aplikace](../articles/app-service-web/web-sites-scale.md).
   > 
   > 

