
```
<!-- 텍스트 -->
<div>
	{% if 조건식 %}
		<textarea type="text" class="textClass" id="textId">{{조건}}</textarea>
	{% else %}
		<textarea type="text" class="textClass" id="textId" placeholder="내용을 작성해주세요..."></textarea>
	{% endif %}
</div>
<!-- //텍스트 -->
```

### HTML 주의

버튼은 똑같이 작성 후 <span style="color:red">display: none</span> 처리할 것

```
<!-- 버튼 -->
<div>
	{% if 조건식 %}
		<input type="button" class="btnClass grey" id="btnId1" value="value1" style="display: none;">
		<input type="button" class="btnClass grey" id="btnId2" value="value2">
	{% else %}
		<input type="button" class="btnClass grey" id="btnId1" value="value1">
		<input type="button" class="btnClass grey" id="btnId2" value="value2" style="display: none;">>
	{% endif %}
</div>
<!-- 버튼 -->
```

### JS 주의

﻿1. 전역 변수는 되도록 사용하지 말 것 (함수 안에다가 선언)<br>
﻿2. 필요한 곳에서만 불러서 사용할 것

```
<script>
// 내용 입력 시 버튼 활성화
let textId = document.querySelector('#textId');

let functionName = function (event) {
	let btnId1 = document.querySelector('#btnId1');
	let btnId2 = document.querySelector('#btnId2');

	if (textId.value == ""){
		btnId1.disabled = true;
		btnId1.setAttribute('class', 'button gray');
		qnaModify.disabled = true;
		qnaModify.setAttribute('class', 'button gray');
	}
	else {
		btnId1.disabled = false;
		btnId1.setAttribute('class', 'button black');
		btnId2.disabled = false;
		btnId2.setAttribute('class', 'button black');
	}
};

textId.addEventListener('change', functionName);
textId.addEventListener('paste', functionName);
textId.addEventListener('input', functionName);
textId.addEventListener('cut', functionName);
</script>
```