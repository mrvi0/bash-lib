# Bash Library

Библиотека переиспользуемых bash-функций и утилит для разработки, развертывания и управления системами.

## 🌍 Выбор языка

- 🇺🇸 [English](../README.md)
- 🇷🇺 [Русский](README.md) (текущий)

## 📋 Содержание

- [🚀 Быстрый старт](#-быстрый-старт)
- [📦 Версионирование](#-версионирование)
- [📁 Структура проекта](#-структура-проекта)
- [🎯 Основные модули](#-основные-модули)
- [📚 Примеры использования](#-примеры-использования)
- [🔧 Способы подключения](#-способы-подключения)
- [🎯 Рекомендации](#-рекомендации)
- [🔧 Установка и настройка](#-установка-и-настройка)
- [🧪 Тестирование](#-тестирование)
- [📖 Документация](#-документация)
- [🤝 Вклад в проект](#-вклад-в-проект)
- [📄 Лицензия](#-лицензия)
- [🆘 Поддержка](#-поддержка)
- [🔄 Версии](#-версии)

[⬆️ К началу](#bash-library)

## 🚀 Быстрый старт

### Простое подключение (без установки)

**Самый простой способ** - подключить библиотеку прямо в скрипт одной строкой:

```bash
#!/usr/bin/env bash
# Подключить всю библиотеку одной строкой
source <(curl -fsSL https://raw.githubusercontent.com/mrvi0/bash-lib/main/bash-lib-standalone.sh)

# Теперь можно использовать все функции
logging::info "Скрипт запущен"
colors::success "Операция выполнена успешно!"

if validation::is_email "user@example.com"; then
    echo "Email валиден"
fi
```

[⬆️ К началу](#bash-library)

### Универсальный способ (рекомендуется)

Для более эффективного использования создайте функцию загрузки:

```bash
#!/usr/bin/env bash

# Универсальная загрузка bash-lib
load_bash_lib() {
    local remote_url="https://raw.githubusercontent.com/mrvi0/bash-lib/main/bash-lib-standalone.sh"
    local cache_dir="$HOME/.cache/bash-lib"
    local cache_file="$cache_dir/bash-lib-standalone.sh"
    
    # Проверить, не загружена ли уже библиотека
    if [[ -n "$__BASH_LIB_IMPORTED" ]]; then
        return 0
    fi
    
    # Создать директорию кэша
    mkdir -p "$cache_dir"
    
    # Использовать кэш если он не устарел (24 часа)
    if [[ -f "$cache_file" ]] && [[ $(($(date +%s) - $(stat -c %Y "$cache_file"))) -lt 86400 ]]; then
        source "$cache_file"
    else
        # Скачать и кэшировать
        if curl -fsSL "$remote_url" -o "$cache_file"; then
            source "$cache_file"
        else
            echo "Ошибка загрузки библиотеки"
            exit 1
        fi
    fi
}

# Загрузить библиотеку
load_bash_lib

# Использовать функции
logging::info "Скрипт запущен"
colors::success "Готово к работе!"
```

[⬆️ К началу](#bash-library)

### Установка

```bash
# Клонировать репозиторий
git clone https://github.com/mrvi0/bash-lib.git
cd bash-lib

# Установить библиотеку
sudo ./scripts/install.sh

# Активировать в текущей сессии
source ~/.bashrc
```

[⬆️ К началу](#bash-library)

### Использование

```bash
#!/usr/bin/env bash
# Подключить библиотеку
source /usr/local/lib/bash-lib/bash-lib.sh

# Использовать функции
logging::info "Начинаем работу"
colors::success "Операция выполнена успешно"

if validation::is_email "user@example.com"; then
    echo "Валидный email"
fi
```

[⬆️ К началу](#bash-library)

## 📦 Версионирование

Каждая стабильная версия библиотеки помечается git-тегом.

### Как подключить конкретную версию:

```bash
source <(curl -fsSL https://raw.githubusercontent.com/mrvi0/bash-lib/v1.0.0/bash-lib-standalone.sh)
```

### Как узнать текущую версию:

```bash
bash_lib::version
```

### Как проверить версию в скрипте:

```bash
if [[ "$__BASH_LIB_VERSION" != "1.0.0" ]]; then
    echo "Ошибка: требуется bash-lib версии 1.0.0"
    exit 1
fi
```

> Список всех версий смотри на вкладке [Releases](https://github.com/mrvi0/bash-lib/releases) или по git-тегам.

[⬆️ К началу](#bash-library)

## 📁 Структура проекта

```
bash-lib/
├── src/                          # Основной код библиотеки
│   ├── core/                     # Базовые функции
│   │   ├── colors.sh            # Цветной вывод
│   │   ├── logging.sh           # Логирование
│   │   └── validation.sh        # Валидация данных
│   ├── io/                      # Работа с файлами и вводом/выводом
│   ├── system/                  # Системные функции
│   ├── development/             # Функции для разработки
│   └── database/                # Работа с базами данных
├── examples/                     # Примеры использования
│   ├── basic_usage.sh          # Базовый пример
│   ├── simple_usage.sh         # Простое подключение
│   ├── cached_usage.sh         # С кэшированием
│   ├── local_usage.sh          # Локальный файл
│   └── universal_usage.sh      # Универсальный способ
├── tests/                        # Тесты
├── docs/                         # Документация
├── scripts/                      # Скрипты установки
├── bash-lib.sh                  # Главный файл для импорта
├── bash-lib-standalone.sh       # Единый файл для простого подключения
└── README.md
```

[⬆️ К началу](#bash-library)

## 🎯 Основные модули

### Core (Базовые функции)

#### Colors (`src/core/colors.sh`)
Цветной вывод в терминал с поддержкой различных цветов и стилей.

```bash
colors::success "Успешная операция"
colors::error "Ошибка"
colors::warning "Предупреждение"
colors::info "Информация"
colors::debug "Отладочная информация"
colors::highlight "Выделенный текст"
```

#### Logging (`src/core/logging.sh`)
Система логирования с различными уровнями и форматами.

```bash
# Установить уровень логирования
logging::set_level debug

# Логирование
logging::info "Информационное сообщение"
logging::debug "Отладочная информация"
logging::warn "Предупреждение"
logging::error "Ошибка"
logging::fatal "Критическая ошибка (с выходом)"

# Логирование в файл
logging::set_file "/var/log/myapp.log"
```

#### Validation (`src/core/validation.sh`)
Валидация различных типов данных.

```bash
# Проверка email
validation::is_email "user@example.com"

# Проверка целых чисел
validation::is_integer "123"
validation::is_positive_integer "456"

# Проверка диапазона
validation::is_in_range "50" "1" "100"

# Проверка порта
validation::is_port "8080"

# Проверка существования файла
validation::file_exists "/path/to/file"

# Комбинированная проверка
validation::all "test@example.com" validation::is_email validation::is_not_empty
```

[⬆️ К началу](#bash-library)

## 📚 Примеры использования

### Базовый пример
```bash
#!/usr/bin/env bash
source <(curl -fsSL https://raw.githubusercontent.com/mrvi0/bash-lib/main/bash-lib-standalone.sh)

# Настройка логирования
logging::set_level info
logging::set_file "/tmp/myapp.log"

# Основная логика
logging::info "Запуск приложения"

# Валидация входных данных
if ! validation::is_email "$1"; then
    colors::error "Неверный email: $1"
    logging::error "Валидация email не прошла"
    exit 1
fi

colors::success "Email валиден: $1"
logging::info "Приложение завершено успешно"
```

[⬆️ К началу](#bash-library)

### Пример с прогресс-баром
```bash
#!/usr/bin/env bash
source <(curl -fsSL https://raw.githubusercontent.com/mrvi0/bash-lib/main/bash-lib-standalone.sh)

echo "Обработка файлов..."
for i in {1..100}; do
    colors::progress_bar "$i" "100" "30" "Обработка"
    sleep 0.01
done
echo "Готово!"
```

[⬆️ К началу](#bash-library)

## 🔧 Способы подключения библиотеки

### 1. Прямая загрузка (самый простой)
```bash
source <(curl -fsSL https://raw.githubusercontent.com/mrvi0/bash-lib/main/bash-lib-standalone.sh)
```
**Преимущества:** Очень просто, всегда актуальная версия
**Недостатки:** Требует интернет при каждом запуске, медленнее

### 2. Кэширование (рекомендуется)
```bash
load_bash_lib() {
    local cache_dir="$HOME/.cache/bash-lib"
    local cache_file="$cache_dir/bash-lib-standalone.sh"
    local remote_url="https://raw.githubusercontent.com/mrvi0/bash-lib/main/bash-lib-standalone.sh"
    
    mkdir -p "$cache_dir"
    if [[ -f "$cache_file" ]] && [[ $(($(date +%s) - $(stat -c %Y "$cache_file"))) -lt 86400 ]]; then
        source "$cache_file"
    else
        curl -fsSL "$remote_url" -o "$cache_file" && source "$cache_file"
    fi
}
load_bash_lib
```
**Преимущества:** Быстро, кэширование на 24 часа, работает без интернета
**Недостатки:** Немного сложнее

### 3. Локальный файл
```bash
# Скачать файл один раз
curl -fsSL https://raw.githubusercontent.com/mrvi0/bash-lib/main/bash-lib-standalone.sh -o bash-lib-standalone.sh

# Использовать в скриптах
source "./bash-lib-standalone.sh"
```
**Преимущества:** Максимальная скорость, работает без интернета
**Недостатки:** Нужно обновлять вручную

### 4. Универсальный способ
См. пример `examples/universal_usage.sh` - комбинирует все способы с приоритетом.

[⬆️ К началу](#bash-library)

## 🎯 Рекомендации по выбору способа

| Сценарий | Рекомендуемый способ | Причина |
|----------|---------------------|---------|
| **Быстрые скрипты** | Прямая загрузка | Максимальная простота |
| **Серьезные проекты** | Кэширование | Баланс скорости и актуальности |
| **Продакшен** | Локальный файл | Надежность и скорость |
| **Разработка** | Универсальный | Гибкость |

[⬆️ К началу](#bash-library)

## 🔧 Установка и настройка

### Автоматическая установка
```bash
# Установка в стандартные директории
sudo ./scripts/install.sh

# Установка в пользовательские директории
./scripts/install.sh -d ~/.local/lib/bash-lib -b ~/.local/bin
```

### Ручная установка
```bash
# Копировать файлы
sudo cp -r src /usr/local/lib/bash-lib/
sudo cp bash-lib.sh /usr/local/lib/bash-lib/

# Добавить в .bashrc
echo 'source /usr/local/lib/bash-lib/bash-lib.sh' >> ~/.bashrc
source ~/.bashrc
```

[⬆️ К началу](#bash-library)

## 🧪 Тестирование

```bash
# Запустить примеры
./examples/basic_usage.sh
./examples/simple_usage.sh
./examples/cached_usage.sh
./examples/local_usage.sh
./examples/universal_usage.sh

# Проверить доступность функций
bash-lib --help
```

[⬆️ К началу](#bash-library)

## 📖 Документация

- [API Reference](../api_reference.md) - Полная документация API
- [Getting Started](../getting_started.md) - Руководство по началу работы
- [Best Practices](../best_practices.md) - Лучшие практики

[⬆️ К началу](#bash-library)

## 🤝 Вклад в проект

1. Форкните репозиторий
2. Создайте ветку для новой функции (`git checkout -b feature/amazing-feature`)
3. Зафиксируйте изменения (`git commit -m 'Add amazing feature'`)
4. Отправьте в ветку (`git push origin feature/amazing-feature`)
5. Откройте Pull Request

[⬆️ К началу](#bash-library)

## 📄 Лицензия

Этот проект лицензирован под MIT License - см. файл [LICENSE](../../LICENSE) для деталей.

[⬆️ К началу](#bash-library)

## 🆘 Поддержка

Если у вас есть вопросы или проблемы:

1. Проверьте [документацию](../)
2. Посмотрите [примеры](../../examples/)
3. Создайте [Issue](https://github.com/mrvi0/bash-lib/issues)

[⬆️ К началу](#bash-library)

## 🔄 Версии

- **v1.0.0** - Первый стабильный релиз с полным функционалом
  - Цветной вывод и форматирование
  - Система логирования с уровнями
  - Валидация данных (email, числа, файлы, порты)
  - Системные утилиты (проверка интернета, информация о системе)
  - Универсальные функции (подтверждения, заголовки, прогресс-бары)
  - Совместимость со старыми функциями
- **v1.1.0** - Планируется: IO модули, расширенные системные функции
- **v1.2.0** - Планируется: Development модули, database модули

[⬆️ К началу](#bash-library) 