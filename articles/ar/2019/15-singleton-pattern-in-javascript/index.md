---
id: 15
title: "الـ Singleton Pattern في الجافاسكربت"
date: "2019-03-28"
language: "ar"
tags: [javascript, design-patterns, singleton, namespace]
original_url: "https://developingschool.com/a/15/%D8%A7%D9%84%D9%80-singleton-pattern-%D9%81%D9%8A-%D8%A7%D9%84%D8%AC%D8%A7%D9%81%D8%A7%D8%B3%D9%83%D8%B1%D8%A8%D8%AA/"
source: "developingschool.com"
---

<div dir="rtl" style="text-align: right;" markdown="1">

# الـ singleton pattern في الجافاسكربت

نمط الـ singleton في الجافاسكربت، يعد من الأنماط الجميلة جدا، وفي بعض الأحيان لا يمكن الاستغناء عنه، فهو نمط له فوائد كثيرة جدا برغم أن عملية تنفيذه عملية سهلة للغاية. فما هو الـ singleton pattern ؟ الـ singleton pattern هو نمط يوفر لنا عملية التأكد من أن الفصيل أو الـ "class" لديه نسخة "instance" واحدة فقط لا أكثر، وهذه النسخة يمكن الوصول لها بشكل globally من خلال نقطة محددة عبر باقي التطبيق أو النظام. يمعنى أخر؛ نحن نقوم في هذا النمط بعملية يتم فيها تقييد استنساخ الكائنات من الفصيل، حيث نقوم باستنساخ كائن واحد فقط من الفصيل، وهذا الكائن يسمى بالـ singleton، وجميع أجزاء وأكواد التطبيق تستطيع أن تصل إلى هذا الـ singleton instance عبر نقطة محددة ومعروفة "point of access" وهذه النقطة غالبا ما تكون مُعَرفة بكشل globally، أي في الـ global scope في الجافاسكربت.

الجافاسكربت تدعم لنا إنشاء الكائنات literally بكشل native، أي يمكننا أن نقوم بإنشاء كائن واحد فقط دون الحاجة إلى تعريف فصائل واستنساخ كائنات منها، وبالتالي إذا كان نمط الـ singleton يقوم بتقييد عملية استنساخ الكائنات من الفصيل إلى كائن واحد فقط، فهل إنشاء كائن literally في الجافاسكربت يعد singleton pattern ؟ دعونا نؤجل الاجابة عن هذا السؤال بعض الوقت، ونذهب إلى الـ implementation لهذا النمط:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var mySingleton = (function(){

    // here we store the reference to the singleton
    var instance;

    function MyClass(){

    }
    MyClass.prototype.methodA = function(){};
    MyClass.prototype.methodB = function(){};


    return {
        getInstance: function(){
            // check if the instance has been instanciated or not
            // make sure that only one instance will be there
            if(!instance){
                instance = new MyClass();
            }
            return instance;
        }
    };

})();

// Usage
var singletonA = mySingleton.getInstance();
var singletonB = mySingleton.getInstance();

console.log(singletonA === singletonB); // true
```

</div>

لو نظرنا إلى الكود السابق ستجد أننا قمنا بتقييد استنشاء الكائنات، ففي كل مرة نستدعي فيها الدالة "getInstance" ستعطينا نفس الـ instance ولن تقوم باستنشاء كائن جديد، وبهذا نكون قد طبقنا نمط الـ singleton. والآن دعونا نتحدث بتفاصيل أكثر عن هذا النمط.

هناك سؤال لابد أن نسأله لأنفسنا مادمنا نتحدث عن الـ singleton pattern. ما هو المغزى والاستفادة من وراء هذا النمط؟ ما هي الحالات التي نحتاج فيها singleton instance؟. من وجهة نظري؛ لا يخلو أي تطبيق أو نظام من هذه الفكرة؛ الفكرة في حد ذاتها؛ فكرة الـ singleton object سواء كتبته على منوال هذا النمط، أو حتى استخدمت object literal في الجافاسكربت. فكرة الكائن الواحد هذه مهمة جدا، وربما أنت تستخدمها باستمرار في المشروعات التي تعمل عليها باختلاف طريقة تضمينها. فعلى سبيل المثال، لو أنك تقوم بعمل مشروع ما، وفي هذا المشروع هناك module خاص بالـ user preferences والذي بدوره يتعامل مع اللغة التي اختارها المستخدم والـ timezone وما شابه، وكذلك يحتوى هذا الـ module على الوظائف الخاصة بهذه الـ preferences كتغيير لغة المستخدم والـ timezone ... إلخ. في هذه الحالة ستجد نفسك أمام singleton object، حيث لا يوجد مبرر أن يكون هناك أكثر من instance لهذا الـ module، هو object واحد يحتوي على الـ state الخاص بالـ user preferences على مدار المشروع. ومن هنا نقول أن هذه الفكرة مهمة جدا أيا كانت طريقة تنفيذها أو طريقة تضمينها. والآن دعونا ننظر إلى الكود السابق بشكل مفصل أكثر.

### Namespacing

واحدة من المميزات التي نحصل عليها من هذا النمط هي الـ namespacing حيث أننا نضمن جميع الأكواد الخاصة بالـ module تحت اسم متغير واحد، وبهذا نقلل عدد الـ global variables ومن ثم تقل احتمالية الـ variables collison. لو نظرنا إلى الجزء الآتي من الكود:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var mySingleton = (function(){


    return {
        getInstance: function(){

        }
    };

})();
```

</div>

لو نظرنا إلى الكود السابق سنجد أن جميع أجزاء الـ module كلها تقع تحت المتغير mySingleton ولن يكون هناك أي global variable أخر غير هذا المتغير.

