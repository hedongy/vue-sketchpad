<template>
    <div class="tu-paper" @mousemove="beginPath($event)" ref="paper">
        <canvas ref="canvas" id="canvas" class="container"
            :width="preWidth" :height="preHeight"
            @mousedown="canvasDown($event)" 
            @mouseup="canvasUp($event)"
            @mousemove="canvasMove($event)">
        </canvas>
        <tu-btn-bar @color="colorHandler" 
            @slider="sliderHandler"
            @back="backHandler"
            @advance="advanceHandler"
            @remove="removeHandler"
            @save="saveHandler">
        </tu-btn-bar>
        <el-dialog title="图片预览" :visible.sync="dialogVisible">
            <!-- <el-row :gutter="20"> -->
            <el-row>
                <el-col :span="24" v-for="(img, i) in imgUrl" :key="i">
                    <el-card style="margin-bottom: 10px;">
                        <img :src="img" alt="" class="image" style="width: 100%;">
                    </el-card>
                </el-col>
            </el-row>
            <div slot="footer">
                <el-button @click="dialogVisible = false">取消</el-button>
                <el-button type="primary" @click="dialogVisible = false">保存</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>

    import TuBtnBar from './btn-bar'
    
    export default {
        name: 'TuPaper',
        components: {TuBtnBar},
        data() {
            return {
                dialogVisible: false,
                // 上一个状态
                preDrawArray: [],
                // 中间状态
                middleArray: [],
                // 下一个状态
                nextDrawArray: [],
                // 使用状态
                canvasMoveUse: false,
                context: {},
                // 配置参数
                config: {
                    lineWidth: 1,
                    lineColor: '#409EFF',
                    shadowBlur: 2
                },
                preWidth: null,
                preHeight: null,
                imgUrl: []
            }
        },
        mounted() {
            const canvas = this.$refs.canvas
            this.context = canvas.getContext('2d')
            this.initDraw()
            this.setCanvasStyle()
        },
        methods: {

            /**
             * 初始化画布
             */
            initDraw() {
                this.preWidth = (this.$refs.paper || {}).offsetWidth - 85
                this.preHeight = (this.$refs.paper || {}).offsetHeight - 30
                const preData = this.context.getImageData(0, 0, this.preWidth, this.preHeight)
                // 空绘图表面进栈
                this.middleArray.push(preData)
            },

            /**
             * 开始
             */
            beginPath(e) {
                const canvas = this.$refs.canvas
                if (e.target !== canvas) {
                    this.context.beginPath()
                }
            },

            /**
             * 鼠标移动
             */
            canvasMove(e) {
                if (this.canvasMoveUse) {
                    const canvasX = e.clientX - e.target.offsetLeft
                    const canvasY = e.clientY - e.target.offsetTop
                    this.context.lineTo(canvasX, canvasY)
                    this.context.stroke()
                }
            },

            /**
             * 鼠标放开
             */
            canvasUp(event) {
                const preData = this.context.getImageData(0, 0, this.preWidth, this.preHeight)
                if (!this.nextDrawArray.length) {
                    // 当前绘图表面进栈
                    this.middleArray.push(preData)
                } else {
                    this.middleArray = []
                    this.middleArray = this.middleArray.concat(this.preDrawArray)
                    this.middleArray.push(preData)
                    this.nextDrawArray = []
                }
                this.canvasMoveUse = false
            },

            /**
             * 鼠标按下
             */
            canvasDown(e) {
                this.canvasMoveUse = true
                // client是基于整个页面的坐标
                // offset是cavas距离顶部以及左边的距离
                const canvasX = e.clientX - e.target.offsetLeft
                const canvasY = e.clientY - e.target.offsetTop
                this.setCanvasStyle()
                // 清除子路径
                this.context.beginPath()
                this.context.moveTo(canvasX, canvasY)
                // 当前绘图表面状态
                const preData = this.context.getImageData(0, 0, this.preWidth, this.preHeight)
                // 当前绘图表面进栈
                this.preDrawArray.push(preData)
            },

            // 设置绘画配置
            setCanvasStyle () {
                this.context.lineWidth = this.config.lineWidth
                this.context.shadowBlur = this.config.shadowBlur
                this.context.shadowColor = this.config.lineColor
                this.context.strokeStyle = this.config.lineColor
            },

            /**
             * 颜色
             */
            colorHandler(color) {
                this.config.lineColor = color
            },

            /**
             * 画笔粗细
             */
            sliderHandler(slider) {
                this.config.lineWidth = slider
            },

            /**
             * 撤回
             */
            backHandler() {
                if (this.preDrawArray.length > 0) {
                    const popData = this.preDrawArray.pop()
                    const midData = this.middleArray[this.preDrawArray.length + 1]
                    this.nextDrawArray.push(midData)
                    this.context.putImageData(popData, 0, 0)
                }
            },

            /**
             * 前进
             */
            advanceHandler() {
                if (this.nextDrawArray.length > 0) {
                    const popData = this.nextDrawArray.pop()
                    const midData = this.middleArray[this.middleArray.length - this.nextDrawArray.length - 2]
                    this.preDrawArray.push(midData)
                    this.context.putImageData(popData, 0, 0)
                }
            },

            /**
             * 删除
             */
            removeHandler() {
                this.context.clearRect(0, 0, this.context.canvas.width, this.context.canvas.height)
                this.preDrawArray = []
                this.nextDrawArray = []
                this.middleArray = [this.middleArray[0]]
            },
            
            /**
             * 保存
             */
            saveHandler() {
                const canvas = document.querySelector('#canvas')
                const src = canvas.toDataURL('image/png')
                this.imgUrl.push(src)
                this.dialogVisible = true
            }
        }
    }
</script>

<style scoped>
    .tu-paper{
        height: 100%;
        overflow: hidden;
        position: relative;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
    .tu-paper > .container{
        background-color: #ffffff;
        box-shadow: 0 0 2px 2px #c2c1c1;
        cursor: crosshair;
        margin: 15px 70px 15px 15px;
    }
</style>
