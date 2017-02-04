###NVIDIA
#### Maxwell (750, 945-980), GM???
##### Asus GTX 980, 4G (чёрный, Strix directCUII).
Определяется PCIe-устройство, но не читается его BIOS. При запуске nouveau с BIOS из файла появляется картинка, но артефачит красноватыми полосками во framebuffer. В dmesg - таймауты.

##### Gainward GTX 980, 4GB (чёрно-коричневый).
При инициализации драйверов nvidia подвисает (даже от nvidia-smi). Если до этого проводился тест памяти  - после инициализации драйвера некоторые буквы становтся белыми. При этом на nouveau показывает даже furmark, но медленно. При подключении x1 такая же ситуация. Обнаружен сколотый конденсатор на pcie. На в 10 раз сниженной частоте памяти работает. На 3ды пониженной частоте памяти ошибки памяти модуля M6. подготовлена под замену модуля М6, вздулись треврдотельные конденсаторы. Сопротивление GPU 4.6JV (норма), на памяти окло 20 Ом.


##### Msi 750 2GB (чёрный)
Сопротивление менее 0.1 Ом как по линии питания памяти, так и по линии питания процессора. Снят GPU. На плате КЗ исчезло. Проверено что на снятом GPU КЗ по линии питания памяти.

#### Fermi (6??, 650-730, 7??), GK???
##### Evga 680 2GB GDDR5 (чёрный)
Пришла с прогаром транзистора по питанию памяти и ШИМом памяти RT8807BGQW с дыркой (по идее аналог APW7088). Также были отломаны некоторые ножки на gs7103 (low-dropout regulator, один из них снят, возможно аналогичен RT9018B, APL5920). После удаления транзистора, дросселей и конденсатора осталось 2Ом на входе PCIE 12в (сильно греется в районе прогара) м 0.03Ом на питании паямти. Поиск места КЗ указал на район C672, хотя он сам не виноват. Основное подозрение на чип паямти на обратной стороне от него. Снят чип памяти M5. Оказались пробиты как VDD (0.42 Ом), так и VDDQ (0.14 Ом). Сопротивление по питанию памяти незначительно увеличилось (0.042 Ом). M6, M2 сняты, сопротивления такого же порядка. M7 - снят, но повреждён не так сильно: сопротивление VVDQ 11 Ом. После этого сопротивление по питаню памяти составило 0.25 Ом. При подаче номинального напряжения памяти в 1.6v ток достиг 2А и термопара показал что температура M8 на 4 градуса выше остальных. После его снятия сопротивление составило порядка 10-1 Ом (при подаче 0.3-2v). Gри подаче 2v ток былнаибольший нагрев-на GPU ~ на 4 градуса. То есть течёт через какие-то сложные элементы и можно пытаться в таком виде запустить, так как перегрва при нормальном напряжении не происходит. Замыкание на PCIE устранено ковырянием в районе прогара с особым вниманием на ноки, соединяющие стороны платы. При запуске карты с подключённым дополнительным питанием появляется 0.89В на GPU.

##### Msi 680 2GB GDDR5 (чёрный)
После старта все напряжения есть, но даже при наличии интегрированного видео - 1 длинных 2 коротких. После этого иногда определялась в nvflash. BIOS был нулевого размера. Перепршит BIOS. После этого не пищит, как будто стартует. После запуска Xorg даже показывает изображение, но с уклоном в синий (нет красного). Бублик работал, но периодически вырубалась (изображение пропадает, вентиляторы крутятся быстрее). Иногда система не стартует, особенно если подключен монитор. при подключении  к другому монитору по dvi синевы нет, а вырубания просиходят в случайном месте, независимо от нагрузки. после вырубания напряжения нет ни на памяти, ни на gpu. БП не просаживается.
Возле линии Analog red обнаружен повреждённый белый элемент, половина которого марирована чёрным.
Имеет скол и в отличие от соседних не нулевое, а бесконченое сопротивление.
обнаружен и заменен расколотый резистор на линии детектирования присутствия доп питания. теперь стартует с подключеным монитором, тесты пройдены, работает.

