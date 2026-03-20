---
id: 11
title: "الفرق بين دالة call ودالة apply ودالة bind في الجافاسكربت"
date: "2018-10-19"
language: "ar"
tags:
  - javascript
  - call
  - apply
  - bind
original_url: "https://developingschool.com/a/11/%D8%A7%D9%84%D9%81%D8%B1%D9%82-%D8%A8%D9%8A%D9%86-%D8%AF%D8%A7%D9%84%D8%A9-call-%D9%88%D8%AF%D8%A7%D9%84%D8%A9-apply-%D9%88%D8%AF%D8%A7%D9%84%D8%A9-bind-%D9%81%D9%8A-%D8%A7%D9%84%D8%AC%D8%A7%D9%81%D8%A7%D8%B3%D9%83%D8%B1%D8%A8%D8%AA/"
source: "developingschool.com"
---

<div dir="rtl" style="text-align: right;" markdown="1">

# الفرق بين دالة call ودالة apply ودالة bind في الجافاسكربت

كل من الدالة call ودالة apply ودالة bind في الجافاسكربت متشابهون إلى حد كبير، والفروقات بينهم صغيرة، ولو تعرفت على أي منهم تستطيع بسهولة أن تتعرف على الدالتين الأخرتين، دعونا في البداية نبدأ بالدالة call، ما هي الدالة call وما هي وظيفتها؟

### Call Function

الدالة call هي دالة معرفة مسبقا في لغة الجافاسكربت، وهي دالة تتبع الكائن Function وبالتالي أي دالة تقوم بتعريفها تأخذ كل خصائص ووظائف الكائن Function، فكما تعرف أن الدوال في الجافاسكربت هي بالأساس الأول عبارة objects، وبالتالي أي دالة يمكن أن تستخدم معها الخاصية call. فماذا تفعل الدالة call هذه ؟؟ بكل بساطة بتقوم بعمل invocation للدوال في الجافاسكربت. دعونا نأخذ مثالا سريعا على هذا الكلام:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function printMyName(){
	console.log('My Name Is ....');
}

printMyName.call(); // "My Name Is ...."
```

</div>

بكل بساطة في الكود السابق قامت الدالة call بعمل تنفيذ "invocation" للدالة printMyName، لحد الآن لم تقم الدالة call بعمل أي شيء جديد، حيث يمكننا أن ننفذ الدوال بالطريقة التقليدية دون الحاجة إلى الدالة call. فمن هنا يبدأ حديثنا، فالدالة call لم تأتي لكي تقوم بعمل invocation للدول فقط، بل لها دور أكبر من هذا.

هناك مقولة مشهورة تقول "إذا وجدت مشكلة ابحث عن المرأة"، طبعا مع اختلافنا مع هذه المقولة، نريد فقط أن نوازن عليها مقولة أخرى ألا وهي "إذا وجدت دالة call أو apply أو bind ابحث عن this"، الدور الرئيسي الذي تلعبه دالة مثل دالة call هو تغيير سياق الدوال، وعادة ما نؤشر إلى أي سياق بواسطة كلمة this، دعونا نأخذ مثالا سريعا:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var User_Mark = {
	id: 5,
	name: 'Mark',
	printUserName: function(){
		console.log('The User Name Is ' + this.name);
	}
};
User_Mark.printUserName(); // 'The User Name Is Mark'
```

</div>

لو تحدثنا سريعا عن الكود السابق، سوف نجد الكلمة المفتاحية this تشير إلى سياق الكائن المُنفذ -أو مجازا الكائن المالك للدالة- وهنا كلمة this تشير إلى الكائن User\_Mark، في بعض الأحيان نريد أن نغير سياق الدوال، أي نجعل كلمة this تشير إلى كائنات أخرى غير الإشارة التلقائية، وهنا يأتي دور الدالة call أو apply أو bind، دعونا ننشئ كأئن أخر وننفذ الدالة User\_Mark.printUserName ولكن في سياق مختلف:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var User_Ahmed = {
	id: 9,
	name: 'Ahmed'
};
User_Mark.printUserName.call(User_Ahmed); // 'The User Name Is Ahmed'
```

</div>

في الكود السابق قمنا بعمل invocation للدالة printUserName بواسطة الدالة call، وكان من المتوقع أن تطبع اسم mark لكنها طبعت اسم ahmed والسبب في هذا يرجع إلى تغير السياق، وبما أن السياق قد تغير، فبالتالي كلمة this اصبحت تشير إلى كائن أخر ألا وهو الكائن User\_Ahmed، وهذه هي النقطة الجوهرية في دالة call واخواتها.

قبل التطرق إلى الـ syntax الخاص بدالة call، هناك سؤال؛ ماذا لو كانت الدوال التي نغير سياقها تحتوي على input parameters ؟ كيف نمررها ؟؟

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var User_Mark = {
	name: 'Mark',
	printUserName: function(upperCase){
		let name = this.name;
		if(upperCase === true){
			name = name.toUpperCase();
		}
		console.log('The User Name Is ' + name);
	}
};

var User_Ahmed = {
	name: 'Ahmed'
};

User_Mark.printUserName.call(User_Ahmed, true); // 'The User Name Is AHMED'
```

