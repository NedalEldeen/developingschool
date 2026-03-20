---
id: 12
title: "الطرق الشائعة المستخدمة لإنشاء الكائنات في الجافاسكربت"
date: "2018-10-19"
language: "ar"
tags:
  - javascript
  - object
  - object-oriented-programming
  - oop
original_url: "https://developingschool.com/a/12/%D8%A7%D9%84%D8%B7%D8%B1%D9%82-%D8%A7%D9%84%D8%B4%D8%A7%D8%A6%D8%B9%D8%A9-%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D8%AE%D8%AF%D9%85%D8%A9-%D9%84%D8%A5%D9%86%D8%B4%D8%A7%D8%A1-%D8%A7%D9%84%D9%83%D8%A7%D8%A6%D9%86%D8%A7%D8%AA-%D9%81%D9%8A-%D8%A7%D9%84%D8%AC%D8%A7%D9%81%D8%A7%D8%B3%D9%83%D8%B1%D8%A8%D8%AA/"
source: "developingschool.com"
---

<div dir="rtl" style="text-align: right;" markdown="1">

# الطرق الشائعة المستخدمة لإنشاء الكائنات في الجافاسكربت

عندما نتحدث عن الكائن الـ object في الجافاسكربت، فنحن نتحدث عن ركن ركيز من أركان الجافاسكربت، فنحن نعرف أن معظم الأشياء في الجافاسكربت هي بالأساس الأول عبارة عن object، فالدوال والمصفوفات والتواريخ ومعظم الأشياء في الجافاسكربت تعتبر وليدة للـ object، ولذلك أقول أن الـ object يعد ركن ركيز من أركان الجافاسكربت.

في هذا الموضوع سوف نتحدث بشكل عام عن بعض الطرق الشائعة للتعامل مع الكائنات في الجافاسكربت من حيث الإنشاء واضافة واستجلاب ومسح البيانات منها. مجرد دردشة عامة ليس أكثر.

### Create object literally

أولا دعونا ننشئ كائن بأبسط طريقة موجودة في الجافاسكربت:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var User = {
    first_name: 'Jack',
    last_name: 'Wilson',
    getFullName: function(){
        return this.first_name + ' ' + this.last_name;
    }
};
```

</div>

هذه الطريقة أو هذا النمط في إنشاء الكائنات يطلق عليه اسم literal أي إنشاء كائن بالمعنى الحرفي للكلمة، أي كائن في الأساس له اسم ويتكون من عدة خصائص ووظائف، وهذا ما قمنا به في الكود السابق.

### Object.create

يمكننا أيضا إنشاء كائن جديد باستخدام الدالة create التابعة للكائن Object، لاحظ معي ان هناك دالة إنشاء اسمها Object -أول حرف Capital-، ودالة الإنشاء هذه لها دالة تتبعها اسمها create، وهذه الدالة مُعَرفة بشكل native في الاصدارات الجديدة من ECMAScript، اي نستطيع أن نستخدمها في أي مكان في الأكواد التي نكتبها. وهذه الدالة سوف نستخدمها لإنشاء كائن جديد، دعونا نلقي نظرة سريعة على الـsyntax الخاص بهذه الدالة:-

**Object.create(proto \[, propertiesObject\])**

هذه الدالة تأخذ معاملان، المعامل الأول هو الـ prototype الذي سوف يتم إلحاقه بالكائن الجديد الذي سوف ننشئه باستخدام الدالة create، والمعامل الثاني هو الخصائص التي نريد أن نُعَرفها داخل الكائن وهذا المعامل اختياري. والعائد من هذه الدالة هو بالفعل الكائن الجديد الذي نحن بصدد إنشائه.

انظر معى إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var newObject = Object.create(null);
console.dir(newObject);
```

</div>

لو أنك قمت بتجربة هذا الكود ونظرت إلى الـ console سوف تجد أنه طبع object ولكنه فارغ تماما، حتى أنه لا يحمل في طياته أي prototype، والسبب في ذلك أننا جعلنا المعامل الأول يساوي null، وفي نظري أي كائن في الجافاسكربت لا يحمل prototype هو بمثابة اليتيم المقطوع من شجرة لا أهل له، وبالتالي صعب أن تجد كائن في الجافاسكربت بدون prototype، ومن هنا نسأل أنفسنا أي prototype سوف نرفقه للكائن الجديد ؟؟ مبدئيا أنت تستطيع أن ترفق أي كائن بدلا من الـ null التي ارفقناها في المثال السابق. فعلى سبيل المثال، انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var dummyPrototype = {
	dummyKey: 'dummy value'
};