##### Palit GTX760 2G GDDR5 (тёмно-коричневый)
Обрыв в предохранителе L2 с маркировкой X (8 Ампер). Вызван КЗ в верхнем плече микросхемы U16. Стоящие рядом аналогичные микросхемы отличаются, как будто их уже меняли. Сопротивления в норме, GPU ~12 Ом. После выпаивания пробитой фазы и установки предохранителя - стала включаться. Заменена фаза питания. Запускается, но при загрузке драйвера возникают подвисания и сообщение об ошибке Xid 8. BIOS был не родной. Прошит на обновлённую версию - подвисания отслись, но уже не при запуске awesome. Выяснилось что при низких напряениях (0.89) - подвисания и артефакиы в 2D, при высоких (1.25)- только остановки после начал работы 3d. 2D работает. Найдены сбитые элементы на обратной стороне, 3шт. Сопротивления на посадочных местах: 898  Ом (Вход 3.3), 8.5КОм  (по входу доп. питания), 24КОм (по входу другого доп. питания). Сбитые конденсаторы возвращены, проверено на другой мат-плате и на x1 - результат не изменился. Тест памяти прямой записью в PCIe BAR пройден. ocl_memtest выдаёт ошибку при запуске. Прямое чтение памяти во время подвисания работает, т.е. видеокарта отвечает на запросы. PGOOD основного ШИМа в норме, а PGOOD системы питания 1.05v присутствует, но имеет очень слабое подтягивание вверх, которое гасится осциллографом. Также возможен непропай 2-й ножки главного ШИМ. Но наибольшее подозрение - несоответствие напряжения установленному в VBIOS. Возможно виноват DrMOS U13 (последний не заменённый на SIC780), который не открывает нижнее плечо.

##### Gigabyte GTX660 3G GK106-400-A1 (тёмно-коричневый)
Имеет прогар в одной из фаз питания GPU. Однако сопротивление GPU по линии питания ~1.1Ом, что по идее меньше сопротивления  аналогичных карт. По питанию памяти и 3.3v с платы - сопортивление нулевое. Также КЗ на транзисторе стоящем на месте прогара. Поиск места кз не производился. Также вероятно сбит C79. 3.3v идут на как минимум на BIOS и на U508.

##### Palit GTX650 1024M GDDR5 (тёмно-коричневый)
Замыкание по линии 12v с разъёма pcie, сопротивления порядка 0.1Ом. В питании GPU напряжение с pcie не используется, используется только для питания памяти. Более чем 2-лапых элементов немного, все не вызвали подозрений. Наибольшие подозрения вызвал бОльший конденсатор непосредственно в районе прихода 12v с платы. был обнаружен пробитый конденсатор на линии 12v c pcie (рядо с pcie), заменен на больший. отремонтировано.

##### ASUS GTX650Ti 1GBDDR5 (чёрный)
Артефакты в 3d (unexpected spikes). Дебаггинг показал что по всей видимости некорректно работают шейдеры на последнем по индексу ядре из 4х. Перепрошивка на K2100M из DOS оказалась нерабочей. Удалось вернуть закоротив CE на VCC при загрузке, и CE на GND перед прошивкой для обхода проверки.

#### Fermi (420 - 625, 6??), GF???
##### Gigabyte 590 2x1536MB GDDR5 (чёрный)
Получена от мастера, как донор памяти Samsung HC04, 1GBit. Тестирование GPU, имеющего видеовыход, говорит об проблемном модуле памяти. До памяти второго GPU достучаться не удалось. Падает в ядре при запросе nvidia-smi или при загрузке драйвера.

##### Gainward 570 1280MB GDDR5 NE5X570010DA-1100 (коричневая)
кз в верхнем плече первой фазы питания gpu. под транзистором прогар. транзистор снят. Причина КЗ - в MOSFET driver U2. Сопротивление GPU 0.5Ом. При поиске КЗ прожёгся элемент рядом с нераспаянной SU3 (чёрный маркирован 1-0). Рядом c ШИМ оторвано два конденсатора. При попытке включения кратковременная попытка подать напряжение на GPU.

