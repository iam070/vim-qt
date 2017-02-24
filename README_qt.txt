README_qt.txt for version 7.4 of Vim: Vi IMproved, with qt gui.

See "README.txt" for general information about Vim.
See "README_qt.txt" for some information of Vim-Qt.

# What is vim-qt

It is an experimental Qt gui for Vim, by equalsraf.

# Features

The goal is to introduce features that make vim-Qt more attractive to work with, or just sleek to look at.

### Dockable Bars

The toolbar and tabbar can be docked in various positions, or float around separated from the window. Check the Showcase for examples of this feature.

### Movable tabs

Tabs are movable using the mouse to drag then to their new location

### Fullscreen

You can place enable/disable fullscreen mode using:

    :set fu
    :set nofu

When fullscreen mode is enabled changing the size of the Vim shell cause it to became centered within the window. For example if you want a setup similar to Writeroom

    :set guioptions=-mT
    :set fu
    :set columns=100

### Themed GUI

Qt widgets can be easily customized using stylesheets. No reason we should not use this feature to customize the gui elements. All the native Qt widgets can be customized (toolbars, menus, tabs, etc), however the main Vim editor must still use regular vim color themes. The widgets are themed using a Qt stylesheet placed at $HOME/.config/Vim/qVim.style. Here is an example stylesheet

    MainWindow {
        background: qlineargradient(x1:0, y1:0, x2:0, y2:0.11,
                 stop:0 #a2a2a2, stop: 1 #424242);
        color       : white;
    }

    QToolBar {
    border-bottom   : 1px solid #333333;
    }

    QToolButton::hover {
    background  : red;
    }

    QMenuBar {
    color       : #eee;
    }

    QMenuBar::item::selected {
    background  : transparent;
    }

    QTabBar::tab {
    background  : transparent;
    color       : #eee;
    border      : 1px solid #333;
    padding     : 5px;
    padding-left    : 10px;
    padding-right   : 10px;
    color       : #ccc;
    border-radius   : 4px;
    }

    QTabBar::tab::selected {
    color       : white;
    font-weight : bold;
    }


# Building vim-Qt

### Linux/BSD systems

For the most part you can build it as you would build vim with support for another gui, just issue the following commands:

    $ ./configure --enable-gui=qt
    $ make
    $ sudo make install

All the regular configure options apply. Just don’t forget, you need libqt and libqt4-devel now. Usually you'll want to pass in something like:

    ./configure --prefix=/usr/ --with-features=huge --enable-gui=qt

If configure is unable to find Qt, try passing in the Qt base dir as follows:

    ./configure --enable-gui=qt --with-qt-dir=/usr/lib/qt4

### Windows

As of commit c0956732b437 we have some initial support to build vim-Qt in windows. You can build vim-Qt for windows provided that you have Qt, CMake and an adequate compiler.

Naturally you need to have Qt installed. The best way to do this depends on your setup - check the Qt website for more details and keep in mind that you need to match your Qt library version with your compiler.

When done building vim, the resulting file will be called qvim.exe. In order for vim to be able to display menus, and load settings the .exe must be in same directory as the runtime folder. You can either copy the executable and the runtime folder to the same location, or if gvim is already installed, just place the qvim.exe in the same folder as gvim.exe.
Visual Studio 2010

From the VS console:

    $ cmake -G "NMake Makefiles" PATH_TO_QTVIM\src
    $ nmake

### MinGW

Make sure all mingw tools are in your path (i.e. gcc, mingw32-make), and then from your console:

    $ cmake -G "MinGW Makefiles" PATH_TO_QTVIM\src
    $ mingw32-make

MinGW - cross compiling

You can also cross compile Vim for mingw32, provided that you have a cross compiler. We already provide a CMake toolchain for this, but you may have to tweak it.

Just call cmake with the given toolchain file

    $ cd src
    $ mkdir build
    $ cd build
    $ cmake -DCMAKE_TOOLCHAIN_FILE=../Toolchain-mingw32.cmake ..

The final binary should be a dinamically linked qvim.exe.


# Running vim-Qt

## Running vim-Qt

If you are running from the source directory, the vim binary is built under src/vim To run vim-Qt you can either call vim with the "-g" argument

    $ ./vim -g

or you can create a symlink, or rename the binary, to either gvim or qvim

    $ ln -s vim qvim
    $ ./qvim

Running vim under any of those names should launch the GUI automatically.

## Known Issues

If you have issues rendering fonts in vim-Qt try setting the QVIM_DRAW_STRING_SLOW environment variable, e.g. in Unix systems:

    $ QVIM_DRAW_STRING_SLOW=1 ./vim -g

or in the windows console

    set QVIM_DRAW_STRING_SLOW=1
    vim.exe -g

this will be slower, but should resolve most font drawing issues that you have.


# Development

### Development Workflow

We currently host vim-Qt at the following locations:

* [Bitbucket][1]
* [Github][2]

We try keep all these in sync, so that you can fetch from any of these services and trigger pull requests at any of these services. However issue reporting only happens here at Bitbucket.

### Submitting bugs

General netiquette applies, just be civil and patient. Please include detailed information with the bug report, it helps narrowing down problems:

* Operating system/Linux distribution
* Qt version
* Sometimes some information about your vim configuration may be helpful too
* If possible chek if the vim-gtk version also has the same issue 

### Master branch

As a rule of thumb the master branch should always build cleanly. This is also the branch from where packages are occasionally generated.
Topic branches

It is not uncommon to see topic branches - named as tb-somefeature - for more experimental features. Keep in mind that these may involve forced updates when a rebase occurs.

For bug fixes when some testing is desired - before going into the master branch - a fix branch may be created e.g. fix-cursorisbroken.

### Upstream Vim

Periodically we merge with the upstream Vim repository(default) - github.com/vim/vim - but we do not keep Vim's history on our repository. The merge event shows up as a merge with a very large commit holding all the changes. The rationale for this is that we should be able to easily rebase our changes on top of other vim trees.

In a nutshell merging vim-qt against vim happens as follows

1. Get the vim-qt source using git (git clone)

2. Create a new branch (e.g. git branch upstream) with the upstream version of Vim

        $ git branch upstream
        $ git checkout upstream
        <erase old files>
        $ cp -r ../vim/upstream/* .

3. Commit the upstream version into the branch (git commit -a)

4. Create a new branch to stage a merge

        git branch merge-upstream
        git checkout merge-upstream

5. Finally merge with the master branch

        git merge --no-commit master
        autoreconf

Finally try building and if it succeeds finalize the 
`git commit`. 
Then merge-upstream can be merged into master.

Step 2 looks a bit weird, it makes more sense if you consider you have a paralell branch to track upstream (which is what I do).

[1]: https://bitbucket.org/equalsraf/vim-qt     "bitbucket"
[2]: https://github.com/equalsraf/vim-qt        "github"

