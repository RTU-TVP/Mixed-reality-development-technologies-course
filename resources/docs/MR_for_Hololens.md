# Пример создание MR проекта под Hololens 2

<aside>
💡 **Если что-то не работает или Вы что-то не знаете - читайте документацию или гуглите!**

</aside>

---

---

## Введение

Набор средств для Смешанной реальности — это кроссплатформенный набор средств разработки с открытым исходным кодом, который можно импортировать в проект смешанной реальности Unity. Несмотря на то, что можно создавать приложения Windows Mixed Reality без этого набор средств, мы рекомендуем воспользоваться преимуществами его компонентов и функций, чтобы ускорить разработку. Ниже приведены некоторые его функции.

### Что вам понадобится

- Unity Editor (в примере используется Unity 6) + UWP module;
- [XR Interaction Toolkit](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@2.5/manual/index.html) (в примере используется 3.0.3) - высокоуровневая система взаимодействия на основе компонентов для создания возможностей XR. Она предоставляет фреймворк, который делает 3D-взаимодействия и пользовательский интерфейс доступными из событий ввода Unity;
- OpenXR Plug-in (1.10.0);
- [XR Plugin Management](https://docs.unity3d.com/Manual/com.unity.xr.management.html) представляет собой простой инструмент управления любыми плагинами XR для конкретной платформы, такими как ARKit и ARCore;
- Mixed Reality OpenXR Plugin;
- Mixed Reality Toolkit Foundation.

> **Примечание от Microsoft:**
Кроме того, хотя мы рекомендуем использовать Unity 2022.3 LTS, приложение, использующее Универсальный конвейер рендеринга (URP), имеет худшую производительность рендеринга в Unity 2022 по сравнению с Unity 2021 при использовании материала URP Lit по умолчанию. Мы рекомендуем приложениям URP использовать Unity 2021 или Unity 6. Для получения дополнительной информации ознакомьтесь с [**известными проблемами в определенных версиях Unity**](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/known-issues).
>

## Первичная настройка проекта

### **Создание проекта**

Создайте новый проект с шаблоном “**Universal 3D**” и назовите его по своему вкусу. Вам следует использовать Unity Hub, поскольку он значительно упрощает управление проектами.

### **Измените настройки сборки**

Поскольку приложение будет работать на Android, измените платформу сборки на Android:

1. Откройте **File > Build Profiles;**
2. На панели **Platforms** выберите **Universal Windows Platform**;
3. Нажмите **Switch Platform**.

**Arhitecture:** ARM-64

**Build Type:** D3D Project

**Target SDK Version:** установлена последней

**Minimum Platform Version:** 10.0.10240.0

**Visual Studio Version:** установлена последней

### **Загрузите и установите инструмент для создания функций смешанной реальности**

Плагин OpenXR для смешанной реальности поставляется в виде пакета для Unity. Лучший способ находить, обновлять и импортировать пакеты функций - это инструмент для создания функций смешанной реальности. Вы можете выполнять поиск пакетов по названию или категории, просматривать их зависимости и предлагаемые изменения в файле манифеста вашего проекта перед импортом.

1. Загрузите последнюю версию инструмента для создания функций смешанной реальности [из Центра загрузки Microsoft](https://aka.ms/MRFeatureTool).
2. После завершения загрузки перейдите к исполняемому файлу **MixedRealityFeatureTool.exe,** а затем используйте его для запуска средства создания функций смешанной реальности.

### **Импортируйте инструментарий для смешанной реальности и пакеты OpenXR**

1. В инструменте создания функций смешанной реальности выберите **Начать**.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/mixed-reality-feature-tool.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/mixed-reality-feature-tool.png)

2. Нажмите кнопку "Обзор" (это кнопка с тремя точками на изображении ниже), затем перейдите к своему проекту и откройте его.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/007-project-path.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/007-project-path.png)

3. Выберите **функции для открытия**.

    > **Примечание**: Возможно, вам придется подождать несколько секунд, пока инструмент обновит пакеты из каналов.
    >
4. На странице "**Discover Features"** обратите внимание, что есть список из шести групп пакетов.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/003-mrft-groups.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/003-mrft-groups.png)

5. Нажмите кнопку "+" слева от **Mixed Reality Toolkit (0 из 10)**, а затем выберите последнюю версию **Mixed Reality Toolkit Foundation**.

    > **Примечание**
    Пакет Mixed Reality Toolkit Foundation - это единственный пакет, который необходимо импортировать и настроить, чтобы использовать MRTK в вашем проекте. Этот пакет включает основные компоненты, необходимые для создания приложения смешанной реальности.
    >
6. Нажмите кнопку "+" слева от **Platform Support (0 из 5)**, а затем выберите последнюю версию **Mixed Reality OpenXR Plugin**.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/008-package-selections.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/008-package-selections.png)

7. После того, как вы сделали свой выбор(ы), нажмите **Get Features**.
8. Выберите **Validate** для проверки выбранных вами пакетов. Вы должны увидеть диалоговое окно с сообщением, что **проблем с проверкой обнаружено не было**. Когда вы это сделаете, нажмите **OK**.
9. На странице **Import Features** в левой колонке **Features** отображаются только что выбранные пакеты. В правой колонке **Required dependencies** отображаются любые зависимости. Вы можете щелкнуть ссылку **Details** для любого из этих элементов, чтобы узнать о них больше.
10. Когда вы будете готовы двигаться дальше, выберите **Import.** На странице **Review and Approve** вы можете просмотреть информацию о пакетах.
11. Выберите **Approve.**
12. Вернитесь в редактор Unity и щелкните пустую область в пользовательском интерфейсе. Вы увидите индикатор выполнения, показывающий, что ваши пакеты импортируются.

### **Настройка параметров серверной части ввода**

1. После импорта пакета Unity появляется предупреждение с вопросом, хотите ли вы включить серверные части, перезапустив редактор, выберите **Yes**.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/unity-restart-option.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/unity-restart-option.png)