var newObject = Object.create(dummyPrototype);
console.dir(newObject);
```

</div>

في هذا المثال قمنا بإضافة prototype من صنعنا نحن لهذا الكائن الجديد، وبما أننا استطعنا أن نضيف prototype للكائن الجديد، هذا معناه أننا نستطيع أن نضيف أي prototype أخر من أي كائن ما كان، وبالتالي لو دققنا النظر قليلا سنجد أن هذه الجزئية سوف تساعدنا كثيرا في عملية الوراثة، بمعنى أننا من الممكن أن نضيف prototype خاص بفصيل ما إلى الكائن الجديد. طبعا هذه الجزئية يطول فيها الحديث وقد تحدثنا بالفعل هنا في أكثر من مقالة، يمكنك الاطلاع على هذه المقالات:-

\- الوراثة في الجافاسكربت

\- الـ prototypal inheritance في الجافاسكربت

\- دردشة حول الـ prototype والـ \_\_proto\_\_ في الجافاسكربت

من الممكن أن يسأل أحدنا ويقول اريد فقط أن أنشئ كائن جديد دون الدخول في تفاصيل الـ prototype مجرد كائن كمان فعلنا في المثال الخاص بالـ User في بداية هذه المقالة، ولكن باستخدام الدالة Object.create، كيف لي أن أفعل هذا ؟؟ بكل بساطة سوف نقوم بإنشاء كائن جديد بواسطة الدالة Object.create وسوف نمرر لها الـ prototype الخاص بالـ Master Object فهي بمثابة الـ default prototype انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var newObject = Object.create(Object.prototype);// newObject = {}; <- the same
console.dir(newObject);
```

</div>

في هذا المثال قمنا بإنشاء كائن جديد له الـ prototype التابع لـ Master Object وحصلنا على نفس الناتج الذي كنا سوف نحصل عليه لو أنشائنا الكائن بالطريقة الحرفية literally، الفارق هنا أن الدالة Object.create ربما تسهل علينا الأمور نوعا ما في عملية الوراثة. قبل أن ننهي هذه الجزئية نريد أن ننشيء الكائن User الذي قمنا بإنشائه بالطريقة literal في أول حديثنا ولكن هذه المرة باستخدام الدالة Object.create، فكما قلنا أن هذه الدالة تأخذ معاملان؛ المعامل الأول هو الخاص بالـ prototype، أما المعامل الثاني هو الخاص بـ خصائص الكائن ووظائفه؛ أي هو المسؤول عن إرفاق الـ properties إلى الكائن الجديد، وبالتالي يمكننا أن ننشيء الكائن User بواسطة الدالة Object.create كالآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var User = Object.create(Object.prototype, {
	'first_name': {
		value: 'Jack',
		writable: true,
		configurable: true,
		enumerable: true
	},
	'last_name': {
		value: 'Wilson',
		writable: true,
		configurable: true,
		enumerable: true
	},
	'getFullName': {
		value: function(){
			return this.first_name + ' ' + this.last_name;
		},
		writable: true,
		configurable: true,
		enumerable: true
	}
});
```

</div>

طبعا لو قمت بطباعة كل من الكائنين User اللذين قمنا بإنشاءهما مرة literally ومرة من خلال الدالة Object.create في الـ console لن تجد فارقا بين الاثنين، والآن ننتقل إلى طريقة أخرى لإنشاء الكائنات في الجافاسكربت.

### new Object

كما قلنا في الفقرة السابقة أن هناك دالة إنشاء مُعرفة في الـ global scope وهي الدالة Object -الحرف الأول capital- وباستخدام المعامل new يمكننا عملinstanciate لكائن جديد، انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var newObject = new Object();
console.log(newObject);
```

</div>

