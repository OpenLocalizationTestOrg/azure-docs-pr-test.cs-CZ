## <a name="build-iot-edge"></a><span data-ttu-id="6469e-101">Sestavení IoT Edge</span><span class="sxs-lookup"><span data-stu-id="6469e-101">Build IoT Edge</span></span>

<span data-ttu-id="6469e-102">Tento kurz používá vlastní moduly toocommunicate IoT Edge s hello předkonfigurovanému řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="6469e-102">This tutorial uses custom IoT Edge modules toocommunicate with hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="6469e-103">Proto musíte toobuild hello IoT Edge moduly z vlastní zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="6469e-103">Therefore, you need toobuild hello IoT Edge modules from custom source code.</span></span> <span data-ttu-id="6469e-104">Hello následující části popisují, jak tooinstall IoT okraj a sestavení hello vlastní modul IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="6469e-104">hello following sections describe how tooinstall IoT Edge and build hello custom IoT Edge module.</span></span>

### <a name="install-iot-edge"></a><span data-ttu-id="6469e-105">Nainstalujte IoT Edge</span><span class="sxs-lookup"><span data-stu-id="6469e-105">Install IoT Edge</span></span>

<span data-ttu-id="6469e-106">Hello následující kroky popisují, jak tooinstall hello předem zkompilovat IoT Edge software na hello Intel NUC:</span><span class="sxs-lookup"><span data-stu-id="6469e-106">hello following steps describe how tooinstall hello pre-compiled IoT Edge software on hello Intel NUC:</span></span>

1. <span data-ttu-id="6469e-107">Konfigurace úložiště inteligentní balíček hello požadované spuštěním následujících příkazů na hello Intel NUC hello:</span><span class="sxs-lookup"><span data-stu-id="6469e-107">Configure hello required smart package repositories by running hello following commands on hello Intel NUC:</span></span>

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    <span data-ttu-id="6469e-108">Zadejte `y` při hello příkaz vás vyzve příliš**zahrnout tento kanál?**.</span><span class="sxs-lookup"><span data-stu-id="6469e-108">Enter `y` when hello command prompts you too**Include this channel?**.</span></span>

1. <span data-ttu-id="6469e-109">Aktualizujte správce balíčků inteligentní hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6469e-109">Update hello smart package manager by running hello following command:</span></span>

    ```bash
    smart update
    ```

1. <span data-ttu-id="6469e-110">Instalovat balíček Azure IoT Edge hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6469e-110">Install hello Azure IoT Edge package by running hello following command:</span></span>

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. <span data-ttu-id="6469e-111">Ověření instalace hello ve spuštěné ukázka hello "Hello, world".</span><span class="sxs-lookup"><span data-stu-id="6469e-111">Verify hello installation by running hello "Hello world" sample.</span></span> <span data-ttu-id="6469e-112">Tato ukázka zapíše soubor hello world zprávy toohello log.txT každých pět sekund.</span><span class="sxs-lookup"><span data-stu-id="6469e-112">This sample writes a hello world message toohello log.txT file every five seconds.</span></span> <span data-ttu-id="6469e-113">Hello následující příkazy hello "Hello, world" ukázku spustit:</span><span class="sxs-lookup"><span data-stu-id="6469e-113">hello following commands run hello "Hello world" sample:</span></span>

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    <span data-ttu-id="6469e-114">Ignorovat jakékoli **neplatný argument** zprávy při zastavení ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="6469e-114">Ignore any **invalid argument** messages when you stop hello sample.</span></span>

    <span data-ttu-id="6469e-115">Použijte následující příkaz tooview hello obsahu souboru protokolu hello hello:</span><span class="sxs-lookup"><span data-stu-id="6469e-115">Use hello following command tooview hello contents of hello log file:</span></span>

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a><span data-ttu-id="6469e-116">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="6469e-116">Troubleshooting</span></span>

<span data-ttu-id="6469e-117">Pokud se zobrazí chyba hello "žádný balíček poskytuje util. linux dev", zkuste restartovat hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="6469e-117">If you receive hello error "No package provides util-linux-dev", try rebooting hello Intel NUC.</span></span>
