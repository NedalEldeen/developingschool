---
id: 16
title: "نمط الـ PubSub في الجافاسكربت - Publish Subscribe Pattern"
date: "2019-04-24"
language: "ar"
tags:
  - javascript
  - design-patterns
  - pubsub
  - publish-subscribe
original_url: "https://developingschool.com/a/16/%D9%86%D9%85%D8%B7-%D8%A7%D9%84%D9%80-pubsub-%D9%81%D9%8A-%D8%A7%D9%84%D8%AC%D8%A7%D9%81%D8%A7%D8%B3%D9%83%D8%B1%D8%A8%D8%AA-publish-subscribe-pattern/"
source: "developingschool.com"
---

<div dir="rtl" style="text-align: right;" markdown="1">

# نمط الـ PubSub في الجافاسكربت - Publish Subscribe Pattern

في البداية نمط الـ PubSub هو اختصار للـ publish subscribe pattern وربما تجد له بعض الأسامي الأخرى، لكن في النهاية مهما كانت المسميات الفكرة واحدة ومحددة ومعروفة. قبل الدخول في تفاصيل هذا النمط علينا أولا أن نتحدث عن المشكلة التي جاء من أجلها هذا النمط ليساعدنا على حلها.

هناك بعض الأنماط والمصطلحات التي بمجرد أن تسمعها عليك أن تضع عينيك على مشكلة ما، فعندما تسمع مثلا نمط الـ PubSub أو نمط الـ Observer أو نمط Mediator أو مصطلح الـ Event Bus أو الـ Event Aggregation أو الـ Modules Communication عليك أن تضع عينيك على مسألة الـ Code Coupling.

ربما قد مر عليك من قبل هذه المصطلحات الـ code coupling أو الـ code decoupling أو الـ lossly coupling وما شابه من هذه المصطلحات. فماذا يعني هذا الكلام ؟ باختصار شديد المقصود من هذا الكلام هو؛ ما هي نسبة وكيفية اعتماد الأكواد على بعضها البعض؟. عندما تقوم ببناء أي نظام أو برنامج أو تطبيق، فأنت بصدد عدة modules المكونة لهذا النظام أو التطبيق. تحدثنا من قبل عن الـ [Module Pattern](../14-module-pattern-in-javascript/)وتكلمنا بالتفصيل عن فكرة الجزء من الكل، وعبرنا عن أهمية تقسيم السوفتوير أو التطبيق إلى وحدات modules مختلفة يتم تجميعها مع بعض لتعطينا في النهاية تطبيق/برنامج قوي. ومن هنا نقول إن أي تطبيق أو برنامج غالبا ما يتكون من عدة modules. فالسؤال هنا؛ كيف يتم التواصل والتخاطب بين هذه الـ modules ؟؟ اجابة هذا السؤال تعد نقطة محورية في أي سوفتوير. انظر معى إلى الصورة الآتية:-

![مخطط يوضح الفرق بين الـ Tightly Coupled Code والـ Lossly Coupled Code](./images/publish-subscribe-pattern-javascript.png)

### Tightly Coupled Code

بمجرد النظر إلى الصورة السابقة، تستطيع بكل سهولة أن تسنبط الاختلافات بين جانبي الصورة، فلو نظرت إلى الجزء الحاص بـ Tightly Coupled Code ستجد أن الـ modules معتمدة بشكل مباشر على بعضها البعض، ومتداخلة ومتشابكة بشكل كبير جدا، وهذا المنهج في كتابة الأكواد له عيوب كبيرة وكثيرة، وعلى رأس هذه العيوب؛ أنه لو حدثت مشكلة في أي من الـ modules غالبا ما سيؤثر على الـ modules الأخرى، وبالتالي سيؤثر على السوفتوير ككل. وكذلك صعوبة اعادة استخدام الأكواد الـ "modules" في مشروعات أخرى لأنها معتمدة ومتداخلة ومتشابكة مع أكواد "modules" أخرى، كذلك صعوبة عمل وحدات اختبار "testing units" للأكواد. واضف إلى ذلك، أن أي تعديل سوف تجريه على أي من الـ modules غالبا ما سيتطلب عمل تغييرات وتعديلات في الـ modules الأخرى. وبالتالي اتباع هذا المنهج في كتابة الأكواد يبتعد كل البعد عن مبدأ الفصل بين وحدات التطبيق/السوفتوير المختلفة، ومبدأ الاكواد النظيفة، ومبدأ اعادة استخدام الأكواد.

