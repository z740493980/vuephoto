<template>
    <div id="perspective" @mousedown="move3D">
    <div id="wrap" v-show="show" ref="wrap1">
        <!--单个transition实现-->
<!--        <transition name="fade" appear>
        <div class="image-block" :style="{transform: 'rotateY(0deg) translateZ(400px)'}" v-if="show">
            <img class="pic" src="../assets/img1.jpg"/>
        </div>
        </transition>
        <div class="image-block" :style="{transform: 'rotateY(30deg) translateZ(400px)'}">
            <img class="pic" src="../assets/img2.jpg"/>
        </div>-->
<!--        <transition-group name="fade" appear>
            <span v-for="(item,index) in product" :key="item.id" >
            <div class="image-block" :style="{transform: 'rotateY('+index*30+'deg) translateZ(400px)',transition: '1s '+ (12 - index) * 0.05 +' s'}">
                    <img :src="item.src"/>
            </div>
            </span>
        </transition-group>-->
        <transition-group >
            <span v-for="(item,index) in target"  v-bind:key="item.id"  v-bind:data-index="item">
                <div class="image-block" :style="{transform: 'rotateY('+index*30+'deg) translateZ(400px)'}">
                    <img :src="item.src"/>
            </div>
            </span>
        </transition-group>
    </div>
    </div>
</template>

<script>
    import  Velocity from 'velocity-animate'
    export default {
        name: "photo3D",
        data() {
            return {
                show: false,
                product:[
                    {
                        "src":require('../../static/images/img1.jpg'),
                        "deceration":"产品1",
                        "price":100,
                        "id":1
                    },
                    {
                        "src":require('../../static/images/img2.jpg'),
                        "deceration":"产品2",
                        "price":150,
                        "id":2
                    },
                    {
                        "src":require('../../static/images/img3.jpg'),
                        "deceration":"产品3",
                        "price":180,
                        "id":3
                    },
                    {
                        "src":require('../../static/images/img3.jpg'),
                        "deceration":"产品3",
                        "price":180,
                        "id":4
                    },
                    {
                        "src":require('../../static/images/img3.jpg'),
                        "deceration":"产品3",
                        "price":180,
                        "id":5
                    },
                    {
                        "src":require('../../static/images/img3.jpg'),
                        "deceration":"产品3",
                        "price":180,
                        "id":6
                    },
                    {
                        "src":require('../../static/images/img3.jpg'),
                        "deceration":"产品3",
                        "price":180,
                        "id":7
                    },
                    {
                        "src":require('../../static/images/img3.jpg'),
                        "deceration":"产品3",
                        "price":180,
                        "id":8
                    },
                    {
                        "src":require('../../static/images/img3.jpg'),
                        "deceration":"产品3",
                        "price":180,
                        "id":9
                    },
                    {
                        "src":require('../../static/images/img3.jpg'),
                        "deceration":"产品3",
                        "price":180,
                        "id":10
                    },
                    {
                        "src":require('../../static/images/img3.jpg'),
                        "deceration":"产品3",
                        "price":180,
                        "id":11
                    },
                    {
                        "src":require('../../static/images/img3.jpg'),
                        "deceration":"产品3",
                        "price":180,
                        "id":12
                    }
                ],
                target:[],
                positionX:0,
                positionY:0,
                nowX:0,
                nowY:0,
                lastX:0,
                lasty:0,
                minsX:0,
                minsY:0,
                roX:0,
                roY:0
            }
        },
        mounted: function() {
            this.$nextTick(function() {
                this.charge();
            });
        },
        methods:{
            charge(){
                this.show = !this.show;
                for (let i = 0; i < this.product.length; i++) {
                    setTimeout(() => {
                        this.target.push(this.product[i]);
                    }, (12 - i) * 100);
                }
            },
            beforeEnter: function (el) {
                console.log(el);
            },
            enter: function (el, done) {
                console.log(el);
                var index = el.dataset.index
                setTimeout(function () {
                    Velocity(
                        el,
                        { 'background - position - y':
                    -100 * index +'%'
                },
                    {
                        duration: 5000 + index * -100,
                            easing
                    :
                        "easeInOutCirc",
                            complete
                    :
                        done
                    }
                )
                }, index * -1000)
            },
            move(e){
                //console.log("当前元素"+e.target);
                let odiv = e.target;        //获取目标元素

                //算出鼠标相对元素的位置
                let disX = e.clientX - odiv.offsetLeft;
                let disY = e.clientY - odiv.offsetTop;
                document.onmousemove = (e)=>{       //鼠标按下并移动的事件
                    //用鼠标的位置减去鼠标相对元素的位置，得到元素的位置
                    let left = e.clientX - disX;
                    let top = e.clientY - disY;

                    //绑定元素位置到positionX和positionY上面
                    this.positionX = top;
                    this.positionY = left;

                    //移动当前元素
                    odiv.style.left = left + 'px';
                    odiv.style.top = top + 'px';
                };
                document.onmouseup = (e) => {
                    console.log(e);
                    document.onmousemove = null;
                    document.onmouseup = null;
                };
            },
            move3D(e){
                //console.log("当前元素"+e.target);
                //let odiv = e.target;        //获取目标元素

                this.lastX = e.clientX;
                this.lastY = e.clientY;

                document.onmousemove = (e)=>{       //鼠标按下并移动的事件
                    //用鼠标的位置减去鼠标相对元素的位置，得到元素的位置

                    this.nowX = e.clientX;
                    this.nowY = e.clientY;

                    this.minsX = this.nowX - this.lastX;
                    this.minsY = this.nowY - this.lastY;

                    //旋转的总度数的值
                    this.roX -= this.minsY * 0.2;
                    this.roY += this.minsX * 0.2;

                    //console.log("test"+this.roX);
                    //console.log("test1"+this.roY);
                    this.$refs.wrap1.style.transform = "rotateX(" + this.roX + "deg) rotateY(" + this.roY + "deg)";
                    this.lastX = this.nowX;
                    this.lastY = this.nowY;
                };
                document.onmouseup = (e) => {
                    console.log(e);
                    document.onmousemove = null;
                    document.onmouseup = null;
                };
            }
        }
    }
</script>

<style scoped>
    * {

    }
    #perspective {
        #background-color: #0b0b0b;
        position: relative;
        height: 100%;
        width: 100%;
        #position: fixed;
        perspective: 800px; /*景深*/
    }

    #wrap {
        position: relative;
        width: 150px;
        height: 178px;
        margin: 200px auto;
        transform-style: preserve-3d; /*构建3d场景*/
        transform: rotateX(-20deg) rotateY(0deg);/*旋转的度数*/
    }

    #wrap .image-block {
        position: absolute;
        transform: rotateY(0deg);
        /*transition: 1s;值的改变有一个过渡的过程*/
    }
    #wrap .image-block img{
        width: 150px;
        height: 178px;
    }
/*    .fade-enter-active, .fade-leave-active {
        #transform: rotateY(0deg) translateZ(400px);
        transition: all 5s ease;
    }
    .fade-enter, .fade-leave-to !* .fade-leave-active in <2.1.8 *! {
        #transform: translateX(10px);
        opacity: 0;
    }*/
</style>
