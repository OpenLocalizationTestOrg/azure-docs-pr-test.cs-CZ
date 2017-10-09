<!--author=SharS last changed: 9/17/15-->

#### <a name="tooconnect-through-hello-serial-console"></a><span data-ttu-id="c3861-101">tooconnect prostřednictvím sériové konzoly hello</span><span class="sxs-lookup"><span data-stu-id="c3861-101">tooconnect through hello serial console</span></span>
1. <span data-ttu-id="c3861-102">Připojte zařízení toohello sériový kabel (přímo nebo přes sériového adaptéru USB).</span><span class="sxs-lookup"><span data-stu-id="c3861-102">Connect your serial cable toohello device (directly or through a USB-serial adapter).</span></span>
2. <span data-ttu-id="c3861-103">Otevřete hello **ovládací panely**a pak otevřete hello **Správce zařízení**.</span><span class="sxs-lookup"><span data-stu-id="c3861-103">Open hello **Control Panel**, and then open hello **Device Manager**.</span></span>
3. <span data-ttu-id="c3861-104">Port hello COM určete, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="c3861-104">Identify hello COM port as shown in hello following illustration.</span></span>
   
     ![Připojení prostřednictvím sériové konzoly](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. <span data-ttu-id="c3861-106">Spusťte PuTTY.</span><span class="sxs-lookup"><span data-stu-id="c3861-106">Start PuTTY.</span></span> 
5. <span data-ttu-id="c3861-107">V pravém podokně hello změnit hello **typ připojení** příliš**sériové**.</span><span class="sxs-lookup"><span data-stu-id="c3861-107">In hello right pane, change hello **Connection type** too**Serial**.</span></span>
6. <span data-ttu-id="c3861-108">V pravém podokně hello zadejte odpovídající port COM. hello.</span><span class="sxs-lookup"><span data-stu-id="c3861-108">In hello right pane, type hello appropriate COM port.</span></span> <span data-ttu-id="c3861-109">Ujistěte se, že hello sériové konfigurační parametry nastavené takto:</span><span class="sxs-lookup"><span data-stu-id="c3861-109">Make sure that hello serial configuration parameters are set as follows:</span></span>
   
   * <span data-ttu-id="c3861-110">Rychlost: 115 200</span><span class="sxs-lookup"><span data-stu-id="c3861-110">Speed: 115,200</span></span>
   * <span data-ttu-id="c3861-111">Datové bity: 8</span><span class="sxs-lookup"><span data-stu-id="c3861-111">Data bits: 8</span></span>
   * <span data-ttu-id="c3861-112">Stop-bity: 1</span><span class="sxs-lookup"><span data-stu-id="c3861-112">Stop bits: 1</span></span>
   * <span data-ttu-id="c3861-113">Parita: žádná</span><span class="sxs-lookup"><span data-stu-id="c3861-113">Parity: None</span></span>
   * <span data-ttu-id="c3861-114">Řízení toku: žádné</span><span class="sxs-lookup"><span data-stu-id="c3861-114">Flow control: None</span></span>
     
     <span data-ttu-id="c3861-115">Tato nastavení jsou uvedené v hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="c3861-115">These settings are shown in hello following illustration.</span></span>
     
     ![Nastavení PuTTY](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > <span data-ttu-id="c3861-117">Pokud hello výchozí nastavení řízení toku nefunguje, zkuste nastavit řízení toku hello tooXON/XOFF.</span><span class="sxs-lookup"><span data-stu-id="c3861-117">If hello default flow control setting does not work, try setting hello flow control tooXON/XOFF.</span></span>
     > 
     > 
7. <span data-ttu-id="c3861-118">Klikněte na tlačítko **otevřete** toostart a sériovou relaci.</span><span class="sxs-lookup"><span data-stu-id="c3861-118">Click **Open** toostart a serial session.</span></span>

