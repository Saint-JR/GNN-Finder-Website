<template>
    <div style="position:relative;height: 100%;perspective: 1000px;overflow: hidden;perspective-origin: center;">
        <div class="pic">
            <img src="../static/2.png" class="picture" :style="{left:pic,filter:'blur('+blur+')',opacity:picOpacity}"/>
            <img src="../static/3.png" class="picture" :style="{right:pic,filter:'blur('+blur+')',opacity:picOpacity}"/>
        </div>
        <div class="text" :style="{transform:'rotateX('+rotateY+'deg) rotateY('+rotateX+'deg)',top:top+'%'}">
            <div style="position: absolute;display:flex;flex-direction: column;align-items: center;position: relative;transition: all 2s;pointer-events: none;" :style="{top:textTop}">
                <div>Graph</div>
                <div>Dismantle</div>
                <!-- <div>&Build</div> -->
                <!-- <div>Platform</div> -->
            </div>
            <div style="font-size:50px;position: absolute;margin-top: 10%;">
                <div style="display: flex;justify-content: center;cursor: pointer;" @click="click">
                    <div v-show="show[0]">></div>
                    <div v-show="show[1]" style="color:#e7d327">P</div>
                    <div v-show="show[2]" style="color:#e7d327">y</div>
                    <div v-show="show[3]" style="color:#e7d327">t</div>
                    <div v-show="show[4]" style="color:#e7d327">h</div>
                    <div v-show="show[5]" style="color:#e7d327">o</div>
                    <div v-show="show[6]" style="color:#e7d327">n</div>
                    <div v-show="show[7]" >&nbsp;</div>
                    <div v-show="show[8]" >G</div>
                    <div v-show="show[9]" >r</div>
                    <div v-show="show[10]" >a</div>
                    <div v-show="show[11]" >p</div>
                    <div v-show="show[12]" >h</div>
                    <div v-show="show[13]" style="color:#e7d327">.</div>
                    <div v-show="show[14]" style="color:#e7d327">p</div>
                    <div v-show="show[15]" style="color:#e7d327">y</div>
                    <div :style="{opacity:opacity[0]}">_</div>
                </div>
                
            </div>
            <div style="font-size:50px;position: absolute;margin-top: 20%;">
                <div style="display: flex;justify-content: center;" v-show="show[16]">
                    <div>>Wait...</div>
                    <div :style="{opacity:opacity[1]}">_</div>
                    <div>&emsp;&emsp;&emsp;&nbsp;&emsp;&ensp;&ensp;</div>
                </div>
                
            </div>
        </div>

        <div style="position:absolute;display:flex;justify-content: space-around;align-items: center;height: 100%;width: 100%;transition: all 2s;" :style="{top:top+100+'%'}">
            <div style="width:60%;height:100%;position: relative;display: flex;justify-content: space-around;flex-direction: column;">
                <div style="font-size: 40px;color:white;font-weight: 700;text-shadow: 2px 2px 2px black;">
                    Graph Dismantle
                </div>
                <div class="build" id="build"></div>
                <div class="buttonDiv">
                    <div :class="buttonClicked[0]?'twoButton rotate':'twoButton'">
                        <div class="cancel" @click="clickButton(0);cancel()">Cancel</div>
                        <div :class="buttonClicked[0]?'button buttonClicked':'button'" @click="addNode();clickButton(0)">Add Node</div>
                    </div>
                    
                    <div :class="buttonClicked[1]?'twoButton rotate':'twoButton'">
                        <div class="cancel" @click="clickButton(1);cancel()">Cancel</div>
                        <div :class="buttonClicked[1]?'button buttonClicked':'button'"  @click="addEdge();clickButton(1)">Add Edge</div>
                    </div>

                    <div :class="buttonClicked[2]?'twoButton rotate':'twoButton'">
                        <div class="cancel" @click="clickButton(2)">Wait...</div>
                        <div :class="buttonClicked[2]?'button buttonClicked':'button'"  @click="clickButton(2)">Start</div>
                    </div>
                    
                </div>
            </div>
            
            
            <div class="build2">
                <div style="position: absolute;font-size: 30px;color: rgba(0, 0, 0, 0.6);font-weight: 700;left: 130px;top: 20px;">Node</div>
                <div style="position: absolute;font-size: 30px;color: rgba(0, 0, 0, 0.6);font-weight: 700;right: 125px;top: 20px;">Edge</div>
                <div style="width:100%">
                    <table>
                    
                        <thead>
                            <tr>
                                <th class="deep">No.</th>
                                <th class="deep">X</th>
                                <th class="deep">Y</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(node,index) in data" :key="index">
                                <td :class="index%2==0?'light':'deep'">{{index}}</td>
                                <td :class="index%2==0?'light':'deep'">{{node.value[0].toFixed(0)}}</td>
                                <td :class="index%2==0?'light':'deep'">{{node.value[1].toFixed(0)}}</td>
                                <img src="../static/cancel.png" class="delete" @click="deleteNode(index)"/>
                            </tr>
                        </tbody>
                    </table>
                    <table>
                        
                        <thead>
                            <tr>
                                <th class="deep">Source</th>
                                <th class="deep">Target</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(edge,index) in links" :key="index">
                                <td :class="index%2==0?'light':'deep'">{{edge.source}}</td>
                                <td :class="index%2==0?'light':'deep'">{{edge.target}}</td>
                                <img src="../static/cancel.png" class="delete" @click="deleteEdge(index)"/>
                            </tr>
                        </tbody>
                    </table>
                </div>
                
            </div>
        </div>

    </div>
