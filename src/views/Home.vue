<template>
    <my-page title="字符画制作">
        <canvas id="html5_08_1" class="canvas" width="180" height="180" style=" background-color: black">
            你的浏览器不支持 Canvas 标签，请使用 Chrome 浏览器 或者 FireFox 浏览器
        </canvas>
        <div class="tools">
            <ui-text-field v-model="text" label="文本" />
            <br>
            <ui-text-field v-model="unit" label="单元字" />
            <br>
            <ui-text-field v-model.number="fontSize" type="number" label="字号" />
            <br>
            <ui-select-field v-model="fontFamily" label="字体">
                <ui-menu-item value="宋体" title="宋体"/>
                <ui-menu-item value="楷体_GB2312" title="楷体_GB2312"/>
                <ui-menu-item value="隶书" title="隶书"/>
                <ui-menu-item value="黑体" title="黑体"/>
                <ui-menu-item value="微软雅黑" title="微软雅黑"/>
                <ui-menu-item value="Kristen ITC" title="Kristen ITC"/>
                <ui-menu-item value="Harrington" title="Harrington"/>
            </ui-select-field>
            <br>
            <ui-checkbox v-model="italic" label="斜体" class="demo-checkbox"/>
            <br>
            <br>
            <ui-raised-button primary label="生成"@click="handle" />
            <!-- <input class="btn btn-primary" type="button" value="生成" @click="handle"> -->
        </div>
        <div class="result-box">
            <textarea id="txtResult" class="form-control" v-model="result" wrap="off" rows="20" cols="120"></textarea>
        </div>
    </my-page>
</template>

<script>
    /* eslint-disable */
    function trim(text) {
        var newArr = [];
        var arr = text.split('\n');
        arr.forEach(function (line) {
            console.log(line.length)
            line = line.replace(/(\s*$)/g, '');
            console.log(line)
            if (line.length) {
                newArr.push(line);
            }
        });
        return newArr.join('\n');
    }

    export default {
        data () {
            return {
                text: '云设工具',
                unit: '云设',
                result: '',
                fontSize: 16,
                fontFamily: '宋体',
                italic: false,
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'help',
                            to: '/help'
                        }
                    ]
                }
            }
        },
        mounted() {
            this.handle()
        },
        methods: {
            handle: function () {
                var canvas = document.getElementById('html5_08_1');
                var ctx = canvas.getContext('2d');

                ctx.clearRect(0, 0, 180, 180);
                ctx.font = (this.italic ? 'italic ' : ' ') + this.fontSize + 'px "' + this.fontFamily + '" ';
                ctx.fillStyle = '#fff';
                ctx.fillText(this.text, 0, this.fontSize + 10);

                var str = '';

                var c = ctx.getImageData(0, 0, 180, 180);
                //chrome浏览器报错，ie浏览器报安全错误信息，原因往下看
                var idx = 0;
                for (var i = 0; i < c.height; ++i) {
                    for (var j = 0; j < c.width; ++j) {
                        var x = i * 4 * c.width + 4 * j,  //imagedata读取的像素数据存储在data属性里，是从上到下，从左到右的，每个像素需要占用4位数据，分别是r,g,b,alpha透明通道
                            r = c.data[x],
                            g = c.data[x + 1],
                            b = c.data[x + 2];
                        if (r < 255) {
                            str = str + '　';
                        } else {
                            str = str + this.unit.charAt(idx % this.unit.length);
                        }
                        idx++;
                    }
                    str = str + '\n';
                }

                str = trim(str);
                this.result = str;
            },
            clear: function () {
                this.text = '';
                this.compute();
            }
        }
    }
</script>

<style scoped>
.tip {
    margin-bottom: 16px;
    color: #999;
}
.canvas {
    display: none;
}
.tools {
    float: left;
    margin-right: 32px;
    margin-bottom: 16px;
}
textarea {
    margin-bottom: 16px;
}
/**/
.result-box {
    float: left;
    max-width: 640px;
}
.result-box .info {
    margin-bottom: 16px;
}
/**/
textarea.form-control {
    height: auto;
}
.form-control {
    display: block;
    width: 100%;
    height: 33px;
    padding: 6px 12px;
    font-size: 14px;
    line-height: 1.42;
    color: #55595c;
    background-color: #fff;
    background-image: none;
    border: 1px solid #ccc;
    border-radius: 2px;
}
</style>