##### Gainward 570 1280MB GDDR5 mHDMI (чёрный)
В текстовом режиме некоторые символы мусорные. При загрузке драйвера сыпет ошибки и падает драйвер. Программный анализ показал, что взводится 2-й бит (пробелы превращаются в $) для многих байтов. Поочерёдный RESET модулей показал, что по всей видимости пролема в неполучении данных при работе с одной дорожки микросхемы памяти M3 (Samsung 037 K4G10325FE HС05). После качания M3 на шарах ситуация изменилась - теперь не взыодится второй, а опускается первый бит. Отпаяна совсем. После отпаивания симптомы мало изменились, даже запустился awesome, но больше артефактов и ошибок Xid. После замены чипа памяти проблемы исчезли: 3d запускается. Без родного охлаждения сильно греется. На стандартном BIOS через несколько десятков секунд работы Furmark появляется потрескивающий звук и через несколько секунд перезагрузка системы. Причина по всей видиомсти в том, что при пиковых нагрузках данная карта превышает лимиты питания (что "исправляется" в Windows-драйверах путём понижения параметров карты для Furmark). На benchnark "Piano" тоже приходит к перезагрузке, но несколько позже. Многие другие держит нормально. После подключения к двум БП (300+450) Furmark работает без проблем, карта полностью рабочая, продано.

##### Zotac GTX560 (чёрный)
Работает, но сильно греется даже при хорошем теплоотводе. Вероятно надо скальпировать. Скальпировано, собрано без крышки; теперь температура не превышает 75 (при запуске без корпуса). Работает. скальпирована удачно, собрана без крышки и продана.

##### Palit GTX560 OC 1GB GDDR5 (коричневый)
Проблемы с одним из битов памяти в M6, XID 44 при загрузке драйвера. First address for 0b100000: 0x106. После замены этого чипа памяти работает, температура GPU в норме. Модель NE5X560THD02-1143F. Карточка требует устойчивого БП, более мощный основной подключался к боле требовательному крайнему 6-pin. Работает. продана

##### Gainward GTX465 (коричневый)
Работает, но в редких случаях не инициализируется. на плате множество снятых конденсаторов, но работает 

##### Palit GTX550Ti 1GB GDDR5 (коричневый)
Вначале были артефакты шрифтов в текстовом режиме. Но потом вообще перестала определяться: при старте запускается с интегрированным видео, хотя все напряжения есть. БЫло обнаружено, что сопротивление по 3.3v необычно низкое: 40Ом. При подаче 3.3v - ток ~0.3А, что даже меньше чем на рабочих картах. Так что на КЗ под GPU это не похоже.

##### Palit GTX 560Ti 2GB GDDR5 (красный)
При загрузке драйвера сыпет ошибки и тормозит драйвер, который с другой картой работает нормально. По словам продавца - проблемы под нагрузкой. Плата пратически идентична Palit GTX460 Sonic. Программа тестирования памяти находит проблемы при случайном тестировании с незагруженным драйвером. Проблема локализована в чипе паяти SM8 (SAMSUNG K4G10325FE-HC04), при помощи ZQ и RESET.

##### Zotac GTX470 1280MB GDDR5 (чёрный)
Получена из сервиса, где была донором с наклейкой GTX465. Есть пайка и снятые элементы, по крайней мере 3. КЗ по дополнительному питания 12v в одном из разъёмов. КЗ вызвано прогаром дорожек под транзистором. После устранения транзистора вместе со всеми дрожками под ним - осталось 3 фазы мз 4х, КЗ нет, но и старта gpu нет. Сопротивление на gpu 0.3Ом, работоспособность gpu не ясна. Память 8 Hynix H5GQ1H24AFR-T0C


##### ???????(zotac?) GTX460 (черная, матовая)
прогревалась, флюс под gpu

Обуглились элементы с нулевым сопротивлением и обозначением SLB (видимо примерно в роли предохранителеей). Обнаружено КЗ всех лапок транзисторов соответстующей фазы. перепаяна соотвествующая фаза с заменой подозрительных элементов. починено, продано.

