---
id: 5
title: "نمط دالة الإنشاء - الـ Constructor Pattern في الجافاسكربت"
date: "2018-05-02"
language: "ar"
tags:
  - javascript
  - constructor-pattern
  - design-patterns
  - object-oriented-programming
  - oop
original_url: "https://developingschool.com/a/5/%D9%86%D9%85%D8%B7-%D8%AF%D8%A7%D9%84%D8%A9-%D8%A7%D9%84%D8%A5%D9%86%D8%B4%D8%A7%D8%A1-%D8%A7%D9%84%D9%80-constructor-pattern-%D9%81%D9%8A-%D8%A7%D9%84%D8%AC%D8%A7%D9%81%D8%A7%D8%B3%D9%83%D8%B1%D8%A8%D8%AA/"
source: "developingschool.com"
---

<div dir="rtl" style="text-align: right;" markdown="1">

# نمط دالة الإنشاء - الـ Constructor Pattern في الجافاسكربت

تحدثنا في موضوع سابق عن [البرمجة الكائنية التوجيه الـ "oop" في الجافاسكربت](../04-object-oriented-programming-in-javascript/)، وتحدثنا عن المفهوم وأهميته بشكل مفصل، وتناولنا موضوع الـ classes ودورها في الـ object oriented programming، والآن يأتي السؤال الأهم؛ كيف نطبق ونستفيد من هذا المفهوم في الجافاسكربت ؟ أولى الخطوات لكي نحقق هذا المفهوم ونطبقه؛ هي إنشاء فصائل "classes" ومن ثم عمل "instantiation" لكائنات من هذه الفصائل. لكن هناك مشكلة سوف تواجهنا عندما نشرع في إنشاء الفصائل في الجافاسكربت، المشكلة هي أن الجافاسكربت -خاصة الاصدارات الأولى- لا تدعم الكلمة المفتاحية "class" التي تتيح لنا إنشاء الفصائل. فلو نظرت إلى بعض اللغات الأخرى مثل الجافا أو السي شارب أو السي بلس بلس أو البي اتش بي ستجد أن السيناريو المتبع لكي تنشيء أي فصيل هو سيناريو شبيه بالآتي؛ تكتب الكلمة المفتاحية "class"، ثم اسم الفصيل، ثم تفتح الأقواس المعقوفة "{}"، وبداخلها تكتب خصائص ووظائف الفصيل. وبهذا تكون قد أنشأت فصيل في مثل هذه اللغات. أما في الجافاسكربت، السيناريو المُتبع يختلف نوعا ما لعدم وجود الكلمة المفتاحية "class"، أو لو تحدثنا بشكل أدق سنقول؛ أن البنية التحتية التي بُنيت عليها لغة الجافاسكربت تختلف كثيرا عن اللغات الأخرى، ومن هنا يأتي دور نمط دالة الإنشاء الـ "Constructor Pattern" لكي يحل لنا هذه المشكلة.

الـ constructor pattern هو عبارة عن نمط إنشائي، يساعدنا في محاكاة عمل الـ classes ومنها عمل instantiate لكائنات جديدة. واسم هذا النمط مستخلص من فكرة دالة الـ contructor الموجودة في أي class. حيث أن الـ classes في معظم حالاتها لديها دالة تسمى الـ constructor وهذه الدالة لها دورين رئيسيين، ألا وهما:-

- بدء عملية الإنشاء للكائنات الجديدة.
- تهيئة هذه الكائنات للاستخدام مع تمرير أي معاملات "arguments" لهذه الكائنات.

ظهرت في الاصدارات الجديدة من الجافاسكربت الكلمة المفتاحية "class" التي تتيح لنا إنشاء فصائل جديدة بشكل native، لكن في الوقت نفسه هذا لا يغنينا عن معرفة نمط دالة الإنشاء، وهذه الجزئية سوف نناقشها في نهاية الموضوع.

