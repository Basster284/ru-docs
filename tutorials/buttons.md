# Кнопки

Один из самых стандратных элементов интерфейса - простая **кнопка**. Я уверен что вы уже знаете о ней, так что перейдём сразу к делу. Обычно кнопки создаются в форме класса `CCMenuItemSpriteExtra`, что является специфичным для GD выводом класса `CCMenuItemSprite`. Вы также можете использовать любой класс который наследует из `CCMenuItem` как кнопка. А если захочется, вы можете сделать свою кастомную систему кнопок используя систему касаний, но в этом документе и речи про это не будет.

Каждая кнопка, которая наследует `CCMenuItem` **должна быть дочерним классом `CCMenu`**. То есть, если вы добавите кнопку как наследователь  `CCLayer`, она просто не будет работать.

Первый аргумент `CCMenuItemSpriteExtra` - параметр `CCNode*`. Это текстура кнопки; любой `CCNode`, по типу табличка (label) или целый слой, но большинство разработчиков используют спрайт.

Если вы хотите создать кнопку с текстом, самым стандартным решением будет класс `ButtonSprite`. А если хочется круглую кнопку (типа 'New' кнопки в GD), используйте `CCSprite` или [специальный от Geode класс `CircleButtonSprite`](#circle-button-sprites).

```cpp
bool MyLayer::init() {
    // ...

    auto spr = ButtonSprite::create("Hi mom!");

    auto btn = CCMenuItemSpriteExtra::create(
        spr, this, nullptr
    );

    // CCMenu*
    menu->addChild(btn);

    // ...
}
```

Это создаст кнопку с текстом `Hi mom!`.

## Вызовы

Ало? Вызовы кнопок называют **селектроами меню**, и они проходят через третий аргумент в `CCMenuItemSpriteExtra::create`. Селекторы меню - это не статичные функции классов которые возвращают `void` и принимают единственный параметр `CCObject*`. Пример селектора меню:

```cpp
class MyLayer : public CCLayer {
public:
    void onButton(CCObject* sender);
};
```

Принято иметь имена всех селекторов меню в форме `onSomething`. Чтобы добавить кнопку в селектор меню, проведите полное имя селектора меню в макрос `menu_selector`:

```cpp
class MyLayer : public CCLayer {
protected:
    bool init() {
        // ...

        auto btn = CCMenuItemSpriteExtra::create(
            /* спрайт */,
            this,
            menu_selector(MyLayer::onButton)
        );

        // ...
    }

public:
    void onButton(CCObject* sender) {
        std::cout << "Button clicked!\n";
    }
};
```

Внутри функции `onButton`, у вас есть доступ к указателю класса `this`. Параметр `sender` - это указатель к кнопке, которая была только что нажата. Вы можете обратно превратить кнопку в класс `CCMenuItemSpriteExtra` используя `static_cast`:

```cpp
class MyLayer : public CCLayer {
    void onButton(CCObject* sender) {
        std::cout << "Button clicked!\n";
        auto btn = static_cast<CCMenuItemSpriteExtra*>(sender);
        // Делать что-то с кнопкой
    }
};
```

> :warning: Использовать `reinterpret_cast` вместо `static_cast` можно, но на практике так делать лучше не надо.

## Пример

Пример популярного счётчика кликов в cocos2d:

```cpp
class MyLayer : public CCLayer {
protected:
    // Класс, хранящий количество кликов
    size_t m_clicked = 0;

    bool init() {
        if (!CCLayer::init())
            return false;

        auto menu = CCMenu::create();

        auto btn = CCMenuItemSpriteExtra::create(
            ButtonSprite::create("Click me!"),
            this,
            menu_selector(MyLayer::onClick)
        );
        btn->setPosition(100.f, 100.f);
        menu->addChild(btn);

        this->addChild(menu);

        return true;
    }

    void onClick(CCObject* sender) {
        // Увеличить количество кликов
        m_clicked++;

        auto btn = static_cast<CCMenuItemSpriteExtra*>(sender);

        // getNormalImage возвращает спрайт кнопки
        auto spr = static_cast<ButtonSprite*>(btn->getNormalImage());
        spr->setString(CCString::createWithFormat(
            "Clicked %d times", m_clicked
        )->getCString());
    }
};
```

## Больше параметров в вызовах

Одна из распространённых проблем при использовании селекторов меню это когда вы даёте больше аргуметов в функции. Например, что если в счётчик кликов мы хотим добавить кнопку уменьшения количества кликов? Нет, мы конечно можем переделать весь `onClick`, но это немного бесполезно. Вместо этого, мы используем **тэги**.

```cpp
class MyLayer : public CCLayer {
protected:
    // Класс, хранящий количество кликов
    size_t m_clicked = 0;

    bool init() {
        if (!CCLayer::init())
            return false;

        auto menu = CCMenu::create();

        auto btn = CCMenuItemSpriteExtra::create(
            ButtonSprite::create("Click me!"),
            this,
            menu_selector(MyLayer::onClick)
        );
        btn->setPosition(100.f, 100.f);
        btn->setTag(1);
        menu->addChild(btn);

        auto btn2 = CCMenuItemSpriteExtra::create(
            ButtonSprite::create("Decrement"),
            this,
            menu_selector(MyLayer::onClick)
        );
        btn2->setPosition(100.f, 60.f);
        btn2->setTag(-1);
        menu->addChild(btn2);

        this->addChild(menu);

        return true;
    }

    void onClick(CCObject* sender) {
        // Увеличить количество кликов
        m_clicked += sender->getTag();

        auto btn = static_cast<CCMenuItemSpriteExtra*>(sender);

        // getNormalImage возвращает спрайт кнопки
        auto spr = static_cast<ButtonSprite*>(btn->getNormalImage());
        spr->setString(CCString::createWithFormat(
            "Clicked %d times", m_clicked
        )->getCString());
    }
};
```

Если захотите передать что-то типо строк, используйте `setUserObject`.

## Передача не числовых параметрам вызовам

Если вы хотите передать то что не передаётся через тэги, к примеру строки, используйте `setUserObject`.

```cpp
class MyLayer : public CCLayer {
protected:
    // Класс, хранящий количество кликов
    size_t m_clicked = 0;

    bool init() {
        if (!CCLayer::init())
            return false;

        auto menu = CCMenu::create();

        auto btn = CCMenuItemSpriteExtra::create(
            ButtonSprite::create("Click me!"),
            this,
            menu_selector(MyLayer::onClick)
        );
        btn->setPosition(100.f, 100.f);
        btn->setUserObject(CCString::create("Button 1"));
        menu->addChild(btn);

        auto btn2 = CCMenuItemSpriteExtra::create(
            ButtonSprite::create("Decrement"),
            this,
            menu_selector(MyLayer::onClick)
        );
        btn2->setPosition(100.f, 60.f);
        btn->setUserObject(CCString::create("Button 2"));
        menu->addChild(btn2);

        this->addChild(menu);

        return true;
    }

    void onClick(CCObject* sender) {
        // Получить объект пользователя
        auto obj = static_cast<CCNode*>(sender)->getUserObject();
        // Превратить в CCString и получить данные
        auto str = static_cast<CCString*>(obj)->getCString();

        auto btn = static_cast<CCMenuItemSpriteExtra*>(sender);

        // getNormalImage возвращает спрайт кнопки
        auto spr = static_cast<ButtonSprite*>(btn->getNormalImage());
        spr->setString(CCString::createWithFormat(
            "Clicked %s", str
        )->getCString());
    }
};
```

If you want to pass multiple parameters, create your own **aggregate type** that inherits from `CCObject` and store the parameters there:

```cpp
struct MyParameters : public CCObject {
    std::string m_string;
    int m_number;

    MyParameters(std::string const& str, int number) : m_string(str), m_number(number) {
        // Не забудьте вызвать autorelease в ваших классах!
        this->autorelease();
    }
};

// Когда создаёте кнопку:
btn->setUserObject(new MyParameters("Hi!", 7));

// В вызове:
auto parameters = static_cast<MyParameters*>(
    static_cast<CCNode*>(sender)->getUserObject()
);
```

> :warning: There also exists a similarly named `setUserData` member in `CCNode`, but using it should be avoided as unlike `setUserObject` it's not garbage collected and will lead to a **memory leak** unless handled carefully.

## Circle button sprites

> :warning: These are actually way more important than what this short paragraph gives off, but I was too lazy to write more.

Geode comes with a concept known as **based button sprites**, which are button sprites that come with common GD button backgrounds and let you add your own sprite on top. These are useful for texture packs, as texture packers can just style the bases and don't have to individually make every mod fit their pack's style.

```cpp
#include <Geode/ui/BasedButtonSprite.hpp>

// ...

auto spr = CircleButtonSprite::createWithSpriteFrameName("top-sprite.png"_spr);
```
