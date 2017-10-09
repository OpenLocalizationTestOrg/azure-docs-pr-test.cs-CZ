Hello systému DNS (Domain Name) je použité toolocate věci na hello Internetu. Například pokud v prohlížeči zadejte adresu nebo kliknutím na odkaz na webové stránce, používá DNS tootranslate hello domény na IP adresu. Hello IP adresa je použít jako adresu, ale není velmi lidského popisný. Například je mnohem snazší tooremember název DNS jako **contoso.com** než je tooremember adresu IP, jako je například 192.168.1.88 nebo 2001:0:4137:1f67:24a2:3888:9cce:fea3.

Hello systému DNS je založena na *záznamy*. Zaznamenává přidružit konkrétní *název*, jako například **contoso.com**, IP adresu nebo jiný název DNS. Při aplikaci, například webový prohlížeč, vyhledává název ve službě DNS, najde záznam hello a používá ho body tooas hello adresu. Pokud hello je hodnota toois body IP adresu, použije hello prohlížeče tuto hodnotu. Pokud ukazuje název DNS tooanother, v hello aplikace má toodo řešení znovu. Nakonec překlad všechny vyprší za IP adresu.

Když vytvoříte web Azure, název DNS se automaticky přiřadí toohello lokality. Tento název má podobu hello  **&lt;yoursitename&gt;. azurewebsites.net**. Při přidání webu jako koncový bod Azure Traffic Manager, je pak web přístupné prostřednictvím hello  **&lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domény.

> [!NOTE]
> Pokud váš web je nakonfigurovaný jako koncový bod Traffic Manager, použijte hello **. trafficmanager.net** adres při vytváření záznamů DNS.
> 
> Můžete použít pouze záznamy CNAME s nástrojem Traffic Manager
> 
> 

Existují také více typů záznamů, každá má své vlastní funkce a omezení, ale pro weby konfigurované tooas Traffic Manager koncových bodů, jsme pouze pro vás k určitému; *CNAME* záznamy.

### <a name="cname-or-alias-record"></a>Záznam CNAME nebo Alias
Záznam CNAME se mapuje *konkrétní* název DNS, jako například **mail.contoso.com** nebo **www.contoso.com**, název domény (kanonický) tooanother. V případě hello weby Azure s využitím Traffic Manager, je název kanonický domény hello hello  **&lt;Moje aplikace >. trafficmanager.net** název domény vašeho profilu Traffic Manageru. Po vytvoření hello CNAME tento alias hello  **&lt;Moje aplikace >. trafficmanager.net** název domény. Hello záznam CNAME se přeložit IP adresu toohello vaše  **&lt;Moje aplikace >. trafficmanager.net** název domény automaticky, takže pokud se změní hello IP adresu webu hello, není nutné tootake žádnou akci.

Jakmile přenos dorazí na Traffic Manager, pak směruje hello provoz tooyour webu, pomocí metody, který je nakonfigurován pro vyrovnávání zatížení hello. Toto je zcela transparentní toovisitors tooyour webu. Pouze uvidí hello vlastní název domény v prohlížeči.

> [!NOTE]
> Některé domény registrátorů umožňují pouze toomap subdomény při použití záznam CNAME, jako **www.contoso.com**a nejsou hlavní názvy, například **contoso.com**. Další informace o záznamy CNAME, naleznete v dokumentaci k hello poskytované vašeho registrátora <a href="http://en.wikipedia.org/wiki/CNAME_record">hello Wikipedia položku na záznam CNAME</a>, nebo hello <a href="http://tools.ietf.org/html/rfc1035">IETF názvy domén – implementace a specifikace</a> dokument.
> 
> 