### **Настройте проект для HoloLens 2**

1. Откройте **Edit > Project Settings…** и нажмите на раздел **XR Plug-in Management;**
2. В окне **XR Plug-in Management**, убедиться, что вы на вкладке **Universal Windows Platform**.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/012-xr-plugin-mgmt-page.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/012-xr-plugin-mgmt-page.png)

3. Убедитесь, что выбран параметр **Initialize XR on Startup**, а затем в разделе **Plug-in Providers**, нажмите **OpenXR**.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/013-init-xr-on-startup.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/013-init-xr-on-startup.png)

4. Плагин OpenXR загружается, а затем под **OpenXR** появляется несколько элементов. Выберите **Microsoft HoloLens feature group**.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/033-ms-hololens-feature-group.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/033-ms-hololens-feature-group.png)

Обратите внимание, что рядом с **OpenXR** есть желтый предупреждающий треугольник. Это указывает на несовместимость настроек, которые необходимо устранить.

### **Устранение несовместимых настроек**

1. Наведите курсор на желтый предупреждающий треугольник рядом с **OpenXR**, затем прочитайте сообщение во всплывающем окне и выберите треугольник.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/014-yellow-triangle-warning.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/014-yellow-triangle-warning.png)

2. В окне **OpenXR Project Validation** перечислены несколько проблем. Нажмите кнопку **Fix All**.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/015-fix-all-button.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/015-fix-all-button.png)

3. Остается одна проблема, в которой указано, что необходимо добавить хотя бы один профиль взаимодействия. Для этого нажмите **Edit**. После этого вы перейдете к настройкам плагина **OpenXR** в окне **Project Settings**.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/016-openxr-screen.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/016-openxr-screen.png)

4. Под **Interaction Profiles** обратите внимание на кнопку со знаком плюс (+).

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/017-add-profile-button.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/017-add-profile-button.png)

5. Выберите профили:
    - **Eye Gaze Interaction Profile;**
    - **Microsoft Hand Interaction Profile;**
    - **Microsoft Motion Controller Profile.**

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/018-interaction-profiles.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/018-interaction-profiles.png)

    Если **Eye Gaze Interaction Profile** или любой другой профиль отображается с желтым треугольником, то исправьте его.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/019-fix-eye-gaze.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/019-fix-eye-gaze.png)

6. В окне **Настройки проекта** в разделе **OpenXR Feature Groups** убедитесь, что выбраны следующие параметры:
    - **Microsoft HoloLens;**
    - **Hand Tracking;**
    - **Motion Controller Model.**

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/020-selected-features.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/020-selected-features.png)

7. Щелкните раскрывающийся список **Depth Submission Mode** и выберите **Depth 16 Bit**.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/021-depth-submission-mode.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/021-depth-submission-mode.png)

    > **Совет**
    Уменьшение формата глубины до 16 бит необязательно, но это может повысить производительность графики в вашем проекте. Чтобы узнать больше, см. [**Совместное использование буфера глубины (HoloLens)**](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/mrtk-unity/performance/perf-getting-started#single-pass-instanced-rendering).
    >

### **Настройка параметров проигрывателя**

1. В левой колонке окна **Project Settings** выберите **Player**.
2. Обратите внимание, что в окне **Player** поле с **Product Name** уже заполнено. Это взято из названия вашего проекта, и оно будет отображаться в меню "Пуск" HoloLens.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/025-product-name.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/025-product-name.png)

    > **Совет**
    Чтобы упростить поиск приложения во время разработки, добавьте знак подчеркивания перед названием, чтобы оно располагалось вверху любого списка.
    >
3. Щелкните раскрывающийся список **Publishing Settings**, а затем в поле **Package name** введите подходящее имя.

    ![https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/026-package-name.png](https://azure.microsofts.workers.dev/en-us/windows/mixed-reality/develop/unity/images/026-package-name.png)

    > **Примечание**
    Имя пакета является уникальным идентификатором приложения. Если вы хотите избежать перезаписи ранее установленных версий приложения с тем же именем, вам следует изменить этот идентификатор перед развертыванием приложения.
    >

### Выберите тип проекта

1. Выберите из списка платформу HoloLens 2 по пути **Mixed Reality > Project Validation Settings > HoloLens 2 Application (UWP)**

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled.png)

2. Откройте окно **Project Settings > OpenXR Project Validation** и если появились какие-то ошибки, то исправьте их.
3. Закройте окно **Project Settings**.

### Выберите тип проекта

1. Откройте панель **MRTK Project Configuration** по пути **Mixed Reality > Toolkit > Utilies > Configure Project for MRTK;**

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%201.png)

2. Нажмите **Next**;

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%202.png)

3. Нажмите **Apply** для того чтобы применить предлагаемые изменения;

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%203.png)

4. Нажмите **Next**;

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%204.png)

5. Нажмите Import **TMP Essantials** для импортирования **TMP**;

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%205.png)

6. Нажмите **Done.**

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%206.png)

### Настройка шейдеров и материалов

Если проект был настроен на использование универсального конвейера рендеринга (URP)  то выполните следующее действие что бы не было проблем с материалами **Mixed Reality > Toolkit > Utilities > Upgrade MRTK Standard Shader for Lightweight Render Pipeline**

**Теперь вы готовы приступить к разработке с OpenXR в Unity!**

## Настройка сцены для работы с MR

### Добавление заготовки для игрока

1. Откройте панель **MRTK Project Configuration** по пути **Mixed Reality > Toolkit > Add to Scene and Configure.** В сцену добавляется заготовка для игрока и основные компоненты для запуска проекта в смешанной реальности;

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%207.png)

2. Нажмите на появившийся игровой объект **MixedRealityToolkit** и поменяйте профиль конфигурации для Hololens 2.

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%208.png)

## **Проверка проекта (Holographic Remoting)**

1. Теперь, если вы нажмете  **Mixed Reality > Remoting > Holographic Remoting** для режима воспроизведения:

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%209.png)

