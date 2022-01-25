[Toc]

## translateX与left的取舍

- 为了让一个div盒子运动起来,可以通过修改translateX使它运动,也可以通过定位并修改left使它水平运动.

- ![timeline-frames-macbook1](C:\Users\董磊\Desktop\timeline-frames-macbook1.png)

- > 左上方的图片是通过改变元素top/left进行动画的帧率，而右上方则是调用translate函数的帧率。我们可以明显看出左图的每一帧都执行了相对较长时间的paint，在每一帧内，cpu都需要计算该元素的其他样式，特别是相对需要复杂计算的盒阴影，渐变，圆角等样式，最后都需要将这些样式应用到该元素内。从这个角度看，如果对于较为老旧的移动设备进行相对复杂的动画，那么效果肯定不理想。
  >
  > ​    而通过调用 translate，会启动硬件加速，即在 GPU 层对该元素进行渲染。这样，CPU 就会相对解放出来进行其他的计算，GPU 对样式的计算相对较快，且保证较大的帧率。我们可以通过 2d 和 3d 的 transform 来启用 GPU 计算。

- 简要总结:因为translateX会使用GPU对元素进行渲染,解放CPU的算力,因此相较于直接修改left,使用translateX对硬件更友好.

## 补间动画的代码

```javascript
    var Tween = {
        Linear: function (t, b, c, d) {
            return c * t / d + b;
        },
        Quad: {
            easeIn: function (t, b, c, d) {
                return c * (t /= d) * t + b;
            },
            easeOut: function (t, b, c, d) {
                return -c * (t /= d) * (t - 2) + b;
            },
            easeInOut: function (t, b, c, d) {
                if ((t /= d / 2) < 1) return c / 2 * t * t + b;
                return -c / 2 * ((--t) * (t - 2) - 1) + b;
            }
        },
        Cubic: {
            easeIn: function (t, b, c, d) {
                return c * (t /= d) * t * t + b;
            },
            easeOut: function (t, b, c, d) {
                return c * ((t = t / d - 1) * t * t + 1) + b;
            },
            easeInOut: function (t, b, c, d) {
                if ((t /= d / 2) < 1) return c / 2 * t * t * t + b;
                return c / 2 * ((t -= 2) * t * t + 2) + b;
            }
        },
        Quart: {
            easeIn: function (t, b, c, d) {
                return c * (t /= d) * t * t * t + b;
            },
            easeOut: function (t, b, c, d) {
                return -c * ((t = t / d - 1) * t * t * t - 1) + b;
            },
            easeInOut: function (t, b, c, d) {
                if ((t /= d / 2) < 1) return c / 2 * t * t * t * t + b;
                return -c / 2 * ((t -= 2) * t * t * t - 2) + b;
            }
        },
        Quint: {
            easeIn: function (t, b, c, d) {
                return c * (t /= d) * t * t * t * t + b;
            },
            easeOut: function (t, b, c, d) {
                return c * ((t = t / d - 1) * t * t * t * t + 1) + b;
            },
            easeInOut: function (t, b, c, d) {
                if ((t /= d / 2) < 1) return c / 2 * t * t * t * t * t + b;
                return c / 2 * ((t -= 2) * t * t * t * t + 2) + b;
            }
        },
        Sine: {
            easeIn: function (t, b, c, d) {
                return -c * Math.cos(t / d * (Math.PI / 2)) + c + b;
            },
            easeOut: function (t, b, c, d) {
                return c * Math.sin(t / d * (Math.PI / 2)) + b;
            },
            easeInOut: function (t, b, c, d) {
                return -c / 2 * (Math.cos(Math.PI * t / d) - 1) + b;
            }
        },
        Expo: {
            easeIn: function (t, b, c, d) {
                return (t == 0) ? b : c * Math.pow(2, 10 * (t / d - 1)) + b;
            },
            easeOut: function (t, b, c, d) {
                return (t == d) ? b + c : c * (-Math.pow(2, -10 * t / d) + 1) + b;
            },
            easeInOut: function (t, b, c, d) {
                if (t == 0) return b;
                if (t == d) return b + c;
                if ((t /= d / 2) < 1) return c / 2 * Math.pow(2, 10 * (t - 1)) + b;
                return c / 2 * (-Math.pow(2, -10 * --t) + 2) + b;
            }
        },
        Circ: {
            easeIn: function (t, b, c, d) {
                return -c * (Math.sqrt(1 - (t /= d) * t) - 1) + b;
            },
            easeOut: function (t, b, c, d) {
                return c * Math.sqrt(1 - (t = t / d - 1) * t) + b;
            },
            easeInOut: function (t, b, c, d) {
                if ((t /= d / 2) < 1) return -c / 2 * (Math.sqrt(1 - t * t) - 1) + b;
                return c / 2 * (Math.sqrt(1 - (t -= 2) * t) + 1) + b;
            }
        },
        Elastic: {
            easeIn: function (t, b, c, d, a, p) {
                if (t == 0) return b;
                if ((t /= d) == 1) return b + c;
                if (!p) p = d * .3;
                if (!a || a < Math.abs(c)) {
                    a = c;
                    var s = p / 4;
                } else var s = p / (2 * Math.PI) * Math.asin(c / a);
                return -(a * Math.pow(2, 10 * (t -= 1)) * Math.sin((t * d - s) * (2 * Math.PI) / p)) + b;
            },
            easeOut: function (t, b, c, d, a, p) {
                if (t == 0) return b;
                if ((t /= d) == 1) return b + c;
                if (!p) p = d * .3;
                if (!a || a < Math.abs(c)) {
                    a = c;
                    var s = p / 4;
                } else var s = p / (2 * Math.PI) * Math.asin(c / a);
                return (a * Math.pow(2, -10 * t) * Math.sin((t * d - s) * (2 * Math.PI) / p) + c + b);
            },
            easeInOut: function (t, b, c, d, a, p) {
                if (t == 0) return b;
                if ((t /= d / 2) == 2) return b + c;
                if (!p) p = d * (.3 * 1.5);
                if (!a || a < Math.abs(c)) {
                    a = c;
                    var s = p / 4;
                } else var s = p / (2 * Math.PI) * Math.asin(c / a);
                if (t < 1) return -.5 * (a * Math.pow(2, 10 * (t -= 1)) * Math.sin((t * d - s) * (2 * Math.PI) / p)) + b;
                return a * Math.pow(2, -10 * (t -= 1)) * Math.sin((t * d - s) * (2 * Math.PI) / p) * .5 + c + b;
            }
        },
        Back: {
            easeIn: function (t, b, c, d, s) {
                if (s == undefined) s = 1.70158;
                return c * (t /= d) * t * ((s + 1) * t - s) + b;
            },
            easeOut: function (t, b, c, d, s) {
                if (s == undefined) s = 1.70158;
                return c * ((t = t / d - 1) * t * ((s + 1) * t + s) + 1) + b;
            },
            easeInOut: function (t, b, c, d, s) {
                if (s == undefined) s = 1.70158;
                if ((t /= d / 2) < 1) return c / 2 * (t * t * (((s *= (1.525)) + 1) * t - s)) + b;
                return c / 2 * ((t -= 2) * t * (((s *= (1.525)) + 1) * t + s) + 2) + b;
            }
        },
        Bounce: {
            easeIn: function (t, b, c, d) {
                return c - Tween.Bounce.easeOut(d - t, 0, c, d) + b;
            },
            easeOut: function (t, b, c, d) {
                if ((t /= d) < (1 / 2.75)) {
                    return c * (7.5625 * t * t) + b;
                } else if (t < (2 / 2.75)) {
                    return c * (7.5625 * (t -= (1.5 / 2.75)) * t + .75) + b;
                } else if (t < (2.5 / 2.75)) {
                    return c * (7.5625 * (t -= (2.25 / 2.75)) * t + .9375) + b;
                } else {
                    return c * (7.5625 * (t -= (2.625 / 2.75)) * t + .984375) + b;
                }
            },
            easeInOut: function (t, b, c, d) {
                if (t < d / 2) return Tween.Bounce.easeIn(t * 2, 0, c, d) * .5 + b;
                else return Tween.Bounce.easeOut(t * 2 - d, 0, c, d) * .5 + c * .5 + b;
            }
        }
    }
```

