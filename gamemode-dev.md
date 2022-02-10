# Vanilla
Эта информация может помочь быстрее понять структуру игрового мода.
Игровой мод Vanilla полностью написан на pawn, но включает в себя следующие плагины:
* `crashdetect`
* `mysql`
* `sscanf` 
* `pawncmd` 
* `streamer` 
* `pawnregex`

Командный процессор: **`Pawn.CMD`**.

Диалоговый процессор: **`easyDialog`**.

-----

## Структура проекта

Все файлы исходного кода находятся в `gamemodes/` с расширением `.inc` для заголовков или `.pwn` для реализаций и делятся на два типа:
### Контроллеры
Являются входной точкой для действий игроков. Контроллеры отвечают за сбор необходимой 
информации и представление её(информации) пользователям. Файлы исходных кодов 
контроллеров отмечаются знаком `@` перед названием файла *(Пример: **@inv.p**)*.

Пример простого контроллера:

`@example.p`
```pawn
// Для использования хуков подключите этот инклюд в каждом файле.
#include <YSI\y_hooks>

// Стандартное использование хука
hook OnPlayerConnect(playerid) {
  SendSuccMessage(playerid, "Привет!");
}

CMD:ame(playerid, params[]) {
  // Благодаря специальному макросу указываем как именно пользователь должен использовать команду.
  @Syntax("/ame [действие]", "s[144]", params[0]);
  SetPlayerChatBubble(playerid, params[0], COLOR_ME, SPEECH_DISTANCE, 6000);
  return true;
}
```

### Сервисы
По своей сути служат расширением стандартного api, содержат в себе бизнес-логику: алгоритмы, 
хранение данных(как в базе данных так и в оперативной памяти). Каждый сервис должен начинаться с
`.h` расширения и содержать реализацию возможности многократного подключения. 
```pawn
#if defined EXAMPLE_SERVICE
  #endinput
#else
  #define EXAMPLE_SERVICE
#endif
```
Кроме того, заголовок содержит все структы сервиса а так же описание его методов с комментариями.
Свойства структуры должны начинаться с нижнего подчеркивания `_`, а функции должны начинаться с названия
сервиса и знака `@`, затем указывается название метода в `UpperCamelCase` для публичных методов и c `__` 
(двойное подчеркивание) для приватных.
```pawn
#define MAX_EXAMPLE 10
enum example_struct {
  _x,
  _y
}
new example_info[MAX_EXAMPLE][example_struct];

// Возвращает x по ключу
forward Example@GetX(id);
// Возвращает y по ключу
forward Example@GetY(id);

// Задает x и y для указанного ключа.
// Не возвращает ничего
forward Example@Set(id, x, y);
```

----

Во время разработки вы можете использовать встроенные в `utils.p` вспомогательные функции и макросы.

### Стилизация сообщений
Существует три базовые функции для упрощения SendClientMessage
```pawn
// Сообщение игроку стилизованное под ошибку
#define SendErrMessage(%0,%1) 		SendClientMessage(%0, COLOR_ERR, %1)

// Сообщение игроку стилизованное под положительный ответ
#define SendSuccMessage(%0,%1) 		SendClientMessage(%0, COLOR_SUCC, %1)

// Сообщение игроку стилизованное под информирование
#define SendInfoMessage(%0,%1)		SendClientMessage(%0, COLOR_INFO, %1)
```
Если цвет части текста необходимо изменить вы можете использовать необходимый макрос
```pawn
#define			COLOR_RED                   0xFF8080EE
#define			STR_COLOR_RED               "{FF8080}"

#define			COLOR_GRAY                  0xAFAFAFAA
#define 		STR_COLOR_GRAY              "{AFAFAF}"			

#define			COLOR_GRAY_FF               0xAFAFAFFF
#define 		STR_COLOR_GRAY_FF           "{AFAFAF}"				

#define			COLOR_GRAY_EE               0xAFAFAFEE
#define 		STR_COLOR_GRAY_EE           "{AFAFAF}"				

#define			COLOR_BLUE                  0x1273FFEE
#define 		STR_COLOR_BLUE              "{1273FF}"			

#define 		COLOR_LIGHTBLUE             0x5ED7FFEE
#define 		STR_COLOR_LIGHTBLUE         "{5ED7FF}"				

#define 		COLOR_YELLOW                0xFFFF45EE
#define 		STR_COLOR_YELLOW            "{FFFF45}"				

#define 		COLOR_WHITE                 0xFFFFFFEE
#define 		STR_COLOR_WHITE             "{FFFFFF}"			

#define 		COLOR_GREEN                 0x33AA33EE
#define 		STR_COLOR_GREEN             "{33AA33}"			

#define 		COLOR_PAPAYA                0xF5DEB3EE
#define 		STR_COLOR_PAPAYA            "{F5DEB3}"				

#define 		COLOR_SALMON                0xFF8282EE
#define 		STR_COLOR_SALMON            "{FF8282}"				

#define 		COLOR_PURPLE                0x8080FFEE
#define 		STR_COLOR_PURPLE            "{8080FF}"				

#define 		COLOR_DEEP_BLUE             0x54BDFCEE
#define 		STR_COLOR_DEEP_BLUE         "{54BDFC}"				

#define 		COLOR_ORANGE                0xFF8000EE
#define 		STR_COLOR_ORANGE            "{FF8000}"				

#define 		COLOR_LEMON                 0xFFFACDEE
#define 		STR_COLOR_LEMON             "{FFFACD}"			

#define 		COLOR_YELLOW_GREEN          0x33AA33EE
#define 		STR_COLOR_YELLOW_GREEN      "{33AA33}"					

#define 		COLOR_LIGHT_CYAN            0xC8DDDDEE
#define 		STR_COLOR_LIGHT_CYAN        "{C8DDDD}"					

#define			COLOR_ME                    0xC2A2DAEE
#define 		STR_COLOR_ME                "{C2A2DA}"			

#define			COLOR_LSPD                  0x8d8dffEE
#define 		STR_COLOR_LSPD              "{8d8dff}"			

#define			COLOR_FACTIONCHAT           0x8080ffEE
#define 		STR_COLOR_FACTIONCHAT       "{8080ff}"

#define			COLOR_BLACK                 0x000000EE
#define 		STR_COLOR_BLACK             "{000000}"

// -----------------------

#define 		COLOR_ERR                   COLOR_RED
#define 		STR_COLOR_ERR               STR_COLOR_RED

#define 		COLOR_SUCC                  COLOR_GREEN
#define 		STR_COLOR_SUCC              STR_COLOR_GREEN

#define 		COLOR_INFO                  COLOR_PAPAYA
#define 		STR_COLOR_INFO              STR_COLOR_PAPAYA
```