تدور فكرة هذا النمط حول استخدام الدالة في الجافاسكربت بطريقة مختلفة نوعا ما عن الطريقة التقليدية، فمعظمنا يعرف أن معظم الأشياء في الجافاسكربت هي عبارة عن كائنات، حتى الدالة تعتبر في حد ذاتها كائن، ومن هنا يمكن النظر إليها من جهتين مختلفتين، انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
// declare a function
function foo(){
    console.log("it's just a function");    
}
// invoke the function
foo();
```

</div>

هذه هي النظرة الكلاسيكية للدوال، نقوم بالإعلان عن دالة، ثم نقوم باستدعاءها وتنفيذها، لكن عندما نتحدث عن الـ constructor في الجافاسكربت، يكون هناك نظرة أخرى للدالة، وهي التعامل معها على كونها constructor لفصيل ما، وبمساعدة الكلمة المفتاحية new نستطيع عمل "instantiate" لكائن جديد من هذا الفصيل. انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
// declare a function as constructor
function Employee(name){
    this.name = name;
}
// instantiate an object from previous constructor
var ali = new Employee("ali");
var eslam = new Employee("eslam");
```

</div>

في هذا الكود قمنا بالإعلان عن دالة الـ Employee، وهذه الدالة سوف نتعامل معها على أنها دالة إنشاء "constructor" لفصيل الموظفيين، ويمكننا من خلال الكلمة المفتاحية new عمل "instantiate" لكائنات ذات قيم مختلفة من فصيل الموظفيين. وهذه تعد النظرة الثانية للدوال في الجافاسكربت.

ومن هنا يمكن أن نلخص ما قالناه في الأعلى؛ أن الدالة في الجافاسكربت، إما أن نتعامل معها كـ دالة عادية؛ نعلن عنها ونستدعيها لنقوم بتنفيذها، أو نتعامل معها على أنها دالة إنشاء "constructor" وهذا هو موضوع حديثنا اليوم.

قبل الحديث عن الـ constructor pattern دعونا ننشئ كائن ما من فئة معينة، ولتكن فئة الموظفيين مثلا، معظمنا يعرف أن الأقواس المعقوفة الـ "curly braces syntax" {...} تتيح لنا إنشاء كائنات في الجافاسكربت، انظر معي إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var employee_mohamedAli = {
	id : 21,
	firstName : "Mohamed",
	lastName : "Ali",
	getFullName : function(){
		return this.firstName + " " + this.lastName;
	},
	salary : 100,
	giveCommission: function(amount){
		this.salary += amount;
	}
};
```

</div>

كما ترى هذا كائن من فصيل الموظفين، والنمط "pattern" المستخدم هنا يقال عليه "literal" ويعني النمط الحرفي لشكل الكائن، الآن لو أردنا أن ننشيء أكثر من كائن من فصيل الموظفين لهم نفس الخصائص والوظائف لكن بقيم مختلفة، ماذا سنفعل وقتها !؟ يمكن بكل بساطة أن ننشيء كل كائن على حدة بالنمط الحرفي "literal pattern" كالآتي:-.

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var employee_MohamedAli = {
	id : 21,
	firstName : "Mohamed",
	lastName : "Ali",
	getFullName : function(){
		return this.firstName + " " + this.lastName;
	},
	salary : 100,
	giveCommission: function(amount){
		this.salary += amount;
	}
};

var employee_OmarHamdy = {
	id : 22,
	firstName : "Omar",
	lastName : "Hamdy",
	getFullName : function(){
		return this.firstName + " " + this.lastName;
	},
	salary : 110,
	giveCommission: function(amount){
		this.salary += amount;
	}
};

console.log(employee_MohamedAli.fullName()); // Mohamed Ali
console.log(employee_OmarHamdy.fullName()); // Omar Hamdy
```

</div>

إذا قمت بتجربة هذا الكود سوف يعمل ولن تظهر أي أخطاء، لكن ليست هذه الطريقة المثلى ولا النمط الأمثل في هذه المشكلة، فلو قلت لك نريد أن ننشيء مئة كائن من فصيل الموظفين، ماذا ستفعل وقتها ؟! هل ستكرر السطور السابقة مئة مرة لكل موظف ؟؟ فرضنا أنك فعلت هذا، إليك سؤال أخر ماذا لو أردت أن تغير أو تضيف أو تمسح شيء من خصائص أو وظائف الفصيل هل ستكرر هذا التعديل مئة مرة ؟؟ بالطبع لا. وبالتالي ما هو البديل ؟؟ هنا يأتى دور الـ constructor pattern نمط دالة الإنشاء. انظر معي إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function Employee(id, firstName, lastName, salary){
    this.id = id;
    this.firstName = firstName;
    this.lastName = lastName;
    this.salary = salary;

    this.getFullName = function(){
        return this.firstName + " " + this.lastName;
    };

    this.giveCommission = function(amount){
		this.salary += amount;
	};
}