2. Вы должны увидеть такой маленький экран:

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2010.png)

3. Введите IP-адрес, который отображается на HoloLens 2, нажмите включить, и все готово.

Приложение [Holographic Remoting Player](https://www.microsoft.com/en-us/p/holographic-remoting-player/9nblggh4sv40) удобно показывает вам IP-адрес, который HoloLens 2 использует в вашей сети.

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2011.png)

## **Возможности**

### **Основные строительные блоки**

Все основные строительные блоки для приложений смешанной реальности представлены в соответствии с другими API Unity. Эти строительные блоки доступны как отдельные функции, так и через Mixed Reality Toolkit. Возможно, вам не понадобятся все из них сразу, но рекомендуется изучить их на ранней стадии. После ознакомления с основными строительными блоками, перечисленными ниже, у вас будет набор инструментов, полный функций, которые вы можете интегрировать в проект смешанной реальности самостоятельно или через MRTK.

| Функция | Возможности |
| --- | --- |
| [Camera](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/camera-in-unity) | Полностью оптимизируйте качество изображения и стабильность голограмм в ваших приложениях для смешанной реальности |
| [World locking and spatial anchors](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/spatial-anchors-in-unity) | Решите проблемы со стабилизацией, настройкой камеры и интегрируйте решение для стабильной системы координат |
| [Shared experiences](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/shared-experiences-in-unity) | Просматривайте одну и ту же голограмму в фиксированной точке пространства и взаимодействуйте с ней коллективно с помощью совместного использования пространственных привязок |
| [Gaze](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/gaze-in-unity) | Позвольте пользователям ориентироваться на голограммы, глядя на них |
| [Motion controllers](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/motion-controllers-in-unity) | Добавляйте пространственные действия в свои приложения для смешанной реальности |
| [Gestures](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/gestures-in-unity) | Используйте жесты рук в качестве входных данных при работе в смешанной реальности |
| [Hand and eye tracking](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/hand-eye-in-unity) | Интегрируйте данные для отслеживания движения рук и глаз в свой пользовательский интерфейс |
| [Spatial mapping](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/spatial-mapping-in-unity) | Нанесите на карту вашего физического пространства наложение виртуальной сетки, чтобы обозначить границы вашего окружения |
| [Spatial sound](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/spatial-sound-in-unity) | Улучшайте свои приложения с помощью захватывающего 3D-звука |
| [Text](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/text-in-unity) | Получайте четкий текст высокого качества с управляемым размером и качественной визуализацией |
| [Voice input](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/voice-input-in-unity) | Записывайте произносимые ключевые слова, фразы и диктовку ваших пользователей |

### **Расширенные возможности**

Другие ключевые функции, которые играют важную роль в приложениях смешанной реальности, доступны через API Unity без каких-либо дополнительных пакетов или настроек. Эти функции могут быть добавлены в проекты Unity с установленным MRTK или без него. Изучив более продвинутые возможности Unity, вы сможете создавать более глубокие и сложные приложения для смешанной реальности.

| Функция | Возможности |
| --- | --- |
| [Photo video camera](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/locatable-camera-in-unity) | Создавайте фотографии и видеоконтент в своем приложении для смешанной реальности |
| [Focus point](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/focus-point-in-unity) | Дайте HoloLens подсказку о том, как наилучшим образом выполнить стабилизацию отображаемых в данный момент голограмм |
| [Tracking loss](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/tracking-loss-in-unity) | Справляйтесь со сценариями, в которых ваше устройство не может найти себя в пространстве приложений |
| [Keyboard input](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/keyboard-input-in-unity) | Вводите данные с клавиатур реального мира и смешанной реальности в своих приложениях |

### Дополнительная информация

## **Classes**

| [AnchorConverter](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.anchorconverter?view=mixedreality-openxr-plugin-1.10) | Предоставляет вспомогательные функции для преобразования объекта привязки Unity в базовый дескриптор привязки OpenXR или COM-объект SpatialAnchor. |
| --- | --- |
| [ARMarker](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.armarker?view=mixedreality-openxr-plugin-1.10) | Представляет собой маркер, обнаруженный устройством дополненной реальности. |
| [ARMarkerManager](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.armarkermanager?view=mixedreality-openxr-plugin-1.10) | Менеджер для ARMarker. Создает, обновляет и удаляет GameObject  маркеры в ответ на обнаруженные поверхности в физической среде. |
| [ARMarkerScale](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.armarkerscale?view=mixedreality-openxr-plugin-1.10) | Единоличное поведение, помогающее масштабировать обнаруженные маркеры. |
| [ControllerModel](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.controllermodel?view=mixedreality-openxr-plugin-1.10) | Предоставляет доступ к потоку байтов, представляющему модель glTF текущего контроллера. |
| [ControllerModelArticulator](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.controllermodelarticulator?view=mixedreality-openxr-plugin-1.10) | Обрабатывает артикуляцию анимируемых частей модели контроллера. |
| [EyeLevelSceneOrigin](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.eyelevelsceneorigin?view=mixedreality-openxr-plugin-1.10) | Добавьте компонент EyeLevelSceneOrigin в сцену, он автоматически переключит исходную сцену Unity в режим просмотра на уровне глаз. Он попытается использовать "Неограниченный" исходный режим, если он поддерживается. |
| [GestureRecognizer](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.gesturerecognizer?view=mixedreality-openxr-plugin-1.10) | Распознаватель жестов интерпретирует взаимодействия пользователя, начиная с рук, контроллеров движения и системных голосовых команд, и заканчивая событиями пространственных жестов, на которые пользователи ориентируются с помощью взгляда или луча, указывающего рукой. |
| [HandMeshTracker](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.handmeshtracker?view=mixedreality-openxr-plugin-1.10) | Представляет руку пользователя и возможность отображать ее в виде сетки рук. |
| [HandTracker](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.handtracker?view=mixedreality-openxr-plugin-1.10) | Представляет руку пользователя и возможность отслеживать суставы рук по ней. |
| [MeshSettings](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.meshsettings?view=mixedreality-openxr-plugin-1.10) | Статическая точка входа для обновления параметров mesh-вычислений. |
| [OpenXRContext](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.openxrcontext?view=mixedreality-openxr-plugin-1.10) | Извлеките текущий экземпляр OpenXR, дескрипторы сеанса и состояния. |
| [PerceptionInterop](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.perceptioninterop?view=mixedreality-openxr-plugin-1.10) | Функции взаимодействия для API восприятия Windows |
| [RequiresNativePluginDLLsAttribute](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.requiresnativeplugindllsattribute?view=mixedreality-openxr-plugin-1.10) |  |
| [SelectKeywordRecognizer](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.selectkeywordrecognizer?view=mixedreality-openxr-plugin-1.10) | Распознаватель ключевых слов, прослушивающий ключевое слово "select", локализованное на системном языке отображения HoloLens 2. |
| [SpatialGraphNode](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.spatialgraphnode?view=mixedreality-openxr-plugin-1.10) | Узел пространственного графика представляет собой точку, отслеживаемую в пространстве, предоставленную драйвером. |
| [TrackingMapManager](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.trackingmapmanager?view=mixedreality-openxr-plugin-1.10) | Класс TrackingMapManager позволяет приложению выбирать запуск в режиме отслеживания исключительно для приложений вместо общей среды по умолчанию. |
| [ViewConfiguration](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.viewconfiguration?view=mixedreality-openxr-plugin-1.10) | Просматривайте конфигурацию текущего сеанса XR и управляйте ею. |
| [XRAnchorStore](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.xranchorstore?view=mixedreality-openxr-plugin-1.10) | Обрабатывает сохраняемые привязки из сцены в хранилище привязок, загружает привязки из хранилища привязок в сцену и управляет привязками, сохраняемыми в хранилище привязок. |
| [XRAnchorTransferBatch](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.xranchortransferbatch?view=mixedreality-openxr-plugin-1.10) | Предоставляет возможность создавать пакет привязок и экспортировать их в двоичный поток для передачи. Обычно на втором устройстве затем поддерживается импорт потока передачи и загрузка исходного пакета привязок. |