هذا الكائن يحمل الـ prototype تلقائيا وهو الـ prototype الخاص بالـ Master Object، وبما أننا لم نمرر أي شيء إلى الـ constuctor فكان الناتج هو كائن فارغ أي empty object، ولكي نضيف إليه خصائص يمكن فعل هذا بكل بساطة كالآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var user = new Object({
    first_name: 'Jack',
    last_name: 'Wilson',
    getFullName: function(){
        return this.first_name + ' ' + this.last_name;
    }
});
```

</div>

**وتلخيصا لما سبق**، يمكننا أن نقول أن هناك ثلاث طرق -ضمن طرق كثيرة- متعارف عليها لكي نبدأ إنشاء كائن جديد في الجافاسكرب:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
// create object literally
var obj = {};
// create object by Object.create();
var obj = Object.create(Object.prototype);
// instanciate new object by Object constructor
var obj = new Object();
```

</div>

بالطبع يوجد طرق ودوال أخرى تساعدنا بشكل أو بأخر في إنشاء الكائنات في الجافاسكربت، وأنت مع الوقت سوف تتعرف على المزيد من الطرق والدوال التي تتيح لنا إنشاء الكائنات، وقبل أن ننهي هذه الجزئية دعونا نأخذ مثالا على إنشاء الكائنات عن طريق الـ constructor pattern نمط دالة الإنشاء:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function Cars(model, color){
    this.model = model;
    this.color = color;
}
// instantiate a new Car object
var BMW = new Car('BMW_2010', 'red');
```

</div>

في المثال السابق استخدمنا نمط دالة الإنشاء، وهذا الأسلوب يعد من أحدى الطرق المستخدمة لإنشاء الكائنات في الجافاسكربت، وكما قلنا هناك الكثير من الطرق والأساليب التي تتيح لنا إنشاء الكائنات، وما قمنا به هو مجرد مرور سريع على بعض الطرق التي قد تكون اعطتنا فكرة عامة حول إنشاء الكائنات في الجافاسكربت، والآن دعونا ننتقل إلى كيفية اضافة واستجلاب البيانات من الكائنات.

## Key - Value Assignment

والآن بعد أن قمنا بإنشاء كائنات في الجافاسكربت بأكثر من طريقة، يأتي الآن السؤال عن كيفية التعامل مع الخصائص والوظائف الخاصة بالكائنات من حيث الإضافة والاستجلاب والمسح.

### Dot Syntax

لكي نضيف خاصية جديدة إلى أي كائن، او استجلاب هذه الخاصية، يمكن فعل هذا بأبسط طريقة ألا وهي الـ Dot Syntax، انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var newObject = {};
// set property 
newObject.whateverKey = "some values";
// get property
var theValue = newObject.whateverKey;
```

</div>

هذه الطريقة تعمل بشكل جيد ولا يوجد بها أي مشكلة، لكن في بعض الأحيان تكون قيمة أو اسم الـ key مخزن في متغير ما، ومن هنا لا تستطيع استخدام الـ Dot Syntax في الاضافة أو الاستجلاب، وهذه النقطة يمكن تجاوزها باستخدام الـ Square Bracket Syntax.

### Square Bracket Syntax

الـ square bracket syntax تعد أيضا من ابسط الطرق لإضافة أو استجلاب الخصائص من الكائن، انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var newObject = {};
// set property 
newObject['whateverKey'] = "some values";
// get property
var theValue = newObject['whateverKey'];
//--------------------//
// use variables as keys
var dynamicKey = 'user_number_10';
newObject[dynamicKey] = "another some values";
```

</div>

## Delete Object Property

بعدما قمنا بالتعرف على الطرق الشائعة لإنشاء الكائنات في الجافاسكربت، وتعرفنا أيضا على كيفية اضافة واستجلاب الخصائص من الكائنات، يأتي الآن الدور على الحديث عن كيفية مسح الخصائص من أي كائن. عملية المسح هذه عملية بسيطة جدا ويتم تنفيذها بواسطة المعامل delete كما في الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var obj = {
    x_key: 'x_value'
};
// approch a
delete obj[x_key]; 
// approch b
delete obj["x_key"];
// approch c
delete obj.x_key;
```

</div>

طبعا عملية مسح الخصائص من الكائنات لها بعض الاعتبارات، وربما في موضوع أخر نتحدث بشكل مفصل على مسح الخصائص من الكائنات. وكانت هذه مجرد دردشة عامة حول بعض الطرق الشائعة في انشاء الكائنات في الجافاسكربت.

</div>
