<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a>Příprava pro aktualizace
Budete potřebovat tooperform hello před kontrolovat a aktualizaci hello následující kroky:

1. Pořízení snímku cloudu data zařízení hello.
2. Zajistěte, aby vaše pevné IP adresy řadiče jsou směrovatelné a může připojit toohello Internetu. Budou to pevné IP adresy používané tooservice aktualizace tooyour zařízení. Toto můžete otestovat spuštěním následující rutiny v každém řadiči z rozhraní Windows PowerShell hello hello zařízení hello:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    **Ukázkový výstup pro Test-Connection, když pevné IP adresy se mohou připojit toohello Internetu**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Po úspěšně jste dokončili tyto ruční předběžné kontroly, můžete pokračovat tooscan a nainstalovat aktualizace hello.