### Lossly Coupled Code

على الجانب الأخر من الصورة الخاص بـ Lossly Coupled Code ستجد أن الموضوع اضحى مختلف تماما، أصبحت الـ modules لا تتداخل ولا تتشابك ولا تعتمد على بعضها البعض بشكل مباشر. وأصبح كل module مستقل بشكل كبير جدا بذاته، ولا يعرف شيء عن الـ modules الأخرى، ويمكن بسهولة التعديل فيه دون الخوف من وجوب تعديلات في الـ modules الأخرى، كذلك اعادة استخدامه في مشروعات اخرى. واضف إلى ذلك أنه إذا حدثت مشكلة في أي من الـ modules ستظل مقتصرة على هذا الـ module ولن تؤثر -كما في الجانب الأخر- في الـ modules الأخرى.

### PubSub Pattern

والآن دعونا ندخل إلى نمط الـ publish subscribe pattern، المعروف بـ PubSub Pattern، هذا النمط هو نمط سلوكي، أي معنيِ بسلوك التطبيق أو النظام، فهو مسئول عن عملية التواصل والتخاطب بين أجزاء التطبيق وبعضها. فعندما نتأمل فكرة التخاطب بين أجزاء التطبيق أو النظام، سنجد أن معظم عمليات التخاطب هذه تكمن في إرسال رسالة أو إشارة من وإلى الـ modules المختلفة، كأن يخاطب الـ module A الـ module B، ويقول له أن ثمة تغيير قد طرأ عليه إذا كان مهتما بهذا التغيير وهكذا ... . كذلك عمليات التخاطب هذه تشمل إرسال البيانات من وإلى الـ modules المختلفة. وبالتالي يمكن القول أن عملية التخاطب هنا يمكن تنفيذها بواسطة فكرة الأحداث، وهنا يصبح نمط الـ PubSub هو المسئول عن معالجة هذه الأحداث والعمليات الخاصة بها، حيث يوجد ثلاث عمليات رئيسية في هذه النمط. أولهم؛ عملية الاشتراك أو الاستماع لحدث ما وهي الـ subscription، ثانيهم؛ عملية إذاعة ونشر الحدث وهي الـ publishing، وثالثهم؛ عملية إلغاء الاشتراك أو الاستماع وهي الـ unsubscription. انظر إلى الكود الآتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var PubSub = {
	/*
	* here we store all event names and callback handlers of subscripers
	* we could name this property; events, topics, subscripers or whatever
	*/
	subscribers: {},
	/*
	* make (subscribers) subscribe to certain event
	*/
	subscribe: function(eventName, cb){

	},
	/*
	* make (publishers) emit/invoke certain event
	*/
	publish: function(eventName, data){

	},
	/*
	* make (subscribers) unsubscribe from certain event
	*/
	unsubscribe: function(eventName, cb){

	},
};
```

</div>

فكما ترى أن هذا النمط عبارة عن singleton object سواء ضمنته على منوال الـ [singleton patten](../15-singleton-pattern-in-javascript/)أو object literal كما في الكود السابق. وأيضا سوف تلاحظ أن هذا النمط يحتوى على بعض الوظائف الرئيسية:-

- أولهم الـ subscribe method، وهي المسئولة عن جعل الـ modules أو الكائنات المسماه هنا بـ "subscribers" أن يستمعوا إلى الأحداث التي هم مهتمون بها.
- ثانيهم الـ publish method، وهي المسئولة عن جعل الـ modules أو الكائنات المسماه هنا بـ "publishers" بعمل invkoe أو publish لحدث ما.
- تالثهم الـ unsubscribe method، وهي المسئولة عن جعل الـ modules أو الكائنات "subscribers" بأن يلغوا عملية الاستماع إلى الأحداث التي كانوا مهتمين بها في وقت ما.

والآن دعونا نتطرق إلى وظيفة وظيفة بالتفاصيل، ولنبدأ بوظيفة الـ subscribe :-

### Subscribe Method

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var PubSub = {
	// [...]
	/*
	* make (subscribers) subscribe to certain event
	* another name for this method could be "on"
	* @param String eventName
	* @param Function cb (will be invoked when any publisher emit the eventName)
	*/
	subscribe: function(eventName, cb){
		// make sure the "this.subscriptions" object has eventName property
		// that holds an array of event handlers
		this.subscriptions[eventName] = this.subscriptions[eventName] || [];
		// add the callback function (event handler) to eventName array
		this.subscriptions[eventName].push(cb);
	},
	// [...]
};
```