**Structs**

| [ARMarkersChangedEventArgs](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.armarkerschangedeventargs?view=mixedreality-openxr-plugin-1.10) | Аргументы события для события markersChanged. Следуя шаблону проектирования, установленному UnityEngine.XR.ARFoundation.ARPlanesChangedEventArgs |
| --- | --- |
| [GestureEventData](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.gestureeventdata?view=mixedreality-openxr-plugin-1.10) | Данные о событии жеста включают тип события, владение рукой, позы и т.д. |
| [HandJointLocation](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.handjointlocation?view=mixedreality-openxr-plugin-1.10) | Представляет данные о местоположении сустава кисти. |
| [ManipulationEventData](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.manipulationeventdata?view=mixedreality-openxr-plugin-1.10) | Данные события манипулирующего жеста |
| [MeshComputeSettings](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.meshcomputesettings?view=mixedreality-openxr-plugin-1.10) | Параметры, описывающие качество и тип сетки, которые должны быть предоставлены. |
| [NavigationEventData](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.navigationeventdata?view=mixedreality-openxr-plugin-1.10) | Данные события навигационного жеста |
| [QRCodeProperties](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.qrcodeproperties?view=mixedreality-openxr-plugin-1.10) | Представляет свойства обнаруженных QR-кодов. |
| [ReprojectionSettings](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.reprojectionsettings?view=mixedreality-openxr-plugin-1.10) | Настройки для управления перепроецированием текущего кадра рендеринга, включая режим перепроецирования и необязательное переопределение плоскости стабилизации. |
| [TappedEventData](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.tappedeventdata?view=mixedreality-openxr-plugin-1.10) | Данные события жеста касания |

**Enums**

| [ARMarkerType](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.armarkertype?view=mixedreality-openxr-plugin-1.10) | Типы маркеров, которые можно отслеживать. |
| --- | --- |
| [FrameTime](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.frametime?view=mixedreality-openxr-plugin-1.10) | Выберите прогнозируемое время отображения кадра при конвейерном рендеринге. |
| [GestureEventType](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.gestureeventtype?view=mixedreality-openxr-plugin-1.10) | Представляет тип события средства распознавания жестов |
| [GestureHandedness](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.gesturehandedness?view=mixedreality-openxr-plugin-1.10) | Представляет руку, которая инициировала жест. |
| [GestureSettings](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.gesturesettings?view=mixedreality-openxr-plugin-1.10) | Представляет набор жестов, которые могут быть распознаны устройством распознавания жестов. |
| [Handedness](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.handedness?view=mixedreality-openxr-plugin-1.10) | Описывает, какую руку представляет текущий трекер раздач. |
| [HandJoint](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.handjoint?view=mixedreality-openxr-plugin-1.10) | Поддерживаемые отслеживаемые суставы рук в OpenXR. |
| [HandJointsMotionRange](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.handjointsmotionrange?view=mixedreality-openxr-plugin-1.10) | Диапазон движения суставов рук, запрашиваемый контроллером. |
| [HandPoseType](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.handposetype?view=mixedreality-openxr-plugin-1.10) | Представляет различные возможные положения рук. |
| [MeshComputeConsistency](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.meshcomputeconsistency?view=mixedreality-openxr-plugin-1.10) | Согласованность вычислений для запроса из XRMeshSubsystem. |
| [MeshType](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.meshtype?view=mixedreality-openxr-plugin-1.10) | Тип сетки для запроса из XRMeshSubsystem. |
| [QRCodeType](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.qrcodetype?view=mixedreality-openxr-plugin-1.10) | Представляет типы QR-кодов, которые могут быть обнаружены. |
| [ReprojectionMode](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.reprojectionmode?view=mixedreality-openxr-plugin-1.10) | Режим перепроектирования описывает режим перепроектирования слоя проекционной композиции. |
| [TrackingMapType](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.trackingmaptype?view=mixedreality-openxr-plugin-1.10) | Типы карт отслеживания |
| [TransformMode](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.transformmode?view=mixedreality-openxr-plugin-1.10) | Типы преобразований, которые можно применять к маркерам. |
| [ViewConfigurationType](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.viewconfigurationtype?view=mixedreality-openxr-plugin-1.10) | Конфигурация представления - это семантически значимый набор одного или нескольких представлений, для которых приложение может отображать изображения. |
| [VisualMeshLevelOfDetail](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.mixedreality.openxr.visualmeshlevelofdetail?view=mixedreality-openxr-plugin-1.10) | Уровень детализации визуальной сетки для запроса из XRMeshSubsystem. |