</template>

<script>
export default {
    name: "home",
    data(){
        return{ 
            rotateY:0,
            rotateX:0,
            windowWidth: document.documentElement.clientWidth,
            windowHeight: document.documentElement.clientHeight,
            textTop:'-10%',
            // left:40,
            pic:'-50%',
            show:[false,false,false,false,false,false,false,false,false,false,false,false,false,false,false,false,false],
            opaInterval:'',
            opacity:[1,0], 
            blur:'0px',
            picOpacity:1,
            top:0,
            data:[{
                value:[62,31],
                name:'0',
                symbolSize:30
            },{
                value:[36,82],
                name:'1',
                symbolSize:35
            }],
            links:[{
                source:0,
                target:1
            }],
            myChart:'',
            option:'',
            buttonClicked:[false,false,false]

        }
    },
    mounted(){
        setTimeout(() => {
            this.blur='0px';
            this.pic='-10%'
            this.textTop='-30%'
            console.log(this.opacity[0])
            // this.rotate=0;
            var count=0;
            var interval=setInterval(()=>{
                this.$set(this.show, count,true)
                count++;
                if(count==16){
                    clearInterval(interval);
                    this.opaInterval=setInterval(()=>{
                        this.$set(this.opacity, 0,this.opacity[0]==0?1:0)
                    },500)
                }
            },100)
        }, 100);


        this.myChart = this.$echarts.init(document.getElementById('build'));

        this.option = {
            title: {
                text: 'Graph Build'
            },
            tooltip: {
                formatter:(params)=>{
                    if(params.dataType=='node')
                        return [params.value[0].toFixed(0),params.value[1].toFixed(0)].toString()
                }
            },
            xAxis: {
                type:'value',
                min:0,
                max:150,
                splitNumber:15
            },
            yAxis: {
                type: 'value',
                min:0,
                max:100,
                splitNumber:10
            },
            series: [
                {
                    type: 'graph',
                    layout: 'none',
                    coordinateSystem: 'cartesian2d',
                    symbolSize: 40,
                    label: {
                        show: true
                    },
                    edgeSymbol: ['circle', 'arrow'],
                    edgeSymbolSize: [4, 10],
                    data: this.data,
                    links: this.links,
                    lineStyle: {
                        color: '#2f4554'
                    }
                }
            ]
        };

        // 使用刚指定的配置项和数据显示图表。
        this.myChart.setOption(this.option);

        console.log(this.buttonClicked)

    },
    methods:{
        focus(e){
            // console.log(e)
            
            // var xPercent=e.offsetX/this.windowWidth*100
            // var yPercent=e.offsetY/this.windowHeight*100
            // // console.log(yPercent)
            // var rY=(this.top-yPercent)/100
            // if(Math.abs(rY)<=0.01){
            //     if(rY<0)
            //         this.rotateY=90*-0.01
            //     else this.rotateY=90*0.01
            // }else{
            //     if(rY<0)
            //         this.rotateY=150*rY
            //     else this.rotateY=90*rY
            // } 
            // var rX=-(this.left-xPercent)/100
            // if(Math.abs(rX)<=0.01){
            //     if(rX<0)
            //         this.rotateX=90*-0.01
            //     else this.rotateX=90*0.01
            // }else{
            //     if(rX<0)
            //         this.rotateX=300*rX
            //     else this.rotateX=60*rX
            // } 
        },
        click(){
            this.blur='10px'
            this.picOpacity=0.6
            
            clearInterval(this.opaInterval)
            this.opacity[0]=0
            this.$set(this.show, 16,true)
            var interval=setInterval(()=>{
                this.$set(this.opacity, 1,this.opacity[1]==0?1:0)
            },500)
            setTimeout(()=>{
                this.pic='-50%'
                clearInterval(interval)
                this.top=-100
            },2000)
        },
        addNode(){
            this.myChart.off('mousedown');
            this.myChart.getZr().off('click');
            this.myChart.getZr().on('click', (params)=> {
                
                var pointInPixel = [params.offsetX, params.offsetY];
                var pointInGrid = this.myChart.convertFromPixel('grid', pointInPixel);
                if (this.myChart.containPixel('grid', pointInPixel)) {
                    this.data.push({
                        name:String(this.data.length),
                        value:pointInGrid,
                        symbolSize:30
                    });
                    this.option.series[0].data=this.data
                    this.myChart.setOption(this.option);
                }
            });
        },
        addEdge(){
            this.myChart.off('mousedown');
            
            this.myChart.getZr().off('click');
            var node=[];
            var count=0;
            this.myChart.on('mousedown',(params)=>{
                if(params.dataType=='node'){
                    node.push(Number(params.dataIndex))
                    count++
                    if(count==2){
                        count=0
                        if(node[0]==node[1]){
                            node=[]
                            return
                        }
                        var link={
                            source:node[0],
                            target:node[1]
                        }
                        var bool=true
                        for(var i in this.links){
                            if(this.links[i].source==link.source&&this.links[i].target==link.target){
                                bool=false;
                                break;
                            }
                        }
                        if(bool){
                            this.links.push(link);
                            this.data[node[1]].symbolSize+=5
                            this.option.series[0].links=this.links
                            this.option.series[0].data=this.data
                            this.myChart.setOption(this.option);
                        }
                        node=[]
                    }
                }
                
                

            })
        },
        clickButton(index){
            
            for(var i in this.buttonClicked){
                if(i==index)
                    this.$set(this.buttonClicked, index,this.buttonClicked[index]?false:true)
                else this.$set(this.buttonClicked, i,false)
            }
            
        },
        cancel(){
            this.myChart.off('mousedown');
            
            this.myChart.getZr().off('click');
        },
        deleteNode(index){
            var deleteEdge=[]
            for(var i in this.links){
                if(this.links[i].source==index||this.links[i].target==index){
                    if(this.links[i].source==index)
                        this.data[this.links[i].target].symbolSize-=5
                    deleteEdge.push(i)
                }
                if(this.links[i].source>index)
                    this.links[i].source--
                    if(this.links[i].target>index)
                    this.links[i].target--
            }
            for(var i=deleteEdge.length-1;i>=0;i--){
                this.links.splice(deleteEdge[i],1)
            }
            for(var i=index+1;i<this.data.length;i++){
                this.data[i].name=String(Number(this.data[i].name)-1);
            }
            this.data.splice(index,1)
            this.option.series[0].links=this.links
            this.option.series[0].data=this.data
            this.myChart.setOption(this.option)
        },
        deleteEdge(index){
            this.data[this.links[index].target].symbolSize-=5
            this.links.splice(index,1)
            this.option.series[0].links=this.links
            this.option.series[0].data=this.data
            this.myChart.setOption(this.option)
        }
    },
}

