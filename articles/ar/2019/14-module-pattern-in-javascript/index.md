---
id: 14
title: "الـ Module Pattern في الجافاسكربت"
date: "2019-02-14"
language: "ar"
tags: [javascript, design-patterns, module-pattern]
original_url: "https://developingschool.com/a/14/%D8%A7%D9%84%D9%80-module-pattern-%D9%81%D9%8A-%D8%A7%D9%84%D8%AC%D8%A7%D9%81%D8%A7%D8%B3%D9%83%D8%B1%D8%A8%D8%AA/"
source: "developingschool.com"
---

<div dir="rtl" style="text-align: right;" markdown="1">

# الـ Module Pattern في الجافاسكربت

![صورة توضح فكرة الـ module - أجزاء السيارة المختلفة](./images/car-modules.png)

في البداية، ما معنى كلمة module؟ بكل بساطة الـ module هو كل وحدة من وحدات أي تركيب، وفي عالم السوفتوير؛ الـ module هو عبارة عن جزء من أجزاء البرنامج، فالبرنامج إما أن يتكون من module واحد أو عدة modules، وعندما نقوم بتجميع الـ modules المختلفة مع بعضها نستطيع في النهاية أن ننشئ software أو نظام قوي للغاية، وقابل للتطوير والصيانة بكل سهولة. قبل أن نتحدث عن كون الـ module مجرد نمط نستخدمه في كتابة الأكواد، علينا أولا أن نتحدث عن الفكرة في حد ذاتها؛ دعونا ننظر مثلا إلى السيارة، لو تأملنا السيارة قليلا سوف نجدها تتكون من عدة وحدات مختلفة، فستجد مثلا الموتور .. المقعد .. الإطار .. المرآة .. وهكذا. وكل وحدة من هذه الوحدات يتم تصنيعها على حدة، وفي النهاية يتم تجميع كل الوحدات لنحصل على السيارة. هذه الفكرة منجزة وفعالة للغاية، فلو كل فريق عَمِلَ فقط على وحدة من وحدات السيارة، وعكف عليها واهتم بها، يستطيع أن يعطينا جزء مصنوع بعناية ودقة عالية، وبهذا نحصل في النهاية على سيارة مصنعة بعناية، بالإضافة إلى سهولة تعديل أي وحدة من هذه الوحدات أو تطويرها. طبعا هذه مجرد دردشة عامة حول الفكرة، ومنها تستطيع أن تستنبط مدى المميزات الناتجة عن استخدام هذه الفكرة أو هذا المفهوم، وعلى رأس هذه المميزات هو استقلالية كل جزء عن باقي الأجزاء، وفي الوقت نفسه جميع الأجزاء تستيطع أن تعمل معا في تناغم لتعطينا في النهاية تركيب معقد عملاق. وقبل أن ننهي هذه الجزئية دعونا ننوه سريعا أن الـ module الواحد ربما يتكون من عدة modules أخرى بداخله. فكما ذكرنا سابقا أننا عندما نتحدث عن الـ module فنحن أمام فكرة "الجزء من الكل". والآن بعد أن تحدثنا بشكل عام عن الفكرة، دعونا نتطرق إلى النمط نفسه، وكيفية تطبيقه في الجافاسكربت حيث جاء الـ module pattern لكي يساعدنا على التغلب على بعض التحديات:-

- أولا؛ تقسيم البرنامج أو التطبيق إلى أجزاء صغيرة في شكل وحدات كما ذكرنا في أول الموضوع.
- ثانيا؛ الكبسلة encapsulation واخفاء البيانات أو خصوصية البيانات data hiding.
- ثالثا؛ سهولة التعديل والتطوير، وإعادة استخدام الأكواد.

