# aArray
;AssocArray3.au3 - replaces AssocArray2.au3 Func AssocArrayCreate(ByRef $aArray, Const $nSize, Const $nGrowth = 30)     If IsNumber($nSize) = 0 Then ; Need a number here         Return False     ElseIf $nSize &lt; 1 Then ; Too small         Return False     EndIf     Local $Temp[2][$nSize]      $aArray = $Temp     $aArray[0][0] = 1;the next element to be filled     Local $itic = DllCall("Kernel32.dll", "int", "GetTickCount")     Local $Prefix = "Assoc1_" &amp; @YEAR &amp; @YDAY &amp; String($itic[0])  ;commented out the next lines which should not be there 15th Dec 2007 ;   Local $set = 1 ;   While IsDeclared($Prefix) ;       $set += 1 ;       $Prefix = "Assoc" &amp; $set &amp; "_" ;   WEnd     $aArray[1][0] = $Prefix     Return True EndFunc;==>AssocArrayCreate  Func AssocArrayAssign(ByRef $aArray, Const $sKey, Const $vValue) ;comment out next line untill I work out what's wrong with it       ;If Not IsDeclared($aArray[1][0]) Then _arrayReset($aArray)     Local $ConstVal = AssignNum($aArray, $sKey)     If $ConstVal >= UBound($aArray, 2) - 1 Then         ReDim $aArray[2][$ConstVal + 100]     EndIf     $aArray[1][$ConstVal] = $vValue     $aArray[0][$ConstVal] = $sKey     Return True EndFunc;==>AssocArrayAssign  Func AssocArrayGet(ByRef Const $aArray, Const $sKey) ;comment out next line untill I work out what's wrong with it       ;If Not IsDeclared($aArray[1][0]) Then _arrayReset($aArray)     If Not IsDeclared($aArray[1][0] &amp; $sKey) Then         SetError(1)         Return False     EndIf     Return $aArray[1][Eval($aArray[1][0] &amp; $sKey) ] EndFunc;==>AssocArrayGet    Func AssignNum(ByRef $aRay, $const)     Local $sConst = $aRay[1][0] &amp; String($const)     If Not IsDeclared($sConst) Then         Assign($sConst, $aRay[0][0], 2);global scope         $aRay[0][0] += 1     EndIf     Return (Eval($sConst)) EndFunc;==>AssignNum  ;If the array is saved then opened in a new script we will need this Func _arrayReset($aArray)     Local $n          For $n = 1 To $aArray[0][0] - 1         If $aArray[0][$n] &lt;> '' Then             If Not IsDeclared($aArray[1][0] &amp; $aArray[0][$n]) Then                 Assign($aArray[1][0] &amp; $aArray[0][$n], $n, 2)             EndIf                          EndIf     Next       EndFunc;==>_arrayInitialise
