# Подвижная платформа 2DOF

💡 **Если что-то не работает или вы что-то не знаете - читай документацию или гуглите!!!**

---

## Введение

**Подвижная платформа** - это кокпит, установленный на подвижную базу, который с помощью мощных электроприводов создает ощущения перегрузки. Универсальная подвижная игровая платформа с сидением, имитирующая перегрузки для авто- и авиа-симуляторов. Это активный кокпит или подвижное кресло с минимальным откликом и реалистичными ощущениями, идеально подходящее для использования с VR-гарнитурами (Oculus, HTC, HP Reverb, Pimax и др.) или с большими экранами и системами Triple Screen. Платформа работает как аттракцион виртуальной реальности.

Основные характеристики:

- Подвижная игровая платформа на **2-х приводах** с гоночным сидением.
- Поддержка авто-авиа-симуляторов (2 в 1), с углами до 15 градусов и максимальным весом пилота до **110 кг**.
- Режимы работы: движение вперед-назад и влево-вправо.

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
  - Выберите объект, к которому хотите прикрепить телеметрию.
  - В инспекторе, нажмите **Add Component** и найдите `CarTelemetryHandler_Sample`.
  - Прикрепите компонент `CarTelemetryHandler_Sample` к выбранному объекту.
- **Настройте ссылки**:
  - В компоненте `CarTelemetryHandler_Sample`, привяжите ссылки на:
    - **Transform**: Установите ссылку на компонент `Transform` объекта для считывания его поворотов по всем осям.
    - **Rigidbody**: Установите ссылку на компонент `Rigidbody` объекта для считывания его скоростей по всем осям.

> Примечание: Компоненты Transform и Rigidbody в Unity предоставляют информацию о положении, поворотах и движении объекта, что важно для точного сбора телеметрических данных.

На этом базовая настройка завершена. Этот обработчик телеметрии идеально подходит для игр, где игрок управляет автомобилем или другим транспортным средством, требующим отслеживания поворотов и скоростей.

---

## Подключение плагина и использование со своими значениями

### Шаги для подключения и использования с кастомными значениями