### Форматирование `__()`, `___()` 
Этот макрос (двойное подчеркивание) позволяет использовать форматирование в одну строку с использованием 
глобального буфера (2048 char).
Пример:
```pawn
SendInfoMessage(playerid, __("Это сообщение %s, %d", "отформатировано", 1));
```
В некоторых случаях может потребоваться локальный буфер, для этого используйте тройное подчеркивание `___()` и переменную `__local_buffer`
```pawn
new __local_buffer[128];
SendInfoMessage(playerid, ___("Это сообщение %s локальным буфером!", "отформатировано"));
```

### Equals(const string[], const string2[])
Сравнивает две строки, возвращает bool

### @Trigger
Этот макрос предназначен для создания события и перехвата его в нескольких местах с `hook`.

`character.p`
```pawn
// Когда персонаж авторизован, отправляем оповещение всем обработчикам
@Trigger("CharacterLoged", "d", self);
```

`@inv_textdraw.p`
```pawn
#include <YSI\y_hooks>

// Перехватываем указанное выше событие в другом файле
hook OnCharacterLoged(playerid) {
    PlayerTextDrawSetString(playerid, td_player_info[0][playerid], character_info[playerid][_name]);
    return Y_HOOKS_CONTINUE_RETURN_1;
}
```

### @Syntax
Макрос позволяет указывать как именно пользователь должен использовать команду, 
макрос статично обращается к переменной `playerid`, так что побеспокойтесь об этом. 
`@Syntax` внутри использует `sscanf`, так что его использование просто необходимо если у команды есть аргументы.
```pawn
CMD:test(playerid, params[]) {
  new myint, mystring[128];
  @Syntax("/test [int] [string]", "ds[144]", myint, mystring);
  // Теперь в myint первый аргумент команды и в mystring второй.
  // Если игрок будет использовать команду неверно -- увидит соответствующую ошибку.
  ...
  return 1;
}
```

### @Admin
Так же для ограничения команд существует этот макрос, который указывает какой уровень администратора необходим для 
использования текущей команды.

```pawn
CMD:gopos(playerid, params[]) {
	@Admin(2);
	new Float:x, Float:y, Float:z;
	@Syntax("/gopos [x] [y] [z]", "fff", x, y, z);
	SetPlayerPos(playerid, x, y, z);
	return true;
}
```
Таким образом команда `gopos` станет доступна только для администраторов 2 уровня и выше.

Укажите 2 аргументом `false`, если выход на дежурство не нужен.
```pawn
CMD:aduty(playerid) {
	@Admin(1, false);
	user_info[playerid][_aduty] = !user_info[playerid][_aduty];
	Character@UpdateColor(playerid);
	if(user_info[playerid][_aduty]) {
		SendAdminMessage(COLOR_GRAY, __("%s заступил на дежурство.", AdminName(playerid)));
	} else {
		SendAdminMessage(COLOR_GRAY, __("%s покинул дежурство.", AdminName(playerid)));
		SendSuccMessage(playerid, "Вы покинули дежурство.");
	}

	return 1;
}
```
