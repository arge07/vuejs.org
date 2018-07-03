---
title: Başlarken
type: guide
order: 2
---

## Vue.js nedir?

Vue (pronounced /vjuː/, like **view**) is a **progressive framework** for building user interfaces. Unlike other monolithic frameworks, Vue is designed from the ground up to be incrementally adoptable. The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects. On the other hand, Vue is also perfectly capable of powering sophisticated Single-Page Applications when used in combination with [modern tooling](single-file-components.html) and [supporting libraries](https://github.com/vuejs/awesome-vue#components--libraries).

If you’d like to learn more about Vue before diving in, we <a id="modal-player"  href="javascript:;">created a video</a> walking through the core principles and a sample project.

If you are an experienced frontend developer and want to know how Vue compares to other libraries/frameworks, check out the [Comparison with Other Frameworks](comparison.html).

## Başlarken

<p class="tip">Resmi Rehber orta düzey HTML, CSS ve JavaScript bildiğinizi varsayar. Yeni başlıyorsanız önce temel kavramları öğrenip tekrar geri gelmenizi tavsiye ederiz. Diğer frameworkler ile uğraşmış olmanız size katkı sağlar fakat bilmek zorunda değilsiniz..</p>

 [JSFiddle Hello World örneği](https://jsfiddle.net/chrisvfritz/50wL7mdz/) Vue.js en kolay deneme yoludur. Basit örnekleri yaparken orda yeni sekme açıp denemekten çekinmeyin. İsterseniz; <a href="https://gist.githubusercontent.com/chrisvfritz/7f8d7d63000b48493c336e48b3db3e52/raw/ed60c4e5d5c6fec48b0921edaed0cb60be30e87c/index.html" target="_blank" download="index.html"><code>index.html</code> dosyasını oluşturup</a> içine Vue kaynağını ekleyerek başlayabilirsiniz:

``` html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

 Diğer seçenekler için [Kurulum](installation.html) sayfasına bakınız. Not: Node.js temeliniz yoksa `vue-cli` ile başlamanızı **önermiyoruz**.

## Render Etme

 Vue.js çekirdeği doğrudan DOM işlemlerine odaklanan; basit bir template yazımına sahiptir.

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Merhaba Vue!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Merhaba Vue!'
  }
})
</script>
{% endraw %}

Böylelikle ilk Vue uygulamamızı yazmış olduk. Basitçe template render işine benzesede, Vue arkaplanda çok fazla iş yaptı. Veri ve DOM bir biri ile bağlantılı olarak ve birbiri ile **etkileşimli** olarak çalıştı. Peki bunu nasıl bilebiliriz? Javascript Konsolu açın (bu sayfada sağ tık yapın ve ögeyi inceleye tıklayın) ve `app.message` değişkenine bir değer atayın(Ör: `app.message = 'Vue.js'`). Hızlıca verinin ve çıktının değiştiğini göreceksiniz.

Metin içine bir değişken eklemiz gerektiğinde ne yapmamız gerektiğine bir bakalım. 

``` html
<div id="app-2">
  <span v-bind:title="message">
    Fare ile bu alanın üzerine gelip bir kaç saniye beklediğinizde 
    dinamik olarak yerleştirlen title özelliğinin değerini göreceksiniz.
  </span>
</div>
```
``` js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Sayfanın yüklenme zamanı: ' + new Date().toLocaleString()
  }
})
```
{% raw %}
<div id="app-2" class="demo">
  <span v-bind:title="message">
    Fare ile bu alanın üzerine gelip bir kaç saniye beklediğinizde 
    dinamik olarak yerleştirlen title özelliğinin değerini göreceksiniz.
  </span>
</div>
<script>
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Sayfanın yüklenme zamanı: ' + new Date().toLocaleString()
  }
})
</script>
{% endraw %}

Burada yeni birşey gördik: `v-bind`. v-bind özelliği gördüğümüzde aklımıza bir **direktifin(directive)** çağrıldığı gelmelidir. Vuede direktifler `v-` ile başlar. Tahmin edersiniz ki bu direktifler vue içinde özel olarak DOM render etmek için yapılandırılmıştır. Basitçe anlatmak gerekirse; "v-bind; `title` özelliğinin `message` değişkeni/parametresi ile birlikte değişeceğini göstermektedir.";

Tekrar Javascript Konsolunu açıp `app2.message = 'yeni mesaj'` olarak çalıştırırsanız, tekrar fare ile üzerine geldiğinizde `title` özelliğinin değiştiğini/güncellendiğini görebilirsiniz.

## Koşullar ve Döngüler

Basitçe elementi ekleme ve kaldırma yapalım:

``` html
<div id="app-3">
  <span v-if="seen">Şimdi beni görüyorsun</span>
