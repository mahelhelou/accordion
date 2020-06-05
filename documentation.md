# Documentation

## Basic Template

### HTML template

```html
<div class="container">
  <div id="media-list">
    <h1>{{ title }}</h1>
    <select>
      <option>Select a type of media...</option>
    </select>
    <ul>
      <li v-for="media in mediaList" @click="toggleDetails(media)">
        <h3>{{ media.title }}</h3>
        <div v-show="media.showDetail">
          <p>{{ media.description }}</p>
          <p class="byline">{{ media.contributor }}</p>
        </div>
      </li>
    </ul>
  </div>
</div>
```

### CSS code

```css
body {
  font-family: 'Open Sans', sans-serif;
  background: #f4f4f4;
}

.container {
  max-width: 80%;
  margin: auto;
  min-height: initial;
  position: relative;
}

.container > div {
  padding-bottom: 30px;
}

h3 {
  cursor: pointer;
  display: inline;
}

p {
  color: #333;
}

.more:after {
  content: '\25BC';
  font-size: 8px;
  position: absolute;
  right: 20px;
  top: 50%;
  margin-top: -10px;
  font-size: 16px;
  color: #333;
}

.less:after {
  content: '\25B2';
  font-size: 8px;
  position: absolute;
  right: 20px;
  top: 50%;
  margin-top: -10px;
  font-size: 16px;
  color: #333;
}

ul {
  list-style-type: none;
  padding-left: 0;
  border-radius: 5px;
}

li {
  border: #eaeaea solid 1px;
  margin: 10px 0;
  padding: 15px;
  border-left: #95effe solid 5px;
  position: relative;
  background: #fff;
  -webkit-box-shadow: 0 1px 2px rgba(0, 0, 0, 0.07);
  -moz-box-shadow: 0 1px 2px rgba(0, 0, 0, 0.07);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.07);
}

li.book {
  border-left: #eb727e solid 5px;
}

li.streaming {
  border-left: #a0f2d0 solid 5px;
}

.byline {
  font-size: 14px;
  font-style: italic;
}

select {
  position: absolute;
  right: 0;
  top: 5px;
  -webkit-appearance: button;
  -webkit-border-radius: 2px;
  -webkit-box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.1);
  -webkit-padding-end: 20px;
  -webkit-padding-start: 2px;
  -webkit-user-select: none;
  background-image: url(img/arrow.png),
    -webkit-linear-gradient(#fff, #fff 40%, #fff);
  background-position: 97% center;
  background-repeat: no-repeat;
  border: 1px solid #eaeaea;
  color: #555;
  font-size: inherit;
  overflow: hidden;
  padding: 5px 10px;
  text-overflow: ellipsis;
  white-space: nowrap;
  width: 300px;
  outline: none;
}
```

### JS code

```js
const media = [
  {
    title: 'Hop on Pop',
    description: "A delightful children's book.",
    type: 'book',
    contributor: 'Dr. Suess',
    showDetail: false
  },
  {
    title: 'The Joy of Painting',
    description: 'Create a world of happy little trees!',
    type: 'DVD',
    contributor: 'Bob Ross',
    showDetail: false
  },
  {
    title: 'Supernatural: The Complete 12th Season',
    description: 'A (literally) neverending roadtrip.',
    type: 'DVD',
    contributor: '',
    showDetail: false
  },
  {
    title: 'Planet Earth II',
    description: 'Hours of beautiful but heart attack-inducing nature footage',
    type: 'streaming video',
    contributor: 'David Attenborough',
    showDetail: false
  },
  {
    title: 'Titanic',
    description: 'The boat sinks.',
    type: 'DVD',
    contributor: 'James Cameron',
    showDetail: false
  },
  {
    title: 'The Sirens of Titan',
    description:
      'Mankind flung its advance agents ever outward, ever outward... it flung them like stones.',
    type: 'book',
    contributor: 'Kurt Vonnegut',
    showDetail: false
  },
  {
    title: 'Better Call Saul',
    description: 'A slow-burning Breaking Bad prequel.',
    type: 'streaming video',
    contributor: '',
    showDetail: false
  },
  {
    title: 'Title 1',
    description: 'A slow-burning Breaking Bad prequel.',
    type: 'e-book',
    contributor: '',
    showDetail: false
  },
  {
    title: 'Title 2',
    description: 'A slow-burning Breaking Bad prequel.',
    type: 'e-book',
    contributor: '',
    showDetail: false
  }
]

const app = new Vue({
  el: '#media-list',
  data: {
    title: 'Treehouse Public Library',
    mediaList: media
  },
  methods: {
    toggleDetails: function(media) {
      // Know which media we've clicked on!
      console.log(media)
      return (media.showDetail = !media.showDetail)
    }
  }
})
```