| Отображаемое имя | Название пакета | **Описание** |
| --- | --- | --- |
| MRTK Core Definitions | org.mixedrealitytoolkit.core | Общие определения, утилиты и компоненты. |
| MRTK Accessibility | org.mixedrealitytoolkit.accessibility | Определения, функции и подсистема для создания доступных интерфейсов смешанной реальности. |
| MRTK Audio Effects | org.mixedrealitytoolkit.audio | Эффекты и функции, улучшающие качество звука в смешанной реальности. |
| MRTK Data Binding and Theming | org.mixedrealitytoolkit.data | Поддержка привязки данных и тематизации элементов пользовательского интерфейса. |
| MRTK Diagnostics | org.mixedrealitytoolkit.diagnostics | Подсистемы и инструменты диагностики и мониторинга производительности. |
| MRTK Environment | org.mixedrealitytoolkit.environment | Функции и подсистемы среды, такие как представление о пространстве и границах. |
| MRTK Extended Assets | org.mixedrealitytoolkit.extendedassets | Дополнительный звук, шрифт, текстура и другие ресурсы для использования в приложениях. |
| MRTK Graphics Tools | org.mixedrealitytoolkit.graphicstools.unity | Шейдеры, текстуры, материалы и модели. |
| MRTK Input | org.mixedrealitytoolkit.input | Компоненты ввода, включая поддержку артикулированных рук, автономное распознавание речи и имитацию ввода в редакторе. |
| MRTK Spatial Manipulation | org.mixedrealitytoolkit.spatialmanipulation | Компоненты и утилиты для пространственного позиционирования и манипулирования ими, включая решатели. |
| MRTK Standard Assets | org.mixedrealitytoolkit.standardassets | Стандартные ресурсы, включая материалы и текстуры, для использования приложениями. |
| MRTK Tools | org.mixedrealitytoolkit.tools | Набор инструментов редактора Unity, используемых для расширения и оптимизации приложений MRTK3. |
| MRTK UX Components | org.mixedrealitytoolkit.uxcomponents | Библиотека компонентов MRTK UX, содержащая префабы, визуальные элементы, готовые элементы управления и все необходимое для начала создания 3D-пользовательских интерфейсов для смешанной реальности. |
| MRTK UX Components (Non-Canvas) | org.mixedrealitytoolkit.uxcomponents.noncanvas | Библиотека компонентов UX, отличных от Canvas, MRTK для создания 3D UX без макета Canvas. Для большинства пользовательских интерфейсов производственного уровня мы рекомендуем динамические гибридные системы пользовательского интерфейса на основе Canvas, расположенные в org.mixedrealitytoolkit.uxcomponents. Однако в некоторых случаях статический пользовательский интерфейс / интерфейс без Canvas может обеспечить улучшенную производительность и пакетную обработку и может быть желателен в сценариях с ограниченными ресурсами. |
| MRTK UX Core | org.mixedrealitytoolkit.uxcore | Основные скрипты взаимодействия и визуализации для создания компонентов пользовательского интерфейса MR.\ n \ Ппримечание: это предназначено для использования при создании библиотек пользовательского интерфейса. Чтобы создать интерфейсы MR с использованием уже существующей библиотеки компонентов, см. org.mixedrealitytoolkit.uxcomponents. |
| MRTK Windows Speech | org.mixedrealitytoolkit.windowsspeech | Реализация речевой подсистемы для встроенных речевых API Windows. Позволяет использовать встроенное распознавание речи Windows для запуска событий и управления взаимодействиями XRI. |

## Создание конечной сборки на устройство

1. Нажмите **File > Build Profiles**;
2. В разделе **Build and Run on** выберите **Remote Device**;
3. Теперь нужно получить IP Address очков (очки и компьютер должны быть подключены к одному WiFi):
    1. Нажмите на иконку WiFi в главном меню;
    2. Откроются настройки WiFi, спуститесь в самый низ и найдите надпись **Addapter properties**, нажмите на неё;
    3. Найдите пункт **IPv4 address**, это и есть нужный нам адрес.
4. Заполните поля:
    1. **Device Portal Address** - (**IPv4 address**) в формате [https://192.168.137.130](https://192.168.137.130/);
    2. **Device Portal Username** - kb-14;
    3. **Device Portal Password** - kb4forall.
5. Нажмите **Build And Run** и сборка создастся и отправиться на очки.

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2012.png)

6. Возможные причины ошибки при создании сборки проекта
    1. кириллица в названии проекта;
    2. кириллица в пути проекта;
    3. неправильный IP адрес или имя или пароль;

## Функции

### MRTK ToolBox

**MRTK Toolbox** - это оконная утилита редактора Unity, которая упрощает поиск и создание готовых компонентов MRTK UX в текущей сцене. Элементы можно отфильтровать в режиме просмотра с помощью строки поиска в верхней части окна. Окно toolbox предназначено для создания готовых префабов MRTK в текущей сцене. Существуют дополнительные компоненты "UX" , которые могут быть добавлены в качестве компонентов сценария, такие как элементы управления [bounds control](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/ux-building-blocks/bounds-control?view=mrtkunity-2022-05) или [object manipulator](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/ux-building-blocks/object-manipulator?view=mrtkunity-2022-05).

