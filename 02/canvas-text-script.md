Кроме отрисовки фигур канвас поддерживает работу с текстом. Вывод текста осуществляется одним из двух методов: fillText или strokeText, которые выводят, соответственно, залитый или обведённый текст.

Параметры текста задаются свойствами контекста font, textAlign и textBaseline. Напишем сообщение Тахомой и увеличим размер шрифта до 30px.

Часть текста не влезла в экран, поэтому поменяем свойство textBaseline, чтобы выровнять текст так, чтобы верхняя часть текста начиналась с указанных координат.

Стоит упомянуть одну особенность текстов на канвасе: они не переносятся. Если написать длинный текст, даже включающий в себя символы переноса строки (\n), канвас вытянет текст в одну строчку.

Чтобы написать текст с переносами, нужно каждую новую строку вынести в отдельный вызов метода fillText (или strokeText).