# TkBidi

## The Problem

TkBidi is a Tcl contribution of Peter Vollmar <biblepix@vollmar.ch> to solve the notorious Tk problem of being unable to display RTL (right-to-left) and BIDI (bi-directional) texts correctly. This refers primarily to languages like Arabic, Hebrew, Persian or Urdu, which require that texts in these languages are displayed beginning from right to left. Moreover, the Arabic alphabet has up to four different forms of each letter, depending on its position in a word. 

The crux of the matter is that texts in these languages are stored in left-to-right alignment, like any other texts. So the correct letter order is only tackled once such texts reach a word processor, an Internet browser or any other display unit. 
While modern display programs on any platform have performed this task well, solving it for Tk widgets has been very challenging indeed.

* Windows: apparently solves the problem on platform or display level, so TkBidi is not needed for Tk widgets on Windows. Any text passed through TkBidi on Windows is reproduced without change.

* Unix/Linux: so far, the Tk library and any associated widgets are not able to perform this task. This is where TkBidi comes in. TkBidi is a complete solution for all Linux Tk widgets. It has been tested extensively on the author's own program "BiblePix" <https://biblepix.vollmar.ch>.

TkBidi ought to be incorporated as a standard solution into Tk.

## The Solution

	* TkBidi produces clean RTL texts for display in Tk widgets on Unix platforms
	* non-RTL characters like numbers or foreign names are reproduced unchanged
	* TkBidi can easily be incorporated in any Tcl program that uses text widgets for international text
	* on Microsoft Windows platforms, the text is returned unchanged 

## Usage

bidi::fixBidi [TEXT] ?vowelled? ?reqW? ?ltr?

The [TEXT] argument is required, any other arguments are extra.

This command provides one or more lines of RTL text in the correct order for Tk widgets.
Only the TEXT string is mandatory. The other options function as follows:

   **vowelled** (0 or 1): The Hebrew and Arabic writing systems are considered "defective" because they omit a great number of vowels like *a, e, i, o, u*. The reader must be able to read words correctly even when only the consonants are written. Both languages however have a classical way of putting missing vowels above or beneath the letters as dots or dashes. This feature is used mainly in poetic and religious texts (foremost the Qur'an and the Hebrew Bible). TkBidi can devowelize texts, i.e. strip any vowel signs if displaying them is a problem (defaults to 1 = vowelled). Modern texts in either language are not usually vowelled, so it is safe to leave this option on.
	
   **ltr** (0 or 1): in some rare cases right-to-left alignment is not desired; with this option set to 1, while adhering to Arabic letter forms, the text is displayed in the "wrong" direction (defaults to 0 = right-to-left)
	
   **reqW** (1 or 0) - *required width*, numbered in characters: Many Tk widgets require a width argument; this option helps RTL texts that are wider than the desired width to split up at the given width. Note that very long texts are not reproduced correctly unless the width of a Tk widget is given in the argument. This option defaults to 0 (no splitting of text lines).

## Examples

*Note: the outcome of the below text examples depends on the ability of your displaying program to handle bidirectional text.*
    
* Simple example with unvowelled Hebrew:

    set T {תירבעב טסקט}
    bidi::fixBidi $T
    
    produces: טקסט בעברית 

* Simple example with unvowelled Arabic:

    bidi::fixBidi "؟ ةروصب ةيآ جمانرب وه ام"
    
    produces: ما هو برنامج آية بصورة ؟


* Arabic long text with required width argument:
        
        set T {، والذي يهدف إلى نشر الكلمة في عدد متزايد من اللغات. الكلمة يتكون من آيتين مختارين من الكتاب المقدس لكل يوم من أيام السنة.}

    bidi::fixBidi $T 1 60

    produces several lines of max. 60 letters.

* Hebrew Bible text example with vowel stripping:

    set T "םֶכְבַבְל־לָכְּב תֶמֱאֶּב ֹותֹא םֶּתְדַבֲעַו הָוהְי־תֶא ּוארְי"
    
    bidi::fixBidi $T 0
    
    produces: .יראו את יהוה ועבדתם אותו באמת בכל לבבכם
