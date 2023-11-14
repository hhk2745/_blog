# Home End 키 변환
"command + ->"

설정
```shell
cd ~/Library
mkdir KeyBindings
cd KeyBindings

vi DefaultKeyBinding.dict

{
    /* Remap Home / End keys to be correct */
    "\UF729" = "moveToBeginningOfLine:"; /* Home */
    "\UF72B" = "moveToEndOfLine:"; /* End */
    "$\UF729" = "moveToBeginningOfLineAndModifySelection:"; /* Shift + Home */
    "$\UF72B" = "moveToEndOfLineAndModifySelection:"; /* Shift + End */
    "^\UF729" = "moveToBeginningOfDocument:"; /* Ctrl + Home */
    "^\UF72B" = "moveToEndOfDocument:"; /* Ctrl + End */
    "$^\UF729" = "moveToBeginningOfDocumentAndModifySelection:"; /* Shift + Ctrl + Home */
    "$^\UF72B" = "moveToEndOfDocumentAndModifySelection:"; /* Shift + Ctrl + End */
}

# 저장 후 재부팅
```