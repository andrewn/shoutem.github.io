---
layout: doc
permalink: /docs/ui-toolkit/components/lightbox
title: Lightbox
section: UI toolkit
---

# Lightbox

`Lightbox` is Shoutem's fork of [`react-native-lightbox`](https://github.com/oblador/react-native-lightbox) component.  
All content within `Lightbox` is transformable, meaning it can be pinched to zoom, panned (translated), etc. 

## Lightbox
![Lightbox example](https://cloud.githubusercontent.com/assets/378279/9074360/16eac5d6-3b09-11e5-90af-a69980e9f4be.gif "Lightbox"){:.docs-component-image}

#### JSX Declaration
```JSX
<Lightbox>
  <Image
    style={% raw %}{{ height: 300 }}{% endraw %}
    source={% raw %}{{ uri: 'http://knittingisawesome.com/wp-content/uploads/2012/12/cat-wearing-a-reindeer-hat1.jpg' }}{% endraw %}
  />
</Lightbox>
```

#### Props

* **activeProps** : object  
  - Optional set of props applied to the content component when in lightbox (open) mode. Use for applying custom styles or higher resolution image source.
* **renderHeader(close)** : function
  - Function that should return custom header instead of default with X button in upper left corner. 
* **renderContent** : function
  - Function that should return custom `Lightbox` content instead of default child content.
* **onClose** : function
  - Triggered when `Lightbox` is closed.
* **onOpen** : function
  - Triggered when `Lightbox` is opened.
* **underlayColor** : string
  - Defines color of touchable background, defaults to `black`.
* **backgroundColor** : string
  - defines color of `Lightbox` background, defaults to `black`.
* **DwipeToDismiss** : bool
  - Enables gestures to dismiss the fullscreen mode by swiping up or down, defaults to `true`.
* **pinchToZoom** : bool
  - Enables pinch to zoom functionality on the fullscreen content, defaults to `true`.
* **springConfig** : object
  - [`Animated.spring`](https://facebook.github.io/react-native/docs/animations.html) configuration, defaults to `{ tension: 30, friction: 7 }`  

#### Style names

* None  
