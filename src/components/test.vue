<template>
    <div>
        <svg width="1000" height="1500">
            <g />
            <rect />
        </svg>
    </div>
</template>

<script>
    import dagreD3 from "dagre-d3";
    import * as d3 from "d3";
    export default {
        data() {
            return {
                list: {
                    nodeInfos: [
                        {
                            id: "node1",
                            label: "节点1",
                        },
                        {
                            id: "node2",
                            label: "节点2",
                        },
                        {
                            id: "node3",
                            label: "节点3",
                        },
                        {
                            id: "node4",
                            label: "节点4",
                        },
                    ],
                    edges: [
                        {
                            source: "node1",
                            target: "node2",
                        },
                        {
                            source: "node2",
                            target: "node3",
                        },
                        {
                            source: "node2",
                            target: "node4",
                        },
                    ]
                }
            };
        },
        mounted() {
            //获取D3
            var g = new dagreD3.graphlib.Graph().setGraph({});
            console.log(g)
            // 添加节点
            this.list.nodeInfos.forEach((item, index) => {
                item.rx = item.ry = 5;//圆角
                g.setNode(item.id, item);
            });
            // 链接关系
            this.list.edges.forEach(item => {
                g.setEdge(item.source, item.target, {});
            });
            g.nodes().forEach(function (v) {
                console.log("Node " + v + ": " + JSON.stringify(g.node(v)));
            });
            g.edges().forEach(function (e) {
                console.log("Edge " + e.v + " -> " + e.w + ": " + JSON.stringify(g.edge(e)));
            });
            //绘制图形
            var svg = d3.select("svg"),
                inner = svg.select("g");
                console.log(inner, 'inner')
            //缩放
            var zoom = d3.zoom().on("zoom", function () {
                inner.attr("transform", d3.event.transform);
            });
            svg.call(zoom);
            var render = new dagreD3.render();
            render(inner, g);
            console.log(inner, 'inner111')
            let code;
            inner.selectAll("g.node").on("click", e => {
                //点击事件
                code = this.list.nodeInfos.filter(item => {
                    return item.id == e;
                });
                console.log(code);
            });
            var initialScale = 0.75;
            svg.call(
                zoom.transform,
                d3.zoomIdentity
                    .translate(
                        (svg.attr("width") - g.graph().width * initialScale) / 2,
                        20
                    )
                    .scale(initialScale)
            );
            svg.attr("height", g.graph().height * initialScale + 40);
        }
    };
</script>

<style lang="less">
    svg {
        font-size: 14px;
    }

    .node rect {
        stroke: #606266;
        fill: #fff;
    }

    .edgePath path {
        stroke: #606266;
        fill: #333;
        stroke-width: 1.5px;
    }
</style>