var mohamedAli = new Employee(21, "Mohamed", "Ali", 100);
var omarHamdy = new Employee(22, "Omar", "Hamdy", 110);

console.log(mohamedAli.fullName()); // Mohamed Ali
console.log(omarHamdy.fullName()); // Omar Hamdy
```

</div>

![مخطط يوضح العلاقة بين الـ class والـ objects](./images/oop-inheritance-class-objects.png)

لاحظ معي أننا قمنا بإنشاء فصيل واحد ألا هو فصيل الموظفين، ثم قمنا بإنشاء كائنات من هذا الفصيل، وبهذا نكون قد فعلنا محاكاة لفكرة الـ classes و الـ objects، ومن هنا يمكن القول بأننا قد وضعنا أرجلنا على أولى خطوات البرمجة على منوال الـ object oriented ، والآن لو نظرنا إلى هذا النمط في الكود السابق ستجد أنه:-

- أقصر في كتابته من النمط الحرفي "literal pattern".
- جيد في قراءة الكود.
- يمكن اعادة استخدامه بسهولة.
- سهولة التعديل عليه، وهذا التعديل يحدث أيضا في جميع الكائنات المستنسخة من الفصيل الرئيسي.
- يجعلنا نكتب أكواد الجافاسكربت على منوال Object Oriented Programming.

من النقاط الأخرى التي يجب أن تلاحظها وأنت تتعامل مع الـ constructor pattern.

- يفضل أن تبدأ دالة الإنشاء "constructor" بحرف "capital"، وهذا يساعدنا بعض الشيء في التفريق بينها وبين الدالة العادية.
- يتم تنفيذ دالة البناء بواسطة المعامل new. أي لكي تقوم بعمل instantiate لكائن جديد من فصيل ما نستخدم الكلمة المفتاحية new.

دعونا نسرد بعض الأسئلة التي قد تأتي إلى ذهن أي منا ألا وهي؛ ماذا تعني كلمة this التي نتسخدمها داخل دالة الإنشاء ؟ وما أهمية الكلمة المفتاحية new ؟ وهل من الممكن أن أقوم بعمل instantiate لكائن دون استخدام الكلمة المفتاحية new ؟ وكذلك ماذا يحدث خلف الكواليس عندما أقوم باستخدام الكلمة المفتاحية new مع دالة الإنشاء ؟ دعونا نجاوب على هذه الأسئلة سؤال سؤال.

### ماذا تعني كلمة this التي نستخدمها داخل دالة الإنشاء ؟

مبدئيا يمكننا استخدام الكلمة المفتاحية this في أكثر من مكان هي ليست معنية بدالة الإنشاء فقط، بل يمكن استخدامها في الدالة العادية وكذلك في الكائنات وفي أكثر من سياق، ولكل سياق معنى مختلف عن الأخر، وأنا أريدك أن تنظر إلى الكلمة المفتاحية this على أنها مؤشر، وأي مؤشر هو في طبيعة الحال يؤشر إلى شيء ما، ومن هنا نسأل؛ إلى أي شيء تؤشر الكلمة المفتاحية this داخل دالة الإنشاء ؟ قبل أن نجيب على هذا السؤال دعونا نمر سريعا عن استخدام كلمة this في الدالة العادية، أنظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function foo(){
	console.log(this);
}
foo(); // window object
```

</div>

في الكود السابق عندما قمنا بطباعة كلمة this وجدناها تشير الي الكائن Window ، والسبب في ذلك أن كلمة this تشير إلى الكائن window الذي قام باستدعاءها، ومن هنا نقول أن كلمة this غالبا ما تشير إلى الكائن الذي يقوم بعمل invoke للدالة. وبما أننا أعلنا عن الدالة بشكل عام أي globally، وكذلك الكائن window هو من استدعى الدالة foo فبذلك عندما قمنا بطباعة this وجدناها تؤشر إلى الكائن window. فالكود السابق هو نفسه الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
// declared globally
foo = function(){
	console.log(this);
}
window.foo(); // window object
```

</div>

وإليك مثال أخر للتأكيد على هذه الجزئية، لو قمنا بالإعلان عن دالة دخل كائن ما كالآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var obj = {
	foo: function(){
		console.log(this);
	}
};
obj.foo(); // obj
```

