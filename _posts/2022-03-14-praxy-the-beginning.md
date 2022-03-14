---
layout: post
title:  "Praxy - The beginning"
date:   2022-03-14
---

It's been some time since the last post, if you can call it a "real post". [Decide for yourself](/2021/10/26/wip.html).

I promise I will be more active on both Twitter and this blog from now on. I have more freetime,
and I have found a project which I am really keen on telling about.

In fact, that is what I am going to tell you about today: Praxy &mdash; A reasonable library for building web UI's without all the hassle.

## Another JS library. Why?

I know, I know - there's plenty already, but, as a software engineer 
with primary focus on frontend I have tried a wide range of frontend frameworks and libraries.
All of them seem to do a couple of things very well, but tend to grow way to big and focus on too many things.

What I've always wanted was a small and lightweight library, with a simple and easy to understand API. This is where I feel most 
other libraries that I've used tend to fail over time.

In short, what I want from a library is:

- Utilize natively supported browser API's
- Quick and easy reactivity
  - Input events: click, input etc.
  - Embedded values in the DOM: `Hello {%raw%}{{value}}{%endraw%}!`
- Little to no business logic defined in HTML
  - Define your HTML, and add sugar on top.
  - Don't let HTML turn into something it isn't.
- First-class Typescript support

## First draft Component API

With that mindset I decided to start building what I think would be a dream library for me. I decided to go with component based 
since that is easy to understand, and a great way to make fractions of code work together.

This is how I structure my projects when using Praxy:

`app.ts` would export your instance of Praxy.<br />
`index.ts` would export all of your components, and would be you bundlers entry point.

```bash
src/
- app.ts
- index.ts
components/
  - component.ts
  - anotherComponent.ts
```

Below is a very early example of the way you structure a component with Praxy:

{% raw %}
```typescript
const App = new Praxy();

/**
 * The component will change {{entity}} to "World!"
 */
const MyComponent = {
  name: 'myComponent',
  template: `
    <div>Hello {{entity}}. My name is {{name}}.</div>
    <div>
      <button class="button">Click me!</button>
    </div>
    <div>
      <input name="input-1" />
    </div>
  `,
  data: {
    entity: 'World!',
    name: 'Peter',
  },
};

App
  .component(MyComponent)
  .on('click', '.button', ({self}) => {
    /**
     * When .button is clicked we set `entity`
     * and "World!" will now be changed to "GitHub!"
     */
    self.set('entity', 'GitHub!');
  })
  .on('input', '[name="input-1"]', ({self, target}) => {
    /**
     * When typing in [name="input-1"] `name` would change,
     * and "Peter" will change to the value of the input field.
     * `target` is the HTMLElement you bind the `on` event to.
     */
    self.set('name', target.value);
  });
```
{% endraw %}

The way you chain together different actions and events in Praxy forces you to keep your components neat, and 
hopefully makes you think in the direction of [atomic design](https://bradfrost.com/blog/post/atomic-web-design/).
