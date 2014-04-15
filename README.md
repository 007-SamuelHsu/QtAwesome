QtAwesome - Font Awesome support for Qt applications
====================================================

Description
-----------

QtAwesome is a simple library that can be used to add Font Awesome icons to your Qt application.
Though the name is QtAwesome and currently it's very Font Awesome based, you can use every other 
iconfont you want.

The class can also be used to manage your own dynamic code-drawn icons, by adding named icon-painters.

The current Font Awesome version is Version 3.2.1


Installation
------------

The easiest way to include QtAweome in your project is to copy the QtAwesome directory to your
project tree and add the the following code include to your Qt project file: 

    include(QtAwesome/QtAwesome.pri)

Now your good to go!!


Usage
-----

You probably want to create a single QtAwesome object for your whole application. 

    QtAwesome* awesome = new QtAwesome( qApp )
    awesome->initFontAwesome();     // This line is important as it loads the font and initializes the named icon map

Add an accessor to this object. (a global function, member of your application object whatever you like).

Use an icon name from the icon copy-past list [http://fortawesome.github.io/Font-Awesome/3.2.1/cheatsheet/](http://fortawesome.github.io/Font-Awesome/3.2.1/cheatsheet/)


Example
--------
  
```c++
// You should create a single object of QtAwesome.
QtAwesome* awesome = new QtAwesome( qApp );
awesome->initFontAwesome();

// Next create your icon with the help of the icon-enumeration: (all dashes are replaced by underscores)
QPushButton* beerButton new QPushButton( awesome->icon( icon_beer ), "Cheers!" );

// You can also use 'string' names to access the icons. (The string version omits the 'icon-' prefix )
QPushButton* coffeeButton new QPushButton( awesome->icon( "coffee" ), "Black please!" );

// When you create an icon you can supply some options for your icons:
// The available options can be found at the "Default options"-section

QVariantMap options;
options.insert( "color" , QColor(255,0,0) );
QPushButton* musicButton = new QPushButton( awesome->icon( icon_music, options ), "Music" ); 

// You can also change the default options. 
// for example if you always would like to have green icons you could call)
awesome->setDefaultOption( "color-disabled", QColor(0,255,0) );

// You can also directly render a label with this font
QLabel* label = new QLabel( QChar( icon_group ) );
label->setFont( awesome->font(16) );

```

Example custom painter
----------------------

This example registers a custom painter for supporting a duplicate icon (it draws 2 plus marks)

```c++
class DuplicateIconPainter : public QtAwesomeIconPainter
{
public:
    virtual void paint( QtAwesome* awesome, QPainter* painter, const QRect& rectIn, QIcon::Mode mode, QIcon::State state, const QVariantMap& options  )
    {
        int drawSize = qRound(rectIn.height()*0.5);
        int offset = rectIn.height() / 4;
        QChar chr = QChar( icon_plus );

        painter->setFont( awesome->font( drawSize ) );

        painter->setPen( QColor(100,100,100) );
        painter->drawText( QRect( QPoint(offset*2, offset*2), QSize(drawSize, drawSize) ), chr , QTextOption( Qt::AlignCenter|Qt::AlignVCenter ) );

        painter->setPen( QColor(50,50,50) );
        painter->drawText( QRect( QPoint(rectIn.width()-drawSize-offset, rectIn.height()-drawSize-offset), QSize(drawSize, drawSize) ), chr , QTextOption( Qt::AlignCenter|Qt::AlignVCenter ) );

    }
};

awesome->give("duplicate", new DuplicateIconPainter() );
```


Default options:
----------------
  
  The following options are default in the QtAwesome class. 

```c++
setDefaultOption( "color", QColor(50,50,50) );
setDefaultOption( "color-disabled", QColor(70,70,70,60));
setDefaultOption( "color-active", QColor(10,10,10));
setDefaultOption( "color-selected", QColor(10,10,10));

setDefaultOption( "text", QString() );      // internal option
setDefaultOption( "text-disabled", QString() );
setDefaultOption( "text-active", QString() );
setDefaultOption( "text-selected", QString() );

setDefaultOption( "scale-factor", 0.9 );
```

  When creating an icon, it first populates the options-map with the default options from the QtAwesome object.
  After that the options are expanded/overwritten by the options supplied to the icon.
 
  It is possible to use another glyph per icon-state. For example to make an icon-unlock symbol switch to locked when selected,
  you could supply the following option.

```c++  
  options.insert("text-selected", QString( icon_lock ) );
```

License
-------

MIT License. Copyright 2013 - Reliable Bits Software by Blommers IT. [http://blommersit.nl/](http://blommersit.nl)

The Font Awesome font is licensed under the SIL Open Font License - [http://scripts.sil.org/OFL](http://scripts.sil.org/OFL)
The Font Awesome pictograms are licensed under the CC BY 3.0 License - [http://creativecommons.org/licenses/by/3.0/](http://creativecommons.org/licenses/by/3.0/)
"Font Awesome by Dave Gandy - http://fortawesome.github.com/Font-Awesome"

Contact
-------

* email: <rick@blommersit.nl>
* twitter: [https://twitter.com/gamecreature](https://twitter.com/gamecreature)
* website: [http://blommersit.nl](http://blommersit.nl)  (warning Dutch content ahead)
* github: [https://github.com/gamecreature/QtAwesome](https://github.com/gamecreature/QtAwesome)

Remarks
-------

I've created this project because I needed some nice icons for my own Qt project. After doing a lot of 
css/html5 work and being spoiled by the ease of twitter bootstrap with Font Awesome, 
I thought it would be nice to be able to use these icons for my Qt project.

I've slightly changed the code from the original, added some more documentation, but it's still
a work in progress. So feel free to drop me an e-mail for your suggestions and improvements! 

There are still some things todo, like:

  * document the usage of another icon font
  * add some tests
  * do some code cleanup
  
And of course last but not least, 

Many thanks go to Dave Gandy an the other Font Awesome contributors!! [http://fortawesome.github.com/Font-Awesome](http://fortawesome.github.com/Font-Awesome)  
And of course to the Qt team/contributors for supplying this great cross-platform c++ library.


Contributions are welcome! Feel free to fork and send a pull request through Github.
