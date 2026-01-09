# SpinePlayerConfig 配置说明

`SpinePlayerConfig` 是 Spine Web Player 的配置接口，用于控制 Spine 动画播放器的各种行为和显示选项。

## 目录

- [资源加载配置](#资源加载配置)
- [动画配置](#动画配置)
- [皮肤配置](#皮肤配置)
- [视口配置](#视口配置)
- [渲染配置](#渲染配置)
- [控制配置](#控制配置)
- [交互控制](#交互控制)
- [回调函数](#回调函数)
- [高级配置](#高级配置)

---

## 资源加载配置

### jsonUrl
- **类型**: `string`（可选）
- **说明**: 骨骼 JSON 文件（.json）的 URL 地址
- **注意**: 如果提供了 `binaryUrl`，则此项应为 undefined
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas"
}
```

### jsonField
- **类型**: `string`（可选）
- **说明**: JSON 文件中包含骨骼数据的字段名称
- **默认值**: 无
- **使用场景**: 当 JSON 文件包含多个数据对象，需要指定具体的骨骼数据字段时使用
- **示例**:
```javascript
{
  jsonUrl: "assets/data.json",
  jsonField: "skeletonData",
  atlasUrl: "assets/skeleton.atlas"
}
```

### binaryUrl
- **类型**: `string`（可选）
- **说明**: 骨骼二进制文件（.skel）的 URL 地址
- **注意**: 如果提供了 `jsonUrl`，则此项应为 undefined
- **优势**: 二进制格式加载速度更快，文件体积更小
- **示例**:
```javascript
{
  binaryUrl: "assets/skeleton.skel",
  atlasUrl: "assets/skeleton.atlas"
}
```

### atlasUrl
- **类型**: `string`（必需）
- **说明**: 骨骼图集文件（.atlas）的 URL 地址
- **注意**: 图集页面图像会自动解析
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas"
}
```

### rawDataURIs
- **类型**: `StringMap<string>`（可选）
- **说明**: 原始数据 URI 映射，将路径映射到 base64 编码的原始数据
- **默认值**: 无
- **使用场景**: 允许将资源直接嵌入到 HTML/JS 中，避免额外的网络请求
- **工作原理**: 当播放器的资源管理器解析 jsonUrl、binaryUrl、atlasUrl 或图集中引用的图像路径时，会首先在原始数据 URI 中查找该路径
- **示例**:
```javascript
{
  jsonUrl: "skeleton.json",
  atlasUrl: "skeleton.atlas",
  rawDataURIs: {
    "skeleton.json": "data:application/json;base64,eyJ...",
    "skeleton.atlas": "data:text/plain;base64,CnNrZWx...",
    "skeleton.png": "data:image/png;base64,iVBORw0KG..."
  }
}
```

---

## 动画配置

### animation
- **类型**: `string`（可选）
- **说明**: 要播放的动画名称
- **默认值**: 空动画
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  animation: "walk"
}
```

### animations
- **类型**: `string[]`（可选）
- **说明**: 用户可以选择的动画名称列表
- **默认值**: 所有动画
- **使用场景**: 限制播放器控件中显示的动画选项
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  animation: "walk",
  animations: ["walk", "run", "jump"]
}
```

### defaultMix
- **类型**: `number`（可选）
- **说明**: 在两个动画之间切换时使用的默认混合时间（秒）
- **默认值**: 0.25
- **作用**: 控制动画过渡的平滑程度
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  animation: "walk",
  defaultMix: 0.5  // 0.5秒的过渡时间
}
```

---

## 皮肤配置

### skin
- **类型**: `string`（可选）
- **说明**: 要设置的皮肤名称
- **默认值**: 默认皮肤
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  skin: "blue-skin"
}
```

### skins
- **类型**: `string[]`（可选）
- **说明**: 用户可以选择的皮肤名称列表
- **默认值**: 所有皮肤
- **使用场景**: 限制播放器控件中显示的皮肤选项
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  skin: "red",
  skins: ["red", "blue", "green"]
}
```

---

## 视口配置

### viewport
- **类型**: `object`（可选）
- **说明**: 在骨骼世界坐标系中视口的位置和大小
- **默认值**: 适合当前动画的边界框，10% 填充，0.25 秒过渡时间

#### viewport.x, viewport.y, viewport.width, viewport.height
- **类型**: `number`（可选）
- **说明**: 视口在骨骼世界坐标系中的位置和大小
- **默认值**: 适合当前动画的边界框

#### viewport.padLeft, viewport.padRight, viewport.padTop, viewport.padBottom
- **类型**: `string | number`（可选）
- **说明**: 视口周围的填充，可以是数字或百分比（例如 "25%"）
- **默认值**: "10%"
- **示例**:
```javascript
{
  viewport: {
    padLeft: "15%",
    padRight: "15%",
    padTop: 50,      // 绝对值
    padBottom: 50
  }
}
```

#### viewport.debugRender
- **类型**: `boolean`（可选）
- **说明**: 是否绘制显示视口边界的线条
- **默认值**: false
- **使用场景**: 调试视口设置时使用
- **示例**:
```javascript
{
  viewport: {
    debugRender: true
  }
}
```

#### viewport.transitionTime
- **类型**: `number`（可选）
- **说明**: 当前视口更改时，过渡到新视口的动画时间（秒）
- **默认值**: 0.25
- **示例**:
```javascript
{
  viewport: {
    transitionTime: 0.5
  }
}
```

#### viewport.animations
- **类型**: `StringMap<Viewport>`（可选）
- **说明**: 特定动画的视口配置
- **默认值**: 无
- **使用场景**: 为不同的动画设置不同的视口
- **示例**:
```javascript
{
  viewport: {
    animations: {
      "walk": {
        x: 0,
        y: 0,
        width: 800,
        height: 600,
        padLeft: "10%",
        padRight: "10%",
        padTop: "10%",
        padBottom: "10%"
      },
      "jump": {
        x: 0,
        y: 100,
        width: 800,
        height: 800,
        padLeft: "5%",
        padRight: "5%",
        padTop: "20%",
        padBottom: "5%"
      }
    }
  }
}
```

---

## 渲染配置

### premultipliedAlpha
- **类型**: `boolean`（可选）
- **说明**: 骨骼的图集图像是否使用预乘 alpha
- **默认值**: true
- **注意**: 必须与 Spine 编辑器中导出设置匹配
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  premultipliedAlpha: true
}
```

### alpha
- **类型**: `boolean`（可选）
- **说明**: 画布是否透明，允许网页背景在画布后面透过显示（当 backgroundColor 的 alpha < ff 时）
- **默认值**: false
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  alpha: true,
  backgroundColor: "#00000000"  // 完全透明
}
```

### preserveDrawingBuffer
- **类型**: `boolean`（必需）
- **说明**: 是否保留绘图缓冲区
- **默认值**: false
- **使用场景**: 如果需要通过 canvas.toDataURL() 截图，则需要设置为 true
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  preserveDrawingBuffer: true
}
```

### backgroundColor
- **类型**: `string`（可选）
- **说明**: 画布背景颜色，格式为 #rrggbb 或 #rrggbbaa
- **默认值**: #000000ff（黑色）或当 alpha 为 true 时为 #00000000（透明）
- **示例**:
```javascript
{
  backgroundColor: "#cccccc"      // 灰色背景
}
// 或带透明度
{
  alpha: true,
  backgroundColor: "#ffffff80"    // 半透明白色
}
```

### fullScreenBackgroundColor
- **类型**: `string`（可选）
- **说明**: 全屏模式下使用的背景颜色，格式为 #rrggbb 或 #rrggbbaa
- **默认值**: backgroundColor 的值
- **示例**:
```javascript
{
  backgroundColor: "#cccccc",
  fullScreenBackgroundColor: "#000000"  // 全屏时使用黑色背景
}
```

### backgroundImage
- **类型**: `object`（可选）
- **说明**: 在骨骼后面绘制的图像
- **默认值**: 无

#### backgroundImage.url
- **类型**: `string`（必需）
- **说明**: 背景图像的 URL

#### backgroundImage.x, backgroundImage.y, backgroundImage.width, backgroundImage.height
- **类型**: `number`（可选）
- **说明**: 背景图像在骨骼世界坐标系中的位置和大小
- **默认值**: 填充视口
- **示例**:
```javascript
{
  backgroundImage: {
    url: "assets/background.png",
    x: 0,
    y: 0,
    width: 1920,
    height: 1080
  }
}
// 或使用默认填充视口
{
  backgroundImage: {
    url: "assets/background.png"
  }
}
```

### mipmaps
- **类型**: `boolean`（可选）
- **说明**: 是否使用 mipmap 和各向异性过滤以获得最高质量的缩放效果（如果可用），否则使用纹理图集的过滤设置
- **默认值**: true
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  mipmaps: true
}
```

---

## 控制配置

### showControls
- **类型**: `boolean`（可选）
- **说明**: 是否显示播放器控件
- **默认值**: true
- **注意**: 当设置为 false 时，不需要外部 CSS 文件
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  showControls: false  // 隐藏控件，通过代码控制
}
```

### showLoading
- **类型**: `boolean`（可选）
- **说明**: 是否显示加载动画
- **默认值**: true
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  showLoading: true
}
```

### debug
- **类型**: `object`（可选）
- **说明**: 显示哪些调试可视化
- **默认值**: 所有选项都为 false

#### debug 选项
- `bones`: 显示骨骼
- `regions`: 显示区域附件
- `meshes`: 显示网格附件
- `bounds`: 显示边界框
- `paths`: 显示路径
- `clipping`: 显示裁剪
- `points`: 显示点
- `hulls`: 显示外壳

**示例**:
```javascript
{
  debug: {
    bones: true,
    regions: false,
    meshes: false,
    bounds: true,
    paths: false,
    clipping: false,
    points: false,
    hulls: false
  }
}
```

---

## 交互控制

### controlBones
- **类型**: `string[]`（可选）
- **说明**: 用户可以拖动以定位的骨骼名称列表
- **默认值**: 无
- **使用场景**: 允许用户通过拖动来交互式控制特定骨骼的位置
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  controlBones: ["head", "left-hand", "right-hand"]
}
```

---

## 回调函数

### success
- **类型**: `(player: SpinePlayer) => void`（可选）
- **说明**: 当骨骼及其资源成功加载时的回调函数
- **默认值**: 无
- **注意**: 如果在轨道 0 上设置了动画，播放器不会设置自己的动画
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  success: (player) => {
    console.log("骨骼加载成功！");
    console.log("可用动画：", player.skeleton.data.animations.map(a => a.name));
    // 可以在这里设置自定义动画或进行其他初始化
  }
}
```

### error
- **类型**: `(player: SpinePlayer, msg: string) => void`（可选）
- **说明**: 当骨骼无法加载或渲染时的回调函数
- **默认值**: 无
- **示例**:
```javascript
{
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  error: (player, msg) => {
    console.error("加载失败：", msg);
    // 显示错误信息给用户
  }
}
```

### frame
- **类型**: `(player: SpinePlayer, delta: number) => void`（可选）
- **说明**: 每帧开始时的回调，在骨骼摆姿势或绘制之前调用
- **默认值**: 无
- **参数**: delta - 自上一帧以来的时间增量（秒）
- **示例**:
```javascript
{
  frame: (player, delta) => {
    // 在每帧开始时执行的逻辑
  }
}
```

### update
- **类型**: `(player: SpinePlayer, delta: number) => void`（可选）
- **说明**: 每帧在骨骼摆姿势之后、绘制之前的回调
- **默认值**: 无
- **参数**: delta - 自上一帧以来的时间增量（秒）
- **使用场景**: 在骨骼摆姿势后、渲染前修改骨骼状态
- **示例**:
```javascript
{
  update: (player, delta) => {
    // 修改骨骼位置、旋转等
    let bone = player.skeleton.findBone("head");
    if (bone) {
      bone.rotation += delta * 45;  // 每秒旋转45度
    }
  }
}
```

### draw
- **类型**: `(player: SpinePlayer, delta: number) => void`（可选）
- **说明**: 每帧在骨骼绘制之后的回调
- **默认值**: 无
- **参数**: delta - 自上一帧以来的时间增量（秒）
- **使用场景**: 在骨骼渲染后绘制自定义内容
- **示例**:
```javascript
{
  draw: (player, delta) => {
    // 在骨骼渲染后绘制额外的内容
    let renderer = player.sceneRenderer;
    // 使用 renderer 绘制自定义图形
  }
}
```

### loading
- **类型**: `(player: SpinePlayer, delta: number) => void`（可选）
- **说明**: 骨骼加载之前的每帧回调
- **默认值**: 无
- **参数**: delta - 自上一帧以来的时间增量（秒）
- **使用场景**: 显示自定义加载界面
- **示例**:
```javascript
{
  loading: (player, delta) => {
    // 更新自定义加载进度
  }
}
```

---

## 高级配置

### downloader
- **类型**: `Downloader`（可选）
- **说明**: 播放器资源管理器使用的下载器
- **默认值**: 新实例
- **使用场景**: 将相同的下载器传递给使用相同资源的多个播放器，确保资源只下载一次
- **示例**:
```javascript
// 创建共享下载器
const sharedDownloader = new spine.Downloader();

// 多个播放器使用同一个下载器
new spine.SpinePlayer("player1", {
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  downloader: sharedDownloader
});

new spine.SpinePlayer("player2", {
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  downloader: sharedDownloader  // 资源只会下载一次
});
```

---

## 完整示例

### 基础示例
```javascript
new spine.SpinePlayer("player-container", {
  jsonUrl: "assets/spineboy.json",
  atlasUrl: "assets/spineboy.atlas",
  animation: "run",
  backgroundColor: "#cccccc",
  showControls: true
});
```

### 高级示例
```javascript
new spine.SpinePlayer("advanced-player", {
  jsonUrl: "assets/character.json",
  atlasUrl: "assets/character.atlas",
  animation: "idle",
  animations: ["idle", "walk", "run", "jump", "attack"],
  skin: "default",
  skins: ["default", "armor", "casual"],
  defaultMix: 0.3,
  
  premultipliedAlpha: true,
  alpha: false,
  backgroundColor: "#2c3e50",
  fullScreenBackgroundColor: "#000000",
  
  viewport: {
    padLeft: "10%",
    padRight: "10%",
    padTop: "15%",
    padBottom: "10%",
    transitionTime: 0.4,
    debugRender: false,
    animations: {
      "jump": {
        x: 0,
        y: 0,
        width: 800,
        height: 1000,
        padTop: "20%"
      }
    }
  },
  
  backgroundImage: {
    url: "assets/background.jpg"
  },
  
  showControls: true,
  showLoading: true,
  mipmaps: true,
  preserveDrawingBuffer: true,
  
  debug: {
    bones: false,
    regions: false,
    meshes: false,
    bounds: false,
    paths: false,
    clipping: false,
    points: false,
    hulls: false
  },
  
  controlBones: ["weapon-handle"],
  
  success: (player) => {
    console.log("播放器加载成功");
    console.log("骨骼：", player.skeleton.data.name);
    console.log("可用动画：", player.skeleton.data.animations.map(a => a.name));
  },
  
  error: (player, msg) => {
    console.error("播放器加载失败：", msg);
  },
  
  update: (player, delta) => {
    // 自定义更新逻辑
  }
});
```

### 嵌入式数据示例
```javascript
new spine.SpinePlayer("embedded-player", {
  jsonUrl: "skeleton.json",
  atlasUrl: "skeleton.atlas",
  rawDataURIs: {
    "skeleton.json": "data:application/json;base64,eyJza2VsZXRvbiI6e...",
    "skeleton.atlas": "data:text/plain;base64,CnNrZWxldG9uLnBuZw...",
    "skeleton.png": "data:image/png;base64,iVBORw0KGgoAAAANSUh..."
  },
  animation: "animation",
  backgroundColor: "#ffffff"
});
```

### 编程控制示例
```javascript
let player = new spine.SpinePlayer("controlled-player", {
  jsonUrl: "assets/character.json",
  atlasUrl: "assets/character.atlas",
  showControls: false,
  backgroundColor: "#00000000",
  alpha: true,
  success: (player) => {
    // 设置按钮事件
    document.getElementById("walk-btn").onclick = () => {
      player.setAnimation("walk", true);
      player.play();
    };
    
    document.getElementById("jump-btn").onclick = () => {
      player.setAnimation("jump", false);
      player.play();
    };
    
    document.getElementById("pause-btn").onclick = () => {
      player.pause();
    };
  }
});
```

---

## 常见问题

### 1. 如何在不显示控件的情况下控制播放器？
设置 `showControls: false`，然后在 `success` 回调中使用播放器 API：
```javascript
{
  showControls: false,
  success: (player) => {
    player.setAnimation("walk", true);
    player.play();
    // 或
    player.pause();
  }
}
```

### 2. 如何实现透明背景？
```javascript
{
  alpha: true,
  backgroundColor: "#00000000"
}
```

### 3. 如何截取播放器画布的截图？
```javascript
{
  preserveDrawingBuffer: true,
  success: (player) => {
    // 截图
    let dataURL = player.canvas.toDataURL();
    let img = document.createElement('img');
    img.src = dataURL;
    document.body.appendChild(img);
  }
}
```

### 4. 如何为不同的动画设置不同的视口？
使用 `viewport.animations` 配置：
```javascript
{
  viewport: {
    animations: {
      "walk": { x: 0, y: 0, width: 800, height: 600 },
      "jump": { x: 0, y: 100, width: 800, height: 900 }
    }
  }
}
```

### 5. 如何共享资源以提高性能？
使用共享的 `Downloader` 实例：
```javascript
const downloader = new spine.Downloader();

new spine.SpinePlayer("player1", {
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  downloader: downloader
});

new spine.SpinePlayer("player2", {
  jsonUrl: "assets/skeleton.json",
  atlasUrl: "assets/skeleton.atlas",
  downloader: downloader
});
```

---

## 参考资源

- [Spine 官方文档](http://esotericsoftware.com/spine-documentation)
- [Spine Web Player 文档](https://esotericsoftware.com/spine-player)
- [Spine Runtimes 指南](http://esotericsoftware.com/spine-runtimes-guide)
- [spine-ts GitHub 仓库](https://github.com/EsotericSoftware/spine-runtimes/tree/4.0/spine-ts)

---

## 版本信息

此文档适用于 spine-ts 4.0.x 版本。

## 许可证

请参阅 [Spine Runtimes 许可协议](http://esotericsoftware.com/spine-runtimes-license)。
