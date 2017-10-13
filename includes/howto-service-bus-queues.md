## <a name="what-are-service-bus-queues"></a>Co jsou fronty služby Service Bus?
Fronty služby Service Bus podporují komunikační model **zprostředkovaného zasílání zpráv**. Součásti distribuované aplikace při používání front nekomunikují navzájem přímo. Místo toho si zprávy vyměňují prostřednictvím fronty, která slouží jako zprostředkovatel. Autor zprávy (odesílatel) předá zprávu do fronty a potom pokračuje v jejím zpracování. Spotřebitel zprávy (příjemce) asynchronně přebírá zprávu z fronty a zpracovává ji. Autor nemusí čekat na odpověď od příjemce, aby mohl pokračovat se zpracováním a odesláním dalších zpráv. Fronty nabízejí doručování zpráv metodou **FIFO (First In First Out)** pro jednoho nebo několik konkurenčních spotřebitelů. To znamená, že příjemci zprávy obvykle přijímají a zpracovávají v pořadí, ve kterém byly přidány do fronty, a každou zprávu přijme a zpracuje jenom jeden příjemce zprávy.

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

Fronty služby Service Bus představují univerzální technologii, kterou můžete použít pro celou řadu scénářů:

* Komunikace mezi webovou rolí a rolí pracovního procesu ve vícevrstvé aplikaci Azure.
* Komunikace mezi místními aplikacemi a aplikacemi hostovanými v Azure v případě hybridního řešení.
* Komunikace mezi součástmi distribuované aplikace spuštěné místně v různých organizacích nebo odděleních organizace.

Použití fronty vám umožňuje snazší škálování aplikací a zvyšuje odolnost vaší architektury proti chybám.