</div>

دالة الـ subscribe هي المسئولة عن عملية الاستماع للأحداث، فعندما يريد أي module أو object أن يستمع إلى حدث ما، فإنه سوف يستمع للأحداث عبر هذه الدالة، وهذه الدالة تأخذ معاملان، الأول؛ هو اسم الحدث، والثاني؛ هو الدالة الـ callback التي سوف يتم تنفيذها عند حدوث ذاك الحدث. حيث تقوم هذه الدالة بحفظ عمليات الاشتراك/الاستماع في الخاصية subscriptions والتي من الممكن أن تجد لها اسماء مختلفة مثل الـ topics أو الـ evnets أو أي اسم أخر. وعندما يتم عمل invoke أو publish لحدث ما يتم تنفيذ جميع الـ callbacks الخاصة بهذا الحدث. والآن دعونا نلقى نظرة على دالة الـ publish:-

### Publish Method

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var PubSub = {
	// [...]
	/*
	* make (publishers) emit/invoke certain event
	* another names for this method could be "emit", "invoke" or "fire"
	* @param String eventName
	* @param Mixed data (could be object, array, string or whatever), this data passed by publishers
	*/
	publish: function(eventName, data){
		if(!!this.subscriptions[eventName]){
			this.subscriptions[eventName].forEach(function(cb){
				cb(data);
			});
		}
	},
	// [...]
};
```

</div>

دالة الـ publish هي المسئولة عن إذاعة الأحداث، أي أنها تقوم بعمل invoke لجميع الـ callback handlers الخاصة بحدث ما، فعندما يريد أي module أو كائن -والتي يطلق عليها هنا اسم الـ publishers- بإذاعة حدث ما، فإنه ينفذ الدالة publish ويمرر لها اسم الحدث كمعامل أول، وكذلك أي بيانات لها علاقة بالحدث كمعامل ثاني للدالة. والآن مع الدالة unsubscribe:-

### Unsubscribe Method

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var PubSub = {
	// [...]
	/*
	* make (subscribers) unsubscribe from certain event
	* another name for this method could be "off"
	* @param String eventName
	* @param Function cb (will be spliced from eventName callback handlers)
	*/
	unsubscribe: function(eventName, cb){
		if(!!this.subscriptions[eventName]){
			var cbIndex = this.subscriptions[eventName].indexOf(cb);
			if(cbIndex != -1){
				this.subscriptions[eventName].splice(cbIndex, 1);
			}
		}
	}
};
```

</div>

الدالة unsubscribe من اسمها، هي عكس الدالة subscribe، حيث أنها المسئولة عن إلغاء عمليات الاستماع للأحداث. والآن دعونا نلقى نظرة على الكود ككل:-