</div>

فكما ترى عندما قمنا بطباعة كلمة this في المثال السابق أشارت إلى الكائن الذي استدعاها ألا وهو الكائن obj، فخلاصة كلامنا أن كلمة this في الدالة العادية تشير إلى الكائن المالك/الكائن المُنفذ لهذه الدالة، لكن السؤال هنا الآن، ماذا في حالة دالة الإنشاء ؟؟ ماذا تشير كلمة this في دالة الإنشاء ؟؟ هذا يعتمد على استخدام كلمة new. فأنت تعرف لكي يكتمل نصاب دالة الإنشاء لابد من استخدام المعامل new، حيث أنك إن لم تستخدم الكلمة المفتاحية new فتصبح الدالة مجرد دالة عادية، وبالتالي إشارة كلمة this تعتمد على استخدام كلمة new، بمعنى أنك بمجرد قيامك بعمل instantiate لكائن جديد من دالة الإنشاء، يقوم المعامل new بتغيير سياق كلمة this من الإشارة "إلى الكائن المالك/الكائن المُنفذ" إلى "الإشارة إلى الكائن الجديد الذي سوف نقوم بعمل instantiate له" انظر إلى الكود الآتي للتوضيح:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function Employee(name){
    this.name = name;
}

var ali = new Employee("Ali");
```

</div>

كلمة this في المثل السابق تشير إلى الكائنات التي سوف يتم إنشاءها بواسطة (دالة الإنشاء + المعامل new)، وبالتالي في المثال السابق كلمة this تشير إلى الكائن ali الذي سوف ننشئه من دالة الإنشاء Employee، وكلما أنشاءنا كائن جديد تكون this معنية بهذا الكائن الجديد.

### استخدام المعامل new.target

ولو ذهبنا إلى سؤال أخر وقلنا هل من الممكن أن نعمل instantiate لكائن دون استخدام كلمة new؟ الإجابة القصيرة، لا يمكن عمل هذا، أما الإجابة الطويلة، نعم يمكن عمل هذا بشكل غير مباشر، دعونا في البداية ننظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function Employee(name){
    this.name = name;
}

var ali = new Employee("Ali"); // with new operator
var khaled = Employee("Khaled"); // without new operater

console.log(ali); // Employee { ... }
console.log(khaled); // undefined
```

</div>

عندما قمنا بعمل instantiate للكائن khaled بدون استخدام المعامل new ، وقمنا بطباعة هذا الكائن حصلنا على undefined ، على عكس الكائن ali ، والسبب يرجع إلى استخدام المعامل new. السؤال هنا هل هنالك طريقة تجعلنا نقوم بعمل instantiate للكائنات دون استخدام الكلمة المفتاحية new بشكل صريح؟ نعم هنالك طريقة لفعل هذا، وذلك باستخدام المعامل "new.target"، وهذا المعامل يساعدنا على اكتشاف "هل الدالة تم استدعاءها بواسطة المعامل new أم لا". وبهذا يمكن عمل اضافة صغيرة داخل دالة الإنشاء للكشف عن؛ "هل استُخدم المعامل new ام لا"، ففي حالة أنه لم يتم استخدام new نضيف هذه الإضافة الصغيرة، انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function Employee(name){
    // check if the function called by new operator or not
    if(!new.target){
        return new Employee(name);
    }
    this.name = name;
}

var ali = new Employee("Ali"); // with new operator
var khaled = Employee("Khaled"); // without new operater