##### Palit GTX460 Sonic (красная)
До выпаяивания ШИМ. Разные симпотомы сменяли друг друга:
 * питание gpu было несколько минут, потом исчезало. gpu был горячий. Все фазы работали нормально.
 * не было питания на gpu

До выпаивания gpu:
 * замкнуто 3.3v видеоплаты, напрямую приходящие с PCIe, на одну ногу кварца, очень странно
 * странный шум по вольтажу на 2-х управляющих ногах ШИМа. Ичтоник шума - дорожка идущая по верхней стороне платы и уходящая в плату
Кварц рабочий, но не активировался. Проверялось при выпаянном ШИМ.

Выпаян gpu.
Замыкание на плате на кварц исчезло. На gpu найдены контакты, сопротивлени которых до ножки кварца ~4Ом.

На карте жжёные дорожки под чипом, gpu потенциально рабочий

#### Tesla (9300 - 405) G9?, GT???
##### XFX GTX280 1GB GDDR3 (чёрный)
Был красный светодиод, до выпаивания транзистора Q502, который по идее отвечает за детектирование наличия дополнительно питания. После этого светодиод стал зелёный, но не определялась: система загружается со встроенным видео. Но 1 раз за пустилсь и через пару секунд монитор погас. При измерении есть питание на GPU, видеопамяти и 5v DVI. Быстро становится тёплая в зоне основных транзисторов питания.

##### Asus GTX280 1GB GDDR3 (чёрный)
Плата идентична XFX GTX280. Сткртует, артефакты в BIOS (точки, не те символы). При подключение pcie x1 артефактов стало как будто больше. Q502 на месте. При последующем включении - запах гари и появилось КЗ на 6-м дросселе (который не по питанию GPU)

##### Leadtek GTS260 896MB GDDR3 (чёрный)
Дорожки такия же как на 280. Продавалась как в неизветсном состоянии. При длительном миспользовании проблем не выявлно кроме возможно редких зависаний, возможно не от неё. Работает.

##### Asus GTS250 1GB GDDR3 (cиний)
При старте на памяти нет питания. Нет контакта на предохранителе на 8А рядом с разъёмом дополнительного питания. При замыкании предохранителя есть старт, идёт ток порядка 1А. Предположительно это была единственная проьлема. Дополнительное питание используется только для памяти. Подозрительно низкое сопортивление до земли с ключа транзистора - плеча, отвечающего за соединение с землёй. Но может с ним и всё впорядке, т.к. непосредственно рядом с ШИМ памяти UP6101BU8 находится резистора между LGATE и GND. предохрантель заменен на ВП1 с аналогичными параметрами. починено. Тест на MSI 9642 был нестабилен, предположительно из-за нехватки питания с разъёма то мат. платы. На нормальной плате с тем же БП проблем нет - работает стабильно. продана

##### Asus GTS250 1GB GDDR3 (cиний)
карта от студента миэта. который снял с нее множество элементов, втч шим, твердотельные конденсаторы, транзисторы. все поставлено назад, кроме пробитых транзисторов. Отсутствует шим памяти, включение без него привело к предположительному выходу из строя двух транзисторов рядом.на шим памяти есть донор.

##### Asus GTS250 512MB GDDR3 (cиний)
Получена из сервиса. КЗ по питанию после дросселей, причём как по питанию GPU, так и по питанию памяти. Большие чёрные элементы eS8, на обратной стороне gpu - конденсаторы, не выпаивались. Исследование падение напряжений при токе порядка 1А, поданного на замкнутые элементы указывает на то что область замыкания близка к конденсаторам на обратной стороне gpu. Причём касается это как линии питани gpu (проверилось на самых больших конденсаторах под gpu) так и линии питания памяти (проверилось на вторых по размеру конденсаторах под gpu). НА обратной стороне чипа 1 сбитый элемент. Ясно, что это не причина КЗ, но надо иметь ввиду. Основная гипотеза - КЗ под gpu.

##### Zotac GTS250 512MB GDDR3 (синий)
На второй-третий раз исследований внезапно вырубился БП по защите. Что до этого - неизвестно.
При последующих включениях бывал сильный запах. Иcтоник не был найден, было найдено просаживание по 3.3v pcie.
Выпаяна линейная система питания, потенциально проблемные конденсаторы. КЗ по питанию после линейной системы - осталось.