الكثير منا يعرف ماذا نقصد بـ الكبسلة وأخفاء البيانات، ومن لا يعرف فسنقول بإيجاز: الكبسلة هي وضع كل البيانات والخصائص والوظائف التي لها علاقة ببعض في مكان واحد. أما أخفاء البيانات، فهي وضع بعض البيانات في مكان لا يمكن لباقي أكواد التطبيق أن تصل إليه إلا عن طريق محدد أو لا تصل إليه اطلاقا وهذا ما نحدده نحن عندما نعطي خصوصية لبيان ما في التطبيق. وغالبا ما ستجد ترابط بين الكبسلة واخفاء البيانات.

### variables collision

والآن دعونا نتطرق إلى بعض المشكلات العملية حتى تتضح الصورة أكثر، لو فرضنا أنك قمت بتعريف متغير "x" في الـ global scope، ثم قمت باستدعاء مكتبة ما، وهذه المكتبة تقوم أيضا بتعريف متغير "x" في الـ global scope، وقتها سيحدث تضارب بين المتغيرات، وستدخل القيم في بعضها، وهذا ما يسمي بالـ variables collision. وحتى لا نقع في مثل هذه المشكلة علينا أن نفكر في الـ module pattern.

### Private Members

المشكلة الأخرى أن لغة الجافاسكربت -خاصة الاصدارات الأولى- لا تدعم بشكل مباشر فكرة خصوصية البيانات الـ private variables/functions، فمثلا لو أن لديك متغير "x" يحمل قيمة ما، ولا تريد لباقي أكواد التطبيق أن تغير هذه القيمة، يمكنها أن تقرأها لكن لا تغيرها، هنا يأتي دور الـ module pattern.

### Scope And Closure

لغة الجافاسكربت كما قلنا لا تدعم بشكل native فكرة خصوصية البيانات "Data Privacy"، وبالتالي علينا أن نطوع اللغة بشكل أو بأخر لكي نحقق هذه الفكرة، والذي سيساعدنا كثيرا في تحقيق هذه الفكرة هو الـ "Scope and Clouser"، الـ "scope and closure" موضوع يطول فيه الحديث، وربما نتحدث عنه بشكل مفصل في وقت لاحق، لكن دعونا في هذه الفقرة نأخذ الجزء الذي يهمنا في الموضوع، جميعنا يعرف أننا بمجرد ما نقوم بتعريف دالة "function" في الجافاسكربت، يكون لهذه الدالة scope خاص بها، ومن قواعد الـ scope في الجافاسكربت أن الـ inner scope يستطيع الوصول إلى الـ outer scope والعكس غير صحيح، انظر معى إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
// global scope
var x = 'I am in the global scope';

function foo(){
    // foo scope
    var y = 'I am in the foo scope';

    // inner scope (foo) can access outer scope (global)
    console.log(x); // I am in the global scope
    console.log(y); // I am in the foo scope
}

console.log(x); // I am in the global scope

// outer scope (global) can not access inner scope (foo)
console.log(y); // error -> y is not defined
```

</div>

في هذا الكود، سنجد أن الـ scope الخاص بالدالة foo يستطيع الوصول إلى الـ global scope، أما العكس غير صحيح، بمعني أن الـ global scope لا يستطيع الوصول إلى الـ scope الخاص بالدالة foo. وبهذه الفكرة البسيطة نستطيع أن نقوم باخفاء البيانات عن طريق الاستفادة من الـ scope، وهنا يبقى جزئية نريد أن نمر عليها سريعا إلى أن نتحدث عنها بشكل مفصل ألا وهي الـ closure. ما هو الـ closure ؟؟ الـ closure بايجاز هو عبارة عن رابطة بين الـ function وبين الـ scope chain الخاص بها، فأينما تم تنفيذ الدالة، فهي تحتفظ بالـ scope chain الخاص بها وقت تعريفها. معنى ذلك أننا عندما نقوم بعمل invokation للدالة في أي مكان في الكود، فهي تحتفظ بالحالة الـ "state" والـ scope chain الخاص بها. طبعا هذه النقطة يطول حقا فيها الحديث لكن اردنا فقط أن نمر سريعا على الجزء الذي سيخدمنا في الـ module pattern.

### Module Creation

والأن دعونا نتطرق إلى إنشاء الـ modules في الجافاسكربت. باستخدام الـ Immediately Invoked Function Expression or IIFE نستطيع أن ننشئ anonymous clusore/scope، والذي يحتوي على بيانات معينة، وتكون هذه البيانات مخفية عن أي scope أخر خارجي. انظر معى إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
// Create Anonymous Closure
(function(){
    // everything (variables and methods) is scoped here

})();
```

