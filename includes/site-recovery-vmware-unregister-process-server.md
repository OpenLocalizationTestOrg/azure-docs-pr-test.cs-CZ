<span data-ttu-id="06336-101">Hello kroky toounregister procesový server se liší v závislosti na jeho stav připojení s hello konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="06336-101">hello steps toounregister a process server differs depending on its connection status with hello Configuration Server.</span></span>

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a><span data-ttu-id="06336-102">Zrušení registrace procesového serveru, který je v připojeném stavu</span><span class="sxs-lookup"><span data-stu-id="06336-102">Unregister a process server that is in a connected state</span></span>

1. <span data-ttu-id="06336-103">Vzdálené do hello procesový server jako správce.</span><span class="sxs-lookup"><span data-stu-id="06336-103">Remote into hello process server as an Administrator.</span></span>
2. <span data-ttu-id="06336-104">Spusťte hello **ovládací panely** a otevřete **programy > odinstalovat program**</span><span class="sxs-lookup"><span data-stu-id="06336-104">Launch hello **Control Panel** and open **Programs > Uninstall a program**</span></span>
3. <span data-ttu-id="06336-105">Odinstalovat program podle názvu hello **Microsoft Azure Site obnovení nebo proces konfigurace serveru**</span><span class="sxs-lookup"><span data-stu-id="06336-105">Uninstall a program by hello name **Microsoft Azure Site Recovery Configuration/Process Server**</span></span>
4. <span data-ttu-id="06336-106">Po dokončení kroku 3 můžete odinstalovat **závislosti pro Microsoft Azure Site Recovery Configuration/Process Server**.</span><span class="sxs-lookup"><span data-stu-id="06336-106">Once step 3 is completed, you can uninstall **Microsoft Azure Site Recovery Configuration/Process Server Dependencies**</span></span>

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a><span data-ttu-id="06336-107">Zrušení registrace procesového serveru, který je v odpojeném stavu</span><span class="sxs-lookup"><span data-stu-id="06336-107">Unregister a process server that is in a disconnected state</span></span>

> [!WARNING]
> <span data-ttu-id="06336-108">Pokud neexistuje žádný způsob, jak toorevive hello virtuální počítač na které hello byl nainstalovaný procesového serveru je třeba použít hello pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="06336-108">Use hello below steps should be used if there is no way toorevive hello virtual machine on which hello Process Server was installed.</span></span>

1. <span data-ttu-id="06336-109">Přihlaste se na konfiguračním serveru tooyour jako správce.</span><span class="sxs-lookup"><span data-stu-id="06336-109">Log on tooyour configuration server as an Administrator.</span></span>
2. <span data-ttu-id="06336-110">Otevřete příkazový řádek pro správu a Procházet adresář toohello `%ProgramData%\ASR\home\svsystems\bin`.</span><span class="sxs-lookup"><span data-stu-id="06336-110">Open an Administrative command prompt and browse toohello directory `%ProgramData%\ASR\home\svsystems\bin`.</span></span>
3. <span data-ttu-id="06336-111">Nyní spusťte příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="06336-111">Now run hello command.</span></span>

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. <span data-ttu-id="06336-112">To bude vyprázdnit hello podrobnosti hello procesového serveru ze systému hello.</span><span class="sxs-lookup"><span data-stu-id="06336-112">This will purge hello details of hello process server from hello system.</span></span>
