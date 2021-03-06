---
layout: doc
permalink: /docs/ui-toolkit/components/video
title: Video
section: UI toolkit
---

# Video

`Video` component can be used to render all types of video items.  
It renders a Video based on the source type. If the source is `URL` to a web player, video is displayed in a `WebView`. If the source is a video stream url, a `video` `HTML` element is displayed in the `WebView`. The component does not support playback of local video files.

## Video
![Video example]({{ site.baseurl }}/img/ui-toolkit/video/video_player@2x.png "Video"){:.docs-component-image}

#### JSX Declaration
```JSX
<Video
    source={...}
    height={...}
    width={...}
    style={...}
/>
```

#### Props

* **source** : string
  - Prop that defines the source of the video that will be rendered

* **height** : number
  - Prop that sets the height of the container where the video preview thumbnail will be rendered
   
* **width** : number
  - Prop that sets the width of the container where the video preview thumbnail will be rendered


#### Style

* **container**
  - Style prop for container `View` that holds the `Video` component