### All Together

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var PubSub = {
	/*
	* here we store all event names and callback handlers of subscripers
	* we could name this property; events, topics, subscripers or whatever
	*/
	subscriptions: {},
	/*
	* make (subscribers) subscribe to certain event
	* another name for this method could be "on"
	* @param String eventName
	* @param Function cb (will be invoked when any publisher emit the eventName)
	*/
	subscribe: function(eventName, cb){
		// make sure the "this.subscriptions" object has eventName property
		// that halds an array of event handlers
		this.subscriptions[eventName] = this.subscriptions[eventName] || [];
		// add the callback function (event handler) to eventName array
		this.subscriptions[eventName].push(cb);
	},
	/*
	* make (publishers) emit/invoke certain event
	* another names for this method could be "emit", "invoke" or "fire"
	* @param String eventName
	* @param Mixed data (could be object, array, string or whatever), this data passed by publishers
	*/
	publish: function(eventName, data){
		if(!!this.subscriptions[eventName]){
			this.subscriptions[eventName].forEach(function(cb){
				cb(data);
			});
		}
	},
	/*
	* make (subscribers) unsubscribe from certain event
	* another name for this method could be "off"
	* @param String eventName
	* @param Function cb (will be spliced from eventName callback handlers)
	*/
	unsubscribe: function(eventName, cb){
		if(!!this.subscriptions[eventName]){
			var cbIndex = this.subscriptions[eventName].indexOf(cb);
			if(cbIndex != -1){
				this.subscriptions[eventName].splice(cbIndex, 1);
			}
		}
	}
};
```

</div>

هذا هو نمط الـ PubSub بكل بساطة، ربما تجد له بعض الاختلافات في طريقة تضمينه، أو كتابته، وربما أيضا تجد أسامي الوظائف الخاصة به محتلفة نوعا ما، كأن تجد مثلا الدالة subscribe اسمها on والدالة unsubscribe اسمها off، لكن في النهاية أنت قد وصلتك الفكرة من هذا النمط. والآن دعونا نتطرق إلى بعض الأمثلة لكي نتعرف على كيفية استخدام هذا النمط و الاستفادة منه.

### Usage 1

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
// callback handler
function moduleHandler(data){
	console.log('i am invoked with the following data');
	console.log(data);
}

// subscribe to eventX
PubSub.subscribe('eventX', moduleHandler);

// publish eventX
PubSub.publish('eventX', {
	some_keys: 'some values',
});

// unsubscribe to eventX
PubSub.unsubscribe('eventX', moduleHandler);
```

</div>

في المثال البسيط السابق قمنا بالاستماع إلى الحدث "eventX" وعندما تم عمل publish لهذا الحدث تم تنفيذ الدالة "moduleHandler"، ومن ثم قمنا بالغاء عملية الاستماع للحدث "evetX" في السطر الأخير.

### Usage 2

لو انتقلنا إلى مثال أخر حيث تخاطب بعض الـ modules مع بعضها من خلال هذا النمط، انظر معي إلى الصورة الآتية:-

![مخطط يوضح مثال على استخدام الـ PubSub Pattern بين الـ modules](./images/pubsub-javascript-example.png)