console.log(ali); // Employee { ... }
console.log(khaled); // Employee { ... } -> it's working okay without new operator
```

</div>

بهذا قد قمنا بعمل instantiate للكائنات دون استخدام المعامل new بالشكل التقليدي، طبعا هذه كانت جزئية هامشية في موضوعنا أردنا فقط أن نمر عليها من باب العلم بالشيء، حيث من الممكن أن تصادف بعض المكتبات أو الـ frameworks التي تستخدم هذا التكنيك. والآن نعود إلى السؤال المهم ماذا يحدث خلف الكواليس عندما نقوم بعمل instantiate لكائن جديد من دالة الإشاء ؟

### ماذا يحدث خلف الكواليس عندما نقوم بعمل instantiate لكائن جديد من دالة الإشاء ؟

انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function Employee(name){
    // this = {}; -> behind the scene (implicitly)
    this.name = name; 
    this.sayMyName = function(){
        return "My name is " + this.name;
    }
    // return this; -> behind the scene (implicitly)
}

var ali = new Employee("Ali");
```

</div>

ولكي نصف ما حدث خلف الكواليس سوف نقول لقد حدث سيناريو شبيه بالآتي:-

1. تم انشاء كائن فارغ "empty object".
2. إضافة هذا الكائن إلى المعامل this.
3. إضافة الخصائص والوظائف إلى الكائن الفارغ الذي تم إرفاقه إلى المعامل this.
4. إرسال this كخرج لدالة الإنشاء.
5. معالجة الـ protoype الخاص بالكائنات الجديدة، وهذه النقطة سنتحدث عنها بالتفصيل لاحقا.

طبعا كل هذا حدث بشكل ضمني بمجرد أنك قمت بعمل instantiate لكائن جديد بواسطة المعامل new. طبعا هذه الجزئية تأخذنا إلى سؤال أخر ألا هو؛ هل يجوز استخدام كلمة return مع دالة الإنشاء ؟

عادة لا يتم استخدام return مع دالة الإنشاء، فالخرج من دالة الإنشاء هو this بما يحمله من خصائص ووظائف كما أشارنا إلى هذا في الفقرة السابقة، لكن لو سألنا هل يجوز أن نستخدم return مع دالة الإنشاء ؟ بالطبع يجوز لكن يجب أن تكون على علم بالآتي؛

- في حال استخدامك للـ return أنت بذلك تحدد الخرج الخاص بدالة الإنشاء، حيث ستقوم بعمل override على الخرج التقليدي "this".
- إذ تجاهلت الـ return، سكون خرج دالة الإنشاء هو الخرج التقليدي "this".

انظر إلى المثال الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function Employee(name){
    this.name = name; 
    return {
        name: "custom name - whatever"
    };
}

var ali = new Employee("Ali");
console.log(ali.name); // custom name - whatever
```

</div>

كان من المتوقع أن يكون خرج الـ console هو ali لكن بما أنك قمت بعمل return لدالة الإنشاء، فبهذا قد قمت بعمل override على الخرج التقليدي وأصبح ناتج الـ console هو custom name - whatever.

قبل الدخول في الققرة الأخيرة من موضوعنا، أريد أن أسالك سؤال وأظن أنك سوف تجاوب عليه بكل سهولة، انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function User(name){
    this.name = name;
}

var mark = new User("mark");
var sarah = new User("sarah");

mark.name = "micheal";
console.log(sarah.name); // What do you think about this output ?
```

</div>

ماذا تتوقع أن يكون ناتج الخرج من الـ console في السطر الأخير ؟؟ هل تأثر بالتعديل الذي حدث في السطر الذي يسبقه ؟ بالتأكيد لم يتأثر لأن كل من mark و sarah كائن مستقل عن الأخر، أينعم كلاهما كائنات من نفس الفصيل لكن إذا حدث تغيير في أي منهما لا يؤثر على الأخر، وبالتالي عندما قمنا بعمل تعديل على الكائن mark هذا لم يوثر على الكائن sarah. السؤال هنا ماذا لو أردنا أن نصنع تغيير، ونريد هذا التغيير أن يحدث في جميع الكائنات المستنسخة من الفصيل ؟ بكل بساطة سيكون التغيير في الفصيل ذات نفسه .. في دالة الإنشاء كالأتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function User(name){
    this.name = "my name is " + name;
}

var mark = new User("mark");
var sarah = new User("sarah");

