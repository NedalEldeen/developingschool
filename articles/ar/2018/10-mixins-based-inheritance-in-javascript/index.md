---
id: 10
title: "الـ Mixins-Based Inheritance في الجافاسكربت"
date: "2018-09-25"
language: "ar"
tags:
  - javascript
  - mixins
  - inheritance
  - object-oriented-programming
  - oop
original_url: "https://developingschool.com/a/10/%D8%A7%D9%84%D9%80-mixins-based-inheritance-%D9%81%D9%8A-%D8%A7%D9%84%D8%AC%D8%A7%D9%81%D8%A7%D8%B3%D9%83%D8%B1%D8%A8%D8%AA/"
source: "developingschool.com"
---

<div dir="rtl" style="text-align: right;" markdown="1">

# الـ Mixins-Based Inheritance في الجافاسكربت

تحدثنا باستفاضة عن كل من الـ classical inheritance و الـ prototypal inheritance، وقلنا أن كل من النوعين يندرجان تحت عملية الـ delegation عن طريق استخدام سلسلة الـ prototype. فماذا عن استخدام الـ Mixins في الوراثة؟

![مخطط يوضح فصيل يرث من عدة فصائل في آن واحد](./images/javascript-inheritance-multiple-parents.png)

هناك بعض التحديات التي تجعلنا نفكر في استخدام منهج أخر في عملية الوراثة، أولى هذه التحديات هي؛ كيف يمكن لنا أن نجعل فصيل ما أو كائن ما يرث من أكثر من أب في آن واحد، فعلى سبيل المثال كيف يمكن لنا أن نجعل الفصيل ClassA يرث من الفصائل ClassZ .. ClassY .. ClassX في آن واحد وجميعهم في نفس المستوى، كما موضح في الصورة ؟!. بالطبع لا يمكن تحقيق هذه الفكرة بواسطة الـ classical inheritance أو الـ prototypal inheritance، لأن الـ prototype في نهاية المطاف يشير إلى كائن واحد فقط. إذا ما الحل؟

هنا يأتي دور الـ Mixins ، فما هي الـ Mixins ؟؟

بكل بساطة الـ Mixins هي عبارة عن فصائل لها وظائفها وسلوكها الخاص، وهذه الفصائل تُعطي وظائفها هذه وسلوكها إلى فصائل أخرى دون أن تصبح أب "parent class" لهذه الفصائل، وبالتالي عندما نتحدث عن الـ Mixins-Based inheritance نقول؛ هي عملية نقل أو استنساخ الوظائف من فصيل إلى أخر دون أن نبني علاقة أب وابن. دعونا نأخذ مثالا حتى تضح الصورة:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
/*
* some mixins having own properties and behaviour
*/
var Mixin__X = {
	hasMixin__X: function(){
		return true;
	}
};

var Mixin__Y = {
	hasMixin__Y: function(){
		return true;
	}
};
```

</div>

هذه صورة بدائية جدا لشكل الـ Mixins، وهي عبارة عن كائنات/فصائل لديها وظائفها التي نريد أن ننسخها إلى كائنات/فصائل أخرى. فعلي سبيل المثال، لدينا فصيل اسمه MainClass نريده أن يأخذ وظائف الـ Mixins الموضحة اعلاه. فكيف يكون ذلك؟ لتطبيق هذه الفكرة هناك عدة طرق أو أشكال مختلفة للتطبيق الفكرة، ولكل طريقة اعتباراتها الخاصة، دعونا نبدأ بالطريقة الأولى وهي عمل دالة من صنعنا نحن مسئولة عن عملية النقل والنسخ من وإلى الكائنات، كما في الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
/*
* you can name this function anything like ...
* Augment .. Copy .. Extend .. Concatenate .. whatever
*/
var Augment = function(RecievingClass, MixinClass){
	for(var k in  MixinClass){
		if(MixinClass.hasOwnProperty(k)){
			RecievingClass[k] = MixinClass[k];
		}
	}
};
```

</div>

فكما ترى الدالة Augment تقوم بنسخ الوظائف الموجودة من أحد الكائنات إلى كائن/فصيل أخر، لو اردنا أن نجعل الفصيل MainClass يكتسب وظائف الـ Mixins سنفعل هذا بكل بساطة كالآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var MainClass = function(){

};
// copy properties of Mixin__X into MainClass.prototype
Augment(MainClass.prototype, Mixin__X);
// also, copy properties of Mixin__Y into MainClass.prototype
Augment(MainClass.prototype, Mixin__Y);

