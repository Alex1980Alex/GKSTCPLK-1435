# Руководство по использованию каркаса в разных проектах

## 🎯 Концепция

Данный каркас предназначен для использования в любых проектах разработки 1С:Предприятие. Он содержит правила Cursor, которые можно применить к любому проекту с файлами конфигурации.

## 📁 Структура использования

```
МойПроект1С/                          # Ваш проект 1С
├── Configuration/                     # Файлы конфигурации 1С
│   ├── *.xml
│   ├── *.bsl
│   └── ...
├── cursor-framework/                  # Каркас как submodule
│   ├── .cursorrules
│   ├── cursor-rules/
│   └── ...
├── .cursorrules -> cursor-framework/.cursorrules  # Символическая ссылка
├── Действия работы Cursor/            # Рабочие файлы проекта
│   └── Ответы Cursor.md
└── README.md                          # Документация проекта
```

## 🚀 Способ 1: Git Submodules (Рекомендуется)

### Добавление каркаса в существующий проект:

```bash
# 1. Перейти в корень проекта 1С
cd /path/to/your/1c-project

# 2. Добавить каркас как submodule
git submodule add https://github.com/Alex1980Alex/1C-Enterprise-Cursor-Framework.git cursor-framework

# 3. Создать символическую ссылку на .cursorrules
# Windows (PowerShell как администратор):
New-Item -ItemType SymbolicLink -Path ".cursorrules" -Target "cursor-framework\.cursorrules"

# Linux/Mac:
ln -s cursor-framework/.cursorrules .cursorrules

# 4. Создать рабочую папку
mkdir "Действия работы Cursor"

# 5. Зафиксировать изменения
git add .
git commit -m "feat: добавлен 1C-Enterprise-Cursor-Framework"
```

### Клонирование проекта с каркасом:

```bash
# Клонирование с submodules
git clone --recursive https://github.com/your-username/your-1c-project.git

# Или если уже клонировали:
git submodule update --init --recursive
```

### Обновление каркаса:

```bash
# Обновить каркас до последней версии
cd cursor-framework
git pull origin main
cd ..
git add cursor-framework
git commit -m "update: обновлен каркас до последней версии"
```

## 🔧 Способ 2: Template Repository

### Создание нового проекта из шаблона:

1. **На GitHub:**
   - Перейти на https://github.com/Alex1980Alex/1C-Enterprise-Cursor-Framework
   - Нажать "Use this template" → "Create a new repository"
   - Указать название вашего 1С проекта

2. **Локально:**
   ```bash
   git clone https://github.com/your-username/your-new-1c-project.git
   cd your-new-1c-project
   
   # Создать папки для файлов конфигурации
   mkdir Configuration
   mkdir Configuration/Forms
   mkdir Configuration/DataProcessors
   # и т.д.
   ```

## 📋 Способ 3: Копирование правил

### Для разовых проектов:

```bash
# 1. Скачать архив каркаса
curl -L https://github.com/Alex1980Alex/1C-Enterprise-Cursor-Framework/archive/main.zip -o framework.zip

# 2. Распаковать только нужные файлы
unzip framework.zip
cp 1C-Enterprise-Cursor-Framework-main/.cursorrules ./
cp -r 1C-Enterprise-Cursor-Framework-main/cursor-rules ./
mkdir "Действия работы Cursor"

# 3. Удалить временные файлы
rm -rf framework.zip 1C-Enterprise-Cursor-Framework-main
```

## 🔄 Способ 4: Централизованные правила

### Создание общей папки правил:

```bash
# 1. Создать папку для всех каркасов
mkdir ~/CursorFrameworks
cd ~/CursorFrameworks

# 2. Клонировать каркас
git clone https://github.com/Alex1980Alex/1C-Enterprise-Cursor-Framework.git

# 3. В каждом проекте создать ссылки
cd /path/to/project1
ln -s ~/CursorFrameworks/1C-Enterprise-Cursor-Framework/.cursorrules .cursorrules

cd /path/to/project2  
ln -s ~/CursorFrameworks/1C-Enterprise-Cursor-Framework/.cursorrules .cursorrules
```

## 📝 Настройка для конкретного проекта

### Адаптация правил под проект:

1. **Создать локальный .cursorrules.local:**
   ```markdown
   # Дополнительные правила для проекта "Управление торговлей"
   
   ## Специфика проекта
   - Используемые конфигурации: УТ 11.5
   - Особенности: интеграция с WMS
   - Стандарты именования: префикс "УТ_"
   ```

2. **Подключить в основном .cursorrules:**
   ```markdown
   # В конце файла добавить:
   
   ## Локальные правила проекта
   @include .cursorrules.local
   ```

## 🎯 Рекомендуемый workflow

### Для новых проектов:
1. **GitHub Template** → создать проект из шаблона
2. Добавить файлы конфигурации 1С
3. Настроить специфичные правила в `.cursorrules.local`

### Для существующих проектов:
1. **Git Submodules** → добавить каркас
2. Создать символическую ссылку на `.cursorrules`
3. Настроить Git workflow согласно каркасу

### Для экспериментов:
1. **Копирование правил** → быстрый старт
2. При необходимости перейти на submodules

## 🔄 Обновление правил во всех проектах

### Автоматический скрипт обновления:

```bash
#!/bin/bash
# update-framework.sh

PROJECTS=(
    "/path/to/project1"
    "/path/to/project2" 
    "/path/to/project3"
)

for project in "${PROJECTS[@]}"; do
    echo "Обновляем каркас в $project"
    cd "$project"
    
    if [ -d "cursor-framework" ]; then
        cd cursor-framework
        git pull origin main
        cd ..
        git add cursor-framework
        git commit -m "update: обновлен каркас Cursor"
    fi
done
```

## 🎨 Пример структуры итогового проекта

```
МойПроект_УправлениеТорговлей/
├── Configuration/                     # 1С конфигурация
│   ├── Catalogs/
│   ├── Documents/
│   ├── DataProcessors/
│   └── Forms/
├── cursor-framework/                  # Submodule каркаса
│   ├── .cursorrules
│   ├── cursor-rules/
│   └── README.md
├── .cursorrules -> cursor-framework/.cursorrules
├── .cursorrules.local                 # Локальные правила проекта
├── Действия работы Cursor/
│   └── Ответы Cursor.md
├── .gitignore
├── .gitmodules                        # Конфигурация submodules
├── README.md                          # Документация проекта
└── CHANGELOG.md                       # История изменений
```

## 💡 Советы и лучшие практики

1. **Используйте Git Submodules** для активных проектов
2. **Template Repository** для новых проектов
3. **Локальные правила** для специфики проекта
4. **Регулярно обновляйте** каркас во всех проектах
5. **Документируйте** специфичные настройки проекта
6. **Тестируйте** правила на небольших изменениях

---

**Преимущества подхода:**
- ✅ Единая система правил для всех проектов
- ✅ Простое обновление каркаса
- ✅ Гибкость настройки под конкретный проект  
- ✅ Версионный контроль правил
- ✅ Возможность rollback при проблемах 