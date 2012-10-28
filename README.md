## ConsoleD

### License

 This library is licensed under Boost License.

### About

ConsoleD is open-source, small library written in [D Programming Language](http://dlang.org) that 
helps you add colors and formatting to your console output. Work on both Windows and Posix operating systems.

#### Important notes:

 * Font styles(underline, strikethrough) have no effect on Windows OS.
 * Light background colors are not supported on Posix, Non-light equivalents are used.
 * _Temponary_: Because `core.sys.posix.sys.ioctl` module was added recently, you must compile project with this [file](https://github.com/D-Programming-Language/druntime/blob/master/src/core/sys/posix/sys/ioctl.d).

### Examples

#### Adding colors

```D
import std.stdio, consoled;

void main()
{
    setConsoleForeground(Color.red);
    writeln("foo"); // Fg: Red | Bg: Default
    
    setConsoleBackground(Color.blue);
    writeln("foo"); // Fg: Red | Bg: Blue
    
    resetConsoleColors(); // Bring back initial state
}
```

or:

```D
import std.stdio, consoled;

void main()
{
    setConsoleColors(Fg.red, Bg.blue); /// Order does not matter as long parameters are Fg or Bg.
    writeln("foo"); // Color: Red | Bg: Blue
    
    resetConsoleColors(); // Bring back initial state
}
```


#### Current Foreground/Background

To get current foreground and background colors, simply call `getConsoleForeground` or `getConsoleBackground`

```D
import std.stdio, consoled;

void main()
{
    auto currentFg = getConsoleForeground();
    auto currentBg = getConsoleBackground();
}
```


#### Font Styles

You can change font styles, like `strikethrough` and `underline`. This feature is Posix only, when called on windows, nothing happens.

```D
import std.stdio, consoled;

void main()
{
    setFontStyle(FontStyle.underline | FontStyle.strikethrough);
    writeln("foo");
    resetFontStyle(); // Or just setFontStyle(FontStyle.none);
}
```

#### Easy colored messages

You can use helper function `writec` or `writecln` to easily  colored messages.

```D
import std.stdio, consoled;

void main()
{
    writecln("Hello ", Fg.blue, "World", Bg.red, "!");
    resetConsoleColors();
}
```

#### Console Size

You can get console size using `getConsoleSize()` which return tuple containg width and height of the console.

```D
import std.stdio, consoled;

void main()
{
    auto size = getConsoleSize();
    writeln(size);
}
```

#### Cursor manipulation

You can set cursor position using `setConsoleCursor()`:

```D
import std.stdio, consoled;

void main()
{
    auto size = getConsoleSize();
    // 6 is half of "insert coin" length.
    setConsoleCursor(size.x / 2 - 6, size.y / 2);
    writeln("Insert coin");
}
```

#### Clearing the screen

You can clear console screen using `clearConsoleScreen()` function:

```D
import std.stdio, consoled, core.thread;

void main()
{
	// Fill whole screen with hashes
    fillArea(ConsolePoint(0, 0), getConsoleSize(), '#');
	
	// Wait 3 seconds
	Thread.sleep(dur!"seconds"(3));
	
	// Clear the screen
	clearConsoleScreen();
}
```


#### Setting the title

To set console title, use `setConsoleTitle()`:


```D
import std.stdio, consoled;

void main()
{
	setConsoleTitle("My new title");
}
```