</div>

بهذا الكود البسيط نكون قد أنشأنا module مستقل بذاته. وهذا الـ module يحتوي على البيانات الخاصة به، ولا يمكن الوصول إلى هذه البيانات إلا إذا اردنا ذلك، عن طرق معينة نحن من نحددها، وهذه الجزئية تأخذنا إلى موضوع الـ module export.

### Namespacing

واحدة من المميزات التي نحصل عليها من هذا النمط هي الـ namespacing حيث أننا نضمن جميع الأكواد الخاصة بالـ module تحت اسم متغير واحد، وبهذا نقلل عدد الـ global variables ومن ثم تقل احتمالية الـ variables collison. لو نظرنا إلى الجزء الآتي من الكود:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var myModule = (function(){


    return {
        some_properties: '',
        some_methods: function(){

        }
    };

})();
```

</div>

لو نظرنا إلى الكود السابق سنجد أن جميع أجزاء الـ module كلها تقع تحت المتغير myModule ولن يكون هناك أي global variable أخر غير هذا المتغير. 

### Module Export

بالتأكيد أي module في البرنامج هو يتعامل بشكل أو بأخر مع باقي أجزاء البرنامج، والفكرة الرئيسية تدور حول كيفية التعامل والتنسيق بين أجزاء البرنامج وبعضها البعض، ومن خلال الـ exporting نستطيع تحديد الكيفية التي سوف يتعامل بها الـ module مع الـ modules الأخري، أو بالأحرى كيف ستتفاعل الأجزاء الأخرى مع الـ module. انظر معي إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var myModule = (function(){
    /*
    * here we will define our private members
    */
    var privateKey = 'i am private';

    /* 
    * export certain data 
    * this consider public API for this module
    */
    return {
        bar: 10,
        foo: function(){
            console.log('Hello World!');
        },
        publicMethod: function(){
            console.log(privateKey);
        },
    };

})();

console.log(myModule.bar); // 10
myModule.foo() // Hello World!
myModule.publicMethod(); // i am private
```

</div>

لو نظرت إلى الكود السابق ستجد أننا قمنا بفتح نافذة لهذه الـ module عن طريق الـ return، وقمنا بعمل assignment للناتج في المتغيير myModule، وهذه النافذة سوف تتيح وتحدد لأجزاء البرنامج الأخرى كيف تتعامل مع هذا الـ module.

### Module Import

لا يتوقف عمل الـ mudole على الـ exporting وحسب، بل يمكنه أيضا أن يأخذ بيانات ما ليقوم بمعالجتها، أو أنه يعتمد على modules أخرى لكي يقوم بوظيفته، وهنا تأتي نقطة الـ importing حيث يمكن تمرير paramters إلى الـ module. انظر إلى الكود الأتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var settings = {
    sound_volume: 50
};

var videoPlayerModule = (function(stngs){
    // private variable
    var sound = stngs.sound_volume || 10;

    return {
        play: function(){
            console.log('play sound at ' + sound);
        }
    };

})(settings);

