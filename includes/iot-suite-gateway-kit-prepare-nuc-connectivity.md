## <a name="prepare-your-intel-nuc"></a><span data-ttu-id="d90e7-101">Příprava vašeho Intel NUC</span><span class="sxs-lookup"><span data-stu-id="d90e7-101">Prepare your Intel NUC</span></span>

<span data-ttu-id="d90e7-102">toocomplete hello hardwaru Instalační program, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="d90e7-102">toocomplete hello hardware setup, you need to:</span></span>

- <span data-ttu-id="d90e7-103">Připojte vaše Intel NUC toohello zdroj napájení součástí hello kit.</span><span class="sxs-lookup"><span data-stu-id="d90e7-103">Connect your Intel NUC toohello power supply included in hello kit.</span></span>
- <span data-ttu-id="d90e7-104">Propojení vaší sítě tooyour Intel NUC pomocí kabelu Ethernet.</span><span class="sxs-lookup"><span data-stu-id="d90e7-104">Connect your Intel NUC tooyour network using an Ethernet cable.</span></span>

<span data-ttu-id="d90e7-105">Teď jste dokončili nastavení hardwaru hello vaše zařízení brány Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="d90e7-105">You have now completed hello hardware setup of your Intel NUC gateway device.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="d90e7-106">Přihlaste se a přístup k Terminálové hello</span><span class="sxs-lookup"><span data-stu-id="d90e7-106">Sign in and access hello terminal</span></span>

<span data-ttu-id="d90e7-107">Dvě možnosti tooaccess máte na vaše Intel NUC terminálu prostředí:</span><span class="sxs-lookup"><span data-stu-id="d90e7-107">You have two options tooaccess a terminal environment on your Intel NUC:</span></span>

- <span data-ttu-id="d90e7-108">Pokud máte klávesnici a sledování připojených tooyour Intel NUC, můžete přímo přistupovat hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="d90e7-108">If you have a keyboard and monitor connected tooyour Intel NUC, you can access hello shell directly.</span></span> <span data-ttu-id="d90e7-109">Hello výchozí přihlašovací údaje jsou uživatelské jméno **kořenové** a heslo **kořenové**.</span><span class="sxs-lookup"><span data-stu-id="d90e7-109">hello default credentials are username **root** and password **root**.</span></span>

- <span data-ttu-id="d90e7-110">Přístup k prostředí hello na vaše Intel NUC pomocí protokolu SSH ze stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="d90e7-110">Access hello shell on your Intel NUC using SSH from your desktop machine.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="d90e7-111">Přihlaste se pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="d90e7-111">Sign in with SSH</span></span>

<span data-ttu-id="d90e7-112">toosign pomocí SSH, musíte hello IP adresu vašeho NUC Intel.</span><span class="sxs-lookup"><span data-stu-id="d90e7-112">toosign in with SSH, you need hello IP address of your Intel NUC.</span></span> <span data-ttu-id="d90e7-113">Pokud máte klávesnici a sledování připojených tooyour Intel NUC, použijte hello `ifconfig` příkaz toofind hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d90e7-113">If you have a keyboard and monitor connected tooyour Intel NUC, use hello `ifconfig` command toofind hello IP address.</span></span> <span data-ttu-id="d90e7-114">Alternativně připojte tooyour směrovač toolist hello adresy zařízení ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="d90e7-114">Alternatively, connect tooyour router toolist hello addresses of devices on your network.</span></span>

<span data-ttu-id="d90e7-115">Přihlaste se pomocí uživatelského jména **kořenové** a heslo **kořenové**.</span><span class="sxs-lookup"><span data-stu-id="d90e7-115">Sign in with username **root** and password **root**.</span></span>

#### <a name="optional-share-a-folder-on-your-intel-nuc"></a><span data-ttu-id="d90e7-116">Volitelné: Sdílené složky na vaše NUC Intel</span><span class="sxs-lookup"><span data-stu-id="d90e7-116">Optional: Share a folder on your Intel NUC</span></span>

<span data-ttu-id="d90e7-117">Volitelně můžete tooshare do složky na vaše Intel NUC s prostředí plochy.</span><span class="sxs-lookup"><span data-stu-id="d90e7-117">Optionally, you may want tooshare a folder on your Intel NUC with your desktop environment.</span></span> <span data-ttu-id="d90e7-118">Sdílení složky umožňuje vám toouse upřednostňované plochy textový editor (například [Visual Studio Code](https://code.visualstudio.com/) nebo [Sublime Text](http://www.sublimetext.com/)) tooedit soubory na vaše Intel NUC místo použití `nano` nebo `vi`.</span><span class="sxs-lookup"><span data-stu-id="d90e7-118">Sharing a folder enables you toouse your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) tooedit files on your Intel NUC instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="d90e7-119">tooshare složka s Windows, konfigurace serveru Samba hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="d90e7-119">tooshare a folder with Windows, configure a Samba server on hello Intel NUC.</span></span> <span data-ttu-id="d90e7-120">Můžete taky použijte server pomocí protokolu SFTP hello v hello Intel NUC s klientem SFTP na stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="d90e7-120">Alternatively, use hello SFTP server on hello Intel NUC with an SFTP client on your desktop machine.</span></span>