console.log(mark.name); // my name is mark
console.log(sarah.name); // my name is sarah
```

</div>

وهذه النقطة تعد من مميزات الـ constructor pattern وقد ذكرناها في أعلى كلامنا، وأردنا أن نؤكد عليها بمثال قبل الدخول في الفقرة الأخيرة.

## Constructor with prototype

لو دققنا النظر في الأمثلة السابقة ربما نجد أن الـ constructor pattern يعاني من بعض المشكلات، دعونا نستحضر مثال من الأمثلة السابقة:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function Employee(id, firstName, lastName, salary){
    this.id = id;
    this.firstName = firstName;
    this.lastName = lastName;
    this.salary = salary;

    this.getFullName = function(){
        return this.firstName + " " + this.lastName;
    };

    this.giveCommission = function(amount){
		this.salary += amount;
	};
}

var mohamedAli = new Employee(21, "Mohamed", "Ali", 100);
var omarHamdy = new Employee(22, "Omar", "Hamdy", 110);

console.log(mohamedAli.getFullName()); // Mohamed Ali
console.log(omarHamdy.getFullName()); // Omar Hamdy
```

</div>

لو نظرنا إلى الكود السابق سوف نجد أن كل من الدالة getFullName ، والدالة giveCommission ، وأي دالة أخرى نعلن عنها داخل دالة الإنشاء، سيتم اعادة إعلانها وتعريفها مع كل كائن نعمل له instantiate من دالة الإنشاء Employee ، وهذا يعد عمل غير مثالي "not optimal"، أو على اقل تقدير هناك طريقة مثالية لتعريف وإعلان الدوال داخل دالة الإنشاء، ومن هنا نقول لحل هذه المشكلة نجعل جميع الكائنات المستنسخة من الـ constructor يتشاركون فيما بينهم في مثل هذه الدوال، بدلا من أعادة تعريفها كل مرة لكل كائن جديد، والذي يساعدنا في فعل هذا هو الـ prototype.

اعرف أن الكثير منكم يعرف ما هو الـ prototype ومن لا يعرف فإليه مضمون الـ prototype في إيجاز، كلما قمنا بإنشاء كائن جديد في الجافاسكربت، يكون لهذا الكائن "اوتوماتيكيا" خاصية الـ prototype، دون أن نُعَرفها نحن لهذا الكائن، هي تتولد معه تلقائيا، ليس هذا فقط؛ بل يمكن أيضا أن نضيف لها خصائص ووظائف نستدعيها في أي وقت. بمعنى أننا لو قمنا باستدعاء دالة معينة في كائن ما، السيناريو الذي يحدث كالآتي، يتم البحث أولا في هذا الكائن، هل يحتوي على هذه الدالة أم لا، في حالة أنه لا يحتوي على هذه الدالة، يتم البحث في الـ prototype الخاص به، فإن لم تكن هذه الدالة موجودة يتم البحث في مستوى أعلى من سلسلة الـ prototype، وتستمر هذه العملية على قدر طول السلسلة حتى تصل إلى الـ Master object فإذا وُجدت الدالة يتم تنفيذها، وإن لم تكون موجودة على طوال السلسلة يكون الناتج خطأ أو غير مُعرف.

والآن لكي نحصل على كود مثالي في الـ constructor pattern سوف نقوم بالإعلان عن الدوال داخل الـ prototype الخاص بدالة الإنشاء، وكما قلنا سابقا أن دالة الإنشاء لا تقوم فقط بإنشاء كائنات جديدة وحسب، بل أيضا تقوم بمعالجة الـ prototype لهذه الكائنات الجديدة، انظر مع إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function Employee(id, firstName, lastName, salary){
    this.id = id;
    this.firstName = firstName;
    this.lastName = lastName;
    this.salary = salary;
}
Employee.prototype.getFullName = function(){
	return this.firstName + " " + this.lastName;
};

Employee.prototype.giveCommission = function(amount){
	this.salary += amount;
};
var mohamedAli = new Employee(21, "Mohamed", "Ali", 100);
var omarHamdy = new Employee(22, "Omar", "Hamdy", 110);

