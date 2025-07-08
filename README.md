# Bash Library

Библиотека переиспользуемых bash-функций и утилит для разработки, развертывания и управления системами.

## 🚀 Быстрый старт

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
├── tests/                        # Тесты
├── docs/                         # Документация
├── scripts/                      # Скрипты установки
├── bash-lib.sh                  # Главный файл для импорта
└── README.md
```

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

## 📚 Примеры использования

### Базовый пример
```bash
#!/usr/bin/env bash
source /usr/local/lib/bash-lib/bash-lib.sh

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

### Пример с прогресс-баром
```bash
#!/usr/bin/env bash
source /usr/local/lib/bash-lib/bash-lib.sh

echo "Обработка файлов..."
for i in {1..100}; do
    colors::progress_bar "$i" "100" "50" "Обработка"
    sleep 0.01
done
echo "Готово!"
```

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

## 🧪 Тестирование

```bash
# Запустить примеры
./examples/basic_usage.sh

# Проверить доступность функций
bash-lib --help
```

## 📖 Документация

- [API Reference](docs/api_reference.md) - Полная документация API
- [Getting Started](docs/getting_started.md) - Руководство по началу работы
- [Best Practices](docs/best_practices.md) - Лучшие практики

## 🤝 Вклад в проект

1. Форкните репозиторий
2. Создайте ветку для новой функции (`git checkout -b feature/amazing-feature`)
3. Зафиксируйте изменения (`git commit -m 'Add amazing feature'`)
4. Отправьте в ветку (`git push origin feature/amazing-feature`)
5. Откройте Pull Request

## 📄 Лицензия

Этот проект лицензирован под MIT License - см. файл [LICENSE](LICENSE) для деталей.

## 🆘 Поддержка

Если у вас есть вопросы или проблемы:

1. Проверьте [документацию](docs/)
2. Посмотрите [примеры](examples/)
3. Создайте [Issue](https://github.com/mrvi0/bash-lib/issues)

## 🔄 Версии

- **v0.1.0** - Базовая функциональность (colors, logging, validation)
- **v0.2.0** - Планируется: IO модули, системные функции
- **v0.3.0** - Планируется: Development модули, database модули 