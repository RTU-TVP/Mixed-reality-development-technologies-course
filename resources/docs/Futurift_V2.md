# Капсула Futurift V2

💡 **Если что-то не работает или вы что-то не знаете - читай документацию или гуглите!!!**

---

## Описание

Аттракцион **FutuRIFT** – это футуристическая кабина, установленная на подвижной платформе, и стойка оператора. Его уникальная форма и функциональные возможности привлекают многих людей, стремящихся испытать это устройство. Платформа позволяет двигаться в разных направлениях, погружая пользователя в виртуальную реальность и делая его активным участником происходящих событий.

---

## Подключение плагина и базовое использование

- **Скачайте плагин**:
  - Плагин представлен в виде файла с расширением `.unitypackage`.
    - Плагин можно найти в [Realease](https://github.com/RTU-TVP/Mixed-reality-development-technologies-course/releases).
- **Импортируйте ресурс в проект**:
  - **Откройте Unity**.
  - **Импортируйте пакет**:
    - Перейдите в меню **Assets** > **Import Package** > **Custom Package**.
    - Найдите скачанный `.unitypackage` файл и выберите его.
    - Нажмите **Import** в появившемся окне, чтобы импортировать пакет в проект.
- **Прикрепите компонент**:
  - Выберите объект, к которому хотите прикрепить обработчик.
  - В инспекторе, нажмите **Add Component** и найдите `FuturiftHandler`.
  - Прикрепите компонент `FuturiftHandler` к выбранному объекту.

    На этом базовая настройка завершена. Обработчик `FuturiftHandler` идеально подходит для использования в аттракционах или играх, где игрок управляет транспортным средством, требующим отслеживания поворотов, например, космическим кораблем или роботом.

---

## Подключение плагина и использование с пользовательскими значениями

Это руководство предназначено для разработчиков, использующих Unity, которые хотят настроить `FutuRiftController` для управления аттракционом **FutuRIFT** с помощью пользовательских параметров.

- **Скачайте плагин**:
  - Плагин представлен в виде файла с расширением `.unitypackage`.
    - Плагин можно найти в [архиве файлов](https://www.notion.so/7ea6f3d3fef44a88a243808a2e1db08a?pvs=21).
- **Импортируйте ресурс в проект**:
  - **Откройте Unity**.
  - **Импортируйте пакет**:
    - Перейдите в меню **Assets** > **Import Package** > **Custom Package**.
    - Найдите скачанный `.unitypackage` файл и выберите его.
    - Нажмите **Import** в появившемся окне, чтобы импортировать пакет в проект.

**Шаги по настройке**

1. **Создайте новый скрипт в Unity**:
    - В Unity, в панели проекта, щелкните правой кнопкой мыши и выберите **Create** > **C# Script**.
    - Назовите скрипт `FutuRiftControllerScript` и откройте его в вашем редакторе кода (например, Visual Studio).
2. **Импортируйте необходимые пространства имен**:
    - В начале файла `FutuRiftControllerScript.cs` добавьте ссылки на необходимые пространства имен.

    ```csharp
    using UnityEngine;
    using ChairControl.ChairWork;
    using ChairControl.ChairWork.Options;
    ```

3. **Создайте класс `FutuRiftControllerScript`**:
    - Измените созданный скрипт, чтобы он выглядел следующим образом:

    ```csharp
    csharpКопировать код
    public class FutuRiftControllerScript : MonoBehaviour
    {
        private FutuRiftController controller;
    
        // Настройки для UDP-соединения
        public string ipAddress = "127.0.0.1";
        public int port = 6065;
    
        // Настройки для углов
        public float initialPitch = 0.0f;
        public float initialRoll = 0.0f;
    
        // Настройки интервала передачи данных
        public int interval = 100;
    
        void Start()
        {
            // Создаем экземпляр UdpOptions
            var udpOptions = new UdpOptions
            {
                IpAddress = ipAddress,
                Port = port
            };
    
            // Создаем экземпляр FutuRiftOptions
            var futuRiftOptions = new FutuRiftOptions
            {
                interval = interval
            };
    
            // Создаем экземпляр FutuRiftController
            controller = new FutuRiftController(
                udpOptions: udpOptions,
                futuRiftOptions: futuRiftOptions
            );
    
            // Настраиваем начальные значения углов
            controller.Pitch = initialPitch;
            controller.Roll = initialRoll;
    
            // Запускаем контроллер
            controller.Start();
        }
    
        void OnDisable()
        {
            // Останавливаем контроллер, когда скрипт отключается
            if (controller != null)
            {
                controller.Stop();
            }
        }
    
        void Update()
        {
            // Пример обновления углов наклона и крена на основе ввода пользователя
            // Это может быть заменено логикой вашего проекта
            if (Input.GetKeyDown(KeyCode.UpArrow))
            {
                controller.Pitch += 1;
            }
            if (Input.GetKeyDown(KeyCode.DownArrow))
            {
                controller.Pitch -= 1;
            }
            if (Input.GetKeyDown(KeyCode.LeftArrow))
            {
                controller.Roll -= 1;
            }
            if (Input.GetKeyDown(KeyCode.RightArrow))
            {
                controller.Roll += 1;
            }
        }
    }
    ```

4. **Прикрепите скрипт к GameObject**:
    - В Unity, выберите объект в сцене, к которому вы хотите прикрепить этот скрипт.
    - В инспекторе нажмите **Add Component** и найдите `FutuRiftControllerScript`.
    - Прикрепите компонент `FutuRiftControllerScript` к выбранному объекту.
5. **Настройте параметры в инспекторе**:
    - После прикрепления скрипта вы увидите поля для настройки IP-адреса, порта, начальных углов и интервала передачи данных. Измените их по необходимости.

**Замечания**

- **Обработка ввода**: Пример кода показывает, как можно обновлять углы на основе ввода пользователя. Вы можете заменить это специальной логикой вашего проекта.
- **Обработка исключений**: Добавьте обработку исключений для более надежного кода, особенно при работе с сетью.
- **Тестирование**: Перед использованием на реальном оборудовании, убедитесь, что все работает правильно в вашей тестовой среде.

Эти шаги помогут вам интегрировать и использовать `FutuRiftController` в вашем проекте Unity, обеспечивая управление углами наклона и крена аттракциона **FutuRIFT** с пользовательскими значениями.

---

## Документация

### FutuRiftController

Этот класс используется для управления аттракционом **FutuRIFT** через различные методы передачи данных, такие как UDP. Он позволяет задать углы наклона (Pitch) и крена (Roll), а также периодически отправлять данные устройству.

**Основные свойства:**

- **Pitch**:
  - Описание: Угол наклона платформы.
  - Диапазон: от -15 до 21 градусов.
  - Пример использования: `controller.Pitch = 10;`
- **Roll**:
  - Описание: Угол крена платформы.
  - Диапазон: от -18 до 18 градусов.
  - Пример использования: `controller.Roll = -5;`

**Основные методы:**

- **Конструкторы**:
  - `FutuRiftController(UdpOptions udpOptions, FutuRiftOptions futuRiftOptions = null)`
    - Описание: Инициализирует контроллер с использованием настроек UDP.
    - Пример использования: `var controller = new FutuRiftController(udpOptions);`
- **Start()**:
  - Описание: Запускает передачу данных устройству.
  - Пример использования: `controller.Start();`
- **Stop()**:
  - Описание: Останавливает передачу данных устройству.
  - Пример использования: `controller.Stop();`

### FutuRiftOptions

Класс для настройки дополнительных параметров FutuRIFT.

**Основные свойства:**

- **interval**:
  - Описание: Интервал передачи данных в миллисекундах.
  - Пример использования: `futuRiftOptions.interval = 100;`

---

## Дополнительно

- Asset - [**Fantasy Forest Environment**](https://assetstore.unity.com/packages/3d/environments/fantasy/fantasy-forest-environment-34245)
  - <https://drive.google.com/file/d/1iNPtkV2Hc3ILhkGAIvzXIUFwme3PuKyJ/view?usp=sharing>

---
