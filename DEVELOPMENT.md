<div align="center">
<p>

[Back to Main Page](./README.md)
</p>

# Development

</div>

## Table of Contents

- [Development](#development)
  - [Table of Contents](#table-of-contents)
  - [Summary](#summary)
  - [Roadmap](#roadmap)
  - [Covered API](#covered-api)
  - [Known Issues](#known-issues)
  - [Raygui - Important Information](#raygui---important-information)

## Summary

**Raylib.nelua** follows original Raylib development cycle with additional features but nothing out of ordinary.

As Raylib updates roll out, **Raylib.nelua** will follow soon after. This binding does not shy away from implementing certain functions to make developers life easier, however. It will never implement new features that would otherwise not fit Raylib. 

## Roadmap

The current roadmap is:

Nothing at the moment. Important API have been covered.

## Covered API

- [x] raylib.h
- [x] raymath.h
- [x] easings.h
- [x] raygui.h
- [x] rlgl.h

## Known Issues

None as of yet.

## Raygui - Important Information

> **Notes**
>
> Due to a little tricky Raygui API and how it returns and requests ints and expect them to be also booleans for certain functions, I've added a helper function `rlgui.getStyleAsBool` (which returns boolean instead of integer which `rlgui.getStyle` does), `rlgui.boolToInt` and `rlgui.intToBool` (self-explanatory what it does).
On certain occasions, as seen in the example `scroll_panel`:

```lua
  -- scrollBarArrows is of type boolean which rlgui.getStyleAsBool returns, unlike its sister function rlgui.getStyle that returns a cint/int32
  local scrollBarArrows = rlgui.getStyleAsBool(rlgui.control.SCROLLBAR, rlgui.scrollBarProperty.ARROWS_VISIBLE)
  
  -- rlgui.checkBox's 3rd parameter requests a pointer to a boolean which is our scrollBarArrow. 
  -- This is where we're unable to simply use rlgui.getStyle() as it returns an int. My knowledge here is limited on how we could convert an integer to a pointer of boolean in Nelua.
  rlgui.checkBox(rl.rectangle{ 565, 280, 20, 20 }, "ARROWS_VISIBLE", &scrollBarArrows)
  
  -- However, now rlgui.setStyle's third parameter requests an integer value. This is easy to provide as we can use the helper function
  -- rlgui.boolToInt() that convers our boolean to an integer (not a pointer!)
  rlgui.setStyle(rlgui.control.SCROLLBAR, rlgui.scrollBarProperty.ARROWS_VISIBLE, rlgui.boolToInt(scrollBarArrows)) -- Pass scrollBarArrows as convered bool to integer.
```

> **Going forward**
>
> Until I manage to find an easier or more intuitive way to have `rlgui.getStyle` either return bool or int, this will have to be way forward.
> I recommend checking out other examples to understand how Raygui binding works in Nelua.