Сопротивление ооочень маленькое, при подаче 2 ампер ничего не греется. Замеры падений напряжений по дорожкам указывают на дырку в плате с оратной стороны gpu, предположительно кз в шарах или подложке. Есть сколы, но небольшие. Ранее в районе gpu в термопасте была найдена мелкая странная крошка металла (или кремния).

##### PNY 9800 512MB GDDR3 (зелёный)
Редко были замечены трудноразличимые артефакты в виде точек. Долго работала и продолжает работать. Из тестов артефакты можно увидеть только в LightMark. отдана.

### AMD/ATI
#### Southern Islands HD7750 - HD7970, R9 270, R9 280, R7 240, R7 250
##### Sapphire HD 7950 3GDDR5(коричневая)
Получена с диагнозом "нет изображения, прогрев не помог". Есть следы от прогрева. На практике вырубалась из-за отсутствия термопасты. ocl_memtest стартует нестабильно, но это похоже на проблему opencl-драйверов. Других проблем не обнаружено - работает. Тестирует Женя

##### PowerColor R7850 1GDDR5(красная)
Все напряжения есть, но Video Bios не читается при загрузке и не определяется PCIe-устройство. Обнаружено отсуствие POK на APL5932A.

#### Northern Islands HD6450, HD6570, HD6670, HD6790 - HD6990, HD64xxM, HD67xxM, HD69xxM, HD7450 - HD7670
##### MSI R6850 1GDDR5(коричневая)
Получена с описанием "не работет поле того как подключили монитор на горячую". Стартует, показывает по VGA, но не определяет edid монитора. Проблема оказалась в драйвере fglrx. C radeon разрешение по VGA подхватывется. Так что карта полностью работает, никаких проблем нет. Продано.

##### MSI AMD HD6870 1GDDR5(коричневая)
При попытке использовать ATI-драйвера - виснет через разное небольшое время. Обнаружен отвал конденсатора C304 рядом с микросхемой линейного стабилизатора uP7701u8, расположенного между VOUT и FB ногами. Замеры ёмкости - 0.6nF при 0.560 между щупами. 3d иногда запускается, но через некоторое время карта останавливается, исччезает напряжение на gpu, из-за того что перестаёт подаваться сигнал enable на основной ШИМ. Дальнейшие измерения показали что его исчезновение по всей видимости вызвано открытием транзистора Q210. Внезапно проблема перестала проявляться, карта работает, вероятно проблема в какой-то защитной схеме, которая выключает сигнал Enable. Тестировалась около 3-х месяцев, проблем не возникло. Однако после очередной замены системы охлаждения после запуска Furmark стали появляться проблемы с выводом картинки (сдвиг отобрааемой зоны и мусор).

#### Evergreen HD5430 - HD5970, Some HD 6xxx
##### XFX HD6770 1GDDR5(чёрная)
Карта стартует нестабильно, обычно вырубается (исчезает напряжение на gpu и памяти) почти сразу после старта.  При запуске с закороченным BIOS на gpu стабильно 1.25v и можно прошить по сети. При запуске с незакороченным BIOS напряжение растёт до 1.3 (в это время карта работает) а потом падает в ноль. Перепрошивка BIOS на меньшее напряжение не дала результата

#### Radeon R700 HD4330 - HD5165, HD5xxV
##### HD 4870 1GB GDDR5 (красная)
Иногда один модуль памяти начинает выдавать мусор (pattern 0B10101010). Адрес при тестировании от начала - порядка 0x307, 0x308. Модули Hynix H5GQ1H24MJR T0C. Тыканием щупа включенного осциллографа на точку имеющую 1.6Ком  удалось вызвать помехи на модуле и локализовался модуль U4. замена на аналогичный сип с донора (470 с прогаром) не помогло, вызывая редкие артефакты. вернули снятый прежде чеп памяти, карта работает и отдана

#### Radeon R600 (HD 2xxx, HD 3xxx) Seriesse
##### HIS HD 3450 (синяя, мелкая)
Всегда работала и работает
