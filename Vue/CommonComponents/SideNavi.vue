<template>
    <div>
        <table style="display: inline-block;text-align: right">
            <tr  v-for="node in nodes" style="height: 60px">
                <td style="margin-left: -5px"><span  class="animated fadeIn" v-if="node.display" style="font-size: 15px;margin-right: 20px">{{node.displayName}}</span></td>
                <td><div class="dot" @click="scroll(node.divName)" @mouseenter="handleMouseEnter(node.divName)"
                         @mouseout="handleMouseOut(node.divName)" :class="{'selected':node.isSelected}"></div></td>
            </tr>
        </table>
    </div>
</template>

<script>
    export default {
        name: "SideNavi",
        props: [
            'nodes'
        ],
        data() {
            return {}
        },
        mounted(){
            var self = this;
            var divIds = [];
            var doc = document;
            var divs=document.querySelectorAll('.navi-div');
            for (var i = 0; i < divs.length; i++) {
                var element = divs[i];
                divIds.push(element.id);
            }
            var $scrollBox = doc.querySelector('.container');
            var scrollCallback = self.debounce(function () {
                var top = $scrollBox.scrollTop; //设置变量top,表示当前滚动条到顶部的值
                divs=document.querySelectorAll('.navi-div');
                var num=-1;
                for (var i in divs){
                    if (top < (divs[i].offsetTop-divs[i].offsetHeight/2)){
                        break;
                    }else if (top>=divs[i].offsetTop-divs[i].offsetHeight/2) {
                        num=i
                    }
                }
                if (num!=-1){
                    for (var i in self.$props.nodes){
                        self.$props.nodes[i].display=false;
                        self.$props.nodes[i].isSelected=false;
                        if (self.$props.nodes[i].divName==divIds[num]){
                            self.$props.nodes[i].display=true;
                            self.$props.nodes[i].isSelected=true;
                        }
                    }
                }else {
                    for (var i in self.$props.nodes){
                        self.$props.nodes[i].display=false;
                        self.$props.nodes[i].isSelected=false;
                    }
                }
            }, 150);
            $scrollBox.addEventListener('scroll', scrollCallback)


        },
        methods: {
            debounce(func, wait, immediate) {
                var timeout, args, context, timestamp, result;
                var later = function () {
                    // 据上一次触发时间间隔
                    var last = new Date() - timestamp;
                    // 上次被包装函数被调用时间间隔last小于设定时间间隔wait
                    if (last < wait && last > 0) {
                        timeout = setTimeout(later, wait - last);
                    } else {
                        timeout = null;
                        // 如果设定为immediate===true，因为开始边界已经调用过了此处无需调用
                        if (!immediate) {
                            result = func.apply(context, args);
                            if (!timeout) context = args = null;
                        }
                    }
                };
                return function () {
                    context = this;
                    args = arguments;
                    timestamp = new Date();
                    var callNow = immediate && !timeout;
                    // 如果延时不存在，重新设定延时
                    if (!timeout) timeout = setTimeout(later, wait);
                    if (callNow) {
                        result = func.apply(context, args);
                        context = args = null;
                    }

                    return result;
                };
            },
            scroll: function (div) {
                this.handleSelect(div);
                if (div) {
                    let anchorElement = document.getElementById(div);
                    if (anchorElement) {
                        anchorElement.scrollIntoView({block: 'start', behavior: 'smooth'});
                    }
                }
            },
            handleSelect(div){
                for (var i in this.$props.nodes){
                    this.$props.nodes[i].isSelected=false;
                    if (this.$props.nodes[i].divName==div){
                        this.$props.nodes[i].isSelected=true;
                        this.$props.nodes[i].display=true;
                    }
                }
            },
            handleMouseEnter(div){
                for (var i in this.$props.nodes){
                    if (this.$props.nodes[i].divName==div){
                        this.$props.nodes[i].display=true;
                        break;
                    }
                }
            },
            handleMouseOut(div){
                for (var i in this.$props.nodes){
                    if (this.$props.nodes[i].divName==div){

                        this.$props.nodes[i].display=false;
                        break;

                    }
                }
            }
        }
    }
</script>

<style scoped>
    .dot {
        width: 10px;
        height: 10px;
        border: 1px solid #b68f79;
        border-radius: 100px;
        background-color: #FFFFFFF;
        opacity: 0.5;
    }
    .dot:hover{
        background-color: #b68f79;
        transform: scale(1.3);
    }
    .selected{
        background-color: #b68f79;
        width: 13px;
        height: 13px;
    }
</style>