**MRTK Toolbox** устанавливается через MRTK Feature Tool в разделе Mixed Reality Toolkit.

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2013.png)

После добавления инструментов, в Unity кнопка для открытия панели инструментов **Mixed Reality > Toolkit > Toolbox.**

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2014.png)

Откроется вот такая панель как на изображении выше, в ней есть множество категорий элементов для пользовательского интерфейса. Их можно разделить на две группы:

- Которые должны использоваться для **Canvas**, сюда относится только последняя категория, когда вы создадите **Canvas**, конвертируете его в **MRTK** Canvas, то его можно будет выбрать и добавлять ему элементы;
- Которые используются независимо от **Canvas**, это простые 3D объекты из которых можно собирать свои пользовательские интерфейсы из объёмных элементов без привязки к **Canvas**.

Рекомендуется делать пользовательские интерфейсы из второго типа элементов, так как список возможностей более обширный.

### Mixed Reality Toolkit Examples

Это сборник примеров реализации различных вещей. Он содержит различные объекты, пользовательские интерфейсы.

**Mixed Reality Toolkit Examples** устанавливается через MRTK Feature Tool в разделе Mixed Reality Toolkit.

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2015.png)

После установки ресурсы будут в **Packages > Mixed Reality Toolkit Examples > Common.**

И ещё множество других примеров может быть импортировано через Package Manager.

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2016.png)

## Продолжение разработки

Основные профили SDK Mixed Reality Toolkit позволяют быстро приступить к работе. Но их нельзя редактировать, это шаблоны, но если их скопировать, то можно модифицировать под свои потребности:

1. Создадим пустую папку в которой будем хранить все созданные профили, это надо для удобства;
2. Нажмём **Clone** рядом с текущим профилем и настроим, что копируем все дочерние профили что бы была возможность и их дальнейшего редактирования.

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2017.png)

### **Обновление материала**

В целях повышения производительности рекомендуется использовать материалы MRTK вместо материалов Unity по умолчанию. Замените материал по умолчанию материалом **MRTK_Standard_White.**

### Диагностика

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2018.png)

Для того что бы отключить экран с диагностической информацией перейдите в раздел **Diagnostics** и отключите параметр **Show Diagnostics**.

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2019.png)

### Spatial Awareness (Пространственное воображение)

Система пространственной осведомленности обеспечивает осведомленность об окружающей среде в реальном мире в приложениях смешанной реальности. При внедрении в Microsoft HoloLens программа Spatial Awareness представляла собой набор сеток, представляющих геометрию окружающей среды, что позволяло создавать убедительные взаимодействия между голограммами и реальным миром.

Перейдите в раздел **Spatial Awareness** и установите флажок **Enable Spatial Awareness System**.

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2020.png)

# ПРОВЕРИТЬ НАБЛЮДАТЕЛЕЙ И ДОПИСАТЬ

### Input (Ввод)

MRTK позволяет настроить отработку логики входных данных с помощью:

- [Контроллеры](https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk2/features/input/controllers?view=mrtkunity-2022-05) / [Controllers](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/input/controllers?view=mrtkunity-2022-05)
- [Создание поставщика входных системных данных](https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk2/features/input/create-data-provider?view=mrtkunity-2022-05) / [Creating an input system data provider](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/input/create-data-provider?view=mrtkunity-2022-05)
- [Диктовка](https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk2/features/input/dictation?view=mrtkunity-2022-05) / [Dictation](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/input/dictation?view=mrtkunity-2022-05)
- [Взгляд](https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk2/features/input/gaze?view=mrtkunity-2022-05) / [Gaze](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/input/gaze?view=mrtkunity-2022-05)
- [Жесты](https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk2/features/input/gestures?view=mrtkunity-2022-05) / [Gestures](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/input/gestures?view=mrtkunity-2022-05)
- [Отслеживание рук](https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk2/features/input/hand-tracking?view=mrtkunity-2022-05) / [Hand tracking](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/input/hand-tracking?view=mrtkunity-2022-05)
- [Указатели](https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk2/features/input/pointers?view=mrtkunity-2022-05) / [Pointers](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/input/pointers?view=mrtkunity-2022-05)
- [Речь](https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk2/features/input/speech?view=mrtkunity-2022-05) / [Speech](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/input/speech?view=mrtkunity-2022-05)
- [Отслеживание взгляда](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/input/eye-tracking/eye-tracking-main?view=mrtkunity-2022-05) / [Eye tracking](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/input/eye-tracking/eye-tracking-main?view=mrtkunity-2022-05)

MRTK имеет функцию, позволяющую визуализировать руки, то есть он показывает, как именно программа отрабатывает ваши руки. По умолчанию в редакторе это функция включена, но в конечной сборке нет.

1. Сделаем копию профиля отслеживания рук;

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2021.png)

2. Включим визуализацию рук.

    ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2022.png)

### Создание объекта для взаимодействия

Создадим куб, с которым сможем взаимодействовать:

1. Создайте куб, уменьшите его размеры до 0.1, передвиньте его вперёд и поднимите X = 0, Y = 1.6, Z = 0,5;
2. Чтобы с объектом можно было взаимодействовать и манипулировать им движениями рук, у него должно быть 3 компонента:
    1. Компонент **Collider**;
    2. Компонент **Object Manipulator**. ObjectManipulator делает объект перемещаемым, масштабируемым и вращаемым, используя одну или две руки. При добавлении компонента автоматически добавляется **Constraint Manager**, так как **Object Manipulator** зависит от него;
    3. Компонент **Constraint Manager** (Диспетчер ограничений).
3. Добавьте Rigidbody по желанию для отработки физики.  

