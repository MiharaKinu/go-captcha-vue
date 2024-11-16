# Go Captcha Vue Package

<p> ⭐️ If it helps you, please give a star.</p>
<img src="http://47.104.180.148/go-captcha/go-captcha-v2.jpg" alt="Poster">


<br/>

| Vue Version            | Go Captcha Version |
|:-----------------------|:------------------:|
| vue >= 2.7.14 && < 3.0 | go-captcha-vue@^1  |
| vue >= 3.0             | go-captcha-vue@^2  |

## Install
```shell
# Greater than or equal to vue2.7.14 and less than vue3.0
yarn add go-captcha-vue@^1
# or
npm install go-captcha-vue@^1
# or
pnpm install go-captcha-vue@^1

############################################
# Greater than vue3.0
yarn add go-captcha-vue@^2
# or
npm install go-captcha-vue@^2
# or
pnpm install go-captcha-vue@^2
```

Use Go Captcha
```ts
import "go-captcha-vue/dist/style.css"
import GoCaptcha from "go-captcha-vue"

Vue.use(GoCaptcha)
```

## 🖖 Click Mode Captcha
```vue
<gocaptcha-click
  :config="{}"
  :data="{}"
  :events="{}"
/>
```

### Parameter Reference
```ts
// config = {}
interface ClickConfig {
  width?: number;
  height?: number;
  thumbWidth?: number;
  thumbHeight?: number;
  verticalPadding?: number;
  horizontalPadding?: number;
  showTheme?: boolean;
  title?: string;
  buttonText?: string;
  iconSize?: number;
  dotSize?: number;
}

// data = {}
interface ClickData {
  image: string;
  thumb: string;
}

// events = {}
interface ClickEvents {
  click?: (x: number, y: number) => void;
  refresh?: () => void;
  close?: () => void;
  confirm?: (dots: Array<ClickDot>, reset:() => void) => void;
}

// component call method
interface ClickExpose {
  reset: Function,
  clear: Function,
  refresh: Function,
  close: Function,
}
```

## 🖖 Slide Mode Captcha
```vue
<gocaptcha-slide
  :config="{}"
  :data="{}"
  :events="{}"
/>

<gocaptcha-slide-region
  :config="{}"
  :data="{}"
  :events="{}"
/>
```
### Parameter Reference
```ts
// config = {}
interface SlideConfig {
  width?: number;
  height?: number;
  thumbWidth?: number;
  thumbHeight?: number;
  verticalPadding?: number;
  horizontalPadding?: number;
  showTheme?: boolean;
  title?: string;
  scope ?: boolean;
}

// data = {}
interface SlideData {
  thumbX: number;
  thumbY: number;
  thumbWidth: number;
  thumbHeight: number;
  image: string;
  thumb: string;
}

// events = {}
interface SlideEvents {
  move?: (x: number, y: number) => void;
  refresh?: () => void;
  close?: () => void;
  confirm?: (point: SlidePoint, reset:() => void) => void;
}

// component call method
interface SlideExpose {
  reset: Function,
  clear: Function,
  refresh: Function,
  close: Function,
}
```
```ts
// config = {}
interface SlideRegionConfig {
  width?: number;
  height?: number;
  thumbWidth?: number;
  thumbHeight?: number;
  verticalPadding?: number;
  horizontalPadding?: number;
  showTheme?: boolean;
  title?: string;
  scope ?: boolean;
}

// data = {}
interface SlideRegionData {
  thumbX: number;
  thumbY: number;
  thumbWidth: number;
  thumbHeight: number;
  image: string;
  thumb: string;
}

// events = {}
interface SlideRegionEvents {
  move?: (x: number, y: number) => void;
  refresh?: () => void;
  close?: () => void;
  confirm?: (point: SlideRegionPoint, reset:() => void) => void;
}

// component call method
interface SlideRegionExpose {
  reset: Function,
  clear: Function,
  refresh: Function,
  close: Function,
}
```

## 🖖 Rotate Mode Captcha
```vue
<gocaptcha-rotate
  :config="{}"
  :data="{}"
  :events="{}"
/>
```

### Parameter Reference
```ts
// config = {}
interface RotateConfig {
  width?: number;
  height?: number;
  thumbWidth?: number;
  thumbHeight?: number;
  verticalPadding?: number;
  horizontalPadding?: number;
  showTheme?: boolean;
  title?: string;
  scope ?: boolean;
}

// data = {}
interface RotateData {
  angle: number;
  image: string;
  thumb: string;
}

// events = {}
interface RotateEvents {
  rotate?: (angle: number) => void;
  refresh?: () => void;
  close?: () => void;
  confirm?: (angle: number, reset:() => void) => void;
}

// component call method
interface RotateExpose {
  reset: Function,
  clear: Function,
  refresh: Function,
  close: Function,
}
```


## Button
```vue
<gocaptcha-button @clickEvent="() => console.log('hello')"/>
```

### params
```ts
interface $Attr {
  config?: CaptchaConfig;
  disabled?: boolean;
  type?: "default" | "warn" | "error" | "success";
  title?: string;
}

interface $Event {
  clickEvent?: ()=>void;     // event -> @clickEvent=""
}

export interface ButtonConfig {
  width?: number;
  height?: number;
  verticalPadding?: number;
  horizontalPadding?: number;
}

```

<br/>

## 👍 Sponsor
<div>
<a href="http://gocaptcha.wencodes.com/sponsor/" target="_blank">http://gocaptcha.wencodes.com/sponsor/</a>
</div>
<br/>