</script>

<style scoped>
    .pic{
        height: 100%;
        width: 100%;
        display: flex;
        justify-content: space-between;
        background-color: rgb(200, 200, 200);
        transition: all 1s;
    }

    .picture{
        height:100%;
        width: auto;
        position: relative;
        transition: all 2s;
    }

    .text{
        text-shadow: 2px 2px 2px black;
        position:absolute;
        height: 100%;
        width: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        transition: all 2s;
        transform-origin: center;
        letter-spacing: 4px;
        font-size: 100px;
        font-weight: 700;
        color: white;
    }

    .build{
        width:100%;
        height:40vw;
        background-color: rgb(220, 220, 220);
        
    }
    
    .build2{
        width:33%;
        height:40vw;
        background-color: rgb(220, 220, 220);
        position: relative;
        /* display: flex; */
        overflow-y: auto;
        overflow-x: hidden;
    }

    .buttonDiv{
        /* border: 2px solid red; */
        width: 100%;
        height: 47px;
        /* height:40vw; */
        display: flex;
        justify-content: space-around;
        align-items: center;
        text-align: center;
        perspective: 500px;
        
    }

    .button{
        position: absolute;
        display: flex;
        justify-content: center;
        align-items: center;
        padding-top: 8px;
        padding-bottom: 8px;
        width:100%;
        font-size:20px;
        /* font-weight: 500; */
        /* border: 2px solid red; */
        background-color: rgb(220, 220, 220);
        border-radius: 10px;
        color: rgb(0, 0, 0,0.6);
        box-shadow: 3px 3px 5px rgb(30, 30, 30);
        margin-bottom:10px;
        cursor:pointer;
        transition: all .5s;
        z-index: 3;
        /* transform: translateZ(5px); */
    }

    .button:hover{
        background-color: rgb(218,254,254);
    }

    .buttonClicked{
        z-index: 0;
    }

    .twoButton{
        width:15%;
        position: relative;
        height: 100%;
        transition: all .5s;
        transform: rotateX(0deg);
        transform-origin: center;
        /* backface-visibility: hidden; */
        /* pointer-events: none; */
    }

    .cancel{
        position:absolute;
        transform: rotateX(-180deg);
        width: 100%;
        background-color: rgb(249,217,222);
        display: flex;
        justify-content: center;
        align-items: center;
        padding-top: 8px;
        padding-bottom: 8px;
        font-size:20px;
        box-shadow: 3px 3px 5px rgb(30, 30, 30);
        margin-bottom:10px;
        cursor:pointer;
        border-radius: 10px;
        pointer-events: auto;
        z-index: 2;
    }

    .rotate{
        transform: rotateX(180deg);
        /* background-color: rgb(255, 0, 0); */
    }

    thead{
        width: 80px;
        height: 40px;
    }

    td,th{
        /* border: 2px solid rgba(220, 220,220, 1); */
        /* height: 10%;
        width: 40%; */
        text-align: center;
        width: 80px;
        height: 40px;
        color: rgba(50, 50, 50, 0.8);
    }

    .deep{
        background-color: rgb(160, 160, 160);
        color: white;
    }

    .light{
        background-color: rgb(235, 235, 235);
    }

    table{
        /* border-collapse: collapse; */
        border-collapse: separate;
        width: 60%;
        margin-top: 12%;
        font-size: 20px;
        position: absolute;
        
    }

    table:first-child{
        left: 45px;
        /* border: 2px solid red; */
    }

    table:last-child{
        right: -125px;
        /* border: 2px solid blue; */
    }

    .delete{
        height:20px;
        width:20px;
        margin-left: 10px;
        margin-top: 10px;
        opacity: 0.4;
        cursor: pointer;
    }

</style>
