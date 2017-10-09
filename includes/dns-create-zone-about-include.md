Zóny DNS je použité toohost hello záznamy DNS pro konkrétní domény. toostart hostovat svoji doménu v Azure DNS, je nutné toocreate zóny DNS pro název domény. Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS.

Například hello doménu "contoso.com" může obsahovat několik záznamů DNS, třeba "mail.contoso.com" (pro poštovní server) a "www.contoso.com" (pro webový server).

Při vytváření zóny DNS v DNS Azure:

* Název Hello hello zóny musí být jedinečný v rámci skupiny prostředků hello a hello zóna ještě nesmí existovat. V opačném hello operace se nezdaří.
* Hello stejný název zóny, lze znovu použít v jiné skupině prostředků nebo jiného předplatného Azure.
* Kde několika zón sdílí hello stejným názvem, každá instance je přiřadit různé adresy názvového serveru. Jen jednu sadu adres se dá nakonfigurovat s hello registrátorem názvu domény.

> [!NOTE]
> Nemáte tooown toocreate název domény zónu DNS s tímto názvem domény v Azure DNS. Však nutné názvových serverů Azure DNS tooconfigure hello tooown hello domény jako hello správné názvové servery pro název domény hello u registrátora názvu domény hello.
> 
> Další informace najdete v tématu [delegovat tooAzure domény DNS](../articles/dns/dns-domain-delegation.md).