- **Основные параметры:**
  - **Manipulation type** - Указывает, можно ли манипулировать объектом одной рукой и/или двумя.
  - **Allow far manipulation** - Указывает, можно ли выполнять манипуляции с помощью удаленного взаимодействия с указателями.
  - **Two handed manipulation type** - Определяет, как манипулирование двумя руками может преобразовать объект. Поскольку это свойство является флагом, можно выбрать любое количество параметров.
    - ***Перемещать***: Перемещение разрешено, если оно выбрано.
    - ***Масштаб***: масштабирование разрешено, если оно выбрано.
    - ***Вращать***: вращение разрешено, если оно выбрано.
  - **Physics** - Настройки в этом разделе отображаются только в том случае, если объект имеет компонент RigidBody.
    - **Release behavior** - Укажите, какие физические свойства должен сохранять объект, которым манипулируют, после выпуска. Поскольку это свойство является флагом, можно выбрать оба варианта.
      - ***Сохранять скорость***: При отпускании объекта, если выбран этот параметр, его линейная скорость сохранится.
      - ***Сохранять угловую скорость***: При отпускании объекта, если выбран этот параметр, он сохранит свою угловую скорость.
    - **Use forces for near**  - Используются ли физические силы для перемещения объекта при выполнении манипуляций вблизи. Если установить это значение в *false*, объект будет ощущаться более непосредственно связанным с рукой пользователя. Значение *true* учитывает массу и инерцию объекта, но может показаться, что объект соединен пружиной. Значение по умолчанию - *false*.

        **Без Rigidbody**

        ![https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/object-manipulator/mrtk_physicsmanipulation_norigidbody.gif?view=mrtkunity-2022-05](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/object-manipulator/mrtk_physicsmanipulation_norigidbody.gif?view=mrtkunity-2022-05)

        **С Rigidbody**

        ![https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/object-manipulator/mrtk_physicsmanipulation_rigidbody.gif?view=mrtkunity-2022-05](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/object-manipulator/mrtk_physicsmanipulation_rigidbody.gif?view=mrtkunity-2022-05)

  - **Manipulation events** - Обработчик манипуляций предоставляет следующие события:
    - ***OnManipulationStarted***: запускается при запуске манипуляции.
    - ***OnManipulationEnded***: запускается по завершении манипуляции.
    - ***OnHoverStarted***: срабатывает, когда рука / контроллер наводит курсор на объект манипулирования, близко или далеко.
    - ***OnHoverEnded***: срабатывает, когда рука / контроллер не наводит указатель мыши на объект, которым манипулируют, вблизи или вдалеке.

### Пример пользовательского интерфейса

Создадим простой пользовательский интерфейс с несколькими кнопками и функцией следования за игроком. При создании пользовательского интерфейса будем использовать уже готовые блоки MRTK из категории 3D объекты.

1. Основные функции будущего пользовательского интерфейса:
    1. Три кнопки:
        1. Закрепить пользовательский интерфейс на месте;
        2. Создания примитивного объекта “Сфера”;
        3. Выходы из игры.
    2. Пользовательский интерфейс может следовать за игроком.
2. Исходя из желаемых возможностей нам подойдёт уже готовый шаблон пользовательского интерфейса **MRTK Toolbox > Near Menus > Near Menu 3x1**.
3. Особенность блока **Near Menus:**
    1. **Отслеживание**: меню следует за вами и остается в пределах 30-60 см от пользователя для взаимодействия с ним вблизи.
    2. **Закреп**: С помощью кнопки "Закрепить" меню можно заблокировать по всему миру и разблокировать.
    3. **Захват и перемещение**: Меню всегда можно захватывать и перемещать. Независимо от предыдущего состояния, при захвате и отпускании меню будет закреплено (с блокировкой по всему миру). Для области, которую можно захватить, предусмотрены визуальные подсказки. Они отображаются при приближении руки.

        ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2023.png)

4. **Структура**
Предварительные настройки ближайшего меню создаются с использованием следующих компонентов MRTK.
    1. [PressableButtonHoloLens2 (Нажимаемая кнопка Hololens2)](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/ux-building-blocks/button?view=mrtkunity-2022-05)
    2. [Grid Object Collection (Коллекция объектов сетки)](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/ux-building-blocks/object-collection?view=mrtkunity-2022-05): расположение нескольких кнопок в сетке
    3. [Manipulation Handler (Обработчик манипуляций)](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/ux-building-blocks/manipulation-handler?view=mrtkunity-2022-05): захватите меню и переместите его
    4. [RadialView Solver (Решатель RadialView)](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/ux-building-blocks/solvers/solver?view=mrtkunity-2022-05): поведение "Следуй за мной" (пометка)
5. **Как настраивать пользовательский интерфейс**
    1. **Добавление / удаление кнопок**
    В разделе `ButtonCollection` объект, добавьте или удалите кнопки.

    ![https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/near-menu/mrtk_ux_nearmenu_custom0.png?view=mrtkunity-2022-05](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/near-menu/mrtk_ux_nearmenu_custom0.png?view=mrtkunity-2022-05)

    1. **Обновите коллекцию объектов сетки**

        Нажмите `Update Collection` кнопку в инспекторе `ButtonCollection` объекта. Это обновит макет сетки.

        ![https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/near-menu/mrtk_ux_nearmenu_custom1.png?view=mrtkunity-2022-05](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/near-menu/mrtk_ux_nearmenu_custom1.png?view=mrtkunity-2022-05)

        Вы можете настроить количество строк, используя `Rows` свойство коллекции объектов сетки.

        ![https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/near-menu/mrtk_ux_nearmenu_custom2.png?view=mrtkunity-2022-05](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/near-menu/mrtk_ux_nearmenu_custom2.png?view=mrtkunity-2022-05)

    2. **Отрегулируйте размер задней панели**

        Отрегулируйте размер объекта `Quad` под `Backplate`.Ширина и высота задней панели должны быть `0.032 * [Number of the buttons + 1]`. Например, если у вас 3 кнопки по 2, ширина задней панели равна `0.032 * 4`, а высота - `0.032 * 3`. Вы можете напрямую ввести это выражение в поле Unity.

        ![https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/near-menu/mrtk_ux_nearmenu_custom3.png?view=mrtkunity-2022-05](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/near-menu/mrtk_ux_nearmenu_custom3.png?view=mrtkunity-2022-05)

        - Размер кнопки HoloLens 2 по умолчанию составляет 3,2x3,2 см (0,032 м)
