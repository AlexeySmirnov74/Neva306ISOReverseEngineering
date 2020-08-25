# Neva306 IS0 Reverse Engineering
Электросчетчик НЕВА 306 IS0 Трехфазный. Изменение показаний в микросхеме памяти

# Что нужно
1. Разобрать счетчик (снять пластиковую оболочку и все пломбы)
2. [Программатор CH341Pro](https://alexeysmirnov74.github.io/Neva306IS0ReverseEngineering/Images/ch341a.jpg "Программатор CH341Pro")
3. [Datasheet AT24C02C](https://alexeysmirnov74.github.io/Neva306IS0ReverseEngineering/DOCS/AT24C02C-PUM-Atmel-datasheet.pdf "Datasheet AT24C02C")
3. [AutoIT](https://www.autoitscript.com/site/wp-content/uploads/2013/02/download_autoit_106x51@2x.png "AutoIT") & [SciTE](https://www.autoitscript.com/cgi-bin/getfile.pl?../autoit3/scite/download/SciTE4AutoIt3.exe "SciTE")
4. Паяльник
5. [Провода Dupont соединительные (для Arduino)](https://alexeysmirnov74.github.io/Neva306IS0ReverseEngineering/Images/40PCS-Dupont-wire-jumper-cables-20cm-2.jpg "Провода Dupont соединительные (для Arduino)")

# Сборка
Счетчик не должен быть подсоединен к сети. Питать процессор счетчика и микросхему памяти мы будем от программатора. Начинаем паять по схеме, представленной ниже:


![Схема сборки для считывания данных из чипа электросчетчика НЕВА 306](https://alexeysmirnov74.github.io/Neva306IS0ReverseEngineering/Images/PinOutCh341a-Neva306ISO.fw.png "Схема сборки для считывания данных из чипа электросчетчика НЕВА 306")


Вот как это вышло у меня:

![Схема подключения](https://alexeysmirnov74.github.io/Neva306IS0ReverseEngineering/Images/begin.jpg "Схема подключения")

# RoadMap

Первые попытки прошить микросхему мусором из рандомных значений, в надежде, а вдруг по-быстрому получится изменить значения - закончились неудачей. Счетчик показывал Error. Тогда будем следовать такому правилу. Что нужно, чтобы значение 1.2 кВт изменилось на 1.3 кВт. 
Подключаем нагрузку в виде чайника и убиваем сразу 2 зайцев: завариваем чай, так как ночка обещает быть долгой, и изменяем тики счетчика.  Хорошо, что можно сохранить память микросхемы в файл и восстановить когда нужно. 
Итак, каждое мигание крайнего справа светодиода красным изменяло значение в памяти. Поэтому, сколько значений в таблице - столько раз я включал и отключал чайник, чтобы светодиод моргнул только 1 раз. Затем я считывал значения и записывал в файлик для дальнейшего анализа. 

|  кВт |  Значение 1 | Значение 2 | Тики |
| ------------ | ------------ | ------------ | ------------ |
|1.2|41|7F|00|
|1.2|41|7F|01|
|1.2|41|7F|02|
|1.2|41|7F|03|
|1.2|41|7F|04|
|1.2|41|7F|05|
|1.2|41|7F|06|
|1.2|41|7F|07|
|1.2|40|80|00|
|1.2|40|80|01|
|1.2|40|80|02|
|1.2|40|80|03|
|1.2|40|80|04|
|1.2|40|80|05|
|1.2|40|80|06|
|1.2|40|80|07|
|1.2|40|81|00|
|1.2|40|81|01|
|1.2|40|81|02|
|1.2|40|81|03|
|1.2|40|81|04|
|1.2|40|81|05|
|1.2|40|81|06|
|1.2|40|81|07|

Значение 1.2 кВт изменилось на 1.3 кВт. Это значит, что мы будем теперь сидеть еще полночи и пить чай до тех пор, пока 1.3 кВт. не станет 1.4 кВт. И эксперимент можно завершать и дальше анализировать байты.

|  кВт |  Значение 1 | Значение 2 | Тики |Просто считаем|
| ------------ | ------------ | ------------ | ------------ | ------------ |
|1.3|3f|82|00|1|
|1.3|3f|82|01|2|
|1.3|3f|82|02|3|
|1.3|3f|82|03|4|
|1.3|3f|82|04|5|
|1.3|3f|82|05|6|
|1.3|3f|82|06|7|
|1.3|3f|82|07|8|
|1.3|3f|83|00|9|
|1.3|3f|83|01|10|
|1.3|3f|83|02|11|
|1.3|3f|83|03|12|
|1.3|3f|83|04|13|
|1.3|3f|83|05|14|
|1.3|3f|83|06|15|
|1.3|3f|83|07|16|
|1.3|3e|84|00|17|
|1.3|3e|84|01|18|
|1.3|3e|84|02|19|
|1.3|3e|84|03|20|
|1.3|3e|84|04|21|
|1.3|3e|84|05|22|
|1.3|3e|84|06|23|
|1.3|3e|84|07|24|
|1.3|3e|85|00|25|
|1.3|3e|85|01|26|
|1.3|3e|85|02|27|
|1.3|3e|85|03|28|
|1.3|3e|85|04|29|
|1.3|3e|85|05|30|
|1.3|3e|85|06|31|
|1.3|3e|85|07|32|
|1.3|3d|86|00|33|
|1.3|3d|86|01|34|
|1.3|3d|86|02|35|
|1.3|3d|86|03|36|
|1.3|3d|86|04|37|
|1.3|3d|86|05|38|
|1.3|3d|86|06|39|
|1.3|3d|86|07|40| <- [Вот так выглядит прошивка для этой строчки](https://alexeysmirnov74.github.io/Neva306IS0ReverseEngineering/Images/tik.png "Вот так выглядит прошивка для этой строчки")
|1.3|3d|87|00|41|
|1.3|3d|87|01|42|
|1.3|3d|87|02|43|
|1.3|3d|87|03|44|
|1.3|3d|87|04|45|
|1.3|3d|87|05|46|
|1.3|3d|87|06|47|
|1.3|3d|87|07|48|
|1.3|3c|88|00|49|
|1.3|3c|88|01|50|
|1.3|3c|88|02|51|
|1.3|3c|88|03|52|
|1.3|3c|88|04|53|
|1.3|3c|88|05|54|
|1.3|3c|88|06|55|
|1.3|3c|88|07|56|
|1.3|3c|89|00|57|
|1.3|3c|89|01|58|
|1.3|3c|89|02|59|
|1.3|3c|89|03|60|
|1.3|3c|89|04|61|
|1.3|3c|89|05|62|
|1.3|3c|89|06|63|
|1.3|3c|89|07|64|
|1.3|3b|8a|00|65|
|1.3|3b|8a|01|66|
|1.3|3b|8a|02|67|
|1.3|3b|8a|03|68|
|1.3|3b|8a|04|69|
|1.3|3b|8a|05|70|
|1.3|3b|8a|06|71|
|1.3|3b|8a|07|72|
|1.3|3b|8b|00|73|
|1.3|3b|8b|01|74|
|1.3|3b|8b|02|75|
|1.3|3b|8b|03|76|
|1.3|3b|8b|04|77|
|1.3|3b|8b|05|78|
|1.3|3b|8b|06|79|
|1.3|3b|8b|07|80|

После очередного тика результат стал 1.4 кВт. 

Прошивки разных значений находятся [в этой папке.](https://github.com/AlexeySmirnov74/Neva306IS0ReverseEngineering/tree/master/Firmware%20for%20CH341A%20pro "в этой папке."). Вы можете загрузить их и делать свои экспеименты. 

Создаем небольшой скриптец, в котором продолжаем увеличивать киловатты и смотреть когда же выскочит ошибка, а точнее, когда будет переполнение байтов и в какую сторону запишутся новые значения. 

```java
;1.3
$kvt = 2.5
$out = ""
$HexVerh = "0xfa" ; 0x0000010
$HexVniz = "0x03" ; 0x0000011
$HexVerh = "0xfa" ; 0x0000012

$HexVniz = "0x03" ; 0x0000013


$HexVerh1 = "0x00"    ; 0x0000002
$HexVniz1 = "0x80"    ; 0x0000003

$HexVerh11 = "0x00"    ; 0x00000ea
$HexVniz11 = "0x80"    ; 0x00000eb

$HexVerh2 = "0x00"     ; 0x0000001
$HexVerh22 = "0x00"    ; 0x00000e9
$i = 1
$count = 0
$counterforhexvniz = 0
$trigger1 = 0
$trigger2 = 0
$set = 0

$ba = 0
While 1
	For $pik = 0 To 7
		$count = $count + 1
		$counterforhexvniz = $counterforhexvniz + 1
		$tekpik = "0x" & $pik
		If $set = 1 And (Hex($HexVerh1, 2) = "04" And Hex($HexVniz1, 2) = "7E" And Hex($HexVniz, 2) = "01" And Hex($HexVerh, 2) = "FF") Or ($ba = 1) Then
			;		if $set=1 and $kvt>=668.0 then
			$ba = 1
			MsgBox(0, " chn=" & $counterforhexvniz & " set=" & $set & " " & $count, $kvt & @CRLF & "0x01|0xe9=" & Hex($HexVerh2, 2) & @CRLF & "0x02=" & Hex($HexVerh1, 2) & @CRLF & "0x03=" & Hex($HexVniz1, 2) & @CRLF & "0x10=FF|0x11=01" & @CRLF & "0x12=" & Hex($HexVerh, 2) & "|0x13=" & Hex($HexVniz, 2) & @CRLF & "0xea=" & Hex($HexVerh11, 2) & @CRLF & "0xeb=" & Hex($HexVniz11, 2) & @CRLF & "count=" & Hex($tekpik, 2))
		EndIf
	Next
	$HexVerh = $HexVerh + $i
	If $HexVniz = 0x01 And Hex($HexVerh, 2) = "FF" Then
		MsgBox(0, $count, $kvt & @CRLF & "0x01|0xe9=" & Hex($HexVerh2, 2) & @CRLF & "0x02=" & Hex($HexVerh1, 2) & @CRLF & "0x03=" & Hex($HexVniz1, 2) & @CRLF & "0x10=FF|0x11=01" & @CRLF & "0x12=" & Hex($HexVerh, 2) & "|0x13=" & Hex($HexVniz, 2) & @CRLF & "0xea=" & Hex($HexVerh11, 2) & @CRLF & "0xeb=" & Hex($HexVniz11, 2) & @CRLF & "count=" & Hex($tekpik, 2))            ;				EndIf
	EndIf

	;если прошло 16 импульсов то переключаем байт который идет вниз
	If $counterforhexvniz = 16 Then
		$counterforhexvniz = 0
		$HexVniz = $HexVniz - $i
		;if 0x13=0x00 then
		If $HexVniz = 0x00 Then
			$HexVniz = "0x80" ;0x13=0x80
			$HexVerh1 = $HexVerh1 + $i ;0x02=0x02+1
			$HexVerh11 = $HexVerh11 + $i ;0xea=0xea+1
			;668.1
			If Hex($HexVerh1, 2) = "05" And Hex($HexVniz1, 2) = "7E" And Hex($HexVerh, 2) = "00" And Hex($HexVniz, 2) = "80" And $set = 1 Then
				$trigger1 = 1
			EndIf

			If $trigger1 = 1 Then
				$trigger1 = 0
				$HexVniz1 = $HexVniz1 - $i ;0x03=0x03-1
				$HexVniz11 = $HexVniz11 - $i ;0xeb=0xeb-1
				;0x03
				If $HexVniz1 = 0x00 Then
					$set = 1
					$HexVniz1 = "0x80"
					$HexVniz11 = "0x80"

					If $trigger2 = 0 Then
						$trigger2 = 1
						$HexVerh2 = $HexVerh2 + $i
						$HexVerh22 = $HexVerh22 + $i
					Else
						$trigger2 = 0
					EndIf
				EndIf
			ElseIf $trigger1 = 0 Then
				$trigger1 = 1
			EndIf
		EndIf
	EndIf
	;если прошло 80 импульсов то киловатты увеличиваются на 0.1
	If $count = 80 Then
		$count = 0
		$kvt = $kvt + 0.1
		$kvt = Round($kvt, 1)
	EndIf
WEnd

```
Скрипт каждый раз выводит значение киловатт и какие байты следует изменить в прошивке для того, чтобы эти кВт записались правильно. Да, можно автоматизировать полностью весь процесс, но задачи такой не было. 

![Counter](https://alexeysmirnov74.github.io/Neva306IS0ReverseEngineering//Images/counter.png "Counter")


Результат после прошивки:

![Результат после прошивки](https://alexeysmirnov74.github.io/Neva306IS0ReverseEngineering/Images/result.jpg "Результат после прошивки")

# Что можно было бы сделать дальше
Добавить wi-fi или bluetooth для удаленного управления (заморозка, сброс, увеличение, уменьшение) значениями.

# Какие гипотезы не подтвердились
Изменить количество импульсов (800/1600 и т.д.) в чипе нельзя. Их там просто нет. 
В чипе не хранится разбивка значений по фазам.
Пьезоэлемент от зажигалки визуально может влиять только если корпус вскрыт. Но никакого сбоя при нескольких десятках попытках с пьезо не было. При закрытом корпусе никакого влияния он не оказывает. 
Катушка [Tesla](https://alexeysmirnov74.github.io/Neva306IS0ReverseEngineering/Images/DC-12-30-ZVS-Tesla-flyback-driver-SGTC-Marx.jpg "Tesla") с длиной дуги до 10 см также не влияет при закрытом корпусе. Дуга не может пройти сквозь корпус и выбить микросхему. Слишком велико расстояние. 

Ребят, я честно пытался разными способами :)

Выводы: В собранном корпусе счетчик надежно защищен от каких-либо электромагнитных воздействий. На это влияет ширина пластика и 3D сборка платы. 
