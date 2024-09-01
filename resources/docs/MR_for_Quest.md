# Пример создание MR проекта под Quest 2/3

<aside>
💡 **Если что-то не работает или вы что-то не знаете - читай документацию или гуглите!!!**

</aside>

---

---

## Введение

Что будет, если совместить виртуальную и дополненную реальность? У вас есть доступ к настоящему миру, при этом вы можете взаимодействовать с цифровым миром – в этом идея смешанной реальности.

***Смешанная реальность (Mixed Reality, MR)*** – это технология, которая объединяет в себе элементы VR и AR. В MR пользователь может взаимодействовать с реальными и цифровыми объектами одновременно.

### Что вам понадобится

- Unity Editor (в примере используется Unity 6) + Android module;
- [AR Foundation](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@5.1/manual/index.html) (в примере используется 6.0.2) - фреймворк Unity для мультиплатформенной разработки AR. Предоставляет единую систему и API для взаимодействия с нашим базовым AR-движком. Он содержит все игровые объекты и классы, необходимые для быстрой разработки интерактивных AR-интерфейсов в Unity;
- [XR Interaction Toolkit](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@2.5/manual/index.html) (в примере используется 3.0.3) - высокоуровневая система взаимодействия на основе компонентов для создания возможностей XR. Она предоставляет фреймворк, который делает 3D-взаимодействия и пользовательский интерфейс доступными из событий ввода Unity;
- OpenXR Plug-in 1.10.0
- [XR Plugin Management](https://docs.unity3d.com/Manual/com.unity.xr.management.html) представляет собой простой инструмент управления любыми плагинами XR для конкретной платформы, такими как ARKit и ARCore;
- [Unity OpenXR: Meta](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@0.1/manual/index.html)  включает поддержку устройств Meta Quest для ваших проектов AR Foundation и предоставляет интерфейс C# для среды выполнения OpenXR Meta. Этот пакет зависит как от [AR Foundation](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0), так и от [подключаемого модуля OpenXR](https://docs.unity3d.com/Packages/com.unity.xr.openxr@1.9).

Этот пакет зависит как от [AR Foundation](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@5.0), так и от [подключаемого модуля OpenXR](https://docs.unity3d.com/Packages/com.unity.xr.openxr@1.6) и реализует следующие функции AR Foundation.

Прежде чем мы углубимся в разработку, вот обзор основных возможностей, которые вы можете использовать для MR:

- **Passthrough (Переход):** Создайте удобную для восприятия 3D-визуализацию физического мира в реальном времени в гарнитурах Meta Quest ([Unity](https://developer.oculus.com/documentation/unity/unity-passthrough/) | [Unreal](https://developer.oculus.com/documentation/unreal/unreal-passthrough-overview/) | [OpenXR](https://developer.oculus.com/documentation/native/android/mobile-passthrough/) | [WebXR](https://developer.oculus.com/documentation/web/webxr-mixed-reality/#passthrough-mode))
- **Scene (Сцена):** Создавайте игры с учетом окружающей среды, которые тесно взаимодействуют с физическим окружением пользователя ([Unity](https://developer.oculus.com/documentation/unity/unity-scene-overview/) | [Unreal](https://developer.oculus.com/documentation/unreal/unreal-scene-overview/) | [OpenXR](https://developer.oculus.com/documentation/native/android/openxr-scene-overview/) | [WebXR](https://developer.oculus.com/documentation/web/webxr-mixed-reality/#plane-detection))
- **Spatial Anchors (Пространственные якоря):** размещайте и сохраняйте виртуальный контент в физическом пространстве ([Unity](https://developer.oculus.com/documentation/unity/unity-spatial-anchors-overview/) | [Unreal](https://developer.oculus.com/documentation/unreal/unreal-spatial-anchors/) | [OpenXR](https://developer.oculus.com/documentation/native/android/openxr-lsa-overview/) | [WebXR](https://developer.oculus.com/documentation/web/webxr-mixed-reality/#persistent-anchors))
- **Shared Spatial Anchors (Общие пространственные привязки):** Создавайте локальный многопользовательский опыт, создавая общую систему отсчета с привязкой к миру для нескольких пользователей ([Unity](https://developer.oculus.com/documentation/unity/unity-shared-spatial-anchors/) | [Unreal](https://developer.oculus.com/documentation/unreal/unreal-shared-spatial-anchors/) | [OpenXR](https://developer.oculus.com/documentation/native/android/openxr-ssa-share-content/))

<aside>
⛔ Для разработки продуктов под **Quest** на **Unity** есть два подхода:
* ***с помощью абстрактного подхода -*** использования **OpenXR** и **AR Foundation**. Данный способ имеет преимущество в том, что проект может переделываться под другие платформы с меньшими затратами ресурсов, а также этот способ имеет схожесть на разработку VR продуктов из-за использования OpenXR и на разработку с AR продуктов под телефон из-за использования AR Foundation.
* ***с помощью SDK от Meta.*** Данный подход своеобразен и имеет сложности из-за того, что он в основном заключается в использовании сторонних решений, с которыми вы менее знакомы. Также из-за меньшей популярности данной технологии на просторах интернета меньше информации о ней.

**Здесь будет использоваться первый способ!!!**

</aside>

## Настройка устройства

Для создания и запуска вашего AR-приложения на устройстве Meta Quest требуются некоторые шаги по настройке. В разделах ниже объясняются необходимые шаги по подготовке вашего устройства Meta Quest к AR.

### Приступая к работе

Если вы настраиваете устройство Meta Quest в первый раз, обратитесь к статье Meta "[Настройка среды разработки и гарнитуры"](https://developer.oculus.com/documentation/unity/unity-env-device-setup/), чтобы узнать, как перевести гарнитуру в режим разработчика и развернуть приложение через Android Debug Bridge (ADB).

### Обновление программного обеспечения Meta

Для обеспечения наилучшего взаимодействия с вашим устройством Meta Quest рассмотрите возможность обновления программного обеспечения Meta Quest до последней версии. Это обеспечит вам доступ к последним улучшениям Meta и исправлениям ошибок. Вы можете обновить программное обеспечение Meta Quest, перейдя в **Настройки** > **Система** > **Обновление программного обеспечения** в универсальном меню вашего устройства.

### Настройка помещения

Для некоторых функций AR Foundation требуется сначала запустить Space Setup на устройстве Meta Quest, прежде чем запускать приложение. Например, если ваше приложение использует плоскости или ограничивающие рамки, AR Foundation не сможет использовать данные плоскости или ограничивающих рамок, если вы сначала не запустите настройку помещения.

Чтобы запустить настройку помещения, выполните следующие действия:

1. Нажмите на спец. кнопку на правом контроллере, чтобы получить доступ к универсальному меню.
2. Перейдите в **Настройки** > **Физическое пространство** > **Настройка пространства** и нажмите кнопку "**Настроить**".
3. Следуйте инструкциям на гарнитуре, чтобы завершить настройку помещения.

**ПРИМЕЧАНИЕ**

Если в меню **Настройки** > **Физическое пространство** на вашем устройстве недоступна опция **Настройка пространства**, вам необходимо обновить программное обеспечение Meta Quest до более свежей версии, чтобы эта опция появилась.

## Первичная настройка проекта

### **Создание проекта**

Создайте новый проект с шаблоном “**Universal 3D**” и назовите его по своему вкусу. Вам следует использовать Unity Hub, поскольку он значительно упрощает управление проектами.

### **Импорт SDK и инструментов**

Для наших текущих потребностей нам необходимо установить несколько дополнительных инструментов для нашего приложения, это можно сделать очень просто, найдя их в **Package Manager:**

- **AR Foundation**;
- **OpenXR Plugin**;
- **XR Interaction Toolkit;**
- XR Plugin Management;
- OpenXR: Meta.

### **Измените настройки сборки**

Поскольку приложение будет работать на Quest, измените платформу сборки на Android, так как данное устройство работает на основе Android:

1. Откройте **File > Build Profiles;**
2. На панели "**Platforms**" выберите "**Android**";
3. Нажмите **Switch Platform**.

### **Измените настройки проекта**

- Quality Settings:
  - Vsync - disabled
  - Anisotropic Textures - Per Texture.
  - Shadowmask Mode - Shadowmask
  - LOD Bias - 0.7
  - Skin Weights - 2 Bones
- Player Settings:
  - Auto Graphics API - disabled
  - Graphics APIs - **Vulkan**
  - Texture Compression format - ASTC
  - Lightmap encoding - High Quality
  - HDR Cubemap encoding set to High Quality
  - Minimum API Level - Android 10.0 (API Level 29)
  - Scripting Backend - IL2CPP
  - Use Incremental GC - enabled
  - Target Architecture - Arm64
  - Active Input Handling - Input System
  - Optimize Mesh Data - enabled
- Physics Settings:
  - Reuse Collision Callbacks - enabled
  - Improved Patch Friction - enabled
  - Default Max Angular Speed - 7
- Time Settings:
  - Maximum Allowed Timestep set to 0.0138 (for 72 Hz)
- XR Plug-in Management
  - OpenXR - enabled
  - Meta Quest feature group - enabled
- XR Plug-in Management - OpenXR
  - Enabled Interaction Profiles - Oculus Touch Controller Profile
  - OpenXR Feature Groups - Meta Quest
- XR Plug-in Management > Project Validatio
  - исправьте ошибки
- Доп настройки графики

    Meta Quest совместим с Универсальным конвейером рендеринга (URP), но настройки URP по умолчанию не подходят для оптимальной сквозной производительности Quest. Список рекомендуемых настроек Unity приведен в таблице ниже, которые более подробно объясняются в следующих разделах.

    | Настройка | Расположение | Рекомендуемое значение |
    | --- | --- | --- |
    | **Terrain Holes** | Universal Render Pipeline Asset | Отключено |
    | **HDR** | Universal Render Pipeline Asset | Отключено |
    | **Post-processing** | Universal Renderer Data
     | Отключено |
    | **Intermediate Texture** | Universal Renderer Data
     | **Авто** |

     Выбор нужного конвейера рендеринга:

    1. Откройте **Edit > Project Settings…** и нажмите на раздел **Graphics;**
    2. В разделе **Default Render Pipeline** измените настройку с **PC** на **Mobile** (в случае, если у вас только один вариант в списке, то проигнорируйте данный пункт);

        ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled.png)

    Выполните следующие действия, чтобы оптимизировать ваш Universal Render Pipeline Asset для Meta Quest:

    1. Найдите ресурс универсального конвейера рендеринга вашего проекта. Один из способов сделать это - ввести `t:UniversalRenderPipelineAsset` в строку поиска окна **Project;**

        > Если ваш проект не содержит ресурс универсального конвейера рендеринга, обратитесь к [Установке универсального конвейера рендеринга в существующий проект](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@14.0/manual/InstallURPIntoAProject.html) из документации URP.
        >
    2. В **Inspector** под заголовком **Rendering** отключите **Terrain Holes;**
    3. В разделе **Quality** отключите **HDR;**
    4. В разделе **Shadow** измените **Max Distance,** к примеру, на 2.5 - это уменьшит радиус обработки теней.

        ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%201.png)

    Выполните следующие действия, чтобы оптимизировать Universal Renderer Data для Meta Quest:

    1. Найдите ресурс данных универсального средства визуализации вашего проекта. Один из способов сделать это - ввести `t:UniversalRendererData` в строку поиска окна **Project**.
    2. Чтобы использовать [XR Simulation](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/xr-simulation/simulation-overview.html) для тестирования вашего приложения, установите для **Depth Priming Mode** вашего средства рендеринга (расположенного под заголовком **Rendering**) значение `Disabled`.
    3. В **Inspector** под заголовком **Post-processing** снимите флажок **Включено**.
    4. В заголовке **Compatibility** установите для **Intermediate Texture** значение **Auto**.

        *Данные универсального средства визуализации отображаются с рекомендуемыми настройками*

    5. Нажмите на **Add Renderer Feature** и добавьте `ARBackgroundRendererFeature` в список функций рендеринга.

        ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%202.png)

## Настройка сцены

В каждой AR-сцене вашего приложения необходимо иметь два игровых объекта: **AR Session** и **XR Origin**.

Игровой объект AR Session позволяет использовать AR на целевой платформе, а игровой объект XR Origin позволяет отслеживать устройства и преобразует отслеживаемые объекты в систему координат Unity. Если какой-либо из этих игровых объектов отсутствует в сцене, AR не будет функционировать должным образом.

Чтобы создать сеанс AR или XR Origin, щелкните правой кнопкой мыши в окне Иерархии и выберите один из следующих параметров в контекстном меню.

- **XR** > **AR Session
AR Session** управляет жизненным циклом AR-взаимодействия. В основном это запускает AR session и обнаруживает входные данные.
- **XR** > **XR Origin (VR)**

### Фон камеры

Для использования Meta требуется, чтобы для **Background Color** вашей камеры (URP) или для **Clear Flags** (Built-In Render Pipeline) было установлено значение **Solid Color**, а для значения альфа-канала цвета **Background** было установлено нулевое значение.
Cледуйте инструкции, чтобы настроить визуализацию сцены с прозрачным фоном камеры:

1. Найдите GameObject с именем **XR Origin** в вашей иерархии.
2. Разверните иерархию, чтобы показать игровые объекты с **Camera Offset** и с **Main Camera**.
3. Выберите игровой объект **Main Camera**.
4. Выберите один из следующих параметров. Параметры зависят от используемого конвейера рендеринга:
    - URP: В разделе **Environment** установите для **Background Type** значение **Solid Color**.
    - Built-In Render Pipeline: установите для **Clear Flags** значение **Solid Color**.
5. Выберите цвет **Background**, чтобы открыть средство выбора цвета.
6. Установите для цвета значение **A** равным `0`.

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%203.png)

Нажмите на объект **Main Camera** и добавьте компонент **AR Camera Manager**

Теперь ваша сцена настроена на поддержку использования Meta.

## **Проверка проекта**

1. Убедитесь, что ваше устройство подключено и включена отладка ADB;
2. Нажмите **File > Build Profiles**;
3. В разделе **Run Device** выберите ваше устройство (если его нет в списке, то скорее всего проблемы с подключением);
4. Это загрузит приложение на ваше устройство и запустит его после установки;
5. После запуска приложения Вы должны увидеть трансляцию с камеры на экране вашего устройства.

## Функции AR

Функции AR реализуют интерфейсы [AR Foundation](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/index.html). Эти функции задокументированы в руководстве по пакету AR Foundation, поэтому данное руководство содержит информацию только об API, в которых среда выполнения OpenXR Meta демонстрирует уникальное поведение, зависящее от платформы.

Этот пакет реализует следующие функции AR:

| Функция | Описание |
| --- | --- |
| [Session](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/session.html) | Включать, отключать и настраивать AR на целевой платформе. |
| [Camera](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/camera.html) | Визуализируйте изображения с камер устройств и выполняйте оценку освещенности. |
| [Planes](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/planes.html) | Используйте данные модели сцены для отслеживания плоских поверхностей. |
| [Bounding boxes](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/bounding-boxes.html) | Используйте данные модели сцены для отслеживания ограничивающих рамок 3D-объектов. |
| [Anchors](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/anchors.html) | Отслеживайте произвольные точки в пространстве. |
| [Raycasts](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/raycasts.html) | Направляйте лучи на отслеживаемые объекты. |
| [Meshing](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/meshing.html) | Создание сеток среды. |

> При разработке AR-приложения обращайтесь как к документации [AR Foundation](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/index.html), так и к [необходимым пакетам](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/index.html#required-packages) для каждой поддерживаемой вами платформы.
>

## Расширения OpenXR

Расширения OpenXR Meta можно найти в [спецификации OpenXR от Khronos Group](https://registry.khronos.org/OpenXR/specs/1.0/html/xrspec.html).

Этот пакет обеспечивает поддержку следующих расширений OpenXR в вашем проекте:

| **Расширение** | **Использование** | Description |
| --- | --- | --- |
| [XR_FB_scene_capture](https://registry.khronos.org/OpenXR/specs/1.0/html/xrspec.html#XR_FB_scene_capture) | [Session](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/session.html) | Это расширение позволяет приложению запрашивать, чтобы система начала собирать информацию о том, что находится в среде вокруг пользователя. |
| [XR_FB_passthrough](https://registry.khronos.org/OpenXR/specs/1.0/html/xrspec.html#XR_FB_passthrough) | [Camera](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/camera.html) | Сквозная передача - это способ показать пользователю их физическое окружение в виртуальной гарнитуре, блокирующей свет. |
| [XR_FB_spatial_entity](https://registry.khronos.org/OpenXR/specs/1.0/html/xrspec.html#XR_FB_spatial_entity) | [Anchors](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/anchors.html),  [Planes](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/planes.html), [Bounding boxes](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/bounding-boxes.html) | Это расширение позволяет приложениям использовать пространственные объекты для указания привязанных к миру систем отсчета. Оно позволяет приложениям сохранять местоположение контента в реальном мире с течением времени. Все расширения Facebook spatial entity и scene зависят от этого. |
| [XR_FB_spatial_entity_query](https://registry.khronos.org/OpenXR/specs/1.0/html/xrspec.html#XR_FB_spatial_entity_query) | [Planes](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/planes.html),  [Bounding boxes](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/bounding-boxes.html),  [Anchors](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/anchors.html) | Это расширение позволяет приложению обнаруживать постоянные пространственные объекты в области и восстанавливать их. Используя систему запросов, приложение может загружать постоянные пространственные объекты из хранилища. |
| [XR_FB_spatial_entity_storage](https://registry.khronos.org/OpenXR/specs/1.0/html/xrspec.html#XR_FB_spatial_entity_storage) | [Planes](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/planes.html),  [Bounding boxes](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/bounding-boxes.html),  [Anchors](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/anchors.html) | Это расширение позволяет сохранять пространственные объекты и сохранять их в течение сеансов. |
| [XR_FB_scene](https://registry.khronos.org/OpenXR/specs/1.0/html/xrspec.html#XR_FB_scene) | [Planes](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/planes.html), [Bounding boxes](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/bounding-boxes.html) | Это расширение расширяет концепцию пространственных объектов, включая способ представления пространственной сущностью комнат, объектов или других границ в сцене. |
| [XR_FB_display_refresh_rate](https://registry.khronos.org/OpenXR/specs/1.0/html/xrspec.html#XR_FB_display_refresh_rate) | [Display Utilities](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/features/display-utilities.html) | На платформах, поддерживающих динамическую настройку частоты обновления дисплея, разработчики приложений могут запрашивать определенную частоту обновления дисплея, чтобы улучшить общее взаимодействие с пользователем. |

## Разбор особенностей функции AR в Quest в отличие от базового AR Foundation

Теперь, когда базовая сцена настроена, можно приступить к изучению различных фич.

### **Захват сцены (Session)**

В отличие от других AR-платформ, Meta OpenXR не обнаруживает отслеживаемые объекты динамически во время выполнения. Вместо этого среда выполнения OpenXR Meta запрашивает данные настройки пространства устройства и возвращает информацию, хранящуюся в ее [модели сцены](https://developer.oculus.com/documentation/native/android/openxr-scene-overview#scene-model). В API Meta OpenXR этот процесс настройки пространства называется *захватом сцены*.

Во время съемки сцены устройство представляет собой интерфейс, с помощью которого пользователи могут помечать поверхности и объекты в своем окружении, такие как стены и мебель. Когда захват сцены завершен, модель сцены сохраняется на устройстве и сохраняется в разных приложениях и сеансах.

![https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/images/scene-capture.png](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/images/scene-capture.png)

Жизненный цикл захвата сцены состоит из четырех этапов:

1. Вы запрашиваете инициирование захвата сцены.
2. Если запрос прошел успешно, в следующем кадре Unity вызывает [OnApplicationPause(bool)](https://docs.unity3d.com/6000.0/Documentation/ScriptReference/MonoBehaviour.OnApplicationPause.html), передача `true`. Ваше приложение приостановлено во время захвата сцены.
3. Пользователь завершает захват сцены по запросу устройства.
4. Unity возобновляется и вызывает `OnApplicationPause` снова, передавая `false`.

```csharp
var arSession = Object.FindAnyObjectByType<ARSession>();

// Чтобы получить доступ к API захвата сцены, приводим подсистему ARSession к MetaOpenXRSessionSubsystem
var success = (arSession.subsystem as MetaOpenXRSessionSubsystem).TryRequestSceneCapture();
// true, если запрос был успешным. В противном случае false.
```

### **Камера (Camera)**

Дополнительные возможности отсутствуют.

### **Плоскости (Planes)**

**Отслеживаемый идентификатор**

В отличие от других AR-платформ, свойство [trackableId](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/api/UnityEngine.XR.ARFoundation.ARTrackable-2.html#UnityEngine_XR_ARFoundation_ARTrackable_2_trackableId) любого [ARPlane](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/api/UnityEngine.XR.ARFoundation.ARPlane.html) из платформы Meta OpenXR сохраняется в течение нескольких сеансов в одном и том же пространстве настроек. Это позволяет вам, например, сохранять `trackableId` пользовательской таблицы, чтобы сохранять виртуальную центральную часть таблицы каждый раз, когда пользователь запускает ваше приложение.

**Классификации плоскостей**

Обратитесь к таблице ниже, чтобы понять соответствие между классификациями AR Foundation и семантическими метками Meta:

| AR Foundation Label | Meta Label | Перевод |
| --- | --- | --- |
| Table | TABLE | Стол |
| Couch | COUCH | Диван |
| Floor | FLOOR | Пол |
| Ceiling | CEILING | Потолок |
| WallFace | WALL_FACE | Стена |
| WallArt | WALL_ART | Настенное Искусство |
| DoorFrame | DOOR_FRAME | Дверная рама |
| WindowFrame | WINDOW_FRAME | Окно |
| InvisibleWallFace | INVISIBLE_WALL_FACE | Невидимая поверхность стены |
| Other | OTHER | Другое |

### Ограничивающие рамки (Bounding boxes)

**Настройка пространства**
Для создания ограничивающих рамок на устройствах Meta Quest требуется, чтобы пользователь сначала выполнил [настройку пространства](https://docs.unity3d.com/Packages/com.unity.xr.meta-openxr@2.0/manual/device-setup.html#space-setup), прежде, чем можно будет использовать какие-либо данные 3D-ограничивающих рамок.

**Классификация ограничивающих рамок**

| AR Foundation Label | Meta Label | Перевод |
| --- | --- | --- |
| Couch | COUCH | Диван |
| Table | TABLE | Стол |
| Bed | BED | Кровать |
| Lamp | LAMP | Лампа |
| Plant | PLANT | Растение |
| Screen | SCREEN | Экран |
| Storage | STORAGE | Хранение |
| Other | OTHER | Другое |

### **Якоря (Anchors)**

**Поддержка дополнительных функций**

Meta OpenXR реализует следующие дополнительные функции системы XRAnchorSubsystem от AR Foundation:

| Особенность | Свойство дескриптора | Поддерживается |
| --- | --- | --- |
| **Trackable Attachment** | [supportsTrackableAttachments](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/api/UnityEngine.XR.ARSubsystems.XRAnchorSubsystemDescriptor.html#UnityEngine_XR_ARSubsystems_XRAnchorSubsystemDescriptor_supportsTrackableAttachments) |  |
| **Synchronous Add Anchor** | [supportsSynchronousAdd](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/api/UnityEngine.XR.ARSubsystems.XRAnchorSubsystemDescriptor.html#UnityEngine_XR_ARSubsystems_XRAnchorSubsystemDescriptor_supportsSynchronousAdd) |  |
| **Save Anchor** | [supportsSaveAnchor](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/api/UnityEngine.XR.ARSubsystems.XRAnchorSubsystemDescriptor.html#UnityEngine_XR_ARSubsystems_XRAnchorSubsystemDescriptor_supportsSaveAnchor) | Да |
| **Load Anchor** | [supportsLoad](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/api/UnityEngine.XR.ARSubsystems.XRAnchorSubsystemDescriptor.html#UnityEngine_XR_ARSubsystems_XRAnchorSubsystemDescriptor_supportsLoadAnchor) | Да |
| **Erase Anchor** | [supportsEraseAnchor](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/api/UnityEngine.XR.ARSubsystems.XRAnchorSubsystemDescriptor.html#UnityEngine_XR_ARSubsystems_XRAnchorSubsystemDescriptor_supportsEraseAnchor) | Да |
| **Get Saved Anchor Ids** | [supportsGetSavedAnchor](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/api/UnityEngine.XR.ARSubsystems.XRAnchorSubsystemDescriptor.html#UnityEngine_XR_ARSubsystems_XRAnchorSubsystemDescriptor_supportsGetSavedAnchorIds) |  |
| **Async Cancellation** | [supportsAsyncCancellation](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/api/UnityEngine.XR.ARSubsystems.XRAnchorSubsystemDescriptor.html#UnityEngine_XR_ARSubsystems_XRAnchorSubsystemDescriptor_supportsAsyncCancellation) |  |

### Лучи (Raycasts)

Этот пакет определяет реализацию [XRRaycastSubsystem.Provider](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/api/UnityEngine.XR.ARSubsystems.XRRaycastSubsystem.Provider.html) от AR Foundation, но эта реализация не использует никаких функций OpenXR. На самом деле, провайдер лучей в этом пакете совершенно пустой. Включая пустого провайдера лучей, этот пакет обеспечивает резервную реализацию лучей Unity-world-space в ARRaycastManager AR Foundation.

Если ваше приложение использует AR-лучи, вам следует использовать API-интерфейсы [ARRaycastManager](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/api/UnityEngine.XR.ARFoundation.ARRaycastManager.html). Не обращайтесь напрямую к MetaOpenXRRaycastSubsystem.

### Создание сетки (**Meshing**)

Дополнительный возможности отсутствуют.

## Продолжение разработки

Для начала, доустановим дополнительные ассеты:

- Package Manager > XR Interaction Toolkit > Samples > Startes Assets;
- Package Manager > XR Interaction Toolkit > Samples > AR Startes Assets;
- Package Manager > XR Interaction Toolkit > Samples > XR Device Simulator.

### **Настройка XR Origin (XR Rig)**

Теперь пересоздадим наш **XR Origin (XR Rig):** удалите текущую реализацию из **Hierarchy** и найдите в **Project** префаб **XR Origin (XR Rig)** по пути **Samples > XR Interaction Toolkit > 3.0.3 > Starter Assets > Prefabs > XR Origin (XR Rig)**. Данный объект представляет реализацию **XR Origin** под VR с уже готовыми настройками, но мы частично изменим их под наши нужды:

- Удалим ненужный для нас компонент **Character Controller** с объекта XR Origin, так как передвижение будет осуществляться за счёт физического перемещения без использования стиков, **XR Gaze Assistance**.
- Также можете удалить объекты **Left/Right Controller Teleport Stabilized Origin, Teleport Interactor, Locomotion** со всем содержимым и **Gaze Stabilized, Gaze Interactor** если не планируете работать с треком глаз.
- Теперь повторите действия по настройке камеры проделанные ранее в пункте “**Фон камеры**”.
- Настроим наши контролеры:
  - В компоненте **ControllerInputActionManager** отключите все параметры в разделе **Locomotion Settings**, так как ни один параметр мы не будем использовать при работе со смешанной реальностью.

После всех этих действий была подготовлена первичная настройка нашего XR Origin: он способен физически перемещаться по помещению следуя за нашим шлемом, есть визуализация самих контроллеров, объект Poke Interactor отвечает за физическое взаимодействие с объектами, объект Near-Far Interactor позволяет нам взаимодействовать с объектами посредством лучей.

Можно протестировать текущие возможности взаимодействия, если добавить в сцену объекты-образцы из **Samples > XR Interaction Toolkit > 3.0.3 > Starter Assets > DemoSceneAssets > Prefabs >** тут будут различные образцы.

### Cетка поверхности

Если вы попробуете сейчас создать какой-то физический объект, то он просто провалится под пол. Давайте добавим отработку поверхности, для этого на **XR Origin** добавьте компонент **AR Plane Manager**.

[`ARPlaneManager`](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@4.1/api/UnityEngine.XR.ARFoundation.ARPlaneManager.html) обнаруживает [`ARPlane`](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@4.1/api/UnityEngine.XR.ARFoundation.ARPlane.html) игровые объекты, создает их, обновляет и удаляет, когда меняется восприятие окружения устройством.

**ARPlane** - компонент, тип отслеживаемого объекта, который содержит данные, связанные с поверхностью.

Теперь добавим визуализацию для поверхностей, в компоненте **AR Plane Manager** заполним поле **Plane Prefab**, среди ассетов найдите префаб **AR Feathered Plane** и вставьте его. Теперь мы будем видеть отрисовку поверхности в виде белых точек.

### Режим отладки

Давайте создадим переключение для режима откладки, в котором у нас будет отрисовываться текущая сетка поверхности:

1. Сделаем свою отрисовку поверхностей:
    1. Создадим пустой префаб, назовём его **ARPlaneColored**;
    2. Добавьте следующие компоненты для отработки логики **ARPlane** и отрисовки поверхностей:
        - **ARPlane;**
        - **ARPlaneMeshVisualizer;**
        - **MeshCollider;**
        - **MeshFilter;**
        - **MeshRenderer;**
        - **LineRenderer.**
    3. У компонента **LineRenderer** измените:
        - **Width** на **0.01;**
        - **Loop** на **true;**
        - **Materials** на **Default-Line;**
        - **Cast Shadows** на **Off;**
        - **Use World Space** на **false.**
    4. Создайте новый материал с шейдером **Universal Render Pipline/Unlit** (настройте его так же, как показано на рисунке ниже, если использовать обычный материал, то могут возникнуть проблемы с прозрачностью) добавьте его в **MeshRenderer > Materials**;

        ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%204.png)

    5. Напишем скрипт **ARPlaneColorizer** для отработки логики отрисовки поверхностей:

        ```csharp
        using UnityEngine;
        using UnityEngine.XR.ARFoundation;
        using UnityEngine.XR.ARSubsystems;
        
        [RequireComponent(typeof(ARPlane))]
        [RequireComponent(typeof(MeshRenderer))]
        public class ARPlaneColorizer : MonoBehaviour
        {
            // Свойство для управления визуализацией плоскости
            public bool isVisualise
            {
                get => _isVisualise;
                set
                {
                    _isVisualise = value;
                    UpdateColor(); // Обновляем цвет при изменении значения
                }
            }
        
            private const float DEFAULT_COLOR_ALPHA = 0.25f; // Прозрачность цвета по умолчанию
            private bool _isVisualise; // Внутреннее поле для свойства isVisualise
            private Color _defaultColor; // Цвет по умолчанию, определенный на основе классификации
        
            private ARPlane _arPlane; // Компонент ARPlane
            private MeshRenderer _meshRenderer; // Компонент MeshRenderer
            private LineRenderer _lineRenderer; // Компонент LineRenderer
        
            private void Awake()
            {
                // Инициализация компонентов
                _arPlane = GetComponent<ARPlane>();
                _meshRenderer = GetComponent<MeshRenderer>();
                _lineRenderer = GetComponent<LineRenderer>();
        
                if (_arPlane == null || _meshRenderer == null)
                {
                    Debug.LogError("ARPlane или MeshRenderer компонент отсутствует.");
                    return;
                }
        
                // Получаем цвет материала по классификации
                _defaultColor = GetColorByClassification(_arPlane.classifications);
                _defaultColor.a = DEFAULT_COLOR_ALPHA; // Устанавливаем прозрачность цвета
        
                // Устанавливаем начальный цвет
                UpdateColor();
        
                // Подписка на событие изменения границ плоскости
                _arPlane.boundaryChanged += OnPlaneBoundaryChanged;
            }
        
            private void OnDestroy()
            {
                // Отписываемся от события при уничтожении объекта
                if (_arPlane != null)
                {
                    _arPlane.boundaryChanged -= OnPlaneBoundaryChanged;
                }
            }
        
            private void OnPlaneBoundaryChanged(ARPlaneBoundaryChangedEventArgs eventArgs)
            {
                // Обновляем цвет при изменении границ плоскости
                UpdateColor();
            }
        
            // Метод для обновления цвета материала плоскости
            private void UpdateColor()
            {
                _meshRenderer.materials[0].color = _isVisualise ? Color.clear : _defaultColor;
                _lineRenderer.startColor = _isVisualise ? Color.clear : Color.white;
            }
        
            // Метод для получения цвета на основе классификации плоскости
            private static Color GetColorByClassification(PlaneClassifications classifications)
            {
                return classifications switch
                {
                    PlaneClassifications.Floor => Color.green,
                    PlaneClassifications.WallFace => Color.white,
                    PlaneClassifications.Ceiling => Color.red,
                    PlaneClassifications.Table => Color.yellow,
                    PlaneClassifications.Couch => Color.blue,
                    PlaneClassifications.Seat => Color.blue,
                    PlaneClassifications.SeatOfAnyType => Color.blue,
                    PlaneClassifications.WallArt => new Color(1f, 0.4f, 0f), //orange
                    PlaneClassifications.DoorFrame => Color.magenta,
                    PlaneClassifications.WindowFrame => Color.cyan,
                    _ => Color.gray // Цвет по умолчанию
                };
            }
        }
        ```

        - **Разъяснение к коду:**

## Описание

            `ARBoundingBoxColorizer` — это компонент Unity, который управляет визуализацией и цветом границ (bounding boxes) объектов, обнаруженных с помощью дополненной реальности (AR). Он предназначен для автоматического назначения цвета границам на основе их классификации и может переключать их видимость. Компонент использует AR Foundation и ARBoundingBox для работы с AR-объектами.

### Компоненты

            - **ARBoundingBox**: Определяет границы объекта, обнаруженного с помощью AR.
            - **MeshRenderer**: Отвечает за визуализацию границ, предоставляя доступ к материалу и его свойствам, таким как цвет и видимость.

### Поля

            - **defaultColorAlpha**: Прозрачность цвета по умолчанию для границ (можно настроить в Unity Inspector).
            - **_defaultColor**: Цвет, назначаемый границам на основе их классификации.
            - **_boundingBox**: Ссылка на компонент ARBoundingBox, используемый для получения размеров и классификации границ.
            - **_meshRenderer**: Ссылка на компонент MeshRenderer, используемый для управления визуализацией и цветом границ.

### Свойства

            - **isVisualise**: Управляет видимостью границ. Если `true`, границы отображаются; если `false`, они скрыты. Состояние видимости синхронизируется с полем `enabled` компонента `MeshRenderer`.

### Методы

            - **Awake()**: Метод инициализации, который вызывается при создании объекта. Он находит компоненты `ARBoundingBox` и `MeshRenderer`, устанавливает цвет и прозрачность границ на основе их классификации и задает их начальное состояние визуализации.
            - **GetColorByClassification(BoundingBoxClassifications classification)**: Статический метод, который возвращает цвет, соответствующий классификации границ. Если классификация неизвестна, возвращается серый цвет.

### Логика работы

            1. **Инициализация**:
                - В методе `Awake()` компонент находит `ARBoundingBox` и `MeshRenderer` среди дочерних объектов.
                - Если один из компонентов не найден, выводится сообщение об ошибке.
                - Устанавливается цвет границ на основе их классификации и настраивается прозрачность.
                - Размеры границ устанавливаются в соответствии с размерами, заданными в `ARBoundingBox`.
                - Состояние визуализации устанавливается в соответствии со значением свойства `isVisualise`.
            2. **Управление визуализацией**:
                - Свойство `isVisualise` управляет видимостью границ, изменяя поле `enabled` компонента `MeshRenderer`.
            3. **Назначение цвета**:
                - Метод `GetColorByClassification()` возвращает цвет, соответствующий определенной классификации границ. Это позволяет визуально различать разные типы объектов.
    6. Добавьте скрипт **ARPlaneColorizer** на наш префаб **ARPlaneColored**;
    7. Замените объект в компоненте **ARPlaneManager** параметр на **PlanePrefab** на созданный префаб.
2. Сделаем свою отрисовку ограничительной рамки:
    1. Создадим пустой префаб, назовём его **ARBoundingBoxColored**;
    2. Добавьте следующий компонент **ARBoundingBox** для отработки логики **ARBoundingBox**;
    3. Создайте 3D объект-куб как дочерний игровой объект;
    4. Создайте новый материал с шейдером **Universal Render Pipline/Simple Lit** (настройте его так же, как показано на рисунке ниже, если использовать обычный материал, то могут возникнуть проблемы с прозрачностью) замените материал у куба в **MeshRenderer > Materials**;

        ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%205.png)

    5. Напишем скрипт **ARBoundingBoxColored** для отработки логики ограничительной рамки:

        ```csharp
        using UnityEngine;
        using UnityEngine.XR.ARFoundation;
        using UnityEngine.XR.ARSubsystems;
        
        // Компонент требует наличия ARBoundingBox
        [RequireComponent(typeof(ARBoundingBox))]
        public class ARBoundingBoxColorizer : MonoBehaviour
        {
            // Свойство для управления визуализацией границ
            public bool isVisualise
            {
                get => _meshRenderer.enabled; // Получаем текущее состояние видимости
                set => _meshRenderer.enabled = value; // Устанавливаем видимость
            }
        
            [SerializeField] private float defaultColorAlpha = 0.25f; // Прозрачность цвета по умолчанию
        
            private Color _defaultColor; // Цвет по умолчанию, определенный на основе классификации
        
            private ARBoundingBox _boundingBox; // Компонент ARBoundingBox
            private MeshRenderer _meshRenderer; // Компонент MeshRenderer
        
            private void Awake()
            {
                // Инициализация компонентов
                _boundingBox = GetComponent<ARBoundingBox>();
                _meshRenderer = GetComponentInChildren<MeshRenderer>();
        
                if (_boundingBox == null || _meshRenderer == null)
                {
                    Debug.LogError("ARBoundingBox или MeshRenderer компонент отсутствует.");
                    return;
                }
        
                // Устанавливаем цвет меша (границ) на основе классификации
                var defaultColor = GetColorByClassification(_boundingBox.classifications);
                defaultColor.a = defaultColorAlpha; // Устанавливаем прозрачность цвета
                _meshRenderer.material.color = defaultColor;
        
                // Устанавливаем размер меша (границ)
                _meshRenderer.transform.localScale = _boundingBox.size;
        
                // Устанавливаем начальное состояние визуализации
                _meshRenderer.enabled = isVisualise;
            }
        
            // Метод для получения цвета на основе классификации границ
            private static Color GetColorByClassification(BoundingBoxClassifications classification)
            {
                return classification switch
                {
                    BoundingBoxClassifications.Couch => Color.blue,
                    BoundingBoxClassifications.Table => Color.yellow,
                    BoundingBoxClassifications.Bed => Color.cyan,
                    BoundingBoxClassifications.Lamp => Color.magenta,
                    BoundingBoxClassifications.Plant => Color.green,
                    BoundingBoxClassifications.Screen => Color.white,
                    BoundingBoxClassifications.Storage => Color.red,
                    BoundingBoxClassifications.None => Color.gray,
                    BoundingBoxClassifications.Other => Color.gray,
                    _ => Color.gray // Цвет по умолчанию для неизвестной классификации
                };
            }
        }
        
        ```

        - **Разъяснение к коду:**

## Описание

            `ARBoundingBoxColorizer` — это компонент Unity, предназначенный для управления визуализацией и цветом границ (bounding boxes) объектов, обнаруженных с помощью технологий дополненной реальности (AR). Компонент работает в связке с `ARBoundingBox` и автоматически назначает цвет границам на основе их классификации. Это полезно для визуального разделения и отладки AR-объектов.

### Основные функции

            - **Управление видимостью границ**: Позволяет включать или выключать визуализацию границ объектов.
            - **Цветовая индикация**: Назначает цвета границам объектов на основе их классификации, что позволяет легко различать разные типы объектов.

### Компоненты

            - **ARBoundingBox**: Определяет границы объекта в AR, предоставляет информацию о размере и классификации объекта.
            - **MeshRenderer**: Отвечает за визуализацию границ и управление материалами, включая их цвет и прозрачность.

### Поля

            - **defaultColorAlpha**: Прозрачность цвета по умолчанию, устанавливаемая через Unity Inspector. Значение по умолчанию — 0.25.
            - **_defaultColor**: Цвет, определяемый на основе классификации границ.
            - **_boundingBox**: Ссылка на компонент `ARBoundingBox`, который содержит информацию о границах объекта.
            - **_meshRenderer**: Ссылка на компонент `MeshRenderer`, отвечающий за отображение границ объекта.

### Свойства

            - **isVisualise**: Свойство для управления видимостью границ. При установке `true`, границы отображаются; при установке `false`, границы скрыты. Управляет полем `enabled` компонента `MeshRenderer`.

### Методы

            - **Awake()**: Метод инициализации, который вызывается при создании объекта. Он находит компоненты `ARBoundingBox` и `MeshRenderer`, устанавливает цвет и прозрачность границ на основе их классификации, задает размеры и начальное состояние визуализации.
            - **GetColorByClassification(BoundingBoxClassifications classification)**: Статический метод, возвращающий цвет на основе классификации границ. Если классификация неизвестна, возвращается серый цвет.

### Логика работы

            1. **Инициализация**:
                - В методе `Awake()` происходит инициализация компонентов `ARBoundingBox` и `MeshRenderer`.
                - Если один из компонентов отсутствует, выводится сообщение об ошибке в консоль.
                - Цвет границ устанавливается в соответствии с классификацией объекта, а прозрачность настраивается на основе значения `defaultColorAlpha`.
                - Размеры границ синхронизируются с размером, заданным в `ARBoundingBox`.
                - Видимость границ определяется текущим значением свойства `isVisualise`.
            2. **Управление визуализацией**:
                - Свойство `isVisualise` контролирует видимость границ, изменяя поле `enabled` компонента `MeshRenderer`.
            3. **Назначение цвета**:
                - Метод `GetColorByClassification()` назначает цвета различным типам объектов на основе их классификации. Это помогает визуально различать объекты разного типа.
    6. Добавьте скрипт **ARBoundingBoxColorizer** на наш префаб **ARBoundingBoxColored**;
    7. Добавьте компонент **ARBoundingBoxManager** на объект XR Origin и заполните параметр **BoundingBoxPrefab** созданным префабом.
3. Теперь приступим к созданию переключения между режимом отладки и обычного рендеринга:
    1. Напишем скрипт **DebugMode** для отработки логики режима отладки:

        ```csharp
        using UnityEngine;
        using UnityEngine.InputSystem;
        using UnityEngine.XR.ARFoundation;
        
        [RequireComponent(typeof(ARPlaneManager))]
        [RequireComponent(typeof(ARBoundingBoxManager))]
        public class DebugMode : MonoBehaviour
        {
            [SerializeField] private InputActionReference toggleSurfaceRenderingAction; // Действие ввода для переключения визуализации
            [SerializeField] private bool isVisualiseOnStart; // Начальное состояние визуализации
        
            private bool _isVisualise; // Текущее состояние визуализации
            private ARPlaneManager _planeManager; // Компонент для управления AR-плоскостями
            private ARBoundingBoxManager _boundingBoxManager; // Компонент для управления AR-границами
        
            private void Awake()
            {
                // Инициализация компонентов
                _planeManager = GetComponent<ARPlaneManager>();
                _boundingBoxManager = GetComponent<ARBoundingBoxManager>();
                _isVisualise = isVisualiseOnStart; // Устанавливаем начальное состояние визуализации
            }
        
            public void OnEnable()
            {
                // Подписка на действие ввода для переключения визуализации
                toggleSurfaceRenderingAction.action.started += OnToggleSurfaceRendering;
        
                // Подписка на события изменения AR-плоскостей и границ
                _planeManager.trackablesChanged.AddListener(OnPlanesChanged);
                _boundingBoxManager.trackablesChanged.AddListener(OnBoundingBoxesChanged);
        
                // Обновление начальной визуализации
                PlaneUpdateVisualisation();
                BoundingBoxUpdateVisualisation();
            }
        
            public void OnDisable()
            {
                // Отписка от действия ввода
                toggleSurfaceRenderingAction.action.started -= OnToggleSurfaceRendering;
        
                // Отписка от событий изменения AR-плоскостей и границ
                _planeManager.trackablesChanged.RemoveListener(OnPlanesChanged);
                _boundingBoxManager.trackablesChanged.RemoveListener(OnBoundingBoxesChanged);
            }
        
            // Метод-обработчик для изменения состояния AR-плоскостей
            private void OnPlanesChanged(ARTrackablesChangedEventArgs<ARPlane> arg0)
            {
                PlaneUpdateVisualisation();
            }
        
            // Метод-обработчик для изменения состояния AR-границ
            private void OnBoundingBoxesChanged(ARTrackablesChangedEventArgs<ARBoundingBox> arg0)
            {
                BoundingBoxUpdateVisualisation();
            }
        
            // Метод-обработчик для переключения визуализации при срабатывании действия ввода
            private void OnToggleSurfaceRendering(InputAction.CallbackContext obj)
            {
                _isVisualise = !_isVisualise; // Переключение состояния визуализации
        
                PlaneUpdateVisualisation();
                BoundingBoxUpdateVisualisation();
            }
        
            // Обновление визуализации AR-плоскостей
            private void PlaneUpdateVisualisation()
            {
                foreach (var arPlane in _planeManager.trackables)
                {
                    if (arPlane.TryGetComponent(out ARPlaneColorizer arPlaneColorizer))
                    {
                        arPlaneColorizer.isVisualise = _isVisualise; // Устанавливаем состояние визуализации
                    }
                }
            }
        
            // Обновление визуализации AR-границ
            private void BoundingBoxUpdateVisualisation()
            {
                foreach (var arBoundingBox in _boundingBoxManager.trackables)
                {
                    if (arBoundingBox.TryGetComponent(out ARBoundingBoxColorizer arBoundingBoxColorizer))
                    {
                        arBoundingBoxColorizer.isVisualise = _isVisualise; // Устанавливаем состояние визуализации
                    }
                }
            }
        }
        
        ```

        - **Разъяснение к коду:**

## Описание

            Компонент `DebugMode` предназначен для управления визуализацией AR-плоскостей и границ AR-объектов в Unity. Это особенно полезно при разработке и отладке AR-приложений, где необходимо быстро переключать режимы отображения объектов в сцене.

### Основные функции

            - **Переключение визуализации**: Позволяет включать или выключать визуализацию AR-плоскостей и границ AR-объектов с помощью настраиваемого действия ввода.
            - **Обновление состояния визуализации**: Поддерживает актуальное состояние отображения объектов при добавлении или изменении AR-объектов в сцене.

### Компоненты

            - **ARPlaneManager**: Управляет отслеживанием и визуализацией AR-плоскостей.
            - **ARBoundingBoxManager**: Управляет отслеживанием и визуализацией границ AR-объектов.
            - **InputActionReference**: Ссылается на действие ввода, которое используется для переключения состояния визуализации.

### Поля

            - **toggleSurfaceRenderingAction**: Ссылка на действие ввода (например, кнопка или жест), которое переключает визуализацию AR-плоскостей и границ.
            - **isVisualiseOnStart**: Начальное состояние визуализации объектов при запуске сцены (включено или выключено).
            - **_isVisualise**: Текущее состояние визуализации объектов. Изменяется при срабатывании действия ввода.
            - **_planeManager**: Ссылка на компонент `ARPlaneManager`, управляющий AR-плоскостями.
            - **_boundingBoxManager**: Ссылка на компонент `ARBoundingBoxManager`, управляющий границами AR-объектов.

### Методы

            - **Awake()**: Метод инициализации, который вызывается при создании объекта. Он находит компоненты `ARPlaneManager` и `ARBoundingBoxManager`, а также устанавливает начальное состояние визуализации.
            - **OnEnable()**: Метод, вызываемый при включении объекта. Подписывается на действие ввода и события изменения AR-объектов.
            - **OnDisable()**: Метод, вызываемый при выключении объекта. Отписывается от действия ввода и событий изменения AR-объектов.
            - **OnPlanesChanged(ARTrackablesChangedEventArgs<ARPlane> arg0)**: Метод-обработчик, вызываемый при изменении AR-плоскостей (добавление, удаление или изменение). Обновляет визуализацию AR-плоскостей.
            - **OnBoundingBoxesChanged(ARTrackablesChangedEventArgs<ARBoundingBox> arg0)**: Метод-обработчик, вызываемый при изменении границ AR-объектов. Обновляет визуализацию границ.
            - **OnToggleSurfaceRendering(InputAction.CallbackContext obj)**: Метод-обработчик для переключения состояния визуализации при срабатывании действия ввода.
            - **PlaneUpdateVisualisation()**: Метод для обновления состояния визуализации всех AR-плоскостей.
            - **BoundingBoxUpdateVisualisation()**: Метод для обновления состояния визуализации всех границ AR-объектов.

### Логика работы

            1. **Инициализация**:
                - В методе `Awake()` происходит инициализация компонентов `ARPlaneManager` и `ARBoundingBoxManager`.
                - Начальное состояние визуализации (`_isVisualise`) задается на основе значения `isVisualiseOnStart`.
            2. **Подписка на события**:
                - При включении объекта (`OnEnable()`), компонент подписывается на действие ввода `toggleSurfaceRenderingAction` и события изменения AR-объектов (`trackablesChanged`).
                - Это позволяет динамически реагировать на изменения AR-плоскостей и границ, а также переключать визуализацию в ответ на действия пользователя.
            3. **Обработка событий**:
                - Методы `OnPlanesChanged()` и `OnBoundingBoxesChanged()` вызываются при изменении AR-плоскостей и границ соответственно, и вызывают обновление состояния визуализации.
            4. **Переключение визуализации**:
                - Метод `OnToggleSurfaceRendering()` переключает текущее состояние визуализации (`_isVisualise`) между включенным и выключенным состоянием.
                - После переключения состояния обновляется визуализация всех AR-плоскостей и границ с помощью методов `PlaneUpdateVisualisation()` и `BoundingBoxUpdateVisualisation()`.
            5. **Обновление визуализации**:
                - Методы `PlaneUpdateVisualisation()` и `BoundingBoxUpdateVisualisation()` проходят по всем отслеживаемым AR-объектам и устанавливают их визуальное состояние в зависимости от значения `_isVisualise`.
    2. Прикрепим данный скрипт к объекту XR Origin;
    3. По желанию выставите значение в редакторе у параметра **isVisualiseOnStart** в зависимости от того, хотите ли вы видеть режим отладки при запуске;
    4. Теперь, перейдём к реализации переключения режима отладки на кнопку:
        1. Найдите **XRI Default Input Actions (Input Action Asset)** по пути Samples > XR Interaction Toolkit > 3.0.3 > Starter Assets > XRI Default Input Actions. Нажмите 2 раза по нему и откройте редактор системы ввода;
        2. Нам надо создать событие ввода - для начала, создайте отдельную карту ввода, нажмите на **+** возле **Action Maps** и назовите **Extension**, это делается для того, чтобы никак не взаимодействовать с уже готовыми событиями ввода, а дополнять уже готовый функционал;

            ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%206.png)

        3. Удалите пустое событие ввода “New action” нажав на него и кнопку Delete;
        4. Создайте новое событие ввода нажав на **+** справа от **Action** и назовите его **Switching Debug Mode;**

            ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%207.png)

        5. Раскройте действие ввода и увидите отсутствие привязки, нажмите на него и справа в пути выберите кнопку, в качестве примера используем кнопку Y на левом контроллере, теперь путь будет выглядеть следующим образом **XRController > XRController (LeftHand) > Optional Controls > secondaryButton**;

            ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%208.png)

        6. Нажмите сверху на Save Asset для сохранения изменений и можете закрыть редактор системы ввода;
        7. Вернитесь к нашему компоненту **DebugMode** и задайте значение для **toggleSurfaceRenderingAction -** найдите в поиске созданное событие ввода и прикрепите его;

            ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%209.png)

4. При разработке могут возникать различные ошибки, часть которых можно отследить в редакторе в консоли, но бывают ситуации, когда они возникают только во время тестирования конечной сборки на устройстве и там нет консоли для отладки. Давайте добавим её и так же сделаем включение и выключения отображения:
    1. Сначала импортируем готовый ресурс с консолью для XR продуктов - [https://github.com/platinio/UnityVRDebug](https://github.com/platinio/UnityVRDebug);
    2. Если у вас нет в проекте **TextMesh Pro**, то импортируйте его, для этого перейдите в **Project Settings > TextMesh Pro** и нажмите на **Import TMP Essentials**;
    3. Теперь добавим нашу консоль в сцену. Для удобства её можно прикрепить к руке, сделав дочерним объектом одного из контроллеров. И для удобства можно изменить размеры консоли под себя, пример настроек с которых можно начать и изменять под себя представлен на изображении ниже (управление консолью происходит с помощью джойстиков на контроллерах, можно переключаться между выводами ошибок и скролить ленту с ошибками);

        ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%2010.png)

    4. Добавим в скрипт **DebugMode** отключение и включения консоли вместе с режимом отладки:
        1. Добавьте ссылку на **Canvas** консоли: `[SerializeField] private Canvas debugPanel;`;
        2. И в методе `OnToggleSurfaceRendering` добавьте переключение состояния **Canvas** `debugPanel.enabled = _isVisualise;`.
5. Была завершена настройка режима отладки с отрисовкой поверхности и объектов согласно их классификации, добавлена консоль для отладки ошибок и возможность переключения между режимом отладки и обычным.

### Пример пользовательского интерфейса

Создадим простой пользовательский интерфейс с базовым функционалом. При создании пользовательского интерфейса под MR или VR различия минимальны, поэтому каждый из вас уже знает как его делать, но освежим память.

1. Создадим пустой **Canvas**, для этого перейдите в **XR > UI Canvas.** Почему не **UI > Canvas**? При создании второго варианта не будет компонента **TrackedDeviceGraphicRaycaster** который нужен для взаимодействия с XR;
2. Добавьте три кнопки для отработки логики:
    1. Сброс настроек помещения;
    2. Создания примитивного объекта “Сфера”;
    3. Выходы из игры.
3. Настройте размеры **Canvas** для удобства использования;
4. Теперь перейдём к написанию логики пользовательского интерфейса:
    1. Создадим скрипт QuickMenuUI;
    2. Создадим ссылки на кнопки с ограничением доступа только для текущего класса;

        ```csharp
        [SerializeField] private Button **resetRoomButton**;
        [SerializeField] private Button **spawnSphereButton**;
        [SerializeField] private Button **exitButton**;
        ```

    3. Подпишем наши будущие методы для отработки логики каждой из кнопок на события нажатия на кнопку;

        ```csharp
        private void Awake()
        {
          resetRoomButton.onClick.AddListener(ResetRoom);
          spawnSphereButton.onClick.AddListener(SpawnSphere);
           exitButton.onClick.AddListener(Exit);
        }
        ```

    4. Сначала реализуем логику выходя из игры. У кого-то могли возникнуть вопросы, что такое `#if UNITY_EDITOR`, `#else`, `#endif` это “Условная компиляция”. В данном случае было сделано условие, если код запускается в редакторе, то происходит остановка его с помощью его функций, если же код запущен не в редакторе, то мы производим выход из нашей игры;

        ```csharp
        private void Exit()
        {
        #if UNITY_EDITOR
            UnityEditor.EditorApplication.isPlaying = false;
        #else
            UnityEngine.Application.Quit();
        #endif
        }
        ```

        > Условная компиляция — это механизм, который позволяет включать или исключать определенные участки кода на этапе компиляции в зависимости от заданных условий.

        Доп. информация:
        >
        > - <https://unityhub.ru/manual/PlatformDependentCompilation>
        > - <https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/preprocessor-directives>
        > - <https://docs.unity3d.com/Manual/PlatformDependentCompilation.html>
        - **Подробное разъяснение**

            Условная компиляция — это механизм, который позволяет включать или исключать определенные участки кода на этапе компиляции в зависимости от заданных условий. В языке C# условная компиляция часто используется в среде разработки Unity для того, чтобы одни и те же исходные файлы могли компилироваться с различными вариантами поведения в зависимости от целевой платформы или контекста (например, редактор Unity или запущенное приложение).

### Пример с Unity

            В приведенном коде используется условная компиляция с помощью директив `#if` и `#else`:

            ```csharp
            csharpКопировать код
            private void Exit()
            {
            #if UNITY_EDITOR
                UnityEditor.EditorApplication.isPlaying = false;
            #else
                UnityEngine.Application.Quit();
            #endif
            }
            ```

### Разбор

            - **`#if UNITY_EDITOR`**: Если код компилируется в среде Unity Editor, включается блок кода ниже этой директивы. В данном случае, если игра запущена в редакторе, выполнение остановится и редактор выйдет из режима проигрывания (`isPlaying = false`).
            - **`#else`**: Если условие `#if UNITY_EDITOR` не выполняется (то есть компиляция идет не для редактора, а для других платформ), выполняется альтернативный блок кода. В данном случае вызывается метод `Application.Quit()`, который завершает приложение.
            - **`#endif`**: Эта директива завершает условный блок компиляции, указывая компилятору конец условного кода.

### Зачем это нужно?

            - **Платформенная независимость**: Позволяет создавать кроссплатформенные приложения, управляя различными аспектами кода для разных платформ (например, Windows, iOS, Android).
            - **Оптимизация**: Исключает ненужный код для конкретной конфигурации, уменьшая размер и потенциальные ошибки.
            - **Удобство разработки**: Упрощает тестирование и отладку, так как можно писать и тестировать код, который выполняется только в среде разработки, не влияя на конечную сборку.

### Примеры условий

            - `UNITY_EDITOR`: Код выполняется в редакторе Unity;
            - `UNITY_STANDALONE_WIN`: Код выполняется на платформе Windows;
            - `UNITY_IOS`: Код выполняется на платформе iOS;
            - Можно использовать и пользовательские символы, добавляемые в настройках проекта.

            Таким образом, условная компиляция — это мощный инструмент для управления кодом в зависимости от контекста компиляции.

    5. Создадим префаб сферы, которую будем создавать по нажатию на кнопку:
        1. Создайте пустой объект;
        2. Добавьте компонент - Rigidbody;
        3. Как дочерний объект создайте примитивный объект - **Grab Interactable Object - Sphere**, для удобства измените масштаб до 0.2;
        4. Вернитесь к родительскому объекту и добавьте компонент - XR Grab Interactable, по желанию можете изменить несколько параметров:
            1. **Selecte Mode (Sinle/Multiple)** - режим захвата, объект может захватываться одним или несколькими **Interactor** (к примеру руками);
            2. **MovementType -** Варианты обработки и выполнения перемещения интерактивного объекта

                | Instantaneous | Перемещайте взаимодействующий объект, задавая положение и поворот преобразования для каждого кадра. Используйте это, если вы хотите, чтобы визуальное представление обновлялось каждый кадр, минимизируя задержку, однако с тем компромиссом, что оно сможет перемещаться через другие коллайдеры без жесткого тела, поскольку оно следует за взаимодействующим элементом. |
                | --- | --- |
                | Kinematic | Перемещайте взаимодействующий объект, перемещая кинематическое жесткое тело в целевое положение и ориентацию. Используйте это, если вы хотите синхронизировать визуальное представление в соответствии с его физическим состоянием, и если вы хотите, чтобы объект мог перемещаться через другие коллайдеры без жесткого тела, поскольку он следует за взаимодействующим элементом. |
                | VelocityTracking | Переместите взаимодействующий объект, установив скорость и угловую скорость твердого тела. Используйте это, если вы не хотите, чтобы объект мог перемещаться через другие коллайдеры без жесткого тела, поскольку он следует за взаимодействующим, однако с тем недостатком, что может показаться, что он отстает и движется не так плавно, как [мгновенно](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@3.0/api/UnityEngine.XR.Interaction.Toolkit.Interactables.XRBaseInteractable.MovementType.html#UnityEngine_XR_Interaction_Toolkit_Interactables_XRBaseInteractable_MovementType_Instantaneous). |
            3. **Throw On Deatch -** Наследует ли этот объект скорость взаимодействия при освобождении, если да, то после этого параметра будут доп. настройки;
            4. **Far Attach Mode -** Управляет тем, как **Interactor** должен настраивать свое преобразование присоединения при удаленном захвате объекта.

                | DeferToInteractor | Позвольте взаимодействующему элементу выбрать режим удаленного подключения. Это поведение по умолчанию. |
                | --- | --- |
                | Far | **Interactor** всегда должен перемещать преобразование присоединения в дальнюю точку попадания при дальнем выборе. Обычно это приводит к тому, что интерактивный объект остается на расстоянии в дальней точке попадания. |
                | Near | **Interactor** всегда должен сбрасывать преобразование присоединения к ближней точке дальнего выбора. Обычно это приводит к перемещению интерактивного объекта к руке. |
            5. **Use Dynamic Attach -** Положение захвата будет зависеть от положения интерактивного элемента при выборе. Unity создаст динамическую точку привязки для каждого интерактивного элемента, который выбирает этот компонент, если да, то после этого параметра будут доп. настройки.
        5. После настройки объекта в окне **Project** создайте папку **Resources** и внутри неё **Prefabs**, после чего перетащите туда созданный объект для того, чтобы создать префаб, и удалите его со сцены.
    6. Реализуем метод для создания сферы, сначала обращаемся к Resources и получаем наш префаб по пути. После чего создаём экземпляр объекта перед меню;

        ```csharp
        private void SpawnSphere()
        {
            var sphere = Resources.Load<GameObject>("Prefabs/Grab Interactable Object - Sphere");    
            Instantiate(sphere, transform.position - transform.forward * 0.25f, Quaternion.identity);
        }
        ```

    7. Реализуем метод для вызова настройки пространства, сначала мы находим компонент **ARSession**, после чего пытаемся привести подсистему к подсистеме Meta Quest, если это получается, то вызываем метод для вызова настройки пространства;

        ```csharp
        private void ResetRoom()
        {
            var arSession = FindAnyObjectByType<UnityEngine.XR.ARFoundation.ARSession>();
            var success = (arSession.subsystem as UnityEngine.XR.OpenXR.Features.Meta.MetaOpenXRSessionSubsystem)?.TryRequestSceneCapture() ?? false;
            Debug.Log($"Запрос на захват сцены Meta OpenXR завершен с результатом: {success}");
        }
        ```

5. После того как логика реализована, вернёмся к редактору и добавим наш компонент на экран, и привяжем кнопки;
6. Из-за того, что мы используем XR и будем передвигаться по пространству, пользовательский интерфейс можно привязать к нам как дочерний объект, а можно добавить ему физических свойств и брать с собой для удобства:
    1. Создайте новый пустой объект и сделайте меню дочерним объектом;
    2. Добавьте на родительский объект два компонента:
        1. **Rigidbody**, сделайте его **Kinematic**;
        2. **XRGrabInteracteble**, сделайте **UseDynamicAttach - true.**
    3. Создайте пустой родительский объект и внутри него создайте 4 примитивных объекта Cubе;
    4. Подгоните размер кубов, как будто это рамки у картины;
    5. Теперь UI можно двигать, хватая объект за рамки;
    6. Можете для красоты сделать рамки полупрозрачными, создав и настроив новый материал.

Был реализован простой пользовательский интерфейс, с базовым функционалом выхода из игры, создания примитивного объекта, с которым можно взаимодействовать, кнопкой для настройки пространства и возможностью передвижения пользовательского интерфейса.

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%2011.png)

### Создание объектов на стене с сохранением позиций

Сначала реализуем создание разноцветных кубов на стенах при нажатии на триггер при наведении рейкастом:

1. Найдите в **Assets** префаб **Ray Interactor** и добавьте его как дочерний объект в правый контроллер;
2. Настройте **Ray Interactor:**
    1. **XR Ray Interactor > Handedness** - **Right** (в зависимости от того в какой контроллер добавили);
    2. **XR Ray Interactor > UI Interaction** и **XR Ray Interactor > Manipulate Attach Transform** - отключите, луч нам нужен будет только для считывания позиции попадания;
    3. **XR Interactor Line Visual > Override Line Legth > Auto Adjust Line Legth -** отключите, это автоматическое уменьшение и увеличение визуальной длинны луча, в данной ситуации может возникать путаница;
    4. **Simple Haptic Feedback** - удалите компонент, он не понадобится.
3. Создадим новый скрипт **SpawnerMulticoloredCubes:**
    1. Создадим ссылки на два действия ввода, на триггер, по которому будет происходить спавн объекта, и на переключатель, он понадобится для того, чтобы **XRRayInteractor** и **NearFarInteractor** не работали одновременно. Также ссылки на **XRRayInteractor** и **NearFarInteractor** и **PlaneClassifications,** где мы выберем, на каких плоскостях будет происходить спавн. ****

        ```csharp
        **[SerializeField] private InputActionReference spawnAction;
        [SerializeField] private InputActionReference switchAction;
        
        [SerializeField] private XRRayInteractor xrRayInteractor;
        [SerializeField] private NearFarInteractor nearFarInteractor;
        
        [SerializeField] private PlaneClassifications targetPlaneClassification;
        [SerializeField] private GameObject objectPrefab;**
        ```

    2. Реализуем два метода, которые будут срабатывать при включении и отключении нашего компонента, при включении мы будем регистрировать события ввода, выключать **XRRayInteractor** и включать **NearFarInteractor**, а при отключении компонента отменять регистрацию на события ввода.

        ```csharp
        private void OnEnable()
        {
            spawnAction.action.Enable();
            spawnAction.action.performed += OnSpawn;
        
            switchAction.action.Enable();
            switchAction.action.performed += OnSwitch;
        
            nearFarInteractor.enabled = true;
            xrRayInteractor.enabled = false;
        }
        
        private void OnDisable()
        {
            spawnAction.action.Disable();
            spawnAction.action.performed -= OnSpawn;
        
            switchAction.action.Disable();
            switchAction.action.performed -= OnSwitch;
        }
        ```

    3. Реализуем переключение, в примере планируется привязка к стику, поэтому, когда отклонение вперёд больше, чем на половину, то происходит переключение, включается **XRRayInteractor** и выключается **NearFarInteractor**, когда стик переходит в любое другое положение, то всё возвращается на свои места.

        ```csharp
        private void OnSwitch(InputAction.CallbackContext context)
        {
            var value = context.ReadValue<Vector2>();
        
            if (value.y > 0.5f)
            {
                xrRayInteractor.enabled = true;
                nearFarInteractor.enabled = false;
            }
            else
            {
                xrRayInteractor.enabled = false;
                nearFarInteractor.enabled = true;
            }
        }
        ```

    4. Реализуем спавн разноцветных кубов. Производится проверка, включён ли **XRRayInteractor**, если да, то пытаемся получить попадание по 3Д объекту, если оно есть, то проверяем, есть ли на нём компонент **ARPlane**, если он находится, то проверяем соответствует ли он классификации которая нужна для спавна объекта. Если все условия удовлетворены, то создаётся куб, задаётся определённое имя, случайный цвет, размер. Если мы попали не по поверхности, то проверяем, не попали ли по кубу, если по нему, то уничтожаем его.

        ```csharp
        private void OnSpawn(InputAction.CallbackContext context)
        {
            if (xrRayInteractor.enabled && xrRayInteractor.TryGetCurrent3DRaycastHit(out var raycastHit, out _))
            {
                if (raycastHit.transform.TryGetComponent(out ARPlane arPlane)
                    && (arPlane.classifications & targetPlaneClassification) != 0)
                {
                    var instantiate = Instantiate(objectPrefab, raycastHit.point, Quaternion.LookRotation(raycastHit.normal));
                    cube.name = "SpawnedObject";
        
                    cube.GetComponent<Renderer>().material.color = Random.ColorHSV();
        
                    return;
                }
        
                if (raycastHit.transform.name == "SpawnedObject")
                {
                    Destroy(raycastHit.transform.gameObject);
                    
                    return;
                }
            }
        }
        ```

    5. После этого добавьте написанный скрипт на XR Origin;
4. Перейдите к редактированию системы ввода и добавим два новых события ввода, пример на изображении ниже;

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%2012.png)

5. Создайте префаб кубика и уменьшите его масштаб до 0.1;
6. Привяжите все ссылки к компоненты.

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Quest%202%203%205923ba64ec4f4cde9f0d1d6fc97e7960/Untitled%2013.png)

Протестируйте получившийся результат. Попробуйте удерживать кнопку **Quest** на правом контроллере для того, чтобы центрировать пространство. Все созданные кубы почему-то изменили свою позицию. Это произошло, потому что они не имеют привязки к текущему пространству, а привязаны к центру игрока и после его сброса они меняют свою позицию. Теперь добавим привязки объектов к пространству:

1. Добавьте **ARAnchorManager** на XR Origin. Это менеджер привязок создает игровые объекты для каждой привязки. Привязка - это определенная точка в пространстве, которую вы хотите отслеживать устройством;
2. Модифицируем метод для спавна
    - **Асинхронное добавление якорей**:
        - Вместо немедленного создания объекта, теперь создается якорь, который фиксирует позицию и ориентацию на поверхности, и это происходит асинхронно.
        - Если добавление якоря не удалось, объект не создается.
    - **Привязка к якорю**:
        - Новый объект привязывается к созданному якорю, что позволяет ему оставаться на месте даже если поверхность, на которую он был установлен, перемещается или изменяется.

    ```csharp
    private async void OnSpawn(InputAction.CallbackContext context)
    {
        if (xrRayInteractor.enabled && xrRayInteractor.TryGetCurrent3DRaycastHit(out var raycastHit, out _))
        {
            if (raycastHit.transform.TryGetComponent(out ARPlane arPlane)
                && (arPlane.classifications & targetPlaneClassification) != 0)
            {
                var hitPose = new Pose(raycastHit.point, Quaternion.LookRotation(raycastHit.normal));
                var result = await anchorManager.TryAddAnchorAsync(hitPose);
    
                if (result.status.IsSuccess())
                {
                    var arAnchor = result.value;
    
                    var instantiate = Instantiate(objectPrefab, arAnchor.pose.position, arAnchor.pose.rotation);
                    instantiate.transform.SetParent(arAnchor.transform);
                    instantiate.name = "SpawnedObject";
    
                    instantiate.GetComponent<Renderer>().material.color = Random.ColorHSV();
                }
    
                return;
            }
    
            if (raycastHit.transform.name == "SpawnedObject")
            {
                Destroy(raycastHit.transform.gameObject);
                    
                return;
            }
        }
    }
    ```

Протестируйте получившийся результат. Попробуйте удерживать кнопку **Quest** на правом контроллере для того, чтобы центрировать пространство. Теперь все созданные кубы остаются на своей позиции. Но если перезайти в игру, то всё сбрасывается. Добавим сохранение их позиций и загрузку при перезапуске проекта:

1. Установите вспомогательную библиотеку для работы с Json. **Package Manager > + > Install package by name** и установите пакет `com.unity.nuget.newtonsoft-json`;
2. Реализуем скрипт **SaveAndLoadAnchorIdsToFile** для сохранения и загрузки идентификаторов якорей (Anchor IDs) в файле JSON на устройстве. Он предназначен для работы в контексте приложения дополненной реальности (AR), где якоря используются для привязки виртуальных объектов к реальному миру. Основная задача этого скрипта — управлять сохранением и удалением идентификаторов якорей (SerializableGuid) в файловой системе устройства:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    using UnityEngine;
    using UnityEngine.XR.ARSubsystems;
    
    public class SaveAndLoadAnchorIdsToFile
    {
        private readonly string _filePath = Path.Combine(Application.persistentDataPath, "SavedAnchorIds.json");
    
        private bool _initialized;
        private readonly Awaitable _initializeTask;
        private readonly HashSet<SerializableGuid> _savedAnchorIds = new();
    
        public SaveAndLoadAnchorIdsToFile()
        {
            _initializeTask = PopulateSavedAnchorIdsFromFile();
        }
    
        /// <summary>
        /// Асинхронно сохраняет `SerializableGuid` в файл, добавляя его к списку уже сохраненных идентификаторов.
        /// Если файла не существует или файл не читается, создается новый файл.
        /// </summary>
        /// <param name="savedAnchorId">`SerializableGuid` для сохранения.</param>
        public async Awaitable SaveAnchorIdAsync(SerializableGuid savedAnchorId)
        {
            try
            {
                if (!_initialized)
                {
                    await _initializeTask;
                }
    
                _savedAnchorIds.Add(savedAnchorId);
                await WriteSavedAnchorIdsToFile();
            }
            catch (Exception e)
            {
                Debug.LogException(e);
            }
        }
    
        /// <summary>
        /// Асинхронно удаляет `SerializableGuid` из файла.
        /// </summary>
        /// <param name="savedAnchorId">`SerializableGuid` для удаления.</param>
        public async Awaitable RemoveAnchorIdAsync(SerializableGuid savedAnchorId)
        {
            try
            {
                if (!_initialized)
                {
                    await _initializeTask;
                }
    
                _savedAnchorIds.Remove(savedAnchorId);
                await WriteSavedAnchorIdsToFile();
            }
            catch (Exception e)
            {
                Debug.LogException(e);
            }
        }
    
        /// <summary>
        /// Возвращает набор SerializableGuid из файла сохранения.
        /// </summary>
        /// <returns>Набор SerializableGuid, сохраненных в файле.
        /// Если файла не существует или файл не читается, возвращается пустой набор.</returns>
        public async Awaitable<HashSet<SerializableGuid>> LoadSavedAnchorIdsAsync()
        {
            if (!_initialized)
            {
                await _initializeTask;
            }
    
            return _savedAnchorIds;
        }
    
        private async Awaitable PopulateSavedAnchorIdsFromFile()
        {
            try
            {
                _savedAnchorIds.Clear();
                if (!File.Exists(_filePath))
                {
                    return;
                }
    
                using var streamReader = File.OpenText(_filePath);
                using var jsonTextReader = new JsonTextReader(streamReader);
    
                var jArray = (JArray)await JToken.ReadFromAsync(jsonTextReader);
                foreach (var jObject in jArray)
                {
                    var idAsJToken = jObject["guid"];
                    if (idAsJToken == null)
                    {
                        continue;
                    }
    
                    var idAsString = idAsJToken.Value<string>();
                    var guid = new Guid(idAsString);
                    var serializableGuid = new SerializableGuid(guid);
                    _savedAnchorIds.Add(serializableGuid);
                }
            }
            catch (Exception e)
            {
                Debug.LogException(e);
            }
            finally
            {
                _initialized = true;
            }
        }
    
        private async Awaitable WriteSavedAnchorIdsToFile()
        {
            var jsonString = JsonConvert.SerializeObject(_savedAnchorIds, Formatting.Indented);
            await File.WriteAllTextAsync(_filePath, jsonString);
        }
    }
    ```

    - **Подробное описание:**

### Поля

        1. **_filePath**:
            - Хранит полный путь к файлу `SavedAnchorIds.json`, который находится в постоянной директории данных приложения (`Application.persistentDataPath`).
        2. **_initialized**:
            - Булевое значение, указывающее, инициализирован ли класс и были ли загружены идентификаторы из файла.
        3. **_initializeTask**:
            - Объект типа `Awaitable`, представляющий задачу инициализации, которая асинхронно загружает сохраненные идентификаторы из файла.
        4. **_savedAnchorIds**:
            - Хранит множество уникальных идентификаторов (типа `SerializableGuid`), которые были сохранены в JSON-файле.

### Конструктор

        - **SaveAndLoadAnchorIdsToFile()**:
            - Инициализирует задачу `_initializeTask` для асинхронной загрузки идентификаторов якорей из файла, когда объект этого класса создается.

### Методы

        1. **SaveAnchorIdAsync(SerializableGuid savedAnchorId)**:
            - Асинхронно добавляет новый идентификатор якоря (`SerializableGuid`) в коллекцию `_savedAnchorIds` и записывает обновленный список идентификаторов в файл.
            - Перед добавлением нового идентификатора метод ждет завершения инициализации (`await _initializeTask`), если она ещё не завершена.
            - Обработка исключений ведется с помощью `try-catch`, чтобы избежать ошибок при сохранении данных.
        2. **RemoveAnchorIdAsync(SerializableGuid savedAnchorId)**:
            - Асинхронно удаляет указанный идентификатор якоря из коллекции `_savedAnchorIds` и обновляет файл.
            - Также ждет завершения инициализации перед удалением идентификатора.
            - Исключения обрабатываются аналогично методу `SaveAnchorIdAsync`.
        3. **LoadSavedAnchorIdsAsync()**:
            - Возвращает набор идентификаторов якорей, сохраненных в файле.
            - Перед возвратом данных метод ждет завершения инициализации, если она ещё не завершена.
            - Возвращает `HashSet<SerializableGuid>`, представляющий все сохраненные идентификаторы якорей.
        4. **PopulateSavedAnchorIdsFromFile()**:
            - Частный метод, который асинхронно загружает идентификаторы якорей из файла `SavedAnchorIds.json`.
            - Если файл не существует или его невозможно прочитать, метод просто завершает выполнение, не добавляя ничего в коллекцию `_savedAnchorIds`.
            - Если файл существует, метод читает его содержимое, десериализует JSON и добавляет идентификаторы в `_savedAnchorIds`.
            - В конце метод устанавливает `_initialized` в `true`, указывая на завершение инициализации.
        5. **WriteSavedAnchorIdsToFile()**:
            - Частный метод, который асинхронно записывает текущие идентификаторы якорей в файл в формате JSON.
            - Он использует `JsonConvert.SerializeObject` для преобразования множества `_savedAnchorIds` в строку JSON и записывает эту строку в файл.

### Использование

        - **Добавление идентификатора якоря**:
            - Вызывается метод `SaveAnchorIdAsync(SerializableGuid)`, чтобы сохранить новый идентификатор в файл.
        - **Удаление идентификатора якоря**:
            - Метод `RemoveAnchorIdAsync(SerializableGuid)` удаляет существующий идентификатор из файла.
        - **Загрузка всех идентификаторов**:
            - Метод `LoadSavedAnchorIdsAsync()` возвращает все сохраненные идентификаторы.
3. Модифицируем класс **SpawnerMulticoloredObject:**
    1. **Добавление и использование класса `SaveAndLoadAnchorIdsToFile`**:

        Новый объект `SaveAndLoadAnchorIdsToFile` используется для управления сохранением и загрузкой идентификаторов якорей в файл. Ранее в скрипте не было никакой логики для сохранения и загрузки состояний якорей.

    2. **Метод `Start()`**:

        В методе `Start()` создается экземпляр `SaveAndLoadAnchorIdsToFile` и вызывается метод `LoadAnchors()`, который загружает ранее сохраненные якоря и объекты на сцену.

        Это добавляет новый шаг к инициализации класса, обеспечивая восстановление предыдущего состояния AR сцены.

    3. **Метод `OnSpawn(InputAction.CallbackContext context)`**:

        Логика создания и удаления объектов изменена, чтобы включить сохранение и удаление идентификаторов якорей:

        1. **Создание якоря**:

            Если `XRRayInteractor` успешно обнаруживает `ARPlane`, создается якорь (`ARAnchor`), к которому привязывается новый объект. После этого идентификатор якоря сохраняется с помощью `SaveAndLoadAnchorIdsToFile`.

        2. **Удаление якоря**:

            Если попадает по уже существующему объекту, якорь удаляется, а его идентификатор удаляется из файла с использованием `SaveAndLoadAnchorIdsToFile`.

    4. **Метод `LoadAnchors()`**:

        Новый метод, который загружает сохраненные якоря при старте приложения. Он проверяет сохраненные идентификаторы якорей и восстанавливает объекты на их позиции.

        Этот метод используется для восстановления состояния сцены AR, создавая объекты на основе данных, сохраненных в предыдущих сессиях.

    ```csharp
    using UnityEngine;
    using UnityEngine.InputSystem;
    using UnityEngine.XR.ARFoundation;
    using UnityEngine.XR.ARSubsystems;
    using UnityEngine.XR.Interaction.Toolkit.Interactors;
    
    public class SpawnerMulticoloredObject : MonoBehaviour
    {
        [SerializeField] private InputActionReference spawnAction;
        [SerializeField] private InputActionReference switchAction;
    
        [SerializeField] private XRRayInteractor xrRayInteractor;
        [SerializeField] private NearFarInteractor nearFarInteractor;
        [SerializeField] private ARAnchorManager anchorManager;
    
        [SerializeField] private PlaneClassifications targetPlaneClassification;
        [SerializeField] private GameObject objectPrefab;
    
        private SaveAndLoadAnchorIdsToFile _saveAndLoadAnchorIdsToPlayerPrefs;
        private bool _isLoaded;
    
        private void OnEnable()
        {
            spawnAction.action.Enable();
            spawnAction.action.performed += OnSpawn;
    
            switchAction.action.Enable();
            switchAction.action.performed += OnSwitch;
    
            nearFarInteractor.enabled = true;
            xrRayInteractor.enabled = false;
        }
    
        private void OnDisable()
        {
            spawnAction.action.Disable();
            spawnAction.action.performed -= OnSpawn;
    
            switchAction.action.Disable();
            switchAction.action.performed -= OnSwitch;
        }
    
        private void Start()
        {
            _saveAndLoadAnchorIdsToPlayerPrefs = new SaveAndLoadAnchorIdsToFile();
    
            LoadAnchors();
        }
    
        private async void OnSpawn(InputAction.CallbackContext context)
        {
            if (xrRayInteractor.enabled && xrRayInteractor.TryGetCurrent3DRaycastHit(out var raycastHit, out _))
            {
                if (raycastHit.transform.TryGetComponent(out ARPlane arPlane)
                    && (arPlane.classifications & targetPlaneClassification) != 0)
                {
                    var hitPose = new Pose(raycastHit.point, Quaternion.LookRotation(raycastHit.normal));
                    var addAnchorResult = await anchorManager.TryAddAnchorAsync(hitPose);
    
                    if (!addAnchorResult.status.IsSuccess())
                    {
                        return;
                    }
    
                    var arAnchor = addAnchorResult.value;
    
                    var instantiate = Instantiate(objectPrefab, arAnchor.pose.position, arAnchor.pose.rotation);
                    instantiate.transform.SetParent(arAnchor.transform);
                    instantiate.name = "SpawnedObject";
    
                    instantiate.GetComponent<Renderer>().material.color = Random.ColorHSV();
    
                    await anchorManager.TrySaveAnchorAsync(arAnchor);
                    await _saveAndLoadAnchorIdsToPlayerPrefs.SaveAnchorIdAsync(arAnchor.trackableId);
    
                    return;
                }
    
                if (raycastHit.transform.name == "SpawnedObject")
                {
                    var arAnchor = raycastHit.transform.parent.GetComponent<ARAnchor>();
    
                    await _saveAndLoadAnchorIdsToPlayerPrefs.RemoveAnchorIdAsync(arAnchor.trackableId);
                    anchorManager.TryRemoveAnchor(arAnchor);
    
                    Destroy(raycastHit.transform.gameObject);
    
                    return;
                }
            }
        }
    
        private void OnSwitch(InputAction.CallbackContext context)
        {
            var value = context.ReadValue<Vector2>();
    
            if (value.y > 0.5f)
            {
                xrRayInteractor.enabled = true;
                nearFarInteractor.enabled = false;
            }
            else
            {
                xrRayInteractor.enabled = false;
                nearFarInteractor.enabled = true;
            }
        }
    
        private async void LoadAnchors()
        {
            if (_isLoaded)
            {
                return;
            }
    
            _isLoaded = true;
    
            var anchorIds = await _saveAndLoadAnchorIdsToPlayerPrefs.LoadSavedAnchorIdsAsync();
    
            if (anchorIds == null)
            {
                return;
            }
    
            foreach (var anchorId in anchorIds)
            {
                var loadAnchorResult = await anchorManager.TryLoadAnchorAsync(anchorId);
                if (loadAnchorResult.status.IsSuccess())
                {
                    var arAnchor = loadAnchorResult.value;
    
                    var instantiate = Instantiate(objectPrefab, arAnchor.pose.position, arAnchor.pose.rotation);
                    instantiate.transform.SetParent(arAnchor.transform);
                    instantiate.name = "SpawnedObject";
    
                    instantiate.GetComponent<Renderer>().material.color = Random.ColorHSV();
                }
            }
        }
    }
    ```

Протестируйте получившийся результат. Попробуйте удерживать кнопку **Quest** на правом контроллере для того, чтобы центрировать пространство. Теперь все созданные кубы остаются на своей позиции. Перезайдите в игру и увидите, что кубы остались на своих местах.

### Сетка пространства

**ARMeshManager -** менеджер сетки, создаёт сетку на основе отсканированной местности, если **ARPlaneManager** создаёт физическое пространство на основе примитивных плоских частей частей, то **ARMeshManager** создаёт более проработанную сетку. Для её добавления:

1. Создайте дочерний объект **ARMeshManagerObject** у XR Origin;
2. На неё добавьте компонент **ARMeshManager;**
3. Создайте и прикрепите префаб состоящий из пустого объекта с компонентами:
    1. **Mesh Filter;**
    2. **Mesh Collider;**
    3. **Mesh Render -** не обязателен, он нужен для визуализации сетки, если добавляете его, то не забудьте добавить и материал.

Лучше оставлять что-то одно **ARMeshManager** или **ARPlaneManager и ARBoudingBoxManager.**

## Подведение итогов

**Поздравляем!** Вы добрались до конца этого курса по Unity MR для Quest.

- Unordered lists, and:

  1. One
  1. Two
  1. Three

- More

> Blockquote

And **bold**, *italics*, and even *italics and later **bold***. Even ~~strikethrough~~. [A link](https://markdowntohtml.com) to somewhere.

And code highlighting:

```js
var foo = 'bar';

function baz(s) {
   return foo + ':' + s;
}
```

Or inline code like `var foo = 'bar';`.

Or an image of bears

![bears](http://placebear.com/200/200)

The end ...