## Hide `By:` when there's no contributor for the media

```html
<li v-for="media in mediaList" @click="toggleDetails(media)">
  <h3>{{ media.title }}</h3>
  <div v-show="media.showDetail">
    <p>{{ media.description }}</p>
    <p v-if="media.contributor" class="byline">By: {{ media.contributor }}</p>
  </div>
</li>
```

## Show only media of a specific type

### Make options list of all media types

```html
<select>
  <option>Select a type of media...</option>
  <option v-for="item in mediaList">{{ item.type }}</option>
</select>
```

### Create a `filterList` function to return specific type

- Add function to every option selected in DOM
- After creating the function in `app.js`, create

```html
<!-- Only related code included -->
<select @change="filterList">
  <option>Select a type of media...</option>
  <option v-for="item in mediaList">{{ item.type }}</option>
</select>
<!-- Next code -->
<li
  v-show="type === media.type"
  v-for="media in mediaList"
  @click="toggleDetails(media)"
>
  ...
</li>
```

- Creating function to filter the list of media according to type

```js
methods: {
   // ...

   // In vanilla js, you have to pass event as param, but in Vue, you don't
   filterList: function () {
      // console.log(event.target.value)
      this.type = event.target.value
   }
}
```

## Remove select menu duplicates

1. Write a computed method that returns a brand new array of media-types

```js
// Computed: feature that helps to perform complex calculations or operations that affect data prop in a way that will be displayed
// Computed prop is automatically updated & cached to browser
computed: {
  uniqueMediaTypes: function () {
    const mediaTypes = []
    this.mediaList.forEach(media => {
      if (!mediaTypes.includes(media.type)) {
        mediaTypes.push(media.type)
      }
    })
    return mediaTypes
  }
}
```

2. Change select options reference to reflect the new unique list

```diff
<option v-for="item in mediaList">{{ item.type }}</option>
```

```html
<select @change="filterList">
  <option>Select a type of media...</option>
  <option v-for="mediaType in uniqueMediaTypes">{{ mediaType }}</option>
</select>
```

## Show all elements if no option has been selected

1. Make initial empty value for `select` menu:

```html
<select value="" @change="filterList">
  <option>Select a type of media...</option>
  <option v-for="mediaType in uniqueMediaTypes">{{ mediaType }}</option>
</select>
```

2. Update the `v-show` directive to show all media if no option has been selected, and show only media types for specific media type option:

```html
<li
  v-show="type === '' || type === media.type"
  v-for="media in mediaList"
  @click="toggleDetails(media)"
>
  ...
</li>
```

## Showing Up & Down arrows on toggle details

1. Bind classes for each state of list item, if details are shown or hidden:

```html
<li
  v-show="type === '' || type === media.type"
  v-for="media in mediaList"
  @click="toggleDetails(media)"
  :class="{'less': media.showDetail, 'more': !showDetail}"
></li>

<!-- Note that the classes rendered as object. If details of media, render 'less' class, if not, render 'more' class -->
```

- You can use `terneray` if statement to bind classes instead of using regular javascript object:

```html
:class="media.showDetail ? 'less' : 'more'"
```

## Coloring each media type

```html
:class="[media.showDetail ? 'less' : 'more', media.type]"
<!-- 'less' or 'more' && 'book' or another media type -->
```

- And we can split classes or media types & make all as lower-case:

```html
:class="[media.showDetail ? 'less' : 'more', media.type.split(' ')[0].toLowerCase()]"
```