## 函数说明

1. **Linear**：线性匀速运动效果；
2. **Quadratic**：二次方的缓动（t^2）；
3. **Cubic**：三次方的缓动（t^3）；
4. **Quartic**：四次方的缓动（t^4）；
5. **Quintic**：五次方的缓动（t^5）；
6. **Sinusoidal**：正弦曲线的缓动（sin(t)）；
7. **Exponential**：指数曲线的缓动（2^t）；
8. **Circular**：圆形曲线的缓动（sqrt(1-t^2)）；
9. **Elastic**：指数衰减的正弦曲线缓动；
10. **Back**：超过范围的三次方缓动（(s+1)*t^3 – s*t^2）；
11. **Bounce**：指数衰减的反弹缓动。

### 参数说明

```javascript
/*
 * t: current time（当前时间）；
 * b: beginning value（初始值）；
 * c: change in value（变化量）；
 * d: duration（持续时间）。
*/
```

## 补间动画的使用

- 使一个盒子从{left:500px}处开始,5000ms内水平线性匀速运动-300px

  - ```javascript
            let oD1 = document.querySelector('#d1')
    
            function Linear(t, b, c, d) {
                return c * t / d + b
            }
            let time = 0
            let timer = setInterval(() => {
                time += 1000 / 60
                let left = Linear(time, 0, -400, 5000)
                oD1.style.transform = 'translateX(' + left + 'px)'
                if (time >= 5000) {
                    clearInterval(timer)
                    timer = null
                }
            }, 1000 / 60)
    ```

  - 可以使用`requestAnimationFrame`代替`setInterval`