</div>

الدالة call تأخد عدة مدخلات "parameters" وأهمهم هو الـ parameter الأول، فالـ parameter الأول هو الكائن الذي ستشير إليه كلمة this في الدالة التي نعمل لها invocation بواسطة الدالة call، وباقي الـ parameters تمرر إلى الدالة، فلو نظرت إلى الكود السابق في السطر الأخير، ستجد أن الدالة call اخذت اثنين parameters الأول هو الكائن User\_Ahmed وهو يعد السياق الذي ستنفذ فيه الدالة printUserName أي هو الكائن الذي ستشير إليه كلمة this، والـ parameter الثاني سوف يتم تمريره إلى الدالة printUserName، وهنا أنت غير مرتبط بعدد معين من الـ parameters، فكل الـ parameters المررة بعد الـ parameter الأول يتم تمررها إلى الدالة التي نعمل لها invocation بواسطة الدالة call.

كل ما سبق بنطبق على الدالة apply والدالة bind مع بعض الاختلافات البسطة، لكن الفكرة واحدة لدي الدوال الثالثة، والفكرة هي تغيير سياق الدوال، أي تغيير المؤشر لدى كلمة this، والآن ما هي هذه الاختلافات، دعونا سريعا نلقى نظرة على الـ syntax الخاص الدالة call:-

**function.call(contextualObj, param1, param2, ...)**

فكما قلنا سابقا الدالة call تاخذ عدة parameters الأول هو الكائن الذي سوف تشير كلمة this إليه، باقي الـ paramters يتم تمريرها إلى الدالة.

### الاختلاف بين الدالة call وبين الدالة apply

الدالة apply تشبه تماما الدالة call، فكل ما قلناه سابقا ينطبق على الدالة apply، الاختلاف المحوري بين الدالتين يقع فيه الـ syntax، حيث أن الدالة apply تأخذ اثنين paramters فقط. الأول؛ هو الكائن الذي يعد السياق الجديد كما في الدالة call، والـ parameter الثاني؛ هو عبارة عن مصفوفة array بيتم تمرير عناصرها إلى الدالة التي نغير سياقها أثناء التنفيذ، انظر إلى الـ syntax الخاص بالدالة apply:-

**function.apply(contextualObj, paramsArray)**

فكما ترى المعامل الثاني للدالة apply عبارة عن مصفوفة بيتم تمرير عناصر إلى الدالة التي نعمل لها invocation. دعونا نأخذ مثالا سريعا على الدالة apply:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var context_A = {
	contextName: 'context_A',
	calc: function(a, b){
		let sumVal = a + b;
		console.log("output of " + this.contextName + " is " + sumVal);
	}
};
context_A.calc(2, 3); // output of context_A is 5

var context_B = {
	contextName: 'context_B'
};
// using apply 
context_A.calc.apply(context_B, [6, 4]); // output of context_B is 10
```

</div>

### الاختلاف بين الدالة call وبين الدالة bind

الدالة bind تشبه إلى حد كبير جدا الدالة call، لكن الاختلاف الجوهري يكمن في توقيت التنفيذ، فمع الدالة call يتم التنفيذ فوري، بمجرد ما تستدعي الدالة call يتم التنفيذ، على عكس الدالة bind، فالدالة bind تقوم بارجاع دالة جديدة "bounded function" يتم تنفيذها في وقت لاحق، انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var User_Mark = {
	name: 'Mark',
	printUserName: function(upperCase){
		let name = this.name;
		if(upperCase === true){
			name = name.toUpperCase();
		}
		console.log('The User Name Is ' + name);
	}
};

var User_Ahmed = {
	id: 9,
	name: 'Ahmed'
};
/*
* note)-> return a bounded function that 
* will be excuted later
*/
var bf = User_Mark.printUserName.bind(User_Ahmed, true);
// excute the bounded function
bf();
```

</div>

غالبا ما نستخدم دالة bind مع الـ callback function، حيث أننا ننشيء bounded function، لننفذها في وقت لاحق وفي سياق مختلف، والآن دعونا نلقي نظرة على الـ syntax الخاص بالدالة bind:-

**boundedFunc = function.bind(contextualObj, param1, param2, ...)**

في النهاية إذا وجدت كل من الدالة call أو الدالة apply أو الدالة bind، ابحث عن الكلمة this، أو بمعنى أخر إذا اردت أن تغير سياق أي دالة عليك أن تفكر في تلك الدوال الثلاثة، فالدوال الثلاث معنيون بالأساس الأول بتغيير السياق، والاختلاف بينهم يكمن في طبيعة وشكل الـ parameters المُمررة إليهم، أو في توقيت التنفيذ.

</div>
