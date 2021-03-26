## jQuery对象转换为DOM对象
### [index]方法
jQuery对象是一个类似数组的对象，可以通过[index]方法获取相应的DOM对象
```
var #cr=$('$cr');
var cr=#cr[0];
```

### jquery对象本身的get方法
```
var #cr=$('#cr');
var cr=#cr.get(0);
```

## DOM对象转换为jQuery对象
只需要将DOM对象用$符号包起来就可以
```
const el=docment.getElementById('app');
const $el=$(el);
```

## 点击复选框跳出提示
```
    <input type="checkbox" name="" id="cr"><label for="cr">我已经阅读了上面制度</label>

    <script src="../resource/jquery-3.3.1.js"></script>
    <script>
        // $(document).ready(function () {
        //     var $cr = $('#cr');
        //     var cr = $cr[0];
        //     $cr.click(function () {
        //         if (cr.checked) {
        //             alert('checked');
        //         }
        //     })
        // })
        // DOM方法

        $(() => {
            $('#cr').click(() => {
                if ($('#cr').is(':checked')) {
                    alert('checked');
                }
            })
        })
    </script>
```