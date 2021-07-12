# IDE struggles

## VScode

### Change shortcut keybindings
* 다음 순서로 해결
> 1. when expression 수정
> 2. (1이 안될 시) 동일한 key binding을 가진 다른 command 확인
> 3. 최근에 설치한 extension 확인

1-2.
* shortcut 적용이 안 된다면 'When expression'을 수정해야 함
* 예를들어, python interactive을 사용할 때 단축키는 jupyter에서만 사용 가능한 조건이 있을 수 있음
* example: jupyter:run selection/line in interactive window는 아래와같이 설정되어있음
```json
{
  "key": "shift+enter",
  "command": "jupyter.execSelectionInteractive",
  "when": "editorTextFocus && jupyter.ownsSelection && !findInputFocussed && !notebookEditorFocused && !replaceInputFocussed && editorLangId == 'python'"
}
```
* 여기서 jupyter.ownsSelection은 jupyter에서 실행되어야 하는 제한조건을 걸어준다
* 해당 부분 삭제
* 참고: [link](https://github.com/microsoft/vscode-jupyter/issues/3993)

3.
* extension 설치 시 keybinding이 중복될 수 있음. 잘 쓰던 단축키가 안되면 최근 설치한 extension 삭제 후 시도

### Frequently used shortcuts
| Command | Windows|
|:---:|:----:|
|python interactive에서 editor로 전환|`ctrl+1`|
|editor에서 python interactive로 전환|`ctrl+2`|
|화면 아래로 한줄 이동|`ctrl+y`|
|화면 위로 한줄 이동|`ctrl+e`|

### VScode에서 jupyter notebook kernel 연결 실패
1. Anaconda 설치 경로에 한글이 포함된 경우 => 경로에 한글 미포함 후 재설치
2. ipykernel을 설치하라는 error가 반복적으로 나타나는 경우
```
pip uninstall pyzmq
pip install pyzmq
```
ref: [stack overflow](https://stackoverflow.com/questions/67818911/failed-to-change-the-jupyter-kernel-in-vs-code/67833255#67833255)

* 원인은 따로 설명되지 않았으나, base env에만 나타나는 문제라고 한다
* 해결 후 VScode 재부팅 필요