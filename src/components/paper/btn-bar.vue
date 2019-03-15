<template>
    <div class="tu-btn-bar">
        <el-tooltip effect="light" content="颜色" placement="left">
            <div class="tu-btn">
                <el-color-picker @change="colorHandler" v-model="color"></el-color-picker>
            </div>
        </el-tooltip>
        <el-tooltip effect="light" placement="left">
            <div slot="content">
                <div class="edit-line">
                    <div class="label">线条粗细：</div>
                    <div class="content">
                        <el-slider :step="0.1" :min="0.1" :max="10" @change="sliderHandler" v-model="slider"></el-slider>
                    </div>
                    <div class="value">{{slider}}</div>
                </div>
            </div>
            <div class="tu-btn">
                <i class="el-icon-edit"></i>
            </div>
        </el-tooltip>
        <el-tooltip effect="light" content="后退" placement="left">
            <div class="tu-btn" @click="backHandler">
                <i class="el-icon-back"></i>
            </div>
         </el-tooltip>
         <el-tooltip effect="light" content="前进" placement="left">
            <div class="tu-btn" @click="advanceHandler">
                <i class="el-icon-back cover"></i>
            </div>
        </el-tooltip>
        <el-tooltip effect="light" content="删除" placement="left">
            <div class="tu-btn" @click="removeHandler">
                <i class="el-icon-delete"></i>
            </div>
        </el-tooltip>
        <el-tooltip effect="light" content="保存" placement="left">
            <div class="tu-btn" @click="saveHandler">
                <i class="el-icon-picture-outline"></i>
            </div>
        </el-tooltip>
    </div>
</template>

<script>
    export default {
        name: 'TuBtnBar',
        data() {
            return {
                color: '#409EFF',
                slider: 1
            }
        },
        methods: {

            /**
             * 颜色
             */
            colorHandler() {
                this.$emit('color', this.color)
            },

            /**
             * 画笔
             */
            sliderHandler() {
                this.$emit('slider', this.slider)
            },

            /**
             * 后退
             */
            backHandler() {
                this.$emit('back')
            },

            /**
             * 前进
             */
            advanceHandler() {
                this.$emit('advance')
            },

            /**
             * 删除
             */
            removeHandler() {
                this.$emit('remove')
            },


            /**
             * 保存
             */
            saveHandler() {
                this.$emit('save')
            }
        }
    }
</script>

<style scoped>
    .tu-btn-bar{
        position: absolute;
        top: 15px;
        right: 10px;
        display: flex;
        flex-direction: column;
        overflow: hidden;
    }
    .tu-btn-bar > .tu-btn{
        flex-basis: 0;
        flex-grow: 1;
        padding: 10px 8px;
        color: #ffffff;
        background-color: rgba(0, 0, 0, 0.8);
        text-align: center;
        cursor: pointer;
    }
    .tu-btn-bar > .tu-btn i{
        font-size: 20px;
    }
    .tu-btn-bar > .tu-btn i.cover{
        transform: rotate(180deg);
    }
    .tu-btn-bar > .tu-btn:first-child{
        border-radius: 5px 5px 0 0;
        padding-bottom: 5px;
    }
    .tu-btn-bar > .tu-btn:last-child{
        border-radius: 0 0 5px 5px; 
    }
    .tu-btn-bar > .tu-btn + .tu-btn{
        border-top: 1px solid rgba(0, 0, 0, 0.4);
    }
    .tu-btn-bar > .tu-btn:hover{
        background-color: rgba(0, 0, 0, 0.7);
    }
    .tu-btn-bar > .tu-btn:active{
        background-color: rgba(0, 0, 0, 0.6);
    }
    .edit-line{
        width: 300px;
        overflow: hidden;
        display: flex;
        flex-direction: row;
    }
    .edit-line > .label{
        flex-basis: 0;
        flex-grow: 0;
        min-width: 70px;
        font-size: 12px;
        font-weight: bold;
        display: flex;
        align-items: center;
        justify-content: flex-end;
    }
    .edit-line > .content{
        flex-basis: 0;
        flex-grow: 1;
    }
    .edit-line > .value{
        flex-basis: 0;
        flex-grow: 0;
        min-width: 35px;
        font-size: 12px;
        font-weight: bold;
        display: flex;
        align-items: center;
        justify-content: flex-end;
    }
</style>