في هذا المثال سنحاول أن نجعل الـ modules تخاطب بعضها عبر الـ PubSub Pattern كما في الصورة، أولا هناك module اسمه UsersModule هذا الـ module سوف يقوم بعمل publish لحدث ما كلما كان هناك مستخدم جديد، أو تم مسح مستخدم، أو أي شيء من هذا القبيل. من الناحية الأخرى هناك الكثير من الـ modules التي تستمع لهذه الأحداث، وسوف نأخذ منهم module واحد ونكتب الأكواد الخاصة به، وأنت تستطيع أن تخمن أي modules أخرى. انظر مع إلى الكود الأتي:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var UsersCounterModule = (function(){
	var usersCount = 0;

	function updateUsersCounter(){
		console.log('users count is ' + usersCount);
	}

	function onNewUser(user){
		usersCount++;
		updateUsersCounter();
	}

	function onNewUsers(users){
		usersCount += users.length;
		updateUsersCounter();
	}

	function setupListeners(){
		PubSub.subscribe('newUser', onNewUser);
		//note: we could use namespacing to name the events
		PubSub.subscribe('UsersModule/newUsers', onNewUsers);
	}

	return{
		init: function(){
			setupListeners();
		}
	}

})();
```

</div>

هذا مجرد module يتعامل فقط مع عدد المستخدمين، طبعا هذه صورة مصغرة من الـ module، أنت تستطيع أن تستنتج جميع الوظائف الخاصة بهذا الـ module. وكما ترى هذا الـ module مستقل بذاته بشكل كبير جدا، هو لا يتداخل ولا يتشابك مع modules أخرى، سوى أنه يتعامل فقط مع الـ PubSub، غير هذا هو لا يعتمد بشكل مباشر على modules أخرى. فكما ترى كلما يتم عمل publish لأي من الأحداث الذي يستمع إليها، أيا ما كان الذي سوف يقوم بعمل publish لهذه الأحداث، سيقوم هذا الـ module بالوظائف المترتبة على الأحداث.

والآن انظر معى الى الـ module الخاص بالمستخدمين والذي سوف يقوم بعمل publish لبعض الأحداث التي لها علاقة بالمستخدمين:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
var UsersModule = (function(){

	function getUsersFromDB(){
		// do some stuff + publish event
		var users = [{}, {}];
		//note: we could use namespacing to name the events
		PubSub.publish('UsersModule/newUsers', users);
	}

	function getUserFromDB(userId){
		// do some stuff + publish event
		var user = {};
		PubSub.publish('newUser', user);
	}

	return{
		init: function(){
			getUsersFromDB();
		},
		getUserFromDB: getUserFromDB
	}

})();
```

</div>

طبعا هذا الـ module لا يعرف شيء عن الـ modules الأخرى، فهو لا يتداخل ولا يتشابك مع modules أخرى، كل ما في الأمر أنه سوف يقوم بعمل publish لبعض الأحداث عن طريق الـ PubSub. ومن يهمه هذه الأحداث من الـ moduels الأخرى يعمل لها subscribe.

في الكود الآتي سوف نقوم بعمل init للـ modules:-

<div dir="ltr" style="text-align: left;" markdown="1">

```javascript
UsersCounterModule.init();
UsersModule.init();
```

</div>

طبعا عملية الـ implementation من الممكن أن تختلف نوعا ما من كود إلى كود أخر، لكن في النهاية الفكرة واحدة، وقد قمنا بإعطاء بعض الأمثلة على هذا، وأنت ربما تضمن هذا النمط بطريقة مختلفة نوعا ما حسب ما يخدم الحالة التي أمامك. أيا ما كان طريقة التضمين، ستكون هناك الكثير من المميزات من وراء استخدام مثل هذا النمط، فكما قولنا سابقا أن هذا النمط يساعدنا في الحصول على أكواد نظيفة، وقابلة لإعادة الاستخدام، ويمكن تطوير الـ modules يالتوازي، بمعني؛ أن كل فريق يمكنه أن يعمل على الـ module الخاص به في التطبيق أو النظام دون انتظار أو الاعتماد على الفرق الأخرى، لأن -كما ترى- كل module مستقل إلى حد كبير عن باقي الـ modules الأخرى.

في النهاية، لا يمكن أن يخلو أي تطبيق قوي أو نظام كبير من مبدأ الـ code decoupling، أيا ما كان طريقة التنفيذ أو النمط المتبع، حيث يوجد أنماط مختلفة لتحقيق هذا المبدأ. ويعد نمط الـ PubSub واحدا من الأنماط التي تساعدنا على تحقيق هذا المبدأ، وثم الحصول على أكواد نظيفة مرتبة قابلة لإعادة الاستخدام.

</div>
