# spine-ts Player 

Spine Web Player 是一个用于在网页上轻松显示 Spine 动画的独立播放器。

## 快速开始

```html
<!DOCTYPE html>
<html>
<head>
    <script src="https://unpkg.com/@esotericsoftware/spine-player@4.0.*/dist/iife/spine-player.js"></script>
    <link rel="stylesheet" href="https://unpkg.com/@esotericsoftware/spine-player@4.0.*/dist/spine-player.css">
</head>
<body>
    <div id="player-container" style="width: 640px; height: 480px;"></div>
    <script>
        new spine.SpinePlayer("player-container", {
            jsonUrl: "assets/spineboy.json",
            atlasUrl: "assets/spineboy.atlas",
            animation: "run",
            backgroundColor: "#cccccc",
            showControls: true
        });
    </script>
</body>
</html>
```

## 文档

- [SpinePlayerConfig 配置说明（中文）](./SpinePlayerConfig.md) - 详细的配置选项文档
- [English Documentation](https://esotericsoftware.com/spine-player) - Official Spine Web Player documentation
- [spine-ts 主文档](https://github.com/EsotericSoftware/spine-runtimes/blob/4.0/spine-ts/README.md)

## 示例

查看 [example](./example) 目录获取更多使用示例。

## 许可证

请参阅 [Spine Runtimes 许可协议](http://esotericsoftware.com/spine-runtimes-license)。