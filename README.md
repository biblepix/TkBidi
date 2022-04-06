# TkBidi

## The Problem

TkBidi is a Tcl contribution of Peter Vollmar <biblepix@vollmar.ch> to solve the notorious Tk problem of being unable to display RTL (right-to-left) and BIDI (bi-directional) texts correctly. This refers primarily to languages like Arabic, Hebrew, Persian or Urdu, which require that texts in these languages are displayed beginning from right to left. Moreover, the Arabic alphabet has up to four different forms of each letter, depending on its position in a word. 

Depending on the platform, this problem is usually solved either on platform level, or on wordprocessor level. Solving it for Tk widgets has been very challenging indeed.

* Windows: apparently solves the problem on platform level, so TkBidi is not needed; texts are reproduced as they are.

* Unix/Linux: so far, the Tk library and any associated widgets are not able to perform this task. Windows seems to have its own underlying mechanism to solve the problem before texts even reach Tk widgets.

TkBidi is a complete solution for all Linux Tk widgets. It has been tested extensively on the author's own program "BiblePix" <https://biblepix.vollmar.ch>.

It ought to be incorporated as a standard solution into Tk.

## The Solution

	* TkBidi Produces clean RTL texts for display in Tk widgets on Unix platforms
	* non-RTL characters like numbers or foreign names are treated as such
	* TkBidi can be incorporated in any Tcl program that uses text widgets for international text
	* on Microsoft Windows platforms, the text is returned unchanged 
	
## Usage

bidi::fixBidi [TEXT] ?vowelled? ?bdf? ?reqW?

#TODO Argumente anordnen?????????????
catch {set $var [bidi::fixBidi $T 1 0 $reqW]}


This command provides one or more lines of RTL text in the correct order for Tk widgets.
Only the TEXT string is mandatory. The other options function as follows:

	° vowelled (0 or 1): Some Arabic and Hebrew texts come with lots of dots and dashes above and beneath the text; this is a classic way used mainly for poetic and religious texts. TkBidi can devowelize (clean) texts from these extra signs if display is a problem (defaults to 1 = vowelled). Modern text are not usually vowelled, so it is safe to leave this option on.
	
	° bdf (0 or 1) ????????????
	
	° reqW (1 or 0) - required width, numberd in characters): Some Tk widgets require a certain width ; this option helps RTL texts that are wider than the required width to split up at the given width . Note that very long texts are not reproduced correctly unless the width of a Tk widget is given in the argument. This option defaults to 0 (no splitting of text lines)

## 
