Azure určí hello verzi toouse Python pro jeho virtuální prostředí s hello následující priority:

1. verze zadaná v souboru runtime.txt v kořenové složce hello
2. verze zadaná v nastavení jazyka Python v konfiguraci webové aplikace hello (hello **nastavení** > **nastavení aplikace** okno pro vaši webovou aplikaci v hello portál Azure)
3. Python 2.7 je výchozí hello, pokud nejsou zadány žádné z výše uvedených hello

Platné hodnoty pro obsah hello 

    \runtime.txt

jsou následující:

* python-2.7
* python-3.4

Pokud hello mikroverze (třetí číslice) je zadán, bude se ignorovat.

