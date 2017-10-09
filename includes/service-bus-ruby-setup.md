## <a name="create-a-ruby-application"></a><span data-ttu-id="d031b-101">Vytvoření Ruby aplikace</span><span class="sxs-lookup"><span data-stu-id="d031b-101">Create a Ruby application</span></span>
<span data-ttu-id="d031b-102">Pokyny najdete v tématu [vytvořit Ruby aplikace na platformě Azure](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="d031b-102">For instructions, see [Create a Ruby Application on Azure](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="d031b-103">Konfigurace vaší aplikace tooUse Service Bus</span><span class="sxs-lookup"><span data-stu-id="d031b-103">Configure Your application tooUse Service Bus</span></span>
<span data-ttu-id="d031b-104">toouse Service Bus, stáhnout a použít balíček hello Azure Ruby, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="d031b-104">toouse Service Bus, download and use hello Azure Ruby package, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="d031b-105">Použijte RubyGems tooobtain hello balíček</span><span class="sxs-lookup"><span data-stu-id="d031b-105">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="d031b-106">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="d031b-106">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="d031b-107">Zadejte "gem instalace azure" hello příkazového okna tooinstall hello gem a závislosti.</span><span class="sxs-lookup"><span data-stu-id="d031b-107">Type "gem install azure" in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="d031b-108">Importovat balíček hello</span><span class="sxs-lookup"><span data-stu-id="d031b-108">Import hello package</span></span>
<span data-ttu-id="d031b-109">Pomocí svém oblíbeném textovém editoru, přidejte následující toohello horní části hello Ruby hello souboru, ve kterém chcete toouse úložiště:</span><span class="sxs-lookup"><span data-stu-id="d031b-109">Using your favorite text editor, add hello following toohello top of hello Ruby file in which you intend toouse storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="d031b-110">Nastavení připojení služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="d031b-110">Set up a Service Bus connection</span></span>
<span data-ttu-id="d031b-111">Použití hello následující kód tooset hello hodnoty oboru názvů, název hello klíče, klíče, podepisující osoba a hostitele:</span><span class="sxs-lookup"><span data-stu-id="d031b-111">Use hello following code tooset hello values of namespace, name of hello key, key, signer and host:</span></span>

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

<span data-ttu-id="d031b-112">Nastavte hello obor názvů hodnotu toohello hodnotu, kterou jste vytvořili místo hello celou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d031b-112">Set hello namespace value toohello value you created rather than hello entire URL.</span></span> <span data-ttu-id="d031b-113">Například použít **"yourexamplenamespace"**, není "yourexamplenamespace.servicebus.windows.net".</span><span class="sxs-lookup"><span data-stu-id="d031b-113">For example, use **"yourexamplenamespace"**, not "yourexamplenamespace.servicebus.windows.net".</span></span>
