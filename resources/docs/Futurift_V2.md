# Интеграция и использование плагина FutuRift V2 в Unity

**💡Уважаемый студент!**

**Если что-то не работает или вы что-то не знаете - читай документацию или гуглите!!!**

---

## Описание

Аттракцион **FutuRIFT** – это высокотехнологичная кабина, установленная на подвижной платформе. Она позволяет пользователю погрузиться в виртуальную реальность благодаря возможности движения в разных направлениях. Управление аттракционом осуществляется с помощью плагина, интегрируемого в проекты Unity.

## Подключение плагина

### Способы добавления плагина в проект

1. **Добавление собранной версии плагина**
   - **Скачайте плагин**:
     - Плагин доступен в виде `.dll` файла, который можно найти на [GitHub](https://github.com/RTUITLab/Futurift-Plugin-Unity/releases).
   - **Импортируйте плагин в Unity**:
     - Переместите скачанный `.dll` файл в папку `Assets` вашего проекта Unity.
     - Рекомендуется создавать папку `Assets/Plugins` для организации плагинов.
     - Unity автоматически обнаружит плагин и позволит использовать его в проекте.

2. **Добавление исходных файлов как часть проекта**
   - **Клонируйте репозиторий**:
     - Скопируйте содержимое папки `ChairControl/ChairWork` в папку `Assets` вашего проекта (например, `Assets/ChairControl`).
   - Это позволит вам использовать и изменять основной класс плагина непосредственно в проекте.

## Использование плагина

### Базовое управление креслом

Основной класс для управления креслом — `FutuRiftController`. Он предоставляет методы и свойства для настройки и управления углами наклона (Pitch) и крена (Roll).

```csharp
class FutuRiftController
{
    float Pitch { get; set; }
    float Roll { get; set; }
    bool IsConnected { get; }
    void Start();
    void Stop();
}
```

### Управление по сети через Futurift Controller

Для управления креслом через сеть используется UDP. В этом случае контроллер принимает команды по указанным IP-адресу и порту.

```csharp
private FutuRiftController controller;

void OnEnable()
{
    controller = new FutuRiftController(new UdpOptions 
    {
        ip = "127.0.0.1", // IP компьютера, на котором запущен контроллер
        port = 6065       // Порт, на который настроен контроллер
    });
    controller.Start();
}
```

### Управление напрямую

Если вы хотите подключиться к креслу напрямую через COM-порт, используйте соответствующий конструктор.

```csharp
private FutuRiftController controller;

void OnEnable()
{
    controller = new FutuRiftController(new ComPortOptions 
    {
        ComPort = 3 // Номер порта, по которому подключено кресло
    });
    controller.Start();
}
```

### Настройка углов наклона и крена

- **Pitch**: Угол наклона кресла вперёд/назад, диапазон значений от -15 до 21 градусов.
- **Roll**: Угол наклона кресла влево/вправо, диапазон значений от -18 до 18 градусов.

При передаче значений, выходящих за пределы допустимого диапазона, они автоматически будут ограничены.

```csharp
controller.Pitch = 10;  // Устанавливаем наклон вперёд на 10 градусов
controller.Roll = -5;   // Устанавливаем наклон влево на 5 градусов
```

### Запуск и остановка контроллера

- **Start()**: Начинает отправку команд на кресло. Если соединение установлено успешно, свойство `IsConnected` будет `true`.
- **Stop()**: Останавливает отправку команд и освобождает используемые порты.

## Подключение плагина и использование с пользовательскими значениями

Этот раздел описывает, как настроить и использовать `FutuRiftController` для управления аттракционом с индивидуальными настройками.

### Шаги по настройке

1. **Создайте новый скрипт в Unity**:
   - Создайте скрипт `FutuRiftControllerScript` и откройте его.

2. **Импортируйте необходимые пространства имен**:

    ```csharp
    using UnityEngine;
    using ChairControl.ChairWork;
    using ChairControl.ChairWork.Options;
    ```

3. **Создайте класс `FutuRiftControllerScript`**:

    ```csharp
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
            var udpOptions = new UdpOptions
            {
                IpAddress = ipAddress,
                Port = port
            };
    
            var futuRiftOptions = new FutuRiftOptions
            {
                interval = interval
            };
    
            controller = new FutuRiftController(udpOptions, futuRiftOptions);
            controller.Pitch = initialPitch;
            controller.Roll = initialRoll;
            controller.Start();
        }
    
        void OnDisable()
        {
            if (controller != null)
            {
                controller.Stop();
            }
        }
    
        void Update()
        {
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

4. **Прикрепите скрипт к GameObject** и настройте параметры в инспекторе по необходимости.

## Документация

### FutuRiftController

- **Pitch**: Угол наклона платформы. Диапазон: от -15 до 21 градусов.
- **Roll**: Угол крена платформы. Диапазон: от -18 до 18 градусов.

### Основные методы

- **Start()**: Запускает передачу данных.
- **Stop()**: Останавливает передачу данных.

### FutuRiftOptions

- **interval**: Интервал передачи данных в миллисекундах.

Эти инструкции помогут вам интегрировать и использовать плагин **FutuRift V2** в ваших проектах на Unity, предоставляя возможность настроить и управлять углами наклона и крена аттракциона с использованием пользовательских значений.

## Дополнительно

- Asset - [**Fantasy Forest Environment**](https://assetstore.unity.com/packages/3d/environments/fantasy/fantasy-forest-environment-34245)
  - [https://drive.google.com/file/d/1iNPtkV2Hc3ILhkGAIvzXIUFwme3PuKyJ/view?usp=sharing](https://drive.google.com/file/d/1iNPtkV2Hc3ILhkGAIvzXIUFwme3PuKyJ/view?usp=sharing)

---