</div>
```

``` js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

{% raw %}
<div id="app-3" class="demo">
  <span v-if="seen">Şimdi beni görüyorsun</span>
</div>
<script>
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
</script>
{% endraw %}

Tekrar konsola gidip `app3.seen = false` kod parçasını çalıştırısanız span etiketiyle birlikte metnin kaldırıldığı göreceksiniz.

Bu örnek bize; etkileşimin sadece veri ve özellik bazlı değil aynı zamanda **DOM elementleri** içinde geçerli olduğunu gösteriyor. Dahası; elementlerin eklenmesi/güncellenmesi/silinmesinde [geçiş efektleri](transitions.html) otomatik sağlanır.

Her biri kendine özgü özelliğe sahip bir kaç direktif daha var. 
There are quite a few other directives, each with its own special functionality. For example, the `v-for` directive can be used for displaying a list of items using the data from an Array:

``` html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
``` js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```
{% raw %}
<div id="app-4" class="demo">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
</script>
{% endraw %}

In the console, enter `app4.todos.push({ text: 'New item' })`. You should see a new item appended to the list.

## Handling User Input

To let users interact with your app, we can use the `v-on` directive to attach event listeners that invoke methods on our Vue instances:

``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app-5" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Note that in this method we update the state of our app without touching the DOM - all DOM manipulations are handled by Vue, and the code you write is focused on the underlying logic.

Vue also provides the `v-model` directive that makes two-way binding between form input and app state a breeze:

``` html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
{% endraw %}

## Composing with Components

The component system is another important concept in Vue, because it's an abstraction that allows us to build large-scale applications composed of small, self-contained, and often reusable components. If we think about it, almost any type of application interface can be abstracted into a tree of components:

![Component Tree](/images/components.png)

In Vue, a component is essentially a Vue instance with pre-defined options. Registering a component in Vue is straightforward:

``` js
// Define a new component called todo-item
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})
```

Now you can compose it in another component's template:

``` html
<ol>
  <!-- Create an instance of the todo-item component -->
  <todo-item></todo-item>
</ol>
```

But this would render the same text for every todo, which is not super interesting. We should be able to pass data from the parent scope into child components. Let's modify the component definition to make it accept a [prop](components.html#Props):

``` js
Vue.component('todo-item', {
  // The todo-item component now accepts a
  // "prop", which is like a custom attribute.
  // This prop is called todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Now we can pass the todo into each repeated component using `v-bind`:

``` html
<div id="app-7">
  <ol>
    <!--
      Now we provide each todo-item with the todo object
      it's representing, so that its content can be dynamic.
      We also need to provide each component with a "key",
      which will be explained later.
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id">
    </todo-item>
  </ol>
</div>
```
``` js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item" :key="item.id"></todo-item>
  </ol>
</div>
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
</script>
{% endraw %}

This is a contrived example, but we have managed to separate our app into two smaller units, and the child is reasonably well-decoupled from the parent via the props interface. We can now further improve our `<todo-item>` component with more complex template and logic without affecting the parent app.

In a large application, it is necessary to divide the whole app into components to make development manageable. We will talk a lot more about components [later in the guide](components.html), but here's an (imaginary) example of what an app's template might look like with components:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### Relation to Custom Elements

You may have noticed that Vue components are very similar to **Custom Elements**, which are part of the [Web Components Spec](https://www.w3.org/wiki/WebComponents/). That's because Vue's component syntax is loosely modeled after the spec. For example, Vue components implement the [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) and the `is` special attribute. However, there are a few key differences:

1. The Web Components Spec is still in draft status, and is not natively implemented in every browser. In comparison, Vue components don't require any polyfills and work consistently in all supported browsers (IE9 and above). When needed, Vue components can also be wrapped inside a native custom element.

2. Vue components provide important features that are not available in plain custom elements, most notably cross-component data flow, custom event communication and build tool integrations.

## Ready for More?

We've briefly introduced the most basic features of Vue.js core - the rest of this guide will cover them and other advanced features with much finer details, so make sure to read through it all!

<div id="video-modal" class="modal"><div class="video-space" style="padding: 56.25% 0 0 0; position: relative;"><iframe src="https://player.vimeo.com/video/247494684" style="height: 100%; left: 0; position: absolute; top: 0; width: 100%; margin: 0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div><script src="https://player.vimeo.com/api/player.js"></script></div>