// instance new object of MainClass
var justClass = new MainClass();
console.log(justClass.hasMixin__X()); //true
console.log(justClass.hasMixin__Y()); //true
```

</div>

لو نظرت إلى الكود السابق ستجد أن الفصيل MainClass اخذ الوظائف الموجودة في كل من الـ Mixin\_\_X والـ Mixin\_\_Y دون أن يصبحوا أباء للفصيل MainClass، وبهذا نكون قد حققنا فكرة الـ Mixin-Based Inheritance.

هناك شكل أخر لتنفيذ هذه الفكرة، وذلك عن طريق استخدام الدالة Object.assign، بدلا من الدالة Augment التي صنعناها نحن في المثال السابق. فالدالة Object.assign معرفة بشكل native في الجافاسكربت، وهي تعمل نفس عمل الدالة Augment، لكنها ظهرت في الاصدارات الحديثة من الـ ECMAScript. لكن في النهاية الفكرة واحدة وإن اختلفت أشكال التنفيذ، والآن لو اردنا أن نستخدم الدالة Object.assign بدلا من الدالة Augment في المثال السابق سيكون كالآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var MainClass = function(){

};
// using Object.assign instead of Augment
Object.assign(MainClass.prototype, Mixin__X, Mixin__Y);

// instance new object of MainClass
var justClass = new MainClass();
console.log(justClass.hasMixin__X()); //true
console.log(justClass.hasMixin__Y()); //true
```

</div>

إذا كنت لا تعرف عن دالة الـ Object.assign، يرجى الاطلاع عليها، حتى تكون ملم بما تفعله هذه الدالة بتفاصيل أكثر. إلى هذه النقطة لا يوجد أي صعوبة في تنفيذ الفكرة لكن هناك بعض النقاط التي لابد أن تأخذها في الحسبان عندما تتعامل مع الـ Mixins.

### Override

بما أن الفصيل أو الكائن يكتسب الوظائف من أكثر من Mixin في آن واحد، فمن الوارد جدا أن تتشابه اسامي الدوال في هذه الـ Mixins، وبالتالي سيحدث override على هذه الدوال، وستكون الغلبة لأخر Mixin يتم استنساخ الدوال منه. "Last one wins".

وبالتالي إذا اردت أن تستنسخ الدوال الموجودة في الـ Mixins والتي تحمل نفس الاسم، فإما أن "تفلتر" أي تختار الدوال التي تحتاج نسخها، أو تضيف الـ Mixins بترتيب ما يخدم الحالة التي أنت بصددها.

### Source Object Mutatuion

من النقاط الأخرى التي يجب أن تخذها في الاعتبار وأنت تتعامل مع الـ Mixins-Based inheritance أن الكائن/الفصيل الذي تريد أن تنسخ إليه الوظائف والسلوك من الـ Mixins سوف يتغير نفسه، أي سيكون في حالة "state" جديدة، وهذا ربما يؤثر على باقية الكود الذي يعتمد على هذا الكائن/الفصيل، وهذه النقطة ربما تكون نقطة حرجة جدا إن كنت تعمل على مشروع كبير ضمن فريق عمل. ومن هنا إما أن يكون الفريق على دراية بأي تغيير يحدث للـ "state" الخاصة بالكائن/الفصيل، أو ربما تستخدم الـ functional mixins أو الـ factory pattern عند اضافة وظائف الـ Mixins الي الكائن/الفصيل. كما في الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
// ...
var AssignMixin = function(RecievingClass, Mixin){
    // you may set a private fields here, if you want
	var newState = Object.assign({}, RecievingClass, Mixin);
	return newState;
};

MainClass.prototype = AssignMixin(MainClass.prototype, Mixin__X);

var justClass = new MainClass();
console.log(justClass.hasMixin__X()); //true
```

</div>

في الكود السابق قمنا بإنشاء كائن فارغ وقمنا بنسخ الدوال إليه، وبهذا لا نؤثر على الـ source object كما في المرات السابقة. هناك نقطة مهمة في الكود السابق ممكن أن نلفت إليها الانتباه الآن إلى أن نتحدث عنها بشكل مفصل أكثر فيما بعد ألا وهي أن الطريقة المستخدمة في الكود السابق تدعم لنا الـ "privacy fields" وهذه النقطة تحتاج إلى موضوع مستقل، اردنا فقط أن نلفت الانتباه إليها.

في النهاية، يبقى السؤال الكبير، أي الطرق التي يجب أن اسلكها في عملية الوراثة ؟؟ بالتاكيد لا توجد اجابة واحدة لكل الحالات، فكل حالة لها اعتباراتها الخاصة، وكذلك كل طريقة في عملية الوراثة لها مميزاتها وعيوبها، وبالتالي كلما فهمت بشكل دقيق الوراثة وطرقها المختلفة، واستطعت أن تحلل وتفنت الحالة التي أمامك، وقتها تستطيع أن تصل إلى القرار المناسب.

</div>
