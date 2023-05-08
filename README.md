# RotationMon
Monitor for screen orientation changes

Here's a quick little utility that monitors for screen resolution changes, handy if you want to change the layout on tablets. Tested on my Surface tablet.

This utility currently doesn't distinguish between flipped landscape or portrait; it checks the orientation angle for whether it's 0/180 or 90/270. 

The core functions:

```
Private Function IsPortraitMode() As Boolean
    Dim dm As DEVMODEW_FORDISP
    dm.dmSize = LenB(dm)
    Dim hr As Long = EnumDisplaySettingsW(0, ENUM_CURRENT_SETTINGS, dm)
    List1.AddItem "EnumMode return=" & hr
    Return (dm.dmDisplayOrientation <> DMDO_0) And (dm.dmDisplayOrientation <> DMDO_180)
End Function
Private Function FormWndProc(ByVal hWnd As LongPtr, ByVal uMsg As Long, ByVal wParam As LongPtr, ByVal lParam As LongPtr, ByVal uIdSubclass As LongPtr, ByVal dwRefData As LongPtr) As LongPtr

Select Case uMsg
    Case WM_SETTINGCHANGE, WM_DISPLAYCHANGE
        Dim bPortrait As Boolean = IsPortraitMode()
        If bPortrait <> bOldIsPortrait Then
            List1.AddItem Format$(Now, "mm-dd Hh:nn:Ss") & ": RotationChange(Portrait=" & bPortrait & ")"
            bOldIsPortrait = bPortrait
            
            '************************************
            'ORIENTATION HAS CHANGED!
        End If
    Case WM_DESTROY
        Call UnSubclass2(hWnd, AddressOf FormWndProc, uIdSubclass)
End Select
FormWndProc = DefSubclassProc(hWnd, uMsg, wParam, lParam)
End Function
```

No dependencies, and no particular requirements, should build on any remotely recent tB version.
