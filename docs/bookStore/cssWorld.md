## css让文字在一行内显示

里面的文字不换行显示，超出部分不隐藏

    p {
        width:100px;
        word-break:keep-all;
        white-space:nowrap;     
    }

里面的文字不换行，超出部分用省略号代替

    p {
        width:100px;
        word-break:keep-all;
        white-space:nowrap;
        overflow:hidden;
        text-overflow:ellipsis; 
    }

    white-space: normal; //默认换行

## Echarts 图表显示问题

实现鼠标hover上去显示全部信息，需配置tootip的formatter函数

    tooltip: {
        show: true,
        trigger: 'axis',
        //formatter: '{a}({b}): {c}%'
        formatter: function (params) {
            var res='<div><p>'+params[0].name+'</p></div>';
            for(var i = 0; i < params.length; i++) {
                res += '<p>'+params[i].seriesName+': '+params[i].data+'%</p>';
            }
            return res;
        }
    }

![charts-01 icon](./charts_tootip.png)


实现鼠标hover上去显示信息过长时换行

![charts-02 icon](./charts_tootip2.png)

options配置如下:

    getChartsOptionsForLine = () => {
        let {detailData}=this.props;
        let nameArr=detailData.map(e=>e.name);

        let format=(e)=>{
            return e.substr(0,4)+'...'
        }
        return {
            xAxis: {
                axisLabel: {
                    interval: 0,
                    rotate: 40,
                    formatter:format
                },
                data: nameArr
            },
            yAxis: {
                axisLine: {
                    show: true
                },
                name : '缺陷数',
            },
            tooltip: {
                show: true,
                //formatter: '{b}: {c}',
                formatter: function (params) {
                    var res='<div style=" max-width: 280px;white-space: normal">' + params.name + ': ' + params.value + '</div>';
                    return res; //换行显示
                }
            },
            series: [
                {
                    type: 'bar',
                    data: detailData,
                    barWidth: 30,
                    itemStyle: {
                        shadowBlur: 2,
                        shadowColor: 'rgba(0, 0, 0, 0.5)',
                        shadowOffsetX: 2,
                        shadowOffsetY: 2
                    }
                }
            ]
        }
    }