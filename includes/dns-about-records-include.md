### <a name="record-names"></a>Názvy záznamů

V Azure DNS se záznamy zadávají pomocí relativních názvů. A *plně kvalifikovaný* název domény (FQDN) obsahuje hello název zóny, zatímco *relativní* název ho neobsahuje. Například hello relativní název záznamu "www" v zóně hello "contoso.com" dává hello záznam plně kvalifikovaný název "www.contoso.com".

*Vrcholu* záznam je záznam DNS na kořenové hello (nebo *vrcholu*) zóny DNS. Například v zóně DNS hello "contoso.com", vytvoří se záznam vrcholu má také plně kvalifikovaný název "contoso.com" hello (to se někdy nazývá *holé* domény).  Podle konvence hello relativní název ' @' je použité toorepresent vrcholu záznamy.

### <a name="record-types"></a>Typy záznamů

Každý záznam DNS má název a typ. Záznamy jsou uspořádány do různých typů podle toohello data, která obsahují. Hello nejběžnějším typem je záznam "A", ve který mapuje název tooan adresu IPv4. Jiné obecným typem je záznam "MX", který mapuje název tooa poštovní server.

Azure DNS podporuje všechny běžné typy záznamů DNS: A, AAAA, CNAME, MX, NS, PTR, SOA, SRV a TXT. [Záznamy SPF se vyjadřují pomocí záznamů TXT](../articles/dns/dns-zones-records.md#spf-records).

### <a name="record-sets"></a>Sady záznamů

Někdy musíte toocreate více než jeden záznam DNS s daným názvem a typem. Předpokládejme například, že webovou stránku hello "www.contoso.com" je hostovaný na dvou různých IP adres. Hello web vyžaduje dva různé záznamy A, jeden pro každou IP adresu. Tady je příklad sady záznamů:

    www.contoso.com.        3600    IN    A    134.170.185.46
    www.contoso.com.        3600    IN    A    134.170.188.221

Azure DNS spravuje všechny záznamy DNS pomocí *sady záznamů*. Sady záznamů (také označované jako *prostředků* sady záznamů) hello kolekce záznamů DNS v zóně, které mají stejný název a jsou hello hello stejného typu. Většina sad záznamů obsahuje jediný záznam. Ale příklady jako hello jeden výše, ve kterém sada záznamů obsahuje více než jeden záznam, nejsou.

Například předpokládejme, že jste již vytvořili záznam "www" v hello zóně "contoso.com", odkazující toohello IP adres "134.170.185.46, (hello první záznam výše).  toocreate hello druhý záznam, který byste přidali, zaznamenejte stávajícího záznamu toohello nastavit, nikoli vytvořit další sady záznamů.

Hello SOA a typy záznamů CNAME jsou výjimkami. standardech DNS Hello nepovolují více záznamů s hello stejný název pro tyto typy, proto může obsahovat tyto sady záznamů pouze jeden záznam.