console.log(mohamedAli.getFullName()); // Mohamed Ali
console.log(omarHamdy.getFullName()); // Omar Hamdy
```

</div>

في هذا الكود قمنا بتعريف جميع الوظائف الخاصة بفصيل الموظفين في الـ prototype ، وليس بداخل دالة الإنشاء كما في الأكواد السابقة، فالذي نرجو إليه من هذا الكود هو منع إعادة تعريف الدوال مع كل كائن جديد نعمل له instantiate، وبهذا النهج نصل إلى كود مثالي أكثر. فالسؤال هنا ما الذي حدث بالتحديد في الكود السابق؟ الذي يحدث كالآتي:- دالة الإنشاء + المعامل new يقومان بمعالجة الـ prototype لكل كائن جديد، أي جعل هذا الـ prototype يساوي نفس الـ prototype الخاص بدالة الإنشاء من خلال الـ reference، وبالتالي في الكود السابق ستجد كل من الكائن mohamedAli والكائن omarHamdy لديهم نفس الدوال التى عرفناها في الـ prototype الخاص بدالة الإنشاء، وذلك عبر الـ prototype الخاص بهم. طبعا هذه النقطة تحتاج إلى شرح مفصل لكن هذا ليس موضوعنا الآن، اردنا فقط أن ننوه عن فكرة معالجة الـ protoype الخاص بالكائنات الجديدة. ولو اردت معرفة المزيد عن الـ prototye يمكنك الاطلاع على موضوع ما هو الـ prototype في الجافاسكربت. هناك أيضا اتجاه أخر لتعريف الدوال داخل الـ prototype، فمن الممكن أن نكتب المثال السابق كالآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
function Employee(id, firstName, lastName, salary){
    this.id = id;
    this.firstName = firstName;
    this.lastName = lastName;
    this.salary = salary;
}
Employee.prototype = {
    getFullName: function(){
    	return this.firstName + " " + this.lastName;
    },
    giveCommission: function(amount){
    	this.salary += amount;
    }
};

var mohamedAli = new Employee(21, "Mohamed", "Ali", 100);
var omarHamdy = new Employee(22, "Omar", "Hamdy", 110);

console.log(mohamedAli.getFullName()); // Mohamed Ali
console.log(omarHamdy.getFullName()); // Omar Hamdy
```

</div>

وكلا الطريقتين صحيحتين، ولا يوجد فارق كبير بينهم. عند هذه النقطة نكون قد تحدثنا بشكل تفصيلي عن نمط دالة الإنشاء وكيفية محاكاة الـ classes في لغات مثل لغة الجافا أو السي بلس بلس، وبهذا نكون قد قطعنا شوطا ليس بالقليل في البرمجة على منوال object oriented programming، وبهذا يمكننا التقدم إلى الآمام والتحدث عن الوراثة في الجافاسكربت.

### لماذا يفضل معرفة نمط دالة الإنشاء رغم أن الاصدارات الجديدة من ECMAScript تتيح لنا إنشاء الفصائل باستخدام الكلمة المفتاحية class بشكل native ؟

معظمنا يعرف أن البرمجة على منوال الـ object oriented توفر لنا الكثير من المزايا، وتعزز بشكل كبير من كتابة الأكواد، ولذلك بدأت تظهر في الاصدارات الجديدة من الجافاسكربت، عناصر الـ object oriented ، مثل الكلمة المفتاحية class والتي تتيح لنا إنشاء الفصائل في الجافاسكربت، لكن هذا لا يمنعنا من أن نكون على دراية بنمط دالة الإنشاء الذي هو بالأساس كان يعالج غياب الـ classes في الجافاسكربت، وهذا للأسباب الآتية:-

1. كثير من المتصفحات لا تدعم الـ classes وخاصة المتصفحات القديمة.
2. كثير من المكتبات وإطارات العمل تستخدم هذا النمط، لذلك من المفضل أن تكون ملم بهذا النمط حتى تتعامل مع هذه المكتبات وإطارات العمل بكل سهولة.
3. ربما تحتاج في بعض الأحيان إلى polyfill لتجعل الأكواد تعمل على جميع المتصفحات دون حدوث مشكلات.
4. استخدام الكلمة المفتاحية class في الاصدارات الجديدة، ما هي إلا عملية تعليب لهذا النمط، ومعظم ما ينطبق على هذا النمط يمكن تطبيقه على الـ classes عندما نُعرفها بشكل صريح.

بالتأكيد، هذا لا يمنع استخدام الـ classes من الاصدارات الجديدة من الجافاسكربت، بل بالعكس استخدامها في مواضعها شيء جيد للغاية، وربما نتحدث عن الـ classes في الجافاسكربت بشكل تفصيلي أكثر في موضوع لاحق.

</div>
