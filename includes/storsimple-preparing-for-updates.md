<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a><span data-ttu-id="fa23b-101">Příprava pro aktualizace</span><span class="sxs-lookup"><span data-stu-id="fa23b-101">Preparing for updates</span></span>
<span data-ttu-id="fa23b-102">Budete potřebovat tooperform hello před kontrolovat a aktualizaci hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fa23b-102">You will need tooperform hello following steps before you scan and apply hello update:</span></span>

1. <span data-ttu-id="fa23b-103">Pořízení snímku cloudu data zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="fa23b-103">Take a cloud snapshot of hello device data.</span></span>
2. <span data-ttu-id="fa23b-104">Zajistěte, aby vaše pevné IP adresy řadiče jsou směrovatelné a může připojit toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="fa23b-104">Ensure that your controller fixed IPs are routable and can connect toohello Internet.</span></span> <span data-ttu-id="fa23b-105">Budou to pevné IP adresy používané tooservice aktualizace tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="fa23b-105">These fixed IPs will be used tooservice updates tooyour device.</span></span> <span data-ttu-id="fa23b-106">Toto můžete otestovat spuštěním následující rutiny v každém řadiči z rozhraní Windows PowerShell hello hello zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="fa23b-106">You can test this by running hello following cmdlet on each controller from hello Windows PowerShell interface of hello device:</span></span>
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    <span data-ttu-id="fa23b-107">**Ukázkový výstup pro Test-Connection, když pevné IP adresy se mohou připojit toohello Internetu**</span><span class="sxs-lookup"><span data-stu-id="fa23b-107">**Sample output for Test-Connection when fixed IPs can connect toohello Internet**</span></span>

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

<span data-ttu-id="fa23b-108">Po úspěšně jste dokončili tyto ruční předběžné kontroly, můžete pokračovat tooscan a nainstalovat aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="fa23b-108">After you have successfully completed these manual pre-checks, you can proceed tooscan and install hello updates.</span></span>