## 补间动画的封装

```javascript
/* 
    示例：
    TweenAnimation(oWrapper,'translateX',0,500,5000,60,'linear')

*/
/**
 * @description: 
 * @param {*} el 对象
 * @param {*} style 属性字符串
 * @param {*} startPos 起始位置
 * @param {*} endPos 终点位置
 * @param {*} duration 持续时间
 * @param {*} frame 帧数
 * @param {*} type 动画类型
 * @param {*} callback 回调函数
 * @return {*} 没有返回值
 */
function tweenAnimation(el, style, startPos, displacement, duration, frame, type, callback) {
    var Tween = {
        // 复制上面的补间动画在此处
    }
    // 初始化参数
    el = (typeof el === 'object') ? el : document.querySelector(el)
    let fn = null
    if (/(\w+)\.?(\w+)?/g.test(type)) {
        if (RegExp.$2) {
            fn = Tween[RegExp.$1][RegExp.$2] ? Tween[RegExp.$1][RegExp.$2] : Tween.Linear
        } else {
            fn = Tween[RegExp.$1] ? Tween[RegExp.$1] : Tween.Linear
        }
    }
    frame = typeof frame === 'number' ? frame : 60
    // 设定timer的储存变量
    if (el[style] === undefined) {
        el[style] = {}
    }
    // 设定interval
    let dis
    let time = 0

    function animation() {
        time += (1000 / frame)
        dis = fn(time, startPos, displacement, duration)
        transformCSS(el, style, dis)
        if (time < duration) {
            el[style].requestID = requestAnimationFrame(animation, 1000 / frame)
        }
    }
    el[style].requestID = requestAnimationFrame(animation, 1000 / frame)
}
```

- 通过将`requestAnimationFrame`的请求ID保存在`el[style].requestID`中,达到可以随时取消补间动画的效果(通过`cancaelAnimationFrame`取消)

## 参考博客

[如何使用Tween.js各类原生动画运动缓动算法](https://www.zhangxinxu.com/wordpress/2016/12/how-use-tween-js-animation-easing/)