videoPlayerModule.play(); // play sound at 50
```

</div>

في الكود السابق قمنا بعمل import لبعض البيانات إلى الـ module، وهذه البيانات ممكن أن تكون module أخر نعمل له extend أو نعدل عليه .. أو تكون هذه البيانات dependencies modules .. أو تكون object كما في المثال السابق .. أو ممكن أن تكون مجرد قيم عادية يتم تمريرها إلى الـ module ... إلى أخره. والآن بعد ما تحدثنا عن الـ export والـ import دعونا نتحدث أكثر عن Data Privacy.

### Private Members

أشرنا في أحد الأمثلة السابقة عن الـ private members وللحديث أكثر عن هذه الجزئية سنقول أن لغة الجافاسكربت لا تدعم الكلمة المفتاحية private وبالتالي نحن نتجه إلى الـ module pattern حيث يساعدنا كثيرا في مجاراة الـ private variables and private functions. انظر معي إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var myModule = (function(){

    var _privateProperty = 'I am a private property member';

    function _privateMethod(){
        return 'I am a private method member';
    }

    return {
        getPrivateProperty: function(){
            return _privateProperty;
        },
        publicMethod: function(){
            return _privateMethod();
        }
    };

})();

myModule.getPrivateProperty(); // I am a private property member
myModule.publicMethod(); // I am a private method member

// try to access private members
myModule._privateProperty; // undefined
myModule._privateMethod(); // error -> myModule._privateMethod() not a function
```

</div>

في الكود السابق قمنا بتعريف بعض الـ private members داخل الـ modules وهذه الـ private members لا يمكن الوصول لها من خارج الـ module إلا عن طريق الـ public method، وبهذا نكون قد حققنا مبدأ Data Privacy. فلو نظرنا إلى أخر سطرين في الكود السابق سنجد أننا لا نستطيع الوصول للهذه البيانات.

ملحوظة:- بعض الأحيان نضيف underscore "_"  قبل اسم الـ private members وهذا يساعدنا بعض الشيء في التفريق بين الـ public members وبين الـ private members.

### Revealing Module Pattern

طبعا الـ module pattern له أشكال ومشتقات كثيرة جدا، ومن أشهر هذه الأشكال الـ revealing module pattern حيث أنك تقوم بعمل export للخصائص والوظائف التي تريدها أن تكون public، انظر معي إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var myModule = (function(){

    // private
    var _a = 10;
    // (we will make it) public
    var b = 500;
    // private
    function _foo(){

    }
    // (we will make it) public
    function bar(){

    }

    return {
        b: b,
        bar: bar
    };

})();
```

</div>

في هذا الكود قمنا بتعريف جميع الخصائص والوظائف الخاصة بالـ module، وفي نهايته قمنا بعمل return للخصائص والوظائف التي نريدها أن تكون public فقط، فمن أهم فوائد الـ revealing module pattern أنك عندما تنظر إلى الجزء الأخير من الـ module تستطيع بكل سهولة أن تتعرف على جميع الـ public members الخاصة بالـ module.

### Counter Example

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var Counter = (function(initialValue, $){

    var _initialValue = initialValue || 0;
    var _currentResult = _initialValue;

    function increment(){
        _currentResult++;
        _render();
    }
    function decrement(){
        _currentResult--;
        _render();
    }
    function _render(){
        $('body').html('Initial Value was ' + _initialValue + ', Current Value Is ' + _currentResult);
    }

    return {
        increment: increment,
        decrement: decrement
    };

})(10, jQuery);

Counter.increment(); // Initial Value was 10, Current Value Is 11
Counter.increment(); // Initial Value was 10, Current Value Is 12
```

</div>

في هذا المثال نكون قد أشملنا جميع النقاط التي ذكرناها من حيث الكبسلة واخفاء البيانات وكذلك الـ exporting والـ importing والـ revealing. وقبل أن ننهي موضوعنا هذا اريد أن أؤكد على فكرة الـ modularity أو "الجزء من الكل"، فالـ modularity مفهوم أوسع بكثير من مجرد نمط، وكونك تتشبع بهذا المفهوم فهذا يعد اضافة كبيرة لك. في النهاية الـ module pattern له اشكال عديدة ويمكن تطوعيه بأشكال عدة لكن تظل في النهاية الفكرة واحدة، وهو نمط مهم للغاية ما دمت تسعى للحصول على اكواد نظيفة مرتبة قابلة للتطوير والصيانة واعادة الاستخدام خاصة في التطبيقات الكبيرة والمتقدمة. 

</div>