6. Теперь перейдём к редактированию нашего пользовательского интерфейса:
    1. В пользовательском интерфейсе уже есть три кнопки, для начала переименуем их названия в Hierarchy что бы не путаться;
    2. Теперь отредактируем содержание текста в них и значков:
        1. Кнопки MRTK используют `ButtonConfigHelper` компонент, помогающий изменить значок, текст и метку кнопки. (Обратите внимание, что некоторые поля могут отсутствовать, если элементы отсутствуют на выбранной кнопке.)

            ![https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/button/mrtk_button_config_helper.png?view=mrtkunity-2022-05](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/button/mrtk_button_config_helper.png?view=mrtkunity-2022-05)

        2. **Набор значков** - это общий набор ресурсов значков, используемых `ButtonConfigHelper` компонентом. Поддерживаются 3 *стиля* значков:
            1. **Значки в формате Quad** отображаются в формате quad с использованием `MeshRenderer`. Это стиль значков по умолчанию.
            2. **Значки в формате Sprite** отображаются с помощью `SpriteRenderer`. Это полезно, если вы предпочитаете импортировать значки в виде листа спрайтов или если вы хотите, чтобы ресурсы значков использовались совместно с компонентами пользовательского интерфейса Unity. Для использования этого стиля вам потребуется установить пакет редактора спрайтов **(Windows -> Package Manager-> 2D Sprite)**
            3. **Значки в формате Char** отображаются с помощью `TextMeshPro` компонента. Это полезно, если вы предпочитаете использовать шрифт значков. Чтобы использовать шрифт HoloLens icon, вам нужно будет создать `TextMeshPro` ресурс шрифта.

            **Как создать новый значок кнопки:**

            1. В окне **Проекта** щелкните правой кнопкой мыши **Ресурсы**, чтобы открыть контекстное меню. (Вы также можете щелкнуть правой кнопкой мыши любое пустое пространство внутри папки **Assets** или одной из ее вложенных папок.)
            2. Выберите **Create > Mixed Reality > Toolkit > IconSet.**

                ![https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/button/001-icon-set.png?view=mrtkunity-2022-05](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/features/images/button/001-icon-set.png?view=mrtkunity-2022-05)

            Чтобы добавить значки **quad** и **sprite**, просто перетащите их в соответствующие массивы. Чтобы добавить значки Char, необходимо сначала создать и назначить ресурс шрифта.

        После настройки отображения кнопок, получился такой пользовательский интерфейс:

        ![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2024.png)

7. Теперь перейдём к написанию логики пользовательского интерфейса:
    1. Создадим скрипт QuickMenuUI;
    2. Создадим ссылки на кнопки с ограничением доступа только для текущего класса (в MRTK для работы с событиями взаимодействия нужно использовать компонент **Interactable**);

        ```csharp
        [SerializeField] private Interactable spawnSphereButton;
        [SerializeField] private Interactable exitButton;
        ```

    3. Подпишем наши будущие методы для отработки логики каждой из кнопок на события нажатия на кнопку;

        ```csharp
        private void Awake()
        {
            spawnSphereButton.OnClick.AddListener(SpawnSphere);
            exitButton.OnClick.AddListener(Exit);
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

    5. Создадим префаб объекта, который будем создавать по нажатию на кнопку и сохраним его в папку “Resources/Prefabs/Object”;
    6. Реализуем метод для создания объекта, сначала обращаемся к Resources и получаем наш префаб по пути. После чего создаём экземпляр объекта перед меню.

        ```csharp
        private void SpawnSphere()
        {
            var sphere = Resources.Load<GameObject>($"Prefabs/Object");    
            Instantiate(sphere, transform.position - transform.forward * 0.25f, Quaternion.identity);
        }
        ```

8. После того как логика реализована, вернёмся к редактору и добавим наш компонент на UI, и привяжем кнопки;

Был реализован простой пользовательский интерфейс, с базовым функционалом.

![Untitled](%D0%9F%D1%80%D0%B8%D0%BC%D0%B5%D1%80%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20MR%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%B4%20Hololens%202%20b18702a1aeec455e949d38107ecbf3a9/Untitled%2025.png)

## Подведение итогов

**Поздравляем!** Вы добрались до конца этого курса по Unity MR для HoloLens 2.

## Дополнительная информация

- Блог профессионального MR разработчика под HoloLens и не только - [https://localjoost.github.io/](https://localjoost.github.io/)
- Репозитории с примерами проектов от того же разработчика - [https://github.com/LocalJoost?tab=repositories](https://github.com/LocalJoost?tab=repositories)
- **Пример проекта -** Ознакомьтесь с [Репозиторием OpenXR Mixed Reality samples repo](https://github.com/microsoft/OpenXR-Unity-MixedReality-Samples) в нем представлены примеры проектов unity, демонстрирующие, как создавать приложения Unity для HoloLens 2 или гарнитур Mixed Reality с использованием плагина OpenXR Mixed Reality.
- Документация к MRTK, документации расписана в MRTK 2.8, часть в MRTK 3, для корректности пользуйтесь двумя источниками:
  - Англоязычный вариант:
    - MRTK 2.8 - [https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/?view=mrtkunity-2022-05](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk2/?view=mrtkunity-2022-05)
    - MRTK 3 - [https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk3-overview/?view=mrtkunity-2022-05](https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk3-overview/?view=mrtkunity-2022-05)
  - Русскоязычный вариант (не везде корректный перевод):
    - MRTK 2.8 - [https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk2/?view=mrtkunity-2022-05](https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk2/?view=mrtkunity-2022-05)
    - MRTK 3 - [https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk3-overview/?view=mrtkunity-2022-05](https://learn.microsoft.com/ru-ru/windows/mixed-reality/mrtk-unity/mrtk3-overview/?view=mrtkunity-2022-05)
