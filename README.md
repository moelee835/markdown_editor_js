# markdown_editor_js
마크다운 유틸리티 다운로드가 차단된 환경에서 사용 가능한 브라우저 기반 마크다운 에디터

### 추가된 기능.
1. 파일 저장 버튼
2. 파일 불러오기 버튼.
3. 미리보기 기능.
4. Ctrl + s로 파일 저장하는 기능.


### Bug fix
[Prevent Default에 의한 특수문자 입력 막힘 : 2025-11-13 08:55]

기존 코드.
```html
  function saveWithKey(event) {
    // 키 저장 기능 활성화.
     if(event.ctrlKey && (event.key === 's' || event.key ==='S')) {
     	  if(filename == null) filename = `markdown-${new Date().toISOString().slice(0,10)}.md`;
 		    fallbackDownload(editor.value, filename);
     }
     event.stopPropagation();
     event.preventDefault();
  }  
```

##### 문제점
1. Ctrl + S가 아닌 경우에도 preventDefault() 가 실행되어 Enter, Backspace 등이 막히는 문제가 발생하였음.
2. preventDefault() 를 안쓰는 경우 브라우저 기본 저장 기능이 활성화되어 파일 탐색기가 오픈되어버림.


##### 해결
stopPropagation과 preventDefault를 조건문 안으로 옮겼음.
컨트롤 + s일 때만 기본 동작을 막도록 수정함.

```html
function saveWithKey(event) {
  // 키 저장 기능 활성화.
  if(event.ctrlKey && (event.key === 's' || event.key ==='S')) {
    if(filename == null) filename = `markdown-${new Date().toISOString().slice(0,10)}.md`;
    fallbackDownload(editor.value, filename);
    event.stopPropagation();
    event.preventDefault();
  }
}
```
