git 4deba2bfca6636d5cdcede3f2068eff3b59c15ce

---

# Локализация

- [Введение](#introduction)
- [Файлы локализации](#language-files)
- [Основы использования](#basic-usage)
- [Формы множественного числа](#pluralization)
- [Сообщения валидации](#validation)
- [Перекрытие файлов локализации из пакетов](#overriding-package-language-files)

<a name="introduction"></a>
## Введение

Фасад `Lang` даёт возможность удобного получения языковых строк, позволяя вашему приложению поддерживать несколько языков интерфейса.

<a name="language-files"></a>
## Файлы локализации

Файлы локализации хранятся в папке `resource/lang` Внутри неё должны располагаться подпапки - языки, поддерживаемые приложением:

	/resources
		/lang
			/en
				messages.php
			/es
				messages.php

#### Пример файла локализации

Файлы локализации возвращают массив пар ключ/значение:

	<?php

	return array(
		'welcome' => 'Добро пожаловать на мой сайт!'
	);

Язык по умолчанию указан в файле настроек `config/app.php`. Вы можете изменить текущий язык во время работы вашего приложения методом `App::setLocale`:

	App::setLocale('es');

#### Резервный язык локализации

Вы также можете установить резервный язык локализации - в случае, если для основного языка нет вариантов перевода, будет браться строка из резервного файла локализации. Обычно это английский язык, но вы можете это поменять. Настройка находится в файле `config/app.php`:

	'fallback_locale' => 'en',

<a name="basic-usage"></a>
## Основы использования

#### Получение строк из языкового файла

	echo Lang::get('messages.welcome');

Первый компонент строки (до точки), передаваемый методу `get` - имя языкового файла, а после указывается имя строки, которую нужно получить.

> **Примечание:** если строка не найдена, то метод `get` вернёт её путь (ключ).

Вы также можете использовать функцию `trans` - короткий способ вызова метода `Lang::get`:

	echo trans('messages.welcome');

#### Замена параметров внутри строк

Сперва определите параметр в языковой строке:

	'welcome' => 'Welcome, :name',

Затем передайте массив вторым аргументом методу `Lang::get`:

	echo Lang::get('messages.welcome', array('name' => 'Dayle'));

#### Проверка существования языковой строки

	if (Lang::has('messages.welcome'))
	{
		//
	}

<a name="pluralization"></a>
## Формы множественного числа

Формы множественного числа - проблема для многих языков, так как все они имеют разные сложные правила для их получения. Однако вы можете легко справиться с ней в ваших языковых файлах используя символ «|» для разделения форм единственного и мнжественного чисел.

	'apples' => 'There is one apple|There are many apples',

Для получения такой строки используется метод `Lang::choice`:

	echo Lang::choice('messages.apples', 10);

Вы можете указать не два, а несколько вариантов выражения множественного числа:	

	echo Lang::choice('товар|товара|товаров', $count, array(), 'ru');

Благодаря тому, что Laravel использует компонент Symfony Translation вы можете легко создать более точные правила для проверки числа:

	'apples' => '{0} There are none|[1,19] There are some|[20,Inf] There are many',

<a name="validation"></a>
## Сообщения валидации

О том, как использовать файлы локализации для сообщений валидации, смотрите соответствующий раздел [документации](/docs/5.0/validation#localization). 

<a name="overriding-package-language-files"></a>
## Перекрытие файлов локализации из пакетов

Многие пакеты идут со своими файлами локализации. Вы можете «перекрыть» их, располагая файлы в папках `resources/lang/packages/{locale}/{package}`. Например, если вам надо перекрыть файл `messages.php` пакета `skyrim/hearthfire`, путь до вашего файла локализации должен выглядеть так: `resources/lang/packages/en/hearthfire/messages.php`. Нет нужды дублировать файл целиком, можно указать только те ключи, которые должны быть перекрыты.