* ملحوظة:- يمكنك الاطلاع على موضوع الـ module pattern في الجافاسكربت إذا كان لديك أي استفسارات عن الكود السابق.

### getInstance Method

والآن نأتي إلى نقطة من أهم النقاط في هذا النمط ألا وهي دالة الـ getInstance، وهذه الدالة هى المسئولة عن التأكد من أن الفصيل ليس لديه إلا نسخة واحد لا أكثر من هذا، انظر إلى الكود الأني:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var mySingleton = (function(){

    // here we store the reference to the singleton
    var instance;

    function MyClass(){

    }
    MyClass.prototype.methodA = function(){};
    MyClass.prototype.methodB = function(){};


    return {
        getInstance: function(){
            // check if the instance has been instanciated or not
            // make sure that only one instance will be there
            if(!instance){
                instance = new MyClass();
            }
            return instance;
        }
    };

})();
```

</div>

في الكود السابق ستجد أن دالة getInstance تعمل بمثابة الحارس، حيث أنها في كل مرة تراجع وتتأكد من أن الفصيل MyClass لديه نسخة مستنشئة منه أم لا؟ في حالة عدم وجود أي نسخة مستنشئة من هذا الفصيل يتم استنشاء نسخة جديدة وتخزينها في متغير للمرات القادمة، ومن ثم ارجاع هذه الـ instance كخرج، وإذا كانت هناك نسخة مستنشئة بالفعل من الفصيل يتم ارجاع الـ reference لهذه النسخة كخرج للدالة. وبهذا لن تكون هناك إلا نسخة واحدة من الفصيل MyClass. انظر معي إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
// Usage
var singletonA = mySingleton.getInstance();
var singletonB = mySingleton.getInstance();

console.log(singletonA === singletonB); // true
```

</div>

لو نظرنا إلى الكود السابق، ستجد أنك كلما استدعيت الدالة getInstance سوف تحصل على نفس الـ singleton instance في كل مرة.

### Lazy Loading

واحدة من النقاط المهمة التي يجب أن تأخذها في عين الاعتبار وأنت تتعامل مع نمط الـ singleton هي الـ lazy loading أو الـ delayed instantiation. في المثال السابق ستجد أننا لم نقم بعمل instantiation للفصيل MyClass إلا عندما قد تم استدعاء الدالة getInstance، فعملية الـ instantiation لا تحدث عند بداية تشغيل الكود، بل تحدث في وقت لاحق، وهذا ما يسمى بالـ lazy load، وهي النقطة تكون مفيدة عندما تكون عملية الـ instantiation معتمدة على عمليات أخرى قبلها.

### Object Literal VS Singleton Pattern

لو عدنا إلى السؤال الذي سألناه في بداية موضوعنا، إذا كانت لغة الجافاسكربت تدعم لنا بشكل native انشاء الكائنات Literally، فهل هذه الكائنات تعد ضمن نمط الـ singleton ؟؟ في الحقيقة يمكننا أن نقول مجازا أن الـ object literal يعد واحدا من الـ techniques التابعة لنمط الـ singleton، لكن عليك أن تأخذ بعض النقاط في عين الاعتبار عندما تقارن بين الـ object literal وبين نمط الـ singleton:-

- أول ظهور لهذا النمط لم يكن في لغة الجافاسكربت. ظهر هذا النمط في لغات أخرى، ولذلك عندما تقرأ عن هذا النمط ستجد أنه عبارة عن عملية يتم فيه التأكد وتقييد علمية الاستنشاء الـ instantiation للفصيل إلى instance واحدة، وبالتالي إذا طبقنا هذا النمط كما في اللغات الأخرى التي بدأ فيها ظهور هذا النمط، فلابد لنا من عملية الـ instantiation. وفي الـ object literal لا توجد عملية الـ instantiation. وهذه تعد نقطة اختلاف، لكن كما قالنا أن الـ object literal يعتبر مجازا تكنيك مختلف لهذا النمط.
- النقطة الأخر تكمن في الـ lazy load، في حالة الـ object literal بيتم تعريف الكائن وحجز مساحة له في الذاكرة أثناء عملية تنفيذ الكود على عكس نمط الـ singletone حيت يتضمن فكرة الـ lazy load، فيتم عمل الـ instantiation للـ singleton instance في وقت لاحق وليس أثناء تنفيذ الكود كما الـ object literal.

### Main Issues Of Singleton Pattern

واحدة من المشكلات الكبيرة التي تنتج عن استخدام هذا النمط هي Tight Coupling أي اعتماد الأكواد على بعضها بشكل أو بأخر ومن ثم يتنج عن هذا صعوبة في اختبار الأكواد وبناء Test Units لأكواد البرنامج أو التطبيق. وهذه النقطة تعد من أهم التحديات التي تقابل نمط الـ singleton.

في النهاية مازال هناك نقاش قائم بين المطوررين حول أهمية هذا النمط من عدمه، كذلك طريقة تضمينه، هناك الكثير من وجهات النظر المختلفة حول هذا النمط، لكن يظل دائما القرار الصحيح متوقف على الحالة التي أنت بصددها، فمن ناحية ما؛ ربما تكون بصدد حالة لا يجوز أو غير محبذ أن تستخدم فيها هذا النمط. ومن ناحية أخرى؛ ربما تكون بصدد حالة أخرى يجوز فيها أو من المحبذ أن تستخدم معها هذا النمط. في النهاية كونك على دراية وعلم ومعرفة بهذا النمط فهذا شيء مفيد جدا وسوف يساعدك على اختيار الأنمطة المناسبة لكل حالة سوف تعمل عليها.

</div>