1. **Скачайте плагин**:
    - Плагин представлен в виде файла с расширением `.unitypackage`.
        - Плагин можно найти в [Realease](https://github.com/RTU-TVP/Mixed-reality-development-technologies-course/releases).
2. **Импортируйте ресурс в проект**:
    - **Откройте Unity**.
    - **Импортируйте пакет**:
        - Перейдите в меню **Assets** > **Import Package** > **Custom Package**.
        - Найдите скачанный `.unitypackage` файл и выберите его.
        - Нажмите **Import** в появившемся окне, чтобы импортировать пакет в проект.
3. **Создайте кастомный компонент для обработки телеметрии**:
    - Вместо использования `CarTelemetryHandler_Sample`, вы можете создать свой собственный класс, унаследованный от `MonoBehaviour`, для обработки специфических данных телеметрии.

### Пример кастомного компонента

```csharp
using System.Collections;
using _2DOF.Core;
using UnityEngine;

public class CustomTelemetryHandler : MonoBehaviour
{
    [SerializeField] private Transform targetTransform;  // Ссылка на Transform объекта
    [SerializeField] private Rigidbody targetRigidbody;  // Ссылка на Rigidbody объекта

    private ObjectTelemetryData _telemetryData;  // Экземпляр для хранения телеметрических данных
    private Coroutine _telemetryCoroutine;  // Корутина для обновления данных

    private void Start()
    {
        // Инициализация данных телеметрии
        _telemetryData = new ObjectTelemetryData();

        // Создание и установка экземпляра SendingData
        SendingData.Instance = new SendingData(_telemetryData);
    }

    private void OnEnable()
    {
        // Запуск корутины для обновления данных телеметрии
        _telemetryCoroutine = StartCoroutine(UpdateTelemetryData());

        // Запуск отправки данных
        SendingData.Instance.SendingStart();
    }

    private void OnDisable()
    {
        // Остановка корутины
        StopCoroutine(_telemetryCoroutine);

        // Остановка отправки данных
        SendingData.Instance.SendingStop();
    }

    private IEnumerator UpdateTelemetryData()
    {
        const float updateInterval = SendingData.WAIT_TIME / 1000f;  // Интервал обновления в секундах

        while (true)
        {
            // Проверка наличия данных телеметрии
            if (_telemetryData == null)
            {
                yield return new WaitForSeconds(updateInterval * 10f);
                continue;
            }

            // Обновление углов поворота
            if (targetTransform != null)
            {
                var rotation = targetTransform.rotation;
                _telemetryData.AnglesX = rotation.eulerAngles.x > 180 
                  ? rotation.eulerAngles.x - 360 
                 : rotation.eulerAngles.x;
                _telemetryData.AnglesZ = rotation.eulerAngles.z > 180 
                  ? rotation.eulerAngles.z - 360 
                 : rotation.eulerAngles.z;
                _telemetryData.AnglesY = rotation.eulerAngles.y > 180 
                  ? rotation.eulerAngles.y - 360 
                  : rotation.eulerAngles.y;
            }

            // Обновление скоростей
            if (targetRigidbody != null)
            {
                var velocity = targetRigidbody.velocity;
                _telemetryData.VelocityX = velocity.x;
                _telemetryData.VelocityY = velocity.y;
                _telemetryData.VelocityZ = velocity.z;
            }

            yield return new WaitForSeconds(updateInterval);
        }
    }
}
```

### Инструкции по использованию кастомного компонента

1. **Добавьте новый скрипт в Unity**:
    - В окне **Project**, щелкните правой кнопкой мыши и выберите **Create** > **C# Script**.
    - Назовите скрипт, например, `CustomTelemetryHandler`.
2. **Откройте скрипт в редакторе**:
    - Скопируйте и вставьте пример кода выше в файл `CustomTelemetryHandler.cs`.
3. **Прикрепите новый компонент к объекту**:
    - Выберите объект, к которому хотите прикрепить новый скрипт.
    - Нажмите **Add Component** и найдите `CustomTelemetryHandler`.
    - Прикрепите компонент `CustomTelemetryHandler` к выбранному объекту.
4. **Настройте ссылки**:
    - В компоненте `CustomTelemetryHandler`, привяжите ссылки на:
        - **Transform**: Установите ссылку на компонент `Transform` объекта для считывания его поворотов по всем осям.
        - **Rigidbody**: Установите ссылку на компонент `Rigidbody` объекта для считывания его скоростей по всем осям.

---

## Документация

Плагин предназначен для сбора и отправки телеметрических данных объекта, таких как углы поворота и скорости движения по разным осям. Он состоит из двух основных классов:

1. **ObjectTelemetryData** - класс, представляющий данные телеметрии объекта.
2. **SendingData** - класс, ответственный за отправку данных в отображаемый в памяти файл.

Эти классы могут использоваться для мониторинга и обработки данных движения объекта в режиме реального времени.

### Класс ObjectTelemetryData

**Описание**

Класс **ObjectTelemetryData** используется для хранения данных телеметрии объекта. Он включает углы поворота и скорости движения по трем осям (X, Y, Z).

**Свойства**

- **AnglesX**: Угол поворота объекта по оси X.
- **AnglesZ**: Угол поворота объекта по оси Z.
- **AnglesY**: Угол поворота объекта по оси Y.
- **VelocityZ**: Скорость объекта по оси Z.
- **VelocityX**: Скорость объекта по оси X.
- **VelocityY**: Скорость объекта по оси Y.
- **DataArray**: Массив, содержащий все данные телеметрии объекта. Возвращает массив вида `[AnglesX, AnglesZ, AnglesY, VelocityZ, VelocityX, VelocityY]`.

**Методы**

- **Reset()**: Сбрасывает все данные телеметрии до нуля.
- **ToString()**: Возвращает строковое представление текущих данных телеметрии.

**Пример использования**

```csharp
csharpКопировать код
// Создание экземпляра ObjectTelemetryData
var telemetryData = new ObjectTelemetryData();

// Заполнение данных
telemetryData.AnglesX = 30.0f;
telemetryData.AnglesY = 45.0f;
telemetryData.AnglesZ = 60.0f;
telemetryData.VelocityX = 5.0f;
telemetryData.VelocityY = 10.0f;
telemetryData.VelocityZ = 15.0f;

// Сброс данных
telemetryData.Reset();

// Получение строкового представления данных
Console.WriteLine(telemetryData.ToString());

```

### Класс SendingData

**Описание**

Класс **SendingData** используется для передачи данных телеметрии в отображаемый в памяти файл, что позволяет другим приложениям или процессам получать доступ к этим данным.

**Свойства**

- **Instance**: Статический экземпляр класса **SendingData**.
- **MAP_NAME**: Имя ячейки памяти, которая будет создана (по умолчанию "2DOFMemoryDataGrabber").
- **WAIT_TIME**: Время ожидания между отправками данных (в миллисекундах, по умолчанию 20 мс).

**Методы**

- **SendingStart()**: Запускает процесс отправки данных.
- **SendingStop()**: Останавливает процесс отправки данных.

**Пример использования**

```csharp
csharpКопировать код
// Создание экземпляра ObjectTelemetryData
var telemetryData = new ObjectTelemetryData();

// Создание экземпляра SendingData
var sendingData = new SendingData(telemetryData);

// Запуск отправки данных
sendingData.SendingStart();

// Остановка отправки данных
sendingData.SendingStop();
```

### Отладка данных с использованием класса HandlerDataDebug

**Описание**

Класс `HandlerDataDebug` предоставляет функционал для отладки данных телеметрии, отправляемых в файл, отображаемый в памяти (memory-mapped file). Этот класс полезен для вывода текущих данных телеметрии в консоль, что упрощает процесс отладки и позволяет убедиться, что данные передаются и обновляются корректно.

**Основные методы**

1. **PrintStart()**:
    - Запускает отдельный поток, который периодически считывает данные из файла, отображаемого в памяти, и выводит их в консоль.
2. **PrintStop()**:
    - Останавливает поток, который выводит данные в консоль.
3. **PrintData(MemoryMappedFile memoryMappedFile)**:
    - Считывает данные из отображаемого файла и выводит их в консоль.

**Пример использования**

```csharp
csharpКопировать код
// Запуск вывода данных в консоль
HandlerDataDebug.PrintStart();

// Ваш основной код здесь...

// Остановка вывода данных в консоль
HandlerDataDebug.PrintStop();
```

**Подробное описание методов**

```csharp
csharpКопировать код
using System;
using System.IO.MemoryMappedFiles;
using System.Threading;
using UnityEngine;

namespace _2DOF.Core
{
    /// <summary>///     Этот класс используется для отладки данных, отправляемых в файл, отображаемый в памяти.
    /// </summary>public static class HandlerDataDebug
    {
        private static Thread _thread;

        /// <summary>///     Запуск вывода данных в консоль.
        /// </summary>public static void PrintStart()
        {
            // Остановка предыдущего потока (если был запущен)
            PrintStop();

            // Создание и запуск нового потока для вывода данных
            _thread = new Thread(() =>
            {
                while (true)
                {
                    // Открытие существующего файла, отображаемого в памяти
                    using var memoryMappedFile = MemoryMappedFile.OpenExisting(SendingData.MAP_NAME);

                    // Считывание и вывод данных
                    PrintData(memoryMappedFile);

                    // Ожидание перед следующей итерацией
                    Thread.Sleep(SendingData.WAIT_TIME);
                }
            });

            // Старт потока
            _thread.Start();
        }

        /// <summary>///     Остановка вывода данных в консоль.
        /// </summary>public static void PrintStop()
        {
            // Прерывание и остановка текущего потока
            _thread?.Abort();
        }

        /// <summary>///     Вывод данных в консоль.
        /// </summary>private static void PrintData(MemoryMappedFile memoryMappedFile)
        {
            try
            {
                // Создание представления для доступа к данным файла
                using var accessor = memoryMappedFile.CreateViewAccessor();

                // Считывание данных в массив байтов
                var bytes = new byte[accessor.Capacity];
                accessor.ReadArray(0, bytes, 0, bytes.Length);

                // Преобразование байтов в строку для вывода
                var str = $"AnglesX: {bytes[0]}, "
                          + $"AnglesZ: {bytes[1]}, "
                          + $"AnglesY: {bytes[2]}, "
                          + $"VelocityZ: {bytes[3]}, "
                          + $"VelocityX: {bytes[4]}, "
                          + $"VelocityY: {bytes[5]}";

                // Вывод строки в консоль Unity
                Debug.Log(str);
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
                throw;
            }
        }
    }
}
```

**Как использовать класс HandlerDataDebug в вашем проекте**

1. **Добавьте `HandlerDataDebug` в проект**:
    - Скопируйте приведенный выше код и добавьте его в свой проект Unity.
2. **Запустите отладку**:
    - Вызовите метод `HandlerDataDebug.PrintStart()` в нужном месте вашего кода для начала вывода данных.
3. **Остановите отладку**:
    - Вызовите метод `HandlerDataDebug.PrintStop()` для остановки вывода данных.

**Важные замечания**

- **Работа с потоками**: Класс `HandlerDataDebug` использует отдельный поток для вывода данных. Будьте внимательны при работе с потоками, чтобы избежать конфликтов и некорректной работы приложения.
- **Memory-Mapped Files**: Этот метод предполагает, что файл, отображаемый в памяти, уже создан и доступен для чтения. Убедитесь, что `SendingData.MAP_NAME` соответствует имени файла, который вы хотите отладить.
- **Производительность**: Постоянный вывод данных в консоль может повлиять на производительность вашего приложения, особенно при частых обновлениях. Используйте этот метод только для отладки и при необходимости.

### Заключение

Данный плагин предоставляет гибкость и позволяет адаптировать сбор и передачу телеметрических данных под конкретные нужды вашего проекта. Важно тестировать и отлаживать настройки, чтобы убедиться, что данные телеметрии собираются и отправляются корректно.
