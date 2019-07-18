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
        methods: {
            scroll: function (div) {
                this.handleSelecte(div);
                if (div) {
                    let anchorElement = document.getElementById(div);
                    if (anchorElement) {
                        anchorElement.scrollIntoView({block: 'start', behavior: 'smooth'});
                    }
                }
            },
            handleSelecte(div){
                for (var i in this.$props.nodes){
                    this.$props.nodes[i].isSelected=false;
                    if (this.$props.nodes[i].divName==div){
                        this.$props.nodes[i].isSelected=true;
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